---
title: "Comprehensive Guide: Backing Up and Recovering Data in MariaDB"
datePublished: Wed Jan 03 2024 12:00:10 GMT+0000 (Coordinated Universal Time)
cuid: clqxq7x2u000o09id5wf39ocu
slug: comprehensive-guide-backing-up-and-recovering-data-in-mariadb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689350473167/673d655e-a4c7-45fc-a331-4632d606acaa.png
tags: mariadb, backup, systemd, databasemanagement, automatic-backups

---

Ensuring the safety and integrity of data in your database management system should be one of the highest priorities. MariaDB, a powerful and widely used open-source relational database system, provides robust mechanisms for backing up and recovering data. This comprehensive guide aims to equip you with the knowledge and practical know-how to implement a reliable backup strategy in MariaDB. From installing the backup utility to configuring automatic backups, creating backup scripts, performing manual backups, and efficiently recovering data, this article covers it all. Keep reading to learn how to safeguard your valuable data in MariaDB.

## Backups

### **Install the Backup Utility**

```bash
sudo apt update
sudo apt install -y mariadb-backup
```

### **Create a Backup Script**

Create a backup script at `/usr/local/bin/mariadb-backup-job.sh`.

```bash
#!/bin/bash
set -euo pipefail

BACKUP_DIR="/var/backups/mariadb"
mkdir -p "$BACKUP_DIR"

FILENAME="$BACKUP_DIR/backup_$(date -u +%Y%m%dT%H%M%SZ).gz"

# Run backup
if ! mariadb-backup --backup --stream=xbstream | gzip > "$FILENAME"; then
    echo "Backup failed: mariadb-backup command error" >&2
    rm -f "$FILENAME"
    exit 1
fi

# Verify file created
if [ ! -f "$FILENAME" ]; then
    echo "Backup failed: No backup file created" >&2
    exit 1
fi

# Size check
if [ $(stat -c "%s" "$FILENAME") -lt 100000 ]; then
    echo "Backup failed: File too small, likely incomplete" >&2
    rm -f "$FILENAME"
    exit 1
fi

# Compression corruption check
if ! gzip -t "$FILENAME" 2>/dev/null; then
    echo "Backup failed: Corrupt gzip stream" >&2
    rm -f "$FILENAME"
    exit 1
fi

echo "Backup completed successfully: $FILENAME"

# Purge backups older than 14 days
find "$BACKUP_DIR" -type f -name "backup_*.gz" -mtime +14 -print -delete
```

Some notes about this script:

* It will store backup files at `/var/backups/mariadb`. Feel free to change the path to suit your needs. Be aware that there are multiple places in the script where you'll need to change it.
    
* The filename of each backup with be in the format of `backup_20230714T0853Z.gz`. The part after `backup_` is the time the backup was started in UTC. Feel free to change the filename on the `FILENAME=` line.
    
* Normally, if there is an error backing up the database, a (very small) file is still created. The script will check if the file is too small. If so, it assumes there was an error so it deletes the file and reports an error.
    
* The script will also delete backup files older than 14 days. You can change the number of days by changing `+14` to whatever you like. You can also remove this part of the script entirely if you never want to remove old backups. Just delete the line that starts with `NUM_BACKUPS=` and the `if` block that comes after it.
    

Make the script executable.

```bash
sudo chmod +x /usr/local/bin/mariadb-backup-job.sh
```

### **Configure Automatic Backups with systemd**

I like to configure database backups to happen automatically on a schedule. My Debian Linux server uses systemd so I leverage it to set a service and a timer to run my backups.

Create `/etc/systemd/system/mariadb-backup-job.service`.

```bash
[Unit]
Description=MariaDB Backup Job
After=mariadb.service
Requires=mariadb.service

[Service]
Type=oneshot
ExecStart=/usr/local/bin/mariadb-backup-job.sh
```

Create `/etc/systemd/system/mariadb-backup-job.timer`.

```bash
[Unit]
Description=Run MariaDB Backup Job Daily

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```

This timer triggers daily at midnight. You can change this. Here are some examples:

* If you want to trigger at 5 AM, use `OnCalendar=*-*-* 05:00:00`
    
* If you want to trigger at midnight every 7 days, use `OnCalendar=*-*-* 00:00:00/7`
    
* If you want to trigger at 6 AM and 6 PM, use `OnCalendar=*-*-* 06,18:00:00`
    

Reload the system daemon to pick up the new service and timer.

```bash
sudo systemctl daemon-reload
```

Enable the timer so it starts when the system starts.

```bash
sudo systemctl enable mariadb-backup-job.timer
```

Start the timer now.

```bash
sudo systemctl start mariadb-backup-job.timer
```

At this point, all MariaDB databases will be automatically backed up to `/var/backups/mariadb` on the schedule you set.

### **Manually Backing Up Databases**

You can back up databases at any time.

```bash
sudo mariadb-backup --user=root --backup --stream=xbstream | sudo gzip > /var/backups/mariadb/backup_$(date -u +%Y%m%dT%H%M%SZ).gz
```

Or you could run the backup script.

```bash
sudo /usr/local/bin/mariadb-backup-job.sh
```

> Running the backup script will run the entire script, including the command which removes backups 14 days or older.

## Recovery

### **Take a Manual Backup**

Before continuing with data recovery, I highly recommend creating a manual backup, in case you need to revert to the state before recovery started.

### **Recover the Data**

Create a directory for storing the recovered files temporarily.

```bash
sudo mkdir /var/backups/mariadb/recovered
```

You can change `/var/backups/mariadb/recovered` to any directory you like. If you do, make sure to use your directory in all of the following commands.

Unzip the compressed database backup in the directory.

```bash
sudo gunzip -c /var/backups/mariadb/backup_20230430T031458Z.gz | sudo mbstream -x --directory=/var/backups/mariadb/recovered
```

Change `/var/backups/mariadb/backup_20230430T031458Z.gz` to the actual path of your backup file.

Prepare the recovery files. This is necessary because files created at backup time are not point-in-time consistent and MariaDB will reject the recovery if not properly prepared.

```bash
sudo mariadb-backup --prepare --target-dir=/var/backups/mariadb/recovered
```

Stop MariaDB.

```bash
sudo systemctl stop mariadb
```

Remove all existing data files.

```bash
sudo rm -rf /var/lib/mysql
```

> MariaDB is meant to be a drop-in replacement for MySQL, so it uses the MySQL name for some things.

Restore the backup files.

```bash
sudo mariadb-backup --copy-back --target-dir=/var/backups/mariadb/recovered
```

Change ownership of the newly recovered files so that the `mysql` user can access them properly.

```bash
sudo chown -R mysql:mysql /var/lib/mysql
```

Start MariaDB.

```bash
sudo systemctl start mariadb
```

The database is now restored to the state it was in when the backup was taken.

Remove the recovery files as they are no longer needed.

```bash
sudo rm -rf /var/backups/mariadb/recovered
```

Protecting your data from unforeseen events or accidental corruption is crucial for maintaining the integrity of your database. By following the steps outlined in this comprehensive guide, you can establish a robust backup strategy tailored to your needs. Remember, regularly backing up your data and understanding the recovery process *will* save you from potential headaches and downtime. With the knowledge gained from this article, you are well-equipped to secure your data and confidently navigate data backup and recovery in MariaDB.

Cover photo by [Eugene Golovesov](https://unsplash.com/@eugene_golovesov?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/a-blue-flower-with-a-white-background-dFZAFukYwlU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).