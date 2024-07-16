# Chapter 1: Basic Linux commands

These are the basic Linux commands to move around in the terminal.

1. **`ls` : List Directory Contents.**

- **Description** : The `ls` command lists the files and directories in the current directory.
- **Usage** : `ls [options] [directory]`
- **Examples** :
  - `ls` : Lists files and directories in the current directory.
  - `ls [directory]` : Lists files and directories in the specified directory.
  - `ls -l` : Lists files and directories with detailed information (permissions, owner, size, etc.).
  - `ls -a` : Lists all files, including hidden files (those starting with a dot).

> Exercise : <br/>
> Use the `ls` command to list the contents of the root directory `/` and write them down.

<br/>

---

<br/>

---

<br/>

---

<br/>

2. **`cd` : Change Directory.**

- **Description** : The `cd` command changes the current working directory.
- **Usage** : `cd [directory]`
- **Examples** :
  - `cd /home/user` : Changes the current directory to `/home/user`.
  - `cd ..` : Moves up one directory level.
  - `cd ~` : Changes the current directory to the user's home directory.

> Exercise: <br/>
> Use the `cd` command to change the current directory to `/usr/bin/`,
> try and guess what is this directory?!

<br/>

---

<br/>

---

<br/>

---

<br/>

3. **`pwd` : Print Working Directory.**

- **Description** : The `pwd` command displays the current working directory path.
- **Usage** : `pwd`
- **Examples** :
  - `pwd` : Prints the full path of the current directory, e.g., `/home/user/Documents`.

> Exercise: <br/>
> Use the `pwd` command to print the current working directory path and write it down.

<br/>

---

<br/>

4. **`mkdir`: Create Directory.**

- **Description** : The `mkdir` command creates a new directory.
- **Usage** : `mkdir [options] directory`
- **Examples** :
  - `mkdir new_folder` : Creates a new directory named new_folder in the
    current directory.
  - `mkdir -p /home/user/new_folder/sub_folder` : Creates the directory
    structure, including parent directories as needed.

> Exercise: <br/>
> Use the `mkdir` command to create a new directory named `cyber` and a new
> directory named `scripts` inside it, in the
> home directory.

5. **`rmdir` : Remove Directory.**

- **Description** : The `rmdir` command removes an empty directory.
- **Usage** : `rmdir [options] directory`
- **Examples** :
  - `rmdir old_folder` : Removes the directory named `old_folder` if it is empty.
  - `rmdir -p parent/child` : Removes the directory child and its parent parent if both are empty.

> Exercise: <br/>
> Use the `rmdir` command to remove the directory `cyber/scripts`.

6. **`touch` : Change File Timestamps or Create Empty File.**

- **Description** : The `touch` command changes file timestamps or creates an empty file if it doesn't exist..
- **Usage** : `touch [options] file`
- **Examples** :
  - `touch file.txt` : Creates an empty file named `file.txt`.
  - `touch -a file.txt` : Changes the access timestamp of the file.
  - `touch -m file.txt` : Changes the modification timestamp of the file.

> Exercise: <br/>
> Use the `touch` command to create a new file named `notes.md` in the
> home directory.

7. **`rm` : Remove File.**

- **Description** : The `rm` command removes a file or directory.
- **Usage** : `rm [options] file/directory`
- **Examples** :
  - `rm file.txt` : Removes the file named `file.txt`.
  - `rm -r dir` : Removes the directory named `dir` and its contents recursively.
  - `rm -i file.txt` : Prompts for confirmation before removing `file.txt`.

> Exercise: <br/>
> Use the `rm` command to remove the file `notes.md` from the home directory.

8. **`cp` : Copy Files and Directories.**

- **Description** : The `cp` command copies files and directories.
- **Usage** : `cp [options] source destination`
- **Examples** :
  - `cp file.txt new_file.txt` : Copies the file `file.txt` to `new_file.txt`.
  - `cp -r dir new_dir` : Copies the directory `dir` and its contents recursively to `new_dir`.

> Exercise: <br/>
> Use the `cp` command to copy the file `.zshrc` to a new directory named `backups`.

9. **`mv` : Move/Rename Files and Directories**

- **Description** : The `mv` command moves or renames files and directories.
- **Usage** : `mv [options] source destination`
- **Examples** :
  - `mv file.txt /home/user/Documents/` : Moves `file.txt` to the specified directory
  - `mv oldname.txt newname.txt` : Renames `oldname.txt` to `newname.txt`.

> Exercise: <br/>
> Make a new file in the home directory named `notes.md` and move it to the
> `backups` directory and rename it to `readme.md`.
