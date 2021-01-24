~~~c
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>
//服务器的实现代码
#define MAX 100
//定义客户端数据结构
typedef struct Client{
	int cfd;//客户端标识
	char name[40];//客户端姓名
}Client;
//客户端最大数量级初始化
Client client[MAX] = {};
size_t cnt = 0;

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
//广播函数，用于将信息发送给其他好友
void broadcast(char *msg,Client c){//传入信息，以及需要发送的客户端信息，每次都需要一个客户端信息发送完成才能发送下一个客户端的信息
	size_t i;
	pthread_mutex_lock(&mutex);//枷锁
	for(i=0;i<cnt;i++){
		if(client[i].cfd != c.cfd){//只要不是自己的那个标识的，其他客户端都需要发送一遍
			if(send(client[i].cfd,msg,strlen(msg),0)<=0){//发送到客户端的信息只需要知道客户端的标识符号和信息就可以
				break;
			}
		}
	}
	pthread_mutex_unlock(&mutex);//解锁
}

void *pthread_run(void *arg){//线程执行函数
	Client cl = *(Client*)(arg);
	while(1){
		char buf[1024]={};
		strcpy(buf,cl.name);
		strcat(buf," :");
		int ret = recv(cl.cfd,buf+strlen(buf),1024-strlen(buf),0);
		if(ret <= 0){
			size_t i;
			for(i=0;i<cnt;i++){
				if(client[i].cfd == cl.cfd){
					client[i] = client[cnt-1];
					--cnt;
					strcpy(buf,"您的好友");
					strcat(buf,cl.name);
					strcat(buf,"退出了");
					break;
				}	
			}
			broadcast(buf,cl);
			close(cl.cfd);
			return NULL;
		}else{
			broadcast(buf,cl);
		}
	}
}

int main(int argc,char *argv[]){
	if(argc != 3){
		fprintf(stderr,"use: %s <ip> [port]\n",argv[0]);
		return -1;
	}
	const char *ip = argv[1];
	unsigned short int port = atoi(argv[2]);

	int sfd = socket(AF_INET,SOCK_STREAM,0);
	if(sfd == -1){
		perror("socket");
		return -1;
	}

	struct sockaddr_in addr;
	addr.sin_family = AF_INET;
	addr.sin_port = htons(port);
	addr.sin_addr.s_addr = inet_addr(ip);
	socklen_t addrlen = sizeof(addr);

	int ret = bind(sfd,(struct sockaddr*)(&addr),addrlen);
	if(ret == -1){
		perror("bind");
		return -1;
	}
	if(listen(sfd,10)==-1){
		perror("listen");
		return -1;
	}
	while(1){
		struct sockaddr_in caddr;
		socklen_t len = sizeof(caddr);
		
		printf("等待客户机连接....\n");
		int cfd = accept(sfd,(struct sockaddr*)(&caddr),&len);//获取到客户端的socket标识符号
		if(cfd == -1){
			perror("accept");
			return -1;
		}
		char buf[100]={};
		recv(cfd,&client[cnt].name,40,0);
		client[cnt].cfd = cfd;//设置客户端的标识符号和名字
		pthread_t id;//线程ID
		strcpy(buf,"您的好友");
		strcat(buf,client[cnt].name);
		strcat(buf,"上线啦");
		broadcast(buf,client[cnt]);
		ret = pthread_create(&id,NULL,pthread_run,(void*)(&client[cnt]));
		cnt++;
		if(ret != 0){
			printf("pthread_create:%s\n",strerror(ret));
			continue;
		}
		printf("有一个客户机成功连接:ip <%s> port [%hu]\n",inet_ntoa(caddr.sin_addr),ntohs(caddr.sin_port));
	}
	return 0;
}


~~~

~~~c
#include <stdio.h>
#include <time.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>


int main(int argc,char *argv[]){
	if(argc != 3){
		fprintf(stderr,"use: %s <ip> [port]\n",argv[0]);	
		return -1;
	}
	const char *ip = argv[1];
	unsigned short int port = atoi(argv[2]);

	int sfd = socket(AF_INET,SOCK_STREAM,0);
	if(sfd == -1){
		perror("socket");
		return -1;
	}
	struct sockaddr_in addr;
	addr.sin_family = AF_INET;
	addr.sin_port = htons(port);
	addr.sin_addr.s_addr = inet_addr(ip);
	socklen_t addrlen = sizeof(addr);

	int ret = connect(sfd,(const struct sockaddr*)(&addr),addrlen);
	if(ret == -1){
		perror("connect");
		return -1;
	}
	printf("连接服务器成功!\n");
	printf("请输入你的网民:");
	char name[100];
	gets(name);
	send(sfd,name,strlen(name)+1,0);
	pid_t pid = fork();//进程的申请
	if(pid == -1){
		perror("fork");	
		return -1;
	}
	if(pid == 0){
		while(1){
			char buf[1024]={};
			gets(buf);
			if(send(sfd,buf,strlen(buf)+1,0)<=0){
				break;	
			}
		}
	}else{
		while(1){
			char buf[1024]={};
			if(recv(sfd,buf,1024,0)<=0){
				break;
			}
			time_t timep;
			time(&timep);
			printf("%s\n",ctime(&timep));
			printf("%s\n",buf);
		}
	}
	close(sfd);
	return 0;	
}


~~~

