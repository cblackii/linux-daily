# Day 6: Processes and Jobs

## Commands practiced

- `ps`
- `ps aux`
- `top`
- `sleep`
- `&`
- `jobs`
- `fg`
- `Ctrl+C`
- `kill`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-06-lab
cd day-06-lab
```

Check your current shell process:

```bash
ps
```

List all running processes:

```bash
ps aux
```

Search for your shell process:

```bash
ps aux | grep bash
```

Open the live process viewer:

```bash
top
```

Press `q` to exit `top`.

Start a short process:

```bash
sleep 10
```

Start a background process:

```bash
sleep 300 &
```

List background jobs:

```bash
jobs
```

Bring the background job to the foreground:

```bash
fg
```

Press `Ctrl+C` to stop it.

Start another background process:

```bash
sleep 300 &
```

Find the process ID:

```bash
ps aux | grep sleep
```

Stop the process by job number:

```bash
kill %1
```

Confirm it stopped:

```bash
jobs
ps aux | grep sleep
```

Clean up:

```bash
cd ..
rmdir day-06-lab
```

## Notes

Today I practiced viewing running processes, starting commands in the background, bringing jobs back to the foreground, and stopping processes with `kill`.
