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

## 14..Write a C program to create a thread that performs addition of two numbers with mutex locks?
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
pthread_mutex_t lock;
typedef struct{
        int a;
        int b;
}num;
void *sumoftwonum(void *arg){
        num *n = (num *)arg;
        pthread_mutex_lock(&lock);
        int sum=n->a + n->b;
        printf("sum of %d and %d is %d\n",n->a,n->b,sum);
        pthread_mutex_unlock(&lock);
}
int main(){
        pthread_t thread;
        pthread_mutex_init(&lock,NULL);
        num *n=malloc(sizeof(num));
        n->a=10;
        n->b=20;
        pthread_create(&thread,NULL,sumoftwonum,n);
        pthread_join(thread,NULL);
        free(n);
        pthread_mutex_destroy(&lock);
}
```
## 15..Implement a C program to create two threads that increment and decrement a shared variable, respectively, using mutex locks?
```c
#include<stdio.h>
#include<pthread.h>
int a=10;
pthread_mutex_t lock;
void *increment(void *arg){
        pthread_mutex_lock(&lock);
        a++;
        printf("Value after incremented:%d\n",a);
        pthread_mutex_unlock(&lock);
}
void *decrement(void *arg){
        pthread_mutex_lock(&lock);
        a--;
        printf("Value after decrement:%d\n",a);
        pthread_mutex_unlock(&lock);
}
int main(){
        pthread_t incthread;
        pthread_t decthread;
        pthread_mutex_init(&lock,NULL);
        pthread_create(&incthread,NULL,increment,NULL);
        pthread_create(&decthread,NULL,decrement,NULL);
        pthread_join(incthread,NULL);
        pthread_join(decthread,NULL);
        printf("Value=%d\n",a);
        pthread_mutex_destroy(&lock);
}
```
## 16.Develop a C program to create a thread that reads input from the user and synchronizes access to shared resources?
```c
#include<stdio.h>
#include<pthread.h>
#include<string.h>
pthread_mutex_t lock;
char buf[100];
void *print(void *arg){
        char temp[100];
        pthread_mutex_lock(&lock);
        printf("Enter the string :");
        fgets(temp,sizeof(temp),stdin);
        strcpy(buf,temp);
        pthread_mutex_unlock(&lock);
        pthread_exit(NULL);
}
int main(){
        pthread_t thread;
        pthread_mutex_init(&lock,NULL);
        pthread_create(&thread,NULL,print,NULL);
        pthread_join(thread,NULL);
        pthread_mutex_lock(&lock);
        printf("you entered from shared resource:%s",buf);
        pthread_mutex_unlock(&lock);
        pthread_mutex_destroy(&lock);
}
```
## 17.Implement a C program to create a thread that prints prime numbers up to a given limit with mutex locks?
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t lock;
int primeornot(int num){
        if(num<2)
                return 0;
        for(int i=2;i<num;i++){
                if(num%i==0)
                        return 0;
        }
        return 1;
}

void *primenumbers(void *arg){
        int num=(*(int *)arg);
        pthread_mutex_lock(&lock);
        printf("Prime numbers upto %d :\n",num);
        for(int i=2;i<=num;i++){
                if(primeornot(i))
                        printf("%d\n",i);
        }
        pthread_mutex_unlock(&lock);
        pthread_exit(NULL);
}
int main(){
        pthread_t thread;
        int num;
        printf("Enter the number:");
        scanf("%d",&num);
        pthread_mutex_init(&lock,NULL);
        pthread_create(&thread,NULL,primenumbers,&num);
        pthread_join(thread,NULL);
        pthread_mutex_destroy(&lock);
}
```
## 18.Implement a C program to create a thread that calculates the sum of Fibonacci numbers upto a given limit using dynamic programming with mutex locks?
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t lock;
int n;
int fib[100];
int sum=0;
void *fibanaccisum(void *arg){
        pthread_mutex_lock(&lock);
        fib[0]=0;
        fib[1]=1;
        sum=fib[0]+fib[1];
        for(int i=2;i<n;i++){
                fib[i]=fib[i-1]+fib[i-2];
                sum+=fib[i];
        }
        printf("Fibanacci numbers upto %d terms:",n);
        for(int i=0;i<n;i++){
                printf("%d ",fib[i]);
        }
        printf("Sum of the fibanacci numbers:%d\n",sum);
        pthread_mutex_unlock(&lock);
        pthread_exit(NULL);
}
int main(){
        pthread_t thread;
        printf("Enter number of terms:");
        scanf("%d",&n);
        if(n<=0){
                printf("Number of terme should be positive only\n");
                return 1;
        }
        pthread_mutex_init(&lock,NULL);
        pthread_create(&thread,NULL,fibanaccisum,NULL);
        pthread_join(thread,NULL);
        pthread_mutex_destroy(&lock);
}
```
## 19.Write a C program to create a thread that checks if a given year is a leap year using dynamic programming with mutex locks?
```c
#include<stdio.h>
#include<pthread.h>
#define max 1000
int dp[max];
pthread_mutex_t lock;
void *Leapyearornot(void *arg){
        int year=(*(int *)arg);
        pthread_mutex_lock(&lock);
        if(year<max && dp[year]!=-1){
                if(dp[year]==1)
                        printf("%d is a leap year",year);
                else
                        printf("%d is not a leap year",year);
        }
        else{
                int result=0;
                if((year%400==0)||((year%4==0)&&(year%100!=0)))
                        result=1;
                if(year<max)
                        dp[year]=result;
                if(result==1)
                        printf("%d is leap year",year);
                else
                        printf("%d is not a leap year",year);
        }
        pthread_mutex_unlock(&lock);
        pthread_exit(NULL);
}
int main(){
        pthread_t thread;
        int year;
        printf("Enter the year:");
        scanf("%d",&year);
        pthread_mutex_init(&lock,NULL);
        pthread_create(&thread,NULL,Leapyearornot,&year);
        pthread_join(thread,NULL);
        pthread_mutex_destroy(&lock);
}
```
## 20.Write a C program to create a thread that checks if a given string is a palindrome using dynamic programming with mutex locks?
```c
#include<stdio.h>
#include<string.h>
#include<pthread.h>
char str[100];
pthread_mutex_t lock;
void *palindromeornot(void *arg){
        char *str=(char *)arg;
        int n=strlen(str);
        int dp[n][n];
        pthread_mutex_lock(&lock);
        for(int i=0;i<n;i++){
                for(int j=0;j<n;j++){
                        dp[i][j]=0;
                }
        }
        for(int i=0;i<n;i++)
                dp[i][i]=1;
        for(int i=0;i<n-1;i++){
                if(str[i]==str[i+1])
                        dp[i][i+1]=1;
        }
        for(int len=3;len<=n;len++){
                for(int i=0;i<-n-len;i++){
                        int j=i+len-1;
                        if(str[i]==str[j] && dp[i+1][j-1])
                                dp[i][j]=1;
                }
        }
        if(dp[0][n-1])
                printf("Palindrome\n");
        else
                printf("Not a Palindrome\n");
        pthread_mutex_unlock(&lock);
        pthread_exit(NULL);

}
int main(){
        pthread_t thread;
        printf("Enter the string:");
        fgets(str,sizeof(str),stdin);
        str[strcspn(str,"\n")]='\0';
        pthread_mutex_init(&lock,NULL);
        pthread_create(&thread,NULL,palindromeornot,str);
        pthread_join(thread,NULL);
        pthread_mutex_destroy(&lock);
}
```


