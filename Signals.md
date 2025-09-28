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

## 3.Write a program to block the SIGTERM signal using sigprocmask().
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<signal.h>
void mysighandler(int signo){
        printf("My signalhandler:%d",signo);
        sleep(10);
}
int main(){
        sigset_t msk;
        signal(SIGTERM,mysighandler);
        sigemptyset(&msk);
        sigaddset(&msk,SIGTERM);
        sigprocmask(SIG_BLOCK,&msk,NULL);
        printf("SIGTERM is now blocked\n");
        sleep(30);
        sigprocmask(SIG_UNBLOCK,&msk,NULL);
        printf("SIGTERM is now unblocked\n");
        sleep(30);
}
```

## 4.Implement a C program to handle the SIGALRM signal using sigaction().
```c
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
void mysighandler(int signo){
        printf("Mysignal handler:%d\n",signo);
        sleep(5);
}
int main(){
        struct sigaction act;
        act.sa_handler=mysighandler;
        act.sa_flags=0;
        sigemptyset(&act.sa_mask);
        sigaction(SIGALRM,&act,NULL);
        printf("Setting alarm for 5 seconds....\n");
        alarm(5);
        printf("Waiting...\n");
        pause();
        printf("program finished.\n");
}
```

## 5.Write a C program to install a custom signal handler for SIGTERM?
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
        signal(SIGTERM,mysighandler);
        while(1){
                printf("Sleeping\n");
                sleep(10);
        }
}
```

