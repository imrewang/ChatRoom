# ChatRoom

简易聊天室

项目内容： 本项目使用C ++实现一个具备服务器端和客户端即时通信且具有私聊功能的聊天室 目的是学习C++网络开发的基本概念，同时也熟悉了Linux下的C++程序编译和简单MakeFile的编写

# 一、服务端

## （一）简介

服务端：能够接受新的客户连接，并将每个客户端发来的信息，广播给对应的目标客户端 

## （二）部署

### 1.环境

ubuntu

C++

### 2.部署截图


## （三）运行截图


## （四）总结

### 1.支持多个客户端接入，实现聊天室基本功能

list<int> clients_list;                   //客户端列表

### 2.启动服务，建立监听端口等待客户端连接 

int sockfd = events[i].data.fd;
			
//新用户连接
if(sockfd == listener)

### 3.使用epoll机制实现并发，增加效率 

//在内核中创建事件表  epfd是一个句柄(红黑树的树根)
epfd = epoll_create(EPOLL_SIZE);

### 4.客户端连接时，发送欢迎消息，并存储连接记录

clients_list.push_back(clientfd);

### 5.客户端发送消息时，根据消息类型，广播给所有用户(群聊)或者指定用户(私聊) 

if(msg.content[0]=='\\'&&isdigit(msg.content[1]))

### 6.客户端请求退出时，对相应连接信息进行清理 

if(strncasecmp(msg.content,EXIT,strlen(EXIT))==0){
		isClientwork = 0;
}

## （五）改进方向

1.信息结构体为
~~~
//定义信息结构，在服务端和客户端之间传送
struct Msg
{
	int type;             //消息类别//0是群聊，1是私聊
	int fromID;           //发送方FD
	int toID;           //接收方FD
	char content[BUF_SIZE];      //文字信息
};
~~~
但实际上type、fromID、toID冗余，但是还是没有删去，适应未来新情况

2.增加在服务端控制服务端关闭的选择，不是让它死循环or阻塞

# 二、客户端

## （一）简介

能够连接服务器，并向服务器发送消息，同时可以接受服务器发来的消息 

## （二）部署

### 1.环境

ubuntu

C++

### 2.部署截图


## （三）运行截图


## （四）总结

### 1.连接服务器 

connect(sock,(struct sockaddr*)&serverAddr,sizeof(serverAddr))

### 2.支持用户输入消息，发送给服务端 

write(pipe_fd[1],send_buf,sizeof(send_buf))

int ret = read(events[i].data.fd,recv_buf,BUF_SIZE);

send(sock,recv_buf,sizeof(recv_buf),0);

### 3.接受并显示服务端发来的消息 

int ret = recv(sock,recv_buf,BUF_SIZE,0);

cout<<msg.content<<endl;

### 4.退出连接

strncasecmp(msg.content,EXIT,strlen(EXIT))==0

### 客户端需要两个进程分别支持以下功能：

子进程： 1.等待用户输入信息 2.将聊天信息写入管道(pipe),并发送给父进程 

父进程:  1.使用epoll机制接收服务端发来的消息，并显示给用户，使用户看到其他用户的信息 2.将子进程发送的聊天信息从管道(pipe)中读取出来，并发送给客户端


## （五）改进方向

### 1.支持对无头消息的发送与接收

