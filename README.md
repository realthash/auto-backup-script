# Auto Backup Script

A lightweight, automated Bash shell script designed to perform incremental backups by archiving files modified within the last 24 hours into a timestamped compressed `tar.gz` archive.

---

## Features

- **Automated Timestamping**: Generates unique archive filenames using UNIX epoch timestamps (e.g., `backup-[1791374027].tar.gz`).
- **Incremental Filtering**: Automatically inspects file modification dates and selectively backs up only files updated within the previous 24 hours.
- **Compression**: Creates compressed `.tar.gz` archives to save disk space.
- **Input Validation**: Ensures valid arguments and directory paths prior to executing operations.

---

## Prerequisites

- Bash shell environment (Linux, macOS, or WSL / Git Bash on Windows).
- Standard UNIX utilities: `tar`, `date`, `bash`.

---

## Installation & Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/realthash/auto-backup-script.git
   cd auto-backup-script
   ```

2. **Grant execution permissions**:
   ```bash
   chmod +x backup.sh
   ```

---

## Usage

Run the script by passing two arguments:
1. `target_directory_name`: The directory containing the files you want to inspect and back up.
2. `destination_directory_name`: The directory where the resulting backup archive should be placed.

### Syntax

```bash
./backup.sh <target_directory> <destination_directory>
```

### Example

To back up files from `./important-documents` to the current working directory (`.`):

```bash
./backup.sh important-documents .
```

### Script Execution Flow

1. Validates that exactly two directory arguments are passed and that both paths exist.
2. Calculates the current timestamp and a 24-hour lookback threshold timestamp.
3. Iterates over all files in the target directory and filters for those modified within the last 24 hours (`modification_time > current_time - 24 hours`).
4. Compresses matching files into `backup-[<timestamp>].tar.gz`.
5. Moves the archive safely into the destination directory.

---

## Automation via Cron Job

To run this backup automatically on a schedule (e.g., every day at midnight), add an entry to your `crontab`:

1. Open your crontab editor:
   ```bash
   crontab -e
   ```

2. Add a schedule entry (replace paths with your actual absolute paths):
   ```cron
   0 0 * * * /bin/bash /path/to/auto-backup-script/backup.sh /path/to/important-documents /path/to/backups >> /path/to/backup.log 2>&1
   ```

---
