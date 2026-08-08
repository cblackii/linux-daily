# Day 19: Text Processing with sed and awk

## Commands practiced

- `sed`
- `sed -n`
- `sed 's/old/new/'`
- `awk`
- `awk -F`
- `printf`
- `sort`
- `grep`
- `tee`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-19-lab
cd day-19-lab
```

Create a small server inventory:

```bash
printf '%s\n' \
  'web-01,production,ubuntu,4,8192,running' \
  'web-02,production,ubuntu,4,8192,running' \
  'db-01,production,ubuntu,8,16384,running' \
  'test-01,staging,ubuntu,2,4096,stopped' \
  'build-01,development,debian,4,8192,running' \
  > servers.csv
cat servers.csv
```

Use `sed` to display selected lines:

```bash
sed -n '1p' servers.csv
sed -n '2,4p' servers.csv
```

Preview a text replacement without changing the file:

```bash
sed 's/production/prod/' servers.csv
cat servers.csv
```

Save the transformed output to a new file:

```bash
sed 's/production/prod/' servers.csv > servers-normalized.csv
cat servers-normalized.csv
```

Use `awk` with a comma field separator:

```bash
awk -F ',' '{print $1}' servers.csv
awk -F ',' '{print $1, $2, $6}' servers.csv
```

Add readable column headings:

```bash
awk -F ',' 'BEGIN {print "HOST ENVIRONMENT STATUS"} {print $1, $2, $6}' servers.csv
```

Select production servers:

```bash
awk -F ',' '$2 == "production" {print $1, $6}' servers.csv
```

Select stopped servers:

```bash
awk -F ',' '$6 == "stopped" {print $1, $2}' servers.csv
```

Select servers with at least 8 GB of memory:

```bash
awk -F ',' '$5 >= 8192 {print $1, $5 " MB"}' servers.csv
```

Calculate total CPU cores and memory:

```bash
awk -F ',' '{cores += $4} END {print "Total CPU cores:", cores}' servers.csv
awk -F ',' '{memory += $5} END {print "Total memory:", memory, "MB"}' servers.csv
```

Count servers by status:

```bash
awk -F ',' '{print $6}' servers.csv | sort | uniq -c
```

Combine `grep`, `sed`, and `awk`:

```bash
grep 'production' servers.csv | sed 's/production/prod/' | awk -F ',' '{print $1, $2, $6}'
```

Create a server inventory report:

```bash
{
  echo 'Day 19 server inventory report'
  date
  echo
  echo 'Production servers:'
  awk -F ',' '$2 == "production" {print $1, $6}' servers.csv
  echo
  echo 'Stopped servers:'
  awk -F ',' '$6 == "stopped" {print $1, $2}' servers.csv
  echo
  awk -F ',' '{cores += $4; memory += $5} END {
    print "Total CPU cores:", cores
    print "Total memory:", memory, "MB"
  }' servers.csv
} | tee inventory-report.txt
```

Review the report:

```bash
cat inventory-report.txt
```

Clean up after the lab and documentation are confirmed:

```bash
cd ..
rm -r day-19-lab
```

## Notes

Today I practiced transforming and analyzing structured text with `sed` and `awk`. I selected lines, previewed text substitutions, used comma-separated fields, filtered records by text and numeric values, calculated totals, combined commands in pipelines, and created a server inventory report.
