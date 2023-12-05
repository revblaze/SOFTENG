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

Batch rename found files, appending `.FOUND` to the end
```bash
find ./foo -type f -name "*fish*.txt" | xargs -I {} sh -c 'mv "$1" "$1.FOUND"' _ {}
```

## Grep & Regex

https://computing.utahtech.edu/it/1100/slides/slides19.php

Search through found files for a string, then output the amount of times it occurs
```bash
find . | grep 'hello' | wc -l
```

Regex rules
```
Text:
      .           Any single character
      [chars]     Character class: Any character of the class ``chars''
      [^chars]    Character class: Not a character of the class ``chars''
      text1|text2 Alternative: text1 or text2

Quantifiers:
      ?           0 or 1 occurrences of the preceding text
      *           0 or N occurrences of the preceding text (N > 0)
      +           1 or N occurrences of the preceding text (N > 1)

Grouping:
      (text)      Grouping of text (used either to set the borders of an alternative as above, or to make backreferences, where the Nth group can be referred to on the RHS of a RewriteRule as $N)

Anchors:
      ^           Start-of-line anchor
      $           End-of-line anchor
```

## sed

Replace all instances of `day` with `night` inside `myfile.txt`
```bash
sed 's/day/night/g' myfile.txt
```

Do not print first line
```bash
sed '1d' file.txt
```

Remove the first character of every line
```bash
sed 's/^.//' file
```

Remove the last character of every line
```bash
sed ’s/.$//’ file
```

Remove lines 7 thru 9 of a file
```bash
sed ‘7,9d’ filename.txt
```

Remove the line containing the string Fred
```bash
sed '/Fred/d' filename.txt
```

Print every `other line of the file
```bash
sed -n '1p' file.txt
```

Print every 5th line, starting with the 2nd line (will print lines 2, 7, 12, 17, etc)
```bash
sed -n '2~5p' file.txt
```

Print lines 2 through 4
```bash
sed -n '2,4p' file.txt
```

## awk
Print the 1st and 3rd columns in the output of a command
```bash
ls -l | awk '{ print $1 $3 }'
```

Print the 1st column in the output of a command and add text
```bash
ps aux | awk '{ print "this process is owned by " $1 }'
```

Print 1st column of values in file separated by `':'`
```bash
awk -F':' '{ print $1 }' /etc/passwd
```

Using regex, search for lines that start with `UUID` and print the 3rd column of results
```bash
awk '/^UUID/ {print $3}' /etc/fstab
```

Add column 1+2+3 from the `grades` file and print `"the average is [sum here]"`
```bash
awk '{ print "the average is ", ($1+$2+$3)/3 }' grades
```


## piping

Remove the last character of every line

## Partition
