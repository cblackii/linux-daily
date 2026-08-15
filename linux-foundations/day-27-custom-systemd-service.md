# Day 27: Custom systemd Service

## Commands practiced

- `systemctl`
- `systemctl status`
- `systemctl start`
- `systemctl stop`
- `systemctl enable`
- `systemctl disable`
- `systemctl daemon-reload`
- `journalctl -u`
- `sudo tee`
- `chmod +x`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-27-lab
cd day-27-lab
```

Create a small service script:

```bash
cat > heartbeat.sh <<'EOF'
#!/bin/bash

while true; do
  echo "$(date) heartbeat from day 27 service" >> /home/ubuntu/day-27-lab/heartbeat.log
  sleep 10
done
EOF
```

Make the script executable:

```bash
chmod +x heartbeat.sh
ls -lh heartbeat.sh
```

Test the script briefly:

```bash
./heartbeat.sh
```

Press `Ctrl+C` after a few seconds.

Check the log:

```bash
cat heartbeat.log
```

Create a systemd service file:

```bash
sudo tee /etc/systemd/system/day27-heartbeat.service > /dev/null <<'EOF'
[Unit]
Description=Day 27 Heartbeat Practice Service
After=network.target

[Service]
Type=simple
User=ubuntu
WorkingDirectory=/home/ubuntu/day-27-lab
ExecStart=/home/ubuntu/day-27-lab/heartbeat.sh
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```

Review the service file:

```bash
cat /etc/systemd/system/day27-heartbeat.service
```

Reload systemd so it notices the new service:

```bash
sudo systemctl daemon-reload
```

Start the service:

```bash
sudo systemctl start day27-heartbeat
```

Check service status:

```bash
systemctl status day27-heartbeat
```

Check the service log file:

```bash
tail heartbeat.log
```

Read service logs from journald:

```bash
journalctl -u day27-heartbeat -n 20
```

Enable the service to start at boot:

```bash
sudo systemctl enable day27-heartbeat
systemctl is-enabled day27-heartbeat
```

Stop the service:

```bash
sudo systemctl stop day27-heartbeat
systemctl status day27-heartbeat
```

Disable the service:

```bash
sudo systemctl disable day27-heartbeat
systemctl is-enabled day27-heartbeat
```

Remove the practice service:

```bash
sudo rm /etc/systemd/system/day27-heartbeat.service
sudo systemctl daemon-reload
sudo systemctl reset-failed
```

Clean up:

```bash
cd ..
rm -r day-27-lab
```

## Notes

Today I practiced creating a custom `systemd` service. The script performed the work, the service file told systemd how to run it, and `systemctl` managed its lifecycle. `daemon-reload` makes systemd reread service files, `start` runs the service now, `enable` configures it to start at boot, `stop` stops it, and `journalctl -u` reads logs for one service.
