# Terminal-Shell-Commands-Cheat-Sheet

This note comes in handy before exam.

### Contents 
- [Basic Terminal Commands](#Basic-Terminal-Commands)
  - [File permission](#File-permission)
- [Shell Scripts](#Shell-Scripts)
  
## Basic Terminal Commands
| Type | Command | Description |
|------|--------|-------------|
| Directory | pwd  | Print current working directory |
|| cd path | Change current working directory |
|| mkdir name | make a new directory |
|| ls (-la) | List files and subdirectories (with long format including hidden files) |
|| rm -r name  | Remove/Delete the directory. Carefully use it, as you cannot undo especially with -f option (force). |
|| open . | open the current working directory |
|| open ~ | open home directory |
| File|cp from to| Copy “from” to “to” |
|| mv from to | Rename/Move “from” to “to” |
|| cat file | Print the content of the file.|
||sh file|run all the command in file|
|| touch file | Create file without content.|
|| vi/emacs file | Open file using vi or [emacs](https://www.gnu.org/software/emacs/download.html)|
|| diff file_1 file_2 | Return difference between file_1 and file_2 |
|Permission| chmod option filename | Change the read, write, and execute permissions option - 3 digit for user, group, and others. Each digit is a sum of 4, 2, 1, or 0. (4: read, 2: write, 1: execute, and 0 : no permission) |
|Process | ps aux | Print all process of all users |
|| kill pid | kills the processes with pid |


```bash
vi ./eg.txt           # open file
i        # insert mode
:wq        # Quit
```

#### File Permissions

| # | Permission              | rwx | Binary |
| - | -                       | -   | -      |
| 7 | read, write and execute | rwx | 111    |
| 6 | read and write          | rw- | 110    |
| 5 | read and execute        | r-x | 101    |
| 4 | read only               | r-- | 100    |
| 3 | write and execute       | -wx | 011    |
| 2 | write only              | -w- | 010    |
| 1 | execute only            | --x | 001    |
| 0 | none                    | --- | 000    |

For a directory, execute means you can enter a directory.

| User | Group | Others | Description                                                                                          |
| -    | -     | -      | -                                                                                                    |
| 6    | 4     | 4      | User can read and write, everyone else can read (Default file permissions)                           |
| 7    | 5     | 5      | User can read, write and execute, everyone else can read and execute (Default directory permissions) |

- u - User
- g - Group
- o - Others
- a - All of the above

```bash
ls -l /foo.sh            # List file permissions
chmod +100 foo.sh        # Add 1 to the user permission
chmod -100 foo.sh        # Subtract 1 from the user permission
chmod u+x foo.sh         # Give the user execute permission
chmod g+x foo.sh         # Give the group execute permission
chmod u-x,g-x foo.sh     # Take away the user and group execute permission
chmod u+x,g+x,o+x foo.sh # Give everybody execute permission
chmod a+x foo.sh         # Give everybody execute permission
chmod +x foo.sh          # Give everybody execute permission
```
## Shell Scripts
- [Variable Assignment](#Variable-Assignment)
- [Loops](#loops)
- [Conditional Statement](#Conditional-Statement)
  - [File test operators](#File-test-operators)
  - [Integer comparison](#Integer-comparison)
  - [String comparison](#String-comparison)
  - [Compound comparison](#Compound-comparison)

### Variable Assignment
Use =(equal sign) w/o any space
To refer to a variable after assignment, add $
```bash
num=1 # same with: export file_name='ex03_input.sh'
echo $num
a=`echo Hello!`   # Assigns result of 'echo' command to 'a' ...
echo $a
a=`ls -l`         # Assigns result of 'ls -l' command to 'a'
echo $a           # Unquoted, however, it removes tabs and newlines.
echo
echo "$a"         # The quoted variable preserves whitespace.
```

### Loops
| Type | Syntax |Short|
|------|--------|----|
|for loops|for arg in [list_of_items_separately_by_space]<br />  do<br />    commands<br />done|for arg in [list] ; do<br /> [Example operating on a file list contained in a variable](#Example-operating-on-a-file-list-contained-in-a-variable) |
|while|while [ condition ]<br />do<br />  command(s)<br />done|while [ condition ] ; do|
|until|until [ condition-is-true ]<br />do<br />  command(s)<br />done|until [ condition-is-true ] ; do|
Iteration: Repeated execution of a command or group of commands, usually -- but not always, while a given condition holds, or until a given condition is met.

##### Example operating on a file list contained in a variable
```bash
#!/bin/bash
# fileinfo.sh
# List of files you are curious about. # Threw in a dummy file, /usr/bin/fakefile.

echo

for file in $FILES
do
  if [ ! -e "$file" ]       # Check if file exists.
  then
    echo "$file does not exist."; echo
    continue                # On to next.
   fi
  ls -l $file | awk '{ print $8 "         file size: " $5 }'  # Print 2 fields.
  whatis `basename $file`   # File info.
  # Note that the whatis database needs to have been set up for this to work.# To do this, as root run /usr/bin/makewhatis.
  echo
done  
```
##### Example Operating on a parameterized file list
```bash
#!/bin/bash

filename="*txt"
for file in $filename
do
 echo "Contents of $file"
 echo "---"
 cat "$file"
 echo
done
```
##### Example Operating on files with a for loop
```bash
#!/bin/bash
# list-glob.sh: Generating [list] in a for-loop, using "globbing" ... # Globbing = filename expansion.

echo

for file in *
#           ^  Bash performs filename expansion
#+             on expressions that globbing recognizes.
do
  ls -l "$file"  # Lists all files in $PWD (current directory).
  #  Recall that the wild card character "*" matches every filename,
  #+ however, in "globbing," it doesn't match dot-files.

  #  If the pattern matches no file, it is expanded to itself.
  #  To prevent this, set the nullglob option
  #+   (shopt -s nullglob).
  #  Thanks, S.C.
done

echo; echo

for file in [jx]*
do
  rm -f $file    # Removes only files beginning with "j" or "x" in $PWD.
  echo "Removed file \"$file\"".
done

echo
```


### Conditional Statement
if [condition]
    then commands
elif [condition]
    then commands
done
#### read splitted string to arr
```bash
string="Lelouch,Akame,Kakashi,Wrath"  # Given string 

IFS=','        # Setting IFS (input field separator) value as ","

read -ra arr <<< "$string"    # Reading the split string into array

for val in "${arr[@]}";       # Print each value of the array by using the loop
do
  printf "name = $val\n"
done
```

#### File test operators
| Comparison Operators | Description |Note|
|----------------------|-------------|----|
|-e|file exists||
|-a|file exists|This is identical in effect to -e. It has been "deprecated," and its use is discouraged.|
|-f|file is a regular file (not a directory or device file)||
|-s|file is not zero size||
|-d|file is a directory||
|-b|file is a block device|if [ -b "$device0" ]<br />then<br />  echo "$device0 is a block device."<br />fi|
|-c|file is a character device|device1="/dev/ttyS1"   # PCMCIA modem card.<br />if [ -c "$device1" ]<br />then<br />  echo "$device1 is a character device."<br />fi|
|-p|file is a pipe|function show_input_type()<br />{<br />   [ -p /dev/fd/0 ] && echo PIPE | echo STDIN<br />}<br />show_input_type "Input"                           # STDIN<br />echo "Input" | show_input_type                    # PIPE|
|-h|file is a symbolic link||
|-S|file is a [socket](https://tldp.org/LDP/abs/html/devref1.html#SOCKETREF)||
|-t|file (descriptor) is associated with a terminal device|This test option may be used to check whether the stdin [ -t 0 ] or stdout [ -t 1 ] in a given script is a terminal.|
|-r|file has read permission (for the user running the test)||
|-w|file has write permission (for the user running the test)||
|-x|file has execute permission (for the user running the test)||
|-g|set-group-id (sgid) flag set on file or directory||
|-u|set-user-id (suid) flag set on file||
|-k|sticky bit set||
|-O|you are owner of file||
|-G|group-id of file same as yours||
|-N|file modified since it was last read||
|f1 -nt f2|file f1 is newer than f2||
|f1 -ot f2|file f1 is older than f2||
|f1 -ef f2|files f1 and f2 are hard links to the same file||
|!|"not" -- reverses the sense of the tests above (returns true if condition absent).||


#### Integer comparison
| Comparison Operators | Description | Example |Note|
|----------------------|-------------|---------|----|
|-eq|is equal to | if [ "$a" -eq "$b" ]||
|-ne|is not equal to | if [ "$a" -ne "$b" ]||
|-gt|is greater than | if [ "$a" -gt "$b" ]||
|-ge|is greater than or equal to | if [ "$a" -ge "$b" ]||
|-lt|is less than | if [ "$a" -lt "$b" ]||
|-le|is less than or equal to | if [ "$a" -le "$b" ]||
|<|is less than (within [double parentheses]()) | (("$a" < "$b"))||
|<=|is less than or equal to (within double parentheses)|(("$a" <= "$b"))||
|>|is greater than (within double parentheses)|(("$a" > "$b"))||
|>=|is greater than or equal to (within double parentheses)|(("$a" >= "$b"))||

#### String comparison
| Comparison Operators | Description | Example |Note|
|----------------------|-------------|---------|----|
|=|is equal to|if [ "$a" = "$b" ]| Note the whitespace framing the =. <br />if [ "$a"="$b" ] is not equivalent to the above.|
|==|is equal to|if [ "$a" == "$b" ]|This is a synonym for =.<br />The == comparison operator behaves differently within a [double-brackets test](#double-brackets-test) than within single brackets.|
|!=|is not equal to|if [ "$a" != "$b" ]|This operator uses pattern matching within a [[ ... ]] construct.|
|<|is less than, in ASCII alphabetical order|if [[ "$a" < "$b" ]] if [ "$a" \< "$b" ]|Note that the "<" needs to be escaped within a [ ] construct.|
|>|is greater than, in ASCII alphabetical order|if [[ "$a" > "$b" ]] if [ "$a" \> "$b" ] |Note that the ">" needs to be escaped within a [ ] construct.|
|-z|string is null, that is, has zero length|if [ -z "$String" ]||
|-n|string is not null||The -n test requires that the string be quoted within the test brackets. <br />Using an unquoted string with ! -z, or even just the unquoted string alone within test brackets (see Example 7-6) normally works, however, this is an unsafe practice. Always quote a tested string. [1]|

#### Compound comparison
| Comparison Operators | Description | Example |Note|
|----------------------|-------------|---------|----|
|-a|logical and|exp1 -a exp2 returns true if both exp1 and exp2 are true.|These are similar to the Bash comparison operators && and ||, used within double brackets. [[ condition1 && condition2 ]]|
|-o|logical or|exp1 -o exp2 returns true if either exp1 or exp2 is true.|The -o and -a operators work with the test command or occur within [single test brackets](#single-test-brackets).|

#### double-brackets test
```bash
[[ $a == z* ]]   # True if $a starts with an "z" (pattern matching).
[[ $a == "z*" ]] # True if $a is equal to z* (literal matching).

[ $a == z* ]     # File globbing and word splitting take place.
[ "$a" == "z*" ] # True if $a is equal to z* (literal matching).
```
#### single test brackets
```bash
if [ "$expr1" -a "$expr2" ]
then
  echo "Both expr1 and expr2 are true."
else
  echo "Either expr1 or expr2 is false."
fi
```

#### Arithmetic and string comparisons
```bash
#!/bin/bash
a=4
b=5

#  Here "a" and "b" can be treated either as integers or strings.
#  There is some blurring between the arithmetic and string comparisons,
#+ since Bash variables are not strongly typed.

#  Bash permits integer operations and comparisons on variables
#+ whose value consists of all-integer characters.
#  Caution advised, however.

echo

if [ "$a" -ne "$b" ]
then
  echo "$a is not equal to $b"
  echo "(arithmetic comparison)"
fi

echo

if [ "$a" != "$b" ]
then
  echo "$a is not equal to $b."
  echo "(string comparison)"
  #     "4"  != "5"
  # ASCII 52 != ASCII 53
fi

# In this particular instance, both "-ne" and "!=" work.

echo

exit 0
```


# Reference 
- [Stanford](https://mally.stanford.edu/~sr/computing/basic-unix.html)
- [Bash Cheat Sheet](https://github.com/RehanSaeed/Bash-Cheat-Sheet)
- [tldp](https://tldp.org/LDP/abs/html/varassignment.html)
- [loop](https://tldp.org/LDP/abs/html/loops.html)
- [conditional](https://tldp.org/LDP/abs/html/tests.html)
- [Split a string](https://www.geeksforgeeks.org/shell-script-to-split-a-string/)
