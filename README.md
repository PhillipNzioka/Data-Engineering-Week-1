# Data-Engineering-Week-1
Summary of what I have learnt in one week as a data engineering student

# What is Linux?
Linux is a free, open-source operating system by Linus Torvalds in 1991. It is a Unix-like system based on a monolithic kernel that manages hardware resources likes the CPU, memory, and storage, serving as the bridge between software applications and physical hardware.

Linux is the foundational operating system for modern data engineering, powering the vast majority of cloud infrastructure (AWS, Azure, GCP) and big data tools like Spark, Kafka, and Airflow.  It is essential for data engineers because it provides the stability, performance, and automation capabilities required to manage large-scale data pipelines and remote servers efficiently.

## Key aspects of Linux in data engineering include:

-**Cloud and Tool Dominance**: Nearly all cloud virtual machines, containers (Docker/Kubernetes), and distributed compute systems run on Linux by default. 
-**Automation and Scripting**: Linux enables seamless automation of ETL pipelines and data tasks through Bash shell scripting and CRON jobs. 
-**Core Command Line Skills**: Data engineers must master terminal commands for file navigation (ls, cd), process management (ps, top), and text editing (nano, vi) to debug pipelines and configure systems directly on servers. 
-**File System and Permissions**: Understanding the Linux tree structure and managing file permissions (chmod, chown) is critical for handling sensitive data and ensuring secure data movement between systems.

## Linux Essentials

#### Basic SSH connection
SSH (Secure Shell) lets you open an encrypted terminal session to a remote machine, typically with `ssh @<server_address>`.
On first connect, you’ll be asked to verify the server’s host key, then authenticate with a password or—preferably—an SSH key you created with ssh-keygen and installed via ssh-copy-id.
If the server uses a custom port, add `-p` , and if you need a specific key file, use `-i <path_to_private_key>`.
For convenience, you can define a short alias in `~/.ssh/config` so you can just run `ssh myserver`.

```ssh
ssh <username>@<server_address>
```

_Example: connect as user 'root' to server at 159.65.222.96 and port 22_ 

![ssh login](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fws3rlc2asj4vf7phdf1.png)

#### Adding a user to a Linux server

To add/create a new user, all you have to do is ‘useradd‘ or ‘adduser‘ with ‘username’. The ‘username’ is a user login name, that is used by user to login into the system.

Only one user can be added and that username must be unique (different from other username already exists on the system).

For example, to add a new user called ‘phillipN‘, use the following command.



![Adding user to Linux server](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mqqj873wkat8a089eksu.png)

The next thing is to make the user a sudoer if you want the user to perform sudo related actions.

```shell
usermod -aG admin phillipN
```

Once you’ve done that, you now need to switch to the created user with the following command.

![Log in to the user](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ouq3qrlpxb4ol21beu8a.png)

You should now be logged in as the user.
Once logged in it's important to make sure the server is upto date.

![Update](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3l3pd5c27bju5p0unamb.png)

Once in a while you can upgrade the server.

![Upgrade](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b5i8atmju8xoyjdo0555.png)

#### File & Directory Management

**ls (List)**
Lists directory contents.

`ls` → Lists files and directories in the current directory
`ls -l` → Long format (permissions, owner, size, etc.)
`ls -a` → Shows hidden files (dotfiles)
`ls -lh` → Human-readable sizes (KB, MB, GB)

>Example: 

![List directory](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/77ewt6t5e82xeox1txlt.png)

**pwd (Print Working Directory)**
Displays the current directory path.

![Current directory](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/672c2nxmsn6un0fg4u87.png)

**cd (Change Directory)**
Navigates through directories.

`cd` <directory_path> → Moves to directory
`cd ..` → Goes up one level
`cd` → Returns to home

>Example: 

![Change directory](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ue84r3fmokoohlj8tr1e.png)


**mkdir (Make Directory)**
Creates new directories.

`mkdir <directory_name>` → Makes directory 
`mkdir -p <path>` → Creates parent directories

>Example: create a directory named sales 


![Create directory](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/453nm2c2y7tftqagsaox.png)

**mv (Move/Rename)**
Moves files and directories from one location to another or to rename them.

Move a file: `mv file.txt /path/to/destination/` moves the file into the specified directory. 
Rename a file: `mv old_name.txt new_name.txt` changes the file's name in place. 


![Move/Rename](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/hy7yk45d4cmhilyys4ai.png)

**cp (Copy)**
Used to copy files and directories from a source to a destination.  The basic syntax is cp [options] source destination.

>Common Usage

Copy a single file: `cp file1.txt file2.txt` creates a copy named _file2.txt_. 
Copy to a directory: `cp file.txt /backup/` copies the file into the specified directory. 
Copy directories: Use the `-r` or `-R` (recursive) flag to copy folders: `cp -r dir1 dir2`


![Copy Files/Folders](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r6soa4np5aw06t6lbwso.png)

**scp(Secure Copy Protocol)** is a Linux command-line utility used to securely transfer files and directories between a local host and a remote host, or between two remote hosts.  It operates over an SSH (Secure Shell) connection, ensuring that all data is encrypted during transit.

##### Key Syntax and Usage
The general syntax is `scp [options] source destination`. The source and destination can be local paths or remote paths formatted as `[user@]host:/path`. 

- Local to Remote: `scp file.txt user@remote_host:/remote/path/`
- Remote to Local: `scp user@remote_host:/remote/file.txt ./local/path/`
- Remote to Remote: `scp user@host1:/path/file.txt user@host2:/path/` 

##### Common Options

`-r`: Recursively copy entire directories. 
`-P port`: Specify a non-standard SSH port (note the capital P). 
`-p`: Preserve file modification times and permissions. 
`-q`: Quiet mode; suppresses progress meter and warnings. 
`-C`: Enable compression to speed up transfers.

To copy files from a Linux server to a Windows machine using SCP, you must run the command from the Windows machine to "pull" the file, as Windows typically does not run an SSH server by default. 

##### _Using Built-in OpenSSH (Windows 10/11)_

Modern Windows versions include a native SCP client. Open Command Prompt or PowerShell and use the following syntax:

```shell
scp username@linux_server_ip:/path/to/remote/file C:\path\to\local\destination

```
- **_username_**: The Linux user account. 
- **_linux_server_ip_**: The IP address or hostname of the Linux server. 
- **_/path/to/remote/file_**: The absolute path on the Linux server. 
- **_C:\path\to\local\destination_**: The local Windows directory where the file will be saved.

Copying files from Windows machine to a specific user on a Linux server using scp

![scp -r](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qq2fjj4k830bkhm08gkd.png)

**rm (Remove)**
Deletes files or directories.

`rm <file>` → Delete file
`rm -r <dir>` → Delete recursively 
`rm -i <file>` → Confirm before delete

![Delete Files/Folders](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8je7g8bhmfqw52g9j9vq.png)

**touch**
Creates empty files or updates timestamps.

![touch](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4ohqszc36ysw1moh3gzb.png)

`clear`
Clears the terminal screen.

##### System Information

`uname` (Unix Name)
Displays system info.

`uname -a` → Full details
`uname -r` → Kernel release


![System info.](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8ve10gva7xyqxohzt5b2.png)

`whoami`
Shows current username.


![whoami](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0kzf0b7qsx1f4qin2b3s.png)

##### Networking

`ssh`
Remote login.

```ssh
ssh user@<host adrress> -p
```

`ifconfig` / `ip addr`
Show interfaces.

`traceroute`
It is a network diagnostic tool that tracks the route packets take from a local machine to a specified host by utilizing the IP protocol's Time To Live (TTL) field.  It sends probe packets with incrementally increasing TTL values to elicit ICMP TIME_EXCEEDED responses from each gateway, thereby revealing the path and latency of each hop.

`wget`
Download files.

```shell
wget https://example.com/file
```

`ufw` / `iptables`
Firewall management.

![ufw](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xa1fo7reodaou6nv4268.png)

##### Package Management
`apt` (Debian/Ubuntu)
`apt update`
`apt install nginx`

![update linux server](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fsrdbsdjobe1h1qjcpyd.png)

##### System Monitoring & Performance

`top` / `htop`
Process monitoring.

![Process monitoring](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rydd53c5opt8cjtmubif.png)

`kill`
Terminate/end process.

![Terminate process](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qs74melfitu117rhw4ci.png)

`free`
Shows memory usage.

![Memory usage](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/j5yo2enbosed02q2ctnx.png)

`df / du`
Disk usage.

![Disk usage](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/q8xo6lmhr411q8uktr2k.png)

`uptime`
Shows system uptime.

![uptime](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/bh0x1xy670uycmrrvvah.png)

`dmesg`
Kernel messages.

`shutdown` / `reboot`
System power control.

##### User Management
`useradd` → add a new user to the linux server

`userdel` → remove a user from the server

`usermod` → Modify user account

`passwd` → Change password.

##### Miscellaneous

`history`
Displays command history.

![Command history](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/iay3ud0ao0h6jcl7j1s1.png)

`clear`
Clears screen.

`exit`
Exits the session.

![Exit session](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/66hxeqtygl1cpzkmtg9b.png)

## Vim
Vim (short for Vi IMproved) is a free and open-source text editor originally created by Bram Moolenaar in 1991 as an enhanced version of the classic vi editor.  It is widely used for software development, system administration, and general text editing, particularly within Unix-like environments (Linux, BSD, macOS) where it is often pre-installed. 

Vim is characterized by its modal editing approach, which distinguishes between modes such as **Normal** (for navigation and commands) and **Insert** (for typing text), allowing users to edit efficiently using only the keyboard without relying on a mouse.  It is highly configurable and extensible through plugins and scripting, supports syntax highlighting, and runs in both terminal and graphical (gVim) interfaces.

#### Vim Cheat Sheet

##### Mode Switching

Command | Explanation
---|---
`i` | Enters Insert Mode before the cursor (to type text).
`a` | Enters Insert Mode after the cursor (append after cursor).
`o` | Enters Insert Mode on a new line below the current one.
`O` | Enters Insert Mode on a new line above the current one.
`v` | Enters Visual Mode (character-wise selection).
`V` | Enters Visual Line Mode (selects entire lines).
`Ctrl + v` | Enters Visual Block Mode (rectangular selection).
`Esc` | Exits any mode back to Normal Mode (the default mode).

#### Navigation (Normal Mode)

Command | Explanation
---|---
`h` | Moves the cursor left.
`j` | Moves the cursor down.
`k` | Moves the cursor up.
`l` | Moves the cursor right.
`w` | Moves to the start of the next word.
`b` | Moves to the start of the previous word.
`e` | Moves to the end of the current word.
`0`  | Moves to the start of the line.
`$` | Moves to the end of the line.
`^` | Moves to the first non-blank character of the line.
`gg` | Moves to the first line of the file.
`G` | Moves to the last line of the file.
`:<num>` | Jumps to the specified line number (e.g., `:50`).
`%` | Jumps to the matching parenthesis, bracket, or brace.

#### Search and Replace

Command | Explanation
---|---
`/ <pattern>` | Searches forward for `<pattern>`.
`? <pattern>` | Searches backward for `<pattern>`.
`n` | Moves to the next search result.
`N` | Moves to the previous search result.
`*` | Searches for the word under the cursor (forward).

#### Saving and Exiting (Command Mode: `:`)

Command | Explanation
---|---
`:w` | Writes (saves) the file.
`:w <filename>` | Saves the file with a new name.
`:q` | Quits the file (only if no changes have been made).
`:wq` | Writes and quits (saves and exits).
`:x` | Equivalent to `:wq`.
`:q!` | Quits without saving (discards changes).
`:wqa` | Writes and quits all open files/windows.









