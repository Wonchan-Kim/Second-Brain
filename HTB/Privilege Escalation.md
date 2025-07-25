Our initial access to a remote server is usually in the context of a low-privileged user, which would not give us complete access over the box. To fain full access, we need to find internal/local vulnerability that would escalate our privileges to the root user on Linux or the administrator/System user on WIndows. 

---
## PrivEsc Checklists
Once we gain initial access to a box, we want to thoroughly enumerate the box to find any potential vulnerabilities we can exploit to achieve a higher privilege level. We can find many checklists and cheat sheets online that have a collection of checks we can run and the commands to run these checks. One excellent resource is hacktricks, which has an excellent checklist for both linux and windows local privilege excalation.

---
## Enumeration scripts
Many of the above commands may be automatically run with a script to go through the report and look for any weaknesses. We can run many scripts to enumerate the server by running common commands that return any interesting findings. Some of the common linux enumeratino scrupts include luinenum and linuxprivchecker, and for windows include seatbelt and jaws. Another useful tool we may use for server enumeratino ins the privilege escalation awesome scrupts suite (PEASS) as it is well mainted to remain up to date and includes scripts for enumerating both Linux and Windows.

These scrupts will run manuy commands known for identifying vulnerabilities and create a lot of noise that may trigger anti-virus socftware or security monitoring software that looks for these types of events. This may prevent the scripts form running or even triger an alarm that the system has been compromised. In some instances, we may want to do a manual enumeration instead of running scripts. 

Ex) using PEASS called LinPEAS
```
kwc0827@htb[/htb]$ ./linpeas.sh
...SNIP...

Linux Privesc Checklist: https://book.hacktricks.xyz/linux-unix/linux-privilege-escalation-checklist
 LEYEND:
  RED/YELLOW: 99% a PE vector
  RED: You must take a look at it
  LightCyan: Users with console
  Blue: Users without console & mounted devs
  Green: Common things (users, groups, SUID/SGID, mounts, .sh scripts, cronjobs)
  LightMangenta: Your username


====================================( Basic information )=====================================
OS: Linux version 3.9.0-73-generic
User & Groups: uid=33(www-data) gid=33(www-data) groups=33(www-data)
...SNIP...
```
Once the script runs, it start collecting information and displaying it in an excellent report.

---
### Kernel exploits
Whenever encountering a server running an old operation system, we should start by looking for potential kernal vulnerabilities that may exist. Suppose the server is not being maintained with the latest updates and patches. In that case it is likely vernalable to specific kernel exploits found on unpatched versions of Linux and Windows.

Above script showed us the linux version to be 3.9.0-73-generic. If we google exploits fo rthis version or use searchsploit, we would find a CVE-2016-5195 otherwise known as dirtycow. We can search for and download the dirtycow exploit and run it on the server to gain root access.

The same concept applies to windows, as there are amny vulnerabilites in unpatched versions of windows, with various vulnerabilities that can be used for privilege escalation. We should keep in mind that kernel exploits can cause system instability, and we should take great care before running them on production systems. It is best to try them in a lab environment and only run them on production systems with explicit approval and oordination with our client. 

---
### Vulnerable software
Another thing we should look for is installed software. For example, we can sue the dpkg -l command on linux or look at C:\Program files in windows to see what software is installed on the system. We should look for public exploits for any installed software, especially if any older versions are in use, containing unpatched vulnerabilities. 

### User Privileges
Another critical aspect to look for after gaining access to a server is the privileges available to the user we have access to. Suppose we are allowed to run specific commands as root (or as another user). In that case, we may be able to escalate our privileges 
1. Sudo
2. SUID
3. Windows token privileges
The sudo command in linux allows a user to execute commands as a different user. It is usually used to allow lower privilidged users to execute commands as root without giving them access to the root user. This is generally done as apecific commands can only be run as root 'like tcpdump'or allow the user to access certain root-only directories. We can check what sudo privileges we have with the sudo -l command:

We can also use su command with sudo to switch to the root user:
Sudo su-
This command requries a password to run any commands with sudo. There are certain occasions where we may be allowed to execute certain applications, ro all applications, without having to provide a password. 

```
kwc0827@htb[/htb]$ sudo -l

    (user : user) NOPASSWD: /bin/echo
```


The NOPASSWD entry shows that the /bin/echo command can be executed without a password. This would be useful if we gained access tot the server through a vulnerability and did not have the user's password. As it says user, we can run sudo as that user and not as root. To do so, we can specify the user with -u user. 
```
kwc0827@htb[/htb]$ sudo -u user /bin/echo Hello World!

    Hello World!

```
Once we find a particular application runnable with sudo, we can look for ways to exploit it to get a shell as the root user. GTFOBins contains a list of commands and how they can exploited through sudo. We can search for the application we have sudo privilege over, and if it exists, it may tell us the exact command we should execute to gain root access using the sudo privilege we have.

LOLBAS also contains a list of Windows applications which we may be able to leverage to perform certain functions, like downloading files or executing commands in the context of a privileged user. 

----
## Scheduled Tasks
In both Linux and Windows, there are methods to have scripts run at specif intervals to carry out a task. Some examples are having an anti virus scan running every hour or a backup script that runs every 30 minutes. There are usually two ways to take advantage of scehdueled tasks or corn jobs (LInux) to escalate our privileges:

- Add new scheduled tasks/cron jobs
- Trick them to execute a malicious software
  
The easiest way? Check if we are allowed to add new sceduled tasks. In linux, a common form of maintaining scheduled takss is through Cron Jons. There are specific direcrotiees that we may be able to utilize tto add new cron jovs if we have the write permissions over them.
Including :
1. /etc/crontab
2. /etc/cron. D
3. /var/spool/cron/crontabs/root
If we can write to a directory called by a cron job, we can write a bash script with a reverse shell command, which should send us a reverse shell when executed.

---
## Exposed Credentials
Next, we can look for files we can read and see if they contain any exposed credentials. This is a very common with configuration files, log files, and user history files. The enumeration scripts usually look for potential passwords in files and provide them to us,
```
...SNIP...
[+] Searching passwords in config PHP files
[+] Finding passwords inside logs (limit 70)
...SNIP...
/var/www/html/config.php: $conn = new mysqli(localhost, 'db_user', 'password123');
```

On the example above, password is exposed, which would allow us to log in to the local mysql databases and look for interesting information. We may also check for Password reuse, as the system user may have used their password for the databases, which may allows us to use the same password to switch to that user, as follows
```
kwc0827@htb[/htb]$ su -

Password: password123
whoami

root
```
Using user credentials to ssh into server as that users

----
### SSH Keys
If read over the .ssh directory for a specific user, we may read their private ssh keys found in /home/user/. Ssd/id_rsa or /root/. Ssh/id_rsa and use it to log in to the server. If we can read the directory, and can read the id_rsa file, we can copy it to our machine and use the -i flag to log in with it
```
kwc0827@htb[/htb]$ vim id_rsa
kwc0827@htb[/htb]$ chmod 600 id_rsa
kwc0827@htb[/htb]$ ssh root@10.10.10.10 -i id_rsa

root@10.10.10.10#
```
Chmod 600 id_rsa on th ekey after we created in on our machine to change the file's permissions to be more restrictive. If ssh keys have lax permissions, ssh server would prevent them from working. 
If found with write access to a users/. Ssh/directory, we can place our public key in the user's ssh directory at home/user/. Ssh/authorized_keys. This technique is usually used to gain ssh access after gaining a shell as that user. The current SSH configuration will not accept keys written by other users, so it will only work if we have already gained control over that user. Must first create a new key with ssh-keygen and the -f flag to specify the output file

```
kwc0827@htb[/htb]$ ssh-keygen -f key

Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase): *******
Enter same passphrase again: *******

Your identification has been saved in key
Your public key has been saved in key.pub
The key fingerprint is:
SHA256:...SNIP... user@parrot
The key's randomart image is:
+---[RSA 3072]----+
|   ..o.++.+      |
...SNIP...
|     . ..oo+.    |
+----[SHA256]-----+
```

This will give us two files: key (which we will use with ssh -i) and key. Pub, which we will copy to the remote machine. Let us copy key. Pub, then on the remote machine, we will add it into root/. Ssh/authrozied_keys:
```
user@remotehost$ echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys

```
Now the remote server should allow us to log in as that user by using our private key:

```
kwc0827@htb[/htb]$ ssh root@10.10.10.10 -i key

root@remotehost# 
```
We can now ssh in as the user root. The linux privilege escalation and the windows privilege escalation modules go into more details on how to use each of these methods for privilege escalation, and many others as well.