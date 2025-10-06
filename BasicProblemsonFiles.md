## 1.Write a C program to create a new file and write “Hello Linux” into it.
```c



## 5.Write a C program to check whether a file exists or not.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd;
        fd=open("file.txt",O_RDONLY);
        if(fd>=0)
                printf("File Exists\n");
        else
                printf("File doesnot Exists\n");
}
```
