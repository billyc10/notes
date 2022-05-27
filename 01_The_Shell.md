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
## Variables
Variables in PowerShell have the `$` prefix, for example
```PowerShell
PS C:\> $Day = "Tuesday"
PS C:\> $Day
Tuesday
```

Variables are available in the scope they're created. E.g. a variable created in a function will only be accessible in that function. You can use a scope modifier `$Scope:<variable-name>` to change this. Some scope modifiers are as follows:

- `global:`
- `local:`
- `<variable-namespace>` - A special modifier such as from the list below:

| Namespace  | Description |
| ---------- | ----------- |
| Env:       | Environment variables defined in the current scope |
| Function:  | Functions defined in the current scope |
| Alias:     | Aliases defined int the current scope  |
| Variable:  | Variables defined int the current scope  |

You can view all variables in a scope by using `Get-ChildItem`. The following example lists all environment variables:
```PowerShell
gci env:
```


## Environment Variables
Environment variables contain data used by the OS and other programs. A basic example would be `windir` which contains the location of the windows installation. Environment variables can be accessed and edited by the shell.

In PowerShell, environment variables are stored as a string and cannot be empty. They are inherited by child processes such as local background jobs. When you change environment variables (such as `PATH`) in PowerShell, it only affects the current session.

Use the variable syntax (`$`) to specify an environment variable
```PowerShell
$Env:<variable-name>
```

`PATH` is a special environment variable, in that it contains the locations of all the possible scripts and functions you may want to execute directly from the command line.

## Tips and Tidbits
### Escaping Characters
You can escape characters such as spaces by wrapping the argument in a quote: `echo "Hello World"` or by using the shell-specific escape character (`` ` `` for powershell, `\` for bash)