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

## 
