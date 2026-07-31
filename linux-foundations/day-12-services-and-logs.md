# Day 12: Services and Logs

## Commands practiced

- `systemctl`
- `systemctl status`
- `systemctl list-units`
- `systemctl is-active`
- `systemctl is-enabled`
- `journalctl`
- `journalctl -u`
- `journalctl -n`
- `journalctl -f`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-12-lab
cd day-12-lab
```

Check the SSH service status:

```bash
systemctl status ssh
```

Check whether SSH is currently active:

```bash
systemctl is-active ssh
```

Check whether SSH is enabled to start at boot:

```bash
systemctl is-enabled ssh
```

List active services:

```bash
systemctl list-units --type=service
```

List failed services:

```bash
systemctl --failed
```

Read recent system logs:

```bash
journalctl -n 20
```

Read logs for the SSH service:

```bash
journalctl -u ssh -n 20
```

Follow live logs:

```bash
journalctl -f
```

Press `Ctrl+C` to stop following logs.

Save a short service report:

```bash
systemctl is-active ssh > service-report.txt
systemctl is-enabled ssh >> service-report.txt
systemctl --failed >> service-report.txt
cat service-report.txt
```

Clean up:

```bash
cd ..
rm day-12-lab/service-report.txt
rmdir day-12-lab
```

## Notes

Today I practiced checking Linux services and reading system logs. `systemctl` manages services, while `journalctl` reads logs collected by systemd. These commands help diagnose whether a server service is running, enabled at boot, failing, or producing useful log messages.
