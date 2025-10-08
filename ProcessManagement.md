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

## 26.Discuss the difference between the fork() and clone() system calls.
### fork() System Call :
- fork() is used to create a new process in UNIX/Linux.
- The new process is called the child, and it is almost an exact copy of the parent.
### clone() System Call :
- clone() is a more flexible and powerful version of fork() in Linux.
- It allows the child process to share parts of its execution context with the parent.
- Often used to create threads in Linux because threads share memory.

## 27.Write a C program to demonstrate the use of the system() function for executing shell commands.
```c
#include<stdio.h>
#include<stdlib.h>
int main(){
        printf("Listing files in current directory:\n");
        system("ls -l");
        printf("\nPrinting current working directory:\n");
        system("pwd");
        printf("\nDisplaying current date and time:\n");
        system("date");
}
```

## 28.Explain the concept of process states in UNIX-like operating systems.
### Common Process States in UNIX/Linux :
#### 1.New (Created)
- The process is being created.
- Example: after calling fork(), the OS creates a new process.
- The process is not ready to run yet.
#### 2.Ready (Runnable)
- The process is ready to run but is waiting for CPU time.
- Example: many processes can be ready, OS scheduler decides who runs next.
#### 3.Running
- The process is currently executing on the CPU.
- Only one process per CPU core can be in this state at a time.
#### 4.Waiting / Blocked
- The process is waiting for some event to occur, like:
- Input/output completion (keyboard, disk, network)
- A signal from another process
- It cannot run until the event happens.
#### 5.Stopped (Suspended)
- The process has been stopped, usually by receiving a signal.
- Example: pressing Ctrl+Z in the terminal stops a process.
#### 6.Terminated / Zombie
- The process has finished execution, but its entry remains in the process table so the parent can read its exit status.
- Called a zombie process until the parent calls wait().

## 29.Describe the purpose of the chroot() system call and provide an example.
### chroot() :
- chroot() stands for “change root”.
- It changes the root directory (/) for the current process and its children to a new directory.
- After calling chroot(), the process cannot access files outside the new root directory.
- Often used for sandboxing or isolating processes for security.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        if(chroot("/home/penumaka-durga-sravani/Documents/Linux/processmanagement")!=0){
                perror("chroot failed");
                exit(1);
        }
        printf("New root directory is /home/sandbox\n");
        system("ls /");
}
```

## 30.. Discuss the role of the execv() function in the exec() family of calls.
#### execv() :
- In UNIX/Linux, the exec() family replaces the current running process image with a new program.
- After a successful exec(), the old program is completely gone — only the new one runs.
- These calls don’t create a new process (unlike fork()); instead, they overwrite the current one.
- Syntax : int execv(const char *path, char *const argv[]);

## 31.Write a C program to create a process using fork() and pass arguments to the child process.

```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/wait.h>
int main(){
        int pid;
        pid=fork();
        char *args[]={"ls","-l","/",NULL};
        if(pid<0){
                printf("Fork system call failed\n");
                exit(1);
        }
        else if(pid==0){
                printf("Child pid is executing command...");
                if(execvp("ls",args)==-1){
                        printf("failed");
                        exit(1);
                }
        }
        else{
                printf("parent child is waiting for child to finish\n");
                wait(NULL);
                printf("Child finished execution\n");
        }
}
```

## 32.Explain the significance of process identifiers (PIDs) in process management.
### PID :
- PID stands for Process Identifier.
- It is a unique number assigned by the operating system to every running process.
- It helps the OS and other programs identify and manage processes.
### Significance of PID :
 1. Identify processes: Each process has a unique PID so the system can tell them apart.
 2. Parent-child tracking: PIDs help track which process created which (using parent PID).
 3. Control processes: The OS and programs can use PIDs to send signals (like stop or kill a process).
 4. Monitor resources: PIDs help the OS keep track of CPU, memory, and other resources used by each process.
 5. Debugging and monitoring: Tools like ps or top use PIDs to show process information.

## 33.Discuss the concept of orphan processes and how they are handled in UNIX-like operating systems.
#### Orphan Processes :
- An orphan process is a child process whose parent has terminated (exited) before the child finishes execution.
- Normally, a parent is responsible for monitoring and cleaning up its child processes.
- When the parent dies first, the child becomes “orphaned.”
#### How UNIX/Linux Handles Orphans :
- In UNIX-like systems, orphaned processes are automatically adopted by the init process (PID 1) or systemd.
- init becomes the new parent of the orphaned process.

## 34.Write a program in C to demonstrate process synchronization using semaphores.
```c
```

## 35.Describe the concept of process priority and how it is managed in operating systems.
- In an operating system (OS), multiple processes may compete for the CPU.
- To decide which process gets CPU time first, the OS assigns a priority to each process.
- Priority = importance level of a process (higher priority → more urgent).
- In OS sheduler decides which process can run next depending on their priority levels.

## 36.Explain the purpose of the fork() system call in creating copy-on-write (COW) processes.
- fork() is a system call used to create a new process (called the child process) from the parent process.
- After a successful fork(), two processes exist:
- Parent: continues execution from the point of fork.
- Child: a copy of the parent’s memory, file descriptors, and resources.
#### Copy-on-Write (COW) with fork()
- To optimize, modern OSes (Linux, BSD, etc.) use Copy-on-Write (COW):
- Instead of making an immediate full copy, parent and child share the same memory pages after fork.
- Both processes’ page tables point to the same physical memory.
- for example page2 on which write operation is performed is duplicated.
- Duplicated page2 is copied into freely available physical frames.
- page2's corresponding entry in page table of child process is modified.

## 37.Discuss the role of the execvp() function in searching for executable files.
- The execvp() function in Unix/Linux is part of the exec family of system calls.
- It is used to replace the current process image with a new program.
- Syntax : int execvp(const char *file, char *const argv[]);
- The p in execvp means search using PATH:If file contains a slash / (absolute or relative path), execvp() tries to execute it directly.
- Example: execvp("/bin/ls", argv);
- If file does not contain a slash, execvp() searches for the executable in the directories listed in the environment variable PATH.
- Example: execvp("ls", argv);
- It will check /bin/ls, /usr/bin/ls, etc., until it finds the command.

## 38.Write a C program to demonstrate the use of the execvpe() function.
- execvpe() is a GNU extension (not POSIX standard).
- Similar to execvp(), but it lets you explicitly pass an environment (envp) to the new program.
- Prototype: int execvpe(const char *file, char *const argv[], char *const envp[]);
- file → name of executable (searched in $PATH if no / given).
- argv → argument list (NULL terminated).
- envp → custom environment (NULL terminated).
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        char *args[]={"env",NULL};
        char *envp[]={"USER=DurgaSravani","PATH=/bin:/usr/bin","MYVAR=HelloFromExecvpe",NULL};
        printf("Before execvpe:running in original process\n");
        if(execvpe("env",args,envp)==-1){
                perror("execvpe failed");
                exit(EXIT_FAILURE);
        }
        printf("This line never prints");
}
```

## 39.Explain the concept of process context switching and its impact on system performance.
### Context Switching:
- A process needs CPU, memory, and registers to run.
- When the CPU switches from executing one process to another, the OS must save the state of the current process and load the state of the next process.
- This operation is called a context switch.
#### Steps in context switching:
1. Save context of current process (registers, program counter, stack pointer).
2. Update PCB (Process Control Block) of the current process.
3. Select the next process to run (via scheduler).
4. Load context of next process from its PCB.
5. Resume execution of the next process.
### Impact on System Performance
- Overhead: Context switching doesn’t do useful work (no actual process progress). It’s pure overhead.
- Latency: Too many switches → higher CPU time spent saving/loading states.
- Cache Pollution: Switching processes flushes CPU caches (L1/L2), reducing efficiency.
- Throughput: Frequent switching lowers throughput because CPU spends more time in management than execution.
- Responsiveness: While overhead increases, context switching allows multiple processes to run seemingly “at once,” improving system responsiveness for users.

## 40.Discuss the role of the clone() system call in creating threads in Linux.
- clone() is a Linux-specific system call that creates a new process or thread.
- Unlike fork(), which always creates a separate process with its own resources, clone() allows the child to share parts of its execution context (memory, file descriptors, signal handlers, etc.) with the parent.
- This sharing makes it possible to implement lightweight processes (threads) in Linux.
- clone() is the underlying system call that Linux uses to implement threads.
- By selecting specific flags, it controls how much the child shares with the parent.
- Libraries like pthreads are built on top of clone().
- Without clone(), Linux wouldn’t have its efficient thread model.
- When we create a thread in Linux:
- The pthreads library internally calls clone() with flags that make the new thread share resources with the parent:
- CLONE_VM → share the same memory space.
- CLONE_FILES → share file descriptors.
- CLONE_SIGHAND → share signal handlers.
- CLONE_THREAD → indicate it’s part of the same thread group.

## 41.Write a C program to create a process group and change its process group ID (PGID).
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        int id=fork();
        int pgid;
        if(id<0){
                printf("Error");
                exit(1);
        }
        if(id==0){
                printf("Child PID:%d,Parent PID:%d\n",getpid(),getppid());
                printf("Before changibg child PGID:%d\n",getpgrp());
                if(setpgid(0,0)==-1){
                        perror("setpgid");
                        exit(1);
                }
                pgid=getpgrp();
                printf("After changing:Child PID = %d\n",pgid);
                sleep(2);
        }
        else{
                printf("Parent PID :%d,PGID :%d\n",getpid(),getpgrp());
                sleep(5);
        }
}
```

## 42.Explain the difference between process creation using fork() and pthread_create().
### 1. fork() – Process Creation :
- fork() is a system call in UNIX/Linux that creates a new process (child process) by duplicating the calling process (parent process).
- After a fork(), two processes exist: parent and child.
- Both execute the same code, but fork() returns:
- 0 to the child process,
- child’s PID to the parent process.
- The child gets a separate memory space (copy of parent’s memory).
- File descriptors are duplicated.
- No memory is shared directly between parent and child (unless explicitly done using IPC like shared memory, pipes, etc.).
- Processes are independent.
- If one crashes, the other survives (unless they share IPC).

### 2.pthread_create() – Thread Creation :
- Definition: pthread_create() creates a new thread within the same process.
- Multiple threads run concurrently within the same process.
- Each thread has:Its own stack,Its own thread ID.
- But they share the same code, data, and heap segments.
- Threads share global variables, heap, and open file descriptors.
- Communication between threads is easier (just access shared variables), but requires synchronization (mutex, semaphores).
- Threads are not independent; if one thread crashes (segfault), usually the entire process dies.

## 43.Write a program in C to demonstrate inter-process communication (IPC) using shared memory.
```c
#include<stdio.h>
#include<unistd.h>
#include<string.h>
#include<stdlib.h>
#include<sys/wait.h>
#include<sys/shm.h>
#include<sys/ipc.h>
#define size 1024
int main(){
        int shmid;
        int *sharedvar;
        shmid=shmget(1234,sizeof(int),0666|IPC_CREAT);
        sharedvar = (int *)shmat(shmid,NULL,0);
        *sharedvar=10;
        printf("Parent: Initial value = %d\n",*sharedvar);
        int pid=fork();
        if(pid==0){
                (*sharedvar)++;
                printf("Child: Incremented value =%d\n",*sharedvar);
                shmdt(sharedvar);
        }
        else{
                wait(NULL);
                printf("Parent: value after child increment=%d\n",*sharedvar);
                shmdt(sharedvar);
                shmctl(shmid,IPC_RMID,NULL);
        }
}
```

## 44.Describe the role of the fork() system call in implementing the shell's job control.
- Job control is a shell feature that allows users to manage multiple processes:
- Run programs in foreground or background.
- Suspend and resume processes with Ctrl+Z and fg/bg commands.
- Track jobs using jobs command.
- Role of fork() in Job Control
- When you type a command in the shell (e.g., ls -l), here’s what happens under the hood:
- 1. Shell parses the command:
- Figures out the program name and arguments.
- 2. Shell calls fork() :
- This creates a child process that is a copy of the shell.
- The child is used to run the command.
- The parent shell continues running to manage jobs.
- 3. Child process calls execvp()
- Replaces itself with the requested program (ls -l, sleep 30, etc.).
- If execvp() fails, child prints an error and exits.
- 4. Parent shell manages job control :
- If the job is foreground : shell waits for the child using waitpid().
- If the job is background : shell does not wait; instead, it prints the job ID and PID, then returns control to the user.
- The shell keeps track of running jobs in a table (job ID ↔ PID).
- 5. Signals and suspension :
- Foreground processes receive signals like Ctrl+C (SIGINT) and Ctrl+Z (SIGTSTP) directly from the terminal.
- The shell (parent) can stop or continue jobs (kill -STOP, kill -CONT) because it knows the child’s PID from fork().

## 45.Explain the purpose of the execlp() function and provide an example.
### Purpose of execlp() :
- Executes a new program by searching for it in the directories listed in the PATH environment variable.
- The p in execlp stands for "PATH search", so you don’t need to provide the full path of the program if it exists in the PATH.
- The l stands for "list", which means arguments are passed as a list of strings instead of an array.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        printf("Before execlp\n");
        if(execlp("ls","ls","-l",NULL)==-1){
                perror("execlp failed");
        }
        printf("This line will not be printed\n");
}
```

## 46.Discuss the significance of the execvp() function in searching for executables in the PATH environment variable.
- The execvp() function in C is part of the exec family of functions used to execute new programs. Its main significance lies in how it searches for executable files using the PATH environment variable, making it very flexible compared to functions like execv() that require the full path of the executable.
```c
#include<stdio.h>
#include<unistd.h>
int main(){
        char *args[]={"ls","-l",NULL};
        execvp("ls",args);
}
```

## 47.Discuss the significance of the setpgid() system call in managing process groups.
- The setpgid() system call in UNIX/Linux is used to set or change the process group ID (PGID) of a process, which plays a crucial role in process group management, job control, and signal handling in a multitasking environment.
- The significance of setpgid() is that it allows processes to be organized into groups, so signals can be sent to all processes in a group, enabling proper job control in a shell.

## 48.Write a C program to create a child process using vfork() and demonstrate its usage.
### vfork() :
- vfork() is used to create a child process like fork().
- The main difference from fork() is that the child shares the address space of the parent until it calls exec() or _exit().
- It is more efficient than fork() for creating a child process that immediately calls exec().
- Important: The child should not modify variables or return from the function before calling exec() or _exit(), because it shares memory with the parent.
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
int main(){
        int x=100;
        printf("Before vfork(),x=%d\n",x);
        int pid=vfork();
        if(pid==0){
                printf("child process(PID:%d)\n",getpid());
                printf("child sees x=%d\n",x);
                x+=50;
                printf("child modifies x to %d\n",x);
                exit(0);
        }
        else{
                printf("parent process(PID: %d)\n",getpid());
                printf("After child terminates,x=%d\n",x);
        }
}
```

## 49.Explain the concept of process priority inheritance and its importance in real-time systems.
- Priority inheritance is a mechanism to handle priority inversion in multitasking systems, especially real-time systems.
- Priority inversion occurs when a high-priority process is blocked because a lower-priority process holds a resource (like a mutex) it needs.
- During this time, if a medium-priority process (which doesn’t need the resource) runs, it can preempt the low-priority process, further delaying the high-priority one. This is called priority inversion.
- Priority inheritance solves this by temporarily raising the priority of the low-priority process holding the resource to the priority of the highest-priority waiting process.
#### Without priority inheritance:
- P1 waits for P2.
- P3 preempts P2.
- P1 suffers indefinite delay → priority inversion.
#### With priority inheritance:
- P2 temporarily inherits P1's high priority.
- P2 finishes quickly, releases resource R.
- P1 resumes, P2 goes back to low priority.
- No unnecessary delays for P1.

## 50.Describe the role of the fork() system call in copy-on-write (COW) mechanism and its benefits.
#### 1. Role of fork() in Copy-on-Write (COW)
- Normally, when a process calls fork(), the entire memory space (code, data, heap, stack) of the parent is duplicated for the child.
- But this full duplication is expensive in terms of time and memory, especially if the child immediately calls exec() to load a new program.
- To optimize this, modern operating systems (like Linux) use the Copy-on-Write (COW) mechanism with fork().
#### How it works with COW:
- Fork is called → The child process is created.
- Instead of copying all memory pages, parent and child share the same physical memory pages.
- These shared pages are marked as read-only.
- If either process writes to a shared page:
- The OS makes a private copy of that page for the process that wrote to it.
- This ensures memory consistency while avoiding unnecessary copies.
- If pages are only read, no copying happens → memory saving.

## 51.Write a C program to create a pipeline between two processes using the pipe() system call.
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<string.h>
int main(){
        int fd[2];
        int pid;
        char writemsg[]="Hello from parents!";
        char readmsg[100];
        pipe(fd);
        pid=fork();
        if(pid==0){
                close(fd[1]);
                read(fd[0],readmsg,sizeof(readmsg));
                printf("Child received:%s\n",readmsg);
                close(fd[0]);
        }
        else{
                close(fd[0]);
                write(fd[1],writemsg,strlen(writemsg)+1);
                close(fd[1]);
        }
}
```

## 52. Explain the concept of process scheduling policies such as FIFO, Round Robin, and Priority scheduling.
#### 1. FIFO (First-In, First-Out) / FCFS (First-Come, First-Served) :
- The process that arrives first in the ready queue is executed first.
- Processes are scheduled in the order they arrive.
- Non-preemptive: once a process starts, it runs until completion.
#### 2. Round Robin (RR) :
- Each process gets a fixed time slice (quantum) to execute.
- After the quantum expires, the process is put at the end of the ready queue, and the next process is scheduled.
#### 3. Priority Scheduling :
- Each process is assigned a priority.
- The CPU is given to the process with the highest priority.

## 53.Write a program in C to demonstrate the use of the nice() system call for adjusting process priority.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/time.h>
#include<sys/resource.h>
int main(){
        int i;
        pid_t pid;
        pid=fork();
        if(pid==0){
                printf("Child process (PID: %d) running with nice value 10\n", getpid());
                nice(10);
                for(int i=0;i<5;i++){
                        printf("Child process iterartion %d\n",i+1);
                        sleep(1);
                }
        }
        else{
                printf("Parent process (PID: %d) running with default nice value\n", getpid());
                for(int i=0;i<5;i++){
                        printf("parent process iterartion %d\n",i+1);
                        sleep(1);
                        }
        }
}
```

## 54.Explain the concept of process swapping and its role in memory management.
- Swapping is a memory management technique where a process is temporarily moved out of main memory (RAM) to a secondary storage (usually disk, called swap space).
- Later, it can be brought back into memory when the CPU needs to execute it again.
#### Role in Memory Management :
- Increases degree of multiprogramming: More processes can exist in the system than the size of RAM allows.
- Prevents starvation: Processes waiting for memory can still eventually get a chance to execute.
- Efficient use of RAM: Keeps CPU busy by ensuring there are always runnable processes in RAM.


## 55.Discuss the difference between the fork() and pthread_create() functions in terms of process/thread creation.
#### 1. fork() :
- Creates a new process (child process).
- The child process is an almost exact copy of the parent (same code, data, heap, stack at the time of creation).
- But it has a different address space (memory is duplicated using Copy-on-Write).
- The child has a new PID (Process ID).
#### 2. pthread_create() :
- Creates a new thread within the same process.
- The new thread runs a specified function.
- It shares the same address space (global variables, heap) with other threads.
- Each thread has its own stack and thread ID.

## 56.Describe the purpose of the prctl() system call in process management.
- prctl() stands for Process Control.
- It is a Linux-specific system call that allows a process to set or get various attributes of itself (or sometimes its children).
- Prototype (from <sys/prctl.h>):
- int prctl(int option, unsigned long arg2, unsigned long arg3,
          unsigned long arg4, unsigned long arg5);
- The behavior of prctl() depends on the option you pass.

## 57.Write a C program to demonstrate the use of the clone() system call to create a thread.
```c
#define _GNU_SOURCE 
#include<sched.h>
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#define STACK_SIZE 1024*1024
int threadfunction(void *arg){
        char *message=(char *)arg;
        for(int i=0;i<5;i++){
                printf("Child thread says:%s(%d)\n",message,i+1);
                sleep(1);
        }
}
int main(){
        char *stack;
        char *stackTop;
        int thread;
        stack=malloc(STACK_SIZE);
        if(stack==NULL){
                perror("malloc");
                exit(1);
        }
        stackTop=stack+STACK_SIZE;
        thread=clone(threadfunction,stackTop,CLONE_VM|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD,"Hello from clone thread!");
        if(thread==-1){
                perror("clone");
                free(stack);
                exit(1);
        }
        for(int i=0;i<5;i++){
                printf("parent thread is running (%d)\n",i+1);
                sleep(1);
        }
        sleep(2);
        free(stack);
}
```

## 58.Explain the concept of process preemption and its impact on system responsiveness.
- Preemption means forcibly interrupting a running process by the operating system so that another process can be executed.
- It happens in preemptive multitasking systems (like Linux, Windows, modern Unix), where the OS scheduler decides which process should run and when.
- Example: If a low-priority process is using the CPU and suddenly a high-priority process (like handling a key press or network packet) arrives, the OS preempts the low-priority process and gives the CPU to the high-priority one.
- ### Impact on System Responsiveness :
- Positive Effects:
- Better responsiveness: Interactive tasks (keyboard, mouse, UI updates) get CPU time quickly.
- Fair sharing: Prevents one process from monopolizing the CPU.
- Real-time handling: Critical tasks (e.g., multimedia playback, networking, embedded control) get immediate attention.
- Trade-offs / Negative Effects:
- Context-switch overhead: Saving and restoring process state takes time, reducing overall efficiency.
- Increased complexity: Scheduler logic becomes more complex compared to cooperative multitasking.
- Starvation risk: If high-priority tasks keep arriving, low-priority ones may rarely get CPU (unless priority aging is used).

## 59.Discuss the role of the exec functions in handling file descriptors during process execution.
- On exex(), file descriptors remain open by default unless marked with FD_CLOEXCEC.
- This allows the new program to reuse parent's open files(e.g., I/O redirection in shells).
- Example:
- ls > out.txt->shell opens out.txt,forks,child exec()s ls,and ls writes into out.txt.
## 60.Write a C program to create a child process using fork() and communicate between parent and child using pipes.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
int main(){
        int fd[2];
        pid_t pid;
        char wmsg[]="Hello from parent";
        char rmsg[100];
        if(pipe(fd)==-1){
                perror("pipe");
                exit(1);
        }
        pid=fork();
        if(pid<0){
                perror("fork");
                exit(1);
        }
        else if(pid==0){
                close(fd[1]);
                read(fd[0],rmsg,sizeof(rmsg));
                printf("Parent sent:%s\n",wmsg);
                close(fd[1]);
        }
        else{
                close(fd[0]);
                write(fd[1],wmsg,strlen(wmsg)+1);
                close(fd[1]);
        }
}
```
## 61.Explain the significance of process priorities and how they affect scheduling decisions.
- Process priority helps the scheduler decide which process should get the CPU first when multiple process are ready to run.
- High priority =more CPU time,Low-priority=less CPU time.
- Helps in : Ensuring critical tasks(real-time,kernal) run quickly. - Balancing background vs interavtive workloads.
## 62.Describe the process of process termination and the various ways it can occur.
- Process termination is the final stage of a process lifecycle, where a process ceases execution and is removed from the system.
- A process ends when:
- Normal Exit -> exit() call.
- Error Exit -> abnormal condition.
- Killed by signal -> eg., Kill -9.
- Parent killed -> with SIGKILL or job control.On termination:
- Resources are freed(memory,files).
- Exit status is sent to the parent(can be checked with wait()).
## 63.Discuss the role of the exit status in process termination and how it can be retrieved by the parent process.
- Every process return an exit status (an integer) when it terminates.
- purpose:
- Indicates success (0) or error (non-zero).
- parent uses it to decide what happened to the child.
### How parent retrives it:
- By calling wait() or waitpid() -> exit status is stored in the status variable.
- use macros WIFEXITED(status) to WEEXITSTATUS(ststus) to interpret.
```c
int status;
pid_t pid = wait(&status);
if (WIFEXITED(status)) {
    printf("Child exited with status %d\n", WEXITSTATUS(status));
}
```
## 64.Write a C program to demonstrate process synchronization using the fork() and wait() system calls.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/wait.h>
int main(){
        int pid;
        printf("parent process started (PID :%d)\n",getpid());
        pid=fork();
        if(pid<0){
                perror("fork failed");
                exit(1);
        }
        else if(pid == 0){
                printf("Child process started (PID: %d)\n",getpid());
                for(int i=1;i<=5;i++){
                        printf("Child working...%d\n",i);
                        sleep(1);
                }
                printf("Child process finished\n");
                exit(0);
        }
        else{
                printf("parent waiting for child to finish...\n");
                wait(NULL);
                printf("Child finished.Parent resumes execution.\n");
        }
        printf("Parent process exiting\n");
}
```
## 65.Explain the concept of process suspension and resumption using signals.
- Process suspension and resumption are mechanisms that allow the operating system (or user) to pause and resume a process at runtime without terminating it.
- SIGSTOP -> suspends a process.
- SIGCONT -> resumes a process.
- Sent when user presses Ctrl+Z in the terminal.Can be caught or ignored by the process.Puts the process in the stopped state.
- Example:
- kill -SIGSTOP <pid>
- kill -SIGCONT <pid>
## 66.Discuss the role of process scheduling algorithms in determining the order of execution among processes.
### Role of Process Scheduling Algorithms :
- Scheduling algorithms determine which process runs next and for how long, based on certain criteria:
- Goals:Fairness, Efficiency, Responsiveness.
- Common algorithms:
- FIFO/FCFS->Simple queue, non-preemptive.
- Round Robin(RR)->preemptive,time slices,good for multitasking.
- Priority Scheduling->Higher priority processes run first.
- Multilevel Queue->Seperate queues for foreground/background.
- Scheduling policy directly affects throughput, latency and user experience.



