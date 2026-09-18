# Incident Response Triage: Executive Summary

## Summary

This report documents the investigation of a security incident that began with a Splunk alert built for the companion [Security Monitoring & Alert Triage with Splunk SIEM](https://github.com/Ihsan-Abdul/Security-Monitoring-Alert-Triage-Splunk-SIEM) project. The Firewall Brute Force Success alert triggered on January 12 and led to an investigation of a coordinated attack campaign that had been running against a firewall, a Windows server, and a Linux server since January 8. The investigation found that the attacker compromised credentials through brute force on both systems, escalated to administrative and root access, and attempted lateral movement toward a domain controller. This report covers the timeline, the impact of the compromise, and the containment, eradication, and recovery actions that would follow in a real environment.

## Incident Overview

- **Attack duration:** Seven days total. Brute force activity against the firewall and Windows server ran January 8 through January 12. Root-level activity on the Linux server continued through January 14, using access already established rather than a fresh brute force attempt.
- **Detection date:** January 12, 2024, on the fifth day of the campaign.
- **Detection trigger:** the Firewall Brute Force Success alert, built to catch a cluster of failed connections followed by a success within a 10-minute window.
- **Attack method:** brute force against SSH (port 22, with the attacker also using the non-standard port 4444) and RDP (port 3389).
- **Systems affected:** a Windows server (`192.168.1.21`) and a Linux server (`192.168.1.100`).
- **Attacker objective:** gain initial access, escalate to an administrative or root account, and attempt to move to additional systems, including a domain controller.
- **Final status:** contained and escalated. The active brute force activity was stopped on January 12, and a review of historical logs showed the campaign had already been running for four days prior to detection. No evidence of data exfiltration was found.

## Root Cause Analysis

The investigation identified weak access controls on internet-facing management services as the underlying cause.

- **No account lockout policy:** neither the Windows nor the Linux server locked an account after repeated failed login attempts, which allowed the brute force attempts to succeed on every day of the campaign.
- **Unrestricted exposure:** both RDP and SSH were reachable directly from the internet, with no VPN or additional layer of authentication required.

## Impact Assessment

The overall impact is rated **High**, since the attacker achieved full administrative control on both affected systems.

- **Integrity: High.** The attacker obtained administrator access on Windows and root access on Linux, which is sufficient to install software, modify configurations, or alter data on either system.
- **Confidentiality: Low.** Only lab credentials were exposed. No production or customer data was present in the environment.
- **Availability: Low.** No ransomware, service disruption, or system outage occurred during the campaign.
- **Scope: Limited.** Two systems were confirmed compromised. The attacker's attempt to reach a domain controller through PsExec did not result in a confirmed additional compromise based on the available logs.

## Recommendations

1. **Enable account lockout.** Lock an account after three failed login attempts on both Windows and Linux.
2. **Require multi-factor authentication** for all administrative access, on both systems.
3. **Remove direct internet exposure** for RDP and SSH. Require a VPN connection before either service is reachable.
4. **Expand monitoring** to alert on administrative account logins from external IPs and on SSH activity across all ports, not only port 22.

---
