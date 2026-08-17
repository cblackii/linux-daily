# Day 28: Health Check Script

## Commands practiced

- `nano`
- `chmod +x`
- `./script-name`
- `date`
- `hostname`
- `uptime`
- `df -h`
- `free -h`
- `systemctl is-active`
- `journalctl -n`
- `tee`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-28-lab
cd day-28-lab
```

Check the commands manually first:

```bash
hostname
uptime
df -h /
free -h
systemctl is-active ssh
journalctl -n 5
```

Create a health check script:

```bash
nano health-check.sh
```

Add this content:

```bash
#!/bin/bash

REPORT="health-report.txt"

{
  echo "Day 28 Linux health check"
  echo "Generated: $(date)"
  echo
  echo "Hostname:"
  hostname
  echo
  echo "Uptime:"
  uptime
  echo
  echo "Root filesystem usage:"
  df -h /
  echo
  echo "Memory usage:"
  free -h
  echo
  echo "SSH service status:"
  systemctl is-active ssh
  echo
  echo "Recent system logs:"
  journalctl -n 5 --no-pager
} | tee "$REPORT"
```

Make the script executable:

```bash
chmod +x health-check.sh
ls -lh health-check.sh
```

Run the script:

```bash
./health-check.sh
```

Review the report:

```bash
cat health-report.txt
```

Add a simple pass/fail check for SSH:

```bash
nano health-check.sh
```

Add this block before the final closing brace:

```bash
  echo
  echo "SSH health result:"
  if systemctl is-active --quiet ssh; then
    echo "PASS: ssh is running"
  else
    echo "FAIL: ssh is not running"
  fi
```

Run the updated script:

```bash
./health-check.sh
cat health-report.txt
```

Create a timestamped copy of the report:

```bash
cp health-report.txt "health-report-$(date +%Y%m%d-%H%M%S).txt"
ls -lh
```

Clean up:

```bash
cd ..
rm -r day-28-lab
```

## Notes

Today I practiced turning manual server checks into a reusable Bash health check script. The script captured hostname, uptime, disk usage, memory usage, SSH service status, and recent logs. `tee` displayed output on screen while saving it to a report file. The `if systemctl is-active --quiet ssh` check added a basic pass/fail result that can be reused in future monitoring scripts.
