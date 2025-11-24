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

## 6.Write a C program to simulate memory allocation using the best-fit algorithm.
```c
#include<stdio.h>
int main(){
        int memoryblocks[10],process[10];
        int blockcount,processblock;
        int allocation[10];
        printf("Enter number of memory blocks:");
        scanf("%d",&blockcount);
        printf("Enter sizes of memory blocks:");
        for(int i=0;i<blockcount;i++){
                scanf("%d",&memoryblocks[i]);
        }
        printf("Enter the number of process:");
        scanf("%d",&processblock);
        printf("Enter memory required for each process:\n");
        for(int i=0;i<processcount;i++){
                scanf("%d",&process[i]);
                allocation[i]=-1;
        }
        for(int i=0;i<processcount;i++){
                int bestindex=-1;
                for(int j=0;j<blockcount;j++){
                        if(memoryblock[j]>=process[i]){
                                if(bestindex=-1 || memoryblocks[i] < memoryblocks[bestindex])
                                        bestindex=j;
                        }
                }
                if(bestindex != -1){
                        allocation[i]=bestindex;
                        memoryblocks[bestindex] = memoryblocks[bestindex]-process[i];
                }
        }
        printf("\nProcess No.\tProcess Size\tBlock No.\n");
        for(int i=0;i<processcount;i++){
                printf("%d\t%d\t\t",i+1,process[i]);
                if(allocation[i] != -1)
                        printf("%d\n",allocation[i]+1);
                else
                        printf("Not allocation\n");
        }
}
```

## 7.Develop a C program to simulate memory allocation using the worst-fit algorithm.
```c
#include<stdio.h>
int main(){
        int memoryblocks[10],process[10];
        int blockcount,processblock;
        int allocation[10];
        printf("Enter number of memory blocks:");
        scanf("%d",&blockcount);
        printf("Enter sizes of memory blocks:\n");
        for(int i=0;i<blockcount;i++){
                scanf("%d",&memoryblocks[i]);
        }
        printf("Enter number of processes:");
        scanf("%d",&processblock);
        printf("Enter memory required for each process:\n");
        for(int i=0;i<processblock;i++){
                scanf("%d",&process[i]);
                allocation[i]=-1;
        }
        for(int i=0;i<processblock;i++){
                int worstindex=-1;
                for(int j=0;j<blockcount;j++){
                        if(memoryblocks[j]>=process[i]){
                                if(worstindex==-1 || memoryblocks[j]>memoryblocks[worstindex])
                                        worstindex = j;
                        }
                }
                if(worstindex != -1){
                        allocation[i]=worstindex;
                        memoryblocks[worstindex] -= process[i];
                }
        }
        printf("\nProcess No.\tProcess Size\tBlock No.\n");
        for(int i=0;i<processblock;i++){
                printf("%d\t\t%d\t\t",i+1,process[i]);
                if(allocation[i]!=-1)
                        printf("%d\n",allocation[i] +1);
                else
                        printf("Not Allocated\n");
        }
}
```

## 8.Implement a C program to simulate memory allocation using the next-fit algorithm.
```c
#include<stdio.h>
int main(){
        int memoryblocks[10],process[10];
        int blockcount,processcount;
        int allocation[10];
        printf("Enter the number of memory blocks:");
        scanf("%d",&blockcount);
        printf("Enter the sizes in the memory blocks:");
        for(int i=0;i<blockcount;i++){
                scanf("%d",&memoryblocks[i]);
        }
        printf("Enter the number of process:");
        scanf("%d",&processcount);
        printf("Enter the memory required for each process:");
        for(int i=0;i<processcount;i++){
                scanf("%d",&process[i]);
                allocation[i]=-1;
        }
        int lastallocated=0;
        for(int i=0;i<processcount;i++){
                int j=lastallocated;
                int count=0;
                while(count<blockcount){
                        if(memoryblocks[j]>=process[i]){
                                allocation[i]=j;
                                memoryblocks[j] -= process[i];
                                lastallocated=j;
                                break;
                        }
                        j=(j+1)%blockcount;
                        count++;
                }
        }
        printf("\nProcess No.\tProcess Size\tBlock No.\n");
        for(int i=0;i<processcount;i++){
                printf("%d\t\t%d\t\t",i+1,process[i]);
                if(allocation[i] != -1)
                        printf("%d\n",allocation[i]+1);
                else
                        printf("Not allocated\n");
        }
}
```

## 9.Write a C program to implement a simple memory allocator using the buddy system.
```c
#include<stdio.h>
#include<math.h>
#define max 1024
int nextpoweroftwo(int n){
        int power=1;
        while(power<n)
                power *= 2;
        return power;
}
int main(){
        int totalmemory=max;
        int allocated[10]={0};
        int processsize,processcount;
        printf("Total available memory : %d KB\n ",totalmemory);
        printf("Enter number of processes:");
        scanf("%d",&processcount);
        for(int i=0;i<processcount;i++){
                printf("\nEnter memory required for process %d (in KB): ",i+1);
                scanf("%d",&processsize);
                if(processsize > totalmemory){
                        printf("Not enough memory to allocate for process %d\n",i+1);
                        continue;
                }
                int blocksize=nextpoweroftwo(processsize);
                if(blocksize > totalmemory){
                        printf("Cannot allocate %d KB for process %d\n",processsize,i+1);
                        continue;
                }
                allocated[i]=blocksize;
                totalmemory -= blocksize;
                printf("process %d allocated %d KB block(buddy size)\n",i+1,blocksize);
                printf("Remaining memory : %d KB\n",totalmemory);
        }
        printf("\n Now freeing all allocated memory..\n");
        for(int i=0;i<processcount;i++){
                if(allocated[i] != 0){
                        totalmemory += allocated[i];
                        printf("Freed %d KB from process %d.Total memory:%d KB\n",allocated[i],i+1,totalmemory);
                }
        }
        printf("\nAll memory is free again: %d KB\n",totalmemory);
}
```

## 10.Develop a C program to implement a memory allocator using a custom memory management algorithm.
```c
#include<stdio.h>
#include<stdlib.h>
#define size 100
int memory[size];
void allocate(int start,int length){
        for(int i=start;i<start+length;i++){
                memory[i]=1;
        }
}
void freememory(int start,int length){
        for(int i=start;i<start+length;i++){
                memory[i]=0;
        }
}
void display(){
        for(int i=0;i<size;i++)
                printf("%d ",memory[i]);
        printf("\n");
}
int main(){
        for(int i=0;i<size;i++){
                memory[i]=0;
        }
        printf("Initial Memory (0=free, 1=used):\n");
        display();
        allocate(10,50);
        printf("\nAfter allocating 50 bytes from index 10 :\n");
        display();
        freememory(20,20);
        printf("\nAfter freeing 20 bytes from index 20 :\n");
        display();
}
```

## 11.Write a C program to demonstrate memory mapping using mmap().
```c
#include<stdio.h>
#include<stdlib.h>
#include<sys/mman.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<unistd.h>
#include<string.h>
int main(){
        int fd;
        char *map;
        fd=open("mmap.txt",O_RDWR|O_CREAT,0666);
        if(fd<0){
                perror("open");
                return 1;
        }
        ftruncate(fd,100);
        map=mmap(NULL,100,PROT_READ|PROT_WRITE,MAP_SHARED,fd,0);
        if(map==MAP_FAILED){
                perror("mmap");
                close(fd);
                return 1;
        }
        strcpy(map,"Hello! This text is written usingg mmap().");
        printf("Data written using mmap:%s\n",map);
        munmap(map,100);
        close(fd);
}
```

## 12.Implement a C program to read from and write to a memory-mapped file.
```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<sys/mman.h>
#include<sys/stat.h>
#include<unistd.h>
#include<string.h>
int main(){
        int fd;
        char *map;
        fd=open("map.txt",O_RDWR|O_CREAT,0666);
        if(fd<0){
                printf("Errorin opening file");
                exit(1);
        }
        ftruncate(fd,100);
        map=mmap(NULL,100,PROT_READ|PROT_WRITE,MAP_SHARED,fd,0);
        if(map==MAP_FAILED){
                perror("mmap");
                close(fd);
                exit(1);
        }
        strcpy(map,"Writing data into a memory-mapped file.");
        printf("Reading from mapped file:\n%s\n",map);
        munmap(map,100);
        close(fd);
}
```

## 13.Develop a C program to demonstrate shared memory usage using shmget() and shmat().
### Writer :
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<string.h>
int main(){
        key_t key = ftok("shmfile",65);
        int shmid=shmget(key,1024,0666|IPC_CREAT);
        char *str = (char*) shmat(shmid,NULL,0);
        printf("Write data:");
        fgets(str,100,stdin);
        printf("data written in memory:%s\n",str);
        shmdt(str);
}
```
### Reader :
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
int main(){
        key_t key=ftok("shmfile",65);
        int shmid=shmget(key,1024,0666);
        char *str=(char *)shmat(shmid,NULL,0);
        printf("Data read from memory:%s\n",str);
        shmdt(str);
}
```

## 14.Write a C program to create a shared memory segment and synchronize access using semaphores.
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<string.h>
#include<sys/sem.h>
#include<unistd.h>
int main(){
        key_t key=ftok("shmfile",65);
        int shmid=shmget(key,1024,0666|IPC_CREAT);
        char *data =(char *)shmat(shmid,NULL,0);
        int semid=shmget(key,1,0666|IPC_CREAT);
        semctl(semid,0,SETVAL,1);
        struct sembuf lock;
        lock.sem_num=0;
        lock.sem_op=-1;
        lock.sem_flg=0;
        semop(semid,&lock,1);
        printf("Enter some text:");
        fgets(data,100,stdin);
        printf("Data written in shared memory: %s\n",data);
        lock.sem_op=1;
        semop(semid,&lock,1);
        shmdt(data);
}
```



# Theory :

## 1. What is memory management in system programming?
Memory management is the process of controlling and coordinating computer memory.
It involves:

Allocating memory to programs

Keeping track of memory usage

Ensuring programs don’t interfere with each other

Reclaiming memory when not needed

✔ It ensures efficient, safe, and error-free memory usage.

## 2. Define virtual memory.
Virtual memory is a memory management technique that gives the illusion of having more memory than physically available.

## 3. Differentiate between physical memory and virtual memory.
| Physical Memory (RAM)    | Virtual Memory                  |
| ------------------------ | ------------------------------- |
| Actual hardware memory   | Logical memory created by OS    |
| Limited in size          | Larger than RAM (RAM + swap)    |
| Fast                     | Slower (because uses disk)      |
| Directly accessed by CPU | Accessed through memory mapping |
| No swapping              | Uses swapping/paging            |

✔ Physical = real

✔ Virtual = OS-created illusion


## 4. What is the role of an operating system in memory management?
The OS performs:

Memory allocation to processes

Memory deallocation when processes finish

Managing virtual memory

Paging and segmentation

Tracking free and used memory

Preventing memory overflow / segmentation faults

Ensuring isolation (one process cannot access another process's memory)

✔ OS ensures correct, safe, and efficient memory usage.

## 5. Explain the purpose of memory allocation.
Memory allocation means assigning memory to a program when it needs it.

Purpose:

Allow program to store data & instructions

Ensure each process gets required memory

Avoid memory conflicts

Improve CPU & program performance

✔ It is like giving a workspace to each program.

## 6. Describe the significance of memory deallocation.
Memory deallocation means releasing memory back to the OS when the program no longer needs it.

Significance:

Prevents memory leaks

Makes memory available for other programs

Improves system performance

Avoids system crash due to memory shortage

✔ Without deallocation → memory filling → system slowdown / crash.

## 7. Define fragmentation in memory management.
Fragmentation occurs when memory is broken into small pieces and cannot be used effectively.

Even though free memory exists → it is unusable due to fragmentation.

✔ Fragmentation reduces usable memory.

## 8. What are the types of fragmentation?
Two main types:

1.Internal Fragmentation

2.External Fragmentation

## 9. Explain internal fragmentation.
Internal fragmentation occurs when:

A memory block is allocated bigger than required

Unused memory inside the block is wasted

## 10. Explain external fragmentation
External fragmentation occurs when:

Total free memory is enough but not continuous

Memory is broken into scattered small holes

## 11. How is fragmentation managed in memory allocation?
Fragmentation is managed using the following techniques:

✔ 1. Compaction

OS rearranges memory to make large continuous blocks

Used to handle external fragmentation

✔ 2. Paging

Breaks memory into fixed-sized pages → avoids external fragmentation

✔ 3. Segmentation + Paging

Uses segments but divides them into pages → reduces fragmentation

✔ 4. Best-fit / Worst-fit / First-fit

Improves memory usage but does not fully remove fragmentation

✔ 5. Dynamic storage allocation

Using variable-sized memory allocation strategies

## 12. Describe the concept of paging.
Paging is a memory management technique that:

Divides virtual memory into equal-sized blocks called pages

Divides physical memory into equal-sized blocks called frames

A page is loaded into any free frame (no need for contiguous memory)

## 13. Explain segmentation.
Segmentation divides memory based on logical divisions of a program, such as:
  - Code segment
  - Data segment
  - Stack
  - Heap

## 14. What is the difference between paging and segmentation?
| Paging                                   | Segmentation                                               |
| ---------------------------------------- | ---------------------------------------------------------- |
| Divides memory into **fixed-size** pages | Divides memory into **variable-size** segments             |
| Based on **physical memory** needs       | Based on **logical program structure** (code, data, stack) |
| No external fragmentation                | Can suffer from external fragmentation                     |
| Page table is needed                     | Segment table is needed                                    |
| Eliminates external fragmentation        | Eliminates internal fragmentation                          |

## 15. Define page table.
A page table is a data structure maintained by the OS that stores mappings between:
Virtual Page Number (VPN) → Physical Frame Number (PFN)

## 16. Define Memory Management Unit (MMU).
The Memory Management Unit (MMU) is a hardware device between the CPU and memory that:

Translates virtual addresses → physical addresses

Uses page tables and TLB

Enforces memory protection

## 17. Explain the role of MMU in memory management.
### 1.Address Translation
Converts virtual address to physical address

### 2.Protection
Prevents processes from accessing memory that is not theirs

### 3.Paging Support
Uses page tables and TLB for fast lookup

### 4.Segmentation Support
Uses segment tables if segmentation is used

### 5.Handling page faults
When a page is not in memory, MMU raises an interrupt

## 18. Describe the translation lookaside buffer (TLB).
TLB is a cache inside the MMU that stores:

Recently used virtual-to-physical address translations

## 19. What is TLB miss? How is it handled?
A TLB miss occurs when:
  - The MMU looks for a virtual address in the TLB
  - The address is not found

Handling TLB Miss:

1.MMU checks the page table

2.If page is present → MMU loads translation into TLB

3.If page is not present in memory → page fault occurs

4.OS loads the page from disk to RAM

5.Update page table

6.Update TLB

7.Restart instruction

## 20. Discuss the working principle of MMU.
MMU Working Principle (Step-by-Step):

1️⃣ CPU generates a virtual address
→ Contains virtual page number + offset

2️⃣ MMU checks TLB

If entry found → gives physical frame number

If not found → TLB miss → check page table

3️⃣ Page Table Lookup

MMU fetches mapping from page table

4️⃣ Check if page is in memory

If not → page fault → OS loads page

5️⃣ Translate Address

Physical Address = Frame Number + Offset


6️⃣ Send physical address to RAM

Summary:

MMU = hardware engine that performs virtual-to-physical translation quickly using TLB and page tables.

## 21. Explain the concept of address translation in MMU.
Address translation is the process of converting a virtual address generated by the CPU into a physical address used by RAM.

The MMU (Memory Management Unit) performs:

1.CPU generates a virtual address

2.MMU checks the page table

3.Page table maps virtual page number → physical frame number

4.Physical address is formed = Physical Frame Number + Offset

Formula:

Virtual Address = Virtual Page Number + Offset  
Physical Address = Physical Frame Number + Offset

## 22. How does MMU support virtual memory?
MMU supports virtual memory through:

⭐ 1. Address Translation

Maps virtual addresses to physical memory.

⭐ 2. Paging

Splits memory into pages (virtual) and frames (physical).

⭐ 3. Page Tables

Stores mapping between virtual pages and physical frames.

⭐ 4. Handling Page Faults

If page not present in RAM, MMU triggers a page fault → OS loads page from disk.

⭐ 5. Protection

Each page has read/write/execute permissions.

## 23. Describe the process of page table traversal in MMU.
steps:

1.CPU sends virtual address to MMU.

2.MMU extracts:

   - VPN (Virtual Page Number)
   - Offset

3.MMU uses VPN to index into the page table.

4.Page table entry (PTE) contains:
    - Valid bit (page present or not)
    - Physical Frame Number (PFN)
    - Protection bits
    - Dirty bit, Accessed bit

5.If valid bit = 1 → MMU creates physical address.

6.If valid bit = 0 → page fault.

## 24. What is page fault handling in MMU?
A page fault occurs when the virtual page is not in RAM.

Steps in Page Fault Handling:

1.CPU generates virtual address → MMU doesn’t find page in RAM.

2.MMU sends page fault interrupt to OS.

3.OS:
   - Finds free frame in RAM
   - If no free frame → performs page replacement

4.OS loads the required page from disk to RAM.

5.Page table is updated (valid bit = 1).

6.Instruction is restarted.

## 25. Explain the page replacement algorithms used in MMU.
When RAM is full and a new page must be loaded, OS must remove an old page.

Page replacement algorithms decide which page to replace.

Common algorithms:

1.FIFO (First In First Out)

2.Optimal Page Replacement

3.LRU (Least Recently Used)

4.LFU (Least Frequently Used)

5.Clock / Second Chance

## 26. Define page replacement algorithms.
A page replacement algorithm decides which memory page to evict when a page fault occurs and RAM has no free space.

## 27. Describe the FIFO page replacement algorithm.
FIFO = First In, First Out

Meaning:
The page that entered the memory first is removed first, regardless of usage.

How it works:
  - Maintain a queue of pages.
  - When a new page arrives:
     - If memory is full → remove the oldest page
     - Insert the new page at the back

## 28. Discuss the optimal page replacement algorithm.
Optimal (OPT) algorithm replaces the page that will not be used for the longest time in the future.

How it works

- Look at the future page references
- Replace the page whose next use is farthest away

## 29. Explain the LRU (Least Recently Used) page replacement algorithm.
LRU removes the page that has not been used for the longest time in the past.

How it works

- Track page usage time
- The page with the oldest last-used timestamp is replaced

## 30. What is the clock page replacement algorithm?
Clock algorithm is a circular buffer version of the Second-Chance algorithm.

Working:

- Frames are arranged in a circle like a clock.
- Each frame has a use/reference bit (R bit).
- A clock hand moves around the circle.
- When page replacement is needed:
   - If R = 0 → replace this page.
   - If R = 1 → set R = 0 and move the clock hand to next frame.
- Continue until a frame with R = 0 is found.

It approximates LRU but with less overhead.

## 31. Discuss the advantages and disadvantages of each page replacement algorithm.
| Algorithm                 | Advantages                                | Disadvantages                                             |
| ------------------------- | ----------------------------------------- | --------------------------------------------------------- |
| **FIFO**                  | Simple, easy to implement                 | Suffers from Belady’s anomaly; may remove important pages |
| **Optimal**               | Minimum page faults                       | Not possible in real systems (needs future knowledge)     |
| **LRU**                   | Good performance, close to optimal        | Needs hardware support; expensive to implement            |
| **Clock / Second Chance** | Efficient, low overhead, approximates LRU | Slightly worse than true LRU                              |
| **NRU**                   | Simple, uses reference and dirty bits     | Doesn’t always choose the best victim                     |


## 32. Compare and contrast different page replacement algorithms.
- FIFO → oldest page; simple but poor accuracy.
- Optimal → theoretical best; cannot be implemented.
- LRU → replaces least recently used; good but expensive.
- Clock → practical approximation of LRU.
- NRU → replaces pages based on R and D bits.

LRU > Clock > FIFO in performance.

Optimal > all (theoretical).

## 33. Explain the working of the NRU (Not Recently Used) page replacement algorithm.
NRU divides pages into four classes using two bits:

R bit (Referenced)

D bit (Dirty)

Classes:

0.R=0, D=0 → not referenced, clean (best candidate)

1.R=0, D=1 → clean but modified

2.R=1, D=0 → referenced recently

3.R=1, D=1 → referenced & dirty (worst candidate)

Working:

OS periodically clears the R bit.

When replacement required:

  1.Choose a page from lowest non-empty class.
  2.Prefer class 0 first.

NRU is simple but not as accurate as LRU.

## 34. Describe the working of the Second Chance page replacement algorithm.
This is an improved version of FIFO.

Working:

- Each page has R bit.
- Examine the oldest page:
   - If R = 0 → replace it.
   - If R = 1 → give “second chance,” set R=0, move it to end of queue.
- Continue until a page with R=0 is found.

This is also called “Clock algorithm without circular buffer.”

## 35. Discuss the enhancements to basic page replacement algorithms.
1.Dirty bit usage (prefer replacing clean pages)

2.Reference bit + history (aging algorithm)

3.Clock algorithm (efficient LRU approximation)

4.N-Chance algorithms (give multiple chances before removal)

5.Working set model (pages of current locality)

These enhancements balance performance vs overhead.

## 36. Define segmentation in memory management.
Segmentation divides a program into logically meaningful variable-sized blocks:

- Code segment
- Data segment
- Stack segment
- Heap segment

## 37. Explain the benefits of segmentation.
✔ Matches programmer’s logical view (functions, modules)
✔ Supports protection per segment
✔ Supports sharing (shared libraries in a segment)
✔ No internal fragmentation (since variable-sized)

## 38. What are the disadvantages of segmentation?
✖ Causes external fragmentation
✖ Complex memory allocation
✖ Requires compaction sometimes
✖ Slower than paging due to variable sizes

## 39. Describe the implementation of segmentation.
The OS maintains a Segment Table.

Each entry contains:
- Base address (start of segment in memory)
- Limit (size of segment)
- Protection bits

MMU translates logical address:

1.Check if offset < limit

2.Physical address = base + offset

Segmentation can be combined with paging → segment + page model.

## 40. Discuss segmentation fault and its causes.
A segmentation fault (segfault) occurs when a program tries to access invalid memory.

Causes:

- Accessing memory outside the segment limit
- Null pointer dereference
- Buffer overflow
- Writing to read-only segments (code segment)
- Stack overflow
- Using wild pointers or dangling pointers

Segfault is raised as a hardware exception and kills the process.

