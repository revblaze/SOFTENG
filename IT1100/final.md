# 5. Commands

`which` (only works for executable programs)
`help` (works on shell commands)
`man` (Linux manual)
`info` (shorter than the manual)
`--help` (help flag)
`whatis` (1 line explanation)

Aliases
```bash
alias p='pwd ; ls -CF'
alias foo='echo hello'
```

Installing packages

```bash
apt update
apt upgrade
apt install <packagename>
apt search <packagename> #apt can be followed by -cache
```


# Chapter 6: Linux Command Line Basics

## Concatenate Files
`cat` is used to display file contents, similar to `less`.
```bash
less /etc/passwd
cat /etc/passwd
```

## Redirection
Redirect stdout to a file using `>` and `>>`.
```bash
ls -l > file_list.txt
echo "Who was in the movie Black sheep?" >> chris_farley.txt
```

## Redirect StdErr
Redirect stderr to a file.
```bash
ls -l /var/i/dont/exist 2> bad_error
```

## Redirect Both Stdout and Stderr
Redirect both stdout and stderr.
```bash
ls -l /var &> both.txt
```

## Redirect Output to Null
Discard output.
```bash
ls -l /var/ &> /dev/null
```

## Piping
Use the pipe `|` operator to pass output between commands.
```bash
ls -l | wc -l
echo "My name is fred" | wc
cat /etc/passwd | less
```

# 7. 


# User Accounts

## The Users in a Linux System
The Users in a Linux system are stored in the `/etc/passwd` file. On a new install, many users are listed because each program gets its own user with limited permissions for protection.

### User Info /etc/passwd
The `/etc/passwd` file is colon-separated with fields for username, password placeholder (`x`), UID, GID, comment field, home directory, and default shell.

## Passwords
Passwords are stored in `/etc/shadow`, encrypted and requiring sudo to view. Each line includes username, encrypted password, date of last password change, password expiry details, and optional fields.

### Passwords and Dates
Dates in `/etc/shadow` are represented as the number of days since January 1, 1970. For example, 17224 represents January 27, 2017.
  ```bash
  sudo cat /etc/shadow | grep joe
  sudo chage -l joe
  ```

## LDAP (Lightweight Directory Access Protocol)
- LDAP servers allow centralized user login/permission information, storing all usernames and passwords in a database.
- Advantages include active login on many computers, single username and password, file/profile access on multiple computers, easier user management on networks.

### /etc/passwd with LDAP
The `/etc/passwd` file lists local users. To view all users including those on LDAP, use `getent passwd`.
  ```bash
  getent passwd
  getent passwd | grep joe
  ```

## Groups in Linux
Users are by default put into a group of the same name. Each group has a unique ID and users can belong to multiple groups.

### /etc/group File
- The `/etc/group` file lists all groups, their IDs, and members.
- In Ubuntu, the sudo group grants admin privileges. In BSD and Redhat, this is the wheel group.

### High Level vs Low Level Commands
- High-level commands like `adduser`, `deluser`, `addgroup`, and `delgroup` automate user/group management.
- Low-level commands like `useradd`, `userdel`, `groupadd`, and `groupdel` require manual options, useful in scripts.

## Adding and Removing Users and Groups
Adding a user automatically sets up their account and personal group. Removing a user can leave orphaned files.

### Working as Other Users
`sudo` executes a single command as root, while `su` switches users. `login` allows logging in as another user.

### Working as Root
The root user has complete system access. Caution should be exercised when using root privileges.

### More about Passwords
Good password practices are crucial for security. Password privacy is important, and passwords should not be shared or written down.

### File Permissions
Files are owned by users and groups, with permissions set for what actions can be performed.

### Ownership and File Permissions
The `chown` and `chgrp` commands change file ownership and group. File permissions determine what actions different user classes can perform on files.

### File Permissions Modes
Symbolic and octal modes are used to set file permissions. Special rules apply to directories and symbolic links.

### More Special Permission Rules
Root can override many permission rules. Permissions on symbolic links apply only to the link itself.

### User Mask or umask
The umask sets default permissions for newly created files. It can be adjusted using the `umask` command.

# Chapter 10: Processes

## Processes
- Modern operating systems like Linux manage multiple tasks using processes.
- `init` has a PID of 1 and is the first program that runs, launching several processes and scripts.
- Daemons are background processes without a user interface.
- Each process has a PID (Process ID) and associated UID (User ID).

### Process Related Commands
- `ps` - Lists processes for the current user on the current terminal.
- `pstree` - Shows processes in a tree structure identifying parent processes.
- `top` - Dynamically lists top running processes.
- `jobs` - Lists jobs run from the terminal.
- `bg` - Puts a program in the background.
- `fg` - Brings a program to the foreground.
- `kill` - Kills a process by PID.
- `killall` - Kills processes by name.

### Process States
- `R` for running
- `S` for sleeping (waiting for an event)
- `D` for uninterruptible sleep (waiting for I/O)
- `T` for stopped
- `Z` for zombie (terminated but not cleaned up)

### Process Key Commands
- `ctrl-c` - Kills a process.
- `ctrl-z` - Puts a process in the background.

### ps Options
- `a` - Lists all processes.
- `x` - Lists BSD-style processes.
- `u` - Lists processes for all users.
- `o` - Customizes the process list (e.g., `ps xao pid,ppid,comm`).

### Killing Processes
- `kill -9 pid` - Forcefully kills a process.
- `killall xclock` - Kills all `xclock` processes.

### System Shutdown and Restart Commands
- `shutdown -r time message` - Restarts the system.
- `shutdown -h now` - Shuts down immediately.
- `halt` - Shuts down but leaves the machine powered on.
- `reboot` - Reboots the system.

# Chapter 11: Modifying the Environment

## Commands Covered
- `printenv` - Prints environment variables.
- `set` - Shows both shell and environment variables.
- `export` - Exports environment to subsequently executed programs.
- `alias` - Creates an alias for commands.

### System Config vs Single User Config
- System configuration files are in `/etc`.
- User configuration files are in the user's home directory.

### The Environment
- The shell stores environment and shell variables, aliases, and shell functions.
- Environment variables hold system properties.

### Environment Variables
- `set` shows both shell and environment variables.
- `printenv` lists currently defined environment variables.
- `echo $PATH` - Displays the PATH variable.

### Environment Variable Types
- Common variables include `DISPLAY`, `EDITOR`, `SHELL`, `HOME`, `LANG`, `PATH`, `PS1`, `PWD`, `TERM`, `USER`.
- Use `echo $VARIABLE` to display a variable's value.

### Aliases
- View aliases with `alias` command.
- Create aliases like `alias p='pwd; ls -CF'`.

### Establishing the Environment
- Startup files define the default environment.
- Types of sessions include login shells (prompt for username and password) and non-login shells (terminal sessions in GUI).

### Startup Files for Login Shells
- `/etc/profile` - Global configuration.
- `~/.bash_profile`, `~/.bash_login`, `~/.profile` - User-specific configurations.
- `~/.bash_logout` - Commands run when logging out.

### Startup Files for Non-login Shells
- `/etc/bash.bashrc` - Global configuration.
- `~/.bashrc` - User-specific configuration.

### Changing Environment Variables
- Modify variables like `PATH=$PATH:/new/path; export PATH` in `.bashrc`.
- `source .bashrc` or logout/login to apply changes.

# Chapter 12: Text Editors

## Types of Text Editors
- GUI-based editors work only on GUI systems.
- Text-based editors work on both GUI and CLI systems.

### Learning Text-Based Editors
- Essential skills: opening, editing, saving, and exiting files.

### Nano
- Comes pre-installed with Linux.
- Basic and easy to learn.
- Common shortcuts are listed at the bottom of the screen.
- Commands:
  - Open: `nano sauce.txt`
  - Save: `Ctrl-o`
  - Exit: `Ctrl-x`

### VI / VIM
- Pre-installed with most Linux distributions.
- Mode-based: normal, insert, and visual modes.
- Commands:
  - Open: `vi sauce.txt`
  - Edit: Enter insert mode with `i`.
  - Save: `:w`
  - Exit: `:q`

### Emacs
- Not pre-installed with Linux.
- Powerful editor with text-based and GUI versions.
- Commands:
  - Open: `emacs sauce.txt`
  - Save: `Ctrl-x Ctrl-s`
  - Exit: `Ctrl-x Ctrl-c`
  - Cancel Shortcut: `Ctrl-g`
  - Undo: `Ctrl-x u`

### Gedit
- GUI-based editor, pre-installed with GUI Linux.
- Can only run on GUI systems.

### Editor Commands Format
- General format: `editor-name filename`
- Examples: `vi testing.txt`, `vim testing.txt`, `gvim testing.txt`, `nano testing.txt`, `emacs testing.txt`, `gedit testing.txt`

# Chapter 13: Customizing the Prompt

## Displaying the Prompt
- Command: `echo $PS1`
- Common escape codes:
  - `\d`: current date
  - `\h`: hostname
  - `\u`: username
  - `\w`: current working directory

### Backing Up the Current Prompt
- Create a new variable and copy the `PS1` variable: `ps1_old="$PS1"`

### Creating a New Prompt
- Example: `PS1=\u@\h:\w\$`
- Using an alias for prompt change: `alias prompt1="PS1='\u@\h:\w\$'"`

### Adding Color to Prompts
- Refer to color schemes and escape codes (e.g., `\033[1;32m` for green).
- Shortcut: use `\e` instead of `\033`.
- Example: `PS1='\[\e[1;32m\][\u@\h \W]\$\[\e[0m\]'`

# Chapter 14: Network Settings

## Network Configuration
- Each machine on a network must have a unique IP address.
- DHCP (Dynamic Host Configuration Protocol) dynamically assigns IPs, possibly changing each time the computer is turned on.
- Static IP configuration is manual, keeping the IP address constant.

### Netmask
- Determines permissions within the network.

### Gateway
- Provides access from the internal network to the outside world.

### Domain Name Servers (DNS)
- Translates domain names to IP addresses.
- DNS servers have a hierarchical structure for resolving names.

## Managing Network Settings
- View current configuration: `ip a` or `ip addr show`.
- Change IPv4 address: Edit the YAML file in `/etc/netplan`.

  ```yaml
  network:
    ethernets:
        ens4:
            addresses: [144.38.218.131/29]
            gateway4: 144.38.218.129
            nameservers:
                search: [cs.dixie.edu]
                addresses: ["144.38.192.2", "144.38.192.3"]
  version: 2
  ```

- Restart the network interface: `sudo netplan apply` or `ip link set ens4 down; ip link set ens4 up`.

## Package Management
- Install packages using `apt` or `yum`.
- Search for software: `apt-cache search <name>`.
- Update list of available programs: `apt-get update`.
- Install a program: `apt-get install <name>`.
- Upgrade a program: `apt-get upgrade <name>`.

### Repositories
- Repositories store software and updates.
- Ubuntu repository components include Main, Restricted, Universe, and Multiverse.
- Repository URLs are stored at `/etc/apt/sources.list`.

# Chapter 15: Disk Partitions and Inodes

## What is a Partition?
- A logical division of a hard drive for data management and organization.
- Reasons for partitioning include dual-boot setups and data encapsulation.

## Linux Partitions
- Minimum requirement: one partition for root (`/`).
- Swap space: used when RAM is full, with a size generally double the RAM.

### Mounting
- Connecting a filesystem/partition to a point in the root filesystem.
- Device naming conventions, e.g., `/dev/sda1`, `/dev/sdb7`.

### Partitioning Tools
- Partition free space using tools like `cfdisk`.
- Format partitions with `mkfs`.
- Create and mount partitions: `mkdir <mount_point>` and `mount <device> <mount_point>`.

### Persistent Mounts
- To make mounts persist, add entries in `/etc/fstab`.

### MSDOS and GPT
- MSDOS: Legacy system with up to 4 primary partitions.
- GPT: Supports large disks, unlimited partitions, and stores data structures twice for recovery.

## Inodes
- Inodes contain metadata about files, excluding filenames.
- Filesystem consists of inodes with unique numbers for each file/directory.
- Operations like renaming or moving files within the same partition don't change inode numbers.

### Viewing Inodes
- `ls -lai` to view inode numbers.
- Inodes are unique within a partition.
- Hard links share inode numbers, while symbolic links have separate inodes.

### Renaming and Moving Files
- Inode numbers remain constant during renaming/moving within the same partition.
- Hard links remain linked; symbolic links may break.
- Filename is stored in the metadata of the directory, not in the inode.

### Inode Metadata
- Hard link count indicates the number of hard links to a file.
- Directories typically have at least 2 hard links (`.` and `..`).
- The inode count can provide insights into the directory structure and file links.


# Chapter 16: Remote Connectivity

## Remote Connectivity Methods
- **wget**: Download content from a website.
  - Example: `wget http://cit.dixie.edu/it/1100/syllabus.pdf`
- **ssh**: Terminal connection to another computer.
  - Example: `ssh smorgan@ssh.cs.dixie.edu`

### VNC Viewer
- Create a visual connection to another computer.
- Tools: GTK VncViewer (Linux), Chicken (Mac), TightVNC (Windows, Mac, Linux).

### SCP and FTP
- Secure file transfer to a remote destination.
- **scp**: Secure copy protocol for transferring files.
- **ftp/sftp**: Interactive file transfer protocol.

### GUI Programs for File Transfer
- Common tools: Cyberduck (Mac, Windows), WinSCP (Windows), Filezilla (Linux, Mac, Windows).
- Access CIT accounts and Scratch for file transfers.

## Network Review
- **Computer Network**: A group of computers with permissions to communicate.
- CIT network vs DSU network.
- Firewalls protect networks from unauthorized access.
- ssh.cs.dixie.edu acts as a firewall for the CIT network.

### Network Related Commands
- **ping**: Examine and monitor network connectivity.
- **traceroute**: Display the route packets take through a network.
- **ip a / ifconfig**: Show IP address.
- **netstat**: Display open ports and networking information.

### Ports
- Communication through specific ports (e.g., port 22 for SSH).
- Selection of ports between 1024 and 65535 for user use.

## Creating a Tunnel
- Format: `ssh <username>@ssh.cs.dixie.edu -L <receivingport>:<destination machine:port>`
- Example for VNC: `ssh <cit-username>@ssh.cs.dixie.edu -L 8467:desdemona.cs.dixie.edu:8467`
- Example for SFTP: `ssh <cit-username>@ssh.cs.dixie.edu -L 8467:scratch.cs.dixie.edu:22`

### SCP Command Line Connections
- SCP securely copies files between machines.
- Example to copy from localhost to remote: `scp filename destination-machine:./new_filename`
- Example to copy from remote to localhost: `scp source-machine:./filename ./new_filename`

## SSH Basic Connection Shortcuts
- Direct connections within and outside the network.
- Examples: `ssh joe@scratch.cs.dixie.edu`, `ssh scratch`, `ssh ssh.cs.dixie.edu`

## Accessing Remote Machines
- **Keys**: Establish trust between computers for secure connections.
- **.ssh directory**: Contains known_hosts, id_rsa, id_rsa.pub, authorized_keys.

### SSH Key Management
- **ssh-keygen**: Generate a public/private key pair.
- **ssh-copy-id**: Copy public key to a remote machine for password-less login.
- Example: `ssh-copy-id smorgan@scratch.cs.dixie.edu`

### Important Considerations
- Be cautious with key generation and management.
- Authorized keys enable seamless access to remote machines.









 ##################


# Chapter 17 Searching for Files
Find, Grep, Regex, awk, sed, partition

## Find
https://computing.utahtech.edu/it/1100/slides/slides17.php

In root `/`, find `filename` (and hide permission errors)
```bash
find / -type f -name "filename" 2> /dev/null
```

In root `/`, find `/some/directory` (and hide permission errors)
```bash
find / -type d -name "/some/directory" 2> /dev/null
```

In home `~`, find JPG files that are larger than 1Mb | count the amount of files
```bash
find ~ -type f -name "*.JPG" -size +1M | wc -l
```

In home `~`, find all files made within the last 10 minutes
```bash
find ~ -type f -name "*" -mmin -10

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

Search through files of current directory `.` (recursively) for a string (ie. `hello`), then output the amount of times it occurs:
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

Remove all instances of the substring `rem`:
```bash
sed 's/rem//g' myfile.txt
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

