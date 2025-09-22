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

## 6.Develop a C program to create a thread that prints the sum of two numbers?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
typedef struct{
        int a;
        int b;
}numbers;
void *print(void *arg){
        numbers *num=(numbers *)arg;
        int sum=num->a + num->b;
        printf("Sum of %d and %d is %d\n",num->a,num->b,sum);
}
int main(){
        pthread_t thread;
        numbers *num=malloc(sizeof(numbers));
        num->a=10;
        num->b=20;
        pthread_create(&thread,NULL,print,num);
        pthread_join(thread,NULL);
        free(num);
}
```

## 7.Implement a C program to create a thread that calculates the square of a number?
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
void *square(void *arg){
        int num=(*(int *)arg);
        int res=num*num;
        printf("Square of %d : %d\n",num,res);
}
int main(){
        pthread_t thread;
        int num;
        printf("Enter the number:");
        scanf("%d",&num);
        pthread_create(&thread,NULL,square,&num);
        pthread_join(thread,NULL);
}
```

## 8.Write a C program to create a thread that prints the current date and time?
```c
#include<stdio.h>
#include<pthread.h>
void *dateandtime(void *arg){
        time_t now;
        struct tm *timeinfo;
        time(&now);
        timeinfo=localtime(&now);
        printf("Current time=%s",asctime(timeinfo));
}
int main(){
        pthread_t thread;
        pthread_create(&thread,NULL,dateandtime,NULL);
        pthread_join(thread,NULL);
}
```

## 9.Develop a C program to create a thread that checks if a number is prime?
```c
#include<stdio.h>
#include<pthread.h>
void *primeornot(void *arg){
        int num=(*(int *)arg);
        int flag=0;
        for(int i=2;i<=num/2;i++){
                if(num%i==0){
                        flag=1;
                        break;
                }
        }
        if(flag)
                printf("%d is not a Prime number",num);
        else
                printf("%d is a prime number",num);
}
int main(){
        pthread_t thread;
        int num;
        printf("Enter the number:");
        scanf("%d",&num);
        pthread_create(&thread,NULL,primeornot,&num);
        pthread_join(thread,NULL);
}
```

## 10.Implement a C program to create a thread that checks if a given string is a palindrome?
```c
#include<stdio.h>
#include<pthread.h>
#include<string.h>
void *palindromeornot(void *arg){
        char *str=(char *)arg;
        int flag=0;
        int len=strlen(str);
        for(int i=0,j=len-1;i<j;i++,j--){
                if(str[i]!=str[j]){
                        flag=1;
                        break;
                }
        }
        if(!flag)
                printf("%s is a palindrome\n",str);
        else
                printf("%s is not a palindrome\n",str);
}

int main(){
        pthread_t thread;
        char str[100];
        printf("Enter the string:");
        fgets(str,sizeof(str),stdin);
        str[strcspn(str,"\n")]='\0';
        pthread_create(&thread,NULL,palindromeornot,str);
        pthread_join(thread,NULL);
}
```

## 11.Write a C program to create a thread that prints "Hello, World!" with thread synchronization?
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t lock;
void *print(void *arg){
        pthread_mutex_lock(&lock);
        printf("Hello world\n");
        pthread_mutex_unlock(&lock);
}
int main(){
        pthread_t thread1,thread2;
        pthread_mutex_init(&lock,NULL);
        pthread_create(&thread1,NULL,print,NULL);
        pthread_create(&thread2,NULL,print,NULL);
        pthread_join(thread1,NULL);
        pthread_join(thread2,NULL);
        pthread_mutex_destroy(&lock);
}
```

## 12.Develop a C program to create two threads that print their thread IDs and synchronize their output?
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t lock;
void *printids(void *arg){
        pthread_mutex_lock(&lock);
        pthread_t id=pthread_self();
        printf("ids : %lu\n",(unsigned long)id);
        pthread_mutex_unlock(&lock);
}
int main(){
        pthread_t thread1,thread2;
        pthread_mutex_init(&lock,NULL);
        pthread_create(&thread1,NULL,printids,NULL);
        pthread_create(&thread2,NULL,printids,NULL);
        pthread_join(thread1,NULL);
        pthread_join(thread2,NULL);
        pthread_mutex_destroy(&lock);
}
```

## 13.Implement a C program to create a thread that generates random numbers and synchronizes access to a shared buffer?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<time.h>
#include<pthread.h>
#define size 10
int buf[size];
int index=0;
pthread_mutex_t lock;
void *print(void *arg){
        srand(time(NULL));
        for(int i=0;i<size;i++){
             pthread_mutex_lock(&lock);
             int num=rand()%100;
             buf[index++]=num;
             printf("num=%d\n",num);
             pthread_mutex_unlock(&lock);
             usleep(100);
        }

}
int main(){
        pthread_t thread;
        pthread_mutex_init(&lock,NULL);
        pthread_create(&thread,NULL,print,NULL);
        pthread_join(thread,NULL);
        for(int i=0;i<size;i++){
                printf("buf=%d",buf[i]);
        }
        pthread_mutex_destroy(&lock);
}
```

