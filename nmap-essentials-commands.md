# 🔍 Nmap Commands Guide: Beginner to Intermediate

> A structured handbook for cybersecurity students learning network scanning with Nmap.

> ⚠️ **Legal Notice:** Only scan networks and systems you own or have **explicit written permission** to test. Unauthorized scanning is illegal and unethical.

---

## Table of Contents

1. [What is Nmap?](#1-what-is-nmap)
2. [Basic Scanning](#2-basic-scanning)
3. [Port Scanning Techniques](#3-port-scanning-techniques)
4. [Service & Version Detection](#4-service--version-detection)
5. [OS Detection](#5-os-detection)
6. [Timing & Performance](#6-timing--performance)
7. [Output Options](#7-output-options)
8. [Firewall Evasion & Stealth](#8-firewall-evasion--stealth)
9. [NSE — Nmap Scripting Engine Basics](#9-nse--nmap-scripting-engine-basics)
10. [Quick Reference Cheat Sheet](#10-quick-reference-cheat-sheet)

---

## 1. What is Nmap?

**Nmap** (Network Mapper) is a free, open-source tool used for network discovery and security auditing. It can detect:

- Which **hosts** are online on a network
- Which **ports** are open on those hosts
- What **services and versions** are running
- What **operating system** a target is using
- Potential **vulnerabilities** via scripting

**Install Nmap:**

```bash
# Debian / Ubuntu
sudo apt install nmap

# Red Hat / CentOS / Fedora
sudo dnf install nmap

# macOS
brew install nmap
```

**Check version:**

```bash
nmap --version
```

---

## 2. Basic Scanning

---

### Scan a Single Host

```bash
nmap <target>
```

Scans the most common 1,000 ports of a single IP address or hostname.

```bash
nmap 192.168.1.1
```

---

### Scan a Hostname

```bash
nmap <hostname>
```

Resolves the hostname via DNS and scans it.

```bash
nmap scanme.nmap.org
```

> 💡 **Tip:** `scanme.nmap.org` is a host maintained by the Nmap project that is legal to scan for practice.

---

### Scan Multiple Hosts

```bash
nmap <target1> <target2> <target3>
```

Scans multiple IPs or hostnames in a single command.

```bash
nmap 192.168.1.1 192.168.1.2 192.168.1.5
```

---

### Scan an IP Range

```bash
nmap <start_ip>-<end_ip>
```

Scans a consecutive range of IP addresses.

```bash
nmap 192.168.1.1-50
```

---

### Scan a Subnet (CIDR Notation)

```bash
nmap <ip/cidr>
```

Scans an entire subnet of hosts using CIDR notation.

```bash
nmap 192.168.1.0/24
```

> 💡 **Tip:** `/24` covers 256 addresses (192.168.1.0 – 192.168.1.255). `/16` covers 65,536 addresses.

---

### Scan from a Target List File

```bash
nmap -iL <filename>
```

Reads targets from a plain text file (one IP or hostname per line).

```bash
nmap -iL targets.txt
```

---

### Exclude Hosts from a Scan

```bash
nmap <range> --exclude <ip>
```

Scans a range but skips specified hosts.

```bash
nmap 192.168.1.0/24 --exclude 192.168.1.1
```

---

### Ping Scan — Host Discovery Only

```bash
nmap -sn <target>
```

Checks which hosts are online without scanning any ports. Fast and lightweight.

```bash
nmap -sn 192.168.1.0/24
```

---

### Skip Host Discovery (Treat Host as Online)

```bash
nmap -Pn <target>
```

Skips the ping step and forces a port scan even if the host appears offline (useful for hosts that block pings).

```bash
nmap -Pn 192.168.1.1
```

---

### Scan Specific Ports

```bash
nmap -p <port(s)> <target>
```

Scans only the specified port(s) instead of the default 1,000.

```bash
nmap -p 22,80,443 192.168.1.1
nmap -p 1-1000 192.168.1.1       # scan a port range
nmap -p- 192.168.1.1              # scan ALL 65535 ports
```

---

## 3. Port Scanning Techniques

---

### TCP SYN Scan (Default / Stealth Scan)

```bash
nmap -sS <target>
```

Sends SYN packets and checks responses. Fast and less likely to appear in logs since it never completes the TCP handshake. **Requires root/sudo.**

```bash
sudo nmap -sS 192.168.1.1
```

> 💡 **Tip:** This is Nmap's default scan when run as root. It is the most commonly used technique.

---

### TCP Connect Scan

```bash
nmap -sT <target>
```

Completes a full TCP handshake. Does not require root, but is more visible in logs.

```bash
nmap -sT 192.168.1.1
```

---

### UDP Scan

```bash
nmap -sU <target>
```

Scans for open UDP ports. Slower than TCP scans because UDP has no handshake confirmation.

```bash
sudo nmap -sU -p 53,67,161 192.168.1.1
```

> 💡 **Tip:** Combine UDP and TCP scans: `nmap -sS -sU <target>` to scan both protocols at once.

---

### ACK Scan

```bash
nmap -sA <target>
```

Sends ACK packets to determine if ports are filtered by a firewall. Does **not** tell you if a port is open — only if it's filtered or unfiltered.

```bash
sudo nmap -sA 192.168.1.1
```

---

### FIN / NULL / Xmas Scans

```bash
nmap -sF <target>    # FIN scan
nmap -sN <target>    # NULL scan
nmap -sX <target>    # Xmas scan
```

Send unusual TCP flag combinations to evade basic firewalls. May not work reliably on Windows hosts.

```bash
sudo nmap -sX 192.168.1.1
```

---

### Window Scan

```bash
nmap -sW <target>
```

Similar to ACK scan but examines the TCP window field to distinguish open vs. closed ports on some systems.

```bash
sudo nmap -sW 192.168.1.1
```

---

### SCTP INIT Scan

```bash
nmap -sY <target>
```

Scans for open SCTP ports (used in telecom/VoIP). Analogous to a TCP SYN scan.

```bash
sudo nmap -sY 192.168.1.1
```

---

### Scan Top N Most Common Ports

```bash
nmap --top-ports <number> <target>
```

Scans only the N most commonly used ports, making scans faster.

```bash
nmap --top-ports 100 192.168.1.1
```

---

## 4. Service & Version Detection

---

### Service Version Detection

```bash
nmap -sV <target>
```

Probes open ports to determine the name and version of the service running on each.

```bash
nmap -sV 192.168.1.1
```

---

### Adjust Version Detection Intensity

```bash
nmap -sV --version-intensity <0-9> <target>
```

Controls how aggressively Nmap probes for version info. `0` is lightest, `9` is most thorough.

```bash
nmap -sV --version-intensity 5 192.168.1.1
```

---

### Light Version Scan (Faster)

```bash
nmap -sV --version-light <target>
```

Equivalent to `--version-intensity 2`. Faster but less accurate.

```bash
nmap -sV --version-light 192.168.1.1
```

---

### Aggressive Version Scan (More Accurate)

```bash
nmap -sV --version-all <target>
```

Tries every single probe. Most accurate but significantly slower.

```bash
nmap -sV --version-all 192.168.1.1
```

---

### Combined Aggressive Scan Mode

```bash
nmap -A <target>
```

Enables OS detection, version detection, script scanning, and traceroute all at once. The most informative general-purpose scan.

```bash
nmap -A 192.168.1.1
```

> 💡 **Tip:** `-A` is great for getting a comprehensive overview of a host, but it is noisy and slower than individual options.

---

## 5. OS Detection

---

### OS Detection

```bash
nmap -O <target>
```

Attempts to identify the operating system of the target by analyzing network responses. **Requires root/sudo.**

```bash
sudo nmap -O 192.168.1.1
```

---

### Aggressive OS Detection

```bash
nmap -O --osscan-guess <target>
```

When Nmap cannot perfectly identify the OS, this flag makes it guess the closest match.

```bash
sudo nmap -O --osscan-guess 192.168.1.1
```

---

### Limit OS Detection to Likely Matches

```bash
nmap -O --osscan-limit <target>
```

Skips hosts that have no open or closed ports, saving time on large scans.

```bash
sudo nmap -O --osscan-limit 192.168.1.0/24
```

---

### OS + Version Detection Together

```bash
nmap -O -sV <target>
```

Combines OS fingerprinting with service version detection for a detailed host profile.

```bash
sudo nmap -O -sV 192.168.1.1
```

---

## 6. Timing & Performance

Nmap has six timing templates (`T0` through `T5`) that control the speed and aggressiveness of a scan.

| Template | Name       | Use Case                                      |
|----------|------------|-----------------------------------------------|
| `T0`     | Paranoid   | IDS evasion, very slow                        |
| `T1`     | Sneaky     | IDS evasion, slow                             |
| `T2`     | Polite     | Less bandwidth, slower                        |
| `T3`     | Normal     | Default speed                                 |
| `T4`     | Aggressive | Faster, assumes a reliable network            |
| `T5`     | Insane     | Very fast, may miss results on slow networks  |

---

### Set Timing Template

```bash
nmap -T<0-5> <target>
```

Applies a timing preset to control scan speed and stealth.

```bash
nmap -T4 192.168.1.1       # fast scan for local networks
nmap -T1 192.168.1.1       # slow and quiet scan
```

---

### Set Minimum Packet Rate

```bash
nmap --min-rate <packets_per_second> <target>
```

Forces Nmap to send packets no slower than the specified rate.

```bash
nmap --min-rate 1000 192.168.1.0/24
```

---

### Set Maximum Packet Rate

```bash
nmap --max-rate <packets_per_second> <target>
```

Caps the rate at which Nmap sends packets (useful to avoid overwhelming a target or network).

```bash
nmap --max-rate 200 192.168.1.1
```

---

### Set Parallel Scan Groups

```bash
nmap --min-parallelism <num>
nmap --max-parallelism <num>
```

Controls how many port probes Nmap sends in parallel. Higher values = faster scan, more resource use.

```bash
nmap --min-parallelism 50 192.168.1.0/24
```

---

### Set Host Timeout

```bash
nmap --host-timeout <time> <target>
```

Skips a host if it takes longer than the specified time (e.g., `30s`, `2m`).

```bash
nmap --host-timeout 30s 192.168.1.0/24
```

---

## 7. Output Options

---

### Normal Output to Terminal (Default)

```bash
nmap <target>
```

Results are printed to the terminal in a human-readable format by default.

```bash
nmap 192.168.1.1
```

---

### Save Output as Normal Text

```bash
nmap -oN <filename> <target>
```

Saves the scan results to a plain text file in the same format as the terminal output.

```bash
nmap -oN scan_results.txt 192.168.1.1
```

---

### Save Output as XML

```bash
nmap -oX <filename> <target>
```

Saves output in XML format — useful for parsing with scripts or importing into other tools.

```bash
nmap -oX scan_results.xml 192.168.1.1
```

---

### Save Output as Grepable Format

```bash
nmap -oG <filename> <target>
```

Saves output in a line-based format that is easy to search with `grep` and `awk`.

```bash
nmap -oG scan_results.gnmap 192.168.1.1
```

---

### Save in All Formats at Once

```bash
nmap -oA <basename> <target>
```

Saves the output in Normal, XML, and Grepable formats simultaneously using the given base name.

```bash
nmap -oA full_scan 192.168.1.1
# Creates: full_scan.nmap, full_scan.xml, full_scan.gnmap
```

---

### Increase Verbosity

```bash
nmap -v <target>
nmap -vv <target>
```

Prints more details during the scan. `-vv` provides even more verbose output.

```bash
nmap -v 192.168.1.1
```

---

### Increase Debugging Output

```bash
nmap -d <target>
nmap -dd <target>
```

Provides very detailed debug information — useful for troubleshooting scan issues.

```bash
nmap -d 192.168.1.1
```

---

### Show Reason for Port State

```bash
nmap --reason <target>
```

Shows why Nmap concluded a port is open, closed, or filtered (e.g., "syn-ack", "reset", "no-response").

```bash
nmap --reason 192.168.1.1
```

---

### Resume an Aborted Scan

```bash
nmap --resume <grepable_output_file>
```

Continues a scan that was interrupted, using a previously saved grepable output file.

```bash
nmap --resume scan_results.gnmap
```

---

## 8. Firewall Evasion & Stealth

> ⚠️ Use these techniques only on systems you are authorized to test. Evasion techniques are meant for legitimate penetration testing to understand firewall effectiveness.

---

### Fragment Packets

```bash
nmap -f <target>
```

Splits packets into small fragments to bypass simple packet-inspection firewalls.

```bash
sudo nmap -f 192.168.1.1
```

---

### Use a Decoy

```bash
nmap -D <decoy1>,<decoy2>,ME <target>
```

Makes the scan appear to originate from multiple IP addresses simultaneously, hiding the real source among decoys. `ME` represents your actual IP.

```bash
sudo nmap -D 10.0.0.5,10.0.0.6,ME 192.168.1.1
```

---

### Spoof Source IP Address

```bash
nmap -S <spoofed_ip> -e <interface> <target>
```

Spoofs the source IP address. Responses will go to the spoofed address, not to you — mainly useful in testing specific scenarios on controlled networks.

```bash
sudo nmap -S 10.0.0.99 -e eth0 192.168.1.1
```

---

### Spoof Source Port

```bash
nmap --source-port <port> <target>
```

Makes packets appear to come from a specific port (e.g., 53 for DNS or 80 for HTTP), which some firewalls trust.

```bash
sudo nmap --source-port 53 192.168.1.1
```

---

### Randomize Target Order

```bash
nmap --randomize-hosts <target>
```

Scans hosts in random order instead of sequentially, making the scan pattern less obvious.

```bash
nmap --randomize-hosts 192.168.1.0/24
```

---

### Append Random Data to Packets

```bash
nmap --data-length <num> <target>
```

Adds random bytes to packets to make them appear more "normal" and bypass length-based detection.

```bash
sudo nmap --data-length 25 192.168.1.1
```

---

### Set TTL Value

```bash
nmap --ttl <value> <target>
```

Manually sets the IP Time-To-Live field in packets to mimic specific operating systems or bypass rules.

```bash
sudo nmap --ttl 64 192.168.1.1
```

---

### Use a Proxy / Idle Zombie Scan

```bash
nmap -sI <zombie_host> <target>
```

Uses a third-party "zombie" host to perform an Idle Scan — the most stealthy scan type, as your IP never touches the target directly.

```bash
sudo nmap -sI 10.0.0.50 192.168.1.1
```

> 💡 **Tip:** The zombie host must have low or predictable IP ID sequence behavior for this to work.

---

## 9. NSE — Nmap Scripting Engine Basics

The **Nmap Scripting Engine (NSE)** allows Nmap to run scripts for advanced tasks like vulnerability detection, authentication testing, and service enumeration. Scripts are located in `/usr/share/nmap/scripts/`.

---

### Run Default Scripts

```bash
nmap -sC <target>
```

Runs a safe set of default scripts against the target. Equivalent to `--script=default`.

```bash
nmap -sC 192.168.1.1
```

---

### Run a Specific Script

```bash
nmap --script <script_name> <target>
```

Runs a single named script against the target.

```bash
nmap --script http-title 192.168.1.1
```

---

### Run Multiple Scripts

```bash
nmap --script <script1>,<script2> <target>
```

Runs multiple specified scripts in one scan.

```bash
nmap --script http-headers,http-methods 192.168.1.1
```

---

### Run Scripts by Category

```bash
nmap --script <category> <target>
```

Runs all scripts belonging to a specific category.

| Category   | Description                                     |
|------------|-------------------------------------------------|
| `default`  | Safe, general-purpose scripts                   |
| `discovery`| Find additional info about hosts/services       |
| `safe`     | Scripts unlikely to crash services              |
| `vuln`     | Check for known vulnerabilities                 |
| `auth`     | Test authentication mechanisms                  |
| `brute`    | Brute-force credential testing                  |
| `exploit`  | Attempt to exploit vulnerabilities              |
| `intrusive`| More aggressive, may affect target stability    |

```bash
nmap --script vuln 192.168.1.1
```

---

### Pass Arguments to a Script

```bash
nmap --script <script_name> --script-args <key=value> <target>
```

Provides input parameters required by certain scripts.

```bash
nmap --script http-brute --script-args http-brute.path=/admin 192.168.1.1
```

---

### Common Useful Scripts

**Check HTTP server headers:**

```bash
nmap --script http-headers -p 80 192.168.1.1
```

**Enumerate SMB shares:**

```bash
nmap --script smb-enum-shares -p 445 192.168.1.1
```

**Check for anonymous FTP access:**

```bash
nmap --script ftp-anon -p 21 192.168.1.1
```

**Check for SSL/TLS vulnerabilities:**

```bash
nmap --script ssl-enum-ciphers -p 443 192.168.1.1
```

**Detect common vulnerabilities:**

```bash
nmap --script vuln 192.168.1.1
```

---

### Search for Available Scripts

```bash
nmap --script-help <script_name>
```

Displays help and usage information for a specific script.

```bash
nmap --script-help http-title
```

**Search scripts from the terminal:**

```bash
ls /usr/share/nmap/scripts/ | grep ftp
```

---

### Update NSE Script Database

```bash
sudo nmap --script-updatedb
```

Refreshes Nmap's internal database of available scripts after adding new ones.

```bash
sudo nmap --script-updatedb
```

---

## 10. Quick Reference Cheat Sheet

| Goal                              | Command                                          |
|-----------------------------------|--------------------------------------------------|
| Simple host scan                  | `nmap 192.168.1.1`                               |
| Discover live hosts               | `nmap -sn 192.168.1.0/24`                        |
| Scan all ports                    | `nmap -p- 192.168.1.1`                           |
| Scan top 100 ports                | `nmap --top-ports 100 192.168.1.1`               |
| TCP SYN scan                      | `sudo nmap -sS 192.168.1.1`                      |
| UDP scan                          | `sudo nmap -sU 192.168.1.1`                      |
| Service version detection         | `nmap -sV 192.168.1.1`                           |
| OS detection                      | `sudo nmap -O 192.168.1.1`                       |
| Full aggressive scan              | `nmap -A 192.168.1.1`                            |
| Fast scan (T4)                    | `nmap -T4 192.168.1.1`                           |
| Stealth + timing                  | `sudo nmap -sS -T2 192.168.1.1`                  |
| Save all output formats           | `nmap -oA results 192.168.1.1`                   |
| Fragment packets                  | `sudo nmap -f 192.168.1.1`                       |
| Use decoys                        | `sudo nmap -D 10.0.0.5,ME 192.168.1.1`           |
| Run default scripts               | `nmap -sC 192.168.1.1`                           |
| Run vuln scripts                  | `nmap --script vuln 192.168.1.1`                 |
| Full recon scan                   | `sudo nmap -A -sS -sV -O -sC -p- 192.168.1.1`   |
| Scan from file + save output      | `nmap -iL targets.txt -oA output`                |

---

### Most Common Scan Combinations

```bash
# Standard discovery + service scan (beginner-friendly)
nmap -sV -sC -T4 192.168.1.1

# Full port scan with OS and version detection
sudo nmap -p- -sS -sV -O -T4 192.168.1.1

# Quiet, stealthy scan with packet fragmentation
sudo nmap -sS -T1 -f --reason 192.168.1.1

# Network-wide host discovery + top ports
nmap -sn 192.168.1.0/24 && nmap -T4 --top-ports 200 192.168.1.0/24

# Save everything for later review
sudo nmap -A -p- -oA full_audit 192.168.1.1
```

---

> 💡 **Learning Resources:**
> - Official Nmap documentation: [https://nmap.org/docs.html](https://nmap.org/docs.html)
> - Nmap book (free online): [https://nmap.org/book/](https://nmap.org/book/)
> - Practice safely at: `scanme.nmap.org`

---

*Guide level: Beginner → Intermediate | Intended for cybersecurity students and ethical hacking learners.*