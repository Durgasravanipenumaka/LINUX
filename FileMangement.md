## 1.Write a C program to create a new text file and write "Hello, World!" to it?
```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd;
        fd=open("hello.txt",O_WRONLY|O_RDONLY|O_CREAT,0666);
        if(fd<0){
                perror("Error");
                return 1;
        }
        write(fd,"Hello World!",strlen("Hello World!"));
}
```
## 2.Develop a C program to open an existing text file and display its contents?
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        char buf[20];
        int fd;
        int ret;
        fd=open("hello.txt",O_RDONLY);
        if(fd<0){
                perror("Error");
                return 1;
        }
        ret=read(fd,buf,20);
        if(ret<0){
                printf("Failed to read the file");
                close(fd);
                return 1;
        }
        buf[ret]='\0';
        printf("%s",buf);
}
```
