TCP SERVER-CLIENT
```
#include <stdio.h>
#include <unistd.h>
#include <netinet/in.h>
#include <string.h>
#include <sys/socket.h>
#include <stdlib.h>
#define PORT 8000
int main (int argc, char const *argv[] )
{
int obj_server, sock, reader;
struct sockaddr_in address;
int opted = 1;
int address_length = sizeof(address);
char buffer[1024] = {0};
char *message= "A Hello Message from server !";
if (( obj_server = socket ( AF_INET, SOCK_STREAM, 0)) == 0)
{
perror( "Opening of Socket Failed !");
exit(EXIT_FAILURE);
}
if (setsockopt(obj_server, SOL_SOCKET, SO_REUSEADDR,&opted, sizeof ( opted )))
{
perror("Can't set the socket" );
exit (EXIT_FAILURE );
}
address.sin_family = AF_INET;
address.sin_addr.s_addr = INADDR_ANY;
address.sin_port = htons( PORT );
if (bind(obj_server, ( struct sockaddr * )&address, sizeof(address))<0)
{
perror ( "Binding of socket failed !" );
exit(EXIT_FAILURE);
}
if (listen ( obj_server, 3) < 0)
{
perror ( "Can't listen from the server !");
exit(EXIT_FAILURE);
}
if ((sock = accept(obj_server, (struct sockaddr *)&address, (socklen_t*)&address_length)) < 0)

{
perror("Accept");
exit(EXIT_FAILURE);
}
reader = read(sock, buffer, 1024);
printf("%s\n", buffer);
send(sock , message, strlen(message) , 0 );
printf("Server : Message has been sent ! \n");
return 0;
}
```

```
#include <stdio.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <string.h>
#define PORT 8000
int main (int argc, char const *argv[] )
{
int obj_socket = 0, reader;
struct sockaddr_in serv_addr;
char *message = "A Hello message from Client !";
char buffer[1024] = {0};
if (( obj_socket = socket (AF_INET, SOCK_STREAM, 0 )) < 0)
{
printf ( "Socket creation error !" );
return -1;
}
serv_addr.sin_family = AF_INET;
serv_addr.sin_port = htons(PORT);
if(inet_pton( AF_INET, "127.0.0.1", &serv_addr.sin_addr)<=0)
{
printf ( "\nInvalid address ! This IP Address is not supported !\n" );
return -1;
}
if (connect( obj_socket, (struct sockaddr *)&serv_addr, sizeof(serv_addr )) < 0)
{
printf ( "Connection Failed : Can't establish a connection over this socket !" );
return -1;
}
send( obj_socket, message, strlen(message) , 0 );
printf ( "\nClient : Message has been sent !\n" );

reader = read ( obj_socket, buffer, 1024 );
printf ( "%s\n",buffer );
return 0;
}
```

DATE-TIME
```
#include<netinet/in.h>
#include<sys/socket.h>
#include<stdio.h>
#include<string.h>
#include<time.h>
int main( )
{
struct sockaddr_in sa;
struct sockaddr_in cli;
int sockfd,conntfd;
int len,ch;
char str[100];
time_t tick;
sockfd=socket(AF_INET,SOCK_STREAM,0);
if(sockfd<0)
{
printf("error in socket\n");
exit(0);
}
else printf("Socket opened");
bzero(&sa,sizeof(sa));
sa.sin_port=htons(5600);
sa.sin_addr.s_addr=htonl(0);
if(bind(sockfd,(struct sockaddr*)&sa,sizeof(sa))<0)
{
printf("Error in binding\n");
}
else
printf("Binded Successfully");
listen(sockfd,50);
for(;;)
{
len=sizeof(ch);
conntfd=accept(sockfd,(struct sockaddr*)&cli,&len);
printf("Accepted");
tick=time(NULL);
snprintf(str,sizeof(str),"%s",ctime(&tick));
printf("%s",str);write(conntfd,str,100);
}
}
```
```
#include"netinet/in.h"
#include"sys/socket.h"
#include"stdio.h"
main()
{
struct sockaddr_in sa,cli;
int n,sockfd;

int len;char buff[100];
sockfd=socket(AF_INET,SOCK_STREAM,0);
if(sockfd<0)
{ printf("\nError in Socket");
exit(0);
}
else printf("\nSocket is Opened");
bzero(&sa,sizeof(sa));
sa.sin_family=AF_INET;
sa.sin_port=htons(5600);
if(connect(sockfd,(struct sockaddr*)&sa,sizeof(sa))<0)
{
printf("\nError in connection failed");
exit(0);
}
else
printf("\nconnected successfully");
if(n=read(sockfd,buff,sizeof(buff))<0)
{
printf("\nError in Reading");
exit(0);
}
else
{printf("\nMessage Read %s",buff);
}}
```

half-duplex
```
#include "stdio.h"
#include "stdlib.h"
#include "string.h"
//headers for socket and related functions
#include <sys/types.h>
#include <sys/socket.h>
//for including structures which will store information needed
#include <netinet/in.h>
#include <unistd.h>
//for gethostbyname
#include "netdb.h"
#include "arpa/inet.h"
#define MAX 1000
#define BACKLOG 5 // how many pending connections queue will hold
int main()
{
    char serverMessage[MAX];
    char clientMessage[MAX];
    //create the server socket
    int  socketDescriptor = socket(AF_INET, SOCK_STREAM, 0);
    

    struct sockaddr_in serverAddress;
    serverAddress.sin_family = AF_INET;
    serverAddress.sin_port = htons(9002);
    serverAddress.sin_addr.s_addr = INADDR_ANY;

    //calling bind function to oir specified IP and port
    bind(socketDescriptor, (struct sockaddr*)&serverAddress, sizeof(serverAddress));    

    listen(socketDescriptor, BACKLOG);
    
    //starting the accepting
    int clientSocketDescriptor = accept(socketDescriptor, NULL, NULL);

    while (1)
    {
        printf("\ntext message here .. :");
        scanf("%s", serverMessage);
        send(clientSocketDescriptor, serverMessage, sizeof(serverMessage) , 0);
        //recieve the data from the server
        recv(clientSocketDescriptor, &clientMessage, sizeof(clientMessage), 0) ;
        //recieved data from the server successfully then printing the data obtained from the server
        printf("\nCLIENT: %s", clientMessage);
       
    }
        //close the socket
        close(socketDescriptor);
        return 0;
}
```

```
#include "stdio.h"
#include "stdlib.h"
#include "string.h"
//headers for socket and related functions
#include <sys/types.h>
#include <sys/socket.h>
//for including structures which will store information needed
#include <netinet/in.h>
#include <unistd.h>
//for gethostbyname
#include "netdb.h"
#include "arpa/inet.h"

int main()
{
int socketDescriptor;

struct sockaddr_in serverAddress;
char sendBuffer[1000],recvBuffer[1000];

pid_t cpid;

bzero(&serverAddress,sizeof(serverAddress));

serverAddress.sin_family=AF_INET;
serverAddress.sin_addr.s_addr=inet_addr("127.0.0.1");
serverAddress.sin_port=htons(5500);

/*Creating a socket, assigning IP address and port number for that socket*/
socketDescriptor=socket(AF_INET,SOCK_STREAM,0);

/*Connect establishes connection with the server using server IP address*/
connect(socketDescriptor,(struct sockaddr*)&serverAddress,sizeof(serverAddress));

/*Fork is used to create a new process*/
cpid=fork();
if(cpid==0)
{
while(1)
{
bzero(&sendBuffer,sizeof(sendBuffer));
printf("\nType a message here ...  ");
/*This function is used to read from server*/
fgets(sendBuffer,10000,stdin);
/*Send the message to server*/
send(socketDescriptor,sendBuffer,strlen(sendBuffer)+1,0);
printf("\nMessage sent !\n");
}
}
else
{
while(1)
{
bzero(&recvBuffer,sizeof(recvBuffer));
/*Receive the message from server*/
recv(socketDescriptor,recvBuffer,sizeof(recvBuffer),0);
printf("\nSERVER : %s\n",recvBuffer);
}
}
return 0;
}
```

full duplex

```
#include<sys/types.h>
#include<sys/socket.h>
#include<stdio.h>
#include<unistd.h>
#include<netdb.h>
#include<arpa/inet.h>
#include<netinet/in.h>
#include<string.h>
#include<strings.h>

int main(int argc,char *argv[])
{
int clientSocketDescriptor,socketDescriptor;

struct sockaddr_in serverAddress,clientAddress;
socklen_t clientLength;

char recvBuffer[1000],sendBuffer[1000];
pid_t cpid;
bzero(&serverAddress,sizeof(serverAddress));
/*Socket address structure*/
serverAddress.sin_family=AF_INET;
serverAddress.sin_addr.s_addr=htonl(INADDR_ANY);
serverAddress.sin_port=htons(5500);
/*TCP socket is created, an Internet socket address structure is filled with
wildcard address & server’s well known port*/
socketDescriptor=socket(AF_INET,SOCK_STREAM,0);
/*Bind function assigns a local protocol address to the socket*/
bind(socketDescriptor,(struct sockaddr*)&serverAddress,sizeof(serverAddress));
/*Listen function specifies the maximum number of connections that kernel should queue
for this socket*/
listen(socketDescriptor,5);
printf("%s\n","Server is running ...");
/*The server to return the next completed connection from the front of the
completed connection Queue calls it*/
clientSocketDescriptor=accept(socketDescriptor,(struct sockaddr*)&clientAddress,&clientLength);
/*Fork system call is used to create a new process*/
cpid=fork();

if(cpid==0)
{
while(1)
{
bzero(&recvBuffer,sizeof(recvBuffer));
/*Receiving the request from client*/
recv(clientSocketDescriptor,recvBuffer,sizeof(recvBuffer),0);
printf("\nCLIENT : %s\n",recvBuffer);
}
}
else
{
while(1)
{

bzero(&sendBuffer,sizeof(sendBuffer));
printf("\nType a message here ...  ");
/*Read the message from client*/
fgets(sendBuffer,10000,stdin);
/*Sends the message to client*/
send(clientSocketDescriptor,sendBuffer,strlen(sendBuffer)+1,0);
printf("\nMessage sent !\n");
}
}
return 0;
}
```

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <strings.h>

//headers for socket and related functions
#include <sys/types.h>
#include <sys/socket.h>
//for including structures which will store information needed
#include <netinet/in.h>
#include <unistd.h>
//for gethostbyname
#include <netdb.h>
#include <arpa/inet.h>

int main()
{
int socketDescriptor;

struct sockaddr_in serverAddress;
char sendBuffer[1000],recvBuffer[1000];

pid_t cpid;

bzero(&serverAddress,sizeof(serverAddress));

serverAddress.sin_family=AF_INET;
serverAddress.sin_addr.s_addr=inet_addr("127.0.0.1");
serverAddress.sin_port=htons(5500);

/*Creating a socket, assigning IP address and port number for that socket*/
socketDescriptor=socket(AF_INET,SOCK_STREAM,0);

/*Connect establishes connection with the server using server IP address*/
connect(socketDescriptor,(struct sockaddr*)&serverAddress,sizeof(serverAddress));

/*Fork is used to create a new process*/
cpid=fork();
if(cpid==0)
{
while(1)
{
bzero(&sendBuffer,sizeof(sendBuffer));
printf("\nType a message here ...  ");
/*This function is used to read from server*/
fgets(sendBuffer,10000,stdin);
/*Send the message to server*/
send(socketDescriptor,sendBuffer,strlen(sendBuffer)+1,0);
printf("\nMessage sent !\n");
}
}
else
{
while(1)
{
bzero(&recvBuffer,sizeof(recvBuffer));
/*Receive the message from server*/
recv(socketDescriptor,recvBuffer,sizeof(recvBuffer),0);
printf("\nSERVER : %s\n",recvBuffer);
}
}
return 0;
}
```
