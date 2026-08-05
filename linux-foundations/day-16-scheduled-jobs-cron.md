# Day 16: Scheduled Jobs with Cron

## Commands practiced

- `crontab -l`
- `crontab -e`
- `crontab -r`
- `date`
- `mkdir`
- `touch`
- `cat`
- `tail`
- `grep`
- `systemctl status cron`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-16-lab
cd day-16-lab
```

Check whether cron is running:

```bash
systemctl status cron
```

List your current cron jobs:

```bash
crontab -l
```

If there are no cron jobs yet, Linux may print:

```text
no crontab for ubuntu
```

Create a script for cron to run:

```bash
echo '#!/bin/bash' > write-time.sh
echo 'date >> "$HOME/day-16-lab/cron-output.txt"' >> write-time.sh
chmod +x write-time.sh
cat write-time.sh
```

Test the script manually:

```bash
./write-time.sh
cat cron-output.txt
```

Open your user crontab:

```bash
crontab -e
```

Add this line at the bottom:

```cron
* * * * * /home/ubuntu/day-16-lab/write-time.sh
```

Save and exit the editor.

Confirm the cron job exists:

```bash
crontab -l
```

Wait one or two minutes, then check the output:

```bash
cat cron-output.txt
tail cron-output.txt
```

Check cron-related logs:

```bash
journalctl -u cron -n 20
```

Remove the cron job when finished:

```bash
crontab -e
```

Delete the cron line you added:

```cron
* * * * * /home/ubuntu/day-16-lab/write-time.sh
```

Confirm it is removed:

```bash
crontab -l
```

Clean up:

```bash
cd ..
rm -r day-16-lab
```

## Notes

Today I practiced scheduling recurring tasks with cron. `crontab -e` edits scheduled jobs for the current user, `crontab -l` lists them, and each cron line defines when a command should run. The five time fields mean minute, hour, day of month, month, and day of week.
