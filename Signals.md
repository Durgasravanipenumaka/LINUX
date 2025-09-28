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
