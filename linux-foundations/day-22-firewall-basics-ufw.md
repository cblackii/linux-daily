# Day 22: Firewall Basics with UFW

## Commands practiced

- `sudo ufw status`
- `sudo ufw status verbose`
- `sudo ufw app list`
- `sudo ufw allow`
- `sudo ufw delete allow`
- `sudo ufw deny`
- `sudo ufw delete deny`
- `sudo ufw show added`
- `ss -tuln`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-22-lab
cd day-22-lab
```

Check whether UFW is installed:

```bash
which ufw
ufw --version
```

Check current firewall status:

```bash
sudo ufw status
sudo ufw status verbose
```

List known application profiles:

```bash
sudo ufw app list
```

Check listening ports before changing firewall rules:

```bash
ss -tuln
```

Preview currently added rules:

```bash
sudo ufw show added
```

Allow SSH by service name:

```bash
sudo ufw allow ssh
sudo ufw show added
sudo ufw status numbered
```

Remove the SSH allow rule:

```bash
sudo ufw delete allow ssh
sudo ufw show added
```

Allow the SSH lab port used from your Mac:

```bash
sudo ufw allow 22/tcp
sudo ufw show added
```

Remove the port rule:

```bash
sudo ufw delete allow 22/tcp
sudo ufw show added
```

Practice adding and removing a deny rule:

```bash
sudo ufw deny 8080/tcp
sudo ufw show added
sudo ufw delete deny 8080/tcp
sudo ufw show added
```

Save a firewall report:

```bash
sudo ufw status verbose > firewall-report.txt
sudo ufw show added >> firewall-report.txt
ss -tuln >> firewall-report.txt
cat firewall-report.txt
```

Clean up:

```bash
cd ..
rm -r day-22-lab
```

## Notes

Today I practiced inspecting and managing firewall rules with UFW. A firewall controls which network traffic is allowed or denied. `ufw status` shows the current firewall state, `ufw allow` permits traffic, `ufw deny` blocks traffic, and `ufw delete` removes rules. `ss -tuln` helps compare firewall rules against services actually listening on the machine.
