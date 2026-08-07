# Day 18: System Troubleshooting

## Commands practiced

- `ps`
- `pgrep`
- `jobs`
- `ss -tuln`
- `curl`
- `tail`
- `grep`
- `kill`
- `$!`
- `$?`
- `tee`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-18-lab
cd day-18-lab
```

Create a small web page for a temporary test service:

```bash
echo '<h1>Day 18 Linux troubleshooting lab</h1>' > index.html
cat index.html
```

Start a temporary web server in the background and save its process ID:

```bash
python3 -m http.server 8088 > server.log 2>&1 &
SERVER_PID=$!
echo "$SERVER_PID" > server.pid
echo "Temporary server PID: $SERVER_PID"
```

Confirm that the background job and process are running:

```bash
jobs
ps -p "$SERVER_PID" -o pid,ppid,stat,cmd
pgrep -af "http.server 8088"
```

Confirm that the server is listening on port 8088:

```bash
ss -tuln | grep ":8088"
```

Test the service:

```bash
curl -I http://127.0.0.1:8088
curl http://127.0.0.1:8088
echo "curl exit status: $?"
```

Read the service log:

```bash
cat server.log
tail server.log
```

Simulate a service failure:

```bash
kill "$SERVER_PID"
sleep 1
jobs
ps -p "$SERVER_PID"
```

Check whether the port is still listening:

```bash
ss -tuln | grep ":8088"
echo "port-check exit status: $?"
```

Try the failed service and inspect the exit status:

```bash
curl http://127.0.0.1:8088
echo "curl exit status: $?"
```

Restart the service:

```bash
python3 -m http.server 8088 >> server.log 2>&1 &
SERVER_PID=$!
echo "$SERVER_PID" > server.pid
sleep 1
```

Verify recovery:

```bash
ps -p "$SERVER_PID" -o pid,ppid,stat,cmd
ss -tuln | grep ":8088"
curl -I http://127.0.0.1:8088
```

Create a short incident report:

```bash
{
  echo "Day 18 incident report"
  echo "Service: Python HTTP server"
  echo "Port: 8088"
  echo "Recovered PID: $SERVER_PID"
  date
  ps -p "$SERVER_PID" -o pid,ppid,stat,cmd
  ss -tuln | grep ":8088"
  tail -n 5 server.log
} | tee incident-report.txt
```

Review the report:

```bash
cat incident-report.txt
```

Stop the temporary service:

```bash
kill "$SERVER_PID"
sleep 1
pgrep -af "http.server 8088"
```

Clean up after the lab and documentation are confirmed:

```bash
cd ..
rm -r day-18-lab
```

## Notes

Today I practiced a basic Linux troubleshooting workflow. I checked whether a process was running, verified that a port was listening, tested a service with `curl`, inspected its logs, checked command exit statuses, simulated a failure, restarted the service, verified recovery, and saved the results in an incident report.
