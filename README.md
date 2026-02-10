# Incident Investigation: Multi-Host Brute Force Campaign

## Executive Summary
A simulated five-day brute force campaign targeting RDP and SSH services was investigated following a Splunk alert. Analysis identified three threat actor IPs compromising administrative accounts on Windows (`192.168.1.21`) and Linux (`192.168.1.100`) hosts. This document outlines the forensic timeline, impact assessment, and recommended containment actions aligned with the NIST Incident Response lifecycle.

## Key Findings
- **Attack Vectors:** Sustained RDP (TCP/3389) and SSH (TCP/22) brute force attacks.
- **Threat Actor Infrastructure:** Source IPs `203.0.113.88`, `203.0.113.45`, `198.51.100.77`.
- **Compromise Scope:** Administrative privileges achieved on both Windows (`Administrator`) and Linux (`root`) targets.
- **Attack Pattern:** Daily bi-modal pattern (1000h SSH, 1500h RDP) from January 8-12.

## Investigation & Analysis
- **Timeline Reconstruction:** Correlated firewall, Windows Security, and Linux auth logs to construct a minute-by-minute attack timeline.
- **Impact Analysis:** Determined credential compromise and established potential for lateral movement.
- **Detection Gap Analysis:** Reviewed alert logic to identify opportunities for earlier detection (reducing time-to-detect from T+5 days to T+1 day).

## Contained Response Playbook
Actions documented follow the NIST IR phases (Containment, Eradication, Recovery):
1.  **Immediate Containment:** Network-level blocking of threat actor IPs; isolation of compromised hosts.
2.  **Eradication Procedures:** Credential resets; forensic disk review for persistence mechanisms.
3.  **Recovery & Hardening:** Implementation of account lockout policies, JEA, and network segmentation rules.

## Artifacts & Documentation
- [Forensic Timeline](Report/Investigation-Timeline.md) (Log-based event reconstruction)
- [Response Action Registry](Report/Response-Actions.md) (Mapped to NIST CSF)
- [Retrospective & SIEM Tuning Recommendations](Report/Lesson-Learned.md)

## Skills Demonstrated
- **Log Forensics:** Analysis of Windows Security, Linux auth.log, and firewall logs.
- **Incident Documentation:** Production of executive and technical reports.
- **Response Planning:** Development of actionable containment and eradication steps.
- **Process Improvement:** Translating investigative findings into detection engineering recommendations.

---
*Note: This investigation was conducted in a controlled lab environment using simulated log data for skill development.*
