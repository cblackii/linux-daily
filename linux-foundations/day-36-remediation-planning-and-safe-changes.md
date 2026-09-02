# Day 36: Remediation Planning and Safe Changes

## Goal

Practice fixing a Linux issue in a controlled way: identify the problem, back up the current file, make a small change, verify the result, and document what changed.

Day 34 focused on incident triage.

Day 35 focused on root cause investigation.

Day 36 focuses on remediation: safely correcting the problem without creating a bigger one.

## Commands practiced

```bash
mkdir
cd
cp
cat
grep
diff
sed
date
hostname
stat
tee
ls
```

## Main concept

Good Linux administration is not just knowing how to change files.

Good administration means:

- Back up before changing.
- Make one focused change at a time.
- Verify the change.
- Keep evidence of what was changed.
- Leave yourself a rollback path.

This is the difference between guessing and operating safely.

## Lab setup

Create today's lab:

```bash
mkdir day-36-lab
cd day-36-lab
mkdir -p configs backups reports
```

Create a broken application config:

```bash
cat > configs/app.conf <<'EOF'
APP_NAME=linux-practice
APP_ENV=production
APP_PORT=8080
LOG_LEVEL=debug
CACHE_ENABLED=false
MAX_CONNECTIONS=50
EOF
```

View the file:

```bash
cat configs/app.conf
```

## Exercise 1: Identify the risky settings

Run:

```bash
grep "LOG_LEVEL" configs/app.conf
grep "CACHE_ENABLED" configs/app.conf
grep "MAX_CONNECTIONS" configs/app.conf
```

What this does:

- `grep` searches the config for specific settings.
- You are confirming the exact values before making changes.

In this lab, the likely problem is:

```bash
CACHE_ENABLED=false
```

If caching should be enabled in production, this setting may explain slow responses or application errors.

## Exercise 2: Create a backup before changing anything

Run:

```bash
cp configs/app.conf backups/app.conf.before-fix
```

Verify the backup exists:

```bash
ls backups
cat backups/app.conf.before-fix
```

What this does:

- `cp` copies the current config into the backup folder.
- The backup gives you a rollback point if the change causes problems.

This is one of the most important habits in Linux administration.

## Exercise 3: Make one focused change

Use `sed` to enable caching:

```bash
sed -i 's/CACHE_ENABLED=false/CACHE_ENABLED=true/' configs/app.conf
```

What this does:

- `sed` edits text.
- `s/old/new/` means substitute old text with new text.
- `-i` edits the file in place.

You are changing only one setting:

```bash
CACHE_ENABLED=false
```

to:

```bash
CACHE_ENABLED=true
```

## Exercise 4: Verify the change

Run:

```bash
grep "CACHE_ENABLED" configs/app.conf
```

Then compare the backup to the changed file:

```bash
diff backups/app.conf.before-fix configs/app.conf
```

What this does:

- `grep` confirms the setting now has the intended value.
- `diff` shows exactly what changed between the backup and the current file.

You should see only one meaningful config change.

That is the point: small, controlled changes are easier to verify and easier to roll back.

## Exercise 5: Capture file metadata

Run:

```bash
stat configs/app.conf
stat backups/app.conf.before-fix
```

What this does:

- `stat` shows file metadata, including size, permissions, ownership, and timestamps.

This helps you prove when a file was changed and compare it to the backup.

## Exercise 6: Create a remediation report

Run:

```bash
{
  echo "Day 36 Remediation Report"
  echo "========================="
  echo
  echo "Change time:"
  date
  echo
  echo "Hostname:"
  hostname
  echo
  echo "Issue:"
  echo "Application cache was disabled in production configuration."
  echo
  echo "Backup file:"
  echo "backups/app.conf.before-fix"
  echo
  echo "Change made:"
  diff backups/app.conf.before-fix configs/app.conf
  echo
  echo "Verification:"
  grep "CACHE_ENABLED" configs/app.conf
  echo
  echo "Rollback plan:"
  echo "Copy backups/app.conf.before-fix back to configs/app.conf if the change causes issues."
} | tee reports/remediation-report.txt
```

What this does:

- Groups the important investigation details.
- Saves the report to `reports/remediation-report.txt`.
- Shows the report in the terminal at the same time.

The report explains what changed, why it changed, how it was verified, and how to roll it back.

## Exercise 7: Practice rollback

Copy the backup back over the changed file:

```bash
cp backups/app.conf.before-fix configs/app.conf
```

Verify the rollback:

```bash
grep "CACHE_ENABLED" configs/app.conf
```

You should see:

```bash
CACHE_ENABLED=false
```

Now reapply the fix:

```bash
sed -i 's/CACHE_ENABLED=false/CACHE_ENABLED=true/' configs/app.conf
grep "CACHE_ENABLED" configs/app.conf
```

What this does:

- The first `cp` restores the original file.
- The `grep` confirms the rollback happened.
- The final `sed` applies the fix again.

Rollback practice matters because production changes should never be one-way doors.

## Verify your work

Run:

```bash
ls -R
cat reports/remediation-report.txt
```

Confirm that you have:

- A config file in `configs/`
- A backup file in `backups/`
- A remediation report in `reports/`
- A changed config showing `CACHE_ENABLED=true`

## Main takeaway

Safe remediation follows this pattern:

1. Confirm the setting or file you plan to change.
2. Back up the current state.
3. Make one focused change.
4. Verify exactly what changed.
5. Document the fix and rollback plan.
6. Practice rollback before you need it under pressure.

This is how you fix Linux systems like an administrator instead of experimenting blindly.

## Cleanup

When finished:

```bash
cd ..
rm -r day-36-lab
```
