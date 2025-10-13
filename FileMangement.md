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
include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
int main(){
        const char *src="/Documents/Linux/files/file1.txt";
        const char *des="/Documents/Linux/processmanagement/file1.txt";
        if(rename(src,des)==0){
                printf("file moved successfully from %s to %s",src,des);
        }
        else{
                printf("Error");
                exit(1);
        }
}
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

## 15.Implement a C program to count the number of lines in a file named "data.txt"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd;
        char buffer[1024];
        int size,count=0;
        fd=open("file1.c",O_RDONLY,0644);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        while((size=read(fd,buffer,sizeof(buffer)))>0){
                for(int i=0;i<size;i++){
                        if(buffer[i]=='\n'){
                                count++;
                        }
                }
        }
        if(size<0){
                printf("Error");
                close(fd);
                exit(1);
        }
        close(fd);
        printf("No of Lines:%d",count);
}
```

## 16.Write a C program to append "Goodbye!" to the end of an existing file named "message.txt"?
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd;
        fd=open("message.txt",O_WRONLY|O_APPEND,0666);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        if(write(fd,"Good Bye",sizeof("Good Bye")-1)<0){
                printf("Error");
                close(fd);
                exit(1);
        }
        close(fd);
}
```

## 17.Implement a C program to change the permissions of a file named "file.txt" to readonly?
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<fcntl.h>
#include<sys/stat.h>
int main(){
        if(chmod("file1.txt",S_IRUSR|S_IRGRP|S_IROTH)==0){
                printf("Permissions are changed\n");
        }
        else{
                printf("Error");
                exit(1);
        }
}
```

## 18.Write a C program to change the ownership of a file named "file.txt" to the user "user1"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<fcntl.h>
int main(){
        struct passwd *pwd=getpwnam("user1");
        if(pwd==NULL){
                printf("Not found");
                exit(1);
        }
        uid_t uid = pwd->pw_uid;
        if(chown("file.txt",uid,-1)==0)
                printf("Ownership changed\n");
        else
                printf("Not changed\n");
}
```

## 19.Develop a C program to get the last modified timestamp of a file named "file.txt"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
#include<sys/stat.h>
#include<time.h>
int main(){
        struct stat filestat;
        if(stat("file.txt",&filestat)<0){
                printf("Error");
                exit(1);
        }
        printf("Last modified time of %s : %s ","file.txt",ctime(&filestat.st_mtime));
}
```

## 20.Implement a C program to create a temporary file and write some data to it?
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<fcntl.h>
int main(){
        int fd;
        fd=open("file.txt",O_WRONLY|O_CREAT,0666);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        char ch[]="Hello world";
        if(write(fd,ch,sizeof(ch))<0){
                printf("Error in writing to file");
                exit(1);
        }
        close(fd);
}
```

## 21.Write a C program to check if a given path refers to a file or a directory?
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<sys/stat.h>
int main(){
        char path[100];
        struct stat pathstat;
        printf("Enter the path:");
        scanf("%s",path);
        if(stat(path,&pathstat)!=0){
                printf("error");
                exit(1);
        }
        if(S_ISREG(pathstat.st_mode)){
                printf("%s is a regular file",path);
        }
        else if(S_ISDIR(pathstat.st_mode)){
                printf("%s is a directory",path);
        }
        else{
                printf("%s is neither a file nor a directory",path);
        }
}
```

## 22.Develop a C program to create a hard link named "hardlink.txt" to a file named "source.txt"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        if(link("source.txt","Hardlink.txt")==0){
                printf("Hardlink Hardlink.txt is created successfully pointing to source.txt\n");
        }
        else{
                perror("Error");
                exit(1);
        }
}
```
## 23.Implement a C program to read and display the contents of a CSV file named "data.csv"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd,len;
        char str[100];
        fd=open("data.csv",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        while((len=read(fd,str,sizeof(str)-1))>0){
                str[len]='\0';
                printf("%s",str);
        }
        close(fd);
}
```

## 24.Write a C program to get the absolute path of the current working directory?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        char cwd[1024];
        if(getcwd(cwd,sizeof(cwd))!=NULL){
                printf("Current working directory:%s",cwd);
        }
        else{
                perror("Error");
                exit(1);
        }
}
```

## 25.Develop a C program to get the size of a directory named "Documents"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
#include<unistd.h>
#include<dirent.h>
#include<string.h>
int getsizeofdir(const char *dirname){
        char path[1024];
        struct stat filestat;
        struct dirent *entry;
        DIR *dir=opendir(dirname);
        long int totalsize=0;
        if(dir==NULL){
                perror("Opened");
                exit(1);
        }
        while((entry=readdir(dir))!=NULL){
                if(strcmp(entry->d_name,".")!=0 && strcmp(entry->d_name,"..")!=0){
                        snprintf(path,sizeof(path),"%s/%s",dirname,entry->d_name);
                        if(stat(path,&filestat)==0)
                                totalsize+=filestat.st_size;
                        else
                                perror("stat");
                }
        }
        closedir(dir);
        return totalsize;
}
int main(){
        const char dirname[]="Documents";
        long int size=getsizeofdir(dirname);
        if(size>=0){
                printf("size of %s = %ld\n",dirname,size);
        }
}
```
## 26.Implement a C program to recursively copy all files and directories from one directory to another?
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
#include<dirent.h>
#include<errno.h>
#include<sys/stat.h>
void copyfiles(const char *src,const char *des){
        int fdin,fdout;
        ssize_t nread;
        char buffer[4096];
        fdin=open(src,O_RDONLY);
        if(fdin<0){
                perror("Open source file");
                exit(1);
        }
        fdout=open(des,O_WRONLY|O_CREAT|O_TRUNC,0644);
        if(fdout<0){
                perror("open destination file");
                close(fdin);
                exit(1);
        }
        while((nread=read(fdin,buffer,sizeof(buffer)))>0){
                if(write(fdout,buffer,nread)!=nread){
                        perror("write");
                        close(fdin);
                        close(fdout);
                }
        }
        if(nread<0)
                perror("read");
        close(fdin);
        close(fdout);
}
void copydirectory(const char *src,const char *des){
        DIR *dir;
        struct dirent *entry;
        struct stat statbuf;
        char srcpath[1024],despath[1024];
        if(mkdir(des,0755)==-1 && errno != EEXIST){
                perror("mkdir");
                exit(1);
        }
        dir=opendir(src);
        if(!dir){
                perror("Opendir");
                exit(1);
        }
        while((entry=readdir(dir))!=NULL){
                if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
                        continue;
                snprintf(srcpath,sizeof(srcpath),"%s/%s",src,entry->d_name);
                snprintf(despath,sizeof(despath),"%s/%s",des,entry->d_name);
                if(stat(srcpath,&statbuf)==-1){
                        perror("staf");
                        exit(1);
                }
                if(S_ISDIR(statbuf.st_mode))
                        copydirectory(srcpath,despath);
                else if(S_ISREG(statbuf.st_mode))
                        copyfiles(srcpath,despath);
        }
        closedir(dir);
}
int main(int argc,char *argv[]){
        if(argc != 3){
                fprintf(stderr,"usage:%s <sourcedir> <destinationdir>\n",argv[0]);
                exit(1);
        }
        copydirectory(argv[1],argv[2]);
        printf("Copied directory from %s to %s\n",argv[1],argv[2]);
}
```

## 27.Write a C program to get the number of files in a directory named "Images"?
```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<dirent.h>
int main(){
        DIR *dir;
        struct dirent *entry;
        int count=0;
        dir=opendir("Images");
        if(dir==0){
                perror("Error");
                exit(1);
        }
        while((entry=readdir(dir))!=NULL){
                if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
                        continue;
                if(entry->d_type == DT_REG)
                        count++;
        }
        closedir(dir);
        printf("Number of files : %d",count);
}
```

## 28.Develop a C program to create a FIFO (named pipe) named "myfifo" in the current directory?
```c
#include<stdio.h>
int main(){
        int arr[]={7,4,2,4,2,1,3};
        int cp[3]={0};
        int count=0;
        for(int i=0;i<7;i++){
                int temp=arr[i];
                int found = 0;
                for(int j=0;j<3;j++){
                        if(cp[j] == temp){
                                found = 1;
                                break;
                        }
                }
                if(!found){
                        cp[count] = temp;
                        count = (count + 1) % 3;
                        for(int k=0;k<3;k++){
                                printf("%d ", cp[k]);
                        }
                        printf("\n");
                }
        }
}
```

## 29.Implement a C program to read data from a FIFO named "myfifo"?
### Writer Program :
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/types.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<string.h>
#include<unistd.h>
int main(){
        const char *fifopath="myfifo";
        char buf[]="Hello";
        if(mkfifo(fifopath,0666)==-1){
                perror("Error creating FIFO");
                exit(1);
        }
        printf("FIFO %s created successfully in the current directory.\n",fifopath);
        int fd;
        fd=open("myfifo",O_WRONLY,0666);
        if(fd==-1){
                perror("Error opening FIFO for writing");
                exit(1);
        }
        write(fd,buf,strlen(buf));
        close(fd);

}
```
### Reader Program :
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<fcntl.h>
int main(){
        int fd;
        char buf[100];
        fd=open("myfifo",O_RDONLY);
        if(fd==-1){
                perror("Error opening FIFO for reading");
                exit(1);
        }
        printf("Waiting for data to be written into 'myfifo'..\n");
        int bytes=read(fd,buf,sizeof(buf)-1);
        if(bytes==-1){
                perror("Error in reading from FIFO");
                close(fd);
                exit(1);
        }
        buf[bytes]='\0';
        printf("Data read from FIFO: %s\n",buf);
        close(fd);
}
```

## 30.Write a C program to truncate a file named "file.txt" to a specified length?
```c
include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
int main(){
        const char filename[]="file.txt";
        long length;
        printf("Enter the length to truncate:");
        if((scanf("%ld",&length))!=1 || length<0){
                perror("Error");
                exit(1);
        }
        if(truncate(filename,length)==-1){
                perror("Truncate failed");
                exit(1);
        }
        printf("File '%s' truncate to %ld bytes successfully",filename,length);
}
```

## 31.Develop a C program to search for a specific string in a file named "data.txt" and display the line number(s) where it occurs?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
#include<fcntl.h>
int main(){
        int fd;
        char searchstr[100];
        fd=open("Images.txt",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        printf("Enter the string to search :");
        fgets(searchstr,sizeof(searchstr),stdin);
        searchstr[strcspn(searchstr,"\n")]='\0';
        char line[1024];
        int lineindex=0;
        int bytesread;
        char buffer[100];
        int found=0;
        int linenumber=1;
        while((bytesread=read(fd,buffer,sizeof(buffer)))>0){
                for(int i=0;i<bytesread;i++){
                        line[lineindex++]=buffer[i];
                        if(buffer[i]=='\n' || lineindex==1023){
                                line[lineindex]='\0';
                                if(strstr(line,searchstr)!=NULL){
                                        printf("Found in line %d : %s",linenumber,line);
                                        found=1;
                                }
                                lineindex=0;
                                linenumber++;
                        }
                }
        }
        if(!found){
                printf("The string %s was not found in the file.\n",searchstr);
        }
        close(fd);
}
```

## 32.Implement a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path?
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
int main(){
        char path[256];
        struct stat path_stat;
        printf("Enter the path:");
        scanf("%s",path);
        if(lstat(path,&path_stat)<0){
                perror("lstat");
                exit(1);
        }
        printf("File type of %s is :",path);
        if(S_ISREG(path_stat.st_mode))
                printf("Regular file\n");
        else if(S_ISDIR(path_stat.st_mode))
                printf("Directory\n");
        else if(S_ISLNK(path_stat.st_mode))
                printf("Symbolic link\n");
        else if(S_ISCHR(path_stat.st_mode))
                printf("Character device\n");
        else if(S_ISBLK(path_stat.st_mode))
                printf("Block device\n");
        else if(S_ISFIFO(path_stat.st_mode))
                printf("FIFO(named pipe\n");
        else if(S_ISSOCK(path_stat.st_mode))
                printf("Socket\n");
        else
                printf("Unknown\n");
}
```

## 33.Write a C program to create a new empty file named "empty.txt"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
int main(){
        int fd;
        fd=open("empty.txt",O_CREAT|O_WRONLY,0666);
        if(fd<0){
                perror("Error");"
                exit(1);
        }
        printf("Empty file created\n");
        close(fd);
}
```

## 34.Develop a C program to get the permissions (mode) of a file named "file.txt"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/stat.h>
int main(){
        struct stat filestat;
        if(stat("Images.txt",&filestat)<0){
                perror("Error ");
                exit(1);
        }
        printf("File permissions for file.txt :\n");
        printf("User: ");
        printf((filestat.st_mode & S_IRUSR) ? "r" : "-");
        printf((filestat.st_mode & S_IWUSR) ? "w" : "-");
        printf((filestat.st_mode & S_IXUSR) ? "x" : "-");
        printf("\n");
        printf("Group: ");
        printf((filestat.st_mode & S_IRGRP) ? "r" : "-");
        printf((filestat.st_mode & S_IWGRP) ? "w" : "-");
        printf((filestat.st_mode & S_IXGRP) ? "x" : "-");
        printf("\n");
        printf("Other: ");
        printf((filestat.st_mode & S_IROTH) ? "r" : "-");
        printf((filestat.st_mode & S_IWOTH) ? "w" : "-");
        printf((filestat.st_mode & S_IXOTH) ? "x" : "-");
        printf("\n");
}
```

## 35.Implement a C program to recursively delete a directory named "Temp" and all its contents?
```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<string.h>
#include<dirent.h>
#include<sys/stat.h>
#include<unistd.h>
void deleteddir(const char *path){
        struct dirent *entry;
        struct stat statbuf;
        char fullpath[512];
        DIR *dir=opendir(path);
        if(!dir){
                perror("opendir");
                return;
        }
        while((entry = readdir(dir)) != NULL){
                if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name,"..")==0)
                        continue;
                snprintf(fullpath,sizeof(fullpath),"%s/%s",path,entry->d_name);
                if(stat(fullpath,&statbuf)==0){
                        if(S_ISDIR(statbuf.st_mode)){
                                deleteddir(fullpath);
                        }
                        else{
                                if(remove(fullpath)==0)
                                        printf("Deleted file:%s\n",fullpath);
                                else
                                        perror("remove");
                        }
                }
        }
        closedir(dir);
        if(rmdir(path)==0)
                printf("Removed directory :%s\n",path);
        else
                perror("rmdir");
}

int main(){
        const char *dirname = "Backup";
        deleteddir(dirname);
        printf("Deletion completed\n");
}
```

## 37.Write a C program to read and display the first 10 lines of a file named "log.txt"?
```c
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#include<fcntl.h>
#include<stdlib.h>
int main(){
        int fd;
        fd=open("log.txt",O_RDONLY);
        if(fd<0){
                printf("Error");
                exit(1);
        }
        int bytes;
        char buf[256];
        char line[256];
        int count=0,k=0;
        while((bytes=read(fd,buf,sizeof(buf)))>0){
                for(int i=0;i<bytes;i++){
                        if(buf[i]=='\n'){
                                line[k]='\0';
                                printf("%s\n",line);
                                k=0;
                                count++;
                                if(count==10){
                                        close(fd);
                                        return 0;
                                }
                        }
                        else{
                                line[k++]=buf[i];
                        }
                }
        }
        if(k>0 && count<10){
                line[k]='\0';
                printf("%s\n",line);
        }
        close(fd);
}
```

## 38.. Develop a C program to read data from a text file named "input.txt" and write it to another file named "output.txt" in reverse order?
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
int main(){
        int fdin,fdout;
        fdin=open("input.txt",O_RDONLY);
        if(fdin<0){
                printf("Error");
                exit(1);
        }
        char str[100];
        int bytes;
        bytes=read(fdin,str,sizeof(str)-1);
        if(bytes<0){
                printf("Error");
                close(fdin);
                exit(1);
        }
        str[bytes]='\0';
        close(fdin);
        for(int i=0,j=strlen(str)-1;i<j;i++,j--){
                int temp=str[i];
                str[i]=str[j];
                str[j]=temp;
        }
        fdout=open("output.txt",O_WRONLY|O_CREAT,0666);
        if(fdout<0){
                printf("Error");
                exit(1);
        }
        if(write(fdout,str,strlen(str))<0){
                printf("error");
                exit(1);
        }
        close(fdout);
        printf("File reversed successfully\n");

}
```
