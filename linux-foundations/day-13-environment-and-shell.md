# Day 13: Environment Variables and Shell Configuration

## Commands practiced

- `env`
- `printenv`
- `echo`
- `export`
- `unset`
- `alias`
- `type`
- `source`
- `cat`
- `grep`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-13-lab
cd day-13-lab
```

View environment variables:

```bash
env
printenv
```

Inspect specific variables:

```bash
echo "$HOME"
echo "$USER"
echo "$SHELL"
echo "$PATH"
```

Create a shell variable:

```bash
LAB_NAME="day-13"
echo "$LAB_NAME"
```

Export a variable so child commands can use it:

```bash
export TRAINING_LEVEL="linux-foundations"
printenv TRAINING_LEVEL
```

Remove the exported variable:

```bash
unset TRAINING_LEVEL
printenv TRAINING_LEVEL
```

Create a temporary alias:

```bash
alias ll='ls -la'
ll
type ll
```

Create a small shell config file for practice:

```bash
echo 'export LAB_TOPIC="environment variables"' > lab-shell-config
echo "alias today='date'" >> lab-shell-config
cat lab-shell-config
```

Load the config into the current shell:

```bash
source lab-shell-config
echo "$LAB_TOPIC"
today
```

Search environment output:

```bash
env | grep "HOME"
env | grep "PATH"
```

Clean up:

```bash
unalias ll
unalias today
unset LAB_NAME
unset LAB_TOPIC
cd ..
rm day-13-lab/lab-shell-config
rmdir day-13-lab
```

## Notes

Today I practiced reading environment variables, creating shell variables, exporting variables, removing variables, creating aliases, and loading shell configuration with `source`. Environment variables store settings that commands and programs can read, while aliases create shortcuts for longer commands.
