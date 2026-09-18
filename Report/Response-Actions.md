# Response Actions

## Summary

This document covers the containment, eradication, and recovery actions taken in response to the campaign described in the [Investigation Timeline](Investigation-Timeline.md). It follows the containment, eradication, and recovery stages of the NIST Incident Response lifecycle.

## Containment

**Immediate actions, taken within the first hour of detection on January 12:**

1. Blocked all three identified attacker IPs at the firewall: `203.0.113.88`, `203.0.113.45`, and `198.51.100.77`.
2. Disabled the compromised `john` account on the Windows server to stop further use of the credential.
3. Disabled the `admin` account pending a credential reset, since it had authenticated from an external IP.
4. Isolated the Linux server from the network while root-level activity was reviewed.

**Short-term actions, taken within the first 24 hours:**

1. Reviewed the domain controller (`WIN-DC`) for any sign that the attempted PsExec connection succeeded. No confirmed compromise of the domain controller was found based on the available process and authentication logs.
2. Reviewed outbound network traffic from both compromised systems for signs of data exfiltration. No confirmed exfiltration was identified in the available logs.
3. Extended the log review back to January 8, which is what surfaced the four days of undetected compromise prior to the alert.

## Eradication

1. Reset credentials for the `john` and `admin` accounts on Windows, and for the `root` account on Linux.
2. Reviewed both systems for unauthorized scheduled tasks, cron jobs, or startup entries that could indicate the attacker had established persistence.
3. Reviewed the Windows server for any additional accounts created during the compromise window. None were found.
4. Patched and hardened the SSH configuration on the Linux server, removing the non-standard ports (4444 and 4445) that had been reachable during the attack.

## Recovery

1. Restored the `john` and `admin` accounts on Windows with new credentials and required a password reset at next login.
2. Returned the Linux server to the network after confirming no persistence mechanism was present and after credentials were rotated.
3. Enabled account lockout after three failed attempts on both Windows and Linux, closing the gap that had allowed the brute force attempts to succeed without interruption.
4. Verified that the four detections built for this campaign, covering firewall brute force, Windows brute force, external admin login, and Linux root compromise, all continued to function correctly after the affected systems were restored.

## Escalation

This incident was escalated to Critical once root access on Linux and administrative access on Windows were both confirmed, consistent with the escalation criteria defined in the [Alert Triage Playbook](https://github.com/Ihsan-Abdul/Security-Monitoring-Alert-Triage-Splunk-SIEM/blob/main/Playbooks/Alert-Triage-Playbook.md). Two factors drove that decision: the confirmed compromise of an administrative account, and the attempted lateral movement toward a domain controller. Either one on its own would have been treated as High severity; together, they met the threshold for a Critical incident requiring immediate containment rather than continued monitoring.

---
