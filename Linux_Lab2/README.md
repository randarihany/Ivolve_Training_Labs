Step 1: Create the Shell Script
Open your terminal and create a new shell script file, e.g., mysql_backup.sh:

```
crontab - l
```


![image](https://github.com/user-attachments/assets/ea082a50-c1f8-41c5-b139-52e934495d62)

Script
```
#!/bin/bash

# Set MySQL user and password
MYSQL_USER="root"  # MySQL username
MYSQL_PASS="randa"  # MySQL password
MYSQL_DB="my_database"  # MySQL database name

# Set backup directory
BACKUP_DIR="/home/randa/mysql_Backups"

# backup directory
DATE=$(date +\%F)  # Current date in YYYY-MM-DD format
BACKUP_FILE="${BACKUP_DIR}/${MYSQL_DB}_backup_${DATE}.sql"

# Create backup directory if it doesn't exist
mkdir -p $BACKUP_DIR

# Run the mysqldump command to backup the database
mysqldump -u$MYSQL_USER -p$MYSQL_PASS $MYSQL_DB > $BACKUP_FILE

# Check if the backup was successful
if [ $? -eq 0 ]; then
  echo "Backup of database $MYSQL_DB completed successfully at $(date)" >> ${BACKUP_DIR}/backup.log
else
  echo "Backup of database $MYSQL_DB failed at $(date)" >> ${BACKUP_DIR}/backup.log
fi

```

![image](https://github.com/user-attachments/assets/789ee139-58ab-459a-b331-a7dbb1852ca7)

# Example of MYSQL:


![image](https://github.com/user-attachments/assets/d5d23c8b-7678-4c39-a909-fc4d59bc6981)

# MYSQL updated to be:

![image](https://github.com/user-attachments/assets/5661d886-9ab3-418d-ae67-3d1da3d9baaf)





bash
Copy
nano /path/to/your/script/mysql_backup.sh
Replace /path/to/your/script/ with the desired path where you want to save the script file.

Add the following content to the script:

#!/bin/bash

# Set MySQL user and password
MYSQL_USER="your_mysql_user"
MYSQL_PASS="your_mysql_password"
MYSQL_DB="your_database_name"

# Set backup directory (ensure the directory exists)
BACKUP_DIR="/path/to/backup/directory"
DATE=$(date +\%F)  # Current date in YYYY-MM-DD format
BACKUP_FILE="${BACKUP_DIR}/${MYSQL_DB}_backup_${DATE}.sql"

# Create backup directory if it doesn't exist
mkdir -p $BACKUP_DIR

# Run the mysqldump command to backup the database
mysqldump -u$MYSQL_USER -p$MYSQL_PASS $MYSQL_DB > $BACKUP_FILE

# Check if the backup was successful
if [ $? -eq 0 ]; then
  echo "Backup of database $MYSQL_DB completed successfully at $(date)" >> ${BACKUP_DIR}/backup.log
else
  echo "Backup of database $MYSQL_DB failed at $(date)" >> ${BACKUP_DIR}/backup.log
fi
Explanation of the script:

MYSQL_USER: Replace with your MySQL username.
MYSQL_PASS: Replace with your MySQL password.
MYSQL_DB: Replace with the name of the database you want to back up.
BACKUP_DIR: The directory where the backup will be stored.
DATE: The current date formatted as YYYY-MM-DD.
The mysqldump command is used to create the backup.
The script also checks whether the backup was successful and logs it to a backup.log file.
Save and exit the script (in nano, press CTRL + X, then Y to confirm, and Enter).

Make the script executable:

bash
Copy
chmod +x /path/to/your/script/mysql_backup.sh
Step 2: Set Up a Cron Job to Run the Script Daily at 5:00 PM
Open the cron job configuration for the user (or root, depending on your permissions):

bash
Copy
crontab -e
Add a new cron job to execute the backup script at 5:00 PM every day:

bash
Copy
0 17 * * * /path/to/your/script/mysql_backup.sh
Explanation of the cron syntax:

0 17 * * *: This means "run at 17:00 (5:00 PM) every day."
0: Minute (0th minute).
17: Hour (17:00 or 5:00 PM).
* * *: Every day, every month, every weekday.
/path/to/your/script/mysql_backup.sh: This is the path to the backup script you created earlier.
Save and exit the cron job editor (in nano, press CTRL + X, then Y to confirm, and Enter).

Step 3: Verify the Cron Job
To verify that your cron job is set correctly:

List the cron jobs to ensure it's scheduled:

bash
Copy
crontab -l
You should see the line you added:

bash
Copy
0 17 * * * /path/to/your/script/mysql_backup.sh
If everything is correct, your backup script will automatically run every day at 5:00 PM.

Step 4: Test the Script (Optional)
You can manually run the script to check if it works as expected:

bash
Copy
/path/to/your/script/mysql_backup.sh
Check the backup directory to confirm that the .sql backup file was created.

Step 5: Check the Logs
The script will log each backup attempt in the backup.log file in the backup directory. You can check this log for any errors or confirmation messages:

bash
Copy
cat /path/to/backup/directory/backup.log
This log will include messages like:

![image](https://github.com/user-attachments/assets/c659f51b-bb9a-4e07-a1e2-1b62dedbf9fa)

## File created:
![image](https://github.com/user-attachments/assets/cb90d34e-d1b2-4a49-9cf2-1061282fbab0)

## part of output that shows current values in db:
![image](https://github.com/user-attachments/assets/fc26ba65-fc44-4494-9b5f-8490a97c07db)



Backup of database your_database_name completed successfully at ...
Backup of database your_database_name failed at ... (in case of any issues).
Conclusion
You now have a fully automated MySQL backup system that runs daily at 5:00 PM. The shell script will create a backup of your MySQL database, and the cron job will ensure it's executed on schedule. You can modify the script and cron schedule based on your needs (e.g., adding additional backups or changing the time).

Let me know if you need further clarification or adjustments!
