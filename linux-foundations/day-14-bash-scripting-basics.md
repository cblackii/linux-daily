# Day 14: Bash Scripting Basics

## Commands practiced

- `nano`
- `bash`
- `chmod +x`
- `./script-name`
- `$1`
- `$?`
- `if`
- `test`
- `[ ]`
- `echo`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-14-lab
cd day-14-lab
```

Create a simple script:

```bash
nano hello.sh
```

Add this content:

```bash
#!/bin/bash

echo "Hello from Bash"
echo "Current user: $USER"
echo "Home directory: $HOME"
```

Run the script with Bash:

```bash
bash hello.sh
```

Make the script executable:

```bash
chmod +x hello.sh
./hello.sh
```

Create a script that accepts an argument:

```bash
nano greet.sh
```

Add this content:

```bash
#!/bin/bash

echo "Hello, $1"
```

Run it with an argument:

```bash
bash greet.sh Charles
bash greet.sh Linux
```

Check command exit status:

```bash
ls
echo "$?"
ls missing-file.txt
echo "$?"
```

Create a script with a basic condition:

```bash
nano check-file.sh
```

Add this content:

```bash
#!/bin/bash

if [ -f "$1" ]; then
  echo "$1 exists and is a regular file"
else
  echo "$1 was not found"
fi
```

Run the condition script:

```bash
bash check-file.sh hello.sh
bash check-file.sh missing-file.txt
```

Make the new scripts executable:

```bash
chmod +x greet.sh check-file.sh
./greet.sh Ubuntu
./check-file.sh hello.sh
```

Clean up:

```bash
cd ..
rm -r day-14-lab
```

## Notes

Today I practiced writing and running basic Bash scripts. I learned how to run scripts with `bash`, make scripts executable with `chmod +x`, pass arguments with `$1`, check command success or failure with `$?`, and use a simple `if` statement to make decisions.
