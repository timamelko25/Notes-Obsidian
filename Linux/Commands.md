- [curl](#curl)
- [wget](#wget)
- [zip](#zip)
- [unzip](#unzip)
- [tar](#tar)
- [ls](#ls)
- [mv](#mv)
- [cp](#cp)
- [rm](#rm)
- [grep](#grep)
- [find](#find)
- [chmod](#chmod)
- [journalctl](#journalctl)
- [df](#df)
- [du](#du)
- [smartctl](#smartctl)
- [top](#top)
- [ps](#ps)
- [systemctl](#systemctl)
- [strace](#strace)
- [netstat](#netstat)
- [ss](#ss)
- [ip](#ip)
- [traceroute](#traceroute)
- [mtr](#mtr)
- [dig](#dig)
- [tcpdump](#tcpdump)
- [dmesg](#dmesg)
- [ssh](#ssh)
- [scp](#scp)
- [ping](#ping)
- [cat](#cat)
- [less](#less)
- [tail](#tail)
- [head](#head)
- [sort](#sort)
- [uniq](#uniq)
- [wc](#wc)
- [sed](#sed)
- [awk](#awk)
- [mount](#mount)
- [umount](#umount)
- [rsync](#rsync)
- [crontab](#crontab)
- [kill](#kill)
- [watch](#watch)
- [screen](#screen)
- [htop](#htop)
- [free](#free)
- [who](#who)
- [uptime](#uptime)

## curl

**Description:** Transfers data to/from servers using protocols like HTTP, HTTPS, and FTP, ideal for downloads and API requests.

**Key Options:**

- `-L`: Follows HTTP redirects.
- `--output [file]`: Saves output to a file.
- `-C -`: Resumes interrupted downloads.
- `-m [seconds]`: Sets maximum operation time.
- `-s`: Silent mode (no progress output).
- `-O`: Saves with remote file name.
- `-I`: Fetches HTTP headers only.
- `-H [header]`: Adds custom headers.
- `-d [data]`: Sends POST data.
- `-T [file]`: Uploads a file.

**Example:**

```bash
curl -L https://example.com --output example.html
```

---

## wget

**Description:** Downloads files from HTTP, HTTPS, or FTP, supporting resuming and recursive downloads.

**Key Options:**

- `-b`: Runs in background.
- `-o [file]`: Logs to a file.
- `-q`: Quiet mode.
- `-i [file]`: Reads URLs from a file.
- `-t [number]`: Sets retry attempts.
- `-O [file]`: Specifies output file.
- `-c`: Resumes partial downloads.
- `-r`: Enables recursive download.
- `-m`: Mirrors a website.

**Example:**

```bash
wget -c https://example.com/file.tar.gz
```

---

## zip

**Description:** Creates compressed `.zip` archives from files or directories.

**Key Options:**

- `-r`: Recursively includes directories.
- `-0`: Stores without compression.
- `-9`: Uses maximum compression.
- `-e`: Encrypts with a password.
- `-d`: Deletes files from archive.

**Example:**

```bash
zip -r archive.zip /path/to/files
```

---

## unzip

**Description:** Extracts files from `.zip` archives.

**Key Options:**

- `-l`: Lists archive contents.
- `-d [dir]`: Extracts to a directory.
- `-P [password]`: Decrypts with a password.

**Example:**

```bash
unzip archive.zip -d /tmp
```

---

## tar

**Description:** Archives files into `.tar` format, often with gzip or bzip2 compression.

**Key Options:**

- `-c`: Creates an archive.
- `-x`: Extracts files.
- `-z`: Uses gzip compression.
- `-j`: Uses bzip2 compression.
- `-v`: Verbose output.
- `-f [file]`: Specifies archive file.
- `-t`: Lists contents.
- `-C [dir]`: Changes directory for extraction.

**Example:**

```bash
tar -czvf archive.tar.gz /path/to/files
```

---

## ls

**Description:** Lists directory contents.

**Key Options:**

- `-a`: Shows hidden files.
- `-l`: Long format (permissions, owner, size).
- `-h`: Human-readable sizes.
- `-R`: Recursively lists subdirectories.
- `-t`: Sorts by modification time.
- `-S`: Sorts by size.

**Example:**

```bash
ls -lah
```

---

## mv

**Description:** Moves or renames files/directories.

**Key Options:**

- `-f`: Forces overwrite without prompting.
- `-i`: Prompts before overwriting.
- `-n`: Prevents overwriting.
- `-u`: Moves only if source is newer.
- `-v`: Verbose output.

**Example:**

```bash
mv file.txt /new/location/
```

---

## cp

**Description:** Copies files or directories.

**Key Options:**

- `-r`: Copies directories recursively.
- `-a`: Preserves file attributes.
- `-i`: Prompts before overwriting.
- `-u`: Copies only newer files.
- `-v`: Verbose output.

**Example:**

```bash
cp -r src/ dest/
```

---

## rm

**Description:** Deletes files or directories.

**Key Options:**

- `-r`: Deletes recursively.
- `-f`: Forces deletion without prompts.
- `-i`: Prompts before deletion.
- `-v`: Shows deleted files.

**Example:**

```bash
rm -rf mydir
```

---

## grep

**Description:** Searches text patterns in files or input using regular expressions.

**Key Options:**

- `-i`: Ignores case.
- `-w`: Matches whole words.
- `-v`: Inverts match.
- `-n`: Shows line numbers.
- `-r`: Searches recursively.
- `-l`: Lists filenames with matches.
- `-c`: Counts matches.

**Example:**

```bash
grep -r "error" /var/log
```

---

## find

**Description:** Searches files by name, type, or size.

**Key Options:**

- `-name [pattern]`: Matches filename.
- `-type [f|d]`: Filters by file (`f`) or directory (`d`).
- `-size [+|-n]`: Filters by size (e.g., `+10M`).
- `-mtime [n]`: Filters by modification time.
- `-exec [cmd]`: Runs command on results.

**Example:**

```bash
find / -type f -name "*.cpp"
```

---

## chmod

**Description:** Changes file/directory permissions.

**Key Options:**

- `-R`: Applies recursively.
- `-v`: Shows changes.
- Permissions: `r` (read=4), `w` (write=2), `x` (execute=1).
- Users: `u` (owner), `g` (group), `o` (others).

**Example:**

```bash
chmod 755 script.sh
```

---

## journalctl

**Description:** Views systemd journal logs.

**Key Options:**

- `-f`: Follows logs in real-time.
- `-u [unit]`: Filters by service.
- `-b`: Shows logs from current boot.
- `-r`: Reverses output.
- `-p [0-7]`: Filters by priority.
- `-n [lines]`: Limits to last `n` lines.

**Example:**

```bash
journalctl -u nginx -f
```

---

## df

**Description:** Displays disk space usage for filesystems.

**Key Options:**

- `-h`: Human-readable sizes.
- `-i`: Shows inode usage.

**Example:**

```bash
df -h
```

---

## du

**Description:** Estimates file/directory disk usage.

**Key Options:**

- `-h`: Human-readable sizes.
- `-s`: Summarizes total size.
- `-c`: Shows grand total.

**Example:**

```bash
du -sh /tmp
```

---

## smartctl

**Description:** Monitors disk health using SMART data.

**Key Options:**

- `-a`: Shows all SMART data.
- `-H`: Displays health status.
- `-i`: Shows disk info.

**Example:**

```bash
smartctl -a /dev/sda
```

---

## top

**Description:** Monitors system processes in real-time.

**Key Options:**

- `-d [seconds]`: Sets refresh interval.
- `-u [user]`: Filters by user.
- `-n [iterations]`: Limits updates.

**Example:**

```bash
top
```

---

## ps

**Description:** Lists current processes.

**Key Options:**

- `aux`: Shows all processes in user format.
- `-e`: Shows every process.
- `-f`: Full-format listing.

**Example:**

```bash
ps aux
```

---

## systemctl

**Description:** Manages systemd services.

**Key Options:**

- `start/stop/restart [service]`: Controls a service.
- `status [service]`: Shows service status.
- `enable/disable [service]`: Manages boot behavior.

**Example:**

```bash
systemctl restart sshd
```

---

## strace

**Description:** Traces system calls for debugging.

**Key Options:**

- `-p [pid]`: Attaches to a process.
- `-o [file]`: Writes output to a file.
- `-f`: Traces child processes.

**Example:**

```bash
strace -p 1234
```

---

## netstat

**Description:** Shows network connections and stats.

**Key Options:**

- `-t`: TCP connections.
- `-u`: UDP connections.
- `-l`: Listening sockets.
- `-n`: Numeric addresses.
- `-p`: Shows process IDs.

**Example:**

```bash
netstat -tulnp
```

---

## ss

**Description:** Displays socket statistics.

**Key Options:**

- `-l`: Listening sockets.
- `-n`: Numeric output.
- `-t`: TCP sockets.
- `-u`: UDP sockets.

**Example:**

```bash
ss -lntu
```

---

## ip

**Description:** Manages network interfaces and routes.

**Key Options:**

- `a`: Lists interfaces.
- `r`: Shows routing table.
- `-br`: Brief output.

**Example:**

```bash
ip a
```

---

## traceroute

**Description:** Traces packet routes to a host.

**Key Options:**

- `-n`: Numeric output (no DNS).
- `-m [hops]`: Sets max hops.

**Example:**

```bash
traceroute -n 8.8.8.8
```

---

## mtr

**Description:** Combines `ping` and `traceroute` for real-time diagnostics.

**Key Options:**

- `-r`: Report mode.
- `-c [count]`: Limits packets.

**Example:**

```bash
mtr 8.8.8.8
```

---

## dig

**Description:** Queries DNS records.

**Key Options:**

- `+short`: Concise output.
- `[type]`: Specifies record type (e.g., `MX`).

**Example:**

```bash
dig example.com
```

---

## tcpdump

**Description:** Captures network packets.

**Key Options:**

- `-i [interface]`: Specifies interface.
- `-n`: Numeric addresses.
- `-w [file]`: Saves to file.

**Example:**

```bash
tcpdump -i eth0
```

---

## dmesg

**Description:** Shows kernel log messages.

**Key Options:**

- `-T`: Human-readable timestamps.
- `-L`: Colorizes output.
- `-C`: Clears the buffer.

**Example:**

```bash
dmesg -T
```

---

## ssh

**Description:** Connects to remote systems securely.

**Key Options:**

- `-p [port]`: Specifies port.
- `-i [key]`: Uses an identity file.
- `-X`: Enables X11 forwarding.

**Example:**

```bash
ssh user@host
```

---

## scp

**Description:** Copies files between hosts securely.

**Key Options:**

- `-P [port]`: Specifies port.
- `-r`: Copies recursively.
- `-i [key]`: Uses an identity file.

**Example:**

```bash
scp file user@host:/path
```

---

## ping

**Description:** Tests network connectivity.

**Key Options:**

- `-c [count]`: Limits packet count.
- `-i [seconds]`: Sets interval between pings.
- `-s [size]`: Sets packet size.

**Example:**

```bash
ping -c 4 8.8.8.8
```

---

## cat

**Description:** Displays file contents.

**Key Options:**

- `-n`: Numbers lines.
- `-b`: Numbers non-blank lines.
- `-s`: Squeezes multiple blank lines.

**Example:**

```bash
cat file.txt
```

---

## less

**Description:** Views files page-by-page.

**Key Options:**

- `-N`: Shows line numbers.
- `-S`: Chops long lines.
- `-i`: Ignores case in searches.

**Example:**

```bash
less file.txt
```

---

## tail

**Description:** Shows last lines of a file.

**Key Options:**

- `-n [lines]`: Shows specific number of lines.
- `-f`: Follows file for new data.
- `-q`: Suppresses headers.

**Example:**

```bash
tail -f /var/log/syslog
```

---

## head

**Description:** Shows first lines of a file.

**Key Options:**

- `-n [lines]`: Shows specific number of lines.
- `-c [bytes]`: Shows specific number of bytes.

**Example:**

```bash
head -n 10 file.txt
```

---

## sort

**Description:** Sorts lines of text.

**Key Options:**

- `-r`: Reverses sort order.
- `-n`: Numeric sort.
- `-k [field]`: Sorts by specific field.

**Example:**

```bash
sort file.txt
```

---

## uniq

**Description:** Removes duplicate lines from sorted input.

**Key Options:**

- `-c`: Counts occurrences.
- `-i`: Ignores case.
- `-u`: Shows unique lines only.

**Example:**

```bash
sort file.txt | uniq
```

---

## wc

**Description:** Counts lines, words, and characters.

**Key Options:**

- `-l`: Counts lines.
- `-w`: Counts words.
- `-c`: Counts bytes.

**Example:**

```bash
wc file.txt
```

---

## sed

**Description:** Edits text streams.

**Key Options:**

- `-i`: Edits file in place.
- `-e [script]`: Adds script to execute.
- `-n`: Suppresses automatic printing.

**Example:**

```bash
sed 's/old/new/' file.txt
```

---

## awk

**Description:** Processes text patterns.

**Key Options:**

- `-F [sep]`: Sets field separator.
- `-v [var=value]`: Assigns variables.
- `-f [file]`: Reads script from file.

**Example:**

```bash
awk '{print $1}' file.txt
```

---

## mount

**Description:** Mounts filesystems.

**Key Options:**

- `-t [type]`: Specifies filesystem type.
- `-o [options]`: Sets mount options (e.g., `ro`).

**Example:**

```bash
mount /dev/sda1 /mnt
```

---

## umount

**Description:** Unmounts filesystems.

**Key Options:**

- `-l`: Lazy unmount.
- `-f`: Forces unmount.

**Example:**

```bash
umount /mnt
```

---

## rsync

**Description:** Syncs files efficiently.

**Key Options:**

- `-a`: Archive mode (preserves attributes).
- `-v`: Verbose output.
- `-r`: Recursive copy.

**Example:**

```bash
rsync -av src/ dest/
```

---

## crontab

**Description:** Schedules recurring tasks.

**Key Options:**

- `-e`: Edits crontab file.
- `-l`: Lists crontab entries.
- `-r`: Removes crontab.

**Example:**

```bash
crontab -e
```

---

## kill

**Description:** Terminates processes by PID.

**Key Options:**

- `-9`: Sends SIGKILL (forceful termination).
- `-l`: Lists signal names.

**Example:**

```bash
kill 1234
```

---

## watch

**Description:** Runs commands repeatedly.

**Key Options:**

- `-n [seconds]`: Sets interval.
- `-d`: Highlights changes.

**Example:**

```bash
watch -n 2 df -h
```

---

## screen

**Description:** Manages terminal sessions.

**Key Options:**

- `-r`: Reattaches to a session.
- `-ls`: Lists sessions.
- `-d`: Detaches a session.

**Example:**

```bash
screen
```

---

## htop

**Description:** Enhanced process viewer.

**Key Options:**

- `-d [delay]`: Sets refresh interval.
- `-u [user]`: Filters by user.
- `-s [column]`: Sorts by column.

**Example:**

```bash
htop
```

---

## free

**Description:** Shows memory usage.

**Key Options:**

- `-m`: Shows in MB.
- `-h`: Human-readable format.
- `-s [seconds]`: Continuous output.

**Example:**

```bash
free -m
```

---

## who

**Description:** Lists logged-in users.

**Key Options:**

- `-a`: Shows all information.
- `-u`: Shows idle time.
- `-H`: Adds headers.

**Example:**

```bash
who
```

---

## uptime

**Description:** Shows system uptime and load average.

**Key Options:**

- `-p`: Pretty format for uptime.
- `-s`: Shows since time.

**Example:**

```bash
uptime
```

---