# Day 15: System Maintenance

## Commands practiced

- `sudo apt update`
- `sudo apt upgrade`
- `sudo apt autoremove`
- `df -h`
- `du -sh`
- `lsb_release -a`
- `uname -a`
- `uptime`
- `cat`
- `>`
- `>>`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-15-lab
cd day-15-lab
```

Check the Ubuntu version:

```bash
lsb_release -a
```

Check kernel and system uptime:

```bash
uname -a
uptime
```

Check disk space for the whole system:

```bash
df -h
df -h /
```

Create a few files and check folder size:

```bash
echo "maintenance practice" > notes.txt
echo "checking disk usage" > disk.txt
echo "checking package updates" > packages.txt
du -sh .
ls -lh
```

Refresh package information:

```bash
sudo apt update
```

Preview available package upgrades:

```bash
apt list --upgradable
```

Upgrade installed packages:

```bash
sudo apt upgrade
```

Remove packages that are no longer needed:

```bash
sudo apt autoremove
```

Save a maintenance report:

```bash
lsb_release -a > maintenance-report.txt
uname -a >> maintenance-report.txt
uptime >> maintenance-report.txt
df -h / >> maintenance-report.txt
du -sh . >> maintenance-report.txt
cat maintenance-report.txt
```

Clean up:

```bash
cd ..
rm -r day-15-lab
```

## Notes

Today I practiced basic Linux system maintenance. I checked the OS version, kernel, uptime, disk space, folder size, available package upgrades, and package cleanup. `apt update` refreshes package information, `apt upgrade` updates installed packages, and `df`/`du` show system and folder storage usage.
