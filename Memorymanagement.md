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
