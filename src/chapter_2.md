# Chapter 2: More Linux commands

More Linux commands to make you feel comfortable in the terminal.

1. **`grep` : Global Regular Expression Print**

- **Description**: `grep` searches for patterns within files and displays matching lines.
- **Usage**: `grep [options] pattern [file...]`
- **Examples:**
  - `grep 'search_term' file.txt`: Searches for `search_term` in `file.txt`.
  - `grep -r 'search_term' directory/`: Recursively searches for `search_term` in `directory/`.

> Exercise : <br/>
> Use the `grep` command to search for the string `touch` in the `.history` file.

2. **`find` : Search for Files in a Directory Hierarchy**

- **Description** : `find` searches for files and directories based on conditions.
- **Usage** : `find [path...] [expression]`
- **Examples** :
  - `find . -name "filename.txt"`: Searches for `filename.txt` in the current directory
  - `find /home/user -type d`: Searches for directories in `/home/user`

> Exercise : <br/>
> Use the `find` command to find the file named `grub` in the `/etc` directory,
> Do you know what is this file?!

<br/>

---

<br/>

---

<br/>

---

<br/>

3. **`cat` : Concatenate and Display Files**

- **Description** : `cat` concatenates and displays files.
- **Usage** : `cat [options] file...`
- **Examples** :
  - `cat file.txt`: Displays the contents of `file.txt`.
  - `cat -n file.txt`: Displays the contents of `file.txt` with line numbers.
  - `cat file1.txt file2.txt`: Concatenates the contents of `file1.txt` and `file2.txt` and displays the result.
  - `cat file1.txt file2.txt > combined.txt`: Combines `file1.txt` and `file2.txt` into `combined.txt`.

> Exercise : <br/>
> Use the `cat` command to concatenate the `.zshrc` and `.bashrc` files into `combinedrc.txt`,
> and display it.

4. **`whatis`: Display One-line Manual Page Descriptions**

- **Description** : `whatis` provides a brief description of a command.
- **Usage** : `whatis [command]`
- **Examples** :
  - `whatis which`: Displays the description of the `which` command.

> Exercise : <br/>
> Use the `whatis` command to learn about the `more` and `less` commands,
> and write down there brief description.

<br/>

---

<br/>

---

<br/>

---

<br/>

5. **`df` : Display Disk Space Usage**

- **Description** : `df` reports file system disk space usage.
- **Usage** : `df [options] [path]`
- **Examples** :
  - `df`: Displays disk space usage for the current directory.
  - `df -h`: Displays disk space usage in human-readable format.
  - `df -h /`: Displays disk space usage for the root directory.

> Exercise : <br/>
> Use the `df` command to see how mush disk space is used in the home directory.

6. **`uptime` : Tell How Long the System Has Been Running**

- **Description** : `uptime` displays the current time, how long the system
  has been running,the number of users, and system load averages.
- **Usage** : `uptime`
- **Examples** :
  - `uptime`: Displays the current system uptime and load averages.
  - `uptime -p`: Displays the current system uptime in pretty format.

> Exercise : <br/>
> Use the `uptime` command to find out how long the system has been running.

7. **`wget` and `curl` : Download Files from the Internet**

- **Description** : `wget` and `curl` are command-line tools for downloading files from the internet.
- **Usage** : `wget [options] url` or `curl [options] url`
- **Examples** :
  - `wget https://example.com/file.zip`: Downloads the file `file.zip` from the internet.
  - `curl -O https://example.com/file.zip`: Downloads the file `file.zip` from the internet.

> Exercise: <br/>
> Use the `wget` or `curl` command to download the `https://github.com/Al-aqua/op-lab-man/blob/main/absolutely-not-a-virus` file from the internet.
> what is the content of the file?

<br/>

---

<br/>

---

<br/>

---

<br/>

8. **Package Management with `apt` in Debian based distros**

- **Description** : `apt` is a package management system for Debian-based systems.
- **Usage** : `apt [options] [command]`
- **Examples** :
  - `sudo apt update`: Updates the package list.
  - `sudo apt upgrade`: Upgrades all installed packages.
  - `apt install [package]`: Installs a package.
  - `apt remove [package]`: Removes a package.

> Exercise : <br/>
> Use the `apt` command to install the `vim` package.
> what is `vim`?

<br/>

---

<br/>

---

<br/>

---

<br/>
