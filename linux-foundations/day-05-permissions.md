# Day 5: File Permissions

## Commands practiced

- `ls -l`
- `chmod`
- `./script-name`
- `whoami`
- `groups`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-05-lab
cd day-05-lab
```

Create a script file:

```bash
echo '#!/bin/bash' > hello.sh
echo 'echo "Hello from Linux permissions practice"' >> hello.sh
```

Inspect the file:

```bash
ls -l hello.sh
```

Try to run it:

```bash
./hello.sh
```

It should fail at first because the file does not have execute permission.

Add execute permission:

```bash
chmod +x hello.sh
ls -l hello.sh
./hello.sh
```

Remove execute permission:

```bash
chmod -x hello.sh
ls -l hello.sh
```

Use numeric permissions:

```bash
chmod 644 hello.sh
ls -l hello.sh
```

```bash
chmod 755 hello.sh
ls -l hello.sh
./hello.sh
```

Check your user and groups:

```bash
whoami
groups
```

Clean up:

```bash
cd ..
rm day-05-lab/hello.sh
rmdir day-05-lab
```

## Notes

Today I practiced reading file permissions with `ls -l` and changing permissions with `chmod`. I learned that scripts need execute permission before they can be run directly with `./script-name`.
