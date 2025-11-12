# LinuxH - Linux Hardening & Inspection Tool

![Bash](https://img.shields.io/badge/Bash-4.0%2B-blue)
![Platform](https://img.shields.io/badge/Platform-Linux-lightgrey)
![GitHub](https://img.shields.io/badge/GitHub-Ready-brightgreen)

A comprehensive, safe-by-default Linux security hardening and inspection tool designed for system administrators and security professionals.

**Note: This script will only be able to install openvpn and configure default firewall and scan for backdoors and malware; rest like connecting to vpn,setting port firewall rules and removing scanned malware need to be manually done as it is not a safe operation for the script to perform**

## 🚀 Quick Installation

```bash
# Clone the repository
git clone https://github.com/passioncoder5/linuxh.git
cd linuxh

# Make executable
chmod +x linuxh

# Run directly
./linuxh --help

# Or install system-wide (optional)
sudo cp linuxh /usr/local/bin/linuxh
```

## 🛡️ Features

- **Safe by Default**: Dry-run mode prevents accidental system changes
- **Comprehensive Security**: Multiple hardening modules in one tool
- **Interactive Prompts**: Confirms dangerous operations before execution
- **Backup System**: Automatically backs up files before modification
- **Cross-Distribution**: Supports multiple package managers (APT, YUM, DNF, Pacman, Zypper)
- **Comprehensive Logging**: Detailed logs of all actions performed

## 📋 Security Modules

| Module | Description | Safety Level |
|--------|-------------|--------------|
| **`analyze`** | Network connection analysis | 🔒 Read-only |
| **`manage-processes`** | Suspicious process management | ⚠️ Destructive |
| **`ufw`** | Firewall configuration | ⚠️ Destructive |
| **`openvpn`** | VPN setup guidance | 🔒 Read-only |
| **`ssh`** | SSH server hardening | ⚠️ Destructive |
| **`sysctl`** | Kernel security settings | ⚠️ Destructive |
| **`services`** | Legacy service disablement | ⚠️ Destructive |
| **`install-tools`** | Security tools installation | ⚠️ Destructive |
| **`scans`** | Security scans (rkhunter/chkrootkit) | 🔒 Read-only |
| **`auto-updates`** | Automatic update configuration | ⚠️ Destructive |
| **`all`** | Run complete security suite | ⚠️ Destructive |

## 🎯 Quick Start Examples

### Safety First - Always Test with Dry-Run
```bash
# See what would be done without making changes (SAFE)
./linuxh --dry-run all

# Preview specific modules
./linuxh --dry-run ssh
./linuxh --dry-run ufw
```

### Production Usage (Requires Root)
```bash
# Apply all security hardening with prompts
sudo ./linuxh --apply all

# Apply specific modules
sudo ./linuxh --apply ssh
sudo ./linuxh --apply ufw
sudo ./linuxh --apply install-tools

# Auto-confirm all prompts (USE WITH CAUTION)
sudo ./linuxh --apply --yes all
```

## ⚙️ Command Line Reference

### Global Options
```bash
--help, -h           Show help message
--dry-run            Show actions without executing (DEFAULT)
--apply              Actually perform changes (requires root)
--yes                Auto-confirm all prompts (dangerous)
--backup-dir DIR     Custom backup directory
--log-file FILE      Custom log file location
```

### Available Commands
```bash
# Analysis & Monitoring
./linuxh analyze                    # Network connection analysis
./linuxh manage-processes          # Suspicious process inspection

# System Hardening
sudo ./linuxh --apply ufw          # Configure UFW firewall
sudo ./linuxh --apply ssh          # Harden SSH configuration
sudo ./linuxh --apply sysctl       # Apply kernel hardening
sudo ./linuxh --apply services     # Disable legacy services

# Security Tools
sudo ./linuxh --apply install-tools # Install security tools
sudo ./linuxh --apply scans         # Run security scans
sudo ./linuxh --apply auto-updates  # Configure automatic updates

# Complete Suite
sudo ./linuxh --apply all          # Run all security modules
```

## 🔧 What Each Module Does

### 🔍 **Analyze Module**
- Scans network connections for suspicious ports
- Identifies processes on known malicious ports
- **Ports monitored**: 4444, 1337, 31337, 6667, 9999, 12345, 54321
- **Safety**: Read-only, non-destructive

### ⚠️ **Manage Processes**
- Finds processes listening on suspicious ports
- Optionally kills suspicious processes
- Can remove malicious executables
- Creates backups before any removal

### 🔥 **UFW Firewall**
- Installs and configures UFW firewall
- Sets default deny incoming, allow outgoing
- Configures essential ports (SSH, HTTP, HTTPS)
- Can detect and allow running services

### 🔒 **SSH Hardening**
- Disables root login
- Disables password authentication (keys only)
- Sets MaxAuthTries to 3
- Disables X11 forwarding
- Tests configuration before applying

### ⚙️ **Kernel Hardening**
- Applies security-focused sysctl settings
- Disables IP forwarding and redirects
- Enables TCP syncookies
- Restricts kernel pointer access

### 🚫 **Service Management**
- Disables legacy insecure services:
  - telnet.socket
  - rsh.socket  
  - rexec.socket
  - rlogin.socket

### 🛠️ **Security Tools**
Installs and configures:
- **fail2ban** - Intrusion prevention
- **clamav** - Antivirus scanning
- **rkhunter** - Rootkit detection
- **chkrootkit** - Rootkit scanning

### 🔍 **Security Scans**
- Runs rkhunter rootkit scan
- Runs chkrootkit scan
- Updates virus definitions
- Non-destructive scanning

### 🔄 **Auto Updates**
- Configures automatic security updates
- Supports APT (unattended-upgrades)
- Supports YUM/DNF (dnf-automatic/yum-cron)

## 🎨 Features in Detail

### 🛡️ Safety First Design
```bash
# Default behavior: SAFE dry-run
./linuxh ssh                    # Shows what would be done
./linuxh --dry-run ssh         # Explicit dry-run (same as above)

# Explicit apply required for changes
sudo ./linuxh --apply ssh      # Actually makes changes
```

### 📝 Comprehensive Logging
- All actions logged to `/var/log/linuxh_security.log`
- Timestamped entries
- Both success and error logging
- Dry-run commands logged for audit

### 💾 Smart Backups
- Automatic backup before file modifications
- Customizable backup directory
- Timestamped backup files
- Backup restoration on errors

### 🎯 Interactive Prompts
```bash
Disable root login? [y/N]: y
Disable password authentication? [y/N]: y
Apply these SSH changes? [y/N]: y
```

### 🌐 Cross-Distribution Support
- **Debian/Ubuntu**: APT package manager
- **RedHat/CentOS**: YUM package manager  
- **Fedora**: DNF package manager
- **Arch Linux**: Pacman package manager
- **OpenSUSE**: Zypper package manager

## ⚠️ Important Warnings

### 🚨 Critical Safety Notes
1. **ALWAYS test with `--dry-run` first**
2. **SSH hardening may lock you out** if key-based auth isn't setup
3. **Some changes require reboot** to take full effect
4. **Backup important data** before applying changes
5. **Test on non-production systems** first

### 🔐 SSH Hardening Warning
The SSH module will:
- ❌ Disable root login
- ❌ Disable password authentication (keys only)
- ❌ Reduce MaxAuthTries to 3
- ❌ Disable X11 forwarding

**Ensure you have SSH key authentication working before applying!**

## 🐛 Troubleshooting

### Common Issues & Solutions

**Permission Denied Errors**
```bash
# Use sudo for apply mode
sudo ./linuxh --apply ssh

# Ensure log directory exists
sudo mkdir -p /var/log
```

**SSH Service Not Found**
```bash
# Check available services
systemctl list-unit-files | grep -i ssh

# Install SSH server if needed
sudo apt update && sudo apt install openssh-server
```

**Package Manager Detection Issues**
```bash
# Manual package manager override
sudo ./linuxh --apply install-tools
# Script auto-detects: APT, YUM, DNF, Pacman, Zypper
```

### Log Files and Debugging
```bash
# Check execution logs
sudo tail -f /var/log/linuxh_security.log

# Check backup directory
sudo ls -la /root/security_backup_*/

# Dry-run to debug
./linuxh --dry-run all
```

## 📊 Example Output

### Dry-run Mode
```
[DRY-RUN] ufw --force reset
[DRY-RUN] ufw default deny incoming
[DRY-RUN] ufw allow 22
✅ No suspicious connections found.
⚠ Found 2 suspicious processes
```

### Success Messages
```
✅ SSH config applied and sshd restarted
✅ UFW enabled with configured rules  
✅ Kernel settings applied
✅ Security tools installed successfully
```

## 🔮 Advanced Usage

### Custom Configuration
Edit the script to modify default values:
```bash
# Customize suspicious ports
SUSPICIOUS_PORTS=(4444 1337 31337 6667 9999 12345 54321 8080 8443)

# Customize essential ports  
ESSENTIAL_PORTS=(22 80 443 53 993 995)

# Change default locations
BACKUP_DIR_DEFAULT="/opt/security_backups"
LOG_FILE_DEFAULT="/var/log/custom_security.log"
```

### Integration with Other Tools
```bash
# Run as part of deployment script
sudo ./linuxh --apply --yes ufw
sudo ./linuxh --apply --yes sysctl
sudo ./linuxh --apply --yes auto-updates

# Schedule regular security scans
0 2 * * * /path/to/linuxh --apply scans
```

## 🤝 Contributing

We welcome contributions! Here's how:

1. **Fork the repository**
   ```bash
   git clone https://github.com/passioncoder5/linuxh.git
   cd linuxh
   ```

2. **Create a feature branch**
   ```bash
   git checkout -b feature/new-security-module
   ```

3. **Make your changes and test**
   ```bash
   ./linuxh --dry-run all
   sudo ./linuxh --apply your-module
   ```

4. **Submit a pull request**

### Areas for Contribution
- New security modules
- Additional package manager support
- Enhanced detection capabilities
- Documentation improvements
- Bug fixes and optimizations


## 🆘 Support

- **GitHub Issues**: [Report bugs or request features](https://github.com/passioncoder5/linuxh/issues)
- **Security Concerns**: Please report security vulnerabilities privately
- **Documentation**: Check this README and script comments

## 🌟 Star History

If you find this tool useful, please consider giving it a star on GitHub!

---

**Remember**: Security is a process, not a product. Always test changes in a safe environment before deploying to production systems.

**Stay safe, stay secure!** 🔐
