# Exam 4
Find, Grep, Regex, awk, sed, partition

## Find
https://computing.utahtech.edu/it/1100/slides/slides17.php

Find filename in root
```bash
find / -type f -name "Fish_Stocking_Report_2014.csv" 2> /dev/null
```

Find jpg files in home that are larger than 1Mb | count the amount of files
```bash
find ~ -type f -name "*.JPG" -size +1M | wc -l
```

Find executable location
```bash
find ~ -type f -name "*.JPG" -size +1M | wc -l
```

Execute command on found files (ie. delete all found)
```bash
find ~ -name 'foo*' -exec rm -rf {} \;
# or
find ./foo -type f -name "*.txt" | xargs rm
```

## Grep & Regex

Search through found files for a string
```bash
find . | grep 'somestring'
```

## awk & sed | piping

## Partition
