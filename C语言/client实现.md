~~~c
#include <stdio.h>
#include<stdlib.h>
#include<arpa/inet.h>
#include<ctype.h>
#include<sys/socket.h>
#include<unistd.h>
#include<string.h>
#define SERV_PORT 6666
#define SERV_IP "127.0.0.1"
int main(void)
{
    int cfd;
    char buf[BUFSIZ];
    struct sockaddr_in serv_addr;
    cfd =  socket(AF_INET,SOCK_STREAM,0);
    memset(&serv_addr,0,sizeof(serv_addr));
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    inet_pton(AF_INET,SERV_IP,&serv_addr.sin_addr.s_addr);
    connect(cfd,(struct sockaddr*)&serv_addr,sizeof(serv_addr));
    
    fgets(buf,sizeof(buf),stdin);
    
    write(cfd,buf,strlen(buf));

   int n =  read(cfd,buf,sizeof(buf));
   write(STDOUT_FILENO,buf,n);
   
    return 0;
}



~~~

