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

Find executable location (ie. `cd`)
```bash
which cd
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


## inodes

https://computing.utahtech.edu/it/1100/slides/slides15.php

List inode number and the metadata of files in root
```bash
ls -lai /
```

Additional metadata of file using stat
```bash
stat file.txt
```

Show many inodes we have and how many are available to us
```bash
df -i
```

## Partition

Naming conventions; each physical device can be partitioned:
- `/dev/sda1` # first partition on first drive of type scsi
- `/dev/sdb7` # seventh partition on second drive of type scsi

### Steps to partition - [screenshot help](https://utahtech.instructure.com/courses/897667/files/154748053?module_item_id=22616069)
1. `sudo cfdisk`
2. Select 'Free space'
3. Enter amount for 'Partition size'
4. Select `[ Write ]`, type 'yes'

### Formatting
1. `sudo cfdisk`
2. Note 'Device' path (ie. `/dev/sda5`)
3. `sudo mkfs.ext4 /dev/sda5`


### Mounting
1. `cd /mnt`
2. `sudo mkdir some-name`
3. `sudo mount /dev/sda5 /mnt/some-name`
4.  Check by using command `mount` after

### Mounting Persistently
5. `sudo nano /etc/fstab`
6. Add the following per mount:

```bash
/dev/sda5 /mnt/some-name-5 ext4 defaults 0 0      # mkfs.ext4
/dev/sda6 /mnt/some-name-6 ext3 defaults 0 0      # mkfs.ext3
```

### Testing (unmount, mount all)
1. `sudo umount /dev/sda5`
2. `sudo mount -a`


# Practice Exam

## Part 1: Grep
Using the file `/usr/share/dict/words`:

1. Locate all words that contain "carp", print the number of those words occuring:

```bash
cat /usr/share/dict/words | grep -e "*carp*" | wc -l
```

2. Locate all words starting with `f`, `i`, `s` or `h`:
```bash
cat /usr/share/dict/words | grep -e "^[fish]" | wc -l
```

3. Locate all words that end in `shy`, output to `shy.txt`:
```bash
cat /usr/share/dict/words | grep -e "shy$" > shy.txt
```

4. Use regular expressions to locate all words that contain `fish` but does not end with `s`.
```bash
cat /usr/share/dict/words | grep -e "fish" | grep -e "[^s]$"
```

5. Use regular expressions to locate all words that begin with `un` and end with `ly`
```bash
cat /usr/share/dict/words | grep -e "^un" | grep -e "ly$"
```

## Part 2: awk/sed
1. Print lines 163-171, but only columns 1 and 4, each column separated with a space
```bash
cat Fish_Stocking_Report_2014.csv | sed -n '163,171p' | awk -F "," '{print $1 " " $4}'
```

2. Print lines 436,446,456,466,476,486; Print the sum of column 4 and 5:
```bash
cat Fish_Stocking_Report_2014.csv | sed -n '436~10p' | sed -n '1,6p' | awk -F ',' '{print $4+$5}'
```

3. Print every other line from 637-669. Include columns 3, 4 and 5. Separate 3 and 4 with a space.
Preface column 5 with the would `" Average Length: "`.
```bash
cat Fish_Stocking_Report_2014.csv | sed -n '637,669p' | sed -n '1~2p' | awk -F "," '{print $3 " " $4 " Average Length: " $5}'
```

