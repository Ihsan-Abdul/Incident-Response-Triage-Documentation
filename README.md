# Incident Response: Multi-Host Brute Force Campaign

## Overview

This project documents the incident response side of a security investigation that began with a Splunk alert built for the companion [Security Monitoring & Alert Triage with Splunk SIEM](https://github.com/Ihsan-Abdul/Security-Monitoring-Alert-Triage-Splunk-SIEM) project. A brute force campaign against RDP and SSH services was investigated after the Firewall Brute Force Success alert fired, revealing that the attack had already been running undetected for four days. This repository covers the timeline reconstruction, impact assessment, and response actions, structured around the NIST Incident Response lifecycle.

## Key Findings

- **Attack vectors:** brute force against RDP (port 3389) and SSH (port 22, with the attacker also using the non-standard ports 4444 and 4445).
- **Attacker infrastructure:** three rotating source IPs — `203.0.113.88`, `203.0.113.45`, and `198.51.100.77`.
- **Compromise scope:** administrative access on Windows (`admin`) and root access on Linux, on hosts `192.168.1.21` and `192.168.1.100`.
- **Attack duration:** seven days total. Brute force activity against the firewall and Windows ran for five days (January 8–12); root-level activity on Linux continued for two additional days (January 13–14) using access already established.
- **Detection gap:** the campaign ran undetected for four full days before the first alert fired on day five.

## Report Contents

- [Executive Summary](Report/Executive-Summary.md) — a high-level overview of the incident, its impact, and the recommended next steps.
- [Investigation Timeline](Report/Investigation-Timeline.md) — a day-by-day reconstruction of the attack, correlating firewall, Windows, and Linux logs.
- [Response Actions](Report/Response-Actions.md) — the containment, eradication, and recovery steps taken, following the NIST IR lifecycle.
- [Lessons Learned](Report/Lessons-Learned.md) — what worked, what the gaps were, and the detection and process changes recommended as a result.

## Investigation and Analysis

- **Timeline reconstruction:** correlated firewall, Windows Security, and Linux authentication logs to build a day-by-day attack timeline.
- **Impact analysis:** confirmed credential compromise on both systems and an attempted move toward a domain controller.
- **Detection gap analysis:** reviewed why the campaign ran for four days before the first alert fired, and what detection changes would close that gap.

## Skills Demonstrated

- **Log forensics:** analysis of Windows Security logs, Linux authentication logs, and firewall logs to reconstruct a single attack timeline across three systems.
- **Incident documentation:** production of an executive summary alongside detailed technical reports, written for two different audiences from the same underlying investigation.
- **Response planning:** development of containment, eradication, and recovery steps mapped to the NIST Incident Response lifecycle.
- **Process improvement:** translating investigation findings into specific, actionable detection engineering recommendations rather than generic advice.

## Related Project

The detections that triggered this investigation, along with the underlying SPL queries and their configuration, are documented in the companion repository: [Security Monitoring & Alert Triage with Splunk SIEM](https://github.com/Ihsan-Abdul/Security-Monitoring-Alert-Triage-Splunk-SIEM).

---
*This investigation was conducted in a controlled lab environment using simulated log data for skill development.*
