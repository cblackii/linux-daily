# Day 8: Archives and Compression

## Commands practiced

- `tar`
- `tar -cf`
- `tar -tf`
- `tar -xf`
- `tar -czf`
- `tar -xzf`
- `gzip`
- `gunzip`
- `file`
- `ls -lh`

## Practice

Run these inside the Linux VM:

```bash
mkdir day-08-lab
cd day-08-lab
```

Create a small project folder:

```bash
mkdir project
echo "Day 8 Linux practice" > project/README.txt
echo "alpha" > project/alpha.txt
echo "beta" > project/beta.txt
echo "gamma" > project/gamma.txt
```

Inspect the files:

```bash
ls
ls project
ls -lh project
```

Create a tar archive:

```bash
tar -cf project.tar project
ls -lh
```

List the archive contents before extracting:

```bash
tar -tf project.tar
```

Extract the archive into a new folder:

```bash
mkdir extracted
tar -xf project.tar -C extracted
ls extracted
ls extracted/project
```

Create a compressed tar archive:

```bash
tar -czf project.tar.gz project
ls -lh
```

Check file types:

```bash
file project.tar
file project.tar.gz
```

Extract the compressed archive:

```bash
mkdir compressed-extracted
tar -xzf project.tar.gz -C compressed-extracted
ls compressed-extracted/project
```

Practice gzip on one file:

```bash
gzip project/alpha.txt
ls project
gunzip project/alpha.txt.gz
ls project
```

Clean up:

```bash
cd ..
rm -r day-08-lab
```

## Notes

Today I practiced creating archives, viewing archive contents before extracting, extracting archives, compressing archives, and checking file types. `tar` bundles files together, while gzip compression makes files smaller.
