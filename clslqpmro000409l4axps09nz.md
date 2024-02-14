---
title: "Creating a Development Database: Syncing Production Data for Efficient Development"
datePublished: Wed Feb 14 2024 12:00:07 GMT+0000 (Coordinated Universal Time)
cuid: clslqpmro000409l4axps09nz
slug: creating-a-development-database-syncing-production-data-for-efficient-development
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689351175558/56fd06bd-0481-4428-9cf1-c034f580f2e3.png
tags: workflow, testing, data-protection, databasemanagement, database-development

---

As you develop software, having a development database that mirrors your production data is a game-changer. It allows you to work with realistic data without the risk of modifying or corrupting the actual production database. A development database serves as a safe sandbox for testing, debugging, and making improvements to your application. In this article, I will provide a step-by-step guide on how to create a script that synchronizes your production data with the development environment.

This guide is for MySQL/MariaDB on Linux servers.

## **Create a Sync Script**

Instead of creating a development database by typing in commands manually, it is better to create a script. This way, you can sync the development database with the production one at any time with one command.

Create a sync script at `/usr/local/bin/sync_dev_db.sh`.

```bash
#!/bin/bash

# Duplicates production database to a development database.
# WARNING: All data on the development database will be lost!

# Enable errexit so the script stops as soon as it encounters an error
set -e

PROD_DB="[production database name]"
DEV_DB="${PROD_DB}_dev"
DUMP_FILE="/tmp/${PROD_DB}_$(date -u +"%Y-%m-%dT%H:%M:%SZ").sql"

# Check if the development database already exists
echo "Checking if development database already exists..."

if mysql -e "USE $DEV_DB";
then
  # Development database already exists, ask to continue before dropping it
  echo "Development database '$DEV_DB' already exists."
  read -p "Remove all its data and replace it with a copy of the production database '$PROD_DB'? (y/N) " answer

  if [[ $answer = y ]] ; then
    # The answer is yes. Drop the database
    echo "Dropping existing development database..."
    mysql -e "DROP DATABASE $DEV_DB;"
  else
    # The answer is no. Abort the script
    echo "Aborted."
    exit 0
  fi
else
  # Development database does not exists. No action needed
  echo "No existing development database found."
fi

# Create the development database
echo "Creating development database..."
mysql -e "CREATE DATABASE $DEV_DB;"

# Dump the production database schema and data to file
echo "Dumping production database to temporary file..."
mysqldump $PROD_DB > $DUMP_FILE

# Import the dump file into the development database
echo "Importing temporary file into development database..."
mysql $DEV_DB < $DUMP_FILE

# Delete the dump file as it's no longer needed
echo "Removing temporary file..."
rm $DUMP_FILE

echo "Done."
```

Make sure you replace `[production database name]` in the script above with the actual name of your production database.

What this script essentially does is:

1. Checks if a development database already exists (by looking for `[production database name]_dev`). If so, it asks and then drops the existing development database
    
2. Creates the development database with the same name as the production database with `_dev` appended to it
    
3. Dumps the production data to a temporary file
    
4. Imports the data from the temporary file into the development database
    
5. Deletes the temporary file
    

Make the script executable.

```bash
sudo chmod +x /usr/local/bin/sync_dev_db.sh
```

## **Create/Sync the Development Database**

> The method in this section will erase all existing data in the development database if it exists.

Run the sync script.

```bash
sudo sync_dev_db.sh
```

A database that is an exact duplicate of your production database now exists in the system. The name has `_dev` appended to it. For example, if your production database is called `mydatabase`, there is now a database called `mydatabase_dev`.

Run this script any time you want to sync the data from production to development.

By implementing a development database that mirrors your production data, you can significantly enhance the efficiency and reliability of your development workflow. The ability to work with realistic data, test new features, and troubleshoot issues in a controlled environment is invaluable. With the script we've discussed, you now have a solid foundation for automating the synchronization process, ensuring that your development database stays up to date. Remember to exercise caution when working with sensitive data and always follow best practices to maintain the integrity and security of your systems.

Cover photo by [Woliul Hasan](https://unsplash.com/ja/@shotbywoliul?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/s8kedguRDCI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText).