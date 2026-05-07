# 30_days_devops_training_veeresh_alladi

DAY 1 _ hands _on:

GitHub:
__________
scm - code management /store


Git:
--------------
Version control - v1
                  v2

git status

file.xt 

git add file.txt
git commit -m "created file"
git push


master -

branching: 

-----------------
Master (main code) - fixed
main (developers)
development (developers)
feature branch - code changes - Hand on
release/deployment/go live - (next) docs
hotfix branch - asap 

Pull Request:
-----------------
feature branch
git push origin test - > compare pr - raise - Hands On

Merge conflicts:
-------------------
main - file.txt (already)
feature - file.txt (chnage)



DAY 2:
----------
DAY 2:
------------
developer  - push - (PR) - ci cd pipeline - build (maven) - Test (unit test cases) - deploy(dev,staging,prod) - post deploy(Health check)


triggers:
-----------

on:
  pull_request:
    branches: [ main ]
  push:
    branches: [ main ]
  schedule:
    - cron: "0 1 * * *"
  workflow_dispatch:


runners:
---------
Jenkins - workernodes
GitHub actions - selfhosted (vm)
               - ubuntu-latest




checkout:
---------
GitHub actions

actions:
--------------
aws setup
python setup
terraform setup
checkout

ci cd create for python code deploy

name: CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest/ self-hosted

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run a script
        run: echo "Build Successful!"

      - name: Run Python file
        run: python3 test.py

  test: (robot/ unit)
    runs-on: ubuntu-latest/test vm
    steps:
      - name: testing
        run: mvn install

  deploy:
    runs-on: ubuntu-latest/deploy vm
 
  post-deploy:
     runs-on:

DAY -3:
--------
Linux :
-------------
01-fundamentals
02-linux-structure
03-linux-distributions
04-setup
05-package-manager
# Core components of a Linux Machine

```plaintext
+----------------------------------------------------+
| User Applications (Vim, Docker, Apache, etc.)     |
+----------------------------------------------------+
| Shell (Bash, Zsh, Fish, etc.)                     |  <-- Part of the OS
+----------------------------------------------------+
| System Libraries (glibc, libc, OpenSSL, etc.)     |  <-- Part of the OS
+----------------------------------------------------+
| System Utilities (ls, grep, systemctl, etc.)      |  <-- Part of the OS
+----------------------------------------------------+
| Linux Kernel (Process, Memory, FS, Network)       |  <-- Core of the OS
+----------------------------------------------------+
| Hardware (CPU, RAM, Disk, Network, Peripherals)   |
+----------------------------------------------------+


(a) Hardware Layer

🔹 The physical components of the computer (CPU, RAM, disk, network interfaces, etc.).
🔹 The OS interacts with hardware using device drivers.
(b) Kernel (Core of Linux OS)

🔹 The Linux Kernel is responsible for directly managing system resources, including:

    Process Management – Schedules processes and handles multitasking.

    Memory Management – Allocates and deallocates RAM efficiently.

    Device Drivers – Acts as an interface between software and hardware.

    File System Management – Manages how data is stored and retrieved.

    Network Management – Handles communication between systems.

(c) Shell (Command Line Interface - CLI)

🔹 A command interpreter that allows users to interact with the kernel.
🔹 Examples: Bash, Zsh, Fish, Dash, Ksh.
🔹 Converts user commands into system calls for the kernel.
(d) User Applications

🔹 End-user programs like web browsers, text editors, DevOps tools, etc.
🔹 Applications interact with the OS using system calls via the shell or GUI.

PART-2:
-----------
Understanding the Folder Structure
Explanation of System Directories
Symbolic Links (Less Significant)
Directory	Description
/sbin -> /usr/sbin	System binaries for administrative commands (linked to /usr/sbin).
/bin -> /usr/bin	Essential user binaries (linked to /usr/bin).
/lib -> /usr/lib	Shared libraries and kernel modules (linked to /usr/lib).
Important System Directories
Directory	Description
/boot	Stores files needed for booting the system (not relevant in containers).
/usr	Contains most user-installed applications and libraries.
/var	Stores logs, caches, and temporary files that change frequently.
/etc	Stores system configuration files.
User & Application-Specific Directories
Directory	Description
/home	Default location for user home directories.
/opt	Used for installing optional third-party software.
/srv	Holds data for services like web servers (rarely used in containers).
/root	Home directory for the root user.
Temporary & Volatile Directories
Directory	Description
/tmp	Temporary files (cleared on reboot).
/run	Holds runtime data for processes.
/proc	Virtual filesystem for process and system information.
/sys	Virtual filesystem for hardware and kernel information.
/dev	Contains device files (e.g., /dev/null, /dev/sda).
Mount Points
Directory	Description
/mnt	Temporary mount point for external filesystems.
/media	Mount point for removable media (USB, CDs).
/data	Likely your mounted volume from Windows (C:/ubuntu-data).

DAY - 4:
-------------
User Management in Linux
Introduction to User Management in Linux
Linux is a multi-user operating system, meaning multiple users can operate on a system simultaneously. Proper user management ensures security, controlled access, and system integrity.

Key files involved in user management:

/etc/passwd – Stores user account details.
/etc/shadow – Stores encrypted user passwords.
/etc/group – Stores group information.
/etc/gshadow – Stores secure group details.
Creating Users in Linux
To create a new user in Linux, use:

useradd Command (For most Linux distributions)
useradd username
This creates a user without a home directory.

To create a user with a home directory:

useradd -m username
To specify a shell:

useradd -s /bin/bash username
adduser Command (For Debian-based systems)
adduser username
This is an interactive command that asks for a password and additional details.

Managing User Passwords
To set or change a user’s password:

passwd username
Enforcing Password Policies
Password expiration: Set password expiry days
chage -M 90 username
Lock a user account
passwd -l username
Unlock a user account
passwd -u username
Modifying Users
Modify an existing user with usermod:

Change the username:
usermod -l new_username old_username
Change the home directory:
usermod -d /new/home/directory -m username
Change the default shell:
usermod -s /bin/zsh username
Deleting Users
To remove a user but keep their home directory:

userdel username
To remove a user and their home directory:

userdel -r username
Working with Groups
Creating Groups
groupadd groupname
Adding Users to Groups
usermod -aG groupname username
Viewing Group Memberships
groups username
Changing Primary Group
usermod -g new_primary_group username
Sudo Access and Privilege Escalation
Adding a User to Sudo Group
On Debian-based systems:

usermod -aG sudo username
On RHEL-based systems:

usermod -aG wheel username
Granting Specific Commands with Sudo
Edit the sudoers file:

visudo
Then add:

username ALL=(ALL) NOPASSWD: /path/to/command

PART-2:
-----------
# File management in Linux

### File and Directory Management
1. **`ls`** – Lists files and directories in the current location.
2. **`cd /path/to/directory`** – Changes the working directory.
3. **`pwd`** – Prints the current working directory.
4. **`mkdir new_folder`** – Creates a new directory.
5. **`rmdir empty_folder`** – Removes an empty directory.
6. **`rm file.txt`** – Deletes a file.
7. **`rm -r folder`** – Deletes a folder and its contents.
8. **`cp file1.txt file2.txt`** – Copies a file.
9. **`cp -r dir1 dir2`** – Copies a directory recursively.
10. **`mv old_name new_name`** – Moves or renames a file or directory.

### File Viewing and Editing
11. **`cat file.txt`** – Displays file content.
12. **`tac file.txt`** – Displays file content in reverse order.
13. **`less file.txt`** – Opens a file for viewing with scrolling support.
14. **`more file.txt`** – Similar to `less`, but only moves forward.
15. **`head -n 10 file.txt`** – Displays the first 10 lines of a file.
16. **`tail -n 10 file.txt`** – Displays the last 10 lines of a file.
17. **`nano file.txt`** – Opens a simple text editor.
18. **`vi file.txt`** – Opens a powerful text editor.
19. **`echo 'Hello' > file.txt`** – Writes text to a file, overwriting existing content.
20. **`echo 'Hello' >> file.txt`** – Appends text to a file without overwriting.


File Permissions Management in Linux
---------------------------------------
Introduction to File Permissions
Linux file permissions determine who can read, write, or execute files and directories. Each file and directory has three levels of permission:

Owner (User): The creator of the file.
Group: Users belonging to the assigned group.
Others: All other users on the system.
Permissions are represented as:

Read (r or 4) – View file contents.
Write (w or 2) – Modify file contents.
Execute (x or 1) – Run scripts or programs.
To check file permissions, use:

ls -l filename
Output example:

-rwxr--r-- 1 user group 1234 Mar 28 10:00 myfile.sh
Changing Permissions with chmod
Using Symbolic Mode
Modify permissions using symbols:

Add (+), remove (-), or set (=) permissions.
Examples:

chmod u+x filename  # Add execute for user
chmod g-w filename  # Remove write for group
chmod o=r filename  # Set read-only for others
chmod u=rwx,g=rx,o= filename  # Set full access for user, read/execute for group, and no access for others
Using Numeric (Octal) Mode
Each permission has a value:

Read (4), Write (2), Execute (1).
Examples:

chmod 755 filename  # User (rwx), Group (r-x), Others (r-x)
chmod 644 filename  # User (rw-), Group (r--), Others (r--)
chmod 700 filename  # User (rwx), No access for others
Changing Ownership with chown
Modify file owner and group:

chown newuser filename  # Change owner
chown newuser:newgroup filename  # Change owner and group
chown :newgroup filename  # Change only group
Recursively change ownership:

chown -R newuser:newgroup directory/
Changing Group Ownership with chgrp
chgrp newgroup filename  # Change group
chgrp -R newgroup directory/  # Change group recursively
Special Permissions
SetUID (s on user execute bit)
Allows users to run a file with the file owner's permissions.

chmod u+s filename
Example: /usr/bin/passwd allows users to change their passwords.

SetGID (s on group execute bit)
Files: Users run the file with the group's permissions. Directories: Files created inside inherit the group.

chmod g+s filename  # Set on file
chmod g+s directory/  # Set on directory
Sticky Bit (t on others execute bit)
Used on directories to allow only the owner to delete their files.

chmod +t directory/
Example: /tmp directory.

Default Permissions: umask
umask defines default permissions for new files and directories. Check current umask:

umask
Set a new umask:

umask 022  # Default: 755 for directories, 644 for files
Conclusion
Understanding file permissions is essential for system security and proper file management. Using chmod, chown, and chgrp, you can control access to files and directories efficiently.



DAY - 5:
--------------
Process Management in Linux
Introduction to Process Management
A process is an instance of a running program. Linux provides multiple utilities to monitor, manage, and control processes effectively. Each process has a unique Process ID (PID) and belongs to a parent process.

Index of Commands Covered
Viewing Processes
ps aux – View all running processes
ps -u username – View processes for a specific user
ps -C processname – Show a process by name
pgrep processname – Find a process by name and return its PID
pidof processname – Find the PID of a running program
Managing Processes
kill PID – Terminate a process by PID
pkill processname – Terminate a process by name
kill -9 PID – Force kill a process
pkill -9 processname – Kill all instances of a process
kill -STOP PID – Stop a running process
kill -CONT PID – Resume a stopped process
renice -n 10 -p PID – Lower priority of a process
renice -n -5 -p PID – Increase priority of a process (requires root)
Background & Foreground Processes
command & – Run a command in the background
jobs – List background jobs
fg %jobnumber – Bring a job to the foreground
Ctrl + Z – Suspend a running process
bg %jobnumber – Resume a suspended process in the background
Monitoring System Processes
top – Interactive process viewer
htop – User-friendly process viewer (requires installation)
nice -n 10 command – Run a command with a specific priority
renice -n -5 -p PID – Change priority of an existing process
Daemon Process Management
systemctl list-units --type=service – List all system daemons
systemctl start service-name – Start a daemon/service
systemctl stop service-name – Stop a daemon/service
systemctl enable service-name – Enable a service at startup
Viewing Process Details
Using ps
Show processes for a specific user:

ps -u username
Show a process by name:

ps -C processname
Using pgrep
Find a process by name and return its PID:

pgrep processname
Using pidof
Find the PID of a running program:

pidof processname
Managing Processes
Killing Processes
To terminate a process by PID:

kill PID
To terminate using process name:

pkill processname
Force kill a process:

kill -9 PID
Kill all instances of a process:

pkill -9 processname
Stopping & Resuming Processes
Stop a running process:

kill -STOP PID
Resume a stopped process:

kill -CONT PID
Changing Process Priority
View process priorities:

top  # Look at the NI column
Change priority of a running process:

renice -n 10 -p PID  # Lower priority (positive values)
renice -n -5 -p PID  # Higher priority (negative values, root required)
Running Processes in the Background
Run a command in the background:

command &
List background jobs:

jobs
Bring a job to the foreground:

fg %jobnumber
Send a running process to the background:

Ctrl + Z  # Suspend process
bg %jobnumber  # Resume in background
Monitoring System Processes
Using top
Interactive process viewer:

Press k and enter a PID to kill a process.
Press r to renice a process.
Press q to quit.
Using htop
A user-friendly alternative to top:

htop
Allows mouse-based interaction for process management.

Using nice & renice
Run a command with a specific priority:

nice -n 10 command
Change the priority of an existing process:

renice -n -5 -p PID
Daemon Processes
Daemon processes run in the background without user intervention. List all system daemons:

systemctl list-units --type=service
Start a daemon:

systemctl start service-name
Stop a daemon:

systemctl stop service-name
Enable a service at startup:

systemctl enable service-name


Linux System Monitoring
----------------------------
Introduction to System Monitoring
Monitoring system resources is essential to ensure optimal performance, detect issues, and troubleshoot problems in Linux. Various tools allow us to monitor CPU, memory, disk usage, network activity, and running processes.

Index of Commands Covered
CPU and Memory Monitoring
top – Real-time system monitoring
htop – Interactive process viewer (requires installation)
vmstat – Report system performance statistics
free -m – Show memory usage
Disk Monitoring
df -h – Check disk space usage
du -sh /path – Show disk usage of a specific directory
iostat – Display CPU and disk I/O statistics
Network Monitoring
ifconfig – Show network interfaces (deprecated, use ip a)
ip a – Show network interface details
netstat -tulnp – Show active connections and listening ports
ss -tulnp – Alternative to netstat for socket statistics
ping hostname – Test network connectivity
traceroute hostname – Show network path to a host
nslookup domain – Get DNS resolution details
Log Monitoring
tail -f /var/log/syslog – Live monitoring of system logs
journalctl -f – Live system logs for systemd-based distros
dmesg | tail – View kernel logs
CPU and Memory Monitoring
Using top
To view real-time CPU and memory usage:

top
Press q to quit.

Using htop
A user-friendly alternative:

htop
Use arrow keys to navigate and F9 to kill processes.

Using vmstat
To check CPU, memory, and I/O stats:

vmstat 1 5  # Update every 1 sec, show 5 updates
Checking Memory Usage
free -m
Shows free and used memory in megabytes.

Disk Monitoring
Using df
Check available disk space:

df -h
Using du
Find the size of a directory:

du -sh /var/log
Using iostat
Check disk and CPU usage:

iostat
Network Monitoring
--------------------------------------------------------------------------
Checking Network Interfaces
ip a  # Show IP addresses and interfaces
Viewing Open Ports and Connections
netstat -tulnp  # Show listening ports
ss -tulnp  # Alternative to netstat
Testing Connectivity
ping google.com  # Test internet connection
traceroute google.com  # Trace the path to Google
Checking DNS Resolution
nslookup example.com
Log Monitoring
Live Monitoring of System Logs
tail -f /var/log/syslog  # Follow logs in real-time
journalctl -f  # Systemd logs
Checking Kernel Logs
dmesg | tail


Disk and Storage Management in Linux
----------------------------------------------------------------------------------------
Introduction to Disk and Storage Management
Managing disks and storage efficiently is crucial for system performance and stability. Linux provides various commands to monitor, partition, format, mount, and manage disk storage.

Index of Commands Covered
Viewing Disk Information
lsblk – Display block devices
fdisk -l – List disk partitions
blkid – Show UUIDs of devices
df -h – Check disk space usage
du -sh /path – Show size of a directory
Partition Management
fdisk /dev/sdX – Create and manage partitions
parted /dev/sdX – Alternative to fdisk for GPT disks
mkfs.ext4 /dev/sdX1 – Format a partition as ext4
mkfs.xfs /dev/sdX1 – Format a partition as XFS
Mounting and Unmounting
mount /dev/sdX1 /mnt – Mount a partition
umount /mnt – Unmount a partition
mount -o remount,rw /mnt – Remount a partition as read-write
Logical Volume Management (LVM)
pvcreate /dev/sdX – Create a physical volume
vgcreate vg_name /dev/sdX – Create a volume group
lvcreate -L 10G -n lv_name vg_name – Create a logical volume
mkfs.ext4 /dev/vg_name/lv_name – Format an LVM partition
mount /dev/vg_name/lv_name /mnt – Mount an LVM partition
Swap Management
mkswap /dev/sdX – Create a swap partition
swapon /dev/sdX – Enable swap space
swapoff /dev/sdX – Disable swap space
Viewing Disk Information
Using lsblk
List all block devices:

lsblk
Using fdisk
View partition details:

fdisk -l
Using df
Check available disk space:

df -h
Using du
Find the size of a directory:

du -sh /var/log
Partition Management
Creating a Partition with fdisk
fdisk /dev/sdX
Follow the interactive prompts to create a partition.

Formatting a Partition
Format as ext4:

mkfs.ext4 /dev/sdX1
Format as XFS:

mkfs.xfs /dev/sdX1
Mounting and Unmounting
Mount a Partition
mount /dev/sdX1 /mnt
Unmount a Partition
umount /mnt
Remount a Partition
mount -o remount,rw /mnt
LVM Management
Create a Physical Volume
pvcreate /dev/sdX
Create a Volume Group
vgcreate vg_name /dev/sdX
Create a Logical Volume
lvcreate -L 10G -n lv_name vg_name
Format and Mount the Logical Volume
mkfs.ext4 /dev/vg_name/lv_name
mount /dev/vg_name/lv_name /mnt
Swap Management
Create a Swap Partition
mkswap /dev/sdX
Enable Swap
swapon /dev/sdX
Disable Swap
swapoff /dev/sdX
Additional Notes - When to Use fdisk, mount, or Both
Check Available Disks
Before creating or mounting anything, always check what block devices exist:

lsblk
Example output:
NAME	MAJ:MIN	RM	SIZE	RO	TYPE	MOUNTPOINT
sda	8:0	0	100G	0	disk	
├─sda1	8:1	0	96G	0	part	/
└─sda2	8:2	0	4G	0	part	[SWAP]
sdb	8:16	0	20G	0	disk	
sda → existing disk (already partitioned)

sdb → new disk, no partitions yet

When to use fdisk
Use fdisk when:

The disk is brand new and has no partitions
You want to create /dev/sdb1, /dev/sdb2, etc.
Inside fdisk:

Press n → create a new partition
Press w → write changes
Then confirm:

lsblk
When to Use mount
Use mount when: The partition already exists and is formatted You just want to make it accessible

sudo mkdir /mnt/mydisk
sudo mount /dev/sdb1 /mnt/mydisk
Now your disk is available at /mnt/mydisk.

When to Use fdisk + mount (Full Setup)
Use fdisk + mkfs + mount when: The disk is completely new You need to partition → format → mount it

# 1. Check available disks
lsblk
# 2. Create partition
sudo fdisk /dev/sdb
# 3. Format the partition
sudo mkfs.ext4 /dev/sdb1
# 4. Mount it
sudo mkdir /data
sudo mount /dev/sdb1 /data


