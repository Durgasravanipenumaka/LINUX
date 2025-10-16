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
#include<stdlib.h>
#include<signal.h>
#include<fcntl.h>
#include<unistd.h>
void sighandler(int sig){
        printf("SIGIO signal received - I/O is possible!\n");
}
int main(){
        struct sigaction act;
        act.sa_handler=sighandler;
        sigemptyset(&act.sa_mask);
        act.sa_flags=0;
        if(sigaction(SIGIO,&act,NULL)==-1){
                perror("sigaction");
                exit(1);
        }       
        printf("Program running..\n");
        raise(SIGIO);
        printf("Program continues after handling SIGIO.\n");
}   
```

## 18.Implement a C program to handle the SIGINFO signal (status request from keyboard).
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

#if defined(SIGINFO)
#define STATUS_SIGNAL SIGINFO
#else
#define STATUS_SIGNAL SIGUSR1   // fallback if SIGINFO is not available
#endif

// Signal handler function
void status_handler(int signum) {
    printf("\n Status requested! Program is alive.\n");
}

int main() {
    struct sigaction sa;

    sa.sa_flags = 0;
    sigemptyset(&sa.sa_mask);
    sa.sa_handler = status_handler;

    if (sigaction(STATUS_SIGNAL, &sa, NULL) == -1) {
        perror("sigaction");
        return 1;
    }

    printf("Program running with PID: %d\n", getpid());
    printf("Send SIGINFO (or SIGUSR1 on Linux) to request status.\n");

    while (1) {
        pause(); // wait for any signal
    }

    return 0;
}
```

## 19.Create a C program to handle the SIGRTMIN signal (minimum real-time signal).
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

// Signal handler function
void rtsignal_handler(int signum) {
    printf("SIGRTMIN signal received!\n");
}

int main() {
    struct sigaction sa;

    sa.sa_flags = 0;
    sigemptyset(&sa.sa_mask);
    sa.sa_handler = rtsignal_handler;

    if (sigaction(SIGRTMIN, &sa, NULL) == -1) {
        perror("sigaction");
        return 1;
    }

    printf("Program running with PID: %d\n", getpid());
    printf("Raising SIGRTMIN signal using raise(SIGRTMIN)...\n\n");

    raise(SIGRTMIN);

    printf("\nProgram continues after handling SIGRTMIN.\n");

    return 0;
}
```

## 20.Implement a program to handle the SIGRTMAX signal (maximum real-time signal).
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void sigrtmax_handler(int signum) {
    printf(" SIGRTMAX signal received!\n");
}

int main() {
    struct sigaction sa;

    sa.sa_flags = 0;
    sigemptyset(&sa.sa_mask);
    sa.sa_handler = sigrtmax_handler;

    if (sigaction(SIGRTMAX, &sa, NULL) == -1) {
        perror("sigaction");
        return 1;
    }

    printf("Program running with PID: %d\n", getpid());
    printf("Waiting for SIGRTMAX signal...\n");

    while (1) {
        pause(); // suspend until a signal is received
    }

    return 0;
}
```

## 21.Write a program to handle the SIGABRT_ABORT signal (abort signal).
```c
#include <stdio.h>
#include <signal.h>
#include <stdlib.h>
#include <unistd.h>

// Signal handler function
void abort_handler(int signum) {
    printf(" SIGABRT signal received! Program called abort().\n");
    // Optional: exit safely
    _exit(1); // or exit(1)
}

int main() {
    struct sigaction sa;

    // Step 1: Clear structure and assign handler
    sa.sa_flags = 0;
    sigemptyset(&sa.sa_mask);
    sa.sa_handler = abort_handler;

    // Step 2: Register handler for SIGABRT
    if (sigaction(SIGABRT, &sa, NULL) == -1) {
        perror("sigaction");
        return 1;
    }

    printf("Program running with PID: %d\n", getpid());
    printf("Now calling abort() to trigger SIGABRT...\n\n");

    // Step 3: Trigger SIGABRT
    abort(); // this sends SIGABRT to this process

    // This line will not execute if handler exits the program
    printf("This line will not be printed if handler exits.\n");

    return 0;
}
```

## 22.. Create a C program to handle the SIGSEGV_SIGBUS signal (segmentation fault or bus error).
```c
#include <stdio.h>
#include <signal.h>
#include <stdlib.h>
#include <unistd.h>

// Signal handler function
void segfault_handler(int signum) {
    printf("⚠️ SIGSEGV or SIGBUS signal received! Invalid memory access.\n");
    _exit(1); // exit safely
}

int main() {
    struct sigaction sa;

    // Step 1: Clear structure and assign handler
    sa.sa_flags = 0;
    sigemptyset(&sa.sa_mask);
    sa.sa_handler = segfault_handler;

    // Step 2: Register handler for SIGSEGV and SIGBUS
    if (sigaction(SIGSEGV, &sa, NULL) == -1) {
        perror("sigaction SIGSEGV");
        return 1;
    }
    if (sigaction(SIGBUS, &sa, NULL) == -1) {
        perror("sigaction SIGBUS");
        return 1;
    }

    printf("Program running with PID: %d\n", getpid());
    printf("Now causing a segmentation fault...\n\n");

    // Step 3: Cause a segmentation fault
    int *ptr = NULL;
    *ptr = 42;  // invalid memory access triggers SIGSEGV

    // This line will not execute if handler exits
    printf("This line will not print\n");

    return 0;
}
```

## 23.Implement a program to handle the SIGUSR1_SIGUSR2 signal (user-defined signal).
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

// Handler for SIGUSR1
void usr1_handler(int signum) {
    printf("SIGUSR1 received!\n");
}

// Handler for SIGUSR2
void usr2_handler(int signum) {
    printf("SIGUSR2 received!\n");
}

int main() {
    struct sigaction sa1, sa2;

    // Setup handler for SIGUSR1
    sa1.sa_flags = 0;
    sigemptyset(&sa1.sa_mask);
    sa1.sa_handler = usr1_handler;
    if (sigaction(SIGUSR1, &sa1, NULL) == -1) {
        perror("sigaction SIGUSR1");
        return 1;
    }

    // Setup handler for SIGUSR2
    sa2.sa_flags = 0;
    sigemptyset(&sa2.sa_mask);
    sa2.sa_handler = usr2_handler;
    if (sigaction(SIGUSR2, &sa2, NULL) == -1) {
        perror("sigaction SIGUSR2");
        return 1;
    }

    printf("Program running with PID: %d\n", getpid());
    printf("Send SIGUSR1 or SIGUSR2 signals using kill command.\n");

    // Wait indefinitely for signals
    while (1) {
        pause(); // suspend until a signal is received
    }

    return 0;
}
```

## 24.Create a C program to handle the SIGCONT_SIGSTOP signal (continue or stop executing).
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

// Handler for SIGCONT
void cont_handler(int signum) {
    printf("SIGCONT received! Program resumed after stop.\n");
}

int main() {
    struct sigaction sa;

    // Setup handler for SIGCONT
    sa.sa_flags = 0;
    sigemptyset(&sa.sa_mask);
    sa.sa_handler = cont_handler;

    if (sigaction(SIGCONT, &sa, NULL) == -1) {
        perror("sigaction SIGCONT");
        return 1;
    }

    printf("Program running with PID: %d\n", getpid());
    printf("You can stop the program using Ctrl+Z or kill -STOP <PID>\n");
    printf("Then resume using 'fg' or kill -CONT <PID>\n");

    // Loop to keep program alive
    while (1) {
        printf("Program is running...\n");
        sleep(2);
    }

    return 0;
}
```

## 25.Write a program to demonstrate how to block signals using sigprocmask().
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void sigint_handler(int signum) {
    printf("\nSIGINT received!\n");
}

int main() {
    sigset_t newmask, oldmask;

    // Register SIGINT handler
    signal(SIGINT, sigint_handler);

    // Initialize signal set and add SIGINT
    sigemptyset(&newmask);
    sigaddset(&newmask, SIGINT);

    printf("Blocking SIGINT (Ctrl+C) for 5 seconds...\n");
    
    // Block SIGINT
    if (sigprocmask(SIG_BLOCK, &newmask, &oldmask) < 0) {
        perror("sigprocmask");
        return 1;
    }

    for (int i = 5; i > 0; i--) {
        printf("%d...\n", i);
        sleep(1);
    }

    printf("Unblocking SIGINT now. Press Ctrl+C to trigger handler.\n");

    // Unblock SIGINT
    if (sigprocmask(SIG_SETMASK, &oldmask, NULL) < 0) {
        perror("sigprocmask");
        return 1;
    }

    // Wait to demonstrate signal handler
    printf("Waiting 10 seconds for SIGINT...\n");
    sleep(10);

    printf("Program finished.\n");
    return 0;
}
```

## 26.Write a program to implement a timer using signals.
- A timer can generate a signal after a certain amount of time or at regular intervals.
- The SIGALRM signal is commonly used for timers.
- signal() or sigaction() can handle the signal when it occurs.
- alarm(seconds) function can be used to trigger SIGALRM after a given number of seconds.
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

// Signal handler function
void timer_handler(int signum) {
    printf("Timer expired! SIGALRM received.\n");
}

int main() {
    // Step 1: Register signal handler for SIGALRM
    signal(SIGALRM, timer_handler);

    printf("Timer started for 5 seconds...\n");

    // Step 2: Set timer for 5 seconds
    alarm(5);  // after 5 seconds, SIGALRM is sent

    // Step 3: Do other work while waiting
    for (int i = 1; i <= 10; i++) {
        printf("Program working... %d\n", i);
        sleep(1);
    }

    printf("Program finished.\n");
    return 0;
}
```

## 27.Write a program to handle a real-time signal using sigqueue().
- Real-time signals are signals reserved for user-defined purposes, ranging from SIGRTMIN to SIGRTMAX.
- sigqueue() is a special system call that allows you to send a signal to a process and pass an integer or pointer value along with it.
- This is more advanced than kill(), which only sends the signal.
- You need:
- Signal handler for the real-time signal.
- sigqueue() to send the signal with data (sigval).
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <stdlib.h>

// Handler for real-time signal
void rt_handler(int sig, siginfo_t *info, void *context) {
    printf("✅ Real-time signal received: %d\n", sig);
    printf("Received value: %d\n", info->si_value.sival_int);
}

int main() {
    struct sigaction sa;
    union sigval value;

    // Step 1: Setup signal handler for SIGRTMIN
    sa.sa_flags = SA_SIGINFO; // allow handler to receive extra info
    sa.sa_sigaction = rt_handler;
    sigemptyset(&sa.sa_mask);

    if (sigaction(SIGRTMIN, &sa, NULL) == -1) {
        perror("sigaction");
        return 1;
    }

    printf("Program running with PID: %d\n", getpid());

    // Step 2: Prepare value to send
    value.sival_int = 42;

    // Step 3: Send real-time signal to self
    if (sigqueue(getpid(), SIGRTMIN, value) == -1) {
        perror("sigqueue");
        return 1;
    }

    // Step 4: Wait for signal to be handled
    sleep(2);

    printf("Program finished.\n");
    return 0;
}
```

## 28.Why we use raise system call explain it programmatically.
- raise() is a C standard library function used to send a signal to the calling process itself.
- Essentially, it allows a program to trigger its own signal handler.
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

// Signal handler function
void my_handler(int signum) {
    printf("⚡ Signal %d received! Handler executed.\n", signum);
}

int main() {
    // Step 1: Register handler for SIGUSR1
    signal(SIGUSR1, my_handler);

    printf("Program running with PID: %d\n", getpid());
    printf("Raising SIGUSR1 using raise()...\n");

    // Step 2: Trigger signal using raise()
    raise(SIGUSR1);

    printf("Program continues after handling signal.\n");

    return 0;
}
```
