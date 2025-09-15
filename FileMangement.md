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
## 4.Write a C program to check if a file named "sample.txt" exists in the current directory?
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd;
        fd=open("Sample.txt",O_RDONLY,0644);
        if(fd==-1){
                perror("Sample.txt doesnot exit in the current directory\n");
        }
        else{
                printf("Sample.txt exits in the current directory\n");
                close(fd);
        }
}
```
## 5.Develop a C program to rename a file from "oldname.txt" to "newname.txt"?
```c
#include<stdio.h>
#include<stdlib.h>
int main(){
        if(rename("sam.txt","sample.txt")==0){
                printf("File name updated successfully\n");
        }
        else{
                perror("Error in renaming file");
                exit(1);
        }
}
```
## 6.Implement a C program to delete a file named "delete_me.txt"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        if(unlink("sample.txt")==0)
                printf("Deleting of file is done successfully\n");
        else{
                perror("Error in deleting file\n");
                exit(1);
        }
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
## 8.Develop a C program to move a file from one directory to another?
```c
```
## 9.Implement a C program to list all files in the current directory?
```c
#include<stdio.h>
#include<unistd.h>
#include<dirent.h>
#include<stdlib.h>
int main(){
        DIR *d;
        struct dirent *dir;
        d=opendir(".");
        if(d==NULL){
                perror("Unable to stop");
                exit(1);
        }
        printf("Files in current directory:\n");
        while((dir=readdir(d))!=NULL){
                printf("%s\n",dir->d_name);
        }
        closedir(d);
}
```

## 10.Write a C program to get the size of a file named "file.txt"?
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<fcntl.h>
int main(){
        int fd;
        fd=open("file1.txt",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        int size=lseek(fd,0,SEEK_END);
        if(size==-1){
                printf("Error");
                close(fd);
                exit(1);
        }
        printf("Size of file1.txt :%d",size);
        close(fd);
}
```

## 11.Develop a C program to check if a directory named "Test" exists in the current directory?
```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<dirent.h>
int main(){
        const char *dirname="Test";
        DIR *dir;
        dir=opendir(dirname);
        if(dir){
                printf("Directory %s exists in the current directory\n",dirname);
                closedir(dir);
        }
        else{
                printf("Directory '%s' does not exist.\n", dirname);
        }
}
```

## 12.Implement a C program to create a new directory named "Backup" in the parent directory?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        if(execlp("mkdir","mkdir","-p","Backup",NULL)==-1){
                perror("Error");
                exit(1);
        }
}
```

## 13.Write a C program to recursively list all files and directories in a given directory?
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<dirent.h>
void listfilesrecursively(const char *basepath,int depth){
        struct dirent *dp;
        DIR *dir=opendir(basepath);
        if(!dir){
                printf("Error");
                exit(1);
        }
        while((dp=readdir(dir))!=NULL){
                if((strcmp(dp->d_name,".")==0)||(strcmp(dp->d_name,"..")==0)){
                        continue;
                }
                for(int i=0;i<depth;i++){
                        printf(" ");
                }
                printf("%s\n",dp->d_name);
                char path[100];
                snprintf(path,sizeof(path),"%s/%s",basepath,dp->d_name);
                struct stat statbuf;
                if(stat(path,&statbuf)==0 && S_ISDIR(statbuf.st_mode)){
                        listfilesrecursively(path,depth+1);
                }
        }
        closedir(dir);
}
```

## 14.Develop a C program to delete all files in a directory named "Temp"?
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<dirent.h>
#include<unistd.h>
int main(){
        const char *dirname="Test";
        struct dirent *entry;
        DIR *dir;
        dir=opendir(dirname);
        if(dir==NULL){
                printf("Error");
                exit(1);
        }
        while((entry=readdir(dir))!=NULL){
                char filepath[100];
                if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
                        continue;
                snprintf(filepath,sizeof(filepath),"%s/%s",dirname,entry->d_name);
                if(unlink(filepath)==0)
                        printf("Deleted:%s\n",filepath);
                else
                        perror("Error\n");
        }
        closedir(dir);
}
```

