# Day 32: Deployment and Rollback Basics

## Commands practiced

- `mkdir`
- `echo`
- `ln -s`
- `ln -sfn`
- `readlink`
- `ls -l`
- `cat`
- `diff`
- `date`
- `tee`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-32-lab
cd day-32-lab
```

Create a release-style directory layout:

```bash
mkdir -p app/releases app/shared app/reports
```

Create version 1 of an application:

```bash
mkdir app/releases/v1
echo "version=1" > app/releases/v1/app.conf
echo "Welcome from version 1" > app/releases/v1/index.html
```

Point `current` to version 1:

```bash
ln -s releases/v1 app/current
ls -l app
readlink app/current
cat app/current/app.conf
cat app/current/index.html
```

Create version 2:

```bash
mkdir app/releases/v2
echo "version=2" > app/releases/v2/app.conf
echo "Welcome from version 2" > app/releases/v2/index.html
```

Compare version 1 and version 2:

```bash
diff app/releases/v1/app.conf app/releases/v2/app.conf
diff app/releases/v1/index.html app/releases/v2/index.html
```

Deploy version 2 by updating the symlink:

```bash
ln -sfn releases/v2 app/current
ls -l app
readlink app/current
cat app/current/app.conf
cat app/current/index.html
```

Simulate a bad release:

```bash
mkdir app/releases/v3
echo "version=3" > app/releases/v3/app.conf
echo "BROKEN RELEASE" > app/releases/v3/index.html
ln -sfn releases/v3 app/current
cat app/current/index.html
```

Rollback to version 2:

```bash
ln -sfn releases/v2 app/current
readlink app/current
cat app/current/app.conf
cat app/current/index.html
```

Create a deployment report:

```bash
{
  echo "Day 32 deployment rollback report"
  date
  echo
  echo "Available releases:"
  ls -l app/releases
  echo
  echo "Current symlink:"
  ls -l app/current
  echo
  echo "Current release path:"
  readlink app/current
  echo
  echo "Current app config:"
  cat app/current/app.conf
  echo
  echo "Current app page:"
  cat app/current/index.html
} | tee app/reports/deployment-report.txt
```

Review the report:

```bash
cat app/reports/deployment-report.txt
```

Clean up:

```bash
cd ..
rm -r day-32-lab
```

## Notes

Today I practiced a basic deployment and rollback pattern. Each release lived in its own folder, and the `current` symlink pointed to the active release. Deploying meant moving `current` to a newer release. Rolling back meant pointing `current` back to the last known good release. This pattern keeps old versions available and makes rollback fast.
