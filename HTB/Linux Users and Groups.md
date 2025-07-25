
Users and groups are indispensable concepts for describing Linux permission system. Each user has a name and unique user id (UID). A group is literally a group that multiple users can belong to, which also has a group name and a unique group id (GID). When an user accesses a system resource, such as a file or directory, linux checks the uid and gid of the group the user belongs to determine if the user has legitimate permissions to access. /etc/password is a text file that contatins the user information for linux, contains information such as users name, user id, and gid they belong to. 

/etc/group is a text file that stores group information in linux. It contains information such as the name of each group, the group id, and a list of users who belong to the group. 

Linux uses permissions to control what users can do with files and directories. Each file and directory has an owner and an ownership group. The owner has the ability to modify the permissions of a file or directory. This allows the owner to set how much access users included in the owner or ownership group will have to that file or directory. 

READ, WRITE, EXECUTE
> [!note] Ownership
> Read: allows to view the contents
> Write: allows to modify or delete the contents of files or directory
> Execute: if the file is program, allows for run, for directories, allows the content of the directory

Ls -l command can be used to view the permission
```
user@user-VirtualBox:~/new_dir$ ls -l
total 12
drwxrwxr-x 2 user user 4096 Dec 2 13:38 dir
---------- 1 user user   13 Dec 2 13:05 hello
-rwxrw-r-- 1 user user   13 Dec 2 13:08 world
user@user-VirtualBox:~/new_dir$
```

First column shows permission flags, and the third column represents the owner. The fourth column represents the ownership group. 
>  drwxrwxr-x 2 user user 4096 Dec 2 13:38 dir

Dir has a permission flag of drwxrwxr -x an owner of user, and an owner ship group of user.
Permission flag can be divided into four groups, d/rwx/rwx/r-x
> [!tip] First character of permission flag
> D: directory || -: regular file || l: link file
 
Left chunks are then divided into three chunks with three letter each. 
First chunk represents permission of the owner, then permission of the users in the ownership group, and permission of all other users.

==CHMOD==
Is a command to change file permissions.
Only be executed by the root user or the owner of the file, in a 
```
chmod PERMISSION FILE_NAME
```
Format.

While chmod permission changes the permission, there is way to change the permission for the owner group.

```
chmod g + or - permission filename
```
==chown==
Command to change a file’s owner or ownership group. It can only be executed by root user, in a 
```
chown USER_NAME[.GROUP_NAME] FILE_NAME 
```
If only need to change the ownership group, then use chgrp command.

```
user@user-VirtualBox:~/new_dir$ ls -l
total 12
drwxrwxr-x 2 user user 4096 Dec  2 13:38 dir
-rwxr--r-- 1 user user   13 Dec  2 13:05 hello
-rwxrw-r-- 1 user user   13 Dec  2 13:08 world
```

How to change the owner of hello file from user to root?
To run the command as root, prefix it with sudo

```
user@user-VirtualBox:~/new_dir$ sudo chown root hello
[sudo] password for user: 
user@user-VirtualBox:~/new_dir$ ls -l
total 12
drwxrwxr-x 2 user user 4096 Dec  2 13:38 dir
-rwxr--r-- 1 root user   13 Dec  2 13:05 hello
-rwxrw-r-- 1 user user   13 Dec  2 13:08 world
```
Now the user can only read the hello file
```
user@user-VirtualBox:~/new_dir$ echo "hello" > hello
bash: hello: Permission denied
user@user-VirtualBox:~/new_dir$ 
```

---
## Special permissions

In addition to r, w and x, there are three special permissions

- Setuid
- Setgid
- Sticky bit
> [!note] setuid
> When regular user runs a file, it runs with the file owner’s permission. /bin/passwd is owned by root but has a setuid allows a normal user to run it with root permission and change the password as well. It is represented by the letters s instead of x in the owner’s execute permissions. If you see S, the setuid is on, but you do not have execute permissions.

> [!note] setgid
> When a normal user runs the file, it runs with the files owning group permissions. The setgid is represented by the letters s instead of x in the owning group’s execute permissions. Similarly, if you dont have execute permissioms but have setgid, it will appear as S in upper case letters.

> [!note] Sticky Bit
> Setting a sticky bit on a directory prevents regular users other than the file and directory owner and the root user from deleting the file. Typically used for public directories. It is represented by the letters t instead of x in a normal user’s execute permissions. Similarly, if you do not have execute permissions, it will appear as T in capitalized letters.

When specifying special permissions, prefix the permission flag with a number: setuid is 4, setgid is 2, sticky bit is 1. 
```
user@user-VirtualBox:~/new_dir$ ls -l
total 12
drwxrwxr-x 2 user user 4096 Dec  2 13:38 dir
-rwxr--r-- 1 root user   13 Dec  2 13:05 hello
-rwxrw-r-- 1 user user   13 Dec  2 13:08 world
user@user-VirtualBox:~/new_dir$ chmod 4775 world
user@user-VirtualBox:~/new_dir$ ls -l
total 12
drwxrwxr-x 2 user user 4096 Dec  2 13:38 dir
-rwxr--r-- 1 root user   13 Dec  2 13:05 hello
-rwsrwxr-x 1 user user   13 Dec  2 13:08 world
```

Prefix the permission flag with a number. 
If just setting the setuid, setgid and stick bit, chmod u+s, chmod g+s, chmod o+t can be used as well.

S