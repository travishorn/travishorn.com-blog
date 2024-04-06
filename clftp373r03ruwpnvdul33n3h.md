---
title: "SurrealDB: Your Ultimate Guide to Smooth Installation and Configuration"
datePublished: Wed Mar 29 2023 13:00:39 GMT+0000 (Coordinated Universal Time)
cuid: clftp373r03ruwpnvdul33n3h
slug: surrealdb-your-ultimate-guide-to-smooth-installation-and-configuration
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680052113665/c62f8e80-dccd-440d-8b66-6145f66bbf61.png
tags: linux, databases, sql, bash-script, surrealdb

---

SurrealDB is a cutting-edge, next-generation serverless database that offers powerful features such as SQL-style query language, real-time queries, and advanced security permissions for multi-tenant access. Despite being a new platform still under heavy development, it has already captured the attention of developers looking for a powerful and ambitious project. If you're interested in using SurrealDB, this guide provides a detailed walkthrough of the installation and configuration process, including creating management scripts and scheduling backups on Linux.

This guide will assume a certain level of technical proficiency and familiarity with Linux. We will use Bash scripting, but you don't necessarily need to know how everything works to take advantage of the automated scripts.

## What is SurrealDB?

While the primary purpose of this guide is to cover some of the more technical aspects of configuring SurrealDB once you've already decided to use it, here is some basic information about *what* SurrealDB is, what makes it so great, or why you might want to use it.

I highly recommend you [visit the SurrealDB homepage](https://surrealdb.com/) as it describes some of the features that are so enticing for developers like me.

> With an SQL-style query language, real-time queries with highly-efficient related data retrieval, advanced security permissions for multi-tenant access, and support for performant analytical workloads, SurrealDB is the next generation serverless database.

Here are a few features I'd like to highlight that piqued my attention.

* Schemaless or schemafull tables with a nice syntax
    
* Record Links instead of JOINs
    
* Powerful aggregation functions and ability to pre-compute analytical queries
    
* Graph connections
    
* Querying via HTTP, REST, WebSocket, JSON-RPC and (coming soon) GraphQL
    
* Events that trigger upon data changes
    
* Realtime pub/sub over WebSockets
    
* Multi-tenancy data separation
    
* Support for geometry, datetime, and duration datatypes, functions, and operators
    
* First-class authentication and permissions
    
* Libraries for all popular languages and frameworks
    
* Built in Rust
    

Fireship did a great "SurrealDB in 100 Seconds" video.

%[https://www.youtube.com/watch?v=C7WFwgDRStM] 

## Install on Linux

Installation is available on MacOS, Linux, Windows, and via Docker. Check [the installation page](https://surrealdb.com/install) for more details. If you're using Linux like me, you can run this single line of code which executes the official installation script.

```sh
curl -sSf https://install.surrealdb.com | sh
```

By default, it will install SurrealDB to a subdirectory in your home directory. For example, my SurrealDB binary is located at `/home/travis/.surrealdb/surreal`.

## Create some management scripts

Many of the operations you'll want to perform regarding the management of your database can be automated with some basic Bash scripts. These are not necessary but highly recommended.

Create a new directory to hold our scripts

```sh
mkdir ~/.surrealdb/scripts
```

Change into that directory

```sh
cd ~/.surrealdb/scripts
```

Create a file called `common_vars.sh` to hold common variables that all our scripts will use

```sh
#!/bin/bash

# Database server will be listening on localhost at this port
surreal_db_port=8000

# Can be any username
surreal_db_user=some_root_username

# Can be any password
surreal_db_pass=some_root_password

# Where the `surreal` binary lives. Also where data files and backups will be
# written to
surreal_db_root=/home/travis/.surrealdb
```

Choose a port, username, and password that works for you. Also, make sure to set the root path to the actual path to the binary.

### Script for Starting the Server

Create a file called `start_server.sh` that we can use to start the database server

```sh
#! /bin/bash

# Import common variables like the port, username, password, etc.
source "$(dirname "$0")/common_vars.sh"

# Start the database server
$surreal_db_root/surreal start \
  --bind 0.0.0.0:$surreal_db_port \
  --auth \
  --log debug \
  --user $surreal_db_user \
  --pass "$surreal_db_pass" \
  file://$surreal_db_root/data
```

My data files will be stored in the `data` directory under the SurrealDB root directory defined in `common_vars.sh`. Use whatever directory works for you.

### Script for Accessing the REPL

A common way to interact with the database is by using the official REPL. You can use the REPL to connect to the database and issue commands or query data right from within your terminal. We can also use the REPL to pull out quick information about our databases for use in automated scripts.

Create a file called `repl.sh` that will start the REPL

```sh
#! /bin/bash

# Get common variables like database port, username, password, etc.
source "$(dirname "$0")/common_vars.sh"

# Use the first argument as the database to connect to. Otherwise use `default`
db=${1:-default}

# Start the REPL
$surreal_db_root/surreal sql \
  --conn http://localhost:$surreal_db_port \
  --user $surreal_db_user \
  --pass "$surreal_db_pass" \
  --ns default \
  --db $db \
  --pretty
```

Note that I'm using a namespace called `default` and a database named `default`. You can change these to fit your use case. The script also supports passing a specific database name as the first argument when executing it.

### Script for Backing Up the Databases

I wanted to make sure I could back up my databases on a schedule (or manually). Another script comes in handy here.

Create a filed called `back_up_databases.sh`

```sh
#! /bin/bash

# Get common variables like database port, username, password, etc.
source "$(dirname "$0")/common_vars.sh"

# Get a list of all databases in the namespace
db_list=$(echo "INFO FOR NS;" | $surreal_db_root/scripts/repl.sh | jq -r '.[0].result.db | keys[]')

# Place all the databases in a list bash can loop over
db_names=($(echo "$db_list"))

# Where to write the backup files
backup_path=$surreal_db_root/backups

# Make sure the backup path exists
mkdir -p $backup_path

# Loop through all listed databases
for db_name in "${db_names[@]}"
do
  # Export a SurrealQL script file for each database
  $surreal_db_root/surreal export \
    --conn http://localhost:$surreal_db_port \
    --user $surreal_db_user \
    --pass "$surreal_db_pass" \
    --ns default \
    --db $db_name \
    $backup_path/backup_${db_name}_$(date --utc +"%Y%m%d%H%M%S").sql
done

# Remove all files that were created at least 14 days ago
find "$backup_path" -type f -ctime +"14" -exec rm {} \;
```

This script will use the REPL script we wrote earlier to get a list of database names in the namespace. It then loops through the list and runs `surreal export` on each, writing the exported script file to a `backups` directory. It then removes all backup files that were created more than 14 days ago. You can change that `14` number to any number that works for you, or even remove the last line completely to never remove old backups.

Note that the line that gets the database names uses `jq`. Make sure you have it installed. I'm using Arch Linux, so here's how to do it with that operating system. If you use a different one, just look up how to install programs in your operating system.

```sh
sudo pacman -S jq
```

Also note that backups will go in the `backups` directory inside the root directory set in `common_vars.sh`. You can change that if you like.

Now that all the scripts are created, we need to make them executable

```sh
chmod u+x start_server.sh
chmod u+x repl.sh
chmod u+x back_up_databases.sh
```

(`common_vars.sh` doesn't need to be executable. It's just being sourced from the other scripts.)

## Running and Using SurrealDB

At this point, you can start SurrealDB

```sh
~/.surrealdb/scripts/start_server.sh
```

And you can interact with it using the REPL

```sh
~/.surrealdb/scripts/repl.sh
```

Try getting some information about the namespace

```plaintext
INFO FOR NS;
```

There probably won't be much there at the moment, unless you've already started using it to create databases, tables, and insert data.

Also, try interacting with it via the built-in HTTP API

```sh
curl --request POST \
  --url http://localhost:24131/sql \
  --header 'Accept: application/json' \
  --header 'DB: default' \
  --header 'NS: default' \
  --user "some_root_username:some_root_password" \
  --data 'INFO FOR NS;'
```

## Start on System Startup

I want SurrealDB to start when my system starts. Making this happen will depend on your operating system. I'm using Arch Linux, which uses `systemd`. So here are the steps for that. You may need to look up how it's done on your operating system if you're using something different.

First, make sure you don't have SurrealDB currently running. Stop any processes that you might have started earlier.

Now, create a new file in `/etc/systemd/system/` called `surrealdb.service`

```sh
sudo vim /etc/systemd/system/surrealdb.service
```

```plaintext
[Unit]
Description=SurrealDB
After=network.target

[Service]
Type=simple
ExecStart=/home/travis/.surrealdb/scripts/start_server.sh
WorkingDirectory=/home/travis/.surrealdb/scripts
User=travis
Group=travis
Restart=always

[Install]
WantedBy=multi-user.target
```

Make sure to change the `ExecStart` path to your actual `start_server.sh` script. Also make sure to change the `User` and `Group` to match your actual account.

Reload the systemd configuration

```sh
sudo systemctl daemon-reload
```

Enable the service we just created so it starts at system startup

```sh
sudo systemctl enable surrealdb
```

Start the service now

```sh
sudo systemctl start surrealdb
```

Check it out to make sure there were no errors

```sh
systemctl status surrealdb
```

I recommend restarting your machine to make sure it's working. Try accessing the server via HTTP from another machine before logging in to your user account.

## Run backups on a schedule

Again, this will depend on your operating system. I'm using Arch Linux, which uses `systemd/Timers`. `cron` is another popular option, but I won't be going over that, here. Here are the steps to set up scheduled backups using `systemd/Timers`.

Create a new file in `/etc/systemd/system/` called `back_up_surrealdb_databases.timer`

```sh
sudo vim /etc/systemd/system/back_up_surrealdb_databases.timer
```

```plaintext
[Unit]
Description=SurrealDB Backup Timer

[Timer]
OnCalendar=hourly
Persistent=true

[Install]
WantedBy=timers.target
```

I set my backups to run every hour. You may want to change this depending on your use case.

The timer is set, but we still need to create the service that the timer should trigger. We must create a matching `.service` file.

```sh
sudo vim /etc/systemd/system/back_up_surrealdb_databases.service
```

```plaintext
[Unit]
Description=SurrealDB Backup Service

[Service]
ExecStart=/home/travis/.surrealdb/scripts/back_up_databases.sh

[Install]
WantedBy=multi-user.target
```

Make sure to change the `ExecStart` path to the actual path to your backup script.

With that timer and service created, we have to reload configuration, enable the timer, and start it.

Reload the systemd configuration

```sh
sudo systemctl daemon-reload
```

Enable the timer

```sh
sudo systemctl enable back_up_surrealdb_databases.timer
```

Start the timer

```sh
sudo systemctl start back_up_surrealdb_databases.timer
```

New backups should automatically appear in the `backups` directory inside the SurrealDB root directory set in `common_vars.sh` every hour. For me, that's at `/home/travis/.surrealdb/backups`. Note that you might not see anything, even after the top of the hour, until you start inserting data into your database.

## Setup Complete!

With the completion of the installation and configuration process, your SurrealDB database is now ready to use. Take advantage of its unique features. Interact with the database via HTTP, REST, WebSockets, or JSON-RPC. With the help of the management scripts created in this guide, you have automated many of the database operations for efficiency. And with the backups scheduled on a timer, your data is secure and retrievable in case of any loss.

Try [browsing some of the documentation](https://surrealdb.com/docs) to see what it's all about. Play around with some of the cool features you might not get with other database systems. You may also enjoy watching [Code to the Moon go over some of the basics in this YouTube video](https://www.youtube.com/watch?v=DPQbuW9dQ7w).