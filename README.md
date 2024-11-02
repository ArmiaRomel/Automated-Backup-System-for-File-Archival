# Automated Backup Script

This script automates the process of archiving and compressing updated files from a target directory into a tar.gz format, then stores the backup in a specified destination directory. It checks each file in the target folder to see if it has been updated within the last 24 hours, and includes only those files in the backup.

## How It Works

The backup process is performed entirely in a shell script. By specifying a target and destination directory, the script checks for files updated within the past 24 hours, compresses them, and stores the backup in a tar.gz file at the destination.

### Key Features
- **Automated Daily Backup**: Archives only files updated in the last 24 hours, ensuring recent changes are backed up without redundancy.
- **Compression**: Saves space by creating a compressed tar.gz file of the backup files.
- **Flexible Directories**: Simply specify the target and destination folders as arguments, allowing for flexible use across different directories.

### Usage
To run the script, use the following command:

```bash
./backup.sh [target_directory_name] [destination_directory_name]
```

- `target_directory_name`: The directory containing files to be backed up.
- `destination_directory_name`: The directory where the compressed backup file will be stored.

  
