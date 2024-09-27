# Chapter 9: Main Memory Management

---

## 1. Introduction to Memory Management

Memory management is a crucial function of any operating system. It refers to
the process of coordinating and managing computer memory, ensuring that each
running program has enough memory to execute and that memory is used
efficiently. The operating system is responsible for allocating memory to
different processes and ensuring memory protection so that one process cannot
interfere with another.

In modern systems, memory management involves various techniques to manage both
**physical memory (RAM)** and **virtual memory**. It also handles the movement
of processes between main memory and disk storage.

Key responsibilities of memory management include:

- **Allocation** of memory to processes.
- **Protection** to prevent unauthorized access.
- **Efficient use** of memory resources.
- **Swapping** processes between main memory and disk as needed.

---

## 2. Memory Hierarchy

Memory in a system can be categorized into multiple levels of hierarchy, each
with different speeds and sizes. At the top is the **fastest** but smallest
memory, and at the bottom is the **slowest** but largest.

1. **Registers and Cache Memory**: Extremely fast, small memory spaces inside
   the CPU used to store temporary data.
2. **Main Memory (RAM)**: The physical memory where processes are loaded for
   execution.
3. **Secondary Storage (HDD/SSD)**: Used when RAM is not sufficient. This is
   much slower but significantly larger in size.
4. **Virtual Memory**: A technique that allows the system to use part of the
   secondary storage as if it were RAM, enabling processes to use more memory
   than physically available.

---

## 3. Memory Allocation Techniques

The OS can allocate memory to processes in different ways:

1. **Single Contiguous Allocation**: A simple technique where all memory is
   allocated to one program. This was used in very early systems but is impractical
   in multi-tasking environments.
2. **Partitioned Allocation**: Memory is divided into multiple partitions.
   There are two types:
   - **Fixed Partitioning**: Memory is divided into fixed-size blocks. If a
     process needs less memory than the block size, some memory is wasted
     (internal fragmentation).
   - **Dynamic Partitioning**: Partitions are created dynamically to fit the
     exact needs of a process, but this can lead to **external fragmentation**
     where free memory is scattered in small blocks.

---

## 4. Paging

**Paging** is one of the most commonly used memory management techniques in
modern operating systems. It eliminates the problem of fragmentation by
dividing memory into fixed-size blocks called **pages** (for virtual memory)
and **frames** (for physical memory).

- **Page Table**: The operating system maintains a **page table** to track
  which pages of a process are in which frames of physical memory.
- **Page Faults**: If a process accesses a page that is not currently in memory,
  a **page fault** occurs, and the operating system must load the page from disk.

### Advantages of Paging:

- It avoids both internal and external fragmentation.
- Provides flexibility since processes can be allocated non-contiguous memory
  blocks.

In Linux, a multi-level paging system is used for virtual memory management.
Processes are not aware of physical memory addresses, and the kernel handles
mapping between virtual and physical memory.

---

## 5. Segmentation

In **segmentation**, memory is divided into **logical segments** such as the
code, data, and stack of a process. Each segment has a varying size based on
the process's needs.

- **Segment Table**: Similar to the page table, the segment table keeps track
  of the base and limit addresses for each segment of a process.

In contrast to paging, segmentation is concerned with dividing programs into
logical units that reflect how the programmer views memory
(e.g., a code segment, a data segment).

---

## 6. Virtual Memory

**Virtual memory** allows an operating system to use secondary storage
(like an SSD or HDD) as an extension of RAM. This allows systems to run
processes that exceed the size of physical memory.

- **Demand Paging**: Pages are only loaded into memory when they are needed by
  the process.
- **Page Replacement Algorithms**: When memory is full, the OS needs to decide
  which pages to swap out. Some common page replacement algorithms are:

1. **FIFO (First-In-First-Out)**: The oldest page is replaced first.
2. **LRU (Least Recently Used)**: The page that hasn’t been used for the
   longest time is replaced.
3. **Optimal Page Replacement**: Replaces the page that will not be used for
   the longest time in the future (though this is mostly theoretical as it requires
   knowledge of future memory access).

---

## 7. Memory Fragmentation

Fragmentation occurs when memory is used inefficiently, leading to wasted
memory space. There are two types of fragmentation:

1. **External Fragmentation**: Occurs when free memory is scattered in small
   blocks across the system, making it difficult to allocate large contiguous
   memory blocks to new processes.
2. **Internal Fragmentation**: Happens when fixed-size memory blocks are
   allocated to processes, but the processes do not fully utilize them.

---

## 8. Memory Protection

**Memory protection** is vital to ensure that one process cannot interfere with
the memory space of another process. Operating systems enforce memory
protection through techniques such as **base and limit registers**.

In Linux, hardware mechanisms like segmentation and paging are used to protect
memory.

---

## 9. Conclusion

Memory management is essential for the efficient operation of any system.
Operating systems use a variety of techniques to allocate, protect, and
optimize memory usage. Understanding how paging, segmentation, virtual memory,
and page replacement algorithms work is critical for optimizing system
performance and resource allocation.

By implementing the memory allocator and understanding these concepts,
students can appreciate the real-world challenges operating systems face in
managing memory efficiently.
