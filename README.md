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
