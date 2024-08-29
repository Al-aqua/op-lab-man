# Chapter 7: Introduction to Threads Programming

## 1. What is a Thread?

A thread is the smallest unit of execution within a process. Unlike processes,
which are independent execution units containing their own memory space,
threads are part of a process and share the same memory space. This shared
environment makes threads more lightweight than processes because they require
fewer resources to create and maintain.

## 2. Threads vs. Processes

- **Processes**
  - A process is a self-contained execution environment.
  - Each process has its own memory space, including code, data, and system
    resources like file descriptors.
  - Processes are isolated from each other, which provides a high level of
    security but comes with the overhead of context switching.
- **Threads**
  - A thread, often referred to as a "lightweight process," exists within a process.
  - Threads within the same process share the same memory space, making
    communication between threads faster.
  - While threads share resources, they can also lead to issues like race
    conditions if not managed properly.

## 3. The Role of Threads in Operating Systems and Programming

Threads play a crucial role in modern operating systems and programming because
they allow multiple tasks to be performed simultaneously within the same
process. This is especially important in applications requiring concurrency,
such as web servers, real-time systems, and applications with a graphical user
interface (GUI).

### Advantages of Threads:

- **Responsiveness**: Threads allow a program to remain responsive by
  performing background tasks while the main program is running.
- **Resource Sharing**: Since threads share memory and resources, they are more
  efficient than processes in performing tasks that require frequent communication.
- **Parallelism**: Threads can be executed in parallel on multi-core processors,
  improving the performance of CPU-bound tasks.

## 4. Creating and Managing Threads in Linux using C

In Linux, threads are created and managed using the POSIX threads (pthreads)
library. This library provides a set of functions for creating, synchronizing,
and managing threads.

### Creating a Thread

To create a thread in Linux using C, you use the `pthread_create` function:

```c
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

void *threadFunction(void *arg) {
    printf("Thread is running!\n");
    return NULL;
}

int main() {
    pthread_t thread;
    int result;

    result = pthread_create(&thread, NULL, threadFunction, NULL);
    if (result != 0) {
        printf("Thread creation failed!\n");
        return -1;
    }

    pthread_join(thread, NULL);
    printf("Thread has finished execution.\n");
    return 0;
}
```

- `pthread_create`: This function creates a new thread. It takes four arguments:
  - A pointer to the `pthread_t` variable where the thread ID will be stored.
  - Thread attributes (can be `NULL` for default attributes).
  - The function the thread will execute.
  - An argument to the thread function (can be `NULL`).
- `pthread_join`: This function waits for the specified thread to terminate.

## 5. Thread Synchronization

Thread synchronization is critical in multi-threaded programming to ensure
that threads do not interfere with each other while accessing shared
resources. Synchronization techniques help maintain data integrity and prevent
race conditions.

### 1. Race Condition

A race condition occurs when two or more threads attempt to change shared
data at the same time, leading to unexpected and incorrect results. Consider
the following example where multiple threads increment a shared counter:

```c
#include <pthread.h>
#include <stdio.h>

int counter = 0;

void *incrementCounter(void *arg) {
    for (int i = 0; i < 100000; i++) {
        counter++; // Not synchronized
    }
    return NULL;
}

int main() {
    pthread_t threads[10];

    for (int i = 0; i < 10; i++) {
        pthread_create(&threads[i], NULL, incrementCounter, NULL);
    }

    for (int i = 0; i < 10; i++) {
        pthread_join(threads[i], NULL);
    }

    printf("Final counter value: %d\n", counter); // Expect 1000000
    return 0;
}
```

**Problem**: Here, you might expect the final counter value to be `1000000`,
but due to the race condition, it might be less. This is because threads can
interrupt each other while modifying the counter, leading to lost updates.

### 2. Using Mutex for Synchronization

To prevent race conditions, you can use a mutex (short for "mutual exclusion").
A mutex ensures that only one thread can execute a piece of code at a time.

```c
#include <pthread.h>
#include <stdio.h>

int counter = 0;
pthread_mutex_t lock;

void *incrementCounter(void *arg) {
    for (int i = 0; i < 100000; i++) {
        pthread_mutex_lock(&lock);   // Lock the mutex before accessing the shared resource
        counter++;
        pthread_mutex_unlock(&lock); // Unlock the mutex after the operation
    }
    return NULL;
}

int main() {
    pthread_t threads[10];
    pthread_mutex_init(&lock, NULL); // Initialize the mutex

    for (int i = 0; i < 10; i++) {
        pthread_create(&threads[i], NULL, incrementCounter, NULL);
    }

    for (int i = 0; i < 10; i++) {
        pthread_join(threads[i], NULL);
    }

    printf("Final counter value: %d\n", counter); // Should be 1000000
    pthread_mutex_destroy(&lock); // Destroy the mutex when done
    return 0;
}
```

**Solution**: Using a mutex ensures that only one thread can increment the
counter at a time, preventing race conditions. Now, the final counter value
should be `1000000` as expected.

### 3. Deadlock

A deadlock occurs when two or more threads are waiting for each other to
release a resource, causing them to be stuck forever. Consider this example:

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

pthread_mutex_t lock1, lock2;

void *thread1(void *arg) {
    pthread_mutex_lock(&lock1);
    printf("Thread 1: Locked lock1\n");

    // Simulate some work
    sleep(1);

    pthread_mutex_lock(&lock2);
    printf("Thread 1: Locked lock2\n");

    pthread_mutex_unlock(&lock2);
    pthread_mutex_unlock(&lock1);
    return NULL;
}

void *thread2(void *arg) {
    pthread_mutex_lock(&lock2);
    printf("Thread 2: Locked lock2\n");

    // Simulate some work
    sleep(1);

    pthread_mutex_lock(&lock1);
    printf("Thread 2: Locked lock1\n");

    pthread_mutex_unlock(&lock1);
    pthread_mutex_unlock(&lock2);
    return NULL;
}

int main() {
    pthread_t t1, t2;
    pthread_mutex_init(&lock1, NULL);
    pthread_mutex_init(&lock2, NULL);

    pthread_create(&t1, NULL, thread1, NULL);
    pthread_create(&t2, NULL, thread2, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&lock1);
    pthread_mutex_destroy(&lock2);
    return 0;
}
```

**Explanation:**

- **Thread 1** locks `lock1` and then tries to lock `lock2`.
- **Thread 2** locks `lock2` and then tries to lock `lock1`.
- Both threads are now waiting for the other to release the lock they need,
  leading to a deadlock.

**How to Avoid Deadlock:**

- **Avoid Nested Locks**: Minimize the use of nested locks and ensure that all
  threads acquire locks in a consistent order.

- **Lock Ordering**: Always acquire locks in a fixed global order to prevent
  circular waiting.

- **Try-Lock**: Use `pthread_mutex_trylock` to attempt locking a mutex
  without blocking, allowing the thread to back off if it cannot acquire the
  lock.

  ```c
  if (pthread_mutex_trylock(&lock2) == 0) {
    // Successfully locked lock2
  } else {
    // Could not lock lock2, handle accordingly
  }

  ```

### 4. Condition Variables

Condition variables are used to signal between threads. They allow one or
more threads to wait until a particular condition occurs. Condition variables
are always used with a mutex to avoid race conditions.

**Example: Producer-Consumer Problem**

In the producer-consumer problem, one or more producer threads generate data
and put it into a buffer, while one or more consumer threads take data out
of the buffer and process it.

```c
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

#define BUFFER_SIZE 10

int buffer[BUFFER_SIZE];
int count = 0;

pthread_mutex_t mutex;
pthread_cond_t cond_producer, cond_consumer;

void *producer(void *arg) {
    int item;
    while (1) {
        item = rand() % 100; // Produce a random item

        pthread_mutex_lock(&mutex);
        while (count == BUFFER_SIZE) {
            pthread_cond_wait(&cond_producer, &mutex); // Wait if buffer is full
        }

        buffer[count] = item;
        count++;
        printf("Produced: %d\n", item);

        pthread_cond_signal(&cond_consumer); // Signal consumer
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

void *consumer(void *arg) {
    int item;
    while (1) {
        pthread_mutex_lock(&mutex);
        while (count == 0) {
            pthread_cond_wait(&cond_consumer, &mutex); // Wait if buffer is empty
        }

        item = buffer[--count];
        printf("Consumed: %d\n", item);

        pthread_cond_signal(&cond_producer); // Signal producer
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

int main() {
    pthread_t prod, cons;

    pthread_mutex_init(&mutex, NULL);
    pthread_cond_init(&cond_producer, NULL);
    pthread_cond_init(&cond_consumer, NULL);

    pthread_create(&prod, NULL, producer, NULL);
    pthread_create(&cons, NULL, consumer, NULL);

    pthread_join(prod, NULL);
    pthread_join(cons, NULL);

    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond_producer);
    pthread_cond_destroy(&cond_consumer);

    return 0;
}
```

**Explanation:**

- **Producer**: Produces an item and places it into the buffer. If the buffer
  is full, the producer waits until there is space.
- **Consumer**: Consumes an item from the buffer. If the buffer is empty,
  the consumer waits until there is data to consume.
- **Condition Variables**: `cond_producer` is used to signal the producer
  that there is space in the buffer, and `cond_consumer` is used to signal
  the consumer that there is data to consume.

This ensures smooth operation without overfilling the buffer or consuming
non-existent data.

> Assignment:
> Write a basic multi-threaded program in any programming language of your
> choice.
