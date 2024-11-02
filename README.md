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

### Example
```bash
./backup.sh /home/user/documents /home/user/backup
```

### Code Explanation

This line specifies that the script should be run in the Bash shell.
```bash
#!/bin/bash
```

This checks if exactly two arguments (target and destination directories) are provided. If not, it prints a usage message and exits.
```bash
if [[ $# != 2 ]]; then
  echo "Usage: backup.sh target_directory_name destination_directory_name"
  exit 1
fi
```

This verifies that both arguments are valid directories. If either one is invalid, it displays an error and exits.
```bash
if [[ ! -d $1 ]] || [[ ! -d $2 ]]; then
  echo "Error: Invalid directory path provided."
  exit 1
fi
```

Assigns the first argument to `targetDirectory` and the second argument to `destinationDirectory`.
```bash
targetDirectory=$1
destinationDirectory=$2
```

Gets the current timestamp in seconds since the epoch (Unix time) and stores it in `currentTS`.
```bash
currentTS=$(date +%s)
```

Creates a unique backup filename with the current timestamp to ensure no overwrites of previous backups.
```bash
backupFileName="backup-$currentTS.tar.gz"
```

Stores the original directory path, allowing the script to return to this location later if needed.
```bash
origAbsPath=$(pwd)
```

Navigates to the destination directory and saves its absolute path in `destDirAbsPath`.
```bash
cd $destinationDirectory
destDirAbsPath=$(pwd)
```

Returns to the original directory and then navigates into the target directory where files are located.
```bash
cd $origAbsPath
cd $targetDirectory
```

Calculates the timestamp for 24 hours ago by subtracting 24 hours (in seconds) from the current timestamp.
```bash
yesterdayTS=$(($currentTS - 24 * 60 * 60))
```

Creates an empty array `toBackup` to hold the list of files that have been modified in the last 24 hours.
```bash
declare -a toBackup
```

Iterates over each file in the target directory. For each file, it checks if the modification timestamp is within the last 24 hours. If it is, the file is added to the `toBackup` array.
```bash
for file in $(ls); do
  if [[ $(date -r $file +%s) -ge $yesterdayTS ]]; then
    toBackup+=($file)
  fi
done
```

Compresses all files in the `toBackup` array into a tar.gz archive named `backupFileName`.
```bash
tar -czvf $backupFileName ${toBackup[@]}
```

Moves the created backup file to the destination directory.
```bash
mv $backupFileName $destDirAbsPath
```

## Terminal Output

Here is the terminal output after executing the backup script:
![Terminal output](https://github.com/user-attachments/assets/17c8911b-9677-4ed4-9bc7-13670684f246)














