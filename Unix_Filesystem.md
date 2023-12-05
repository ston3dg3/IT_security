# This file explains how the Unix filesystem works

The graph below represets how the folders are structured starting from the root folder **/**

![I think there was supposed to be an image here...](Unix_Filesystem_Graph.png "Unix Filesystem")


There are many ways to traverse this filesystem. Here are some helpful commands:

1. `cd <directory>` - changes current directory
2. `ls` - displays contents of the current folder
    - flag **-l** list format
    - flag **-a** list all files (including hidden)
    - flag **-h** human readable file size format
    - flag **-t** sort list by time of modification


There are many ways to display contents of a file:
1. `cat`
2. `less`
3. text editor like `nano` or `vim`
4. `tail -n <int>` - display last **int** lines of a file 
5. `head -n <int>` - display first **int** lines of a file

To Find old forgotten files in the filesystem you could use:
```find /path/to/search -type f -name '*Uni*'```
this will return the absolute paths to each FILE (flag **-f**) that has the word **Uni** in its name. The stars mean that any character combination can come before and after the word.
In order to clean out the input from files which you have no permission to, do:
```find /path/to/search -type f -name '*Uni*' 2>/dev/null```
The 2 stands for output from command find that is classified as ERROR. For more info on output types, see the file [Unix_Bash_Parser]. So every output from the find command which returns an error, will be sent to the file /dev/null where it will get deleted. 