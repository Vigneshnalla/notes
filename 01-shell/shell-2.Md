### A  shebang (#!) 
- is the first line in a script file that tells the operating system what interpreter to use to execute the script. 


### `set` Commands

- **`set -exo pipefial`**
- **`set -x`**: Enables debugging, printing each command and its arguments as they are executed.
- **`set -e`**: Causes the script to exit immediately if any command returns a non-zero exit status (error).
- **`set -o pipefail`**: if any command in the pipeline fails  the entire pipeline returns a non-zero exit status causing the script to stop
ex:
``` bash
#!/bin/bash
set -o pipefail

echo "Starting script"
# Simulate a failure in the first command of the pipeline
non_existent_command | echo "This will not run"

echo "Script completed"

```
If you don't set set -o pipefail, the script would not fail because it only checks the exit status of the last command in the pipeline (i.e., echo "This will not run"), which would still run even if the first command fails.


### Before executing any script, it's better to check the permissions

## `chmod`
- **userPerm** - **group_perm** - **other**
  
- **`chmod 777 file.txt`**: Grants full permissions (read, write, and execute) to the user, group, and others.
- **`chmod g+w file.txt`**: Adds write permission for the group.
- **`chmod g-w file.txt`**: Removes write permission for the group.
- **`chmod g=rx file.txt`**: Sets group permissions to read and execute.
- **`chmod o+x file.txt`**: Adds execute permission for others.
- **`chmod o-x file.txt`**: Removes execute permission for others.
- **`chmod o=r file.txt`**: Sets others' permissions to read only.

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

```
### `ps` Command

The `ps` command in Linux/Unix is used to display information about running processes on a system. It shows the currently active processes, their IDs, and resource usage, among other details. 

### Common `ps` Command Options:

- **`ps`**: Displays the processes running in the current shell.
- **`ps aux`**: Displays all processes running on the system for all users.
  - **`a`**: Shows processes for all users.
  - **`u`**: Displays the user/owner of the process.
  - **`x`**: Shows processes that do not have a terminal (e.g., background processes).
  
- **`ps -ef`**: Displays all processes running on the system in a full-format listing.
  - **`-e`**: Shows all processes.
  - **`-f`**: Shows full details, including the parent process ID (PID).

- **`ps -aux | grep <process_name>`**: Used to search for a specific process in the output.
  
### Example Output:

```bash
$ ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 169028  5628 ?        Ss   01:00   0:10 /sbin/init
vignesh    245  0.0  0.3 244176  6884 pts/0    Ss   02:03   0:00 bash
vignesh    267  0.1  1.2 1769404 24844 pts/0   Sl+  02:04   0:23 /usr/bin/python3
```
#### columsn in ps
PID: Process ID
%CPU: Percentage of CPU usage
%MEM: Percentage of memory usage
VSZ: Virtual memory size
STAT: Process status
START: Start time of the process
TIME: Total accumulated CPU time
COMMAND: Command that started the process


#### Grep
Channels : studout, studproc, stud error
some commands will send the outputh in changess on that time piple will not work as expected


## commands
    ps -ef | grep amazon | awk '{print $2}'


## curl
-X POST: Specifies the HTTP method as POST.
-d: Sends the specified data in the request body.

### `find` Command

The `find` command is used to search for files and directories within a specified directory hierarchy. It allows you to search based on a variety of conditions such as name, size, type, modification date, and more.

#### Basic Syntax:

```bash
find <path> <conditions> <actions>
find /path/to/search -name "filename.txt"
. Find files modified within the last 7 days:
find /path/to/search -mtime -7
Find files larger than 1GB:
    find /path/to/search -size +1G


if []
then

else

fi

for i in {1..5}
do
    echo "$i"
done

while [ condition ]
do
    # commands
done


do 

done while []


### `kill` Command

The `kill` command in Linux is used to send signals to processes, typically to terminate them. By default, `kill` sends the `SIGTERM` signal, which gracefully terminates the process. You can specify different signals to control processes in various ways.

#### Basic Syntax:

```bash
kill [options] <pid>
```
- **`<pid>`**: The Process ID (PID) of the process to which the signal should be sent.
- **`[options]`**: Options for specifying the type of signal.

#### Common Signals:
- **`SIGTERM (15)`**: Terminate the process gracefully (default signal).
- **`SIGKILL (9)`**: Forcefully kill the process. The process cannot ignore this signal.
- **`SIGSTOP (19)`**: Pause (stop) the process.
- **`SIGCONT (18)`**: Resume a stopped process.
- **`SIGHUP (1)`**: Hang up the process (often used to restart processes).

#### Examples:

1. **Kill a process by PID:**

```bash
kill 1234
```
- Sends the `SIGTERM` signal to the process with PID 1234. This allows the process to clean up and terminate gracefully.

2. **Force kill a process:**

```bash
kill -9 1234
```
- Sends the `SIGKILL` signal to the process with PID 1234. This immediately terminates the process without giving it a chance to clean up.

3. **Send a different signal (e.g., SIGHUP):**

```bash
kill -1 1234
```
- Sends the `SIGHUP` signal to the process with PID 1234. This is often used to request a process to reload its configuration without terminating it.

4. **Kill all processes by name** (using `pkill`):

```bash
pkill firefox
```






### commands left [trap , trace]