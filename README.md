# Linux Assessment Submission

## Candidate Details

| Field | Details |
|---|---|
| **Name** | SHIWANSHU KUMAR JHA |
| **Mobile Number** | 7979756698 |
| **Email ID** | shiwanshukumarjha7@gmail.com |
| **Git Repository** | https://github.com/shiwanshu97/Linux_Assessment.git |

---

# Question 1: Set Up Your DevOps Project Structure

## Objective

Create a complete project directory from scratch, apply correct permissions, and set ownership. The structure built here will be used directly by the script in Question 2.

---

## Tasks Performed

### 1. Create Project Directory Structure

Created the directory `/home/ec2-user/webapp/` with the following subdirectories using a single `mkdir -p` command:

- `scripts/`
- `logs/`
- `config/`

### Command Used

```bash
mkdir -p /home/ec2-user/webapp/{scripts,logs,config}
```

### Explanation

Used `mkdir -p` to create the `webapp` directory along with the `scripts`, `logs`, and `config` subdirectories in a single command.

---

### 2. Create Configuration File

Created the file `config/app.conf` using `cat >` and added the following content:

```text
APP_NAME=WebApp
PORT=8080
```

### Command Used

```bash
cat > /home/ec2-user/webapp/config/app.conf
```

### Explanation

Used `cat >` to create `app.conf` and added application configuration settings such as application name and port number.

---

### 3. Create Empty Log File

Created an empty log file at `logs/app.log` using `touch`.

### Command Used

```bash
touch /home/ec2-user/webapp/logs/app.log
```

### Verification Command

```bash
ls -l /home/ec2-user/webapp/logs/app.log
```

### Explanation

Used `touch` to create an empty `app.log` file and verified that it was `0 bytes` using `ls -l`.

---

### 4. Set File Permissions

Applied permissions to the scripts directory and configuration file.

### Commands Used

```bash
chmod 755 /home/ec2-user/webapp/scripts
chmod 644 /home/ec2-user/webapp/config/app.conf
```

### Explanation

Applied `chmod 755` to the `scripts` directory and `chmod 644` to `app.conf` to control access permissions for owner, group, and others.

---

## Meaning of Permission 755

```text
7 = rwx  → Owner can Read, Write, Execute
5 = r-x  → Group can Read and Execute
5 = r-x  → Others can Read and Execute
```

### Summary

- Owner has full access
- Group can only read and execute
- Others can only read and execute

`755` is commonly used for directories and executable files.

---

## Meaning of Permission 644

```text
6 = rw-  → Owner can Read and Write
4 = r--  → Group can only Read
4 = r--  → Others can only Read
```

### Summary

- Owner can modify the file
- Group can only view the file
- Others can only view the file

`644` is commonly used for configuration and text files.

---

### 5. Change Ownership

Recursively changed ownership of the entire `webapp/` directory to `root:root`.

### Command Used

```bash
sudo chown -R root:root /home/ec2-user/webapp
```

### Verification Command

```bash
ls -lR /home/ec2-user/webapp/
```

### Explanation

Used `sudo chown -R root:root` to recursively change ownership of the entire `webapp` directory and verified it using `ls -lR`.

---

# Question 2: Write an Interactive Log Script

## Objective

Using the `webapp/` structure from Question 1, write a bash script that takes user input, reads a config file, and writes timestamped log entries. The log entries created here will be used in Question 3.

---

## Tasks Performed

### 1. Create Bash Script

Created a new bash script file at:

```text
/home/ec2-user/webapp/scripts/log_user.sh
```

using `vim` editor and added the correct shebang line.

### Command Used

```bash
vim /home/ec2-user/webapp/scripts/log_user.sh
```

### Shebang Line

```bash
#!/bin/bash
```

### Explanation

Used Vim to create `log_user.sh` and added the shebang line `#!/bin/bash` to specify the Bash interpreter.

---

### 2. Prompt User Input

Used `read -p` to prompt the user to enter their name and stored it in a variable called `username`.

### Command Used

```bash
read -p "Enter your name: " username
```

### Explanation

Used `read -p` to take user input interactively and stored the entered value inside the `username` variable.

---

### 3. Display Configuration File

Used `cat` with the absolute path to display the contents of `config/app.conf`.

### Command Used

```bash
cat /home/ec2-user/webapp/config/app.conf
```

### Explanation

Used `cat` to display the application configuration file contents directly on the terminal.

---

### 4. Append Log Entry

Appended login details into `logs/app.log` using `echo >>`.

### Command Used

```bash
echo "Login: $username Date: $(date)" >> /home/ec2-user/webapp/logs/app.log
```

### Display Log File

```bash
cat /home/ec2-user/webapp/logs/app.log
```

### Explanation

Used `echo >>` to append login details along with the current date and time into `app.log`.

---

### 5. Give Execute Permission and Run Script

Provided execute permission to the script and ran it multiple times with different usernames.

### Commands Used

```bash
chmod +x /home/ec2-user/webapp/scripts/log_user.sh
```

### Run Script

```bash
/home/ec2-user/webapp/scripts/log_user.sh
```

### Example Usernames Used

- Chirag
- Priya
- Ravi

### Explanation

Used `chmod +x` to give execute permission to the script and executed it multiple times with different usernames to generate multiple log entries.

---

# Question 3: User Management and File Permission Control

## Objective

Create 4 Linux users. Two users must have write access to the `log_user.sh` script created in Question 2, while the other two users must have read-only access. Linux groups and `chmod` permissions were used to control access.

---

## Tasks Performed

### 1. Create a Group Called `writers`

### Command Used

```bash
sudo groupadd writers
```

### Explanation

Created a new group named `writers` using `groupadd` to manage shared file access and permissions for multiple users.

---

### 2. Create Four Users with Home Directories

### Commands Used

```bash
sudo useradd -m user1
sudo useradd -m user2
sudo useradd -m user3
sudo useradd -m user4
```

### Explanation

Created four users using the `useradd -m` command, which automatically generated separate home directories under `/home`.

---

### 3. Add Write-Access Users to Writers Group

### Commands Used

```bash
sudo usermod -aG writers user1
sudo usermod -aG writers user2
```

### Explanation

Added `user1` and `user2` to the `writers` group using `usermod -aG` so they could share group-based permissions and collaboratively access the script.

---

### 4. Change Group Ownership of Script

### Command Used

```bash
sudo chown root:writers /home/ec2-user/webapp/scripts/log_user.sh
```

### Explanation

Changed the group ownership of `log_user.sh` to `writers` so members of the group could access the file according to group permissions.

---

### 5. Set File Permissions

### Command Used

```bash
sudo chmod 664 /home/ec2-user/webapp/scripts/log_user.sh
```

### Explanation

Set permissions to `664` so the owner and writers group could read and modify the file, while others only had read access.

---

## Permission Layout (chmod 664)

```text
chmod 664 log_user.sh

       6          6          4
Owner(rw)  Group(rw)  Others(r)

   root      writers    user3,user4
```

### Meaning of 664

```text
6 = rw- → Read and Write
6 = rw- → Read and Write
4 = r-- → Read Only
```

---

### 6. Verify Permissions

### Verification Command

```bash
ls -l /home/ec2-user/webapp/scripts/log_user.sh
```

### Expected Output

```text
-rw-rw-r--  1 root writers  log_user.sh
```

### Explanation

Verified the final ownership and permissions of `log_user.sh` using `ls -l`, confirming that the file belongs to `root:writers` with `664` access rights.

---

### 7. Test User Access

Switched to each user account and tested access permissions.

### Commands Used

```bash
su - user1
su - user2
su - user3
su - user4
```

### Access Verification

| User | Group Membership | Access Level |
|---|---|---|
| user1 | writers | Read & Write |
| user2 | writers | Read & Write |
| user3 | others | Read Only |
| user4 | others | Read Only |

### Explanation

Tested file access by switching between users to confirm that:

- Members of the `writers` group had read and write access.
- Other users had read-only access according to the `664` permission layout.

---

# Conclusion

Successfully completed the Linux assessment by:

- Creating and managing Linux directories and files
- Applying file permissions and ownership
- Writing an interactive Bash script
- Managing Linux users and groups
- Implementing permission-based access control using `chmod`, `chown`, and groups

The complete project has been uploaded to the GitHub repository for submission and verification.
