# Investigation Timeline

## Summary

This timeline reconstructs the attack campaign identified through the detections built in the companion [Security Monitoring & Alert Triage with Splunk SIEM](https://github.com/Ihsan-Abdul/Security-Monitoring-Alert-Triage-Splunk-SIEM) project. It correlates firewall, Windows, and Linux logs to trace the attack from initial access through to detection and containment.

## Timeline Overview

| Date | Time | Event | System | Source IP | Evidence |
|------|------|-------|--------|-----------|----------|
| Jan 8 | 10:00 | SSH brute force begins, root targeted | Linux | 203.0.113.88 | Firewall DENY logs, port 4444 |
| Jan 8 | 10:00:15 | Successful SSH login as root | Linux | 203.0.113.88 | SSH auth log, port 4445 |
| Jan 8 | 15:00 | RDP brute force begins against 'john' | Windows | 203.0.113.88 | Event ID 4625 |
| Jan 8 | 15:06 | Successful RDP login as 'john' | Windows | 203.0.113.88 | Event ID 4624 |
| Jan 8 | 15:07 | 'admin' account login from external IP | Windows | 203.0.113.88 | Event ID 4672 |
| Jan 8 | 15:09 | Encoded PowerShell execution | Windows | — | Event ID 4688 |
| Jan 8 | 15:12 | PsExec connection attempt toward domain controller (WIN-DC) | Windows | — | Process creation log, plaintext credentials in command line |
| Jan 9 | 10:00 / 15:00 | Same pattern repeats | Linux / Windows | 203.0.113.45 | Firewall and auth logs |
| Jan 10 | 10:00 / 15:00 | Same pattern repeats | Linux / Windows | 203.0.113.88 (SSH), 203.0.113.45 (RDP) | Firewall and auth logs |
| Jan 11 | 10:00 / 15:00 | Same pattern repeats | Linux / Windows | 198.51.100.77 | Firewall and auth logs |
| Jan 12 | 10:00 / 15:00 | Final brute force activity against the firewall and Windows | Linux / Windows | 198.51.100.77 (SSH), 203.0.113.88 (RDP) | Firewall and auth logs |
| Jan 12 | 15:05 | Firewall Brute Force Success alert triggers | Firewall | — | Splunk alert |
| Jan 12 | 15:07 | Windows Admin External Login alert triggers | Windows | — | Splunk alert |
| Jan 12 | 15:10 | Investigation begins | All systems | — | Analyst review |
| Jan 12 | 15:45 | Containment executed against all three known attacker IPs | Network | 203.0.113.88, 203.0.113.45, 198.51.100.77 | Firewall block rules |
| Jan 13–14 | 10:00 | Root account activity continues on Linux, using access already established | Linux | 198.51.100.77 | SSH auth log |

## Daily Attack Pattern

### Day 1 (January 8) — Initial Compromise

**Morning, 10:00 AM:**
- Eight failed SSH login attempts against the root account on port 4444.
- A successful login on port 4445, fifteen seconds after the last failure.
- Root access established on the Linux server.

**Afternoon, 3:00 PM:**
- Six failed RDP login attempts against the `john` account.
- A successful login on the seventh attempt.
- Seven minutes later, the `admin` account authenticates from the same external IP.
- Two minutes after that, PowerShell executes with an encoded command.
- Three minutes after that, a PsExec connection is attempted toward the domain controller, using credentials visible in plaintext in the command line.

### Days 2 through 4 (January 9–11) — Continued Activity

- The same two attack windows repeat daily: SSH against root at 10:00 AM, RDP against `john` at 3:00 PM.
- The attacker rotates across three source IPs (`203.0.113.88`, `203.0.113.45`, `198.51.100.77`) rather than reusing one, which is consistent with an attempt to avoid a detection rule built around a single IP.
- Every attempt across these three days succeeds. No lockout or blocking action interrupts the pattern.

### Day 5 (January 12) — Detection and Containment

- The same attack pattern runs one final time against the firewall and Windows server.
- **15:05:** the Firewall Brute Force Success alert triggers.
- **15:07:** the Windows Admin External Login alert triggers.
- **15:10:** investigation begins, correlating the two alerts against the full set of firewall, Windows, and Linux logs.
- **15:45:** all three known attacker IPs are blocked at the firewall.

### Days 6 and 7 (January 13–14) — Residual Linux Activity

- Root-level activity continues on the Linux server for two additional days beyond the firewall containment, using the access already established on January 8 through 12 rather than a fresh brute force attempt against the firewall.
- This detail was found during the timeline reconstruction, after the initial containment, and is the basis for the detection gap discussed below.

## Investigation Methodology

The detections used to identify and investigate this campaign were built in the companion [Security Monitoring & Alert Triage with Splunk SIEM](https://github.com/Ihsan-Abdul/Security-Monitoring-Alert-Triage-Splunk-SIEM) project:

1. **Firewall Brute Force Success** — detected repeated denied connections followed by an allowed connection to SSH or RDP.
2. **Windows RDP Brute Force** — identified five or more failed logins within a 10-minute window.
3. **Windows Admin Login from External IP** — flagged any administrative account login from outside the internal network.
4. **Linux SSH Root Attack** — detected a successful root login following a cluster of failed attempts.

The full detection catalog, including the SPL queries and their configuration, is documented in [Detections](https://github.com/Ihsan-Abdul/Security-Monitoring-Alert-Triage-Splunk-SIEM/tree/main/Detections).

## Key Findings

1. **Days 1 through 4 produced no alerts, despite successful compromise on both systems each day.** The detections that eventually fired were tuned correctly, but nothing was in place to catch the very first successful attempt.
2. **Port evasion delayed detection on Linux.** The attacker used ports 4444 and 4445 for SSH rather than the standard port 22, which meant a detection rule scoped only to port 22 would have missed this activity entirely.
3. **Administrative login monitoring caught the escalation, but only after the fact.** The external admin login alert fired correctly on January 12, but the same login pattern had already succeeded on the four days prior without triggering a review.
4. **Correlation across systems was done manually, after the alert fired.** The firewall, Windows, and Linux logs were reviewed separately before being connected into a single timeline, rather than being correlated automatically at the time each event occurred.

## Conclusion

This timeline shows a coordinated, seven-day campaign against a firewall, a Windows server, and a Linux server, using the same three source IPs and the same daily timing throughout. The investigation reconstructed the full attack chain, identified the gap that allowed four days of undetected compromise, and supported the containment actions taken on January 12. The two additional days of Linux activity discovered afterward are addressed in the response actions and lessons learned that follow.

---
