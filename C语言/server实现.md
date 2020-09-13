~~~c
#include <stdio.h>
#include <unistd.h>
#include <sys/socket.h>
#include <stdlib.h>
#include<ctype.h>
#include<arpa/inet.h>
#define SERV_PORT 6666
#define SERV_IP "127.0.0.1"
int main()
{   char buf[BUFSIZ];
    int lfd,cfd;
    int n;
    struct sockaddr_in serv_addr ,clie_addr;
    socklen_t clie_addr_len;
    lfd = socket (AF_INET,SOCK_STREAM ,0);
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(SERV_PORT);
    serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    bind (lfd,(struct sockaddr*)&serv_addr,sizeof(serv_addr) );
    listen(lfd,128);
    clie_addr_len = sizeof(clie_addr);
    cfd = accept(lfd,(struct sockaddr*)&clie_addr,&clie_addr_len);
    n = read(cfd,buf,sizeof(buf));
    while(1){

    for(int i= 0;i<n;i++){
        buf[i] = toupper(buf[i]);
    }
    write(cfd,buf,n);
    }
    close(lfd);
    close(cfd);
    return 0; 
}


~~~

