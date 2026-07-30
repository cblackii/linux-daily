# Day 9: Networking Basics

## Commands practiced

- `ip addr`
- `ip route`
- `hostname -I`
- `ping`
- `curl`
- `ss`
- `getent hosts`
- `cat /etc/resolv.conf`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-09-lab
cd day-09-lab
```

Check your machine's network addresses:

```bash
hostname -I
ip addr
```

Check the default network route:

```bash
ip route
```

Test basic connectivity:

```bash
ping -c 4 8.8.8.8
```

Test DNS name resolution:

```bash
ping -c 4 google.com
getent hosts google.com
```

Inspect DNS configuration:

```bash
cat /etc/resolv.conf
```

Make a basic web request:

```bash
curl -I https://example.com
```

Check listening and connected network sockets:

```bash
ss -tuln
```

Save networking notes:

```bash
hostname -I > network-notes.txt
ip route >> network-notes.txt
cat network-notes.txt
```

Clean up:

```bash
cd ..
rm day-09-lab/network-notes.txt
rmdir day-09-lab
```

## Notes

Today I practiced checking Linux network addresses, routes, DNS resolution, web connectivity, and open network sockets. These commands help diagnose whether a server can reach the network and whether name lookup is working.
