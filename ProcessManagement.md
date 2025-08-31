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






