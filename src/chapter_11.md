# Chapter 14: Security and Protection

---

## 1. Introduction to OS Security

Security in an operating system (OS) is crucial for protecting system resources like files, processes, and hardware from unauthorized access and malicious activities. In Linux, security is enforced through access control, user authentication, and network security.

Key security principles:

- **Confidentiality**: Ensures sensitive data is accessible only to authorized users.
- **Integrity**: Protects data from unauthorized modifications.
- **Availability**: Ensures that system resources are available to legitimate users.

---

## 2. Access Control Models

Access control in Linux determines how users interact with files, directories, and other system resources. Linux primarily uses **Discretionary Access Control (DAC)**, where the owner of a file or directory sets access permissions.

### 2.1 File Permissions in Linux

Linux employs a permission system that defines access for the **owner**, **group**, and **others**. Permissions include:

- **Read (r)**: View file contents.
- **Write (w)**: Modify file contents.
- **Execute (x)**: Run the file as a program.

To view permissions, use the `ls -l` command:

```bash
ls -l filename
```

To modify permissions, use the chmod command:

```bash
chmod 755 filename
```

This allows the owner to read, write, and execute the file, while others can only read and execute it.

### 2.2 File Ownership

Files and directories have an **owner** and an associated **group**. Ownership can be modified using the `chown` command:

```bash
chown user:group filename
```

Properly managing file permissions and ownership is critical for maintaining system security.

---

## 3. User Authentication

User authentication ensures that only authorized users can access the system.

### 3.1 Password Authentication

Linux stores password hashes in the `/etc/shadow` file. Authentication occurs by comparing the user's input with the stored hash.

### 3.2 SSH Authentication

SSH (Secure Shell) provides a secure method for remote login to a Linux system. SSH can be configured for password-based or key-based authentication, with key-based authentication being more secure.

#### Configuring SSH Key-Based Authentication:

1. Generate an SSH key pair on the client machine:

   ```bash
   ssh-keygen -t rsa
   ```

2. Copy the public key to the server:

   ```bash
   ssh-copy-id username@server
   ```

3. Verify that SSH is running on the server:

   ```bash
   sudo systemctl status sshd
   ```

4. Connect to the server using the public key:

   ```bash
   ssh username@server
   ```

By default, SSH listens on port 22, but you can change this for additional security by editing the `/etc/ssh/sshd_config` file.

---

## 4. Firewalls in Linux

A firewall controls incoming and outgoing network traffic, enhancing system security by allowing or denying access based on predefined rules. Linux supports multiple firewall tools, including `iptables` and `ufw` (Uncomplicated Firewall).

### 4.1 Introduction to `ufw` (Uncomplicated Firewall)

`ufw` is a user-friendly firewall tool available on many Linux distributions. It simplifies the process of managing firewall rules.

**Basic ufw Commands:**

- **Enable the firewall:**
  ```bash
  sudo ufw enable
  ```
- **Check the status:**
  ```bash
  sudo ufw status
  ```
- **Allow SSH traffic (port 22):**
  ```bash
  sudo ufw allow ssh
  ```
- **List all rules:**
  ```bash
  sudo ufw status numbered
  ```
- **Disable the firewall:**
  ```bash
  sudo ufw disable
  ```
- **Deny all incoming connections:**
  ```bash
  sudo ufw default deny incoming
  ```

### 4.2 Advanced Firewall Configuration with `iptables`

For more granular control over network traffic, `iptables` can be used. It is a powerful firewall tool that allows administrators to set specific rules for handling traffic based on IP addresses, ports, and protocols.

For example, to allow SSH traffic using iptables:

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

While `ufw` is easier to manage, `iptables` offers more flexibility for advanced users.

---

## 5. Threats and Common Attacks

There are several common threats that can target a Linux system:

- **Brute-force attacks**: Automated attempts to guess passwords or SSH keys.
- **Denial-of-Service (DoS) attacks**: Overloading the system with traffic, making it unavailable to legitimate users.
- **Privilege escalation**: Exploiting vulnerabilities to gain unauthorized access to higher privilege levels.

By properly configuring firewalls and SSH, these threats can be mitigated effectively.
