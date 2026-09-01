# Day 35: Root Cause Investigation

## Goal

Practice a structured Linux troubleshooting workflow for moving from symptoms to likely root cause.

Yesterday focused on incident triage: quickly checking the system, service, logs, ports, processes, disk, and memory.

Today focuses on investigation: comparing evidence, narrowing the failure, and documenting what most likely caused the issue.

## Commands practiced

```bash
date
hostname
uptime
ls
cat
tail
grep
sort
uniq
wc
diff
find
stat
df
free
tee
```

## Main concept

Root cause investigation is the process of asking:

- What changed?
- When did it change?
- What errors appeared after the change?
- Is the issue repeated or isolated?
- What evidence supports the conclusion?

The goal is not to guess. The goal is to use system evidence to make the strongest possible conclusion.

## Lab setup

Create today's lab folder:

```bash
mkdir day-35-lab
cd day-35-lab
mkdir -p configs logs reports
```

Create a known-good configuration file:

```bash
cat > configs/app.conf.good <<'EOF'
APP_NAME=linux-practice
APP_ENV=production
APP_PORT=8080
LOG_LEVEL=info
CACHE_ENABLED=true
EOF
```

Create a current configuration file with a problem:

```bash
cat > configs/app.conf.current <<'EOF'
APP_NAME=linux-practice
APP_ENV=production
APP_PORT=8080
LOG_LEVEL=debug
CACHE_ENABLED=false
EOF
```

Create a simulated application log:

```bash
cat > logs/app.log <<'EOF'
2026-08-31 07:55:01 INFO application started
2026-08-31 07:56:14 INFO listening on port 8080
2026-08-31 08:01:33 WARN cache disabled
2026-08-31 08:02:10 WARN slower response time detected
2026-08-31 08:03:22 ERROR cache lookup failed
2026-08-31 08:04:18 ERROR request timeout
2026-08-31 08:05:44 ERROR request timeout
2026-08-31 08:06:12 INFO health check degraded
EOF
```

## Exercise 1: Capture the investigation context

Run:

```bash
date
hostname
uptime
```

What this does:

- `date` records when you are investigating.
- `hostname` confirms which system you are on.
- `uptime` shows how long the system has been running and gives a quick load average.

In real troubleshooting, always record the system and time before making changes.

## Exercise 2: Inspect the evidence files

Run:

```bash
ls -R
cat configs/app.conf.current
tail -n 20 logs/app.log
```

What this does:

- `ls -R` shows the lab folder structure recursively.
- `cat` prints the current configuration.
- `tail` shows the most recent log entries.

You are looking for clues: warnings, errors, recent changes, or unusual settings.

## Exercise 3: Compare known-good and current config

Run:

```bash
diff configs/app.conf.good configs/app.conf.current
```

What this does:

- `diff` compares two files line by line.
- It shows which settings changed between the good configuration and the current one.

Expected finding:

```bash
LOG_LEVEL=info
CACHE_ENABLED=true
```

changed to:

```bash
LOG_LEVEL=debug
CACHE_ENABLED=false
```

This gives you a possible cause: caching was disabled.

## Exercise 4: Search the logs for symptoms

Run:

```bash
grep "WARN" logs/app.log
grep "ERROR" logs/app.log
grep "cache" logs/app.log
```

What this does:

- `grep "WARN"` finds warning messages.
- `grep "ERROR"` finds failure messages.
- `grep "cache"` finds log entries related to cache behavior.

The logs connect the config change to the symptoms:

- Cache was disabled.
- Cache lookup failed.
- Requests started timing out.

## Exercise 5: Count repeated errors

Run:

```bash
grep "ERROR" logs/app.log | wc -l
grep "request timeout" logs/app.log | wc -l
```

What this does:

- `grep` filters the log to matching lines.
- `wc -l` counts how many matching lines were found.
- The pipe sends the filtered output into the counter.

This helps you decide whether the issue happened once or repeatedly.

## Exercise 6: Check file details

Run:

```bash
stat configs/app.conf.current
stat logs/app.log
```

What this does:

- `stat` shows file metadata, including size, ownership, permissions, and timestamps.

In real troubleshooting, timestamps can help answer: did this file change around the time the issue started?

## Exercise 7: Check system resources

Run:

```bash
df -h
free -h
```

What this does:

- `df -h` checks disk usage.
- `free -h` checks memory usage.

This helps rule out common system-level causes like full disk or low memory.

## Exercise 8: Create a root cause report

Run:

```bash
{
  echo "Day 35 Root Cause Investigation Report"
  echo "======================================"
  echo
  echo "Investigation time:"
  date
  echo
  echo "Hostname:"
  hostname
  echo
  echo "Uptime:"
  uptime
  echo
  echo "Configuration differences:"
  diff configs/app.conf.good configs/app.conf.current
  echo
  echo "Warnings found:"
  grep "WARN" logs/app.log
  echo
  echo "Errors found:"
  grep "ERROR" logs/app.log
  echo
  echo "Error count:"
  grep "ERROR" logs/app.log | wc -l
  echo
  echo "Likely root cause:"
  echo "CACHE_ENABLED changed from true to false. Logs show cache warnings and request timeouts after that setting."
} | tee reports/root-cause-report.txt
```

What this does:

- Groups multiple checks into one report.
- Saves the report with `tee`.
- Documents the evidence and likely conclusion.

Good troubleshooting is not just finding the answer. It is showing how you got there.

## Verify your report

Run:

```bash
cat reports/root-cause-report.txt
```

Confirm the report includes:

- Investigation time
- Hostname
- Uptime
- Config differences
- Warnings
- Errors
- Error count
- Likely root cause

## Main takeaway

Root cause investigation follows the evidence:

1. Capture system context.
2. Inspect current logs and files.
3. Compare known-good and current state.
4. Search for repeated symptoms.
5. Rule out resource issues.
6. Write down the most likely cause with supporting evidence.

This is how you move from reaction mode to administrator mode.

## Cleanup

When finished:

```bash
cd ..
rm -r day-35-lab
```
