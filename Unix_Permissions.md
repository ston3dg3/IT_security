# This is a file explaining the Unix permissions sytem


In Unix-like operating systems, file and directory permissions are represented by a series of characters that indicate the access rights for the **owner**, **group**, and **others**. When you use the `ls -la` command, you'll see a detailed listing that includes permission information. Let's break down a typical output:

```
-rw-r--r--  1 user user  12345 Dec  1 12:34 myfile

First character (either "-" or "d") specifies if the object is a file or directory
Next 3 characters are permissions for OWNER: rw-   corresponds to 6
Next 3 characters are permissions for GROUP: r--   corresponds to 4
Next 3 characters are permissions for OTHER: r--   corresponds to 4
The '1' tells us how mary hard links there are to that object
The next string specifies the owner of the file (here its "user")
The next string specifies the group of the file (here its also "user")
The next few integers (12345) specify the size of the data in Bytes
The next 3 fields (Dec 1 12:34) are the date of creation
The last field is the name of the object itself (file/folder)

The integer representation of this permission will be: 644
```

The default permissions for newly created files are determined using a [UMASK](UMASK.md). A umask is a predefined value (sequence of integers) which will be subtracted from the original default permission values (usually 777 for files and 666 for folders).

### In order to change the permissions, the commands below are available:

```chmod 777 <filename>``` - sets the permission block to 777 (rwxrwxrwx)
```chmod +x <filename>```  - adds execution rights to all 3 permission fields
```chmod u+rw,g+rx,o=r```  - add read/write to user, add read/execute to group, set others to read only

### To change the group of the object (file/folder):

```chgrp <new_group> <filename>```


### Collection of useful permission commands:D

To view the [UMASK](UMASK.md) set for the system type
```umask```


