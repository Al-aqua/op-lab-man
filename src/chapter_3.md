# Chapter 3: Shell Scripting in Linux

## Introduction to Shell Scripting

Shell scripting allows you to automate repetitive tasks, manage system
operations, and customize your environment by writing scripts. Shell scripts
are written for the shell, or command line interpreter, which interprets and
executes commands.

### 1. What is a Shell Script?

A shell script is a text file containing a series of commands that the shell
can execute. Scripts can include commands, variables, loops, and conditionals
to perform complex tasks.

### 2. How write Your First Shell Script

**Step 1: Create a File**

Create a new file with a .sh extension ` touch first_script.sh` and open it
in any text editor.

**Step 2: Add the Shebang**

The shebang (#!) at the top of the file tells the system which interpreter to use. For bash, use:

```bash
#!/bin/bash
```

**Step 3: Add Commands**

```bash
#!/bin/bash
echo "Hello, World!"
```

**Step 4: Make the Script Executable**

`chmod +x first_script.sh`

**Step 5: Run the Script**

`./first_script.sh`

### 3. Variables

Variables store data that can be used later in the script.

```bash
#!/bin/bash
name="John Doe"
echo "My name is $name"
```

### 4. User Input

User input is data that is provided by the user when the script is run.

```bash
#!/bin/bash
read -p "Enter your name: " name
echo "Hello, $name!"
```

### 5. Conditional Statements

Conditional statements allow you to perform different actions based on different conditions.

```bash
#!/bin/bash
read -p "Enter a number: " num
if [ $num -gt 0 ]; then
    echo "$num is positive"
elif [ $num -lt 0 ]; then
    echo "$num is negative"
else
    echo "$num is zero"
fi
```

### 6. Loops

Loops allow you to repeat a block of code multiple times.

`for` Loops

```bash
#!/bin/bash
for i in {1..10}; do
    echo "Number: $i"
done
```

`while` Loops

```bash
#!/bin/bash
i=1
while [ $i -le 10 ]; do
    echo "Number: $i"
    i=$((i + 1))
done
```

### 7. Functions

Functions allow you to reuse code in your script.

```bash
#!/bin/bash
function greet() {
    echo "Hello, $1!"
}
greet "John Doe"
greet "Joe Mama"
```

### 8. Script Arguments

Script arguments are data passed to the script when it is run.

```bash
#!/bin/bash
echo "Script name: $0"
echo "Argument 1: $1"
echo "Argument 2: $2"
```

### 9. Practical Examples

Backup Script

```bash
#!/bin/bash
backup_dir="/backup"
mkdir -p $backup_dir
cp -r /home/user/documents $backup_dir
echo "Backup completed!"
```

> Exercise : <br/>
>
> 1. Write a script that output your user name, uptime.
> 2. Write a guessing game where a user has to guess a number between 1 and 10
>    and the script will tell the user if their guess is too high or too low.

<div class="warning">
<h3>Assignment : </h3>
Do a research on how to pipe commands and redirect commands.
</div>
