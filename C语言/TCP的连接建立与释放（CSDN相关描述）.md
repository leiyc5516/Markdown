## TCP --- 传输控制协议

## 报头格式：

![img](https://img-blog.csdn.net/20170308174344729?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM5NTExODA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（1）源端口和目的端口：与UDP类似，TCP的分用是通过端口实现的。

（2）序号：TCP是面向字节流的，在TCP连接中传送的字节流的每一个字节都是有顺序的，整个要传送的字节流的起始序号必须要在连接建立时设置。首部中的序号字段值表示本报文段的数据的第一个字节的序号。该字段也称为“报文段序号”。

（3）确认号：是期望收到对方下一个报文段的第一个数据字节的序号。

（4）4位首部长度：它指出TCP报文段的数据起始处距离TCP报文段起始处有多远，因此也称为“数据偏移”。由于4位二进制数最大能表示十进制数字是15，因此数据偏移最大值是60，则选项长度最大为40字节。

（5）保留：占6位，为今后使用，目前置为0

（6）设置位：

紧急URG：当URG=1时，表明紧急指针有效，它告诉系统此报文段中有紧急数据，应尽快传送，而不需要按原来的顺序排列等待。发送方会将紧急数据插入到本报文段最前面，在紧急数据之后的仍是普通数据。这是需要与首部紧急指针(Urgent Point)配合使用。

确认ACK：仅当ACK=1时确认字段有效，建立连接后所有传输的报文段必须将ACK置为1。

推送PSH：PSH=1时表示，该报文段希望到达对端时，将这个报文以及缓存区之间缓存尚未交付的数据一并交给进程。

复位RST：当RST=1时表明，TCP链接出现严重差错，必须释放连接，然后再重新建立连接。当其为 1 的时候也可以用来拒绝一个非法的报文段或拒绝打开一个连接，也称为重建位或重置位。

同步SYN：连接建立时用来同步序号，SYN=1表示这是一个连接请求或接受报文。SYN=1,ACK=0时表明这是一个连接请求报文，SYN=1,ACK=1表示同意与对方建立连接。

终止FIN：用来释放一个连接。FIN=1表示数据已经发送完，要求释放连接。

（7）窗口：指发送本报文段一方的接收窗口，告诉对方：从本报文段首部中的确认序号算起，接收方目前允许对方发送的数据量。窗口值经常动态变化着。

（8）检验和：检验和字段检验的范围包括首部和数据两部分。和UDP一样，在计算检验和时，要在TCP报文段的前面加上12字节的伪首部。

（9）紧急指针：仅在URG=1时才有意义，它指出本报文段中的紧急数据的字节数。紧急指针指出了紧急数据的末尾在报文段中的位置。

以上是对TCP报头各个字段的简单解释，详情参考资料《计算机网络  谢希仁  第六版》

## TCP建立连接（三次握手）：

连接建立的过程：

***\*·\****Client向Server发送连接请求

***\*·\****Server接收到Client的请求后，同意建立连接后向Client发送ACK确认报文，并为这次连接的建立分配资源

***\*·\****Client接收到对方发来的ACK确认后也向Server发送ACK确认报文，连接建立完成

![img](https://img-blog.csdn.net/20170309153936779?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM5NTExODA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

思考题：TCP的连接建立为什么需要三次握手(即进行了两次确认)？

答：这主要是**为了防止已经失效的连接请求报文段突然又传给了服务器**，因而会产生错误。假如现在Client端向Server端发送请求连接，而这个报文段却在网络结点中长时间滞留了，在连接释放以后Client发送的这个报文段才到达对方，此时这个报文段就是一个已经失效的连接请求报文段。但Server收到这个报文段后并不知其失效，以为是Client又发送的一次请求，就会向Client发送同意建立连接的确认报文段。但问题是Client并没有给Server发送请求，因此也会对这个确认报文段不予理睬，也不会发送数据。但Server会以为连接建立了，并会一直处于等待状态，等待对方的数据，在这样的情况下，Server的一些资源就会浪费了。

（选自谢希仁的《计算机网络》第六版，我在网上也找了相关的内容，分享一个连接：http://www.cnblogs.com/TechZi/archive/2011/10/18/2216751.html）

## TCP拆除连接（四次挥手）：

数据传输完成后，Client和Server都处于ESTABLISHED状态，通信的双方都可以进行释放连接的动作。

TCP连接释放的过程比较复杂，下来我们一条一条分析：

***\*·\****Client主动发送释放连接的请求，并停止发送数据。Client将连接释放报文段的首部的FIN置为1，其序号为u，它等于前面已传送过的数据的最后一个字节的序号加1。此时Client进入FIN-WAIT-1（终止等待1）状态。

***\*·\****Server接收到Client的释放请求后发出确认，确认号是u+1，而确认报文段自己的序号是v，等于Server前面已经传送过的数据的最后一个字节的序号加1。接着Server进入CLOSE-WAIT（关闭等待）状态，并且TCP会通知高层应用程序，此时Client到Server的连接已经关闭，但Server到Client的连接仍然存在，因此此时的TCP连接处于半关闭状态。Server仍然有可能会给Client发送数据，Client也要接收。

***\*·\****Client收到来自对方的确认后，进入FIN-WAIT-2（终止等待2）状态，等待Server释放连接。

***\*·\****Server发送完数据后发送释放报文段，其应用程序就通知TCP释放连接。Server发送的报文段中必须将FIN置为1。由于在半关闭状态Server可能也发送了数据，因此假定此时Server的序号为w，除此之外，Server也必须重复上次发送的确认号u+1。发送完后Server进入LAST-ACK（最后确认）状态，等待Client确认。

***\*·\****Client收到释放报文段后发送确认报文段。此时Client并没有释放掉连接，而是进入TIME-WAIT（时间等待）状态，等过了2MSL时间后才进入到CLOSED状态。

至于这里为什么要等待2MSL时间后才关闭，可以参考这篇文章：http://blog.csdn.net/qq_33951180/article/details/60468267

![img](https://img-blog.csdn.net/20170309152935699?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM5NTExODA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 补充：UDP --- 用户数据报协议

## 报头格式：

![img](https://img-blog.csdn.net/20170309134130222?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzM5NTExODA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 特点：

（1）UDP是无连接的。

（2）UDP使用尽最大努力交付，即不保证可靠交付。

（3）UDP是面向报文的。发送方的UDP对应用程序交下来的报文，在添加首部后就直接交付给IP层，既不合并也不拆分，而是保留这些报文的边界。

（4）没有拥塞控制，因此网络出现拥塞时不会使源主机的发送速率很低。

（5）支持一对一、一对多、多对一、多对多的交互通信。

（6）首部开销小。

***\*总结一点：UDP是一个简单的面向数据报的运输层协议，它不提供可靠性服务。\****