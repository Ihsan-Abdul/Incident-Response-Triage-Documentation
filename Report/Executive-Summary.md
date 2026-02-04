# Incident Response Triage: Executive Summary

## Summary
This document details my analysis of a security incident that started with an alert from a Splunk detection I built. The "Firewall Bruteforce" alert led to my investigation of a **five-day attack campaign**. I found that attackers compromised credentials through brute force on Windows and Linux systems, escalated privileges, and tried lateral movement. My analysis showed what containment actions were needed, including blocking malicious IPs and isolating affected systems. This would be escalated to an Incident Response team for further work. This report shows my findings and recommendations from this investigation.

## Incident Overview
*   **Attack Duration:** Five days straight (January 8-12).
*   **Attack Detection:** January 12, 2024 (Day 5).
*   **Detection Trigger:** My Splunk rule for "multiple failed connections followed by success."
*   **Attack Method:** Brute force attacks against SSH (port 22/4444) and RDP (port 3389).
*   **Systems Affected:** Windows Server (`192.168.1.21`) and Linux Server (`192.168.1.100`).
*   **Attacker Goal:** Get access, become admin/root, try to move to other systems.
*   **Final Status:** **Contained & Escalated.** The immediate threat was stopped on January 12. My check of past logs showed this had been happening for 4 days already. No data was found to be stolen.

## Root Cause Analysis
My investigation found the main problem was **weak access controls on internet-facing management services**.
*   **No Account Protection:** No lockouts or strong passwords let brute-force attacks work every time.
*   **Too Much Exposure:** RDP and SSH were open to the internet without extra protection like MFA.

## Impact Assessment
I rated the impact as **HIGH** because the attackers got full control of the systems.
*   **Integrity:** **HIGH.** Attackers got admin and root access. They could install malware or change anything.
*   **Confidentiality:** **LOW.** Only lab system credentials were taken, no real data.
*   **Availability:** **LOW.** No ransomware or system takedowns happened.
*   **Scope:** **LIMITED.** Only two servers were hit. They didn't successfully move to other systems.

## Recommendations
Based on what I found, here's what should be done to stop this from happening again:
1.  **Add Account Lockout:** Lock accounts after 3 failed login attempts.
2.  **Require MFA:** Use multi-factor authentication for all admin access.
3.  **Use VPN:** Don't expose RDP/SSH directly to internet - require VPN first.
4.  **Better Monitoring:** Alert on admin logins from outside and SSH on unusual ports.

---
