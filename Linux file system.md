
inode metadata, each file having its own inode. 
no files can have 0 or more than one inode.

inode consists of
- inode number
- file mode
- link numbers
- owner and group id
- file size and address
- final file access, modification info
- inode modifying info
- location of data saved
Reference count, refcount 는 inode 를 참조하고 있는 파일의 개수를 의미하고, number 는 node 의 고유 번호를 의미한다. if refcount is 0, program judges that there is no other objects access to the dedicated object, remove the object that refcount is 0. 

하드링크- 생성되는 파일의 inode 를 링크하고자 하는 파일에 직접 연결