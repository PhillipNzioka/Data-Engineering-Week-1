## Introduction

Linux is a free, open-source Unix-like operating system (created by Linus Torvalds in 1991) that powers most cloud infrastructure and big data tools. It's essential for data engineers due to its stability, performance, and automation capabilities.

## Why Linux Matters for Data Engineering

1. **Cloud & Tool Dominance**: Runs on AWS, Azure, GCP, Docker, Kubernetes, Spark, Kafka, and Airflow

2. **Automation**: Enables ETL pipeline automation via Bash scripting and CRON jobs

3. **Command Line Skills**: Requires mastery of navigation, process management, and text editing

4. **File System & Permissions**: Critical for secure data handling with chmod and chown

### Linux Essentials

#### Basic SSH connection
SSH (Secure Shell) lets you open an encrypted terminal session to a remote machine, typically with `ssh @<server_address>`.

```ssh
ssh <username>@<server_address>
```
![ssh login](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fws3rlc2asj4vf7phdf1.png)

### Creating a user to a Linux server

To create a new user, all you have to do is ‘useradd‘ or ‘adduser‘ with ‘username’. The ‘username’ is a user login name, that is used by user to login into the system.

<img width="960" height="397" alt="adduser" src="https://github.com/user-attachments/assets/c67202ca-b917-4237-a918-49268c0f25ec" />

Once you’ve created the user, you can switch to the created user

```shell
su - phillipN
```
Once logged in it's important to make sure the server is upto date.

![Update](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3l3pd5c27bju5p0unamb.png)

Once in a while you can upgrade the server.

![Upgrade](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b5i8atmju8xoyjdo0555.png)

####File Operations	

`ls`
Lists directory contents

![List directory](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/77ewt6t5e82xeox1txlt.png)


`cd`
Navigates through directories

`cd <directory_path>`  → Moves to directory
`cd ..` → Goes up one level
`cd` → Returns to home

![Change directory](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ue84r3fmokoohlj8tr1e.png)


`pwd`
Prints working directory

![Current directory](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/672c2nxmsn6un0fg4u87.png)


`mkdir`
Creates a directory

`mkdir <directory_name>` → Makes directory 
`mkdir -p <path>` → Creates parent directories

![Create directory](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/453nm2c2y7tftqagsaox.png)


`mv`
mv does two jobs depending on what you give it as the destination. If you give it a new path, it moves the file there. If you give it a new name in the same folder, it renames it. Either way, the original is gone.

`cp`
Used to copy files and directories from a source to a destination.  The basic syntax is cp [options] source destination.

>Common Usage

Copy a single file: `cp file1.txt file2.txt` creates a copy named _file2.txt_. 
Copy to a directory: `cp file.txt /backup/` copies the file into the specified directory. 
Copy directories: Use the `-r` or `-R` (recursive) flag to copy folders: `cp -r dir1 dir2`

![Copy Files/Folders](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r6soa4np5aw06t6lbwso.png)

`rm`
Deletes files or folders permanently.

`rm <file>` → Delete file
`rm -r <dir>` → Delete recursively 
`rm -i <file>` → Confirm before delete

![Delete Files/Folders](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8je7g8bhmfqw52g9j9vq.png)


`touch`
Creates empty files.

![touch](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4ohqszc36ysw1moh3gzb.png)

#### System Monitoring & Performance

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


### Vim Editor Essentials

- Modal editing: Normal mode (navigation) and Insert mode (typing)
- Key shortcuts: `i` (insert), `Esc` (exit mode), `:wq` (save & quit), `:q!` (quit without saving)
- Navigation: `h/j/k/l` (arrows), `gg` (top), `G` (bottom), `/pattern` (search)

### Critical Security & Maintenance

- Use SSH keys instead of passwords for authentication
- Create non-root users with `sudo` privileges
- Regular system updates: `apt update && apt upgrade`

### PostgreSQL Installation & Configuration for Data Engineering

####1. Install PostgreSQL (Only If Not Already Installed)

```shell
# Check if PostgreSQL is already installed
psql --version

# If not installed, update package list and install
sudo apt update
sudo apt install -y postgresql postgresql-contrib

# Verify installation
systemctl status postgresql

```

#### 2. Configure PostgreSQL to Allow Remote Traffic

```shell
# Edit the main PostgreSQL configuration file
sudo nano /etc/postgresql/*/main/postgresql.conf

# Find and change the listen_addresses line:
# From: #listen_addresses = 'localhost'
# To:   listen_addresses = '*'

# Save and exit (:wq in vim or Ctrl+X then Y in nano)

```

```shell
# Edit the client authentication configuration file
sudo nano /etc/postgresql/*/main/pg_hba.conf

# Add this line at the end to allow connections from any IP
host    all             all             0.0.0.0/0               md5

# For specific subnet (more secure), use:
# host    all             all             192.168.1.0/24         md5

```

#### 3. Configure Firewall to Allow PostgreSQL Traffic (Port 5432)

```shell
# Allow PostgreSQL default port through firewall
sudo ufw allow 5432/tcp

# Reload firewall to apply changes
sudo ufw reload

# Verify firewall rules
sudo ufw status verbose

```
#### 4. Restart PostgreSQL and Check Status

```shell
# Restart PostgreSQL to apply configuration changes
sudo systemctl restart postgresql

# Check service status
sudo systemctl status postgresql

# Check if PostgreSQL is listening on all interfaces
sudo netstat -tulpn | grep 5432
# OR
ss -tulpn | grep postgres

```
#### 5. Create Database and Schema

```sql
# Switch to postgres system user
sudo -i -u postgres

# Create database 'phillipdb'
CREATE DATABASE phillipdb;

# Connect to the new database
\c phillipdb

```

```sql
-- Inside psql, create schema 'staging'
CREATE SCHEMA staging;

-- Verify schema was created
\dn

```

#### 6. Add Data to the Staging Schema

```shell
# Connect to phillipdb with staging schema
psql -d phillipdb

```

```sql
-- Create a sample table in staging schema
CREATE TABLE staging.customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Insert sample data
INSERT INTO staging.customers (name, email) VALUES 
    ('John Doe', 'john.doe@example.com'),
    ('Jane Smith', 'jane.smith@example.com'),
    ('Phillip N', 'phillip.n@example.com');

-- Query to verify data
SELECT * FROM staging.customers;

-- Import data from CSV file (common data engineering task)
COPY staging.customers (name, email)
FROM '/data/customers.csv'
DELIMITER ','
CSV HEADER;

```


#### SCP(Secure Copy Protocol)
It is a Linux command-line utility used to securely transfer files and directories between a local host and a remote host, or between two remote hosts.  It operates over an SSH (Secure Shell) connection, ensuring that all data is encrypted during transit.

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


## Conclusion
Linux is not just an optional skill for data engineers—it's the foundational operating system upon which modern data engineering is built. Mastering Linux commands and concepts directly translates to the ability to build, deploy, and maintain robust data pipelines in production environments.
