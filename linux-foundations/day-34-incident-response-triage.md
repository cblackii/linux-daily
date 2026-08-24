# Day 34: Incident Response Triage

## Goal

Practice the first-pass Linux checks an admin runs when a service or system appears unhealthy.

Incident response triage is not about fixing everything immediately. It is about quickly answering:

- What machine am I on?
- What time did the issue happen?
- Is the service running?
- Are logs showing errors?
- Are system resources healthy?
- Are expected network ports listening?
- What evidence should I capture before making changes?

## Commands practiced

```bash
date
hostname
uptime
systemctl
journalctl
ss
ps
pgrep
df
free
tail
grep
wc
tee
```

## Lab setup

Create a workspace for today's practice:

```bash
mkdir day-34-lab
cd day-34-lab
mkdir -p app/logs reports
```

Create a simulated application log:

```bash
cat > app/logs/app.log <<'EOF'
2026-08-23 08:00:01 INFO app started
2026-08-23 08:01:12 INFO connected to database
2026-08-23 08:05:44 WARN slow response from database
2026-08-23 08:07:03 ERROR failed login attempt for user admin
2026-08-23 08:09:20 INFO health check passed
2026-08-23 08:12:31 ERROR failed to write cache file
2026-08-23 08:14:00 WARN disk usage nearing threshold
2026-08-23 08:15:10 INFO request completed
EOF
```

## Exercise 1: Identify the system and time

Run:

```bash
date
hostname
uptime
```

What this does:

- `date` confirms the current system time.
- `hostname` confirms which machine you are investigating.
- `uptime` shows how long the system has been running and gives a quick load average.

This matters because incident notes should always include the time and host. Without that, troubleshooting gets messy fast.

## Exercise 2: Check service state

Check whether SSH is active:

```bash
systemctl is-active ssh
systemctl --no-pager status ssh
```

What this does:

- `systemctl is-active ssh` gives a short service state.
- `systemctl status ssh` shows whether the service is running, failed, stopped, or recently restarted.
- `--no-pager` keeps the output directly in the terminal instead of opening a scrollable viewer.

If a service is down, this is often one of the fastest ways to confirm it.

## Exercise 3: Check recent service logs

Run:

```bash
journalctl -u ssh -n 20 --no-pager
```

What this does:

- `journalctl` reads system logs managed by systemd.
- `-u ssh` filters logs for the SSH service.
- `-n 20` shows the most recent 20 log entries.
- `--no-pager` prints the output directly.

Logs help explain why a service is failing, restarting, rejecting connections, or behaving unexpectedly.

## Exercise 4: Check listening ports

Run:

```bash
ss -tuln
```

What this does:

- `ss` shows socket and network connection information.
- `-t` shows TCP sockets.
- `-u` shows UDP sockets.
- `-l` shows listening sockets.
- `-n` shows numeric addresses and ports instead of trying to resolve names.

This helps confirm whether a service is actually listening for network connections.

For SSH, look for port `22`.

## Exercise 5: Check running processes

Run:

```bash
pgrep -af ssh
ps aux | grep ssh
```

What this does:

- `pgrep -af ssh` searches running processes by name and shows full command details.
- `ps aux` lists running processes.
- `grep ssh` filters that process list for SSH-related entries.

This helps confirm whether the process exists even before you inspect deeper logs.

## Exercise 6: Check system resources

Run:

```bash
df -h
free -h
```

What this does:

- `df -h` shows filesystem disk usage in human-readable format.
- `free -h` shows memory usage in human-readable format.

Many incidents are caused by simple resource problems: full disk, low memory, or overloaded systems.

## Exercise 7: Inspect application logs

View the end of the simulated log:

```bash
tail -n 20 app/logs/app.log
```

Search for errors:

```bash
grep "ERROR" app/logs/app.log
```

Search for warnings:

```bash
grep "WARN" app/logs/app.log
```

Count errors:

```bash
grep "ERROR" app/logs/app.log | wc -l
```

What this does:

- `tail` shows recent log activity.
- `grep` filters for matching text.
- `wc -l` counts matching lines.
- The pipe sends matching log lines into the counter.

This is a common admin pattern: inspect recent logs, filter for symptoms, then count how often they appear.

## Exercise 8: Create a triage report

Capture your findings into a report:

```bash
{
  echo "Day 34 Incident Triage Report"
  echo "============================="
  echo
  echo "Time:"
  date
  echo
  echo "Hostname:"
  hostname
  echo
  echo "Uptime:"
  uptime
  echo
  echo "Disk:"
  df -h /
  echo
  echo "Memory:"
  free -h
  echo
  echo "SSH service:"
  systemctl is-active ssh
  echo
  echo "Listening ports:"
  ss -tuln
  echo
  echo "Application errors:"
  grep "ERROR" app/logs/app.log
} | tee reports/triage-report.txt
```

What this does:

- The command group collects multiple checks.
- The pipe sends all output into `tee`.
- `tee` displays the report on screen and saves it to `reports/triage-report.txt`.

This is useful because good troubleshooting leaves a record of what you saw before making changes.

## Verify your work

Run:

```bash
ls reports
cat reports/triage-report.txt
```

You should see:

```bash
triage-report.txt
```

The report should include system information, service status, resource usage, listening ports, and application errors.

## Main takeaway

Incident triage is a repeatable checklist:

1. Confirm the host and time.
2. Check whether the service is running.
3. Read recent logs.
4. Check ports and processes.
5. Check disk and memory.
6. Save your findings before making changes.

The goal is to build a clear picture before touching the system.

## Cleanup

When finished, you can remove the lab:

```bash
cd ..
rm -r day-34-lab
```
