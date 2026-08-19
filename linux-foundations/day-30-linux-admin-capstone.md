# Day 30: Linux Admin Capstone

## Commands practiced

- `hostname`
- `uptime`
- `df -h`
- `free -h`
- `systemctl`
- `journalctl`
- `ss -tuln`
- `tar -czf`
- `sha256sum`
- `find`
- `tee`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-30-lab
cd day-30-lab
```

Create a small application directory:

```bash
mkdir -p app/config app/logs app/data backups reports
echo "app_name=day30-capstone" > app/config/app.conf
echo "environment=training" >> app/config/app.conf
echo "2026-08-18 INFO app started" > app/logs/app.log
echo "sample data" > app/data/data.txt
find app -type f | sort
```

Run basic system checks:

```bash
hostname
uptime
df -h /
free -h
```

Check service health:

```bash
systemctl is-active ssh
systemctl status ssh
```

Check listening network ports:

```bash
ss -tuln
```

Read recent logs:

```bash
journalctl -n 20 --no-pager
```

Create a backup:

```bash
tar -czf backups/app-backup.tar.gz app
ls -lh backups
tar -tzf backups/app-backup.tar.gz
```

Create checksums:

```bash
find app -type f -exec sha256sum {} \; | sort > reports/original-checksums.txt
sha256sum backups/app-backup.tar.gz > reports/backup-checksum.txt
cat reports/original-checksums.txt
cat reports/backup-checksum.txt
```

Restore the backup into a test folder:

```bash
mkdir restore-test
tar -xzf backups/app-backup.tar.gz -C restore-test
find restore-test -type f | sort
```

Verify restored files:

```bash
cd restore-test
find app -type f -exec sha256sum {} \; | sort > ../reports/restored-checksums.txt
cd ..
diff reports/original-checksums.txt reports/restored-checksums.txt
```

No output from `diff` means the restored files match the originals.

Create a final admin report:

```bash
{
  echo "Day 30 Linux admin capstone report"
  date
  echo
  echo "System identity:"
  hostname
  echo
  echo "Uptime:"
  uptime
  echo
  echo "Disk usage:"
  df -h /
  echo
  echo "Memory usage:"
  free -h
  echo
  echo "SSH status:"
  systemctl is-active ssh
  echo
  echo "Listening ports:"
  ss -tuln
  echo
  echo "Application files:"
  find app -type f | sort
  echo
  echo "Backup archive:"
  ls -lh backups/app-backup.tar.gz
  echo
  echo "Backup checksum:"
  cat reports/backup-checksum.txt
  echo
  echo "Restore verification:"
  diff reports/original-checksums.txt reports/restored-checksums.txt
  echo "If no diff output appeared above, restore verification passed."
} | tee reports/day-30-admin-report.txt
```

Review the report:

```bash
cat reports/day-30-admin-report.txt
```

Clean up:

```bash
cd ..
rm -r day-30-lab
```

## Notes

Today I practiced a small Linux administration workflow from end to end. I checked system health, inspected service status, reviewed network ports, read logs, created a backup, restored the backup, verified file checksums, and saved the results in an admin report. This combines several foundational skills into one repeatable operations checklist.
