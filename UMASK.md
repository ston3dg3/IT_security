# This file explains what UMASK is in Unix based systems

A umask is a predefined value which determines what permissions to assign to newly created files and folders

To calculate the permissions, we need to subtract this umask from the default
permissions given to files and folders respectively.

**Warning: The umask values need to be 4 digits like: 0000 when setting a new umask**

The default umask value for normal user is usually `0002`. 
The default umask value for root user is usually `0022`.
The default file permissions upon creation are usualy `666`.
The default folder permissions upon creation are usually `777`.

The permissions for files and folders generally are defined as follows (for UMASK = 0):
```
7   all permissions  | rwx
6   read and write   | rw-
5   read, execute    | r-x 
4   read only        | r--
3   write, execute   | -wx 
2   write only       | -w-
1   execute only     | --x
0   no permissions   | ---
```
So a file with permissions 666 would have the perms: rw-rw-rw-


The permissions for the UMASK are defined a little differently:
```
Octal    permissions   files directory
7   all permissions  | ---      ---
6   read and write   | --x      --x
5   read, execute    | -w-      -w-
4   ---------        | -w-      -wx
3   write, execute   | r--      r--
2   ----------       | r--      r-x
1   execute only     | rw-      rw-
0   --------------   | rw-      rwx
```
In this case, 


Let's see an example with umasks. The permissions for newly created files will be calculated as follows:

#### Example umask = 002 (default group user)
```
                    folder        |            file
Before mask:  777   (drwxrwxrwx)  |   666    (-rw-rw-rw-) 
Operations : -002   (========w=)  |  -002    (========w=) 
After mask :  775   (drwxrwxr-x)  |   664    (-rw-rw-r--) 
```

#### Example umask = 022 (default root user)
```
                    folder        |            file
Before mask:  777   (drwxrwxrwx)  |   666    (-rw-rw-rw-)   
Operations : -022   (==========)  |  -022    (==========)   
After mask :  755   (drwxr-xr-x)  |   644    (-rw-r--r--)    
```

#### Example umask = 027 (default root user)
```
                    folder        |            file
Before mask:  777   (drwxrwxrwx)  |   666    (-rw-rw-rw-)   
Operations : -027   (==========)  |  -027    (==========)   
After mask :  750   (drwxr-x---)  |   640    (-rw-r-----)    
```

**Note that in the last example the file permissions are different than we could expect.** This is because we do not perform an arithmetic subtraction of the UMASK from the default permissions but rather subtract each octal value in the corresponding position. If the result of the subtraction is negative, it defaults to the lowest permissions possible, which is 0. So we have:
```
 6  6  6
-0 -2 -7
--------
 6  4  0
``` 
