# Day 23: Secure File Transfer with SCP

## Commands practiced

- `scp`
- `ssh`
- `mkdir`
- `ls -lh`
- `cat`
- `sha256sum`
- `diff`
- `tar -czf`
- `tar -tzf`
- `rm`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-23-lab
cd day-23-lab
```

Create source and destination folders:

```bash
mkdir outgoing incoming
```

Create files to transfer:

```bash
echo "Day 23 secure file transfer practice" > outgoing/report.txt
echo "server=linux-lab" > outgoing/server-info.txt
echo "status=ready" >> outgoing/server-info.txt
ls -lh outgoing
cat outgoing/report.txt
cat outgoing/server-info.txt
```

Confirm SSH works to localhost:

```bash
ssh ubuntu@localhost "hostname"
```

If prompted for a password, use the Linux VM password for the `ubuntu` user.

Copy one file with `scp`:

```bash
scp outgoing/report.txt ubuntu@localhost:/home/ubuntu/day-23-lab/incoming/
ls -lh incoming
cat incoming/report.txt
```

Verify the copied file matches the original:

```bash
sha256sum outgoing/report.txt incoming/report.txt
diff outgoing/report.txt incoming/report.txt
```

Copy a second file:

```bash
scp outgoing/server-info.txt ubuntu@localhost:/home/ubuntu/day-23-lab/incoming/
ls -lh incoming
```

Create a compressed archive for transfer:

```bash
tar -czf outgoing-bundle.tar.gz outgoing
ls -lh outgoing-bundle.tar.gz
tar -tzf outgoing-bundle.tar.gz
```

Copy the archive:

```bash
scp outgoing-bundle.tar.gz ubuntu@localhost:/home/ubuntu/day-23-lab/incoming/
ls -lh incoming
```

Verify the archive copy:

```bash
sha256sum outgoing-bundle.tar.gz incoming/outgoing-bundle.tar.gz
```

Review the SSH command shape you use from your Mac:

```bash
echo "ssh -p 2222 ubuntu@localhost"
```

Review the SCP command shape from your Mac to this VM:

```bash
echo "scp -P 2222 local-file.txt ubuntu@localhost:/home/ubuntu/"
```

Clean up:

```bash
cd ..
rm -r day-23-lab
```

## Notes

Today I practiced transferring files over SSH with `scp`. I copied individual files, verified copied files with checksums and `diff`, packaged a folder with `tar`, copied the archive, and reviewed the difference between `ssh -p` and `scp -P`. SSH uses lowercase `-p` for port, while SCP uses uppercase `-P`.
