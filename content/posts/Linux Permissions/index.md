---
title: Understanding Linux Permissions - A Beginner-Friendly Guide
author: b33tl3
description: Learn how Linux file permissions work, including ownership, read, write, and execute permissions, as well as practical usage of chmod, chown, and chgrp.
date: 2026-06-21 00:00:00+0000
categories: [Linux, Cybersecurity]
tags: [Linux, Permissions, chmod, chown, Access Control, System Administration]
---

# Understanding Linux File Permissions
![Challenge](LinuxPermissions.png)
*Figure 1: Overview of Linux file permissions, ownership categories, permission types, and common permission configurations.*
## Introduction
One of the most important security features in Linux is its permission system. Every file and directory in Linux has permissions that determine who can access it and what actions they can perform. These permissions help protect sensitive data, prevent unauthorized modifications, and maintain system stability. <br>

Whether you are a Linux administrator, developer, or cybersecurity professional, understanding file permissions is a fundamental skill. In this article, we'll explore how Linux permissions work, how to view and modify them, and why they play such a critical role in system security.

## Why Permissions Matter
Imagine a system where every user can read, modify, or delete any file. Such an environment would quickly become insecure and difficult to manage. <br>

Linux permissions help:
 - Protect sensitive information.
 - Control who can access files and directories.
 - Prevent accidental modifications.
 - Restrict unauthorized users.
 - Enforce the Principle of Least Privilege.

Permissions form the foundation of Linux security and are essential for managing multi-user environments.

## Understanding Ownership
Every file and directory in Linux is associated with: <br>
1. **Owner (User)** - The user who owns the file.
2. **Group** - A collection of users with shared access rights.
3. **Others** - All other users on the system.

For example:
```bash
linux ls -l 
total 0
-rw-rw-r-- 1 b33tl3 b33tl3 0 Jun 21 10:35 script.sh
```
In this example:
 - Owner: `b33tl3`
 - Group: `b33tl3`
 - Others: Everyone else
Linux evaluates permissions differently for each of these categories. <br>

## Permission Types
Linux uses three basic permission types:

### Read (r)
Allows viewing the contents of a file.
```bash
cat notes.txt
```
For directories, read permission allows users to list directory contents.

### Write (w)
Allows modifying or deleting a file.
```bash
echo "New content" >> notes.txt
```
For directories, write permission allows users to create or remove files.

### Execute (x)

Allows running a file as a program or script.
```bash
./script.sh
```
For directories, execute permission allows users to enter the directory.

## Understanding Permission Structure
Permissions are represented as three sets:

```text
rwx r-x r--
│   │   │
│   │   └── Others
│   └────── Group
└────────── Owner
```
Each permission has a numeric value:
| Permission  | Value |
| ----------- | ----- |
| Read (r)    | 4     |
| Write (w)   | 2     |
| Execute (x) | 1     |

These values are combined to create numeric permission codes.

## Common Numeric Permissions
### 777
```text
rwx rwx rwx
```
Everyone has full access.<br>
**Not recommended** because it creates significant security risks.

### 755
```text
rwx r-x r-x
```
 - Owner: Read, Write, Execute
 - Group: Read, Execute
 - Others: Read, Execute
Commonly used for scripts and directories.

### 644
```text
rw- r-- r--
```
 - Owner: Read, Write
 - Group: Read
 - Others: Read
Commonly used for regular files.

### 700
```text
rwx --- ---
```
Only the owner has access. <br>
Useful for private directories and scripts.

### 600
```text
rw- --- ---
```
Only the owner can read and write. <br>
Commonly used for sensitive files such as SSH keys.

## Viewing Permissions
To view file permissions, use:
```bash
ls -l
```
Example output:
```bash
-rw-rw-r-- 1 b33tl3 b33tl3 0 Jun 21 10:35 script.sh
```
Breaking it down:
```text
-rw-rw-r--
```

 - `-` = Regular file
 - `rw-` = Owner permissions
 - `rw-` = Group permissions
 - `r--` = Others permissions

This command is one of the most frequently used tools when troubleshooting Linux access issues.

## Modifying Permissions with chmod
The `chmod` command allows you to change permissions.
### Make a Script Executable
```bash
chmod +x script.sh
```

Before:
```bash
-rw-rw-r-- script.sh
```

After:
```bash
-rwxrwxr-x script.sh
```

### Remove Write Permission
```bash
chmod -w notes.txt
```

### Add Write Permission to Group
```bash
chmod g+w notes.txt
```

### Using Numeric Values
Grant owner full access and allow everyone else to read and execute:
```bash
chmod 755 script.sh
```

Set a regular file permission:
```bash
chmod 644 notes.txt
```

Restrict access to the owner only:
```bash
chmod 700 private_script.sh
```

## Changing Ownership
Ownership can be changed using `chown`. <br>
Change owner:
```bash
sudo chown john file.txt
```

Change owner and group:
```bash
sudo chown john:developers file.txt
```
To change only the group:
```bash
sudo chgrp developers file.txt
```
These commands are particularly useful when managing shared projects and system files.

## Hands-On Linux Permissions Lab
![Challenge](HandsOn.png) <br>
*Figure 2: Practical exercise demonstrating file creation, permission modification, and permission testing.*<br>
Let's walk through a simple permissions exercise.

### Step 1: Create Files
```bash
touch linux.txt
echo "My Linux notes" > notes.txt
vim script.sh
```

Add the following content to `script.sh`:
```bash
echo "Hello Hacker"
```

View the files:
```bash
ls -l
```

### Step 2: Read Files
Display contents:
```bash
cat notes.txt
```

Read the first five lines of a file:
```bash
head -n 5 /etc/passwd
```

Read the last five lines:
```bash
tail -n 5 /etc/passwd
```

### Step 3: Modify Permissions
Make the script executable:
```bash
chmod +x script.sh
```

Make a file read-only:
```bash
chmod a-w linux.txt
```

Set permissions numerically:
```bash
chmod 640 notes.txt
```

Create a directory and assign permissions:
```bash
mkdir project
chmod 755 project
```

### Step 4: Test Permissions
Try writing to a read-only file:
```bash
echo "test" > linux.txt
```

Expected result:
```bash
Permission denied
```

Try running a script without execute permissions:
```bash
./script.sh
```

Expected Output:
```bash
Permission denied
```

After adding execute permissions:
```bash
chmod +x script.sh
./script.sh
```

Output:
```bash
Hello Hacker
```

These experiments demonstrate how permissions directly affect what users can do on a system.

## Common Mistakes
### Using 777 Everywhere
Many beginners use:
```bash
chmod 777 file.txt
```
to quickly solve permission issues. <br>
While this often works, it grants full access to everyone and creates unnecessary security risks.

### Forgetting Execute Permissions
A script may contain valid code but still fail to run because it lacks execute permissions.

### Incorrect Ownership
Files copied between users or systems may inherit incorrect ownership, preventing legitimate access. <br>
Always verify ownership using:
```bash
ls -l
```

## Security Best Practices
When managing Linux systems:
 - Follow the Principle of Least Privilege.
 - Grant only the permissions that are required.
 - Avoid using `777` unless absolutely necessary.
 - Use groups to simplify permission management.
 - Regularly audit file permissions.
 - Protect sensitive files using restrictive permissions such as `600` or `700`.
 - Review ownership after transferring files between users.

These practices significantly reduce the risk of unauthorized access and accidental changes.

## Conclusion
Linux file permissions are a core component of system security and administration. By understanding ownership, permission types, numeric values, and commands such as `chmod`, `chown`, and `chgrp`, you can effectively control access to files and directories while maintaining a secure environment. <br>

Mastering Linux permissions not only improves your ability to manage systems but also provides a strong foundation for cybersecurity, system administration, and DevOps practices. As with many Linux concepts, the best way to learn is through hands-on practice. Experiment with different permission settings, observe the results, and develop an intuition for how Linux enforces access control. <br>

Understanding permissions is often the first step toward troubleshooting access issues, securing sensitive data, and managing Linux systems effectively. <br>

Thanks for reading!😊