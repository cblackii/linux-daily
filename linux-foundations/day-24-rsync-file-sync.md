# Day 24: File Sync with rsync

## Commands practiced

- `rsync`
- `rsync -av`
- `rsync -avn`
- `rsync --delete`
- `mkdir`
- `touch`
- `echo`
- `diff`
- `find`
- `ls -lh`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-24-lab
cd day-24-lab
```

Create source and destination folders:

```bash
mkdir source backup
```

Create files in the source folder:

```bash
echo "Day 24 rsync practice" > source/README.txt
echo "server=linux-lab" > source/server.conf
echo "status=active" >> source/server.conf
mkdir source/logs
echo "app started" > source/logs/app.log
find source -type f
```

Run a dry-run first:

```bash
rsync -avn source/ backup/
```

Sync the files:

```bash
rsync -av source/ backup/
find backup -type f
cat backup/README.txt
```

Verify copied files match:

```bash
diff source/README.txt backup/README.txt
diff source/server.conf backup/server.conf
```

Change one source file:

```bash
echo "last_checked=today" >> source/server.conf
rsync -avn source/ backup/
rsync -av source/ backup/
cat backup/server.conf
```

Create a file only in the backup folder:

```bash
echo "old backup file" > backup/old.txt
find backup -type f
```

Preview delete behavior:

```bash
rsync -avn --delete source/ backup/
```

Sync again and delete files that no longer exist in source:

```bash
rsync -av --delete source/ backup/
find backup -type f
```

Compare source and backup file lists:

```bash
find source -type f | sort
find backup -type f | sort
```

Save a sync report:

```bash
{
  echo "Day 24 rsync report"
  date
  echo
  echo "Source files:"
  find source -type f | sort
  echo
  echo "Backup files:"
  find backup -type f | sort
} > sync-report.txt

cat sync-report.txt
```

Clean up:

```bash
cd ..
rm -r day-24-lab
```

## Notes

Today I practiced syncing files with `rsync`. A dry-run with `-n` previews what would happen before changing files. `-a` uses archive mode to preserve file metadata, and `-v` shows verbose output. A trailing slash on `source/` means copy the contents of the directory into the destination. `--delete` removes destination files that no longer exist in the source.
