# Lessons Learned

## Summary
Key findings and improvements identified from investigating the five-day attack campaign. Focuses on what worked, what didn't, and how to do better next time.

## What Worked Well

### 1. Alert Configuration
**Success:** Custom Splunk rules correctly detected the attack patterns when they fired.
**Evidence:** All 3 critical alerts triggered on Day 5:
- Firewall Bruteforce Success (High)
- Windows Admin External Login (Critical)  
- Linux SSH Root Attack (Critical)

### 2. Timeline Reconstruction
**Success:** Successfully connected events across 3 systems using common data points.
**Evidence:** Correlated by:
- Same IPs (203.0.113.88, .45, 198.51.100.77)
- Same timing (10:00 AM SSH, 3:00 PM RDP daily)
- Same patterns (6-8 fails then success)

## What Needs Improvement

### 1. Detection Timing
**Problem:** Attack succeeded for 4 days before alerts fired.
**Data:** 
- Days 1-4: Successful compromises, no alerts
- Day 5: Alerts finally triggered
**Lesson:** Lower thresholds for admin account monitoring.

### 2. Missed Connections
**Problem:** Looked at Windows, Linux, and firewall logs separately at first.
**Data:** Same attacker IPs hitting all 3 systems, but checked individually.
**Lesson:** Build correlation searches that check all systems for same IPs simultaneously.

### 3. Port Evasion
**Problem:** Standard SSH monitoring on port 22 missed attacks on 4444/4445.
**Data:** All SSH attacks used port 4444 for fails, 4445 for success.
**Lesson:** Monitor SSH authentication behavior, not just port 22.

## Immediate Actions for Next Time

### 1. Faster Investigation
**Current:** Took 20 minutes to connect all dots.
**Goal:** Under 10 minutes for similar attacks.
**Method:** Pre-built search that checks all logs for new malicious IPs.

### 2. Better Alert Order
**Current:** Multiple alerts fired around same time.
**Goal:** Clear priority order for triage.
**Method:** Tag alerts with investigation steps (1. Check firewall, 2. Check Windows, 3. Check Linux).

### 3. Document as You Go
**Current:** Built timeline after investigation.
**Goal:** Build timeline during investigation.
**Method:** Use notepad/document during initial triage.

## Prevention Recommendations

### 1. Account Security
**Problem:** No lockout policies enabled brute force.

**Lesson:** Lock accounts after 3 failed attempts.

### 2. Access Controls
**Problem:** Admin services exposed directly to internet.

**Lesson:** Require VPN for RDP/SSH access.

### 3. Monitoring Gaps
**Problem:** No alert for external admin logins.

**Lesson:** Critical alert for any admin login from outside.


## Conclusion
This investigation showed that basic detection rules work, but need better timing and correlation. The biggest lesson: attacks don't happen in isolation - connecting events across systems is key to seeing the full picture.

For any other similar attack, detection should happen on Day 1 not Day 5.

---
