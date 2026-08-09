# Day 21: SSH Keys and Remote Access

## Commands practiced

- `ssh`
- `ssh-keygen`
- `ls -la`
- `cat`
- `chmod`
- `ssh -p`
- `ssh -i`
- `ssh-keygen -lf`
- `mkdir -p`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-21-lab
cd day-21-lab
```

Check your current SSH connection:

```bash
whoami
hostname
echo "$SSH_CONNECTION"
```

Inspect the SSH folder in your home directory:

```bash
ls -la ~/.ssh
```

If the folder does not exist, create it:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
ls -ld ~/.ssh
```

Create a practice SSH key pair:

```bash
ssh-keygen -t ed25519 -f ./day21_key -C "day-21-practice"
```

When prompted for a passphrase, press `Enter` twice to leave it empty for this lab.

Inspect the key files:

```bash
ls -la
cat day21_key.pub
```

Check the key fingerprint:

```bash
ssh-keygen -lf day21_key.pub
```

Fix private key permissions:

```bash
chmod 600 day21_key
ls -la day21_key
```

Review the SSH login command you use from your Mac:

```bash
echo "ssh -p 2222 ubuntu@localhost"
```

Review what an SSH key login command looks like:

```bash
echo "ssh -i ./day21_key -p 2222 ubuntu@localhost"
```

Do not run that key login command yet. This practice key has not been added to the server's `authorized_keys` file.

Inspect authorized keys if the file exists:

```bash
ls -la ~/.ssh
cat ~/.ssh/authorized_keys
```

Clean up the practice key files:

```bash
rm day21_key
rm day21_key.pub
cd ..
rmdir day-21-lab
```

## Notes

Today I practiced the basics of SSH keys and remote access. SSH uses a public/private key pair: the private key stays secret on the client, while the public key can be placed on a server in `~/.ssh/authorized_keys`. File permissions matter because SSH refuses to use private keys that are too open.
