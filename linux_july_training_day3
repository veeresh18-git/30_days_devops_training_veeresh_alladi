

### Topic: Linux File Permissions (Complete) + Ownership + ACL Introduction



```
Company

CEO
│
├── HR
│     Alice
│
├── DevOps
│     Bob
│
└── Finance
      Charlie
```


> Can Finance edit HR salary files?

Answer: No.

That's exactly what Linux permissions solve.

---

# Part 2: Understanding Permissions (15 mins)

Create a file:

```bash
touch salary.txt
ls -l
```

Output:

```text
-rw-r--r-- 1 user1 user1 0 Jul 13 salary.txt
```

Break it down:

```
-rw-r--r--
```

```
-

File

d

Directory
```

```
rw-

Owner

r--

Group

r--

Others
```



Imagine your phone.

Owner

```
You
```

Group

```
Your Family
```

Others

```
Everyone else
```

---

## Meaning of Each Permission

### Read (r)

Example:

Book

```
Can Read

Cannot Edit
```

---

### Write (w)

Notebook

```
Can Modify

Can Delete

Can Add
```

---

### Execute (x)

Remote

```
Permission to Run
```

Without execute

```
Cannot run script
```

---

# Hands-on

```bash
echo "Hello DevOps" > script.sh

./script.sh
```

Output

```
Permission denied
```

Now

```bash
chmod +x script.sh

./script.sh
```

Works (if it has a valid script header and executable content).

---

# Part 3: Numeric Permissions 



| Permission | Value |
| ---------- | ----: |
| Read       |     4 |
| Write      |     2 |
| Execute    |     1 |



Read + Write

```
4+2
```

Answer

```
6
```

Read + Execute

```
4+1
```

Answer

```
5
```

All

```
4+2+1
```

Answer

```
7
```

---

## Most Common Permissions

```
777
```

Everyone can do everything.

Dangerous.

---

```
755
```

Owner

```
rwx
```

Others

```
r-x
```

Used for scripts.

---

```
644
```

Owner edits.

Everyone reads.

Used for configuration files.

---

```
600
```

Private file.

Only owner.

Example

```
Bank Password
```

---

# Hands-on

```bash
touch project.txt

chmod 777 project.txt
ls -l
```

Now

```bash
chmod 755 project.txt
```

Now

```bash
chmod 644 project.txt
```

Now

```bash
chmod 600 project.txt
```

Students observe the permission changes after each command.

---

# Part 4: Ownership (10 mins)

Create

```bash
touch report.txt

ls -l
```

Shows

```
user1 user1
```

Meaning

```
Owner

Group
```

---

Change owner

```bash
sudo chown user2 report.txt
```

Check

```bash
ls -l
```

Now owner becomes

```
user2
```

---

Change group

```bash
sudo chgrp developers report.txt
```

Check again

```bash
ls -l
```

---

# Mini Company Lab (15 mins)

## Create Users

```bash
sudo useradd -m hr
sudo useradd -m developer
sudo passwd hr
sudo passwd developer
```

---

## Create Groups

```bash
sudo groupadd HR
sudo groupadd DEV
```

---

## Add Users

```bash
sudo usermod -aG HR hr
sudo usermod -aG DEV developer
```

---

## Create HR Folder

```bash
sudo mkdir /company

sudo mkdir /company/hr
```

---

Assign ownership

```bash
sudo chown hr:HR /company/hr
```

Permissions

```bash
sudo chmod 770 /company/hr
```

Meaning

```
Owner

Full

Group

Full

Others

No access
```

---

Login as Developer

```bash
su - developer
```

Try

```bash
cd /company/hr
```

Output

```
Permission denied
```

Students immediately understand why permissions exist.

---

# Homework

Create this structure:

```
Company

HR

DevOps

Finance
```

Requirements:

* HR user owns HR folder.
* DevOps user owns DevOps folder.
* Finance user owns Finance folder.
* Users should **not** be able to access other departments' folders.
* Use:

  * `chmod`
  * `chown`
  * `chgrp`
  * `ls -l`

---


* ✅ Linux file permissions (`r`, `w`, `x`)
* ✅ Numeric permissions (`755`, `644`, `600`, `777`)
* ✅ Symbolic permissions (`chmod +x`, `chmod -w`, etc.)
* ✅ Ownership (`chown`)
* ✅ Group ownership (`chgrp`)
* ✅ User vs Group vs Others
* ✅ Real-world security using users, groups, and permissions
