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
## 21.Implement a C program to create a thread that performs selection sort on an array of integers?
```c
#include<stdio.h>
#include<pthread.h>
void *selectionsort(void *arg){
        int *arr=(int *)arg;
        int n=arr[0];
        int *data=&arr[1];
        for(int i=0;i<n-1;i++){
                int minindex=i;
                for(int j=i+1;j<n;j++){
                        if(data[j] < data[minindex]){
                                minindex=j;
                        }
                }
                if(minindex!=i){
                        int temp=data[i];
                        data[i]=data[minindex];
                        data[minindex]=temp;
                }
        }
        pthread_exit(NULL);
}
int main(){
        pthread_t thread;
        int n;
        printf("Enter the number of elements:");
        scanf("%d",&n);
        int arr[n+1];
        arr[0]=n;
        printf("Enter the elements in the array:");
        for(int i=1;i<=n;i++){
        scanf("%d",&arr[i]);
        }
        pthread_create(&thread,NULL,selectionsort,arr);
        pthread_join(thread,NULL);
        printf("Sorted array : ");
        for(int i=1;i<=n;i++){
                printf("%d ",arr[i]);
        }
        printf("\n");
}
```
## 22.Develop a C program to create a thread that calculates the area of a triangle?
```c
#include<stdio.h>
#include<pthread.h>
struct triangle{
        int b;
        int h;
}val;
void *areaoftriangle(void *arg){
        float area;
        area=0.5 * val.b * val.h;
        printf("Area of triangle:%.2f\n",area);
        pthread_exit(NULL);
}
int main(){
        pthread_t thread;
        printf("Enter the breadth:");
        scanf("%d",&val.b);
        printf("Enter the height:");
        scanf("%d",&val.h);
        pthread_create(&thread,NULL,areaoftriangle,&val);
        pthread_join(thread,NULL);
}
```
## 23.Write a C program to create a thread that calculates the sum of squares of numbers from 1 to 100?
```c
#include<stdio.h>
#include<pthread.h>
void *sumofsquares(void *arg){
        int n=(*(int *)arg);
        int sum=0;
        for(int i=1;i<=n;i++){
                sum += i*i;
        }
        printf("Sum of squares of numbers from 1 to 100:%d\n",sum);
        pthread_exit(NULL);
}
int main(){
        pthread_t thread;
        int n=100;
        pthread_create(&thread,NULL,sumofsquares,&n);
        pthread_join(thread,NULL);
}
```
## 24.Write a C program to create a thread that generates a random array of integers?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
#include<time.h>
struct Arraydata{
        int *arr;
        int size;
};
void *randomnum(void *arg){
        struct Arraydata *data=(struct Arraydata *)arg;
        srand(time(NULL));
        for(int i=0;i<data->size;i++){
                data->arr[i]=rand()%100;
        }
        pthread_exit(NULL);
}
int main(){
        pthread_t thread;
        int n;
        printf("Enter the size:");
        scanf("%d",&n);
        int arr[n];
        struct Arraydata data;
        data.size=n;
        data.arr=arr;
        pthread_create(&thread,NULL,randomnum,&data);
        pthread_join(thread,NULL);
        printf("Random Array:\n");
        for(int i=0;i<n;i++){
                printf("%d ",arr[i]);
        }
        printf("\n");
}
```
## 25.Implement a C program to create a thread that performs bubble sort on an array of integers?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
struct Arraydata{
        int size;
        int *arr;
};
void *bubblesort(void *arg){
        struct Arraydata *data=(struct Arraydata *)arg;
        int *arr=data->arr;
        for(int i=0;i<data->size;i++){
                for(int j=i+1;j<data->size;j++){
                        if(arr[i]>arr[j]){
                                int temp=arr[i];
                                arr[i]=arr[j];
                                arr[j]=temp;
                        }
                }
        }
}
int main(){
        struct Arraydata data;
        int n;
        printf("Enter the size:");
        scanf("%d",&n);
        int arr[n];
        data.size=n;
        printf("Enter the elements:\n");
        for(int i=0;i<n;i++){
                scanf("%d",&arr[i]);
        }
        data.arr=arr;
        pthread_t thread;
        pthread_create(&thread,NULL,bubblesort,&data);
        pthread_join(thread,NULL);
        printf("Array after sorted:\n");
        for(int i=0;i<n;i++){
                printf("%d ",arr[i]);
        }
}
```
## 26..Develop a C program to create a thread that calculates the greatest common divisor (GCD) of two numbers?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
struct numbers{
        int a;
        int b;
}n;
void *gcdofanumber(void *arg){
        struct numbers *nums=(struct numbers *)arg;
        int a=nums->a;
        int b=nums->b;
        while(a!=b){
                if(a>b)
                        a=a-b;
                else
                        b=b-a;
        }
        printf("Gcd of a number:%d",a);
        pthread_exit(NULL);
}
int main(){
        pthread_t thread;
        printf("Enter the two numbers:");
        scanf("%d %d",&n.a,&n.b);
        pthread_create(&thread,NULL,gcdofanumber,&n);
        pthread_join(thread,NULL);
}
```

## 27.Write a C program to create a thread that calculates the sum of Fibonacci numbers up to a given limit?
```c
#include<stdio.h>
#include<pthread.h>
void *sumoffibanacci(void *arg){
        int n=(*(int *)arg);
        int a=0,b=1,next,sum=0;
        for(int i=0;i<n;i++){
                sum += a;
                next=a+b;
                a=b;
                b=next;
        }
        printf("Sum of fibanacci series upto given limit %d is : %d\n",n,sum);
}
int main(){
        pthread_t thread;
        int limit;
        printf("Enter the limit:");
        scanf("%d",&limit);
        pthread_create(&thread,NULL,sumoffibanacci,&limit);
        pthread_join(thread,NULL);
}
```

## 28.Implement a C program to create a thread that calculates the sum of even numbers from 1 to 100?
```c
#include<stdio.h>
#include<pthread.h>
void *sumofevennumbers(void *arg){
        int sum=0;
        for(int i=0;i<=100;i++){
                if(i%2==0){
                        sum += i;
                }
        }
        printf("Sum of even numbers upto 100:%d\n",sum);
        return NULL;
}
int main(){
        pthread_t thread;
        pthread_create(&thread,NULL,sumofevennumbers,NULL);
        pthread_join(thread,NULL);
}
```
## 29.Develop a C program to create a thread that calculates the factorial of a given number using iteration?
```c
#include<stdio.h>
#include<pthread.h>
void *factorialofanumber(void *arg){
        int num=(*(int *)arg);
        int fact=1;
        for(int i=1;i<=num;i++){
                fact=fact*i;
        }
        printf("factorial of a number %d is %d\n",num,fact);
        return NULL;
}
int main(){
        int num;
        pthread_t thread;
        printf("Enter the number:");
        scanf("%d",&num);
        pthread_create(&thread,NULL,factorialofanumber,&num);
        pthread_join(thread,NULL);
}
```

## 30.Write a C program to create a thread that checks if a given year is a leap year?
```c
#include<stdio.h>
#include<pthread.h>
void *leapyearornot(void *arg){
        int year = (*(int *)arg);
        if((year%400)==0 || ((year%4)==0 && (year%100)!=0))
                printf("%d is a leapyear\n",year);
        else
                printf("%d is not a leapyear\n");
        return NULL;
}
int main(){
        int year;
        pthread_t thread;
        printf("Enter the year:");
        scanf("%d",&year);
        pthread_create(&thread,NULL,leapyearornot,&year);
        pthread_join(thread,NULL);
}
```

## 31.Implement a C program to create a thread that performs multiplication of two matrices?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
#define r1 3
#define c1 3
#define r2 3
#define c2 3
int A[r1][c1];
int B[r2][c2];
int c[r1][c2];
void *matrixmult(void *arg){
        int row=*(int *)arg;
        free(arg);
        for(int j=0;j<c2;j++){
                c[row][j]=0;
                for(int k=0;k<c1;k++){
                        c[row][j] += A[row][k] * B[k][j];
                }
        }
        return NULL;
}
int main(){
        pthread_t thread[r1];
        printf("Enter the elements in matrix1:\n");
        for(int i=0;i<r1;i++){
                for(int j=0;j<c1;j++){
                        scanf("%d",&A[i][j]);
                }
        }
        printf("Enter the elements in matrix2:\n");
        for(int i=0;i<r2;i++){
                for(int j=0;j<c2;j++){
                        scanf("%d",&B[i][j]);
                }
        }
        if(c1!=r1){
                printf("Matrix multiplication is not possible\n");
                return -1;
        }
        for(int i=0;i<r1;i++){
                int *row=malloc(sizeof(int));
                *row=i;
                pthread_create(&thread[i],NULL,matrixmult,row);
        }
        for(int i=0;i<r1;i++){
                pthread_join(thread[i],NULL);
        }
        printf("resultant Matrix:\n");
        for(int i=0;i<r1;i++){
                for(int j=0;j<c2;j++){
                        printf("%d ",c[i][j]);
                }
                printf("\n");
        }
}
```

## 32.Develop a C program to create a thread that calculates the average of numbers from 1 to 100?
```c
#include<stdio.h>
#include<pthread.h>
void *average(void *arg){
        int n=*(int *)arg;
        float sum=0,avg=0;
        for(int i=1;i<=n;i++){
                sum += i;
        }
        avg=sum/n;
        printf("Average=%.2f",avg);
        return NULL;
}
int main(){
        pthread_t thread;
        int limit;
        printf("Enter Limit:");
        scanf("%d",&limit);
        pthread_create(&thread,NULL,average,&limit);
        pthread_join(thread,NULL);
}
```

## 33.Implement a C program to create a thread that generates a random string?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
#include<time.h>
#include<string.h>
#define charset "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789"
void *generaterandomstring(void *arg){
        int n=*(int *)arg;
        char *str=malloc(n+1);
        if(str==NULL){
                printf("Memory allocation failed\n");
                pthread_exit(NULL);
        }
        int charsetsize=strlen(charset);
        for(int i=0;i<n;i++){
                str[i]=charset[rand()%charsetsize];
        }
        str[n]='\0';
        printf("Random string:%s\n",str);
        free(str);
        return NULL;
}
int main(){
        pthread_t thread;
        int length;
        srand(time(NULL));
        printf("Enter the length of the string:");
        scanf("%d",&length);
        pthread_create(&thread,NULL,generaterandomstring,&length);
        pthread_join(thread,NULL);
}
```

## 34.Write a C program to create a thread that checks if a given number is a perfect square?
```c
#include<stdio.h>
#include<pthread.h>
#include<math.h>
void *perfectsquareornot(void *arg){
        int n=*(int *)arg;
        int i=0;
        int found=0;
        while((i*i)<=n){
                if((i*i)==n){
                        found=1;
                        break;
                }
                i++;
        }
        if(found)
                printf("%d is perfect square\n",n);
        else
                printf("%d is not a perfect square\n",n);
        return NULL;
}
int main(){
        int n;
        pthread_t thread;
        printf("Enter the number:");
        scanf("%d",&n);
        pthread_create(&thread,NULL,perfectsquareornot,&n);
        pthread_join(thread,NULL);
}
```

## 35.Write a C program to create a thread that calculates the sum of digits of a given number?
```c
#include<stdio.h>
#include<pthread.h>
void *sumofdigits(void *arg){
        int num=*(int *)arg;
        int sum=0;
        while(num>0){
                sum += num%10;
                num=num/10;
        }
        printf("Sum of digits in a number:%d\n",sum);
}
int main(){
        pthread_t thread;
        int num;
        printf("Enter the number:");
        scanf("%d",&num);
        pthread_create(&thread,NULL,sumofdigits,&num);
        pthread_join(thread,NULL);
}
```

## 36.Implement a C program to create a thread that calculates the factorial of a given number using recursion?
```c
#include<stdio.h>
#include<pthread.h>
int factorialfun(int n){
        int fact=1;
        if(n==0 || n==1)
                return 1;
        else
                return n*factorialfun(n-1);
}
void *factorial(void *arg){
        int n = *(int *)arg;
        int result=factorialfun(n);
        printf("Factorial of a number:%d\n",result);
}
int main(){
        int n;
        pthread_t thread;
        printf("Enter the number:");
        scanf("%d",&n);
        pthread_create(&thread,NULL,factorial,&n);
        pthread_join(thread,NULL);
}
```

## 37.Develop a C program to create a thread that finds the maximum element in an array?
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
struct array{
        int *arr;
        int size;
};
void *maximumelement(void *arg){
        struct array *a=(struct array *)arg;
        int *arr=a->arr;
        int len=a->size;
        int max=arr[0];
        for(int i=0;i<len;i++){
                if(arr[i]>max)
                        max=arr[i];
        }
        printf("Maximum element in the array:%d\n",max);
        return NULL;
}
int main(){
        int size;
        pthread_t thread;
        struct array a;
        printf("Enter the size of the array:");
        scanf("%d",&size);
        a.size=size;
        a.arr=(int *)malloc(size * sizeof(int));
        printf("Enter the elements in the array:");
        for(int i=0;i<size;i++){
                scanf("%d",&a.arr[i]);
        }
        pthread_create(&thread,NULL,maximumelement,&a);
        pthread_join(thread,NULL);
        free(a.arr);
}
```

## 38.Write a C program to create a thread that sorts an array of strings?
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<string.h>
void *sortstrings(void *arg){
        char **arr=(char **)arg;
        int n=5;
        for(int i=0;i<n-1;i++){
                for(int j=0;j<n-i-1;j++){
                        if(strcmp(arr[j],arr[j+1])>0){
                                char *temp=arr[j];
                                arr[j]=arr[j+1];
                                arr[j+1]=temp;
                        }
                }
        }
        pthread_exit(NULL);
}
int main(){
        pthread_t th;
        char *arr[]={"banana","apple","orange","grape","cherry"};
        int n=5;
        printf("Before sorting:\n");
        for(int i=0;i<n;i++){
                printf("%s\n",arr[i]);
        }
        pthread_create(&th,NULL,sortstrings,arr);
        pthread_join(th,NULL);
        printf("\nAfter sorting:\n");
        for(int i=0;i<n;i++){
                printf("%s\n",arr[i]);
        }
}
```

## 39.Implement a C program to create a thread that calculates the square root of a number?
```c
#include<stdio.h>
#include<pthread.h>
void *squareroot(void *arg){
        int n=*(int *)arg;
        double root=0.0;
        if(n<0){
                printf("Square root of negative number is not real.\n");
                pthread_exit(NULL);
        }
        while(root*root <= n){
                root +=0.0001;
        }
        root -= 0.0001;
        printf("Square root of %d = %.2f\n",n,root);
        pthread_exit(NULL);
}
int main(){
        int n;
        pthread_t tid;
        printf("Enter the number:");
        scanf("%d",&n);
        pthread_create(&tid,NULL,squareroot,&n);
        pthread_join(tid,NULL);
}
```

## 40.Develop a C program to create a thread that calculates the average of numbers in an array?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
struct array{
        int *arr;
        int size;
};
void *average(void *arg){
        struct array *a=(struct array *)arg;
        int *arr=a->arr;
        int n=a->size;
        int sum=0;
        float avg;
        for(int i=0;i<n;i++){
                sum += arr[i];
        }
        avg=sum/n;
        printf("Average of numbers in array:%.2f\n",avg);
}
int main(){
        pthread_t tid;
        int size;
        printf("Enter the size:");
        scanf("%d",&size);
        struct array a;
        a.size=size;
        a.arr=(int *)malloc(size*sizeof(int));
        printf("Enter the elements in the array:");
        for(int i=0;i<size;i++){
                scanf("%d",&a.arr[i]);
        }
        pthread_create(&tid,NULL,average,&a);
        pthread_join(tid,NULL);
        free(a.arr);
}
```

## 41.Write a C program to create a thread that generates a random password?
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<string.h>
#include<time.h>
void *randompassword(void *arg){
        int length=*(int *)arg;
        char password[length+1];
        const char charset[]="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$%^&*()";
        srand(time(NULL));
        for(int i=0;i<length;i++){
                int index=rand() % (sizeof(charset)-1);
                password[i]=charset[index];
        }
        password[length]='\0';
        printf("Generated password: %s\n",password);
        pthread_exit(NULL);
}
int main(){
        pthread_t tid;
        int length;
        printf("Enter the length:");
        scanf("%d",&length);
        pthread_create(&tid,NULL,randompassword,&length);
        pthread_join(tid,NULL);
}
```

## 42.Implement a C program to create a thread that checks if a number is even or odd?
```c
#include<stdio.h>
#include<pthread.h>
void *evenorodd(void *arg){
        int n=*(int *)arg;
        if(n%2==0)
                printf("%d is even number\n",n);
        else
                printf("%d is odd number\n",n);
        pthread_exit(NULL);
}
int main(){
        pthread_t tid;
        int num;
        printf("Enter the number:");
        scanf("%d",&num);
        pthread_create(&tid,NULL,evenorodd,&num);
        pthread_join(tid,NULL);
}
```

## 43.Develop a C program to create a thread that calculates the sum of elements in an array?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
struct array{
        int *arr;
        int size;
};
void *sumofele(void *arg){
        struct array *a = (struct array *)arg;
        int *arr=a->arr;
        int len=a->size;
        int sum=0;
        for(int i=0;i<len;i++){
                sum += arr[i];
        }
        printf("Sum of the elements:%d\n",sum);
}
int main(){
        int size;
        pthread_t thread;
        printf("Enter the size:");
        scanf("%d",&size);
        struct array a;
        a.size=size;
        a.arr=(int *)malloc(size*sizeof(int));
        printf("Enter the elements in the array:");
        for(int i=0;i<size;i++){
                scanf("%d",&a.arr[i]);
        }
        pthread_create(&thread,NULL,sumofele,&a);
        pthread_join(thread,NULL);
}
```

## 44.Write a C program to create a thread that calculates the factorial of numbers from 1 to 10?
```c
#include<stdio.h>
#include<pthread.h>
void *factorial(void *arg){
        int fact=1;
        for(int i=1;i<=10;i++){
                fact=fact*i;
                printf("factorial of %d is %d\n",i,fact);
        }
        return NULL;
}
int main(){
        pthread_t thread;
        pthread_create(&thread,NULL,factorial,NULL);
        pthread_join(thread,NULL);
}
```

## 45.Implement a C program to create a thread that calculates the area of a rectangle?
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
struct area{
        int length;
        int breadth;
};
void *areaofrectangle(void *arg){
        struct area *a=(struct area*)arg;
        float area;
        area=(a->length)*(a->breadth);
        printf("Area of rectangle : %f",area);
        return NULL;
}
int main(){
        int length,breadth;
        pthread_t thread;
        printf("Enter the length:");
        scanf("%d",&length);
        printf("Enter the breadth:");
        scanf("%d",&breadth);
        struct area a;
        a.length=length;
        a.breadth=breadth;
        pthread_create(&thread,NULL,areaofrectangle,&a);
        pthread_join(thread,NULL);
}
```

## 46.Develop a C program to create a thread that generates random numbers?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
#include<time.h>
void *randomnumber(void *arg){
        int len=*(int *)arg;
        for(int i=0;i<len;i++){
                int n=rand()%100;
                printf("Random numbers:%d\n",n);
        }
        return NULL;
}
int main(){
        int n;
        printf("How many randon numbers to generate?\n");
        scanf("%d",&n);
        pthread_t thread;
        srand(time(NULL));
        pthread_create(&thread,NULL,randomnumber,&n);
        pthread_join(thread,NULL);
}
```

## 47.Implement a C program to create a thread that sorts an array of integers?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
struct array{
        int *arr;
        int size;
};
void *sortanarray(void *arg){
        struct array *a=(struct array *)arg;
        int *arr=a->arr;
        int n=a->size;
        int max=arr[0];
        for(int i=0;i<n;i++){
                for(int j=i+1;j<n;j++){
                        if(arr[i]>arr[j]){
                                int temp=arr[i];
                                arr[i]=arr[j];
                                arr[j]=temp;
                        }
                }
        }
        return NULL;
}
int main(){
        pthread_t thread;
        int size;
        printf("Enter the size:");
        scanf("%d",&size);
        struct array a;
        a.size=size;
        a.arr=(int *)malloc(size * sizeof(int));
        printf("Enter the elements in the array:");
        for(int i=0;i<a.size;i++){
                scanf("%d",&a.arr[i]);
        }
        pthread_create(&thread,NULL,sortanarray,&a);
        pthread_join(thread,NULL);
        printf("Array after sorting:");
        for(int i=0;i<size;i++){
                printf("%d ",a.arr[i]);
        }
        free(a.arr);
}
```

## 48.Write a C program to create a thread that searches for a given number in an array?
```c
#include<stdio.h>
#include<pthread.h>
#include<stdlib.h>
struct array{
        int *arr;
        int size;
        int ele;
};
void *sortanarray(void *arg){
        struct array *a=(struct array *)arg;
        int *arr=a->arr;
        int n=a->size;
        int search=a->ele;
        int found=0;
        for(int i=0;i<n;i++){
                if(arr[i]==search){
                        found=1;
                        break;
                }
        }
        if(found)
                printf("%d element is found\n",search);
        else
                printf("%d element is not found\n",search);

        return NULL;
}
int main(){
        pthread_t thread;
        int size,search;
        printf("Enter the size:");
        scanf("%d",&size);
        struct array a;
        a.size=size;
        a.arr=(int *)malloc(size * sizeof(int));
        printf("Enter the elements in the array:");
        for(int i=0;i<a.size;i++){
                scanf("%d",&a.arr[i]);
        }
        printf("Enter the element to search:");
        scanf("%d",&search);
        a.ele=search;
        pthread_create(&thread,NULL,sortanarray,&a);
        pthread_join(thread,NULL);
        free(a.arr);
}
```

## 49.Develop a C program to create a thread that reverses a given string?
```c
#include<stdio.h>
#include<pthread.h>
#include<string.h>
void *reversestring(void *arg){
        char *str = (char *)arg;
        int len = strlen(str);
        for(int i=0,j=len-1;i<j;i++,j--){
                char temp=str[i];
                str[i]=str[j];
                str[j]=temp;
        }
        return NULL;
}
int main(){
        char str[100];
        printf("Enter the string:");
        fgets(str,sizeof(str),stdin);
        str[strcspn(str,"\n")]='\0';
        pthread_t thread;
        pthread_create(&thread,NULL,reversestring,str);
        pthread_join(thread,NULL);
        printf("String after reversing:%s\n",str);
}
```

## 50.Implement a C program to create a thread that reads input from the user?
```c
#include<stdio.h>
#include<pthread.h>
#include<string.h>
void *readinput(void *arg){
        char *str = (char *)arg;
        printf("Enter the string:");
        fgets(str,100,stdin);
        str[strcspn(str,"\n")]='\0';
        return NULL;
}
int main(){
        char str[100];
        pthread_t thread;
        pthread_create(&thread,NULL,readinput,str);
        pthread_join(thread,NULL);
        printf("String : %s\n",str);
}
```

## 51.Write a C program to create a thread that performs addition of two matrices?
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#define max 10
struct matrixdata{
        int A[max][max];
        int B[max][max];
        int result[max][max];
        int rows;
        int cols;
};
void *additionofmatrix(void *arg){
        struct matrixdata *data=(struct matrixdata *)arg;
        for(int i=0;i<data->rows;i++){
                for(int j=0;j<data->cols;j++){
                        data->result[i][j]=data->A[i][j]+data->B[i][j];
                }
        }
        return NULL;
}
int main(){
        struct matrixdata data;
        pthread_t thread;
        printf("Enter number of row:");
        scanf("%d",&data.rows);
        printf("Enter number of colums:");
        scanf("%d",&data.cols);
        printf("Enter the matrix1:\n");
        for(int i=0;i<data.rows;i++){
                for(int j=0;j<data.cols;j++){
                        scanf("%d",&data.A[i][j]);
                }
        }
        printf("Enter the matrix2:\n");
        for(int i=0;i<data.rows;i++){
                for(int j=0;j<data.cols;j++){
                        scanf("%d",&data.B[i][j]);
                }
        }
        pthread_create(&thread,NULL,additionofmatrix,&data);
        pthread_join(thread,NULL);
        printf("Resultant matrix after addition:\n");
        for(int i=0;i<data.rows;i++){
                for(int j=0;j<data.cols;j++){
                        printf("%d ",data.result[i][j]);
                }
                printf("\n");
        }
}
```

## 52.Develop a C program to create a thread that calculates the length of a given string?
```c
#include<stdio.h>
#include<pthread.h>
#include<string.h>
void *strlength(void *arg){
        char *str=(char *)arg;
        int i=0;
        while(str[i]!='\0'){
                i++;
        }
        printf("Length of the %s : %d",str,i);
}
int main(){
        pthread_t thread;
        char str[100];
        printf("Enter the string:");
        fgets(str,sizeof(str),stdin);
        str[strcspn(str,"\n")]='\0';
        pthread_create(&thread,NULL,strlength,str);
        pthread_join(thread,NULL);
}
```

## 53.Write a C program to create two threads using pthreads library. Each thread should print "Hello, World!" along with its thread ID?
```c
#include<stdio.h>
#include<unistd.h>
#include<pthread.h>
void *print(void *arg){
        printf("Hello wold\n");
        printf("PID : %d\n",getpid());
        return NULL;
}
int main(){
        pthread_t thread1,thread2;
        pthread_create(&thread1,NULL,print,NULL);
        pthread_create(&thread2,NULL,print,NULL);
        pthread_join(thread1,NULL);
        pthread_join(thread2,NULL);
}
```

## 54.Modify the previous program to pass arguments to the threads and print those arguments along with the thread ID?
```c
#include<stdio.h>
#include<unistd.h>
#include<pthread.h>
void *print(void *arg){
        int n=*(int *)arg;
        printf("Hello wold\n");
        printf("PID of %d : %d\n",n,getpid());
        return NULL;
}
int main(){
        pthread_t thread1,thread2;
        int t1=1,t2=2;
        pthread_create(&thread1,NULL,print,&t1);
        pthread_create(&thread2,NULL,print,&t2);
        pthread_join(thread1,NULL);
        pthread_join(thread2,NULL);
}
```

## 55.Write a C program to demonstrate thread synchronization using mutex locks. Create two threads that increment a shared variable using mutex locks to ensure proper synchronization?
```c
#include<stdio.h>
#include<pthread.h>
int x=10;
pthread_mutex_t lock;
void *incrementation(void *arg){
        int n=*(int *)arg;
        for(int i=0;i<n;i++){
                pthread_mutex_lock(&lock);
                x++;
                pthread_mutex_unlock(&lock);
        }
        return NULL;
}
int main(){
        pthread_t thread1,thread2;
        int increments=10;
        pthread_mutex_init(&lock,NULL);
        pthread_create(&thread1,NULL,incrementation,&increments);
        pthread_create(&thread2,NULL,incrementation,&increments);
        pthread_join(thread1,NULL);
        pthread_join(thread2,NULL);
        pthread_mutex_destroy(&lock);
        printf("Final value of shared variable:%d\n",x);
}
```
