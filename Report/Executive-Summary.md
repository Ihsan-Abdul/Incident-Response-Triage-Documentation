# Incident Response Triage: Executive Summary

## Summary
This document details my triage and analysis of a security incident that began with an alert from a custom Splunk detection I engineered. After receiving a "Firewall Bruteforce" alert, I conducted a structured investigation that showcased a **five-day, coordinated attack campaign**. I confirmed the attackers had successfully compromised credentials through brute force on both Windows and Linux systems, escalated privileges, and attempted lateral movement. I executed immediate containment actions, including blocking malicious IPs and isolating affected hosts. The next steps taken would be to escalated the incident to the Incident Response team for further forensic examination. This report captures my findings, actions, and recommendations aquired throug this hands-on investigation.

## Incident Overview
*   **Attack Duration:** Five consecutive days (January 8-12).
*   **Attack Detection:** January 12, 2024 (Day 5 of the campaign).
*   **Detection Trigger:** My custom Splunk correlation rule for "multiple failed connections followed by success."
*   **Primary Attack Vector:** Credential-based brute force attacks against internet-facing SSH (port 22/4444) and RDP (port 3389) services.
*   **Impacted Systems:** One Windows Server (`192.168.1.21`) and one Linux Server (`192.168.1.100`).
*   **Attacker Goal:** Initial access, privilege escalation to administrative accounts, and attempted lateral movement toward a domain controller.
*   **Final Status:** **Contained & Escalated.** The immediate threat was neutralized on January 12. My investigation into previous logs confirmed prior breaches, dating back 4 days before detection. No evidence of successful data exfiltration was found.

## 3. Root Cause Analysis
Based my investigation, the root cause was identified as **inadequate access controls on external-facing management services**.
*   **Weak Authentication Policies:** Absence of account lockout mechanisms and the use of weak passwords allowed brute-force attacks to succeed with a 100% success rate in the simulated environment.
*   **Excessive Network Exposure:** Critical RDP and SSH services were directly exposed to the internet without secondary authentication controls, like Multi-Factor Authentication (MFA).

## 4. Impact Assessment
I assessed the impact of this incident as **HIGH** driven primarily by a complete loss of integrity on the affected systems.
*   **Integrity:** **HIGH.**  Attackers gained root and administrator-level access, resulting in a total loss of trust in the compromised systems. They had the capability to install persistent malware, create backdoors, and tamper with logs.
*   **Confidentiality:** **LOW.** Administrative credentials were compromised, but the affected systems were lab environments containing no sensitive production data.
*   **Availability:** **LOW.** The attacker did not execute availability attacks such as ransomware
*   **Scope:** **LIMITED.** The attack was contained to two specific servers. My investigation found no evidence of successful lateral movement at the time of response.
*   The compromise of administrative credentials and the resulting full system control represents a severe security event. The limited scope and lab environment reduce the business consequences but do not diminish the critical technical severity of the breach, which would necessitate immediate containment and extensive remediation in a production setting.

## 5. Key Recommendations
Based on my findings during the triage process, I recommend the following immediate actions to prevent recurrence:
1.  **Implement Account Lockout Policy:** Enforce a lockout after 3 failed authentication attempts to mitigate brute-force attacks.
2.  **Enforce Multi-Factor Authentication (MFA):** Require MFA for all remote access to administrative services.
3.  **Reduce Attack Surface:** Require a VPN for administrative access instead of directly exposing ports to the internet.
4.  **Enhance Monitoring:** Expand detection rules to alert on anomalous administrative account activity like logins from external IPs and logins at unusual times to catch malicious behavior faster.

---
**Note:** This summary is based on my analysis of simulated log data within a controlled lab environment, conducted for the purpose of developing and demonstrating security operations competencies. 
