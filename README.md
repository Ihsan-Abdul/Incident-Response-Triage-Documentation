# Incident Response Triage Documentation

## Overview
This project shows my hands-on investigation of a security incident using simulated logs. I analyzed what happened, documented the attack timeline, and determined what response actions would be needed.

## What I Did

### 1. Incident Analysis
I investigated a five-day attack campaign that started with a "Firewall Bruteforce" alert. My analysis showed:
- Attackers compromised both Windows and Linux systems
- They gained admin/root access through brute force attacks
- The same attack happened daily for 5 consecutive days

### 2. Timeline Reconstruction  
I built a detailed timeline showing exactly when attacks happened:
- **January 8-12**: Daily attacks at 10:00 AM (SSH) and 3:00 PM (RDP)
- **Three attacker IPs**: 203.0.113.88, 203.0.113.45, 198.51.100.77
- **Compromised systems**: Windows server 192.168.1.21 and Linux server 192.168.1.100

### 3. Response Planning
Based on my findings, I documented what response actions would be needed:
- **Containment**: Block attacker IPs, isolate compromised systems
- **Eradication**: Reset compromised passwords, check for backdoors
- **Recovery**: Implement better security controls to prevent future attacks

### 4. Lessons Learned
I identified what worked well and what could be improved:
- **What worked**: My detection rules caught the attack when they fired
- **What needs work**: Detection should happen on Day 1, not Day 5

## Project Files

### Report Documents
- [Executive Summary](Report/Executive-Summary.md) - What happened and why it matters
- [Investigation Timeline](Report/Investigation-Timeline.md) - When attacks happened, step by step
- [Response Actions](Report/Response-Actions.md) - What to do to fix the problem
- [Lessons Learned](Report/Lesson-Learned.md) - What I learned and how to improve

## Skills Demonstrated
- Analyze security logs to understand what happened
- Build clear timelines of security incidents
- Determine appropriate response actions
- Document findings professionally
- Learn from investigations to improve security

## Related Project
This investigation started with detection rules I built in my [Security Monitoring Project](https://github.com/Ihsan-Abdul/Security-Monitoring-Alert-Triage-Splunk-SIEM). That project shows how I created the alerts that detected this attack.

## Note
This work uses simulated log data for security operations training. It demonstrates investigation and analysis skills in a controlled environment.

---
