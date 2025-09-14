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
#### 1. Bootstrapping: The First Process
- When the system boots, the kernel is loaded into memory.
- The kernel creates the first process, traditionally called init (or systemd in modern Linux).
- PID (Process ID) = 1.
- init becomes the ancestor (root) of all other processes in the system.
#### 2.Parent–Child Relationship:
- New process is created by using fork() system call.
- fork() makes an almost identical copy of the parent process.
- The new process is the child, while the original remains the parent.
- After fork(), either process can call exec() to replace its memory image with a new program.
- Every process has a Parent Process ID (PPID) stored in its process table entry.
- fork() returns two times once it returns child's pid in parent process and it returns 0 in child process.
#### 3.Orphan and Zombie process:
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

## 13.Explain how the execve() system call works and provide a code example.
- execve() is part of the exec family of system calls.
- The exec family replaces the current process image (code, data, heap, stack) with a new program image.
- Unlike fork(), which creates a new process, execve() does not create a new PID — it reuses the same process ID but loads a completely new program.
- syntax : int execve(const char *pathname, char *const argv[], char *const envp[]);
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
int main(){
        char *args[]={"/bin/ls","-l",NULL};
        char *envp[]={NULL};
        printf("Before execve");
        if(execve("/bin/ls",args,envp)==-1){
                perror("execve failed");
                exit(1);
        }
        printf("This statement will never executed");
}
```

## 14.Discuss the role of the fork() system call in implementing multitasking.
- fork() is used to create a new process.
- fork() makes an almost identical copy of the parent process which is called child process and orginal process is called parent process.
- Execution of child process starts from where fork() system call invoked.
- fork() returns two times once it returns child's pid in parent process and it returns 0 in child process.
- on failure it returns -1.
#### Role of fork() in multitasking :
- Multitasking requires multiple processes to run concurrently.
- fork() provides the mechanism to spawn these processes.
- After fork(), both parent and child can execute different parts of the program.
- For example, the parent might handle user input, while the child does background computation.
- With CPU scheduling, the operating system switches rapidly between parent and child (and other processes).
- On multi-core systems, they may even run truly in parallel.
- Often after a fork(), the child process replaces its memory image with a new program using exec() system calls.
- The child gets its own copy of the parent’s memory space (using copy-on-write for efficiency).
- This ensures processes don’t overwrite each other’s data, which is crucial for multitasking.
- fork() is the backbone of multitasking in Unix-like systems. It creates new processes, allowing multiple tasks to run concurrently, either by sharing CPU time (time-sharing) or truly in parallel on multiple cores.

## 15.Write a C program to create multiple child processes using fork() and display their PIDs.
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
int main(){
        int id,n;
        printf("Number of child process:");
        scanf("%d",&n);
        for(int i=0;i<n;i++){
                id=fork();
                if(id<0){
                        printf("fork() failed");
                        exit(1);
                }
                else if(id==0){
                        printf("child:%d\tPID:%d\tPPID:%d\n",i+1,getpid(),getppid());
                        exit(0);
                }
                else
                        continue;
        }
        for(int i=0;i<n;i++){
                wait(NULL);
        }
        printf("Parent PID: %d has created %d child process\n",getpid(),n);
}
```

## 16.How does the exec() system call replace the current process image with a new one?
- When a process calls one of the exec() functions (execl, execp, execv, execve, etc.), the current process image is replaced with a new program image.
- The process does not create a new PID (unlike fork()); instead, the same process is overwritten with a new program.
- Process calls exec() : example: execve("/bin/ls", args, envp);
- The kernel reads the executable file (ELF format in Linux) from disk.
- The old code, data, heap, and stack segments of the calling process are discarded and replaced with the new process image/new process memory segments.
- The CPU instruction pointer is reset to the new program’s entry point (main() in C programs).
- The process resumes execution as if it had just started the new program.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/wait.h>
int main(){
        int pid;
        pid=fork();
        if(pid<0){
                printf("fork() error");
                exit(1);
        }
        else if(pid==0){
                char *args[]={"ls","-l",NULL};
                execv("/bin/ls",args);
                printf("Exec fails");
                exit(1);
        }
        else{
                wait(NULL);
                printf("child finished execution");
        }
}
```

## 17.Explain the concept of process scheduling in operating systems.
- Scheduller will have access to the entire list.
- Scheduller is component of kernel space which helps to schedule the program.
- Scheduller decides which program runs next,which program executes next depends on scheduller mechanisms.
- Primarily we have two mechanisms:
#### 1.Round-Robbin scheduller.
- Each process will get equal amount of CPU time for execution in cyclic order.
- Time given process for small duration is called CPU time.
- After expiring of CPU time at last node scheduller comes back to the first process.
- Use case: Best suited for time-sharing and interactive systems, as it ensures fairness and responsiveness.
#### 2.Pre-emptive Priority based scheduller:
- Which will have high priority executes first.
- Priority value depends upon the nice value.
- Every process in pre emptive have nice value.Depends on nice value the program gets priority. Having high nice value then it is placed in 1st.This is called Preemption.
- Use case: Useful when some tasks are more critical than others.

## 18.Describe the role of the clone() system call in process management.
- clone() is a system call in Linux used to create a new process.
- Unlike fork(), it gives you control over what the new process shares with the parent.
#### Role in Process Management
##### 1.Creates new processes or threads:
- If we want a separate process → use clone() like fork().
- If we want a thread (same memory, different execution) → clone() makes that possible.
##### 2.Shares resources selectively:
- You can decide if the new process shares:
- Memory (CLONE_VM) → both see the same variables.
- File descriptors (CLONE_FILES) → both can read/write the same files.
- Signal handlers (CLONE_SIGHAND).
- File system info (CLONE_FS).
- This flexibility makes it different from fork() (which always copies resources).
##### 3.Basis for Threads in Linux
- The pthread library (used in C/C++) internally uses clone() to create threads.
- That’s why threads in Linux can share memory, files, etc.

## 19.Write a program in C to create a zombie process and explain how to avoid it.
#### Zombie process:
- A zombie process is a process that has finished execution but still has an entry in the process table.
- This happens when the parent does not call wait() to collect the child’s exit status.
- As a result, the child remains in the “zombie” state until the parent terminates or calls wait().
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        int id;
        id=fork();
        if(id<0){
                printf("fork error");
                exit(1);
        }
        else if(id==0){
                printf("child PID:%d and PPID:%d\n",getpid(),getppid());
                exit(0);
        }
        else{
                printf("parent process id:%d\n",getpid());
                sleep(20);
                printf("parent process exiting...");
        }
}
```
### For Avoiding of Zombie process:
- 1.Parent calls wait() or waitpid().For example : wait(NULL);
- This collects the child’s exit status and removes it from the process table.
- 2.Use signal(SIGCHLD, SIG_IGN);
- ells the kernel to automatically clean up child processes when they exit.
- 3.Double fork technique
- The child forks again, and its child is adopted by init (or systemd), which reaps it.

## 20.Discuss the significance of the setuid() and setgid() system calls in process management.
- Every process in Linux runs with two main identities:
- 1.UID (User ID): Tells which user owns the process.
- 2.GID (Group ID): Tells which group the process belongs to.
#### 1.setuid(uid) :
- Changes the real and/or effective UID of a process.
- Syntax : int setuid(uid_t uid);
- After calling this, the process runs with the new user privileges.
- If the calling process is root, it can change to any UID.
- If it’s a normal user, it can only change to its own UID (to drop privileges).
#### 2.setgid(gid)
- Changes the real GID, effective GID, and sometimes the saved GID of a process.
- Syntax: int setgid(gid_t gid);
- After calling this, the process runs with the new group’s privileges.
- Root can switch to any group; normal users can only switch to their own group ID.

## 21.Explain the concept of process groups and their significance in UNIX-like operating systems.
A process group is a collection of processes treated as a single unit. They are essential for job control, signal handling, and foreground/background execution in UNIX-like systems, allowing the shell to manage multiple processes in a structured and efficient way.

## 22.Write a C program to demonstrate the use of the waitpid() function for process synchronization.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/wait.h>
int main(){
        int wpid,pid,status;
        pid=fork();
        if(pid<0){
                printf("fork failed");
                exit(1);
        }
        else if(pid==0){
                printf("Child process:PID = %d\n",getpid());
                sleep(3);
                printf("Child process finished\n");
                exit(42);
        }
        else{
                printf("parent process:waiting for child %d to finish..\n",pid);
                wpid=waitpid(pid,&status,0);
                if(wpid==-1){
                        printf("Error");
                        exit(1);
                }
        }
        if(WIFEXITED(status))
                printf("parent: child %d exited with exited status %d\n",wpid,WEXITSTATUS(status));
        else
                printf("parent : child %d didnot exit normally\n",wpid);
}
```
## 23.Discuss the role of the execle() function in the exec() family of calls.
```c
#include<stdio.h>
#include<unistd.h>
int main(){
        char *envp[]={"MYVAR=HelloWorld",NULL};
        printf("Before execle\n");
        execle("/usr/bin/env","env",NULL,envp);
        perror("execle failed");
}
```

## 24.Describe the purpose of the nice() system call in process scheduling.
- nice() is a system call in Linux used to influence process scheduling priority.
- It doesn’t set the exact priority, but it adjusts the "niceness" value of a process.
- Niceness = how "kind" a process is to other processes.
- Range: -20 to +19:
- -20 → highest priority (less nice, grabs more CPU).
- +19 → lowest priority (more nice, lets others use CPU).
- Default = 0

## 25.Write a program in C to create a daemon process.
#### Daemon process:
- A daemon process is a special kind of process in UNIX/Linux that:
- Runs in the background (not attached to any terminal).
- Provides services or does work continuously.
- Starts usually at boot time and keeps running until shutdown.
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<fcntl.h>
int main(){
        int pid;
        pid=fork();
        if(pid<0){
                printf("Error");
                exit(1);
        }
        if(pid>0){
                printf("Daemon process is created with pid: %d",pid);
                exit(0);
        }
        if(setsid()<0){
                printf("setsid failed");
                exit(1);
        }
        close(STDIN_FILENO);
        close(STDOUT_FILENO);
        close(STDERR_FILENO);
        while(1){
                int fd;
                fd=open("daemon.txt",O_WRONLY|O_CREAT|O_APPEND,0644);
                if(fd>0){
                        write(fd,"This is daemon process\n",30);
                        close(fd);
                }
        }
        sleep(5);
}
```                                                                                                                                                                               




