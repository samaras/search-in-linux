# How to search for files in Linux

### Introduction

This guide was created to help you understand how to search for files in a Linux system. The tutorial was based on a Linux Mint 18.3 installation.
Linux uses different commands to help you look for files using regular expressions. Imagine looking for a file, but don't dont remember 
much except a small part of the name of the file. 

## Prerequisites

Before you begin this guide you'll need the following:

- Desktop/laptop/server with Ubuntu/Linux Mint installed or any other Linux distribution really.

## Step 1 — Using the grep command

Grep is used to search for characters inside files. It also has support for regular expressions. The family of grep commands include the 
following commands: grep, egrep, fgrep.
egrep and grep are more or less the same with just minor differences. fgrep uses fixed strings to extract text from files. Grep can also take in input from the standard input.

Search for pattern in files:
```
$ grep [options] pattern files
```

The commands will read each line one at time.

* `\*` will match any character
* `?` or `.`  will only match a single character
* `[xxx]` will match a range of characters 
* `\x` will escape characters such as ? or \ which are used in other regexp
* `^pattern` will match anything that starts with pattern 
* `$pattern` will match anything that ends with pattern


Search recursively for pattern in the home directory: 
```
$ grep -r pattern /home
```

Display the _find_ commands in your bash_history with the following:

```command
grep -a find .bash_history
```

You'll see the following output:

```
[Output]

find /home -size +10000k
man find
man find > find.txt
gedit find.txt
find -type f -mtime -1 -print

```

Now transition to the next step by telling the reader what's next.

## The find command

The find command is a slow but reliable option if you are searching for a file in your system. If the file exists, find is more likely to find it eventually. It has the following format:

`find [path] [options]`

$ find /home/tom -name 'index*'
Find can used to search for files in choosen directories. You can specify a list of directories to search in. 
The find command can also search using the following criteria: 
* Search by size of the file
* Search by name of the file 
* Search by permission of the files -
* Search by group the file owner belongs to -gid 
* Search by the owner of the file -uid & -user
* Search by type    -type
    The possible arguments to -type are b{block device}, c {character device}, d {directory}, p {named pipe}, f {regular file}, l {symbolic link}


Find files names that start with "FB":
```
find /home -name 'FB*'
```

Find files that are larger than 10 MB in the home folder:
```
find /home -size +10000k
```

Find files by the type of permission allowed:
```
$ find /home -perm 
```

Find files by the group of the owner of the file belongs to:

```
$ find -user root
```
<$>[warning]
**Warning:** The -user option will only work if the user exists and is not removed. Use the -uid argument to reference the user. 
<$>

The -maxdepth levels option is used to limit how far deep the find command will search in the subdirectories.

The -exec options allows you to execute other commands on the files found. The following will remove any .tmp files found in the **home** directory.

```
find /home -name "*.tmp" -exec rm {} \;
```

To list files of a certain type which have also been accessed in the past 24 hours you can run the follwoing command:

```
find /home -mtime -1 \! -type d
```

The -type options specifies the type of file. In this case a directory. You add a \! to list any file type except a directory. 


## Locate and the whereis command

The `whereis` command is used to search for files in the systems path. 

```
whereis elixir 
```


The `locate` command is for finding files using just a simple string or filename. The command assumes you have called the `updatedb`. The `updatedb` command creates or updates a database file which has all the files attached to the linux root directory. Locate will use this database to give you the location of the file.


Find all instances of filename:
```
$ locate filename
```

## ls command

The `ls` command is used to list the contents of the current working directory. Its one of the most used linux commands. 

The most common options used with `ls` are the -a and -l options.
The `ls -a` command is used to list all files including hidden files, while the `ls -l` command is for listing all files with all their size, permissions and ownership information. You can also use special characters to abbreviate file names, this is called 'globbing'. It should not be confused with regular expressions which is used in the grep command. 

```
ls -l filename
```

To list all text files in the directory run:
```
$ ls *.txt
```

The following will list any file that has a number as its name including the extension. If you omit the last star at the end, only 
files ending with numbers will be listed e.g `ls *[0-9]`.  
```
$ ls *[0123456789]*
$ ls *[0-9]*
```

I got the following output on my home folder:

```
[digit_files Output]
buffalo_0.18.2_Linux_x86_64.tar.gz
Calculus Made Easy33283-pdf.pdf
ccenh.mp4
Cogele El Golpe - King Bongo [360p].mp4
com.keramidas.virtual.WIFI_AP_LIST-20190908-150043.tar.gz
COR14.3 (7).pdf
De Mthuda - Wamuhle ft Njelic.mp3
DesignPatternsInPython_ver0.1.pdf
design-principles4.pdf
Django 2.0 release notes _ Django documentation _ Django.pdf
djangoadvent-articles06_object-permissions.rst
DomAC6HW0Accg_6.jpg
Dxmd1ybWsAEIfc8.jpg
ejabberd 2.1.pdf
FB_IMG_1431202962739.jpg
FB_IMG_1450904477927.jpg
FB_IMG_1458068934007.jpg
```

```
$ ls *[-a-z][0-9]*
```

```
ls FB_IMG_1*[0-9]*.jpg
```

I got the following output on my home folder:

```
[secondary_label Output]

FB_IMG_1431202962739.jpg
FB_IMG_1450904477927.jpg
FB_IMG_1458068934007.jpg
```
I could also have gotten the same output using an asterik representing any zero or more characters after the **'FB_IMG_'** string. 

```
ls FB_IMG_*
```

You can escape characters with a slash "\"
```
ls *\?*  
```


## Nugget

You can search the manual pages for help in looking for a particular name and description using the `apropos` command. This will search the whatis database for files that have the specified string.

```
$ apropos search
```
The following output lists all the commands I can use to 'search' on my computer.

```
[Output]
ALTER_TEXT_SEARCH_CONFIGURATION (7) - change the definition of a text search configuration
ALTER_TEXT_SEARCH_DICTIONARY (7) - change the definition of a text search dictionary
ALTER_TEXT_SEARCH_PARSER (7) - change the definition of a text search parser
ALTER_TEXT_SEARCH_TEMPLATE (7) - change the definition of a text search template
anemotaxis (6x)      - directional search on a plane.
apropos (1)          - search the manual page names and descriptions
badblocks (8)        - search a device for bad blocks
bsearch (3)          - binary search of a sorted array
bzegrep (1)          - search possibly bzip2 compressed files for a regular expression
bzfgrep (1)          - search possibly bzip2 compressed files for a regular expression
bzgrep (1)           - search possibly bzip2 compressed files for a regular expression
CREATE_TEXT_SEARCH_CONFIGURATION (7) - define a new text search configuration
CREATE_TEXT_SEARCH_DICTIONARY (7) - define a new text search dictionary
CREATE_TEXT_SEARCH_PARSER (7) - define a new text search parser
CREATE_TEXT_SEARCH_TEMPLATE (7) - define a new text search template
docker-search (1)    - Search the Docker Hub for images
DROP_TEXT_SEARCH_CONFIGURATION (7) - remove a text search configuration
DROP_TEXT_SEARCH_DICTIONARY (7) - remove a text search dictionary
DROP_TEXT_SEARCH_PARSER (7) - remove a text search parser
DROP_TEXT_SEARCH_TEMPLATE (7) - remove a text search template
FcConfigGetCacheDirs (3) - return the list of directories searched for cache files
find (1)             - search for files in a directory hierarchy
git-bisect (1)       - Use binary search to find the commit that introduced a bug
hsearch (3)          - hash table management
hsearch_r (3)        - hash table management
lfind (3)            - linear search of an array
lsearch (3)          - linear search of an array
lzegrep (1)          - search compressed files for a regular expression
lzfgrep (1)          - search compressed files for a regular expression
lzgrep (1)           - search compressed files for a regular expression
manpath (1)          - determine search path for manual pages
res_nsearch (3)      - resolver routines
res_search (3)       - resolver routines
strpbrk (3)          - search a string for any of a set of bytes
tsearch (3)          - manage a binary tree
wcschr (3)           - search a wide character in a wide-character string
wcscspn (3)          - search a wide-character string for any of a set of wide characters
wcspbrk (3)          - search a wide-character string for any of a set of wide characters
wcsrchr (3)          - search a wide character in a wide-character string
wmemchr (3)          - search a wide character in a wide-character array
wx-config (1)        - wxWidgets configuration search and query tool
XtFindFile (3)       - search for a file using substitutions in the path list
XtResolvePathname (3) - search for a file using standard substitution
xzegrep (1)          - search compressed files for a regular expression
xzfgrep (1)          - search compressed files for a regular expression
xzgrep (1)           - search compressed files for a regular expression
zegrep (1)           - search possibly compressed files for a regular expression
zfgrep (1)           - search possibly compressed files for a regular expression
zgrep (1)            - search possibly compressed files for a regular expression
zipgrep (1)          - search files in a ZIP archive for lines matching a pattern

```


## Conclusion

The article introduced the most frequently used command to search for files in linux. I hope you learned something new by reading this article,
and can now fully embrace the power of some the commands listed here. For more information on the commands there is the man pages which will provide more technical 
information. The `ls` command alone has over 70 options.


<!------------ Formatting ------------------------->

<!-- 
Learn about formatting commands and terminal output at https://do.co/style#code

Key presses should be written in ALLCAPS with in-line code formatting: `ENTER`.

Use a plus symbol (+) if keys need to be pressed simultaneously: `CTRL+C`.

This is a <^>variable<^>.

This is an `<^>in-line code variable<^>`

Learn more about how to use variables to highlight important items at https://do.co/style#variables

![Alt text for screen readers](/path/to/img.png)

Learn more about images at https://do.co/style#images-and-other-assets
-->
