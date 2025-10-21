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

## 4.Create a program to remove an existing message queue using the msgctl system call. Ensure that the program prompts the user for confirmation before deleting the message queue.
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#define KEY 19235
int main(){
        int msgid;
        char choice;
        msgid=msgget(KEY,0666|IPC_CREAT);
        if(msgid==-1){
                printf("couldnot access message queue\n");
                exit(1);
        }
        printf("Do you want to delete messege queue?(Y/N)\n");
        scanf(" %c",&choice);
        if(choice=='y' || choice=='Y'){
                if(msgctl(msgid,IPC_RMID,NULL)==-1){
                        printf("Error: couldnot remove messege queue\n");
                        exit(1);
                }
                printf("Message queue deleted successfully\n");
        }
        else{
                printf("Message queue deletion cancelled\n");
        }
}
```

## 5.Design a multithreaded program where threads communicate through named pipes.
```c 
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/stat.h>
#include<string.h>
#define FIFONAME "myfifo"
void* writerthread(void *arg){
        int fd;
        char buffer[100];
        fd=open(FIFONAME,O_WRONLY);
        if(fd==-1){
                perror("Error");
                pthread_exit(NULL);
        }
        for(int i=0;i<5;i++){
                snprintf(buffer,sizeof(buffer),"Message %d from writer",i+1);
                write(fd,buffer,strlen(buffer)+1);
                printf("Writer sent : %s\n",buffer);
                sleep(1);
        }
        close(fd);
        pthread_exit(NULL);
}
void* readerthread(void *arg){
        int fd;
        char buffer[100];
        fd=open(FIFONAME,O_RDONLY);
        if(fd==-1){
                perror("Error");
                pthread_exit(NULL);
        }
        for(int i=0;i<5;i++){
                read(fd,buffer,sizeof(buffer));
                printf("Reader received: %s\n",buffer);
        }
        close(fd);
        pthread_exit(NULL);
}

int main(){
        pthread_t reader,writer;
        if(mkfifo(FIFONAME,0666|O_CREAT)==-1){
                printf("Error");
        }
        pthread_create(&writer,NULL,writerthread,NULL);
        pthread_create(&reader,NULL,readerthread,NULL);
        pthread_join(writer,NULL);
        pthread_join(reader,NULL);
        unlink(FIFONAME);
}
```

## 6.Write a C program where two processes communicate using message queues. Implement sending and receiving messages between the processes using msgget, msgsnd, and msgrcv.
### Server :
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#define KEY 19236
#define SRMMSGTYPE 1
int main(){
        int msgid;
        printf("Waiting for the message\n");
        msgid=msgget(KEY,0666|IPC_CREAT);
        if(msgid==-1){
                printf("msgget Error");
                exit(1);
        }
        char rdbuf[100]={0};
        if(msgrcv(msgid,&rdbuf,100,SRMMSGTYPE,0)==-1){
                printf("msgrcv Error");
                exit(1);
        }
        printf("Message received:%s\n",rdbuf+8);
}
```
### Client :
```c
#include<string.h>
#include<stdlib.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#define KEY 19236
#define SRMMSGTYPE 1
int main(){
        int msgid;
        msgid=msgget(KEY,0);
        if(msgid==-1){
                printf("Error");
                exit(1);
        }
        char txmsg[100]={0};
        long *temp=(long *)txmsg;
        temp[0]=SRMMSGTYPE;
        printf("Enter the message:");
        scanf("%s",txmsg+8);
        if(msgsnd(msgid,txmsg,8+strlen(txmsg+8),0)==-1){
                printf("Msgsnd error");
                exit(1);
        }
        printf("Message sent\n");
}
```

## 7.Implement a program where two processes communicate synchronously using message queues. Ensure that one process waits for the other to finish before proceeding.
### sender :
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#include<unistd.h>
#define KEY 19237
#define MSGTYPEDATA 1
#define MSGTYPEACK 2
int main(){
        int msgid;
        char msg[100];
        long type;
        msgid=msgget(KEY,IPC_CREAT|0666);
        if(msgid==-1){
                printf("Cannot create message queue");
                exit(1);
        }
        while(1){
                printf("Enter the message:");
                fgets(msg,sizeof(msg),stdin);
                msg[strcspn(msg,"\n")]='\0';
                type=MSGTYPEDATA;
                if(msgsnd(msgid,&type,sizeof(msg),0)==-1){
                        printf("Sending failed\n");
                        exit(1);
                }
                type=MSGTYPEACK;
                if(msgrcv(msgid,&type,sizeof(msg),MSGTYPEACK,0)==-1){
                        printf("receiving failed");
                        exit(1);
                }
                printf("Reply from receiver:%s\n",msg);
                if(strcmp(msg,"exit")==0)
                        break;
        }
        msgctl(msgid,IPC_RMID,NULL);
}
```
### Receiver :
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#define KEY 19237
#define MSGTYPEDATA 1
#define MSGTYPEACK 2
int main(){
        int msgid;
        char msg[100];
        long type;
        msgid=msgget(KEY,0666);
        if(msgid==-1){
                printf("msgid");
                exit(1);
        }
        while(1){
                type=MSGTYPEDATA;
                if(msgrcv(msgid,&type,sizeof(msg),MSGTYPEDATA,0)==-1){
                        printf("receiving failed");
                        exit(1);
                }
                printf("Message from sender:%s\n",msg);
                printf("Enter reply:");
                fgets(msg,sizeof(msg),stdin);
                msg[strcspn(msg,"\n")]='\0';
                type=MSGTYPEACK;
                if(msgsnd(msgid,&type,sizeof(msg),0)==-1){
                        printf("sending failed\n");
                        exit(1);
                }
                if(strcmp(msg,"exit")==0)
                        break;
        }
}
```
