## 1.Write a C program to catch and handle the SIGINT signal.
```c
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
#include<stdlib.h>
void mysighandler(int signo){
        printf("My signal number %d\n",signo);
        sleep(3);
}
int main(){
        signal(SIGINT,mysighandler);
        while(1){
                printf("Sleeping\n");
                sleep(10);
        }
}
```

## 2.Create a C program to ignore the SIGCHLD signal temporarily.
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
#include<sys/wait.h>
int main(){
        int pid;
        printf("Parent PID: %d\n",getpid());
        signal(SIGCHLD,SIG_IGN);
        printf("SIGCHLD is now being ignored.\n");
        pid=fork();
        if(pid==0){
                printf("Child(PID %d) exiting...\n",getpid());
                exit(0);
        }
        else{
                sleep(2);
                printf("Child exited,but parent ignored SIGCHLD.\n");
                signal(SIGCHLD,SIG_DFL);
                printf("SIGCHLD restored to default.\n");
                pid=fork();
                if(pid==0){
                        printf("Second child(PID %d)exiting...\n",getpid());
                        exit(0);
                }
                else{
                        sleep(2);
                        printf("Now parent can reap child normally.\n");
                        wait(NULL);
                }
        }
}
```

