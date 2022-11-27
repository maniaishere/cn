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

ftp
```
#include "stdio.h"
#include "stdlib.h"
#include "string.h"
//headers for socket and related functions
#include <sys/types.h>
#include <sys/socket.h>
#include <sys/stat.h>
//for including structures which will store information needed
#include <netinet/in.h>
#include <unistd.h>
//for gethostbyname
#include "netdb.h"
#include "arpa/inet.h"

// defining constants
#define PORT 9002
#define BACKLOG 5
int main()
{
    int size;
    int socketDescriptor = socket(AF_INET, SOCK_STREAM, 0);
    struct sockaddr_in serverAddress, clientAddress;
   
    socklen_t clientLength;
    
    struct stat statVariable;
   
    char buffer[100], file[1000];
    
    FILE *filePointer;
    bzero(&serverAddress, sizeof(serverAddress));
    
    serverAddress.sin_family = AF_INET;
    serverAddress.sin_addr.s_addr = htonl(INADDR_ANY);
    serverAddress.sin_port = htons(PORT);
    
    bind(socketDescriptor, (struct sockaddr *)&serverAddress, sizeof(serverAddress));
 
    listen(socketDescriptor,BACKLOG);

    printf("Server has started working ...");
    int clientDescriptor = accept(socketDescriptor,(struct sockaddr*)&clientAddress,&clientLength);

    while(1){
        bzero(buffer,sizeof(buffer));
        bzero(file,sizeof(file));
        recv(clientDescriptor,buffer,sizeof(buffer),0);
        filePointer = fopen(buffer,"r");
        stat(buffer,&statVariable);
        size=statVariable.st_size;
        fread(file,sizeof(file),1,filePointer);
        send(clientDescriptor,file,sizeof(file),0);
    }
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
#include <sys/stat.h>
//for including structures which will store information needed
#include <netinet/in.h>
#include <unistd.h>
//for gethostbyname
#include "netdb.h"
#include "arpa/inet.h"

// defining constants
#define PORT 9002

int main()
{
  
    int serverDescriptor = socket(AF_INET, SOCK_STREAM, 0);
    struct sockaddr_in serverAddress;

    char buffer[100], file[1000];
    
    bzero(&serverAddress, sizeof(serverAddress));
    serverAddress.sin_family = AF_INET;
    serverAddress.sin_addr.s_addr = inet_addr("127.0.0.1");
    serverAddress.sin_port = htons(PORT);

    connect(serverDescriptor,(struct sockaddr*)&serverAddress,sizeof(serverAddress));
    
    while (1){
        printf("File name : ");
        scanf("%s",buffer);
        send(serverDescriptor,buffer,strlen(buffer)+1,0);
        printf("%s\n","File Output : ");
        recv(serverDescriptor,&file,sizeof(file),0);
        printf("%s",file);
    }
    return 0;
}                                 
```
remote
```
#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <stdlib.h>
#include <netdb.h>
#include <netinet/in.h>
#include <string.h>
#include <sys/stat.h>
#include <arpa/inet.h>
#include <unistd.h>
#define MAX 1000
int main()
{

    int serverDescriptor = socket(AF_INET, SOCK_DGRAM, 0);
    int size;
    char buffer[MAX], message[] = "Command Successfully executed !";
    struct sockaddr_in clientAddress, serverAddress;

    socklen_t clientLength = sizeof(clientAddress);

    bzero(&serverAddress, sizeof(serverAddress));
    serverAddress.sin_family = AF_INET;
    serverAddress.sin_addr.s_addr = htonl(INADDR_ANY);
    serverAddress.sin_port = htons(9976);

    bind(serverDescriptor, (struct sockaddr *)&serverAddress, sizeof(serverAddress));
    while (1)
    {
        bzero(buffer, sizeof(buffer));
        recvfrom(serverDescriptor, buffer, sizeof(buffer), 0, (struct sockaddr *)&clientAddress, &clientLength);
        system(buffer);
        printf("Command Executed ... %s ", buffer);
        sendto(serverDescriptor, message, sizeof(message), 0, (struct sockaddr *)&clientAddress, clientLength);
    }
    close(serverDescriptor);
    return 0;
}
```

```
#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <unistd.h>
#include <netdb.h>
#include <netinet/in.h>
#include <string.h>
#include <arpa/inet.h>
#define MAX 1000

int main()
{
    int serverDescriptor = socket(AF_INET, SOCK_DGRAM, 0);
    char buffer[MAX], message[MAX];
    struct sockaddr_in cliaddr, serverAddress;
    socklen_t serverLength = sizeof(serverAddress);

    bzero(&serverAddress, sizeof(serverAddress));
    serverAddress.sin_family = AF_INET;
    serverAddress.sin_addr.s_addr = inet_addr("127.0.0.1");
    serverAddress.sin_port = htons(9976);

    bind(serverDescriptor, (struct sockaddr *)&serverAddress, sizeof(serverAddress));

    while (1)
    {
        printf("\nCOMMAND FOR EXECUTION ... ");
        fgets(buffer, sizeof(buffer), stdin);
        sendto(serverDescriptor, buffer, sizeof(buffer), 0, (struct sockaddr *)&serverAddress, serverLength);
        printf("\nData Sent !");
        recvfrom(serverDescriptor, message, sizeof(message), 0, (struct sockaddr *)&serverAddress, &serverLength);
        printf("UDP SERVER : %s", message);
    }
    return 0;
}
```

arp
```
#include<sys/types.h>
#include<sys/socket.h>
#include<net/if_arp.h>
#include<sys/ioctl.h>
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<math.h>
#include<complex.h>
#include<arpa/inet.h>
#include<netinet/in.h>
#include<netinet/if_ether.h>
#include<net/ethernet.h>
#include<stdlib.h>
int main()
{
	struct sockaddr_in sin={0};
	struct arpreq myarp={{0}};
	unsigned char *ptr;
	int sd;
	sin.sin_family=AF_INET;
	printf("Enter IP address: ");
	char ip[20];
	scanf("%s", ip); if(inet_pton(AF_INET,ip,&sin.sin_addr)==0) {
		printf("IP address Entered '%s' is not valid \n",ip);
		exit(0); 
	}
	memcpy(&myarp.arp_pa,&sin,sizeof(myarp.arp_pa));
       	strcpy(myarp.arp_dev,"echo");
       	sd=socket(AF_INET,SOCK_DGRAM,0);
       	printf("\nSend ARP request\n");
       	if(ioctl(sd,SIOCGARP,&myarp)==1)
	{
		printf("No Entry in ARP cache for '%s'\n",ip);
		exit(0);
       	}
	ptr=&myarp.arp_pa.sa_data[0];
	printf("Received ARP Reply\n");
	printf("\nMAC Address for '%s' : ",ip);
       	printf("%p:%p:%p:%p:%p:%p\n",ptr,(ptr+1),(ptr+2),(ptr+3),(ptr+4),(ptr+5));
       	return 0;
}
```

udp echo
```
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<netinet/in.h>
#include<unistd.h>
// time

#define MAXLINE 1024
#define PORT 5035

int main(){

  int socketDescriptor = socket(AF_INET, SOCK_DGRAM, 0);
  int number;
  socklen_t addressLength;
  char message[MAXLINE];

  struct sockaddr_in  serverAddress,clientAddress;
  serverAddress.sin_family = AF_INET;
  serverAddress.sin_addr.s_addr=INADDR_ANY;
  serverAddress.sin_port=htons(PORT);

  bind(socketDescriptor,(struct sockaddr*)&serverAddress,sizeof(serverAddress));

  printf("\nServer Started ...\n");

  while(1){
    printf("\n");
    addressLength = sizeof(clientAddress);

    number = recvfrom(socketDescriptor,message,MAXLINE,0,(struct sockaddr*)&clientAddress,&addressLength);

    printf("\n Client's Message: %s ",message);

    if(number<6)
      perror("send error");

      sendto(socketDescriptor,message,number,0,(struct sockaddr*)&clientAddress,addressLength);
  }
  return 0;
}
```
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>

#define MAXLINE 1024
#define PORT 5035

int main(){
  // socket descriptor creation in udp mode
  int serverDescriptor = socket(AF_INET, SOCK_DGRAM, 0);
  
  // for storing  address of address
  socklen_t addressLength;
  
  // preparing message 
  char sendMessage[MAXLINE],recvMessage[MAXLINE];
  printf("\nEnter message :");
  fgets(sendMessage,sizeof(sendMessage),stdin);
  
  // storing address in serverAddress
  struct sockaddr_in serverAddress;
  serverAddress.sin_family = AF_INET;
  serverAddress.sin_addr.s_addr = INADDR_ANY;
  serverAddress.sin_port = htons(PORT);
  
  // storing address size 
  addressLength = sizeof(serverAddress);

  // checking connection
  connect(serverDescriptor,(struct sockaddr*)&serverAddress,addressLength);
  
  // sending and receiving the messages 
  sendto(serverDescriptor,sendMessage,MAXLINE,0,(struct sockaddr*)&serverAddress,addressLength);
  recvfrom(serverDescriptor,recvMessage,MAXLINE,0,NULL,NULL);
 
  printf("\nServer's Echo : %s\n",recvMessage);

  return 0;
}
```
FTP-2
```
#include <stdio.h>

#include <stdlib.h>

#include <string.h>

#include <arpa/inet.h>

#define SIZE 1024


void write_file(int sockfd){

  int n;

  FILE *fp;

  char *filename = "recv.txt";

  char buffer[SIZE];


  fp = fopen(filename, "w");

  while (1) {

    n = recv(sockfd, buffer, SIZE, 0);

    if (n <= 0){

      break;

      return;

    }

    fprintf(fp, "%s", buffer);

    bzero(buffer, SIZE);

  }

  return;

}


int main(){

  char *ip = "127.0.0.1";

  int port = 7000;

  int e;


  int sockfd, new_sock;

  struct sockaddr_in server_addr, new_addr;

  socklen_t addr_size;

  char buffer[SIZE];


  sockfd = socket(AF_INET, SOCK_STREAM, 0);

  if(sockfd < 0) {

    perror("[-]Error in socket");

    exit(1);

  }

  printf("[+]Server socket created successfully.\n");


  server_addr.sin_family = AF_INET;

  server_addr.sin_port = port;

  server_addr.sin_addr.s_addr = inet_addr(ip);


  e = bind(sockfd, (struct sockaddr*)&server_addr, sizeof(server_addr));

  if(e < 0) {

    perror("[-]Error in bind");

    exit(1);

  }

  printf("[+]Binding successfull.\n");


  if(listen(sockfd, 10) == 0){

printf("[+]Listening....\n");

}else{

perror("[-]Error in listening");

    exit(1);

}


  addr_size = sizeof(new_addr);

  new_sock = accept(sockfd, (struct sockaddr*)&new_addr, &addr_size);

  write_file(new_sock);

  printf("[+]Data written in the file successfully.\n");


  return 0;

}

```
```
#include <stdio.h>

#include <stdlib.h>

#include <unistd.h>

#include <string.h>

#include <arpa/inet.h>

#define SIZE 1024


void send_file(FILE *fp, int sockfd){

  int n;

  char data[SIZE] = {0};


  while(fgets(data, SIZE, fp) != NULL) {

    if (send(sockfd, data, sizeof(data), 0) == -1) {

      perror("[-]Error in sending file.");

      exit(1);

    }

    bzero(data, SIZE);

  }

}


int main(){

  char *ip = "127.0.0.1";

  int port = 7000;

  int e;


  int sockfd;

  struct sockaddr_in server_addr;

  FILE *fp;

  char *filename = "send.txt";


  sockfd = socket(AF_INET, SOCK_STREAM, 0);

  if(sockfd < 0) {

    perror("[-]Error in socket");

    exit(1);

  }

  printf("[+]Server socket created successfully.\n");


  server_addr.sin_family = AF_INET;

  server_addr.sin_port = port;

  server_addr.sin_addr.s_addr = inet_addr(ip);


  e = connect(sockfd, (struct sockaddr*)&server_addr, sizeof(server_addr));

 if(e == -1) {

    perror("[-]Error in socket");

    exit(1);

  }

printf("[+]Connected to Server.\n");


  fp = fopen(filename, "r");

  if (fp == NULL) {

    perror("[-]Error in reading file.");

    exit(1);

  }


  send_file(fp, sockfd);

  printf("[+]File data sent successfully.\n");


  printf("[+]Closing the connection.\n");

  close(sockfd);


  return 0;

}
```
