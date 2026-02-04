# Response Actions

## Containment
**Based on the attack logs I analyzed:**

### Block Attacker IPs
- **203.0.113.88**: Attacked on January 8 & 12 (Windows & Linux)
- **203.0.113.45**: Attacked on January 9 & 10 (Windows & Linux)  
- **198.51.100.77**: Attacked on January 11 & 12 (Windows & Linux)
- Block all 3 IPs at the firewall

### Isolate Compromised Systems
- **192.168.1.21**: Windows server - admin account accessed
- **192.168.1.100**: Linux server - root account accessed  
- Both systems showed successful logins from attacker IPs

### Stop Data Exfiltration
- Block connections to **203.0.113.77:443**
- Logs show large transfers to this IP after each attack
- This happened at 15:12 daily after successful compromise

## Eradication  
**Based on what the logs show attackers did:**

### Remove Compromised Accounts
- **Windows**: 'john' and 'admin' accounts - passwords known to attackers
- **Linux**: 'root' account - password known to attackers  
- Reset all passwords on both systems

### Check for Backdoors
- **Windows**: Look for scheduled tasks created around attack times
- **Linux**: Check /root/.ssh/authorized_keys for new entries
- Both: Check for new files in system directories

### Remove Attacker Tools
- **Windows**: Look for encoded PowerShell scripts
- **Windows**: Check for PsExec.exe usage
- Both: Remove any unknown executables

## Recovery
**How to fix what was broken:**

### System Restoration
- Consider rebuilding both servers 
- Attackers had admin/root access for 5 days
- Systems can't be trusted without complete rebuild

### New Security Settings
- **Account lockout**: 3 failed attempts → lock for 30 minutes
- **MFA required**: For all admin/root access
- **VPN required**: No direct RDP/SSH from internet
- 
### Better Monitoring
- **Alert rule**: Any admin login from outside network = Critical
- **Alert rule**: SSH on non-standard ports (not 22) = High  
- **Correlation**: Link Windows and Linux alerts from same IP

## Evidence
**From the logs I analyzed:**
- Windows Event IDs: 4624, 4625, 4672, 4688
- Linux /var/log/auth.log with root login attempts
- Firewall logs showing attack patterns
- Timeline showing 5-day attack sequence

## Escalation
**This needs ro be escalated because:**
- Two systems fully compromised (admin/root access)
- Attack worked for 5 days before detection
- Lateral movement attempted (PsExec to domain controller)
- Data exfiltration attempted (data transfers)

---
