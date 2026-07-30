# Day 10: Package Management

## Commands practiced

- `apt update`
- `apt search`
- `apt show`
- `apt list --installed`
- `apt install`
- `apt remove`
- `which`
- `command --version`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-10-lab
cd day-10-lab
```

Refresh the package index:

```bash
sudo apt update
```

Search for a package:

```bash
apt search tree
```

Show package details:

```bash
apt show tree
```

Install a small package:

```bash
sudo apt install tree
```

Confirm the command is available:

```bash
which tree
tree --version
```

Create a small directory structure:

```bash
mkdir -p project/docs project/scripts project/logs
touch project/README.md
touch project/docs/notes.txt
touch project/scripts/run.sh
touch project/logs/app.log
```

Use the installed command:

```bash
tree project
```

Check installed packages:

```bash
apt list --installed | grep tree
```

Remove the package:

```bash
sudo apt remove tree
```

Confirm it is gone:

```bash
which tree
```

Clean up:

```bash
cd ..
rm -r day-10-lab
```

## Notes

Today I practiced using Ubuntu's package manager to search for software, inspect package details, install a package, verify the installed command, use it, and remove it. `apt` manages system software from trusted repositories.
