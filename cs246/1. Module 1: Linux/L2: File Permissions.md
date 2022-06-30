# File Permissions 

``` bash 
ls shows all files (non-hidden) 
ls -a shows all files (including hidden) 
ls -al or ls -l shows the long form of directories 
```

The cmd `ls -al` gives the "long form" listing of all files in the directory (cwd) 

**The format for file permission display:**
``` bash 
-rw--r----- g7guo cs246 25 Sep 9 15:27 abc.txt
type/perms  owner group size  modified filename
```

- ***Type***: "`-`" for a *regular file* (*Ex*: `-rw-rwxr--`), "`d`" for a *directory* (*Ex*. `drw-rwxr--`)
- ***Permissions***: 3 groups of 3 bits (`rwxrwxrwx`)
  - *User bits*: apply to the file's owner 
  - *Group bits*: apply to the members of the file's group 
  - *Other bits*: apply to everyone else 
- ***Groups***: a user can belong to one or more groups, a file can be associated with one group


`r` - read bit, `w` - write bit, `x` - execute bit 

| Bit | Meaning for ordinary files | Meaning for directories
|---|------|---|
|`r`| File's contents can be read|Directory's contents can be read (e.g `ls`, globbing, text completion)|
|`w`|File's contents can be modified| Directory's contents can be modified (add/remove files)|
|`x`| File's contents can be executed as a program| Directory can be navigated (e.g can `cd` to it)|

Note: If a directory's executable bit is not set, then there is no access to that directory, nor to any file within it regardless of what other bits are set. 

## Changing permissions: 
```bash 
chmod <mode> <file>
```

The `<mode>` consists of three parts: 

|User types|Operators|Permissions|
|---|---|---|
|<ul><li>`u` = user (owner)<li>`g` = group<li>`o` = other<li>`a` = all<ul>|<ul><li>`+` = add permissions<li>`-` = subtract permissions<li>`=` = set permissions exactly<ul>|<ul><li>`r` = read<li>`w` = write<li>`x` = execute<ul>|

*Example* - Give others permission to read file: `chmod o+r <file>`

*Example* - Make everyone's permission to read and execute: `chmod a=rx <file>`

*Example* - Give owner full control: `chmod u+rwx <file>` or `u=rwx`

# Shell Scripts 
> **Shell scripts**: files containing a sequence of shell commands, executed as a program 

*Ex*. Print the date, current user and current directory 

**In a text file (program):**

~~~BASH
#!/bin/bash --> Shebang Line
date 
whoami
pwd
~~~

`#!/bin/bash` is the **Shebang line**: tells the OS `/bin/bash` is the location of the program you should use to interpret this file 

Then, give the file executable permissions: `chmod u+x myscript`

Execute the script: `./myscript`

> `.` represents the current directory 
> 
> `..` represents one directory higher than the current directory 
> > So `cd ../` goes up one directory

# Variables

*Example*.
```bash 
x=1
``` 
NOTE: No Spaces in between!

```bash 
echo $x
> 1
```

Notes: 
- `$`: fetches the value of → use `$` when fetching yje values of a variable
- No `$` when setting a variable 
- Good practice: `${x}` brace brackets
- All variables in BASH are ***strings*** (Ex. `x` contains the string "`1`")

~~~BASH
$dir = ./s22cs246 -- set dir as a variable
$echo $dir
> home/rob 

$ls $dir
> contents of my s22cs246 dir 
~~~

Note: `$` Expansion occur in double quotes `""` but not single quotes `''`

| Notation | Meaning |
|---|---|
| `$1`, `$2`, ..., etc | ***Special vars***: cooresponding cmd line args |
| `$?` | status of most recently executed command |
| `-eq` | arithmetic equality → treats variables like numbers |

*Example*. Check whether a word is in the dictionary or not: 
```bash 
./isItaWord hello
```

*Example*. Prints nothing if word is not found, otherwise, prints the word: 

~~~BASH
#!/bin/bash
egrep "n${1}$ /usr/share/dict/words
~~~

*Ex*. A good password should not be in the dictionary; answers whether a password is guarenteed bad or not 

~~~ bash 
egrep "^${1}$ /usr/share/dict/words > /dev/null

# Use '#' to comment in BASH
# Note: redirecting stout to /dev/null suppresses output
# Note: every program returns a status code to the OS when finished 
~~~ 

~~~ bash 
if [ $? -eq 0 ]; then 
	echo Bad password 
else 
	echo Maybe not a terrible password 
fi	
~~~

*Example*. Verify that there is a correct number of arguments, prints how to use the program otherwise 

~~~ bash 
#!/bin/bash 

usage() { # good idea to make usage a function in case you need to print in multiple places 
	echo "usage; ${0} password (and ${1})"}
	
if [ $# -ne 1 ]; then 
	usage
	exit 1
fi	
~~~

Be careful with spacing! There must be a blank space between the opening square bracket and the condition being tested, and a blank space between the condition and the closing square bracket.

# General Form of Statement 
General form of an `if` in bash 
```bash 
if <condition1>; then # use semicolon to separate "then"
	<statements>
elif <condition2>; then
	<statements>
elif <condition3>; then
	...
else
	<statements>
fi
```

For all available comparison and other conditions, see the LINUX reference sheet provided 

*Example*. Loops - print numbers from `1` to `$!`

~~~ bash 
#!/bin/bash
# usage ...
x=1
while [ $x -le $1 ]; do
	echo $x
	x=$((x+1)) #$((...)) is syntax for arithmetics 
done 	
~~~

Note: the special syntax `$(( ... ))` is used to tell the shell that the information within the doubled-parentheses are to be treated as integers (by default bash treats values as strings).

*Example*. Looping over a list 

*Example 1*: 

~~~ bash 
for x in a b c do echo $x; done 

> a
> b
> c
~~~

~~~ bash 
for x in abc; do echo $x; done 

> a
> b
> c
~~~

~~~ bash 
for x in "abc"; do echo $x; done 

> abc
~~~

*Example 2*: 

~~~ bash 
for x in $(ls) do echo "File: $x"; done 

> File: HelloScript 
> File: alice.txt
> File: myfile.txt
~~~

~~~ bash 
for x in "$(ls)" do echo "File: $x"; done 

> File: HelloScript
> alice.txt
> myfile.txt
~~~
