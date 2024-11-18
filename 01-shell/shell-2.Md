### A  shebang (#!) 
- is the first line in a script file that tells the operating system what interpreter to use to execute the script. 

## ls -ltr
- used to list the contents with detailed info and sorted by modifation time in reverse order

    drwxr-xr-x  2 user group   4096 Nov 16 09:20 directory1
    -rwxr-xr-x  1 user group   2048 Nov 17 10:45 script.sh

### `free -h`, `free -t`, `free -[m,g]`, `free -s 5`

Columns in `free` Output:
- **total**: Total memory available (physical memory + swap).
- **used**: Memory that is currently in use by processes, including cache and buffers.
- **free**: Memory that is completely free and not being used.
- **shared**: Memory that is shared between multiple processes.
- **buff/cache**: Memory used by the kernel for buffers and cache (which can be released if needed).
- **available**: Memory that is available for new processes, including free memory and reclaimable memory.

### nproc return the no of cpus
### `df` Command:
- **Purpose**: The `df` command is used to check disk space usage for file systems.

#### Common Options for `df`:
- `-h`: Displays the disk usage in a human-readable format (e.g., `MB`, `GB`).
- `-T`: Shows the file system type (e.g., ext4, xfs).
- `-a`: Displays all file systems, including dummy ones.
- `-x`: Excludes file systems of a specific type from the output.

### Output Columns Explained:
- **Filesystem**: Specifies the file system or partition (e.g., `/dev/sda1`).
  - **Use**: To know which disk or partition is being reported.

- **Size**: The total size of the file system or partition.
  - **Use**: To understand the total capacity of the file system.

- **Used**: The amount of disk space that is already used.
  - **Use**: To see how much of the disk is occupied.

- **Avail**: The amount of available (free) space.
  - **Use**: To know how much space is still free to store data.

- **Use%**: The percentage of used space on the file system.
  - **Use**: To quickly gauge how full the file system is.

- **Mounted on**: The directory where the file system is mounted.
  - **Use**: To know where the file system is being used in the directory structure (e.g., `/`, `/data`).

### Example Output:
```plaintext
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/sda1      ext4  50G   5G    45G  10%  /
tmpfs          tmpfs  16G   1G    15G   7%  /dev/shm
/dev/sdb1      ext4  100G  20G   80G  20%  /data

```

### `top` Command:
- **Purpose**: The `top` command is used to display  real-time information about **CPU usage**, **memory usage**, and **running processes**.
- Continuously updates the information, providing live data on system performance.
- Allows you to interactively manage processes, such as killing or renicing them.

### Key Metrics Displayed by `top`:
- **PID**: Process ID – Unique identifier for each process.
- **USER**: The user who owns the process.
- **PR**: Process priority.
- **VIRT**: Virtual memory used by the process.
- **RES**: Resident memory (RAM) used by the process.
- **SHR**: Shared memory used by the process.
- **S**: Process status (e.g., running, sleeping, etc.).
- **%CPU**: Percentage of CPU used by the process.
- **%MEM**: Percentage of memory used by the process.
- **TIME+**: Total CPU time used by the process.
- **COMMAND**: The command that started the process.

### Example Output:
```plaintext
top - 15:30:01 up  5:03,  2 users,  load average: 0.10, 0.12, 0.14
Tasks: 128 total,   1 running, 127 sleeping,   0 stopped,   0 zombie
%Cpu(s):  4.0 us,  2.0 sy,  0.0 ni, 94.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :  32000.0 total,  10245.0 free,  11500.0 used,   5255.0 buff/cache
MiB Swap:   4096.0 total,   4096.0 free,      0.0 used.   15000.0 avail Mem
