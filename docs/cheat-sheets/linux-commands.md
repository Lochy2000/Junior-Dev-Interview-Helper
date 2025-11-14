# Linux Command Line Cheat Sheet

> Essential terminal commands for developers

## 📋 Table of Contents

- [Navigation](#navigation)
- [File Operations](#file-operations)
- [File Viewing](#file-viewing)
- [Permissions](#permissions)
- [Search & Find](#search--find)
- [Process Management](#process-management)
- [Network](#network)
- [System Info](#system-info)
- [Shortcuts](#shortcuts)

---

## Navigation

```bash
# Print working directory
pwd

# List files
ls                    # Basic list
ls -l                 # Long format (permissions, size, date)
ls -la                # Include hidden files
ls -lh                # Human-readable sizes
ls -lt                # Sort by modification time
ls -lS                # Sort by size

# Change directory
cd /path/to/directory # Absolute path
cd ~/Documents        # Home directory
cd ..                 # Parent directory
cd -                  # Previous directory
cd                    # Home directory

# Create directory
mkdir folder_name
mkdir -p path/to/nested/folders  # Create parent directories

# Remove directory
rmdir folder_name                # Empty directory only
rm -r folder_name                # Recursive (with contents)
rm -rf folder_name               # Force recursive (⚠️ dangerous!)
```

---

## File Operations

```bash
# Create empty file
touch file.txt

# Create file with content
echo "Hello World" > file.txt      # Overwrite
echo "More text" >> file.txt       # Append

# Copy files
cp source.txt destination.txt
cp -r source_folder dest_folder    # Recursive (folders)
cp -i file.txt dest/               # Interactive (confirm overwrite)

# Move/Rename
mv old_name.txt new_name.txt       # Rename
mv file.txt /path/to/destination/  # Move
mv *.txt documents/                # Move multiple files

# Delete files
rm file.txt
rm -i file.txt                     # Interactive (confirm)
rm -f file.txt                     # Force
rm *.txt                           # Delete all .txt files

# Create symbolic link
ln -s /path/to/original link_name

# Find file type
file document.pdf
```

---

## File Viewing

```bash
# View entire file
cat file.txt                       # Print to terminal

# View file page by page
less file.txt                      # Navigate with arrows, q to quit
more file.txt                      # Similar to less

# View first/last lines
head file.txt                      # First 10 lines
head -n 20 file.txt                # First 20 lines
tail file.txt                      # Last 10 lines
tail -n 20 file.txt                # Last 20 lines
tail -f log.txt                    # Follow (live updates)

# View with line numbers
cat -n file.txt
nl file.txt

# Count lines, words, characters
wc file.txt                        # All
wc -l file.txt                     # Lines only
wc -w file.txt                     # Words only
wc -c file.txt                     # Characters only
```

---

## Permissions

```bash
# View permissions
ls -l file.txt
# -rw-r--r-- 1 user group 1234 Jan 1 12:00 file.txt
# - = file, d = directory
# rw- = owner (read, write)
# r-- = group (read only)
# r-- = others (read only)

# Change permissions (symbolic)
chmod u+x file.txt                 # Add execute for owner
chmod g-w file.txt                 # Remove write for group
chmod o+r file.txt                 # Add read for others
chmod a+x file.txt                 # Add execute for all

# Change permissions (octal)
chmod 755 file.txt                 # rwxr-xr-x
chmod 644 file.txt                 # rw-r--r--
chmod 600 file.txt                 # rw-------
chmod 777 file.txt                 # rwxrwxrwx (⚠️ dangerous!)

# Change owner
sudo chown user:group file.txt
sudo chown user file.txt
sudo chown -R user:group folder/   # Recursive

# Make script executable
chmod +x script.sh
./script.sh                        # Run script
```

**Permission Numbers:**
- 4 = read (r)
- 2 = write (w)
- 1 = execute (x)
- 755 = 7(rwx) 5(r-x) 5(r-x)

---

## Search & Find

```bash
# Find files by name
find . -name "*.txt"               # Current directory
find /path -name "file.txt"        # Specific path
find . -iname "*.TXT"              # Case-insensitive

# Find files by type
find . -type f                     # Files only
find . -type d                     # Directories only

# Find by size
find . -size +10M                  # Larger than 10MB
find . -size -1k                   # Smaller than 1KB

# Find by modification time
find . -mtime -7                   # Modified in last 7 days
find . -mtime +30                  # Modified more than 30 days ago

# Find and execute command
find . -name "*.log" -delete       # Find and delete
find . -name "*.txt" -exec cat {} \;  # Find and cat

# Search file contents
grep "search_term" file.txt        # Search in file
grep -r "search_term" .            # Recursive search
grep -i "search_term" file.txt     # Case-insensitive
grep -n "search_term" file.txt     # Show line numbers
grep -v "search_term" file.txt     # Invert match (exclude)
grep -c "search_term" file.txt     # Count matches

# Locate (faster than find, uses database)
locate file.txt
sudo updatedb                      # Update locate database
```

---

## Process Management

```bash
# List running processes
ps                                 # Current user processes
ps aux                             # All processes (detailed)
ps aux | grep python               # Find Python processes

# Real-time process viewer
top                                # Interactive view (q to quit)
htop                               # Better interface (if installed)

# Kill process
kill PID                           # Graceful stop (SIGTERM)
kill -9 PID                        # Force stop (SIGKILL)
killall process_name               # Kill by name

# Find process ID
pgrep process_name
pidof process_name

# Background/Foreground
command &                          # Run in background
jobs                               # List background jobs
fg %1                              # Bring job 1 to foreground
bg %1                              # Resume job 1 in background

# Run command immune to hangups
nohup command &                    # Continue after logout
nohup python script.py > output.log 2>&1 &

# Resource usage
free -h                            # Memory usage
df -h                              # Disk usage
du -sh folder/                     # Folder size
du -sh *                           # Size of each item
```

---

## Network

```bash
# Check connectivity
ping google.com                    # Test connection
ping -c 4 google.com               # Ping 4 times

# Download file
curl -O https://example.com/file.txt
wget https://example.com/file.txt

# HTTP request
curl https://api.example.com       # GET request
curl -X POST https://api.example.com -d "data"

# Network info
ifconfig                           # Network interfaces (old)
ip addr                            # Network interfaces (new)
ip a                               # Short form

# DNS lookup
nslookup google.com
dig google.com

# Network connections
netstat -tuln                      # All listening ports
ss -tuln                           # Socket statistics (newer)
lsof -i :8000                      # What's using port 8000

# SSH
ssh user@hostname                  # Connect to remote server
ssh -p 2222 user@hostname          # Custom port
scp file.txt user@host:/path       # Copy file to remote
scp user@host:/path/file.txt .     # Copy from remote

# Transfer files
rsync -avz source/ dest/           # Sync directories
rsync -avz -e ssh source/ user@host:dest/  # Over SSH
```

---

## System Info

```bash
# System information
uname -a                           # All system info
uname -r                           # Kernel version
hostname                           # Computer name
whoami                             # Current user
id                                 # User ID and groups

# Disk usage
df -h                              # Disk space (human-readable)
df -i                              # Inode usage

# Memory
free -h                            # RAM usage
cat /proc/meminfo                  # Detailed memory info

# CPU
lscpu                              # CPU info
cat /proc/cpuinfo                  # Detailed CPU info

# Uptime
uptime                             # How long system running
w                                  # Who's logged in and uptime

# Date and time
date                               # Current date/time
date "+%Y-%m-%d %H:%M:%S"          # Custom format
cal                                # Calendar

# Environment variables
env                                # All variables
echo $PATH                         # Specific variable
export VAR=value                   # Set variable
```

---

## Compression

```bash
# Tar (archive)
tar -cvf archive.tar files/        # Create tar
tar -xvf archive.tar               # Extract tar
tar -tvf archive.tar               # List contents

# Tar + Gzip (.tar.gz or .tgz)
tar -czvf archive.tar.gz files/    # Create
tar -xzvf archive.tar.gz           # Extract
tar -tzvf archive.tar.gz           # List contents

# Zip
zip archive.zip file1 file2        # Create zip
zip -r archive.zip folder/         # Recursive
unzip archive.zip                  # Extract
unzip -l archive.zip               # List contents

# Gzip (single file)
gzip file.txt                      # Compress (creates file.txt.gz)
gunzip file.txt.gz                 # Decompress
```

---

## Text Processing

```bash
# Sort
sort file.txt                      # Alphabetically
sort -n file.txt                   # Numerically
sort -r file.txt                   # Reverse
sort -u file.txt                   # Unique only

# Unique lines
uniq file.txt                      # Remove adjacent duplicates
sort file.txt | uniq               # Remove all duplicates
sort file.txt | uniq -c            # Count occurrences

# Cut columns
cut -d',' -f1 file.csv             # First column (comma delimiter)
cut -d':' -f1,3 /etc/passwd        # Columns 1 and 3

# Stream editor
sed 's/old/new/' file.txt          # Replace first occurrence per line
sed 's/old/new/g' file.txt         # Replace all occurrences
sed -i 's/old/new/g' file.txt      # Edit file in place

# Awk
awk '{print $1}' file.txt          # Print first column
awk -F',' '{print $1,$3}' file.csv # Custom delimiter
```

---

## Shortcuts

```bash
# Command Line Shortcuts
Ctrl + C                           # Kill current process
Ctrl + Z                           # Suspend current process
Ctrl + D                           # Exit shell (EOF)
Ctrl + L                           # Clear screen
Ctrl + A                           # Move to start of line
Ctrl + E                           # Move to end of line
Ctrl + U                           # Delete from cursor to start
Ctrl + K                           # Delete from cursor to end
Ctrl + W                           # Delete word before cursor
Ctrl + R                           # Search command history
Ctrl + P                           # Previous command
Ctrl + N                           # Next command

# History
history                            # Show command history
!123                               # Run command 123 from history
!!                                 # Run last command
!$                                 # Last argument of previous command
!*                                 # All arguments of previous command

# Other useful
clear                              # Clear screen
exit                               # Exit shell
man command                        # Manual for command
command --help                     # Help for command
which command                      # Location of command
alias ll='ls -la'                  # Create alias
source ~/.bashrc                   # Reload config
```

---

## Pipes & Redirection

```bash
# Pipe (pass output to next command)
ls -la | grep ".txt"               # Find txt files
cat file.txt | wc -l               # Count lines
ps aux | grep python | awk '{print $2}'

# Redirect output
command > file.txt                 # Overwrite file
command >> file.txt                # Append to file
command 2> error.log               # Redirect errors
command > output.txt 2>&1          # Redirect both output and errors

# Redirect input
command < input.txt                # Read from file

# Discard output
command > /dev/null                # Discard output
command 2> /dev/null               # Discard errors
command > /dev/null 2>&1           # Discard both
```

---

## Tips & Tricks

```bash
# Run multiple commands
command1 && command2               # Run command2 if command1 succeeds
command1 || command2               # Run command2 if command1 fails
command1; command2                 # Run both regardless

# Command substitution
echo "Today is $(date)"
echo "Files: $(ls | wc -l)"

# Wildcards
*                                  # Any characters
?                                  # Single character
[abc]                              # a, b, or c
[a-z]                              # Any lowercase letter

# Examples
ls *.txt                           # All txt files
ls file?.txt                       # file1.txt, fileA.txt, etc.
ls file[12].txt                    # file1.txt, file2.txt
```

---

**Keywords**: Linux commands, terminal commands, bash commands, command line, shell commands, Linux cheat sheet, Unix commands, CLI reference, Linux for developers
