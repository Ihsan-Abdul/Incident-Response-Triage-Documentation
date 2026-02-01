# Investigation Timeline

## Summary
This timeline reconstructs the five-day attack campaign I discovered through log analysis. It documents the attacks timeline from  access attempts to containment, correlating events across Windows, Linux, and firewall systems.

## Timeline Overview
| Date | Time | Event | System | Source IP | Evidence |
|------|------|-------|--------|-----------|----------|
| Jan 8 | 10:00 | SSH brute force begins | Linux | 203.0.113.88 | Firewall DENY logs |
| Jan 8 | 10:02 | Successful SSH as root | Linux | 203.0.113.88 | SSH auth.log |
| Jan 8 | 15:00 | RDP brute force begins | Windows | 203.0.113.88 | Event ID 4625 |
| Jan 8 | 15:06 | Successful RDP as 'john' | Windows | 203.0.113.88 | Event ID 4624 |
| Jan 8 | 15:07 | Admin login external IP | Windows | 203.0.113.88 | Event ID 4672 |
| Jan 8 | 15:09 | PsExec lateral movement | Domain Controller | 192.168.1.50 | Process Creation |
| Jan 9 | 10:00 | SSH brute force | Linux | 203.0.113.45 | Same pattern |
| Jan 9 | 15:00 | RDP brute force | Windows | 203.0.113.45 | Same pattern |
| Jan 12 | 15:00 | Final RDP attack | Windows | 198.51.100.77 | Alert triggered |
| Jan 12 | 15:30 | Investigation begins | All systems |  | Correlation |
| Jan 12 | 15:45 | Containment completed | Network | All attacker IPs | Blocked |

## Daily Attack Pattern

### Day 1 (January 8) - Initial Compromise
**Morning (10:00 AM):**
- 8 failed SSH login attempts to root on port 4444
- Successful login on port 4445 
- Root access established on Linux server

**Afternoon (3:00 PM):**
- 6 failed RDP login attempts against 'john' account
- Successful login on 7th attempt
- Within 7 minutes: Admin account accessed from external IP
- Within 9 minutes: PsExec attempted toward domain controller

### Day 2-4 (January 9-11) - Persistence
- Same attacks 10:00 AM SSH, 3:00 PM RDP
- Rotating between IPs 203.0.113.45, 198.51.100.77
- 100% success rate on brute force attempts

### Day 5 (January 12) - Detection & Response
**Alert Timeline:**
- **15:05**: "Firewall Bruteforce Success" alert 
- **15:05**: "Windows RDP Bruteforce" alert 
- **15:07**: "Windows Admin External Login" alert 
- **15:10**: Investigation initiated
- **15:30**: Full attack chain discovered
- **15:45**: Containment executed

## Investigation Methodology

### Detection Queries Used
I leveraged detection queries from my [Security Monitoring Project](https://github.com/Ihsan-Abdul/Security-Monitoring-Alert-Triage-Splunk-SIEM):

1. **Firewall Bruteforce Success** - Detected multiple DENY followed by ALLOW patterns  
2. **Windows RDP Bruteforce Detection** - Identified 5+ failed logins within 10 minutes
3. **Windows Admin External Login** - Alerted on admin account access from external IPs
4. **Linux SSH Root Attack** - Detected successful root access after brute force

* Queries available in [Detections catalog](https://github.com/Ihsan-Abdul/Security-Monitoring-Alert-Triage-Splunk-SIEM/tree/main/Detections)*

## Key Findings

### Detections Identified
1. **Days 1-4**: No alerts triggered although access was gained
2. **Port Evasion**: Standard port 22 monitoring missed 4444/4445 attacks
3. **Admin Monitoring**: External admin logins not alerted in real-time
4. **Correlation**: Isolated events not linked across systems initially


## Conclusion
This timeline showcases a consistent five-day attack campaign exploiting weak authentication. My investigation successfully reconstructed the complete attack chain, identified persistence patterns, and triggered appropriate containment actions upon detection.

---

*Note: This timeline is from simulated log data for showcasing security investigation methodologies.*
