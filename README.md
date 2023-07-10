# Terminal-Shell-Commands-Cheat-Sheet

A cheat sheet for shell commands.

### Contents 
- [Basic Terminal Commands](#Basic-Terminal-Commands)
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

## File Permissions

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
coming soon

# Reference 
[Stanford](https://mally.stanford.edu/~sr/computing/basic-unix.html)
[Bash Cheat Sheet](https://github.com/RehanSaeed/Bash-Cheat-Sheet)
[tldp](https://tldp.org/LDP/abs/html/varassignment.html)
[loop](https://tldp.org/LDP/abs/html/loops.html)
[conditional](https://tldp.org/LDP/abs/html/tests.html)
