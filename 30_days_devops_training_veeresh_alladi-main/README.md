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
