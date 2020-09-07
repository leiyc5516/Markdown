#### Java网络编程的简介

**所谓网络编程：**值得是多台主机间的数据通讯操作

 网络定义的核心指的 是两台以上的电脑就可以称为网络；

##### 在通信的协议上包括但不限于：IP，TCP，UDP等等

**通讯操作需要分为客户端服务器端：**

* C/S开发结构开发两套系统，一套为服务端一套为客户端，开发者可以自定义传输协议使用一些私密的端口，相对的安全性能更好；开发成本高
* B/S只开发服务端，开发成本低，利用浏览器作为客服端使用HTTP协议，和使用80端口相对的安全性不高

这次主要讲解的是cC/S模型主要讲解TCP（可靠的数据连接）/UDP（不可靠的数据连接）；

#### TCP的程序开发是网络程序开发的基本模型，器核心的特点是使用两个类实现数据通讯：

* ServerSocket：设置服务器监听的端口，辨别用户
* Socket：指明需要的连接的服务器的IP地址和端口，描述每一个独立的用户

Echo程序：

客户端

```java
package net;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintStream;
import java.net.Socket;
 
public class Client {
    public static final int port = 8080;   
    public static final String host = "localhost";
    public static void main(String[] args) {    
        System.out.println("Client Start...");    
        while (true) {    
            Socket socket = null;  
            try {  
                //创建一个流套接字并将其连接到指定主机上的指定端口号  
                socket = new Socket(host,port);    
 
                //读取服务器端数据    
                BufferedReader input = new BufferedReader(new InputStreamReader(socket.getInputStream()));    
                //向服务器端发送数据    
                PrintStream out = new PrintStream(socket.getOutputStream());    
                System.out.print("请输入: \t");    
                String str = new BufferedReader(new InputStreamReader(System.in)).readLine();    
                out.println(str);    
 
                String ret = input.readLine();     
                System.out.println("服务器端返回过来的是: " + ret);    
                // 如接收到 "OK" 则断开连接    
                if ("OK".equals(ret)) {    
                    System.out.println("客户端将关闭连接");    
                    Thread.sleep(500);    
                    break;    
                }    
 
                out.close();  
                input.close();  
            } catch (Exception e) {  
                System.out.println("客户端异常:" + e.getMessage());   
            } finally {  
                if (socket != null) {  
                    try {  
                        socket.close();  
                    } catch (IOException e) {  
                        socket = null;   
                        System.out.println("客户端 finally 异常:" + e.getMessage());   
                    }  
                }  
            }  
        }    
    }    
}
```

服务端

```Java
package net;
 
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintStream;
import java.net.ServerSocket;
import java.net.Socket;
 
public class Server {
    public static final int port = 8080;//监听的端口号     
 
    public static void main(String[] args) {    
        System.out.println("Server...\n");    
        Server server = new Server();    
        server.init();    
    }    
 
    public void init() {    
        try {    
            //创建一个ServerSocket，这里可以指定连接请求的队列长度  
            //new ServerSocket(port,3);意味着当队列中有3个连接请求是，如果Client再请求连接，就会被Server拒绝 
            ServerSocket serverSocket = new ServerSocket(port);    
            while (true) {    
                //从请求队列中取出一个连接
                Socket client = serverSocket.accept();    
                // 处理这次连接    
                new HandlerThread(client);    
            }    
        } catch (Exception e) {    
            System.out.println("服务器异常: " + e.getMessage());    
        }    
    }    
 
    private class HandlerThread implements Runnable {    
        private Socket socket;    
        public HandlerThread(Socket client) {    
            socket = client;    
            new Thread(this).start();    
        }    
 
        public void run() {    
            try {    
                // 读取客户端数据    
                BufferedReader input = new BufferedReader(new InputStreamReader(socket.getInputStream()));    
                String clientInputStr = input.readLine();//这里要注意和客户端输出流的写方法对应,否则会抛 EOFException  
                // 处理客户端数据    
                System.out.println("客户端发过来的内容:" + clientInputStr);    
 
                // 向客户端回复信息    
                PrintStream out = new PrintStream(socket.getOutputStream());    
                System.out.print("请输入:\t");    
                // 发送键盘输入的一行    
                String s = new BufferedReader(new InputStreamReader(System.in)).readLine();    
                out.println(s);    
 
                out.close();    
                input.close();    
            } catch (Exception e) {    
                System.out.println("服务器 run 异常: " + e.getMessage());    
            } finally {    
                if (socket != null) {    
                    try {    
                        socket.close();    
                    } catch (Exception e) {    
                        socket = null;    
                        System.out.println("服务端 finally 异常:" + e.getMessage());    
                    }    
                }    
            }   
        }    
    }    
}
```

##### **多人连接会出现连接不了**（单线程开发的不合理性）

+ 最好的方法是每个客户是一个线程（服务器启动多个线程，每个线程为单独为一个用户实现Echo）

#### UDP编程案例：是不可靠

```java
package udp;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class UDPServer {
public static void main(String[] args) throws Exception {
	 DatagramSocket server = new DatagramSocket(9000);//监听的端口
	   String str = new String("www.dns.com");
	    DatagramPacket packet = new DatagramPacket(str.getBytes(),0, str.length(),InetAddress.getByName("localhost"),9999);//接收数据
	    server.send(packet);//发送消息
	    System.out.println("消息发送完毕.....");
	    server.close();
	}
  
}
```

```java
package udp;

import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class UDPClient {
public static void main(String[] args) throws Exception {
    DatagramSocket client = new DatagramSocket(9999);//连接的端口
    byte data[] = new byte[1024];//存储接收的消息
    DatagramPacket packet = new DatagramPacket(data, data.length);//接收数据
    System.out.println("客户端等待接收消息.........");
    client.receive(packet);//接收消息，所有的消息都在data字节数组之中
    System.out.println("接收的消息内容为"+new String(data,0,data.length));
    client.close();
	}
}
```

