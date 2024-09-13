# Chapter 8: CPU Scheduling and Scheduling Algorithms

---

## 1. Introduction to CPU Scheduling

CPU scheduling is a fundamental aspect of multitasking operating systems.
It determines the order in which processes run on the CPU, maximizing resource
utilization and ensuring system responsiveness. Since a computer can only have
one CPU execute a single task at any given moment, the operating system must
efficiently decide which process to run next.

In this chapter, we will explore different scheduling algorithms, how they
work, and their practical applications in a Linux environment.

---

## 2. CPU Burst and IO Burst

Processes alternate between CPU bursts (performing computations) and IO bursts
(waiting for input/output operations). The CPU scheduling decisions are made
during these transitions:

- **CPU-bound processes**: Spend more time performing calculations (CPU bursts).
- **IO-bound processes**: Spend more time waiting for input/output operations
  (IO bursts).

Efficient scheduling ensures that CPU and IO-bound processes don't
unnecessarily wait for each other, optimizing overall system performance.

---

## 3. Types of CPU Scheduling

- **Preemptive Scheduling**: The CPU can interrupt a running process and
  allocate it to another process. Preemption is key in ensuring that no single
  process monopolizes the CPU. This is critical in time-sharing systems where
  multiple users or applications need the CPU at frequent intervals.
- **Non-preemptive Scheduling**: A running process retains control of the CPU
  until it voluntarily releases it by either terminating or switching to a
  waiting state. It's simpler but can lead to inefficient CPU usage if a process
  takes too long.

_Example: Linux uses preemptive scheduling, but for specific system calls or
critical tasks, it may temporarily switch to non-preemptive mode._

---

## 4. Scheduling Algorithms

Let’s dive into the most common CPU scheduling algorithms, how they work,
and their practical implementation:

### 4.1. First-Come, First-Served (FCFS)

- **How it works**: Processes are scheduled in the order they arrive in the
  ready queue.
- **Type**: Non-preemptive.
- **Pros**: Simple and easy to implement.
- **Cons**: Can lead to long wait times, particularly if the first process in
  the queue is CPU-bound (known as the "convoy effect").

**Example in C:**

```c
#include <stdio.h>

int main() {
    int n, i;
    printf("Enter number of processes: ");
    scanf("%d", &n);

    int burst_time[n], waiting_time[n], turnaround_time[n];
    int total_waiting_time = 0, total_turnaround_time = 0;

    // Input burst times
    for (i = 0; i < n; i++) {
        printf("Enter burst time for process P%d: ", i + 1);
        scanf("%d", &burst_time[i]);
    }

    waiting_time[0] = 0;

    // Calculate waiting time
    for (i = 1; i < n; i++) {
        waiting_time[i] = waiting_time[i - 1] + burst_time[i - 1];
        total_waiting_time += waiting_time[i];
    }

    // Calculate turnaround time
    for (i = 0; i < n; i++) {
        turnaround_time[i] = burst_time[i] + waiting_time[i];
        total_turnaround_time += turnaround_time[i];
    }

    // Output results
    printf("\nProcess\tBurst Time\tWaiting Time\tTurnaround Time\n");
    for (i = 0; i < n; i++) {
        printf("P%d\t%d\t\t%d\t\t%d\n", i + 1, burst_time[i], waiting_time[i], turnaround_time[i]);
    }

    printf("\nAverage Waiting Time: %.2f", (float)total_waiting_time / n);
    printf("\nAverage Turnaround Time: %.2f", (float)total_turnaround_time / n);

    return 0;
}
```

**Explanation**:

1. **Input Burst Times**: Collect burst times for each process.
2. **Calculate Waiting Time**: Waiting time for each process is computed based on previous processes.
3. **Calculate Turnaround Time**: Turnaround time is the sum of burst time and waiting time.
4. **Output Results**: Display process details along with average waiting and turnaround times.

### 4.2. Shortest Job Next (SJN)

- **How it works**: The process with the shortest burst time is selected next.
- **Type**: Non-preemptive.
- **Pros**: Minimizes average waiting time.
- **Cons**: Can lead to starvation of longer processes.

**Example in C:**

```c
#include <stdio.h>

int main() {
    int n, i, j;
    printf("Enter number of processes: ");
    scanf("%d", &n);

    int burst_time[n], waiting_time[n], turnaround_time[n];
    int total_waiting_time = 0, total_turnaround_time = 0;

    // Input burst times
    for (i = 0; i < n; i++) {
        printf("Enter burst time for process P%d: ", i + 1);
        scanf("%d", &burst_time[i]);
    }

    // Sort processes by burst time
    for (i = 0; i < n - 1; i++) {
        for (j = i + 1; j < n; j++) {
            if (burst_time[i] > burst_time[j]) {
                int temp = burst_time[i];
                burst_time[i] = burst_time[j];
                burst_time[j] = temp;
            }
        }
    }

    waiting_time[0] = 0;

    // Calculate waiting time
    for (i = 1; i < n; i++) {
        waiting_time[i] = waiting_time[i - 1] + burst_time[i - 1];
        total_waiting_time += waiting_time[i];
    }

    // Calculate turnaround time
    for (i = 0; i < n; i++) {
        turnaround_time[i] = burst_time[i] + waiting_time[i];
        total_turnaround_time += turnaround_time[i];
    }

    // Output results
    printf("\nProcess\tBurst Time\tWaiting Time\tTurnaround Time\n");
    for (i = 0; i < n; i++) {
        printf("P%d\t%d\t\t%d\t\t%d\n", i + 1, burst_time[i], waiting_time[i], turnaround_time[i]);
    }

    printf("\nAverage Waiting Time: %.2f", (float)total_waiting_time / n);
    printf("\nAverage Turnaround Time: %.2f", (float)total_turnaround_time / n);

    return 0;
}
```

**Explanation:**

1. **Input Burst Times**: Collect burst times for each process.
1. **Sort Processes**: Sort processes based on burst times.
1. **Calculate Waiting Time**: Waiting time is calculated based on the sorted order.
1. **Calculate Turnaround Time**: Compute turnaround time as the sum of burst time and waiting time.
1. **Output Results**: Display sorted process details and average times.

### 4.3. Priority Scheduling

- **How it works**: Each process is assigned a priority. The process with the
  highest priority is selected next.
- **Type**: Can be preemptive or non-preemptive.
- **Pros**: Allows important tasks to be completed first.
- **Cons**: Can lead to starvation of lower-priority processes.

**Example in C**

```c
#include <stdio.h>

#define MAX 10

int main() {
    int n, i, j;
    printf("Enter number of processes: ");
    scanf("%d", &n);

    int burst_time[MAX], priority[MAX], waiting_time[MAX], turnaround_time[MAX];
    int total_waiting_time = 0, total_turnaround_time = 0;

    // Input burst times and priorities
    for (i = 0; i < n; i++) {
        printf("Enter burst time and priority for process P%d: ", i + 1);
        scanf("%d %d", &burst_time[i], &priority[i]);
    }

    // Sort processes by priority
    for (i = 0; i < n - 1; i++) {
        for (j = i + 1; j < n; j++) {
            if (priority[i] < priority[j]) {
                int temp = burst_time[i];
                burst_time[i] = burst_time[j];
                burst_time[j] = temp;

                temp = priority[i];
                priority[i] = priority[j];
                priority[j] = temp;
            }
        }
    }

    waiting_time[0] = 0;

    // Calculate waiting time
    for (i = 1; i < n; i++) {
        waiting_time[i] = waiting_time[i - 1] + burst_time[i - 1];
        total_waiting_time += waiting_time[i];
    }

    // Calculate turnaround time
    for (i = 0; i < n; i++) {
        turnaround_time[i] = burst_time[i] + waiting_time[i];
        total_turnaround_time += turnaround_time[i];
    }

    // Output results
    printf("\nProcess\tBurst Time\tPriority\tWaiting Time\tTurnaround Time\n");
    for (i = 0; i < n; i++) {
        printf("P%d\t%d\t\t%d\t\t%d\t\t%d\n", i + 1, burst_time[i], priority[i], waiting_time[i], turnaround_time[i]);
    }

    printf("\nAverage Waiting Time: %.2f", (float)total_waiting_time / n);
    printf("\nAverage Turnaround Time: %.2f", (float)total_turnaround_time / n);

    return 0;
}
```

**Explanation**:

1. **Input Burst Times and Priorities**: Collect burst times and priorities
   for each process.
2. **Sort Processes**: Sort processes based on priority.
3. **Calculate Waiting Time**: Compute waiting time based on sorted order.
4. **Calculate Turnaround Time**: Calculate turnaround time.
5. **Output Results**: Display process details, including priorities, and
   average times.

### 4.4. Round Robin Scheduling

- **How it works**: Each process is assigned a fixed time slice (quantum).
  The CPU cycles through processes, allocating each a turn.
- **Type**: Preemptive.
- **Pros**: Fair distribution of CPU time among processes.
- **Cons**: Time quantum must be carefully chosen to avoid excessive context
  switching.

**Example in C:**

```c
#include <stdio.h>

#define MAX 10

int main() {
    int n, i, time_quantum, time = 0, remaining;
    printf("Enter number of processes: ");
    scanf("%d", &n);

    int burst_time[MAX], remaining_time[MAX], waiting_time[MAX], turnaround_time[MAX];
    int total_waiting_time = 0, total_turnaround_time = 0;

    // Input burst times and time quantum
    for (i = 0; i < n; i++) {
        printf("Enter burst time for process P%d: ", i + 1);
        scanf("%d", &burst_time[i]);
        remaining_time[i] = burst_time[i];
    }

    printf("Enter time quantum: ");
    scanf("%d", &time_quantum);

    remaining = n;

    // Simulate Round Robin Scheduling
    while (remaining > 0) {
        for (i = 0; i < n; i++) {
            if (remaining_time[i] > 0) {
                if (remaining_time[i] > time_quantum) {
                    remaining_time[i] -= time_quantum;
                    time += time_quantum;
                } else {
                    time += remaining_time[i];
                    waiting_time[i] = time - burst_time[i];
                    remaining_time[i] = 0;
                    remaining--;
                }
            }
        }
    }

    // Calculate waiting time
    for (i = 1; i < n; i++) {
        waiting_time[i] = waiting_time[i - 1] + burst_time[i - 1];
        total_waiting_time += waiting_time[i];
    }

    // Calculate turnaround time
    for (i = 0; i < n; i++) {
        turnaround_time[i] = burst_time[i] + waiting_time[i];
        total_turnaround_time += turnaround_time[i];
    }

    // Output results
    printf("\nProcess\tBurst Time\tWaiting Time\tTurnaround Time\n");
    for (i = 0; i < n; i++) {
        printf("P%d\t%d\t\t%d\t\t%d\n", i + 1, burst_time[i], waiting_time[i], turnaround_time[i]);
    }

    printf("\nAverage Waiting Time: %.2f", (float)total_waiting_time / n);
    printf("\nAverage Turnaround Time: %.2f", (float)total_turnaround_time / n);

    return 0;
}
```

**Explanation**:

1. **Input Burst Times and Time Quantum**: Collect burst times and time quantum.
2. **Round Robin Simulation**: Allocate CPU time in fixed slices.
3. **Calculate Waiting and Turnaround Times**: Compute times based on the simulation.
4. **Output Results**: Display process details and average times.

### 4.5. Multilevel Feedback Queue Scheduling

- **How it works**: Processes are managed in multiple queues based on their
  behavior and requirements. The system adjusts process priorities dynamically.

**Example in C:**

```c
#include <stdio.h>

#define MAX 10

int main() {
    int n, i, time_quantum, time = 0, remaining;
    printf("Enter number of processes: ");
    scanf("%d", &n);

    int burst_time[MAX], waiting_time[MAX], turnaround_time[MAX];
    int remaining_time[MAX], process_queue[MAX];
    int total_waiting_time = 0, total_turnaround_time = 0;
    remaining = n;

    printf("Enter time quantum: ");
    scanf("%d", &time_quantum);

    // Input burst times
    for (i = 0; i < n; i++) {
        printf("Enter burst time for process P%d: ", i + 1);
        scanf("%d", &burst_time[i]);
        remaining_time[i] = burst_time[i];
    }

    // Simulate Multilevel Feedback Queue
    while (remaining > 0) {
        for (i = 0; i < n; i++) {
            if (remaining_time[i] > 0) {
                if (remaining_time[i] > time_quantum) {
                    remaining_time[i] -= time_quantum;
                    time += time_quantum;
                } else {
                    time += remaining_time[i];
                    waiting_time[i] = time - burst_time[i];
                    total_waiting_time += waiting_time[i];
                    remaining_time[i] = 0;
                    remaining--;
                }
            }
        }
    }

    // Calculate turnaround time
    for (i = 0; i < n; i++) {
        turnaround_time[i] = burst_time[i] + waiting_time[i];
        total_turnaround_time += turnaround_time[i];
    }

    // Output results
    printf("\nProcess\tBurst Time\tWaiting Time\tTurnaround Time\n");
    for (i = 0; i < n; i++) {
        printf("P%d\t%d\t\t%d\t\t%d\n", i + 1, burst_time[i], waiting_time[i], turnaround_time[i]);
    }
        printf("\nAverage Waiting Time: %.2f", (float)total_waiting_time / n);
        printf("\nAverage Turnaround Time: %.2f", (float)total_turnaround_time / n);

    return 0;
}
```

**Explanation**:

1. **Input Burst Times**: Collect burst times for each process.
2. **Multilevel Feedback Queue Simulation**: Processes are managed with a fixed
   time quantum.
3. **Calculate Waiting and Turnaround Times**: Compute these times after the
   simulation.
4. **Output Results**: Display results for each process.

> Linux uses a CPU scheduling algorithm called
> **Completely Fair Scheduler (CFS)**. CFS is designed to provide fair CPU time
> to processes while considering their priority and ensuring that all processes
> get a reasonable amount of CPU time.

---

## 5. Key Considerations in CPU Scheduling

- **Starvation**: Processes may be indefinitely delayed if they are always
  preempted by higher-priority processes. Algorithms like Round Robin and
  Multilevel Feedback Queue help mitigate this issue.
- **Throughput**: The rate at which processes are completed. A good scheduling
  algorithm improves throughput by reducing waiting and turnaround times.
- **Fairness**: Ensuring that all processes get a fair share of the CPU.
  Fairness can be improved by using algorithms that avoid indefinite delays.
- **Complexity**: Some algorithms, like Multilevel Feedback Queue, can be
  complex to implement but provide better performance in dynamic environments.

---

## 6. Summary

CPU scheduling is fundamental to operating systems, affecting how efficiently
processes are executed and how resources are allocated. Understanding different
algorithms allows system designers to choose the best approach based on specific
needs and constraints. The choice of scheduling algorithm impacts system
performance, user experience, and overall system efficiency.

- **FCFS**: Simple but can lead to long wait times for short processes.
- **SJN**: Reduces average waiting time but may lead to process starvation.
- **Priority Scheduling**: Can prioritize important tasks but risks starving
  lower-priority processes.
- **Round Robin**: Ensures fair allocation of CPU time among processes but
  requires careful selection of time quantum.
- **Multilevel Queue**: Allows flexibility in scheduling different types of
  processes but can be complex to manage.
- **Multilevel Feedback Queue**: Dynamically adjusts priorities, balancing
  responsiveness and fairness.
