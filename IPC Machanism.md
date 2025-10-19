## 1.Implement a program that uses pipes for communication between a parent and child process. Show how data can be passed between processes using pipes.
## Create a program where two processes communicate synchronously using pipes.Ensure that one process waits for the other to finish before proceeding.
```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
int main(){
        int fds[2];
        int pid;
        pipe(fds);
        pid=fork();
        if(pid==0){
                close(fds[0]);
                char wrbuf[]="Hello";
                write(fds[1],wrbuf,strlen(wrbuf));
        }
        else{
                close(fds[1]);
                char rdbuf[32]={0};
                read(fds[0],rdbuf,32);
                printf("%s\n",rdbuf);
        }
}
```

## 2.Implement a program that uses Named pipes for communication between two processes.
### Server :
```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/stat.h>
int main(){
        int fd;
        mkfifo("srcfifo.txt",0666);
        printf("waiting for message from client\n");
        fd=open("srcfifo.txt",O_RDONLY,0);
        char rdbuf[32]={0};
        read(fd,rdbuf,32);
        printf("message received from client:%s\n",rdbuf);
        close(fd);
}
```
### Client :
```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd;
        fd=open("srcfifo.txt",O_WRONLY,O_CREAT,0);
        char wrbuf[]="Hello";
        write(fd,wrbuf,strlen(wrbuf));
        printf("message sent to server\n");
        close(fd);
}
```

## 3.Write a C program to create a message queue using the msgget system call. Ensure that the program checks for errors during the creation process.
### Server :
```c
#include<stdlib.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#define KEY 19238
#define SRVMSGTYPE 1
int main(){
        int msgid;
        printf("Waiting for message from client\n");
        msgid=msgget(KEY,0666|IPC_CREAT);
        if(msgid<0){
                printf("Error");
                exit(1);
        }
        char rdbuf[64]={0};
        if(msgrcv(msgid,rdbuf,sizeof(rdbuf),SRVMSGTYPE,0)==-1){
                printf("msgrcv failed");
                exit(1);
        }
        printf("Message received from client:%s\n",rdbuf+8);
}
```
### Client :
```c
#include<stdlib.h>
#include<sys/types.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#define KEY 19238
#define SRVMSGTYPE 1
int main(){
        int msgid;
        msgid=msgget(KEY,0);
        if(msgid<0){
                printf("Error");
                exit(1);
        }
        char wrbuf[64]={0};
        long *temp=(long *)wrbuf;
        temp[0]=SRVMSGTYPE;
        scanf("%s",wrbuf+8);
        if(msgsnd(msgid,wrbuf,8+strlen(wrbuf+8),0)==-1){
                perror("Error");
                exit(1);
        }
        printf("Message sent\n");
}
```


