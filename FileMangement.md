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
## 3.Implement a C program to create a new directory named "Test" in the current directory?
```c
#include<stdio.h>
#include<unistd.h>
int main(){
        char *argv[]={"mkdir","Test",NULL};
        execv("/bin/mkdir",argv);
        printf("execv error");
        return 1;
}
```
## 7.Write a C program to copy the contents of one file to another?
```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int x;
        char buf[100];
        int srcfd,desfd;
        srcfd=open("file1.txt",O_RDONLY);
        if(srcfd<0){
                perror("Error");
                return 1;
        }
        desfd=open("file2.txt",O_WRONLY|O_CREAT|O_TRUNC,0666);
        if(desfd==-1){
                perror("error");
                close(srcfd);
                return 1;
        }
       while((x=read(srcfd,buf,100))>0){
                if(write(desfd,buf,x)!=x){
                        perror("Error writing to file2.txt");
                        close(srcfd);
                        close(desfd);
                        return 1;
                }
        }
        if(x<0){
                perror("Error reading from file1.txt");
        }
        close(srcfd);
        close(desfd);
        return 0;

}
```
