## 1.Write a C program to create a new file and write “Hello Linux” into it.
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<stdlib.h>
#include<string.h>
int main(){
        int fd;
        char str[]="Hello Linux";
        fd=open("filee.txt",O_WRONLY|O_CREAT,0666);
        if(fd<0){
                perror("error");
                exit(1);
        }
        write(fd,str,strlen(str));
}
```
## 2.Write a program to read the contents of a file and display it on the screen.
```c
#include<stdio.h>
#include<string.h>
#include<fcntl.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        int fd;
        int bytes;
        char str[100];
        fd=open("filee.txt",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        while((bytes=read(fd,str,10))>0){
                str[bytes]='\0';
                printf("%s",str);
        }
        close(fd);
}
```


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
