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
## 3.Write a program to copy the contents of one file into another.
```c
#include<stdio.h>
#include<fcntl.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
int main(){
        int src,des;
        int bytes;
        char str[100];
        src=open("filee.txt",O_RDONLY);
        if(src<0){
                printf("Error");
                exit(1);
        }
        des=open("files.txt",O_WRONLY|O_CREAT,0666);
        if(des<0){
                printf("Error");
                exit(1);
        }
        while((bytes=read(src,str,10))>0){
                str[bytes]='\0';
                write(des,str,strlen(str));
        }
        close(src);
        close(des);
}
```
## 4.Write a program to count the number of characters, words, and lines in a file.
```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
#include<stdlib.h>
int main(){
        int fd,bytes;
        char str[1024];
        fd=open("Images.txt",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        int lines=0;
        int characters=0;
        int words=0,inword=0;
        while((bytes=read(fd,str,sizeof(str)))>0){
                for(int i=0;str[i]!='\0';i++){
                        characters++;
                        if(str[i]=='\n')
                                lines++;
                        if(str[i]==' '||str[i]=='\n'||str[i]=='\t')
                                inword=0;
                        else if(inword==0){
                                words++;
                                inword=1;
                        }
                }
                if(bytes==0){
                        printf("Error");
                        close(fd);
                        exit(1);
                }
        }
        close(fd);
        printf("Number of lines:%d\nNumber of words:%d\nNumber of characters:%d\n",lines,words,characters);
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
