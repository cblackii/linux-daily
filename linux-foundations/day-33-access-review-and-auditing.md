# Day 33: Access Review and Auditing

## Commands practiced

- `whoami`
- `id`
- `groups`
- `getent passwd`
- `getent group`
- `last`
- `lastlog`
- `sudo -l`
- `find`
- `stat`
- `tee`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-33-lab
cd day-33-lab
```

Confirm your current user identity:

```bash
whoami
id
groups
```

Review the `ubuntu` user account:

```bash
getent passwd ubuntu
getent group ubuntu
getent group sudo
```

Check your sudo permissions:

```bash
sudo -l
```

Review recent login history:

```bash
last | head
```

Review last login information:

```bash
lastlog | head
```

Inspect SSH-related files:

```bash
ls -la ~/.ssh
find ~/.ssh -maxdepth 1 -type f -print
```

Check permissions on the SSH directory:

```bash
stat ~/.ssh
```

If `authorized_keys` exists, inspect it:

```bash
cat ~/.ssh/authorized_keys
stat ~/.ssh/authorized_keys
```

Find world-writable files in your home directory:

```bash
find ~ -maxdepth 3 -type f -perm -002 -print
```

Find executable files in your home directory:

```bash
find ~ -maxdepth 3 -type f -perm -111 -print
```

Create a small access review report:

```bash
{
  echo "Day 33 access review report"
  date
  echo
  echo "Current user:"
  whoami
  echo
  echo "ID and groups:"
  id
  groups
  echo
  echo "Ubuntu account:"
  getent passwd ubuntu
  echo
  echo "Sudo group:"
  getent group sudo
  echo
  echo "Recent logins:"
  last | head
  echo
  echo "SSH directory:"
  ls -la ~/.ssh
  echo
  echo "World-writable files under home:"
  find ~ -maxdepth 3 -type f -perm -002 -print
  echo
  echo "Executable files under home:"
  find ~ -maxdepth 3 -type f -perm -111 -print
} | tee access-review-report.txt
```

Review the report:

```bash
cat access-review-report.txt
```

Clean up:

```bash
cd ..
rm -r day-33-lab
```

## Notes

Today I practiced a basic Linux access review. I checked my current identity, group memberships, sudo permissions, recent login history, SSH files, file permissions, world-writable files, and executable files. Access reviews help confirm who can log in, who can use elevated privileges, and whether sensitive files have appropriate permissions.
