# Day 4: Pipes and Filters

## Commands practiced

- `|`
- `sort`
- `uniq`
- `wc`
- `grep`
- `cat`
- `head`
- `tail`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-04-lab
cd day-04-lab
```

Create a practice file:

```bash
echo "linux" > tools.txt
echo "git" >> tools.txt
echo "docker" >> tools.txt
echo "linux" >> tools.txt
echo "terminal" >> tools.txt
echo "git" >> tools.txt
echo "ssh" >> tools.txt
```

Read the file:

```bash
cat tools.txt
```

Sort the lines:

```bash
sort tools.txt
```

Remove repeated lines after sorting:

```bash
sort tools.txt | uniq
```

Count lines, words, and characters:

```bash
wc tools.txt
wc -l tools.txt
```

Search, then count matches:

```bash
grep "git" tools.txt
grep "git" tools.txt | wc -l
```

Save filtered output:

```bash
sort tools.txt | uniq > unique-tools.txt
cat unique-tools.txt
```

Clean up:

```bash
cd ..
rm day-04-lab/tools.txt
rm day-04-lab/unique-tools.txt
rmdir day-04-lab
```

## Notes

Today I practiced sending the output of one command into another command using pipes. I also practiced sorting text, removing duplicate lines, counting output, and saving filtered results.
