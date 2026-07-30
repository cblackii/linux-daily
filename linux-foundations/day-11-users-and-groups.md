# Day 11: Users and Groups

## Commands practiced

- `whoami`
- `id`
- `groups`
- `getent passwd`
- `getent group`
- `sudo -l`
- `sudo whoami`
- `cat /etc/passwd`
- `cat /etc/group`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-11-lab
cd day-11-lab
```

Check your current username:

```bash
whoami
```

Check your user ID, group ID, and group memberships:

```bash
id
groups
```

Check what user `sudo` becomes:

```bash
sudo whoami
```

List what your user is allowed to run with `sudo`:

```bash
sudo -l
```

Look up your user account from the system database:

```bash
getent passwd ubuntu
```

Look up common groups:

```bash
getent group sudo
getent group adm
```

Inspect user account records:

```bash
cat /etc/passwd
```

Inspect group records:

```bash
cat /etc/group
```

Save a short identity report:

```bash
whoami > identity-report.txt
id >> identity-report.txt
groups >> identity-report.txt
cat identity-report.txt
```

Search for your user and sudo group:

```bash
grep "ubuntu" /etc/passwd
grep "sudo" /etc/group
```

Clean up:

```bash
cd ..
rm day-11-lab/identity-report.txt
rmdir day-11-lab
```

## Notes

Today I practiced checking the current user, user IDs, group memberships, sudo privileges, and Linux account records. Users identify who is logged in, groups organize permissions, and `sudo` temporarily runs commands with administrator privileges.
