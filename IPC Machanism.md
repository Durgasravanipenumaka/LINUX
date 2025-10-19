## 1.Implement a program that uses pipes for communication between a parent and child process. Show how data can be passed between processes using pipes.
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


