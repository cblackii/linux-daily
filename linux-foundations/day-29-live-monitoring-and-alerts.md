# Day 29: Live Monitoring and Simple Alerts

## Commands practiced

- `watch`
- `uptime`
- `df -h`
- `free -h`
- `ps`
- `pgrep`
- `systemctl is-active`
- `awk`
- `tee`
- `date`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-29-lab
cd day-29-lab
```

Run manual checks first:

```bash
uptime
df -h /
free -h
systemctl is-active ssh
ps aux | head
```

Use `watch` to repeat a command every two seconds:

```bash
watch uptime
```

Press `Ctrl+C` to stop watching.

Watch disk usage:

```bash
watch -n 2 'df -h /'
```

Press `Ctrl+C` to stop.

Watch memory usage:

```bash
watch -n 2 'free -h'
```

Press `Ctrl+C` to stop.

Create a monitoring script:

```bash
nano monitor-check.sh
```

Add this content:

```bash
#!/bin/bash

REPORT="monitor-report.txt"
DISK_USED=$(df / | awk 'NR==2 {print $5}' | tr -d '%')
SSH_STATUS=$(systemctl is-active ssh)

{
  echo "Day 29 monitoring report"
  echo "Generated: $(date)"
  echo
  echo "Uptime:"
  uptime
  echo
  echo "Root filesystem:"
  df -h /
  echo
  echo "Memory:"
  free -h
  echo
  echo "SSH status: $SSH_STATUS"
  echo
  echo "Disk used percent: $DISK_USED%"
  echo

  if [ "$SSH_STATUS" = "active" ]; then
    echo "PASS: ssh is active"
  else
    echo "FAIL: ssh is not active"
  fi

  if [ "$DISK_USED" -lt 80 ]; then
    echo "PASS: disk usage is below 80%"
  else
    echo "WARN: disk usage is 80% or higher"
  fi
} | tee "$REPORT"
```

Make the script executable:

```bash
chmod +x monitor-check.sh
```

Run the script:

```bash
./monitor-check.sh
cat monitor-report.txt
```

Run the script repeatedly with `watch`:

```bash
watch -n 5 ./monitor-check.sh
```

Press `Ctrl+C` after a few runs.

Create a temporary background process:

```bash
sleep 300 &
pgrep -af "sleep 300"
```

Check for that process:

```bash
pgrep -af "sleep 300" > process-check.txt
cat process-check.txt
```

Stop the process:

```bash
pkill -f "sleep 300"
pgrep -af "sleep 300"
```

Clean up:

```bash
cd ..
rm -r day-29-lab
```

## Notes

Today I practiced live monitoring and simple alert logic. `watch` reruns a command on a timer, which is useful for observing changing system state. The monitoring script collected uptime, disk usage, memory usage, and SSH service status. It then used basic `if` statements to print pass/warn/fail style results.
