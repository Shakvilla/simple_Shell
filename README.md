# siple_shell
# README.md
# ALX Simple Shell Team Project
> This is an ALX collaboration project on Shell. We were tasked to create a simple shell that mimics the Bash shell. Our shell shall be called hsh
> 
## Project was completed using
- C language
- Shell
- Betty linter

## General Requirement for project

- All files will be compiled on Ubuntu 20.04 LTS using gcc, using the options -Wall -Werror -Wextra -pedantic -std=gnu89
- All files should end with a new line
- A [README.md](http://readme.md/) file, at the root of the folder of the project is mandatory
- Use the Betty style. It will be checked using [betty-style.pl](http://betty-style.pl/) and [betty-doc.pl](http://betty-doc.pl/)
- Shell should not have any memory leaks
- No more than 5 functions per file
- All header files should be include guarded
- Write a README with the description of the project

## Description

**hsh** is a simple UNIX command language interpreter that reads commands from either a file or standard input and executes them.

### How **hsh** works

- Prints a prompt and waits for a command from the user
- Creates a child process in which the command is checked
- Checks for built-ins, aliases in the PATH, and local executable programs
- The child process is replaced by the command, which accepts arguments
- When the command is done, the program returns to the parent process and prints the prompt
- The program is ready to receive a new command
- To exit: press Ctrl-D or enter "exit" (with or without a status)
- Works also in non interactive mode

### Compilation
`gcc -Wall -Werror -Wextra -pedantic -std=gnu89 *.c -o hsh`
### Invocation
Usage: **hsh** [filename]
To invoke **hsh**, compile all `.c` files in the repository and run the resulting executable.
**hsh** can be invoked both interactively and non-interactively. If **hsh** is invoked with standard input not connected to a terminal, it reads and executes received commands in order.
Example:
```
$ echo "echo 'hello'" | ./hsh
'hello'
$
```
If **hsh** is invoked with standard input connected to a terminal (determined by [isatty](https://linux.die.net/man/3/isatty)(3)), an *interactive* shell is opened. When executing interactively, **hsh** displays the prompt `$`  when it is ready to read a command.
Example:
```
$./hsh
$
```
Alternatively, if command line arguments are supplied upon invocation, **hsh** treats the first argument as a file from which to read commands. The supplied file should contain one command per line. **hsh** runs each of the commands contained in the file in order before exiting.

Example:
```
$ cat test
echo 'hello'
$ ./hsh test
'hello'
$
```
### Environment
Upon invocation, **hsh** receives and copies the environment of the parent process in which it was executed. This environment is an array of *name-value* strings describing variables in the format *NAME=VALUE*. A few key environmental variables are:

### HOME

The home directory of the current user and the default directory argument for the **cd** builtin command.
```
$ echo "echo $HOME" | ./hsh
/home/projects
```
### PWD
The current working directory as set by the **cd** command.

```
$ echo "echo $PWD" | ./hsh
/home/projects/alx/simple_shell

```
### OLDPWD
The previous working directory as set by the **cd** command.

```
$ echo "echo $OLDPWD" | ./hsh
/home/projects/alx/printf

```
### PATH
A colon-separated list of directories in which the shell looks for commands. A null directory name in the path (represented by any of two adjacent colons, an initial colon, or a trailing colon) indicates the current directory.

```
$ echo "echo $PATH" | ./hsh
/home/projects/.cargo/bin:/home/projects/.local/bin:/home/projects/.rbenv/plugins/ruby-build/bin:/home/projects/.rbenv/shims:/home/projects/.rbenv/bin:/home/projects/.nvm/versions/node/v10.15.3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/projects/.cargo/bin:/home/projects/workflow:/home/projects/.local/bin
```
### Command Execution
After receiving a command, **hsh** tokenizes it into words using `" "` as a delimiter. The first word is considered the command and all remaining words are considered arguments to that command. **hsh** then proceeds with the following actions:
1. If the first character of the command is neither a slash (`\\`) nor dot (`.`), the shell searches for it in the list of shell builtins. If there exists a builtin by that name, the builtin is invoked.
2. If the first character of the command is none of a slash (`\\`), dot (`.`), nor builtin, **hsh** searches each element of the **PATH** environmental variable for a directory containing an executable file by that name
3. If the first character of the command is a slash (`\\`) or dot (`.`) or either of the above searches was successful, the shell executes the named program with any remaining given arguments in a separate execution environment.
### Exit Status
**hsh** returns the exit status of the last command executed, with zero indicating success and non-zero indicating failure.
If a command is not found, the return status is `127`; if a command is found but is not executable, the return status is 126.
All builtins return zero on success and one or two on incorrect usage (indicated by a corresponding error message).
### Signals
While running in interactive mode, **hsh** ignores the keyboard input `Ctrl+c`. Alternatively, an input of end-of-file (`Ctrl+d`) will exit the program.
User hits `Ctrl+d` in the third line.
```
$ ./hsh
$ ^C
$ ^C
$
```
### Variable Replacement
**hsh** interprets the `$` character for variable replacement.
### $ENV_VARIABLE
`ENV_VARIABLE` is substituted with its value.
Example:
```
$ echo "echo $PWD" | ./hsh
/home/projects/alx/simple_shell
```


