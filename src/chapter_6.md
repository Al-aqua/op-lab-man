# Chapter 6: Processes and Process Creation

Understanding processes and how they are created is fundamental to managing
and optimizing a Linux system. This chapter explores what processes are,
how they are created, and the tools available to manage them effectively.

## What is a Process?

A process is a running instance of a program. It is an active entity that
requires system resources such as CPU time, memory, and input/output (I/O)
operations to execute its instructions. In Linux, every process is assigned
a unique Process ID (PID), which is used to identify and manage it.

## Key Concepts:

- **Parent and Child Processes:** n Linux, every process is spawned by another
  process, known as the parent process. The new process is called a child
  process. The `init` or `systemd` process is the ancestor of all processes and
  has a PID of 1.

- **Foreground and Background Processes:** Foreground processes run in the
  terminal, blocking input until they finish, while background processes run
  independently of the terminal, allowing continued input.

## Process creation

Process creation in Linux is a fundamental concept that involves duplicating
an existing process (the parent) to create a new process (the child). This is
primarily achieved using system calls like `fork()`, `exec()`, and `wait()`. Let's
explore these in more detail with examples.

> [!NOTE]
> We will be using the c programming language to explore these concepts.
> The c and c++ compiler(`gcc`) comes installed by default in Linux.
> to compile the code, use
>
> ```bash
> gcc source_filr.c -o exec_file
> ```

**1. `fork()` - Creating a New Process**

The `fork()` system call is used to create a new process by duplicating the
current process. The newly created process is called the child process,
and the original process is called the parent process. After a fork()
call, two processes will execute: the parent and the child.

Basic Example of fork() in C:

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        // Error in fork
        fprintf(stderr, "Fork Failed\n");
        return 1;
    } else if (pid == 0) {
        // Child process
        printf("This is the Child Process. PID: %d\n", getpid());
    } else {
        // Parent process
        printf("This is the Parent Process. PID: %d, Child PID: %d\n", getpid(), pid);
    }

    return 0;
}

```

**Explanation:**

- The `fork()` call returns twice: once in the parent process and once in the
  child process.
  - In the child process, `fork()` returns `0`.
  - In the parent process, `fork()` returns the PID of the child process.
- The `getpid()` function returns the PID of the current process.
- The program prints different messages depending on whether it's running in
  the parent or child process.

**Expected Output:**

```sh
This is the Parent Process. PID: 1234, Child PID: 1235
This is the Child Process. PID: 1235
```

**2.`exec()` - Replacing a Process Image**

After creating a child process, it is common to replace the child’s process
image with a new program using the `exec()` family of functions. This does not
create a new process but replaces the current process's memory with a new program.

Example Using exec() in C:

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        // Error in fork
        fprintf(stderr, "Fork Failed\n");
        return 1;
    } else if (pid == 0) {
        // Child process
        printf("Child Process - Executing ls command\n");
        execlp("/bin/ls", "ls", NULL);
        // If exec fails
        printf("Exec Failed\n");
    } else {
        // Parent process
        wait(NULL);
        printf("Child Complete\n");
    }

    return 0;
}
```

**Explanation:**

- After `fork()`, the child process uses `execlp()` to execute the ls command.
- If `exec()` succeeds, the original child process is replaced by `ls`, and
  the following code (`printf("Exec Failed\n")`) is not executed.
- The parent process waits for the child to complete using `wait()`.

**Expected Output:**

```sh
Child Process - Executing ls command
# Output of ls command
Child Complete
```

**3. `wait()` - Synchronizing Processes**

The `wait()` system call makes the parent process wait until all of its child
processes have finished executing. This is useful for ensuring that the parent
does not exit before the child, or to capture the child's exit status.

Example Using wait() in C:

```c
#include <stdio.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
  pid_t pid = fork();

  if (pid < 0) {
    // Error in fork
    fprintf(stderr, "Fork Failed\n");
    return 1;
  } else if (pid == 0) {
    // Child process
    printf("Child Process - Executing ls command\n");
    execlp("/bin/ls", "ls", NULL);
    // If exec fails
    printf("Exec Failed\n");
  } else {
    // Parent process
    wait(NULL);
    printf("Child Complete\n");
  }

  return 0;
}
```

**Explanation:**

- The child process prints its PID and then simulates some work by sleeping
  for 2 seconds.
- The parent process calls `wait()`, which pauses the parent until the child
  process completes.
- After the child finishes, the parent resumes and prints a completion message.

**Expected Output:**

```sh
Parent Process. Waiting for Child to Complete
Child Process. PID: 1235
Child Process Complete
Parent Process Resuming After Child Completion
```

**4. `exit()` - Terminating a Process**

The `exit()` system call is used to terminate a process. This is useful for
terminating a process in an orderly manner.

Example Using exit() in C:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        // Error in fork
        fprintf(stderr, "Fork Failed\n");
        return 1;
    } else if (pid == 0) {
        // Child process
        printf("Child Process. PID: %d\n", getpid());

        // Simulate some work in the child process
        // Now exit with a specific status code
        exit(1);  // Exit with status 1 to indicate an error or specific condition
    } else {
        // Parent process
        int status;
        wait(&status);  // Wait for the child and capture its exit status

        // Check if the child terminated normally
        if (WIFEXITED(status)) {
            int exit_status = WEXITSTATUS(status);  // Get the child's exit status
            printf("Child Process Exited with Status: %d\n", exit_status);
        } else {
            printf("Child Process Did Not Terminate Normally\n");
        }
    }

    return 0;
}
```

**Explanation:**

1. `exit(1)` in the Child Process:
   - In the child process, the `exit(1)` function is called. This function
     terminates the child process and returns an exit status code of `1` to the
     operating system.
   - The exit status code can be any integer. A status of `0` usually
     indicates successful completion, while non-zero values (like `1`) are
     often used to indicate errors or specific conditions.
2. `wait(&status)` in the Parent Process:
   - The parent process calls `wait(&status)`, which makes it wait until the
     child process finishes. The status variable is used to store the child's
     exit information.
   - The `wait()` function captures more than just whether the child exited;
     it also captures how the child exited, including whether it was terminated
     by a signal or completed normally with an `exit()` call.
3. Checking the Exit Status:
   - `WIFEXITED(status)`: This macro checks whether the child process
     terminated normally via `exit()` or `\_exit()`. If it did, the macro
     returns a non-zero value.
   - `WEXITSTATUS(status)`: If `WIFEXITED(status)` is true, this macro
     extracts the actual exit status code provided by the child process
     via `exit()`. In this case, it retrieves the `1` that was passed to
     `exit(1)`.

**Expected Output:**

```sh
Child Process. PID: 1235
Child Process Exited with Status: 1
```

## Managing Processes

Linux provides several commands to manage and monitor processes:

1. `ps`: Displays a snapshot of the current processes.
   - `ps aux`: Lists all processes running on the system, along with detailed
     information.
2. `top`: Provides a dynamic, real-time view of running processes.
   - Displays system resource usage and allows you to sort processes by CPU
     or memory usage.
3. `htop`: An enhanced version of top with a more user-friendly interface.
   - Allows you to interactively manage processes (e.g., kill processes,
     sort by different metrics).
4. `kill`: Sends a signal to a process, typically used to terminate it.
   - `kill PID`: Sends the default signal (`SIGTERM`) to terminate the
     process with the given PID.
   - `kill -9 PID`: Sends the `SIGKILL` signal to forcefully terminate
     the process.
5. `nice` and `renice`: Adjust the priority of a process.
   - `nice`: Starts a process with a modified priority.
   - `renice`: Changes the priority of an already running process.
   - `nice -n 10 ./cpu_task.sh`: Increases the priority of the process by 10.
   - `renice -n -10 PID`: Decreases the priority of the process with the
     given PID by 10.
6. `bg` and `fg`: Resume background jobs or bring them to the foreground.
   - `bg`: Moves a suspended job to the background.
   - `fg`: Brings a background job to the foreground.

## Monitoring Process Performance

Monitoring process performance is key to ensuring efficient system operation.
Tools like `top`, `htop`, and `vmstat` provide real-time data on process
performance, helping identify resource-intensive processes.

**Understanding Load Average:**

- The load average represents the average number of processes waiting for CPU
  time over 1, 5, and 15-minute intervals.
- High load averages indicate that processes are spending more time waiting
  for CPU resources, which may signal a need for optimization or resource
  scaling.

**Using `strace` for Debugging:**

- `strace`: Traces system calls and signals received by a process, useful
  for debugging and understanding process behavior.

```sh
strace -p PID
```
