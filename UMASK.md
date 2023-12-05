# This file explains what UMASK is in Unix based systems

A umask is a predefined value which determines what permissions to assign to newly created files and folders

To calculate the permissions, we need to subtract this umask from the default
permissions given to files and folders respectively.

**Warning: The umask values need to be 4 digits like: 0000 when setting a new umask**

The default umask value for normal user is usually `002`. 
The default umask value for root user is usually `02 2`.
The default file permissions upon creation are usualy `777`.
The default folder permissions upon creation are usually `666`.

The permissions are defined as follows:
```
0   all permissions  | rwx  
1   read and write   | rw- 
2   read, execute    | r-x
3   read only        | r--
4   write, execute   | -wx
5   write only       | -w-
6   execute only     | --x
7   no permissions   | ---
```
If the permission is set to high then it is disabled. That means a code like `123`
would correspond to permissions `--x -w- -wx` or `--x-w--wx`

Let's see an example with umasks. The permissions for newly created files will be calculated as follows:

#### Example umask = 002 (default group user)
```
                    file          |          folder
Before mask:  777   (-rwxrwxrwx)  |   666    (drw-rw-rw-) 
Operations : -002   (========w=)  |  -002    (========w=) 
After mask :  775   (-rwxrwxr-x)  |   664    (drw-rw-r--) 
```

#### Example umask = 022 (default root user)
```
                    file          |          folder
Before mask:  777   (-rwxrwxrwx)  |   666    (drw-rw-rw-)   
Operations : -022   (==========)  |  -022    (==========)   
After mask :  755   (-rwxr-xr-x)  |   644    (drw-r--r--)    
```