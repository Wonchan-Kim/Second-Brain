## Directory structure

Root directory / is the top level directory in Linux, with an absolute path being / .

You can run cd /; ls -l or ls -l / to see the files and directories that exist in the root directory. 

/bin directory contains the basic commands and programs that are available to users.
/boot: this is the directory that contains the files needed to boot the system
/dev: the physical devices attached to the computer are accessible through device drivers. Those devices are accessible as files in this directory
/etc: contains configuration files for os or services that run on top of operating system.
/home: home directory of each regular users. Each regular user has their own home directory of each regular user. 
/lib: a directory that contains library files required by the system. For example, dynamic library files required by a program in /bin or /sbin exsit in /lib directory
/opt: a directory that holds software packages
/proc: various files and subdirectories, that represent processes and kernel related resources allowing users and applications to be accessible for related data
/root: the root user’s home directory
/sbin: as in /bin directory, this directory contains basic user commands and programs. Expecially contains commands and programs to the root user.
/tmp: this directory can be used when a user or program needs to create files temporarily. Files that have existed in this directory for a long time are automatically deleted.
/usr: a directory that contains user binaries, documentation, libraries, header files and more
/var directory utilized when a program or system needs to use and store files that are variable in real time. /var/log for example, stoes various log files. 