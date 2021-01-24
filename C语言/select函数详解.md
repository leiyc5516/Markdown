~~~scss
 select函数的功能和调用顺序

使用select函数可以完成非阻塞方式工作的程序，它能够监视我们需要监视的文件描述符的变化情况——读写或是异常。
非阻塞方式：non-block，就是进程或线程执行此函数时不必非要等待事件的发生，一旦执行肯定返回，以返回值的不同来反映函数的执行情况，如果事件发生则与阻塞方式相同，若事件没有发生，则返回一个代码来告知事件未发生，而进程或线程继续执行，所以效率较高。
select函数用来统一监视多个文件描述符的：
1、 是否存在套接字接收数据？
2、 无需阻塞传输数据的套接字有哪些?
3、 哪些套接字发生了异常？
~~~

select函数调用过程：
![这里写图片描述](E:\github\C语言\70.jpg)
由上图知，调用select函数需要一些准备工作，调用后还需要查看结果。

~~~scss
设置文件描述符

select可以同时监视多个文件描述符（套接字）。
此时需要先将文件描述符集中到一起。集中时也要按照监视项（接收，传输，异常）进行区分，即按照上述3种监视项分成三类。
使用fd_set数组变量执行此项操作，该数组是存有0和1的位数组。
~~~

![这里写图片描述](E:\github\C语言\80.jpg)

~~~scss
数组是从下标0开始，最左端的位表示文件描述符0。如果该位值为1，则表示该文件描述符是监视对象。
图上显然监视对象为fd1和fd3。

“是否应当通过文件描述符的数字直接将值注册到fd_set变量？”
当然不是！操作fd_set的值由如下宏来完成：

`FD_ZERO(fd_set* fdset)`： 将fd_set变量的所有位初始化为0。
`FD_SET（int fd, fd_set* fdset）`：在参数fd_set指向的变量中注册文件描述符fd的信息。
`FD_CLR(int fd, fd_set* fdset)`：参数fd_set指向的变量中清除文件描述符fd的信息。
`FD_ISSET(int fd, fd_set* fdset)`：若参数fd_set指向的变量中包含文件描述符fd的信息，则返回真。
~~~

![这里写图片描述](E:\github\C语言\90.jpg)

### 设置监视范围及超时

select函数：

```c
#include <sys/select.h>
#include <sys/time.h>
int select(int maxfd, fd_set* readset, fd_set* writeset, fd_set* exceptset, 
const struct timeval* timeout);1234
```

~~~scss
select函数共有5个参数，其中参数和返回值：
`maxfd`：监视对象文件描述符数量。
`readset`：将所有关注“是否存在待读取数据”的文件描述符注册到fd_set变量，并传递其地址值。
`writeset`： 将所有关注“是否可传输无阻塞数据”的文件描述符注册到fd_set变量，并传递其地址值。
`exceptset`：将所有关注“是否发生异常”的文件描述符注册到fd_set变量，并传递其地址值。
`timeout`：调用select后，为防止陷入无限阻塞状态，传递超时信息。
返回值：错误返回-1，超时返回0。当关注的事件返回时，返回大于0的值，该值是发生事件的文件描述符数。

select函数用来验证3种监视项的变化情况。根据监视项声明3个fd_set变量，分别向其注册文件描述符信息，并把变量的地址传递到函数的第二到第四个参数。但是，在调用select函数前需要决定2件事：
“文件描述符的监视范围是？”
“如何设定select函数的超时时间？”

**第一，文件描述符的监视范围与第一个参数有关。**实际上，select函数要求通过第一个参数传递监视对象文件描述符的数量。因此，需要得到注册在fd_set变量中的文件描述符数。但每次新建文件描述符时，其值都会增1，故只需将最大的文件描述符值加1再传递到select函数即可。（加1是因为文件描述符的值从0开始）
**第二，超时时间与最后一个参数有关。**其中timeval结构体如下：
~~~



```c
struct timeval
{
    long tv_sec;
    long tv_usec;
};12345
```

~~~scss
本来select函数只有在监视文件描述符发生变化时才返回，未发生变化会进入阻塞状态。指定超时时间就是为了防止这种情况发生。
将上述结构体填入时间值，然后将结构体地址值传给select函数的最后一个参数，此时，即使文件描述符中未发生变化，只要过了指定时间，也可以从函数返回。不过这种情况下，select函数返回0。 不想设置超时最后一个参数只需要传递NULL。

### 调用select函数后查看结果

如果select返回值大于0，说明文件描述符发生了变化。

关于文件描述符变化：
**文件描述符变化是指监视的文件描述符中发生了相应的监视事件。**
例如通过select的第二个参数传递的集合中存在需要读取数据的描述符时，就意味着文件描述符发生变化。

怎样获知哪些文件描述符发生了变化？向select函数的第二到第四个参数传递的fd_set变量中将产生变化，如下图：
~~~

![这里写图片描述](E:\github\C语言\100.jpg)

~~~scss
select函数调用完成后，向其传递的fd_set变量中将发生变化。原来为1的所有位均变为0，但发生变化的文件描述符对应位除外。因此，可以认为值为1的位置上的文件描述符发生了变化。
~~~



### select函数调用实例

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/time.h>
#include <sys/select.h>

#define BUF_SIZE 30

int main(int argc, char* argv[])
{
    fd_set reads,temps;
    int result, str_len;
    char buf[BUF_SIZE];
    struct timeval timeout;
    FD_ZERO(&reads);
    FD_SET(0, &reads);//监视文件描述符0的变化, 即标准输入的变化
    /*超时不能在此设置！
    因为调用select后，结构体timeval的成员tv_sec和tv_usec的值将被替换为超时前剩余时间.
    调用select函数前，每次都需要初始化timeval结构体变量.
    timeout.tv_sec = 5;
    timeout.tv_usec = 5000;*/
    while(1)
    {
        /*将准备好的fd_set变量reads的内容复制到temps变量，因为调用select函数后，除了发生变化的fd对应位外，
        剩下的所有位都将初始化为0，为了记住初始值，必须经过这种复制过程。*/
        temps = reads;
        //设置超时
        timeout.tv_sec = 5;
        timeout.tv_usec = 0;

        //调用select函数. 若有控制台输入数据，则返回大于0的整数，如果没有输入数据而引发超时，返回0.
        result = select(1, &temps, 0, 0, &timeout);
        if(result == -1)
        {
            perror("select() error");
            break;
        }
        else if(result == 0)
        {
            puts("timeout");
        }
        else
        {
            //读取数据并输出
            if(FD_ISSET(0, &temps))
            {
                str_len = read(0, buf, BUF_SIZE);
                buf[str_len] = 0;
                printf("message from console: %s", buf);
            }
        }
    }

    return 0;
}
```

程序运行结果：

```
nihao
message from console: nihao
goodbye
message from console: goodbye
timeout
timeout
```