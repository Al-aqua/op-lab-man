# Chapter 5: Linux File System and File Security

1. **Introduction to Linux File System**
   The Linux file system is a fundamental component of the operating system,
   providing a structured way to store and organize files on disk. It enables
   users and applications to access, modify, and manage data efficiently. This
   section introduces the core concepts of the Linux file system, its directory
   structure, and the different types of files found in a Linux environment.

## Overview of File Systems

A file system is a method and data structure that an operating system uses to
control how data is stored and retrieved. Without a file system, information
placed in a storage medium would be one large block with no way to tell where
one piece of information stops and the next begins. By separating the data
into pieces and giving each piece a name, the information is easily isolated
and identified.

### Common File Systems in Linux:

1. **ext4 (Fourth Extended File System):**

   - The default file system for many Linux distributions.
   - Offers high performance, reliability, and large storage capacity.
   - Features include journaling, extents, and backward compatibility with ext3 and ext2.

2. **XFS**

   - Known for high performance and scalability.
   - Particularly well-suited for systems with large files and high I/O workloads.
   - Supports advanced features like quotas and defragmentation.

3. **Btrfs (B-tree File System):**

   - Designed for high-capacity and high-performance storage.
   - Features include snapshots, pooling, and checksums for data integrity.
   - Focuses on fault tolerance, repair, and easy administration.

### Directory Structure

The Linux directory structure is hierarchical, resembling an inverted tree.
The topmost directory is called the root directory and is denoted by a forward
slash (/). Below the root, the file system branches into various directories,
each serving a specific purpose.

**Key Directories and Their Purposes:**

- **`/` (Root): The base of the file system hierarchy. All other directories and files stem from here.**
- **`/bin`: Essential user binaries (executables) needed for system booting and single-user mode.**
- **`/boot`: Files required for booting the system, including the kernel and bootloader configuration files.**
- **`/dev`: Device files representing hardware components and peripherals.**
- **`/etc`: Configuration files for the system and installed applications.**
- **`/home`: Home directories for individual users. Each user has a personal directory under `/home`.**
- **`/lib`: Shared libraries needed by binaries in `/bin` and `/sbin`.**
- **`/mnt`: Mount point for temporarily mounted file systems.**
- **`/opt`: Optional software packages and third-party applications.**
- **`/proc`: Virtual file system providing process and kernel information.**
- **`/root`: Home directory for the root (superuser).**
- **`/sbin`: System binaries essential for system administration.**
- **`/srv`: Data for services provided by the system.**
- **`/tmp`: Temporary files created by the system and applications.**
- **`/usr`: User programs and utilities.**
- **`/var`: Variable data files, such as logs, mail spools, and temporary files.**

### File Types

In Linux, everything is considered a file, including hardware devices and
processes. The file types are identified using the `ls -l` command, which lists
files and their details.

### Common File Types:

- **Regular Files (`-`):** The most common type, representing data files, text files, scripts, and binaries.
- **Directories (`d`):** Special files that contain other files and directories.
- **Symbolic Links (`l`):** Pointers to other files or directories, allowing for flexible and dynamic file system structures.
- **Device Files (`b` for block devices, `c` for character devices):** Represent hardware devices. Block devices allow random access (e.g., hard drives), while character devices are accessed sequentially (e.g., keyboards).
- **Sockets (`s`):** Used for inter-process communication.
- **Pipes (`p`):** Enable data transfer between processes in a first-in, first-out (FIFO) manner.

## File Permissions

File permissions in Linux are a critical aspect of system security and access
control. They determine who can read, write, or execute a file. Understanding
how to view and modify these permissions is essential for managing files and
maintaining a secure system.

### Understanding File Permissions

Each file and directory has a set of permissions associated with three categories of users:

- **Owner**: The user who owns the file.
- **Group**: A set of users who share access rights to the file.
- **Others**: All other users on the system.

Permissions are divided into three types:

- **Read (`r`)**: Permission to read the contents of the file.
- **Write (`w`)**: Permission to modify or delete the file.
- **Execute (`x`)**: Permission to execute the file (if it is a script or binary).

These permissions are displayed using the `ls -l` command, which shows a 10-character string representing the permissions and file type:

```bash
ls -l
```

Example output:

```bash
-rwxr-xr--
```

Explanation of the characters:

- The first character indicates the file type:

  - `-`: Regular file
  - `d`: Directory
  - `l`: Symbolic link
  - `b`: Block device
  - `c`: Character device
  - `s`: Socket
  - `p`: Pipe

- The next nine characters are the permissions, divided into three groups of three:
  - `rwx` for the owner
  - `r-x` for the group
  - `r--` for others

In the example above, the file is a regular file (`-`), the owner has read,
write, and execute permissions (`rwx`), the group has read and execute
permissions (`r-x`), and others have read-only permissions (`r--`).

### Changing File Permissions

You can change file permissions using the `chmod` command in two modes: symbolic and numeric.

**Symbolic Mode:**

- Uses letters to represent the user category (`u` for owner, `g` for group, `o` for others, `a` for all) and permissions (`r, w, x`).
- `+` adds a permission, `-` removes a permission, and `=` sets a permission.

Examples:

```bash
chmod u+x filename     # Adds execute permission for the owner
chmod g-w filename     # Removes write permission for the group
chmod o=r filename     # Sets read-only permission for others
chmod a+rw filename    # Adds read and write permissions for all
```

**Numeric Mode:**

- Uses a three-digit octal number to represent permissions. Each digit is a
  sum of values: read (4), write (2), and execute (1).

Example:

```bash
chmod 755 filename     # Sets rwx for owner (4+2+1=7), r-x for group (4+1=5), and r-x for others (4+1=5)
chmod 644 filename     # Sets rw- for owner (4+2=6), r-- for group (4), and r-- for others (4)
chmod 700 filename     # Sets rwx for owner (4+2+1=7), --- for group (0), and --- for others (0)
```

### Special Permissions

Special permissions add additional control for executable files and directories.

**Set User ID (SUID):**

- When applied to an executable file, allows users to execute the file with
  the file owner's privileges.
- Represented by an `s` in the owner's execute position (`rws`).

Example:

```bash
chmod u+s filename
```

**Set Group ID (SGID):**

- When applied to an executable file, allows users to execute the file with
  the file group's privileges.
- When applied to a directory, files created within the directory inherit
  the group ownership.
- Represented by an `s` in the group's execute position (`r-s`).

Example:

```bash
chmod g+s directory
```

**Sticky Bit:**

- When applied to a directory, ensures that only the file owner can delete or
  rename files within that directory.
- Represented by a `t` in the others' execute position (`rwt`).

Example:

```bash
chmod +t directory
```

### Viewing and Modifying Permissions

**Viewing Permissions:**

`ls -l` displays the permissions, owner, group, and other details of files and directories.

Example:

```bash
ls -l
// Output
-rw-r--r-- 1 user group 1234 Jan  1 12:34 file.txt
```

**Modifying Permissions:**

- `chmod` command changes file permissions.
- `chown` command changes file ownership.
- `chgrp` command changes the group ownership.

Examples:

```bash
chmod 755 file.txt       # Change permissions to rwxr-xr-x
chown user file.txt      # Change owner to user
chgrp group file.txt     # Change group to group
```

## File Security and Access Control

File security and access control are essential components of a secure Linux
system. They ensure that sensitive data is protected and that only authorized
users can access or modify files. This section covers basic and advanced
mechanisms for securing files, including umask, Access Control Lists (ACLs),
and Mandatory Access Control (MAC) systems like SELinux and AppArmor.

**Understanding umask**

The umask (user file creation mode mask) command determines the default
permissions for new files and directories. It acts as a filter that removes
specific permissions when new files and directories are created.

**Calculating umask:**

- The default permissions for new files are `666` (read and write for all).
- The default permissions for new directories are `777` (read, write, and execute for all).
- The umask value is subtracted from these defaults to determine the final permissions.

**Viewing and Setting umask:**

- Use the `umask` command to view the current umask value.

```bash
umask
```

- To set a new umask value, use the umask command followed by the desired value.

```bash
umask 022
```

**Example Calculation:**

- If the umask is 022, the default permissions for a new file will be 644
  (666 - 022), resulting in read and write permissions for the owner, and
  read-only permissions for the group and others.
- For a new directory, the permissions will be 755 (777 - 022), resulting in
  read, write, and execute permissions for the owner, and read and execute
  permissions for the group and others.

### Access Control Lists (ACLs)

ACLs provide a more flexible permission mechanism than the traditional file
permissions. They allow you to set permissions for individual users or groups
on a per-file or per-directory basis.

**Using ACLs:**

- `getfacl`: Displays the ACL of a file or directory.

```bash
getfacl filename
```

- `setfacl`: Sets or modifies the ACL of a file or directory.

```bash
setfacl -m u:username:rwx filename  # Add/modify permissions for a user
setfacl -m g:groupname:rw filename  # Add/modify permissions for a group
setfacl -x u:username filename      # Remove ACL entry for a user
```

### SELinux and AppArmor

SELinux (Security-Enhanced Linux) and AppArmor are MAC systems that provide
additional security by enforcing policies that restrict how programs can
access files and resources.

**SELinux:**

- Enforces policies that define how processes interact with files, devices, and other processes.
- Policies are defined in terms of contexts, consisting of user, role, type, and level.

**Basic Commands:**

- `sestatus`: Checks the status of SELinux.
- `getenforce`: Displays the current mode of SELinux (Enforcing, Permissive, or Disabled).
- `setenforce`: Sets the mode of SELinux.

```bash
setenforce 0   # Permissive mode
setenforce 1   # Enforcing mode
```

**AppArmor:**

- Uses profiles to restrict the capabilities of programs.
- Profiles define the file access permissions and capabilities for a program.

**Basic Commands:**

- `aa-status`: Displays the current status of AppArmor.
- `aa-enforce`: Sets a profile to enforcing mode.
- `aa-complain`: Sets a profile to complain mode (logs violations but does not enforce).

## Encryption and Secure Deletion

Understanding the importance of data confidentiality and security is crucial
in today's digital world. This section explores the robust tools Linux
provides for encrypting files, securing entire file systems, and employing
techniques for secure file deletion.

### File Encryption

Encrypting files ensures that unauthorized users cannot access the data
without the appropriate decryption key. One of the most widely used tools
for file encryption in Linux is gpg (GNU Privacy Guard).

**Using gpg for File Encryption and Decryption:**

- Encrypting a File:

```bash
gpg -c filename
```

This command prompts you to enter a passphrase. The encrypted file is saved with a `.gpg` extension.

- Decrypting a File:

```bash
gpg -d filename.gpg
```

Enter the passphrase used for encryption to decrypt the file.

**Encrypting File Systems**

Encrypting entire file systems provides a higher level of security by ensuring
that all data stored within the file system is encrypted. LUKS
(Linux Unified Key Setup) is a standard for disk encryption.

### Secure Deletion

When files are deleted in a typical manner, the data is not completely erased
from the disk and can be recovered using forensic tools. Secure deletion
ensures that data is irrecoverable.

**Using shred for Secure Deletion:**

`shred`: Overwrites a file multiple times to make it harder to recover.

```bash
shred -u filename
```

The `-u` option removes the file after overwriting it.

**Using wipe for Secure Deletion:**

`wipe`: Securely erases files or entire disk partitions.

> Note: Traditional secure deletion tools may not be effective on SSDs due
> to wear leveling. Using encryption and secure erase commands provided by
> the SSD manufacturer is recommended.

## Backups and Recovery

Regular backups and the ability to recover data are essential components
of maintaining a reliable and secure Linux system. This section explores
various tools and techniques for creating backups and recovering data.

### Creating Backups

Creating regular backups ensures that you can restore your system to a
previous state in case of data loss, hardware failure, or other unforeseen
events. Linux offers several powerful tools for this purpose.

- **Using `tar` for Archiving:**

  - tar: Creates compressed archive files.

  ```bash
  tar -czvf backup.tar.gz /path/to/directory
  ```

  - `-c`: Create a new archive.
  - `-z`: Compress the archive with gzip.
  - `-v`: Verbose mode, shows the progress.
  - `-f`: Specifies the archive file name.

- **Using rsync for Incremental Backups:**

  - `rsync`: Synchronizes files and directories between two locations.

  ```bash
  rsync -av --delete /source/directory /backup/directory
  ```

  - `-a`: Archive mode, preserves permissions, timestamps, and symbolic links.
  - `-v`: Verbose mode.
  - `--delete`: Deletes files in the destination that are not present in the source.

### Recovering Data

In the event of data loss, having a solid recovery strategy can help minimize
downtime and data loss.

- **Restoring from tar Archives:**

  - tar: Extracts files from an archive.

  ```bash
  tar -xzvf backup.tar.gz -C /path/to/restore
  ```

  - `-x`: Extracts files from the archive.
  - `-z`: Decompress the archive with gzip.
  - `-v`: Verbose mode, shows the progress.
  - `-f`: Specifies the archive file name.
  - `-C`: Specifies the directory to extract files to.

- **Restoring with rsync:**

  - `rsync`: Synchronizes files from the backup to the source location.

  ```bash
  rsync -av /backup/directory /source/directory
  ```

> There are a lot of tools that can streamline data backups recovery and
> automate it like TimeShift.

### Recovering Deleted Files:

- `testdisk`: A powerful tool for recovering lost partitions and making
  non-booting disks bootable again.

  ```bash
  sudo testdisk
  ```

  Follow the interactive menu to analyze and recover partitions.

- `photorec`: Recovers lost files from hard disks and CD-ROMs.

  ```bash
  sudo photorec
  ```

  Follow the interactive menu to recover files.
