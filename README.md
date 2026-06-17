# Exercise 26 - S3 Backup Solution

## Objective

Implement a backup strategy for:

- Application files
- Configuration files

Store backups in AWS S3 and demonstrate the restore process.

---

## Architecture

Application Files + Config Files
        |
        v
   backup.tar.gz
        |
        v
     AWS S3
        |
        v
   Restore Process
        |
        v
Recovered Files

---

## Technologies Used

- AWS S3
- AWS CLI
- Linux
- Shell Script
- Git

---

## Step 1: Create Sample Application and Config Files

### Create Application Directory

```bash
mkdir app
```

### Create Config Directory

```bash
mkdir config
```

### Create Application File

```bash
echo "My Application" > app/app.txt
```

### Create Config File

```bash
echo "PORT=8080" > config/config.txt
```

### Verify

```bash
find app config
```

Output:

```text
app
app/app.txt
config
config/config.txt
```

---

## Step 2: Create Backup Archive

Command:

```bash
tar -czf backup.tar.gz app config
```

Explanation:

- c = create archive
- z = compress using gzip
- f = filename

Verify:

```bash
tar -tzf backup.tar.gz
```

Output:

```text
app/
app/app.txt
config/
config/config.txt
```

---

## Step 3: Configure AWS CLI

Command:

```bash
aws configure
```

Provide:

```text
AWS Access Key
AWS Secret Key
Region
Output Format
```

Verify:

```bash
aws s3 ls
```

---

## Step 4: Upload Backup to S3

Command:

```bash
aws s3 cp backup.tar.gz s3://sanjay-backupbt-26/
```

Verify:

```bash
aws s3 ls s3://sanjay-backupbt-26/
```

Output:

```text
backup.tar.gz
```

---

## Step 5: Simulate Disaster

Delete application and config files.

Command:

```bash
rm -rf app config
```

Verify:

```bash
ls
```

Output:

```text
backup.tar.gz
```

---

## Step 6: Restore Backup

Download backup from S3.

Command:

```bash
aws s3 cp s3://sanjay-backupbt-26/backup.tar.gz .
```

Restore:

```bash
tar -xzf backup.tar.gz
```

Verify:

```bash
find app config
```

Output:

```text
app
app/app.txt
config
config/config.txt
```

Restore completed successfully.

---

## Automation Using Shell Script

Created file:

```bash
backup.sh
```

Content:

```bash
#!/bin/bash

DATE=$(date +%F)

tar -czf backup-$DATE.tar.gz app config

aws s3 cp backup-$DATE.tar.gz s3://sanjay-backupbt-26/
```

Make executable:

```bash
chmod +x backup.sh
```

Run:

```bash
./backup.sh
```

---

## Challenges Faced and Solutions

### Issue 1

Command:

```bash
rm app
```

Error:

```text
rm: cannot remove 'app': Is a directory
```

Reason:

`rm` cannot remove directories directly.

Solution:

```bash
rm -rf app config
```

---

### Issue 2

Command:

```bash
tree
```

Error:

```text
Command 'tree' not found
```

Solution:

```bash
sudo apt install tree -y
```

Alternative:

```bash
find .
```

---

### Issue 3

Command:

```bash
find.
```

Error:

```text
Command 'find.' not found
```

Reason:

Space missing between command and argument.

Solution:

```bash
find .
```

---

### Issue 4

Git Push Authentication

GitHub requested:

```text
Username:
Password:
```

Reason:

GitHub no longer accepts account passwords for git push.

Solution:

Generate GitHub Personal Access Token (PAT) and use it instead of account password.

---

## Outcome

Successfully implemented:

- Backup of application files
- Backup of configuration files
- Upload to AWS S3
- Restore from AWS S3
- Shell script automation
- Disaster recovery validation

---

## Skills Learned

- AWS S3
- AWS CLI
- Linux File Management
- Backup and Restore
- Disaster Recovery
- Shell Scripting
- Git and GitHub
