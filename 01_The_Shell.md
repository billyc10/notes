# The Shell

## Introduction
A shell is a program which allows the user to interact with the OS, oftening using a command line interface. A program which may display a shell is a terminal. A majority of Windows operating systems use PowerShell as the command shell, whilst a majority of linux OS use Bash.

As I am currently writing this on a Windows machine, it will be a predominantly PowerShell focused.

## Using Powershell
The terminal uses presents the user with a *prompt*, usually telling you your current working directory such as: 

```powershell
C:\Users\Billy\>
```

The user will then type a *command* followed by *arguments*. For example:

```powershell
C:\Users\Billy\> echo hello world
```

will execute the command `echo` which simply prints the given arguments (white-space separated) `hello` and `world`.

```powershell
C:\Users\Billy\> echo hello world_
hello
world
```
### Environment Variables
djkdfj

### Navigation



## Tips and Tidbits
### Escaping Characters
You can escape characters such as spaces by wrapping the argument in a quote: `echo "Hello World"` or by using the shell-specific escape character (`` ` `` for powershell, `\` for bash)