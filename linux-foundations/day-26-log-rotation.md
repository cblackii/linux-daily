# Day 26: Log Rotation

## Commands practiced

- `logrotate`
- `logrotate -d`
- `logrotate -f`
- `cat`
- `ls -lh`
- `du -sh`
- `mkdir`
- `echo`
- `gzip`
- `zcat`
- `find`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-26-lab
cd day-26-lab
```

Check whether `logrotate` is installed:

```bash
which logrotate
logrotate --version
```

Create a practice log directory and log file:

```bash
mkdir logs
echo "2026-08-14 INFO application started" > logs/app.log
echo "2026-08-14 WARN disk usage check requested" >> logs/app.log
echo "2026-08-14 ERROR sample error event" >> logs/app.log
ls -lh logs
cat logs/app.log
```

Check the size of the log directory:

```bash
du -sh logs
```

Create a local logrotate config:

```bash
cat > app-logrotate.conf <<'EOF'
/home/ubuntu/day-26-lab/logs/app.log {
    rotate 3
    size 1
    compress
    missingok
    notifempty
    copytruncate
}
EOF
```

Review the config:

```bash
cat app-logrotate.conf
```

Run logrotate in debug mode first:

```bash
logrotate -d app-logrotate.conf
```

Debug mode shows what logrotate would do, but it does not rotate files.

Force a rotation for practice:

```bash
logrotate -f app-logrotate.conf
```

Inspect the rotated files:

```bash
ls -lh logs
find logs -type f -print
```

View the current log:

```bash
cat logs/app.log
```

View the compressed rotated log:

```bash
zcat logs/app.log.1.gz
```

Add new log entries:

```bash
echo "2026-08-14 INFO application restarted" >> logs/app.log
echo "2026-08-14 INFO health check passed" >> logs/app.log
cat logs/app.log
```

Force another rotation:

```bash
logrotate -f app-logrotate.conf
ls -lh logs
```

Create a log rotation report:

```bash
{
  echo "Day 26 log rotation report"
  date
  echo
  echo "Logrotate config:"
  cat app-logrotate.conf
  echo
  echo "Rotated log files:"
  ls -lh logs
  echo
  echo "Log directory size:"
  du -sh logs
} > log-rotation-report.txt
```

Review the report:

```bash
cat log-rotation-report.txt
```

Clean up:

```bash
cd ..
rm -r day-26-lab
```

## Notes

Today I practiced rotating log files with `logrotate`. Log rotation prevents log files from growing forever. `rotate 3` keeps three old copies, `size 1` rotates once the file is at least one byte, `compress` compresses rotated logs, and `copytruncate` copies the current log to a rotated file while truncating the original file in place. `logrotate -d` is a safe debug mode that previews actions before making changes.
