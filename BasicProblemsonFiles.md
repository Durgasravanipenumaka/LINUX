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
## 3.Write a program using system calls (open, read, write) to copy one file to another.
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
        des=open("files.txt",O_WRONLY|O_CREAT|O_TRUNC,0666);
        if(des<0){
                printf("Error");
                exit(1);
        }
        while((bytes=read(src,str,sizeof(str)))>0){
                str[bytes]='\0';
                if(write(des,str,bytes)!=bytes){
                        printf("Error");
                        close(src);
                        close(des);
                        exit(1);
                }
        }
        close(src);
        close(des);
        printf("File copied successfully\n");
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
## 6.Write a program to append text to an existing file.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
#include<fcntl.h>
int main(){
        int fd;
        char str[100];
        fd=open("filee.txt",O_WRONLY|O_APPEND|O_CREAT,0666);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        printf("Enter the string:");
        fgets(str,sizeof(str),stdin);
        str[strcspn(str,"\n")]='\0';
        write(fd,str,strlen(str));
        close(fd);
}
```
## 7.Write a program to read a file character by character using getc().
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<fcntl.h>
#include<unistd.h>
int main(){
        FILE *fp;
        int fd;
        char ch;
        fd=open("filee.txt",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        fp=fdopen(fd,"r");
        if(fp<0){
                printf("Error");
                exit(1);
        }
        while((ch=getc(fp))!=EOF){
                printf("%c",ch);
        }
        close(fd);
}
```
## 8.Write a program to read a file line by line using fgets().
```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<string.h>
#include<unistd.h>
int main(){ 
        int fd;
        FILE *fp;
        char str[100];
        fd=open("Images.txt",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        fp=fdopen(fd,"r");
        if(fp==NULL){
                printf("Error");
                exit(1);
        }
        while((fgets(str,sizeof(str),fp))!=NULL){
                printf("%s",str);
        }
        fclose(fp);
}
```
## 9.Write a program to count how many times a particular word occurs in a file.
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<fcntl.h>
#include<unistd.h>
int main(){
        int fd;
        char str[100];
        int bytes;
        char word[50];
        int count=0;
        printf("Enter the word to search:");
        scanf("%s",word);
        fd=open("Images.txt",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        while((bytes=read(fd,str,sizeof(str)))>0){
                str[bytes]='\0';
                char *token=strtok(str," ,.;:\n\t?!");
                while(token!=NULL){
                        if(strcmp(word,token)==0)
                                count++;
                        token=strtok(NULL," ,.;:\n\t?!");
                }
        }
        if(bytes<0){
                printf("Error");
                close(fd);
                exit(1);
        }
        close(fd);
        printf("%s repeates %d times\n",word,count);
}
```
## 10.Write a program to find the size of a file in bytes.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd;
        int size;
        char str[100];
        fd=open("Images.txt",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        size=lseek(fd,0,SEEK_END);
        printf("Size of the file:%d\n",size);
        close(fd);
}
```
## 11.Write a program to change file permissions using chmod() system call.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<sys/stat.h>
int main(){
        int mode;
        printf("Enter new permissions in octal form:");
        scanf("%o",&mode);
        if(chmod("Images.txt",mode)<0){
                printf("Error");
                exit(1);
        }
        printf("File permissions for Images.txt changed successfully\n");
}
```
## 12.Write a program to display file information (size, owner, permissions, etc.) using stat() system call.
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<pwd.h>
#include<grp.h>
#include<time.h>
void permissions(mode_t mode){
        printf("Permissions:");
        printf((mode & S_IRUSR) ? "r" : "-");
        printf((mode & S_IWUSR) ? "w" : "-");
        printf((mode & S_IXUSR) ? "x" : "-");
        printf((mode & S_IRUSR) ? "r" : "-");
        printf((mode & S_IWUSR) ? "w" : "-");
        printf((mode & S_IXUSR) ? "x" : "-");
        printf((mode & S_IROTH) ? "r" : "-");
        printf((mode & S_IWOTH) ? "w" : "-");
        printf((mode & S_IXOTH) ? "x" : "-");
        printf("\n");
}
int main(){
        char filename[100];
        struct stat filestat;
        printf("Enter filename:");
        scanf("%s",filename);
        if(stat(filename,&filestat)<0){
                printf("Stat");
                exit(1);
        }
        printf("File information for %s:\n",filename);
        printf("size:%ld bytes\n",filestat.st_size);
        printf("Number of Links:%ld\n",filestat.st_nlink);
        printf("Owner UID:%d\n",filestat.st_uid);
        printf("Owner Name:%s\n",getpwuid(filestat.st_uid)->pw_name);
        printf("Group GID:%d\n",filestat.st_gid);
        printf("Group Name:%s\n",getgrgid(filestat.st_gid)->gr_name);
        permissions(filestat.st_mode);
        printf("Last Access time:%s",ctime(&filestat.st_atime));
        printf("Last Modification time:%s",ctime(&filestat.st_mtime));
        printf("Last status changed time%s",ctime(&filestat.st_ctime));
}
```
