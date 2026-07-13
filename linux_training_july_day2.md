
---

# Linux User Management 

Imagine EPAM has:

* HR Team
* DevOps Team
* Developers
* Managers

Can a Developer see HR salary files?

**No.**

Why?

Because every employee has:

* Username
* Group
* Permissions

Linux works exactly the same way.

---

# Concept 1: What is a User?

Every person using Linux is called a **User**.

Example:

```
Laptop

Rahul
Priya
Admin
Guest
```

Each login is a user.

Command:

```bash
whoami
```

Output

```
veeresh
```

Explain:

> This tells Linux who is currently logged in.

---

## Show Current User Details

```bash
id
```

Example Output

```text
uid=1000(veeresh)
gid=1000(veeresh)
groups=1000(veeresh),27(sudo)
```

Explain simply:

Imagine your Aadhaar card.

```
Name
ID Number
Department
```

Linux also stores:

```
Username
User ID
Group
```

---

# Concept 2: Create User

Story:

Your company hired a new employee.

Need a new login.

Command

```bash
sudo useradd ravi
```

Set password

```bash
sudo passwd ravi
```

Linux asks

```
New password
Retype password
```

Now Ravi can login.

---

# Hands-on Activity

Create three employees

```bash
sudo useradd dev1

sudo useradd dev2

sudo useradd tester
```

Set passwords

```bash
sudo passwd dev1
sudo passwd dev2
sudo passwd tester
```

Show

```bash
cat /etc/passwd
```

Explain

Think of it like the company's employee register.

Every employee is listed there.

---

# Concept 3: Switch User

Current user

```
veeresh
```

Need to login as Ravi.

Command

```bash
su ravi
```

Ask password.

Now

```bash
whoami
```

returns

```
ravi
```

Explain

Exactly like logging into another Gmail account.

---

# Concept 4: Delete User

Employee resigned.

Remove account.

```bash
sudo userdel ravi
```

Delete user with home folder

```bash
sudo userdel -r ravi
```

Explain

Without `-r`

Only account removed.

Home folder remains.

---

# Groups

Story

Imagine company teams.

```
DevOps Team

Ravi

Rahul

Priya
```

All belong to one team.

Linux calls this

Group

---

Create group

```bash
sudo groupadd devops
```

Add user

```bash
sudo usermod -aG devops ravi
```

Check groups

```bash
groups ravi
```

Output

```
ravi devops
```

Explain

Ravi now belongs to DevOps Team.

---

# Real DevOps Example

Production server

Only DevOps team should deploy applications.

Developers

Cannot.

So Linux checks

```
Which group?
```

Then allows access.

---

# User Management Mini Lab

Create users

```
Developer

Tester

Manager
```

Commands

```bash
sudo useradd developer

sudo useradd tester

sudo useradd manager
```

Create group

```bash
sudo groupadd project
```

Add users

```bash
sudo usermod -aG project developer

sudo usermod -aG project tester
```

Verify

```bash
groups developer
```

---

# File Management 

Imagine your laptop.

```
Documents

Photos

Movies

Music
```

Linux also stores everything inside folders.

Everything is either

File

or

Directory

---

## Create Folder

Command

```bash
mkdir DevOps
```

Explain

Like creating a new folder in Windows.

---

Inside it

```bash
cd DevOps
```

---

Create files

```bash
touch docker.txt

touch terraform.txt

touch kubernetes.txt
```

Ask students

Open Windows.

Create three files.

Linux does same thing.

---

View Files

```bash
ls
```

Meaning

Show everything.

---

Long Format

```bash
ls -l
```

Explain

Like File Properties in Windows.

Shows

Owner

Permission

Size

Date

---

Hidden Files

```bash
ls -la
```

Explain

Windows has hidden folders.

Linux too.

Files beginning with

```
.
```

are hidden.

---

# Copy File

Story

Need backup.

Original

```
Resume.doc
```

Backup

```
Resume_Copy.doc
```

Linux

```bash
cp docker.txt backup.txt
```

---

Copy Folder

```bash
cp -r DevOps Backup
```

Explain

Without

```
-r
```

Linux refuses.

---

Move File

Story

Move photo

Downloads

↓

Pictures

Linux

```bash
mv docker.txt Notes/
```

---

Rename

```bash
mv docker.txt docker-notes.txt
```

Explain

Move and Rename use the same command.

---

Delete File

```bash
rm docker-notes.txt
```

Explain

No Recycle Bin.

Gone forever.

---

Delete Folder

```bash
rm -r Backup
```

Explain

Linux asks

Delete everything?

Yes.

---

# Find Files

Imagine

Need

```
salary.xlsx
```

But don't know location.

Use

```bash
find . -name salary.xlsx
```

---

# Search Inside File

Imagine

1000-line log.

Need word

```
ERROR
```

Use

```bash
grep ERROR app.log
```

Explain

Google Search

inside a file.

---

# View File

Create

```bash
echo "Welcome to Linux" > linux.txt
```

View

```bash
cat linux.txt
```

---

Large File

```bash
less linux.txt
```

Explain

Like scrolling a PDF.

Press

```
q
```

to exit.

---

First Lines

```bash
head linux.txt
```

Last Lines

```bash
tail linux.txt
```

---

Live Log

```bash
tail -f app.log
```

Real DevOps example:

Application running.

Logs continuously generated.

Need live monitoring.

---

# Complete Hands-on Lab 

Imagine you joined a company.

Your manager says:

Create project.

Inside create

```
Scripts

Logs

Terraform
```

Inside Scripts create

```
deploy.sh
```

Inside Logs

```
application.log
```

Give execute permission to deploy script.

Copy it to Backup folder.

Rename it.

Delete Backup.

Commands

```bash
mkdir Project

cd Project

mkdir Scripts Logs Terraform

touch Scripts/deploy.sh

touch Logs/application.log

chmod +x Scripts/deploy.sh

mkdir Backup

cp Scripts/deploy.sh Backup/

mv Backup/deploy.sh Backup/deploy_backup.sh

rm -r Backup

tree
```

A great way to combine **Linux user management + Markdown + GitHub** is to ask each user to create their own notes and show that they **cannot access each other's files**.

---

# Hands-on Lab: user1 and user2 with README.md

## Objective

* Create two users
* Each user creates their own `README.md`
* Verify ownership
* Demonstrate that one user cannot view the other's private file

---

## Step 1: Create Users (as root or with sudo)

```bash
sudo useradd -m user1
sudo passwd user1

sudo useradd -m user2
sudo passwd user2
```

Verify:

```bash
cat /etc/passwd | grep user
```

Expected output:

```text
user1:x:1001:1001::/home/user1:/bin/bash
user2:x:1002:1002::/home/user2:/bin/bash
```

---

# Step 2: Login as user1

```bash
su - user1
```

Check:

```bash
whoami
pwd
```

Output:

```text
user1
/home/user1
```

---

# Step 3: Create Project Folder

```bash
mkdir LinuxNotes
cd LinuxNotes
```

---

# Step 4: Create README.md

```bash
touch README.md
```

Edit it:

```bash
nano README.md
```

Add:

```markdown
# Linux Notes

## Owner
user1

## Commands Learned

- pwd
- ls
- mkdir
- touch
- chmod
```

Save:

* Ctrl + O
* Enter
* Ctrl + X

Verify:

```bash
cat README.md
```

---

# Step 5: Check Permissions

```bash
ls -l
```

Output:

```text
-rw-r--r-- 1 user1 user1 README.md
```

Explain:

```
Owner  -> user1
Group  -> user1
Others -> Read
```

Ask students:

> Can user2 read this file?

**Yes**, because `Others` have read permission.

---

# Step 6: Make File Private

```bash
chmod 600 README.md
```

Check:

```bash
ls -l
```

Output:

```text
-rw------- 1 user1 user1 README.md
```

Explain:

```
Owner  -> Read + Write
Group  -> No Access
Others -> No Access
```

---

# Step 7: Logout

```bash
exit
```

---

# Step 8: Login as user2

```bash
su - user2
```

Check:

```bash
whoami
pwd
```

Output:

```text
user2
/home/user2
```

---

# Step 9: Try to Read user1's File

```bash
cat /home/user1/LinuxNotes/README.md
```

Output:

```text
cat: /home/user1/LinuxNotes/README.md: Permission denied
```

Explain:

> Linux checks the owner and permissions. Since `user2` is neither the owner nor allowed by the permissions, access is denied.

---

# Step 10: Create user2's Own Notes

```bash
mkdir LinuxNotes
cd LinuxNotes

touch README.md

nano README.md
```

Add:

```markdown
# Linux Notes

## Owner
user2

## Commands Learned

- cp
- mv
- rm
- chmod
- chown
```

Save and verify:

```bash
cat README.md
```

---

# Step 11: Make user2's File Private

```bash
chmod 600 README.md
```

Check:

```bash
ls -l
```

Output:

```text
-rw------- 1 user2 user2 README.md
```

---

# Step 12: Switch Back to user1

```bash
exit

su - user1
```

Try:

```bash
cat /home/user2/LinuxNotes/README.md
```

Output:

```text
Permission denied
```

---

# Final Verification

As root (or with `sudo`), run:

```bash
ls -l /home/user1/LinuxNotes
ls -l /home/user2/LinuxNotes
```

Example:

```text
-rw------- 1 user1 user1 README.md
-rw------- 1 user2 user2 README.md
```

---

# Discussion Questions

Ask your students:

1. Who owns the first `README.md`?
2. Why couldn't `user2` read `user1`'s file?
3. What does `chmod 600` do?
4. What would happen if we changed it to `644`?
5. Which permission controls access for "others"?

---

# Bonus Demonstration

Change the permissions as `user1`:

```bash
chmod 644 README.md
```

Now switch to `user2` and try:

```bash
cat /home/user1/LinuxNotes/README.md
```

---

# End with One Easy Memory Trick

| Windows           | Linux         |
| ----------------- | ------------- |
| New Folder        | `mkdir`       |
| New File          | `touch`       |
| Open Folder       | `cd`          |
| Show Files        | `ls`          |
| Copy              | `cp`          |
| Cut/Paste         | `mv`          |
| Delete File       | `rm`          |
| Delete Folder     | `rm -r`       |
| Search File       | `find`        |
| Search Text       | `grep`        |
| Current User      | `whoami`      |
| Create User       | `useradd`     |
| Change Password   | `passwd`      |
| Delete User       | `userdel`     |
| Create Group      | `groupadd`    |
| Add User to Group | `usermod -aG` |
