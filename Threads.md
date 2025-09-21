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
