# Day 17: Log Investigation

## Commands practiced

- `cat`
- `head`
- `tail`
- `grep`
- `wc -l`
- `cut`
- `awk`
- `sort`
- `uniq -c`
- `journalctl`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-17-lab
cd day-17-lab
```

Create a small practice log:

```bash
echo '2026-08-05 09:00:01 INFO user=ubuntu action=login status=success ip=10.0.0.5' > app.log
echo '2026-08-05 09:03:12 WARN user=ubuntu action=sudo status=success ip=10.0.0.5' >> app.log
echo '2026-08-05 09:05:44 ERROR user=guest action=login status=failed ip=10.0.0.9' >> app.log
echo '2026-08-05 09:08:19 INFO user=ubuntu action=apt status=success ip=10.0.0.5' >> app.log
echo '2026-08-05 09:10:33 ERROR user=guest action=ssh status=failed ip=10.0.0.9' >> app.log
echo '2026-08-05 09:12:50 INFO user=admin action=backup status=success ip=10.0.0.7' >> app.log
```

Read the log:

```bash
cat app.log
head app.log
tail app.log
```

Search for log levels:

```bash
grep "ERROR" app.log
grep "WARN" app.log
grep "INFO" app.log
```

Count matching lines:

```bash
grep "ERROR" app.log | wc -l
grep "failed" app.log | wc -l
grep "success" app.log | wc -l
```

Extract fields with `awk`:

```bash
awk '{print $3}' app.log
awk '{print $4}' app.log
awk '{print $6}' app.log
```

Extract values with `cut`:

```bash
cut -d ' ' -f 3 app.log
cut -d ' ' -f 4 app.log
```

Count repeated users:

```bash
awk '{print $4}' app.log | sort | uniq -c
```

Count repeated IP addresses:

```bash
awk '{print $7}' app.log | sort | uniq -c
```

Save an error report:

```bash
grep "ERROR" app.log > error-report.txt
cat error-report.txt
```

Look at recent system logs:

```bash
journalctl -n 10
```

Clean up:

```bash
cd ..
rm -r day-17-lab
```

## Notes

Today I practiced reading and investigating log files. I searched for specific log levels, counted matching lines, extracted fields with `awk` and `cut`, counted repeated values with `sort | uniq -c`, and saved filtered log output into a report file.
