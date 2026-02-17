# 🐧 Linux Essential Commands: Beginner to Intermediate Guide

> A clean, structured reference handbook for learning Linux commands step by step.

---

## Table of Contents

1. [File & Directory Management](#1-file--directory-management)
2. [File Viewing & Editing](#2-file-viewing--editing)
3. [File Searching](#3-file-searching)
4. [Permissions & Ownership](#4-permissions--ownership)
5. [Process Management](#5-process-management)
6. [Networking](#6-networking)
7. [Package Management](#7-package-management)
8. [Disk Management](#8-disk-management)
9. [User Management](#9-user-management)
10. [Archiving & Compression](#10-archiving--compression)
11. [System Information & Control](#11-system-information--control)
12. [Redirects, Pipes & Useful Tricks](#12-redirects-pipes--useful-tricks)

---

## 1. File & Directory Management

---

### `pwd` — Print Working Directory

```bash
pwd
```

Displays the full path of the current directory you are in.

```bash
$ pwd
/home/username/Documents
```

---

### `ls` — List Directory Contents

```bash
ls [options] [directory]
```

Lists files and folders in the current (or specified) directory.

```bash
ls              # basic listing
ls -la          # detailed list including hidden files
```

---

### `cd` — Change Directory

```bash
cd [directory]
```

Navigates to a specified directory.

```bash
cd /home/username/Documents   # go to a specific path
cd ..                         # go one level up
cd ~                          # go to home directory
```

---

### `mkdir` — Make Directory

```bash
mkdir [options] directory_name
```

Creates a new directory.

```bash
mkdir projects                    # create a single folder
mkdir -p projects/web/css         # create nested folders at once
```

---

### `rmdir` — Remove Empty Directory

```bash
rmdir directory_name
```

Deletes an empty directory.

```bash
rmdir old_folder
```

---

### `touch` — Create an Empty File

```bash
touch filename
```

Creates a new empty file, or updates the timestamp of an existing one.

```bash
touch notes.txt
touch file1.txt file2.txt   # create multiple files at once
```

---

### `cp` — Copy Files or Directories

```bash
cp [options] source destination
```

Copies a file or directory from one location to another.

```bash
cp report.txt backup/report.txt       # copy a file
cp -r projects/ projects_backup/      # copy a directory recursively
```

---

### `mv` — Move or Rename Files

```bash
mv [options] source destination
```

Moves a file/directory to a new location, or renames it.

```bash
mv old_name.txt new_name.txt          # rename a file
mv report.txt /home/username/docs/    # move a file to another directory
```

---

### `rm` — Remove Files or Directories

```bash
rm [options] file
```

Deletes files or directories. **Use with caution — this is irreversible.**

```bash
rm unwanted.txt               # delete a file
rm -rf old_project/           # forcefully delete a directory and its contents
```

---

### `ln` — Create Links

```bash
ln -s target link_name
```

Creates a symbolic (soft) link to a file or directory.

```bash
ln -s /usr/local/bin/python3 ~/mypy    # create a shortcut to python3
```

---

## 2. File Viewing & Editing

---

### `cat` — Concatenate and Display File Contents

```bash
cat [options] filename
```

Prints the entire content of a file to the terminal.

```bash
cat notes.txt
cat file1.txt file2.txt     # display multiple files in sequence
```

---

### `less` — View File Content Page by Page

```bash
less filename
```

Opens a file for scrollable reading. Press `q` to quit, arrow keys to navigate.

```bash
less /var/log/syslog
```

---

### `head` — Show First Lines of a File

```bash
head [options] filename
```

Displays the first 10 lines of a file by default.

```bash
head access.log               # first 10 lines
head -n 20 access.log         # first 20 lines
```

---

### `tail` — Show Last Lines of a File

```bash
tail [options] filename
```

Displays the last 10 lines of a file. Useful for monitoring logs.

```bash
tail error.log                # last 10 lines
tail -f /var/log/syslog       # follow live log updates in real time
```

---

### `nano` — Simple Terminal Text Editor

```bash
nano filename
```

Opens a beginner-friendly terminal editor. Use `Ctrl+O` to save, `Ctrl+X` to exit.

```bash
nano config.txt
```

---

### `vim` — Advanced Terminal Text Editor

```bash
vim filename
```

A powerful editor. Press `i` to enter insert mode, `Esc` then `:wq` to save and quit.

```bash
vim script.sh
```

---

### `wc` — Word Count

```bash
wc [options] filename
```

Counts lines, words, and characters in a file.

```bash
wc -l report.txt      # count lines only
wc -w essay.txt       # count words only
```

---

## 3. File Searching

---

### `find` — Search for Files and Directories

```bash
find [path] [options] [expression]
```

Searches for files and directories based on name, type, size, and more.

```bash
find /home -name "*.txt"              # find all .txt files in /home
find . -type d -name "config"         # find directories named "config"
```

---

### `grep` — Search Text Inside Files

```bash
grep [options] pattern [file]
```

Searches for a specific pattern (text or regex) within files.

```bash
grep "error" server.log               # find lines containing "error"
grep -ri "todo" ~/projects/           # case-insensitive recursive search
```

---

### `locate` — Quickly Find Files by Name

```bash
locate filename
```

Searches a pre-built database for files (much faster than `find`). Run `sudo updatedb` to refresh the database.

```bash
locate nginx.conf
```

---

### `which` — Find Command Location

```bash
which command_name
```

Shows the full path of a command's executable.

```bash
which python3
which git
```

---

## 4. Permissions & Ownership

---

### `chmod` — Change File Permissions

```bash
chmod [options] mode file
```

Changes read/write/execute permissions for a file or directory.

```bash
chmod 755 script.sh           # rwx for owner, rx for group & others
chmod +x deploy.sh            # add execute permission for everyone
```

**Permission reference:**

| Number | Permission |
|--------|------------|
| 4      | Read (r)   |
| 2      | Write (w)  |
| 1      | Execute (x)|

---

### `chown` — Change File Owner

```bash
chown [options] owner[:group] file
```

Changes the owner (and optionally the group) of a file or directory.

```bash
chown alice report.txt                # change owner to alice
chown alice:developers project/       # change owner and group
```

---

### `chgrp` — Change Group Ownership

```bash
chgrp group_name file
```

Changes only the group ownership of a file.

```bash
chgrp developers app.py
```

---

### `umask` — Set Default Permission Mask

```bash
umask [mask]
```

Displays or sets the default permission mask applied when new files are created.

```bash
umask          # show current mask
umask 022      # set default: files get 644, dirs get 755
```

---

## 5. Process Management

---

### `ps` — Display Running Processes

```bash
ps [options]
```

Shows a snapshot of currently running processes.

```bash
ps aux                        # show all running processes with details
ps aux | grep nginx           # find a specific process
```

---

### `top` — Real-Time Process Viewer

```bash
top
```

Displays a live, updating view of running processes and system resource usage. Press `q` to quit.

```bash
top
```

---

### `htop` — Enhanced Interactive Process Viewer

```bash
htop
```

A more user-friendly, colorful alternative to `top` (may need to be installed).

```bash
htop
```

---

### `kill` — Terminate a Process by PID

```bash
kill [signal] PID
```

Sends a signal to a process, usually to terminate it.

```bash
kill 1234           # gracefully stop process with PID 1234
kill -9 1234        # forcefully kill the process
```

---

### `killall` — Kill Processes by Name

```bash
killall process_name
```

Kills all running processes with the specified name.

```bash
killall firefox
killall -9 nginx
```

---

### `bg` / `fg` — Background and Foreground Jobs

```bash
bg [job_id]
fg [job_id]
```

`bg` resumes a suspended process in the background; `fg` brings it to the foreground.

```bash
bg %1       # resume job 1 in the background
fg %1       # bring job 1 to the foreground
```

---

### `jobs` — List Background Jobs

```bash
jobs
```

Lists all jobs running in the background of the current shell session.

```bash
jobs
```

---

### `nohup` — Run a Command That Survives Logout

```bash
nohup command &
```

Runs a command immune to hangups, keeping it running even after you log out.

```bash
nohup python3 server.py &
```

---

## 6. Networking

---

### `ping` — Test Network Connectivity

```bash
ping [options] host
```

Sends ICMP packets to a host to check if it's reachable.

```bash
ping google.com
ping -c 4 192.168.1.1     # send exactly 4 packets
```

---

### `curl` — Transfer Data from URLs

```bash
curl [options] URL
```

Fetches content from a URL. Useful for testing APIs and downloading files.

```bash
curl https://example.com
curl -O https://example.com/file.zip     # download a file
```

---

### `wget` — Download Files from the Web

```bash
wget [options] URL
```

Downloads files from the internet directly to your machine.

```bash
wget https://example.com/archive.tar.gz
wget -c https://example.com/bigfile.zip   # resume a partial download
```

---

### `ifconfig` / `ip` — Network Interface Configuration

```bash
ifconfig
ip addr show
```

Displays or configures network interface settings (IP addresses, etc.).

```bash
ifconfig                  # show all network interfaces (older systems)
ip addr show              # modern equivalent
```

---

### `netstat` — Network Statistics

```bash
netstat [options]
```

Displays active network connections, ports, and routing tables.

```bash
netstat -tuln             # show listening TCP/UDP ports
netstat -an | grep :80    # check if port 80 is in use
```

---

### `ssh` — Secure Shell Remote Login

```bash
ssh [options] user@host
```

Connects to a remote machine securely over the network.

```bash
ssh alice@192.168.1.10
ssh -p 2222 alice@server.example.com    # connect on a custom port
```

---

### `scp` — Secure Copy Over SSH

```bash
scp source user@host:destination
```

Copies files between local and remote machines securely.

```bash
scp report.txt alice@192.168.1.10:/home/alice/
scp alice@192.168.1.10:/var/log/app.log ./local/   # download from remote
```

---

### `nslookup` / `dig` — DNS Lookup

```bash
nslookup domain
dig domain
```

Queries DNS to resolve a domain name to an IP address.

```bash
nslookup google.com
dig openai.com
```

---

## 7. Package Management

---

### Debian / Ubuntu — `apt`

```bash
sudo apt update              # refresh package list
sudo apt upgrade             # upgrade all installed packages
sudo apt install package     # install a package
sudo apt remove package      # uninstall a package
sudo apt search keyword      # search for a package
```

**Examples:**

```bash
sudo apt update && sudo apt upgrade
sudo apt install git curl vim
```

---

### Red Hat / CentOS / Fedora — `dnf` / `yum`

```bash
sudo dnf update              # update all packages
sudo dnf install package     # install a package
sudo dnf remove package      # remove a package
sudo dnf search keyword      # search packages
```

**Examples:**

```bash
sudo dnf install nginx
sudo dnf remove httpd
```

---

### `snap` — Snap Package Manager

```bash
snap install package_name
snap remove package_name
snap list
```

Installs sandboxed, self-contained applications.

```bash
sudo snap install code --classic     # install VS Code
snap list                            # list installed snaps
```

---

## 8. Disk Management

---

### `df` — Disk Free Space

```bash
df [options]
```

Shows disk space usage for all mounted filesystems.

```bash
df -h         # human-readable sizes (KB, MB, GB)
df -h /home   # check a specific partition
```

---

### `du` — Disk Usage of Files/Directories

```bash
du [options] [path]
```

Shows how much disk space files or directories are using.

```bash
du -sh ~/Documents          # total size of Documents folder
du -sh * | sort -h          # list sizes sorted smallest to largest
```

---

### `lsblk` — List Block Devices

```bash
lsblk
```

Displays a tree of all storage devices and their partitions.

```bash
lsblk
lsblk -f      # also show filesystem types and mount points
```

---

### `mount` / `umount` — Mount and Unmount Filesystems

```bash
mount device mountpoint
umount mountpoint
```

Attaches or detaches a storage device to the filesystem tree.

```bash
sudo mount /dev/sdb1 /mnt/usb
sudo umount /mnt/usb
```

---

### `fdisk` — Partition Manager

```bash
sudo fdisk -l
```

Lists all disk partitions. Use interactively to create/delete partitions.

```bash
sudo fdisk -l                  # list all partitions
sudo fdisk /dev/sdb            # open partition editor for /dev/sdb
```

---

## 9. User Management

---

### `whoami` — Show Current User

```bash
whoami
```

Prints the username of the currently logged-in user.

```bash
$ whoami
alice
```

---

### `who` — Show Logged-In Users

```bash
who
```

Lists all users currently logged into the system.

```bash
who
```

---

### `id` — Show User and Group IDs

```bash
id [username]
```

Displays the UID, GID, and group memberships of a user.

```bash
id
id alice
```

---

### `useradd` — Add a New User

```bash
sudo useradd [options] username
```

Creates a new user account.

```bash
sudo useradd -m bob           # create user with a home directory
sudo useradd -m -s /bin/bash carol
```

---

### `passwd` — Change User Password

```bash
passwd [username]
```

Sets or changes a user's password.

```bash
passwd                  # change your own password
sudo passwd bob         # change another user's password (as root)
```

---

### `userdel` — Delete a User

```bash
sudo userdel [options] username
```

Removes a user account from the system.

```bash
sudo userdel bob               # remove user only
sudo userdel -r bob            # remove user and their home directory
```

---

### `usermod` — Modify a User Account

```bash
sudo usermod [options] username
```

Modifies an existing user's properties.

```bash
sudo usermod -aG sudo alice         # add alice to the sudo group
sudo usermod -l newname oldname     # rename a user
```

---

### `groups` — Show Group Memberships

```bash
groups [username]
```

Lists all groups a user belongs to.

```bash
groups
groups alice
```

---

### `sudo` — Execute Command as Superuser

```bash
sudo command
```

Runs a command with administrator (root) privileges.

```bash
sudo apt update
sudo nano /etc/hosts
```

---

## 10. Archiving & Compression

---

### `tar` — Archive Files

```bash
tar [options] archive_name files
```

Creates or extracts `.tar` archives. Commonly combined with compression flags.

```bash
tar -czvf archive.tar.gz folder/    # create a compressed archive
tar -xzvf archive.tar.gz            # extract a compressed archive
tar -tzvf archive.tar.gz            # list contents without extracting
```

**Flag reference:**

| Flag | Meaning                   |
|------|---------------------------|
| `-c` | Create archive            |
| `-x` | Extract archive           |
| `-z` | Use gzip compression      |
| `-j` | Use bzip2 compression     |
| `-v` | Verbose (show progress)   |
| `-f` | Specify filename          |

---

### `gzip` / `gunzip` — Compress and Decompress Files

```bash
gzip filename
gunzip filename.gz
```

Compresses a file into `.gz` format, or decompresses it.

```bash
gzip report.txt             # creates report.txt.gz
gunzip report.txt.gz        # restores report.txt
```

---

### `zip` / `unzip` — Zip Archive Management

```bash
zip archive.zip file1 file2
unzip archive.zip
```

Creates or extracts `.zip` archives.

```bash
zip -r project.zip project/         # zip a folder recursively
unzip project.zip -d /tmp/output/   # extract to a specific directory
```

---

## 11. System Information & Control

---

### `uname` — System Information

```bash
uname [options]
```

Displays system and kernel information.

```bash
uname -a        # all system info (kernel, architecture, hostname)
uname -r        # kernel version only
```

---

### `uptime` — System Uptime

```bash
uptime
```

Shows how long the system has been running, and the current load average.

```bash
uptime
```

---

### `free` — Memory Usage

```bash
free [options]
```

Displays total, used, and free RAM and swap memory.

```bash
free -h         # human-readable format
```

---

### `history` — Command History

```bash
history [n]
```

Lists previously entered commands. Use `!number` to re-run a command.

```bash
history          # show full history
history 20       # show last 20 commands
!42              # re-run command number 42
```

---

### `date` — Show or Set Date and Time

```bash
date [options]
```

Displays the current date and time, or sets it.

```bash
date
date "+%Y-%m-%d %H:%M:%S"    # custom format
```

---

### `reboot` / `shutdown` — Restart or Power Off

```bash
sudo reboot
sudo shutdown [options] [time]
```

Reboots or shuts down the system.

```bash
sudo reboot
sudo shutdown -h now          # shut down immediately
sudo shutdown -r +5           # reboot in 5 minutes
```

---

### `systemctl` — Manage System Services

```bash
systemctl [command] service_name
```

Controls systemd services (start, stop, restart, enable on boot, etc.).

```bash
sudo systemctl start nginx          # start the nginx service
sudo systemctl stop nginx           # stop it
sudo systemctl restart nginx        # restart it
sudo systemctl enable nginx         # start on boot
sudo systemctl status nginx         # check its status
```

---

### `journalctl` — View System Logs

```bash
journalctl [options]
```

Queries and displays logs from the systemd journal.

```bash
journalctl -u nginx               # logs for the nginx service
journalctl -f                     # follow live log output
journalctl --since "1 hour ago"   # recent logs
```

---

### `alias` — Create Command Shortcuts

```bash
alias name='command'
```

Creates a shortcut for a longer command. Add it to `~/.bashrc` to make it permanent.

```bash
alias ll='ls -la'
alias update='sudo apt update && sudo apt upgrade'
```

---

## 12. Redirects, Pipes & Useful Tricks

---

### `|` — Pipe Output to Another Command

Sends the output of one command as input to another.

```bash
ls -la | less                     # scroll through a long listing
ps aux | grep python              # filter process list
cat access.log | grep "404"       # find 404 errors in a log
```

---

### `>` and `>>` — Redirect Output to a File

`>` overwrites a file; `>>` appends to it.

```bash
echo "Hello World" > hello.txt        # write (overwrite)
echo "Another line" >> hello.txt      # append
ls -la > file_list.txt                # save directory listing to file
```

---

### `<` — Redirect Input from a File

Feeds a file's contents as input to a command.

```bash
sort < names.txt
wc -l < report.txt
```

---

### `2>` — Redirect Error Output

Redirects standard error (stderr) to a file separately from normal output.

```bash
command 2> errors.log             # save errors only
command > output.log 2>&1         # save both output and errors to one file
```

---

### `&&` and `||` — Chain Commands

`&&` runs the next command only if the previous one succeeded. `||` runs it only if the previous one failed.

```bash
sudo apt update && sudo apt upgrade       # upgrade only if update succeeds
mkdir newdir && cd newdir                 # create dir and enter it
ping -c1 google.com || echo "No internet"
```

---

### `xargs` — Build Commands from Input

```bash
command | xargs another_command
```

Takes output from one command and passes it as arguments to another.

```bash
find . -name "*.log" | xargs rm         # delete all found .log files
cat urls.txt | xargs wget               # download all URLs in a file
```

---

### `tee` — Read and Write Simultaneously

```bash
command | tee filename
```

Writes output to both the terminal and a file at the same time.

```bash
ls -la | tee directory_listing.txt
ping google.com | tee ping_results.txt
```

---

### `clear` — Clear the Terminal Screen

```bash
clear
```

Clears all text from the terminal window. Shortcut: `Ctrl + L`.

---

### `man` — Manual Pages

```bash
man command_name
```

Opens the official manual/documentation for any Linux command. Press `q` to quit.

```bash
man ls
man grep
man chmod
```

---

## Quick Reference Cheat Sheet

| Category        | Key Commands                                      |
|-----------------|---------------------------------------------------|
| Navigation      | `pwd`, `ls`, `cd`, `tree`                         |
| Files           | `touch`, `cp`, `mv`, `rm`, `mkdir`, `rmdir`       |
| Viewing         | `cat`, `less`, `head`, `tail`, `wc`               |
| Searching       | `find`, `grep`, `locate`, `which`                 |
| Permissions     | `chmod`, `chown`, `chgrp`, `umask`                |
| Processes       | `ps`, `top`, `kill`, `jobs`, `bg`, `fg`           |
| Networking      | `ping`, `curl`, `wget`, `ssh`, `scp`              |
| Packages        | `apt`, `dnf`, `snap`                              |
| Disk            | `df`, `du`, `lsblk`, `mount`                      |
| Users           | `whoami`, `useradd`, `passwd`, `sudo`, `usermod`  |
| Archiving       | `tar`, `gzip`, `zip`, `unzip`                     |
| System          | `uname`, `uptime`, `free`, `systemctl`, `history` |
| Shell Tricks    | `\|`, `>`, `>>`, `&&`, `man`, `alias`              |

---

> 💡 **Tip:** When in doubt, use `man <command>` or `command --help` to explore options for any Linux command.

---

*Guide level: Beginner → Intermediate | Best used as a study and daily reference.*