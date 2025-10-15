## 1.Write a C program to demonstrate dynamic memory allocation using malloc().
```c
#include<stdio.h>
#include<stdlib.h>
int main(){
        int *ptr;
        int n;
        printf("Enter the number of elements:");
        scanf("%d",&n);
        ptr=(int *)malloc(n*sizeof(int));
        if(ptr==NULL){
                printf("Memory not allocated");
                exit(0);
        }
        else{
                printf("Memory sucessfully allocated using malloc.\n");
                for(int i=0;i<n;i++){
                        printf("Enter the element:");
                        scanf("%d",&ptr[i]);
                }
                printf("Elents are:");
                for(int i=0;i<n;i++){
                        printf("%d ",ptr[i]);
                }
                printf("\n");
                free(ptr);
        }
}
```

## 2.Implement a C program to allocate memory for an array dynamically using calloc().
```c
#include<stdio.h>
#include<stdlib.h>
int main(){
        int n,*ptr;
        printf("Enter number of elements:");
        scanf("%d",&n);
        ptr=(int *)calloc(n,sizeof(int));
        if(ptr==NULL){
                printf("Memory not allocated");
                exit(0);
        }
        else{
                printf("Memory successfully allocated.\n");
                for(int i=0;i<n;i++){
                        printf("Enter the element:");
                        scanf("%d",&ptr[i]);
                }
                printf("Elements are:");
                for(int i=0;i<n;i++){
                        printf("%d ",ptr[i]);
                }
                printf("\n");
                free(ptr);
        }
}
```

## 3.Write a C program to resize dynamically allocated memory using realloc().
```c
#include<stdio.h>
#include<stdlib.h>
int main(){
        int n,*ptr;
        printf("Enter the number of elements:");
        scanf("%d",&n);
        ptr=(int *)malloc(n*sizeof(int));
        if(ptr==NULL){
                printf("Memory is not allocated\n");
                exit(0);
        }
        printf("Memory successfully allocated.\n");
        for(int i=0;i<n;i++){
                printf("Enter the number:");
                scanf("%d",&ptr[i]);
        }
        printf("\nEntered elements:");
        for(int i=0;i<n;i++){
                printf("%d ",ptr[i]);
        }
        int newn;
        printf("\nEnter the new size:");
        scanf("%d",&newn);
        ptr=(int *)realloc(ptr,newn*sizeof(int));
        if(ptr==NULL){
                printf("Memory reallocated failed");
                exit(0);
        }
        printf("Memory successfully reallocated using realloc.\n");
        if(newn>n){
                for(int i=n;i<newn;i++){
                        printf("Enter new element:");
                        scanf("%d",&ptr[i]);
                }
        }
        printf("Final array:");
        for(int i=0;i<newn;i++){
                printf("%d ",ptr[i]);
        }
        free(ptr);
}
```

## 4.Develop a program in C to allocate memory for a linked list node dynamically.
```c
#include<stdio.h>
#include<stdlib.h>
struct node{
        int data;
        struct node *nxt;
};
int main(){
        struct node *head=NULL;
        head=(struct node *)malloc(sizeof(struct node));
        if(head==NULL){
                printf("Memory not allocated.\n");
                exit(0);
        }
        head->data=10;
        head->nxt=NULL;
        printf("Node created successfully.\n");
        printf("Data=%d\n",head->data);
        free(head);
}
```

## 5.Implement a C program to simulate memory allocation using the first-fit algorithm.
```c
#include<stdio.h>
int main(){
        int memoryblocks[10],processes[10];
        int allocation[10];
        int blockcount,processcount;
        for(int i=0;i<10;i++){
                allocation[i]=-1;
        }
        printf("Enter number of memory blocks :");
        scanf("%d",&blockcount);
        printf("Enter size of each memory block :\n");
        for(int i=0;i<blockcount;i++){
                scanf("%d",&memoryblocks[i]);
        }
        printf("Enter number of process:");
        scanf("%d",&processcount);
        printf("Enter the size of each process:\n");
        for(int i=0;i<processcount;i++){
                scanf("%d",&processes[i]);
        }
        for(int i=0;i<processcount;i++){
                for(int j=0;j<blockcount;j++){
                        if(memoryblocks[j]>=processes[i]){
                                allocation[i]=j;
                                memoryblocks[j] -= processes[i];
                                break;
                        }
                }
        }
        printf("\nprocess No.\tprocess Size\tBlock Allocated\n");
        for(int i=0;i<processcount;i++){
                printf("%d\t\t%d\t\t",i+1,processes[i]);
                if(allocation[i]!=-1)
                        printf("%d\n",allocation[i]+1);
                else
                        printf("Not allocated");
        }
        printf("\n");
}
```

## 6.Write a C program to simulate memory allocation using the best-fit algorithm.
```c
#include<stdio.h>
int main(){
        int memoryblocks[10],process[10];
        int blockcount,processblock;
        int allocation[10];
        printf("Enter number of memory blocks:");
        scanf("%d",&blockcount);
        printf("Enter sizes of memory blocks:");
        for(int i=0;i<blockcount;i++){
                scanf("%d",&memoryblocks[i]);
        }
        printf("Enter the number of process:");
        scanf("%d",&processblock);
        printf("Enter memory required for each process:\n");
        for(int i=0;i<processcount;i++){
                scanf("%d",&process[i]);
                allocation[i]=-1;
        }
        for(int i=0;i<processcount;i++){
                int bestindex=-1;
                for(int j=0;j<blockcount;j++){
                        if(memoryblock[j]>=process[i]){
                                if(bestindex=-1 || memoryblocks[i] < memoryblocks[bestindex])
                                        bestindex=j;
                        }
                }
                if(bestindex != -1){
                        allocation[i]=bestindex;
                        memoryblocks[bestindex] = memoryblocks[bestindex]-process[i];
                }
        }
        printf("\nProcess No.\tProcess Size\tBlock No.\n");
        for(int i=0;i<processcount;i++){
                printf("%d\t%d\t\t",i+1,process[i]);
                if(allocation[i] != -1)
                        printf("%d\n",allocation[i]+1);
                else
                        printf("Not allocation\n");
        }
}
```

## 7.Develop a C program to simulate memory allocation using the worst-fit algorithm.
```c
#include<stdio.h>
int main(){
        int memoryblocks[10],process[10];
        int blockcount,processblock;
        int allocation[10];
        printf("Enter number of memory blocks:");
        scanf("%d",&blockcount);
        printf("Enter sizes of memory blocks:\n");
        for(int i=0;i<blockcount;i++){
                scanf("%d",&memoryblocks[i]);
        }
        printf("Enter number of processes:");
        scanf("%d",&processblock);
        printf("Enter memory required for each process:\n");
        for(int i=0;i<processblock;i++){
                scanf("%d",&process[i]);
                allocation[i]=-1;
        }
        for(int i=0;i<processblock;i++){
                int worstindex=-1;
                for(int j=0;j<blockcount;j++){
                        if(memoryblocks[j]>=process[i]){
                                if(worstindex==-1 || memoryblocks[j]>memoryblocks[worstindex])
                                        worstindex = j;
                        }
                }
                if(worstindex != -1){
                        allocation[i]=worstindex;
                        memoryblocks[worstindex] -= process[i];
                }
        }
        printf("\nProcess No.\tProcess Size\tBlock No.\n");
        for(int i=0;i<processblock;i++){
                printf("%d\t\t%d\t\t",i+1,process[i]);
                if(allocation[i]!=-1)
                        printf("%d\n",allocation[i] +1);
                else
                        printf("Not Allocated\n");
        }
}
```

## 8.Implement a C program to simulate memory allocation using the next-fit algorithm.
```c
#include<stdio.h>
int main(){
        int memoryblocks[10],process[10];
        int blockcount,processcount;
        int allocation[10];
        printf("Enter the number of memory blocks:");
        scanf("%d",&blockcount);
        printf("Enter the sizes in the memory blocks:");
        for(int i=0;i<blockcount;i++){
                scanf("%d",&memoryblocks[i]);
        }
        printf("Enter the number of process:");
        scanf("%d",&processcount);
        printf("Enter the memory required for each process:");
        for(int i=0;i<processcount;i++){
                scanf("%d",&process[i]);
                allocation[i]=-1;
        }
        int lastallocated=0;
        for(int i=0;i<processcount;i++){
                int j=lastallocated;
                int count=0;
                while(count<blockcount){
                        if(memoryblocks[j]>=process[i]){
                                allocation[i]=j;
                                memoryblocks[j] -= process[i];
                                lastallocated=j;
                                break;
                        }
                        j=(j+1)%blockcount;
                        count++;
                }
        }
        printf("\nProcess No.\tProcess Size\tBlock No.\n");
        for(int i=0;i<processcount;i++){
                printf("%d\t\t%d\t\t",i+1,process[i]);
                if(allocation[i] != -1)
                        printf("%d\n",allocation[i]+1);
                else
                        printf("Not allocated\n");
        }
}
```

## 9.Write a C program to implement a simple memory allocator using the buddy system.
```c
#include<stdio.h>
#include<math.h>
#define max 1024
int nextpoweroftwo(int n){
        int power=1;
        while(power<n)
                power *= 2;
        return power;
}
int main(){
        int totalmemory=max;
        int allocated[10]={0};
        int processsize,processcount;
        printf("Total available memory : %d KB\n ",totalmemory);
        printf("Enter number of processes:");
        scanf("%d",&processcount);
        for(int i=0;i<processcount;i++){
                printf("\nEnter memory required for process %d (in KB): ",i+1);
                scanf("%d",&processsize);
                if(processsize > totalmemory){
                        printf("Not enough memory to allocate for process %d\n",i+1);
                        continue;
                }
                int blocksize=nextpoweroftwo(processsize);
                if(blocksize > totalmemory){
                        printf("Cannot allocate %d KB for process %d\n",processsize,i+1);
                        continue;
                }
                allocated[i]=blocksize;
                totalmemory -= blocksize;
                printf("process %d allocated %d KB block(buddy size)\n",i+1,blocksize);
                printf("Remaining memory : %d KB\n",totalmemory);
        }
        printf("\n Now freeing all allocated memory..\n");
        for(int i=0;i<processcount;i++){
                if(allocated[i] != 0){
                        totalmemory += allocated[i];
                        printf("Freed %d KB from process %d.Total memory:%d KB\n",allocated[i],i+1,totalmemory);
                }
        }
        printf("\nAll memory is free again: %d KB\n",totalmemory);
}
```

## 10.Develop a C program to implement a memory allocator using a custom memory management algorithm.
```c
#include<stdio.h>
#include<stdlib.h>
#define size 100
int memory[size];
void allocate(int start,int length){
        for(int i=start;i<start+length;i++){
                memory[i]=1;
        }
}
void freememory(int start,int length){
        for(int i=start;i<start+length;i++){
                memory[i]=0;
        }
}
void display(){
        for(int i=0;i<size;i++)
                printf("%d ",memory[i]);
        printf("\n");
}
int main(){
        for(int i=0;i<size;i++){
                memory[i]=0;
        }
        printf("Initial Memory (0=free, 1=used):\n");
        display();
        allocate(10,50);
        printf("\nAfter allocating 50 bytes from index 10 :\n");
        display();
        freememory(20,20);
        printf("\nAfter freeing 20 bytes from index 20 :\n");
        display();
}
```

## 11.Write a C program to demonstrate memory mapping using mmap().
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/mman.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<unistd.h>
#include<string.h>
int main(){
        int fd;
        char *map;
        fd=open("mmap.txt",O_RDWR|O_CREAT,0666);
        if(fd<0){
                perror("open");
                return 1;
        }
        ftruncate(fd,100);
        map=mmap(NULL,100,PROT_READ|PROT_WRITE,MAP_SHARED,fd,0);
        if(map==MAP_FAILED){
                perror("mmap");
                close(fd);
                return 1;
        }
        strcpy(map,"Hello! This text is written usingg mmap().");
        printf("Data written using mmap:%s\n",map);
        munmap(map,100);
        close(fd);
}
```

## 12.Implement a C program to read from and write to a memory-mapped file.
```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<sys/mman.h>
#include<sys/stat.h>
#include<unistd.h>
#include<string.h>
int main(){
        int fd;
        char *map;
        fd=open("map.txt",O_RDWR|O_CREAT,0666);
        if(fd<0){
                printf("Errorin opening file");
                exit(1);
        }
        ftruncate(fd,100);
        map=mmap(NULL,100,PROT_READ|PROT_WRITE,MAP_SHARED,fd,0);
        if(map==MAP_FAILED){
                perror("mmap");
                close(fd);
                exit(1);
        }
        strcpy(map,"Writing data into a memory-mapped file.");
        printf("Reading from mapped file:\n%s\n",map);
        munmap(map,100);
        close(fd);
}
```

## 13.Develop a C program to demonstrate shared memory usage using shmget() and shmat().
### Writer :
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<string.h>
int main(){
        key_t key = ftok("shmfile",65);
        int shmid=shmget(key,1024,0666|IPC_CREAT);
        char *str = (char*) shmat(shmid,NULL,0);
        printf("Write data:");
        fgets(str,100,stdin);
        printf("data written in memory:%s\n",str);
        shmdt(str);
}
```
### Reader :
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
int main(){
        key_t key=ftok("shmfile",65);
        int shmid=shmget(key,1024,0666);
        char *str=(char *)shmat(shmid,NULL,0);
        printf("Data read from memory:%s\n",str);
        shmdt(str);
}
```

## 14.Write a C program to create a shared memory segment and synchronize access using semaphores.
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<string.h>
#include<sys/sem.h>
#include<unistd.h>
int main(){
        key_t key=ftok("shmfile",65);
        int shmid=shmget(key,1024,0666|IPC_CREAT);
        char *data =(char *)shmat(shmid,NULL,0);
        int semid=shmget(key,1,0666|IPC_CREAT);
        semctl(semid,0,SETVAL,1);
        struct sembuf lock;
        lock.sem_num=0;
        lock.sem_op=-1;
        lock.sem_flg=0;
        semop(semid,&lock,1);
        printf("Enter some text:");
        fgets(data,100,stdin);
        printf("Data written in shared memory: %s\n",data);
        lock.sem_op=1;
        semop(semid,&lock,1);
        shmdt(data);
}
```

## 15.Implement a C program to simulate page replacement algorithms like FIFO, LRU, and optimal.
```c


