# Day 31: Configuration File Management

## Commands practiced

- `cp`
- `diff`
- `grep`
- `sed`
- `awk`
- `mkdir`
- `cat`
- `tee`
- `sha256sum`
- `date`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-31-lab
cd day-31-lab
```

Create a practice application config:

```bash
mkdir -p app backups reports
cat > app/app.conf <<'EOF'
app_name=linux-training
environment=dev
port=8080
log_level=info
backup_enabled=false
EOF
```

Inspect the config:

```bash
cat app/app.conf
grep "environment" app/app.conf
grep "port" app/app.conf
```

Create a timestamped backup before editing:

```bash
cp app/app.conf "backups/app.conf.$(date +%Y%m%d-%H%M%S).bak"
ls -lh backups
```

Create a stable backup for comparison:

```bash
cp app/app.conf backups/app.conf.original
```

Record the original checksum:

```bash
sha256sum app/app.conf > reports/original-checksum.txt
cat reports/original-checksum.txt
```

Preview config values with `awk`:

```bash
awk -F '=' '{print "Key:", $1, "Value:", $2}' app/app.conf
```

Update config values with `sed`:

```bash
sed -i 's/environment=dev/environment=training/' app/app.conf
sed -i 's/log_level=info/log_level=debug/' app/app.conf
sed -i 's/backup_enabled=false/backup_enabled=true/' app/app.conf
cat app/app.conf
```

Compare the original and updated config:

```bash
diff backups/app.conf.original app/app.conf
```

Validate required config keys:

```bash
grep "^app_name=" app/app.conf
grep "^environment=" app/app.conf
grep "^port=" app/app.conf
grep "^log_level=" app/app.conf
grep "^backup_enabled=" app/app.conf
```

Create an updated checksum:

```bash
sha256sum app/app.conf > reports/updated-checksum.txt
cat reports/updated-checksum.txt
```

Create a config change report:

```bash
{
  echo "Day 31 configuration management report"
  date
  echo
  echo "Current config:"
  cat app/app.conf
  echo
  echo "Changes from original:"
  diff backups/app.conf.original app/app.conf
  echo
  echo "Original checksum:"
  cat reports/original-checksum.txt
  echo
  echo "Updated checksum:"
  cat reports/updated-checksum.txt
} | tee reports/config-change-report.txt
```

Review the report:

```bash
cat reports/config-change-report.txt
```

Restore the original config:

```bash
cp backups/app.conf.original app/app.conf
cat app/app.conf
diff backups/app.conf.original app/app.conf
```

Clean up:

```bash
cd ..
rm -r day-31-lab
```

## Notes

Today I practiced managing configuration files safely. I created a config file, backed it up before editing, changed values with `sed`, compared versions with `diff`, validated required keys with `grep`, recorded checksums, generated a report, and restored the original config. A safe configuration workflow is backup, edit, compare, validate, document, and restore if needed.
