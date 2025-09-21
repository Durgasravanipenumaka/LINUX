## 1.Write a C program to create a thread that prints "Hello, World!"?
```c
#include<stdio.h>
#include<pthread.h>
void *print(void *arg){
        printf("Hello world\n");
        return NULL;
}
int main(){
        pthread_t threadid;
        if(pthread_create(&threadid,NULL,print,NULL)!=0){
                perror("Error");
                return 1;
        }
        pthread_join(threadid,NULL);
}
```

## 2.Modify the above program to create multiple threads, each printing its own message?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
#define n 5
void *print(void *args){
        int nthreads=*((int *)args);
        printf("Number of threads:%d\n",nthreads);
        free(args);
        return NULL;
}
int main(){
        pthread_t nthreads[n];
        for(int i=0;i<n;i++){
                int *thread=malloc(sizeof(int));
                *thread=(i+1);
                if(pthread_create(&nthreads[i],NULL,print,thread)!=0){
                        perror("thread error");
                        return 1;
                }
        }
        for(int i=0;i<n;i++){
                pthread_join(nthreads[i],NULL);
        }
}
```

## 3.Develop a C program to create two threads that print numbers from 1 to 10 concurrently?
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
void *print(void *arg){
        int num=(*(int *)arg);
        for(int i=1;i<=10;i++){
                printf("%d %d\n",i,num);
                usleep(10000);
        }
}
int main(){
        pthread_t thread1;
        pthread_t thread2;
        int id1=1,id2=2;
        pthread_create(&thread1,NULL,print,&id1);
        pthread_create(&thread2,NULL,print,&id2);
        pthread_join(thread1,NULL);
        pthread_join(thread2,NULL);
}
```

## 4.Implement a C program to create a thread that calculates the factorial of a given number?
```c
#include<stdio.h>
#include<pthread.h>
void *factorial(void *arg){
        int fact=1;
        int num=(*(int *)arg);
        while(num){
                fact=fact*num;
                num--;
        }
        printf("factorial:%d\n",fact);
}
int main(){
        pthread_t thread;
        int n;
        printf("Enter the number:");
        scanf("%d",&n);
        pthread_create(&thread,NULL,factorial,&n);
        pthread_join(thread,NULL);
}
```

## 5.Write a C program to create two threads that print their thread IDs?
```c
#include<stdio.h>
#include<pthread.h>
void *print(void *arg){
        pthread_t id=pthread_self();
        printf("thread id : %lu\n",(unsigned long)id);
}
int main(){
        pthread_t thread1;
        pthread_t thread2;
        pthread_create(&thread1,NULL,print,NULL);
        pthread_create(&thread2,NULL,print,NULL);
        pthread_join(thread1,NULL);
        pthread_join(thread2,NULL);
}
```
