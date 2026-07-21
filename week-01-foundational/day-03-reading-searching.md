# Day 3: Reading and Searching Files

## Commands practiced

- `cat`
- `less`
- `head`
- `tail`
- `grep`
- `find`
- `>`
- `>>`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-03-lab
cd day-03-lab
```

Create a practice file:

```bash
echo "Linux is powerful" > notes.txt
echo "The terminal is becoming familiar" >> notes.txt
echo "Practice builds confidence" >> notes.txt
echo "Linux commands are tools" >> notes.txt
```

Read the file:

```bash
cat notes.txt
head notes.txt
tail notes.txt
less notes.txt
```

Search inside the file:

```bash
grep "Linux" notes.txt
grep "terminal" notes.txt
```

Find files:

```bash
find . -name "notes.txt"
find . -type f
```

Save command output:

```bash
ls > files.txt
cat files.txt
grep "Linux" notes.txt > linux-lines.txt
cat linux-lines.txt
```

Clean up:

```bash
cd ..
rm day-03-lab/notes.txt
rm day-03-lab/files.txt
rm day-03-lab/linux-lines.txt
rmdir day-03-lab
```

## Notes

Today I practiced reading file contents, searching text, finding files, and saving command output into new files.
