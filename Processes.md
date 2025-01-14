### Definition
In Linux, a **process is any active (running) instance of program**. But what is a program? Well, technically, a program is any executable file held in storage on your machine. Anytime you run a program, you have created a process.
>[!tip] processes
>The **signals** and **processes** contorol almost every task of the system.
### PID
Each process is allocated a unique number, **process identifier (PID)**. It's an integer between 2 and 32,768. When a process is started, the numbers restart from 2, and *the number 1* is typically reserved for the **init** process as shown in the above example. *The process #1 manages other processes.*
### Process State

![[Pasted image 20250113231323.png]]
### Process and memory
When we run a program, the code that will be executed is stored in a disk file. In general, a **linux process can't write to the memory area**. The area is for holding the program code so that the code can be loaded into memory as read-only (so, it can be safely shared). 

![[Pasted image 20250113232050.png]]

The system libraries can also be shared. Therefore, there need be only one copy of `printf()` in memory, every if there are many program calling it.

When we run two programs, there are variables unique to each programs, unlike the shared libraries, these are in separate data space of each process, and usually can't be shared. In other words, a process has its own stack space, used for local variables. It also has its own environment variables which are maintained by each process. A process should also has its own program counter, a record of where it has gotten to in its execution (execution thread)
### Process Table
The **process table** describes all the processes that currently loaded. The `ps` command shows the processes. By default, it shows only processes that maintain a connection with a terminal, a console, a serial line, or a pseudo terminal. Other processes that can run without communication with a user on a terminal are system processes that Linux manages shared resources. To see all processes, we use `-e` option and `-f` to get full information (`ps -ef`)
### System Processes
Here is **STAT** output from `ps`:
```shell
$ ps -ax
  PID TTY      STAT   TIME COMMAND
    1 ?        Ss     1:48 init [3]
    2 ?        S<     0:03 [migration/0]
    3 ?        SN     0:00 [ksoftirqd/0]
 ....
 2981 ?        S<sl  10:14 auditd
 2983 ?        S<sl   3:43 /sbin/audispd
 ....
 3428 ?        SLs    0:00 ntpd -u ntp:ntp -p /var/run/ntpd.pid -g
 3464 ?        Ss     0:00 rpc.rquotad
 3508 ?        S<     0:00 [nfsd4]
 ....
 3812 tty1     Ss+    0:00 /sbin/mingetty tty1
 3813 tty2     Ss+    0:00 /sbin/mingetty tty2
 3814 tty3     Ss+    0:00 /sbin/mingetty tty3
 3815 tty4     Ss+    0:00 /sbin/mingetty tty4
.....
19874 pts/1    R+     0:00 ps -ax
19875 pts/1    S+     0:00 more
21033 ?        Ss     0:39 crond
24765 ?        Ss     0:01 /usr/sbin/httpd
```

The meaning of the code in the table below:

| STAT code | Description                                                                                      |
| --------- | ------------------------------------------------------------------------------------------------ |
| R         | Running or runnable (either exectuting or about to run).                                         |
| D         | Uninterruptible sleep (waiting) - usually waiting for input or output to complete.               |
| S         | Sleeping. Usually waiting for an event to occur, such as a signal or input to become available.  |
| T         | Stopped. Usually stopped by shell job control or the process is under the control of a debugger. |
| Z         | Defunct or zoombie process.                                                                      |
| N         | Low priority task, nice.                                                                         |
| W         | Paging.                                                                                          |
| s         | The process is a session leader.                                                                 |
| +         | The process is in the foreground process group.                                                  |
| \|        | The process is multithreaded.                                                                    |
| <         | High priority task.                                                                              |
Let's look at the following process:
```shell
1 ?        Ss     1:48 init [3]
```
Each **child process** is started by **parent process**. When linux starts, it runs a single program, **init**, with process #1. This is OS process manager, and it's the prime ancestor of all processes. Then, other system processes are started by **init** or by other processes started by **init**. The login procedure is one of the example. The **init** starts the getty program once for each terminal that we can use to log in, and it is shown in the `ps` as below:
```shell
 3812 tty1     Ss+    0:00 /sbin/mingetty tty1
```
The **getty** processes wait for activity at the terminal, prompt the user with the login prompt, and then pass control to the login program, which sets up the user environment, and starts a shell. When the user shell exits, **init** starts another **getty** process.

==The ability to **start new processes** and to **wait for them to finish** is fundamental to the system.== We can do the same thing within our own program with the system calls `fork()`, `exec()`, and `wait()`.

A **system call** is a **controlled entry point into the kernel**, allowing a process to request that the kernel perform some action for the process.

Actually, a system call changes the processor state from *user mode* to *kernel mode*, so that the CPU can access protected kernel memory. The kernel makes a range of services accessible to programs via the system call aplication programming interface (*API*).
### Process Scheduling
Let's look at the `ps` STAT output for `ps -ax` itself:
```shell
23603 pts/1    R+     0:00 ps ax
```
The STAT `R` indicates the process `23603` is in a run state. In other words, it tells its own state. The indicator shows that the program is ready to run, and is not necessarily running. The `R+` indicates that the process is in foreground not waiting for other processes to finish nor waiting for input or output to complete. That's why we may see two such processes listed in `ps` output.

>[!tip] Process Scheduler
>Linux kernel using a **process scheduler** to decide which process will get the next **time slice** based on the **process priority**.

Usually, several programs are competing for the same resources. A program that performs short burst of work and pause for input is considered better behaved than the one that long the processor by continually calculating/querying the system. Well-behaved programs are termed **nice** programs, and in a sense this **niceness** can be measured.
