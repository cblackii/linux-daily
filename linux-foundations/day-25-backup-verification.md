# Day 25: Backup Verification

## Commands practiced

- `mkdir`
- `echo`
- `find`
- `tar -czf`
- `tar -tzf`
- `tar -xzf`
- `sha256sum`
- `diff`
- `du -sh`
- `ls -lh`
- `tee`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-25-lab
cd day-25-lab
```

Create a small directory that needs a backup:

```bash
mkdir -p app-data/config app-data/logs app-data/uploads
echo "app_name=linux-lab" > app-data/config/app.conf
echo "environment=training" >> app-data/config/app.conf
echo "application started" > app-data/logs/app.log
echo "sample upload content" > app-data/uploads/file-01.txt
find app-data -type f | sort
```

Check the size of the data:

```bash
du -sh app-data
ls -lh app-data/config app-data/logs app-data/uploads
```

Create a compressed backup archive:

```bash
tar -czf app-data-backup.tar.gz app-data
ls -lh app-data-backup.tar.gz
```

Inspect the archive before restoring:

```bash
tar -tzf app-data-backup.tar.gz
```

Create checksums for the original files:

```bash
find app-data -type f -exec sha256sum {} \; | sort > original-checksums.txt
cat original-checksums.txt
```

Restore the backup into a separate folder:

```bash
mkdir restore-test
tar -xzf app-data-backup.tar.gz -C restore-test
find restore-test -type f | sort
```

Compare original files to restored files:

```bash
diff app-data/config/app.conf restore-test/app-data/config/app.conf
diff app-data/logs/app.log restore-test/app-data/logs/app.log
diff app-data/uploads/file-01.txt restore-test/app-data/uploads/file-01.txt
```

Create checksums for restored files:

```bash
cd restore-test
find app-data -type f -exec sha256sum {} \; | sort > ../restored-checksums.txt
cd ..
cat restored-checksums.txt
```

Compare checksum reports:

```bash
diff original-checksums.txt restored-checksums.txt
```

Create a backup verification report:

```bash
{
  echo "Day 25 backup verification report"
  date
  echo
  echo "Original data size:"
  du -sh app-data
  echo
  echo "Backup archive:"
  ls -lh app-data-backup.tar.gz
  echo
  echo "Archive contents:"
  tar -tzf app-data-backup.tar.gz
  echo
  echo "Checksum comparison:"
  diff original-checksums.txt restored-checksums.txt
  echo "If no diff output appeared above, the restored files match the originals."
} | tee backup-verification-report.txt
```

Review the report:

```bash
cat backup-verification-report.txt
```

Clean up:

```bash
cd ..
rm -r day-25-lab
```

## Notes

Today I practiced creating and verifying a backup. A backup is not proven until it has been restored and checked. `tar -czf` creates a compressed archive, `tar -tzf` lists archive contents, `tar -xzf` extracts it, `sha256sum` creates file fingerprints, and `diff` confirms whether original and restored files match.
