# Day 20: Disk and Filesystem Investigation

## Commands practiced

- `lsblk`
- `findmnt`
- `df -h`
- `df -i`
- `du -sh`
- `du -ah`
- `stat`
- `file`
- `find`
- `sort -h`
- `tee`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-20-lab
cd day-20-lab
```

Inspect disks and block devices:

```bash
lsblk
lsblk -f
```

Inspect mounted filesystems:

```bash
findmnt
findmnt /
```

Check filesystem capacity and available space:

```bash
df -h
df -h /
```

Check inode usage:

```bash
df -i
df -i /
```

Create practice directories and files of different sizes:

```bash
mkdir -p data/logs data/backups data/uploads
printf 'application started\nwarning: disk check requested\n' > data/logs/app.log
dd if=/dev/zero of=data/backups/backup.img bs=1M count=5 status=progress
dd if=/dev/zero of=data/uploads/upload.bin bs=1M count=2 status=progress
touch data/uploads/empty-file.txt
```

Inspect the directory structure and file types:

```bash
find data -maxdepth 2 -type f
file data/logs/app.log
file data/backups/backup.img
file data/uploads/empty-file.txt
```

Inspect detailed file metadata:

```bash
stat data/logs/app.log
stat data/backups/backup.img
```

Measure directory usage:

```bash
du -sh data
du -sh data/*
```

List files and directories by size:

```bash
du -ah data | sort -h
```

Find files larger than 1 MB:

```bash
find data -type f -size +1M -print
```

Find empty files:

```bash
find data -type f -empty -print
```

Find files modified during the last day:

```bash
find data -type f -mtime -1 -print
```

Create a disk investigation report:

```bash
{
  echo 'Day 20 disk and filesystem report'
  date
  echo
  echo 'Root filesystem:'
  findmnt /
  echo
  echo 'Capacity:'
  df -h /
  echo
  echo 'Inode usage:'
  df -i /
  echo
  echo 'Lab directory usage:'
  du -sh data
  echo
  echo 'Largest lab entries:'
  du -ah data | sort -h | tail -n 5
  echo
  echo 'Files larger than 1 MB:'
  find data -type f -size +1M -print
} | tee disk-report.txt
```

Review the report:

```bash
cat disk-report.txt
```

Clean up after the lab and documentation are confirmed:

```bash
cd ..
rm -r day-20-lab
```

## Notes

Today I practiced investigating Linux disks and filesystems. I inspected block devices and mount points, checked capacity and inode usage, measured directory sizes, identified large and empty files, reviewed file metadata, and created a disk investigation report.
