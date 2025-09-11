## 1.Explain the concept of process creation in operating systems.
- In an Operating System (OS), a process is simply a program in execution. Process creation happens when a new process is made by another process (called the parent process). The new process created is the child process.
- The very first process is init.the PID of init process is 1.
- Parent process strats from main() and child process starts from where the fork() gets invokes.
- fork() invokes two times once in parent process and once in child process.
- fork() returns 1 in parent process and 0 in child process.

## 2.Differentiate between the fork() and exec() system calls.
### fork() :
- fork() is a system call used to create a new process.
- It makes a copy of parent process, new process is called child process.
- fork() returns different values. 0 to the child process and 1 to the parent process.
### exec():
- exec() is a family of system calls used to replace the current process image with a new process.
- when a process calls exec() the memory segments of current process replaced with the memory segments of new process.

## 3.Write a C program to demonstrate the use of fork() system call.
```c
#include<stdio.h>
#include<unistd.h>
int main(){
        int id;
        id=fork();
        if(id<0){
                printf("Error");
                return 1;
        }
        else if(id==0)
                printf("child process\n");
        else
                printf("parent process\n");
}
```
## 4.What is the purpose of the wait() system call in process management?
- wait() is a blocking system call used by a parent process to get the exit code of the child process.
- parent process come out of the blocking state only after the child process terminates.

## 5.Describe the role of the exec() family of functions in process management.
The role of the exex() family of functions is to replace the memory segments of a.out or executable file with new program memory segments.

## 6.Write a C program to illustrate the use of the execvp() function.
```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/wait.h>
int main(){
         pid_t pid;
        char *args[4]={"ls","-l",NULL};
        pid=fork();
        if(pid<0){
                perror("Error");
                return 1;
        }
        else if(pid==0){
                printf("Child process:executing ls-l command\n");
                execvp("ls",args);
                perror("execvp error");
        }
        else{
                wait(NULL);
                printf("Parent Process:child process finished exexcution");
        }
}
```
## 7.How does the vfork() system call differ from fork()?
- If you use fork() write on technique is applied, but if you use vfork() write on copy technique is not applied.

## 8.Discuss the significance of the getpid() and getppid() system calls.
### getpid():
- It returns the process id of calling process.
- Syntax: pid_t getpid(void);
### getppid():
- It returns the Parent Process id of calling process.
- Syntax: pid_t getppid(void);
  
```c
#include<stdio.h>
#include<unistd.h>
int main(){
        printf("Process id:%d\n",getpid());
        printf("parent process id:%d\n",getppid());
}
```
## 9.Explain the concept of process termination in UNIX-like operating systems.
- Process termination in UNIX-like systems occurs when a process ends normally, due to an error, or by a signal; it returns an exit status to the parent, which must collect it using wait() to   prevent zombie processes.

## 10.Write a program in C to create a child process using fork() and print its PID.
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
int main(){
        int id;
        id=fork();
        if(id<0){
                printf("Fork failed");
                exit(1);
        }
        else if(id==0){
                printf("Child Process\n");
                printf("Child process PID :%d\n",getpid());
        }
        else{
                printf("parent process\n");
                printf("parent process PID :%d\n",getpid());
                printf("child process PID :%d\n",id);
        }

}
```
## 11.Describe the process hierarchy in UNIX-like operating systems.
- In UNIX-like operating systems, processes are organized in a hierarchical tree structure. Each process has a parent (except the very first one) and may have child processes. Let’s go step by step:
### 1. Bootstrapping: The First Process
- When the system boots, the kernel is loaded into memory.
- The kernel creates the first process, traditionally called init (or systemd in modern Linux).
- PID (Process ID) = 1.
- init becomes the ancestor (root) of all other processes in the system.
### 2.Parent–Child Relationship:
- New process is created by using fork() system call.
- fork() makes an almost identical copy of the parent process.
- The new process is the child, while the original remains the parent.
- After fork(), either process can call exec() to replace its memory image with a new program.
- Every process has a Parent Process ID (PPID) stored in its process table entry.
- fork() returns two times once it returns child's pid in parent process and it returns 0 in child process.
### 3.Orphan and Zombie process:
- Orphan Process:
- If a parent process terminates even before the child process is called as Orphan process.
- parent process id of Orphan process is 1(PID of init process).
- Zombie Process:
- If a child terminates before giving the exit status of child process then parent process can become zombie process.
- When a child terminates, it still has an entry in the process table until the parent collects its exit status via wait(). Such a process is in the zombie state.

## 12.What is the purpose of the exit() function in C programming?
- The exit() function is used to terminate a program immediately.
- It is declared in the header file: #include <stdlib.h>
- The integer argument passed to exit() indicates how the program ended.
- exit(0) → Normal termination (success).
- exit(1) or any non-zero value → Abnormal termination (error).




