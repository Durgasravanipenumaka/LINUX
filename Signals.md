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

## 6.Implement a program to handle the SIGSEGV signal (segmentation fault).
```c
#include<stdio.h>
#include<unistd.h>
#include<signal.h>
#include<stdlib.h>
void sighandler(int signo){
        printf("signal Number:%d",signo);
        exit(0);
}
int main(){
        signal(SIGSEGV,sighandler);
        printf("Program started(PID = %d):\n",getpid());
        printf("Now causing a segmentation fault...\n");
        sleep(2);
        int *ptr=NULL;
        *ptr=100;
        printf("This line will not executed!\n");
}
```

## 7.Create a program to handle the SIGILL signal (illegal instruction).
```c
#include<stdio.h>
#include<unistd.h>
#include<signal.h>
#include<stdlib.h>
void sighandler(int signo){
        printf("signal Number:%d\n",signo);
        exit(1);
}
int main(){
        signal(SIGILL,sighandler);
        printf("Program started(PID = %d)\n",getpid());
        printf("Now causing a Illegal instruction...\n");
        sleep(2);
        __asm__("ud2");
        printf("This line will not executed!\n");
}
```

## 8.Write a program to handle the SIGABRT signal (abort).
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void sighandler(int signo){
        printf("Sighandler:%d\n",signo);
        exit(0);
}
int main(){
        struct sigaction act;
        act.sa_handler=sighandler;
        sigemptyset(&act.sa_mask);
        act.sa_flags=0;
        sigaction(SIGABRT,&act,NULL);
        printf("Program started(PID : %d)\n",getpid());
        sleep(1);
        printf("Now raising SIGABRT...\n");
        sleep(1);
        abort();
        printf("This line will not execute!\n");
}
```

## 9.Implement a C program to handle the SIGQUIT signal.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void sighandler(int signo){
        printf("Signal Handler:%d\n",signo);
        exit(0);
}
int main(){
        signal(SIGQUIT,sighandler);
        printf("Program started(PID : %d)\n",getpid());
        printf("Press Ctrl+\\ to send SIGQUIT\n");
        while (1) {
              printf("Running... Press Ctrl+\\ to quit\n");
              sleep(5);
        }
}
```

## 10.Write a program to handle the SIGTERM signal (termination request).
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
void sighandler(int signo){
        printf("Signal handler :%d\n",signo);
        exit(0);
}
int main(){
        signal(SIGTERM,sighandler);
        printf("Program started (PID = %d)\n", getpid());
        printf("Send SIGTERM using: kill -15 %d\n", getpid());
        while (1) {
              printf("Running... waiting for SIGTERM\n");
              sleep(5);
         }
}
```

## 11.Write a program to handle the SIGTSTP signal (terminal stop).
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<signal.h>
void sighandler(int signo){
        printf("Signal handler:%d\n",signo);
        exit(0);
}
int main(){
        signal(SIGTSTP,sighandler);
        printf("program started(PID=%d)\n",getpid());
        printf("press ctrl+z to send SIGTSTP\n");
        while(1){
                printf("Running.... press ctrl+z to try stopping\n");
                sleep(5);
        }
}
```

## 12.Write a program to handle the SIGVTALRM signal (virtual timer expired).
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<sys/time.h>
#include<unistd.h>
void sighandler(int signo){
        printf("\nSIGVTALRM received! virtual timer expired.\n");
        printf("Signal:%d",signo);
        exit(0);
}
int main(){
        signal(SIGVTALRM,sighandler);
        struct itimerval timer;
        timer.it_value.tv_sec=3;
        timer.it_value.tv_usec=0;
        timer.it_interval.tv_sec=0;
        timer.it_interval.tv_usec=0;
        setitimer(ITIMER_VIRTUAL,&timer,NULL);
        printf("program started(PID=%d)\n",getpid());
        printf("Virtual timer set for 3 seconds of CPU time.\n");
        unsigned long count=0;
        while(1){
                count++;
        }
}
```

## 13.Write a program to handle the SIGWINCH signal (window size change).
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
#include<sys/ioctl.h>
void sighandler(int signo){
        struct winsize w;
        ioctl(STDOUT_FILENO,TIOCGWINSZ,&w);
        printf("\nSIGWINCH received! New window size: %d rows,%d cols\n",w.ws_row,w.ws_col);
}
int main(){
        struct sigaction act;
        act.sa_handler=sighandler;
        sigemptyset(&act.sa_mask);
        act.sa_flags=0;
        sigaction(SIGWINCH,&act,NULL);
        printf("Program started (PID=%d)\n", getpid());
        printf("Resize your terminal window to trigger SIGWINCH.\n");
        while(1){
                pause();
        }
}
```

## 14.Implement a C program to handle the SIGXFSZ signal (file size limit exceeded).
```c
#include<stdio.h>
#include<stdlib.h>
#include<signal.h>
#include<unistd.h>
#include<string.h>
#include<fcntl.h>
#include<sys/resource.h>
void sighandler(int signo){
        printf("\nSIGXFSZ caught: file size limit exceeded(signal %d)\n",signo);
        exit(1);
}
int main(){
        int fd;
        char buf[512];
        struct rlimit rl;
        signal(SIGXFSZ,sighandler);
        rl.rlim_cur = 2048;
        rl.rlim_max = 2048;
        if(setrlimit(RLIMIT_FSIZE,&rl)==-1){
                perror("setrlimit");
                return 1;
        }
        printf("Program started(pid=%d).File size limit=%llu bytes\n",getpid(),(unsigned long long)rl.rlim_cur);
        fd=open("file.txt",O_CREAT|O_WRONLY|O_TRUNC,0644);
        if(fd==-1){
                perror("Open");
                return 1;
        }
        memset(buf,'A',sizeof(buf));
        printf("Writing to file untill SIGXFSZ is triggered...\n");
        while(1){
                ssize_t w=write(fd,buf,sizeof(buf));
                if(w==-1){
                        perror("Write");
                        close(fd);
                        return 1;
                }
        }
}
```

## 15.Create a program to handle the SIGPWR signal (power failure restart).
```c
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
void sighandler(int signo){
        printf("Received SIGPWR(signal %d)-power failure detected!\n",signo);
}
int main(){
        signal(SIGPWR,sighandler);
        printf("Program running. PID :%d\n",getpid());
        printf("Send SIGPWR signal to test (kill -SIGPWR %d)\n",getpid());
        while(1){
                sleep(1);
        }
}
```

## 16.Write a program to handle the SIGSYS signal (bad system call).
```c
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
#include<sys/syscall.h>
void sighandler(int signo){
        printf("Received SIGSYS! (signal %d) - bad system call detected!\n",signo);
}
int main(){
        signal(SIGSYS,sighandler);
        printf("Program running. PID :%d\n",getpid());
        printf("Generating a bad system call...\n");
        syscall(9999);
        while(1){
                sleep(1);
        }
}
```

## 17.Write a C program to handle the SIGIO signal (I/O is possible on a descriptor).
```c
