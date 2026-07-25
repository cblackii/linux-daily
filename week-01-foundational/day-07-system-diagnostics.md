# Day 7: System Diagnostics

## Commands practiced

- `uname`
- `hostname`
- `uptime`
- `df`
- `du`
- `free`
- `which`
- `type`
- `history`
- `clear`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-07-lab
cd day-07-lab
```

Check basic system identity:

```bash
hostname
uname
uname -a
```

Check how long the system has been running:

```bash
uptime
```

Check disk space:

```bash
df
df -h
```

Create files and check directory size:

```bash
echo "one" > one.txt
echo "two" > two.txt
echo "three" > three.txt
du
du -h
du -sh .
```

Check memory:

```bash
free
free -h
```

Find where commands live:

```bash
which ls
which bash
which grep
```

Check what kind of command something is:

```bash
type cd
type ls
type grep
```

Review recent commands:

```bash
history
history | tail
```

Clear the terminal screen:

```bash
clear
```

Clean up:

```bash
cd ..
rm day-07-lab/one.txt
rm day-07-lab/two.txt
rm day-07-lab/three.txt
rmdir day-07-lab
```

## Notes

Today I practiced checking Linux system information, disk usage, memory usage, command locations, command types, and shell history. These commands help diagnose what kind of machine I am using and what resources are available.
