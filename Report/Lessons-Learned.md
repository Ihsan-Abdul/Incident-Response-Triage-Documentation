# Lessons Learned

## Summary

This document reviews what worked well during the response to this incident, where the gaps were, and what changes would reduce the impact of a similar campaign in the future.

## What Worked Well

1. **The detections built for this campaign caught it.** The Firewall Brute Force Success and Windows Admin External Login alerts both fired correctly and pointed the investigation toward the right systems immediately.
2. **Correlating three log sources reconstructed the full attack chain.** Reviewing the firewall, Windows, and Linux logs together, rather than individually, is what confirmed this was a single coordinated campaign using three rotating source IPs, rather than three unrelated events.
3. **Containment was fast once the alert fired.** All three attacker IPs were blocked and the affected accounts were disabled within an hour of detection.

## What Needs Improvement

1. **Four days of compromise went undetected before the first alert fired.** The brute force attempts succeeded every day from January 8 onward, but nothing flagged the activity until January 12. The detections were tuned correctly; they simply were not built or enabled early enough to catch the very first successful attempt.
2. **Two additional days of activity were only discovered after the fact.** The root-level activity on the Linux server that continued through January 14 was found by extending the log review after containment, not by an alert. A detection that specifically watches for continued activity from an account already flagged as compromised would have caught this without requiring a manual review.
3. **Non-standard port usage delayed the Linux investigation.** The attacker's use of ports 4444 and 4445 instead of port 22 meant that a detection scoped only to the standard SSH port would have missed the activity entirely.
4. **No account lockout policy was in place on either system.** This is the single largest factor in why the brute force attempts succeeded consistently across seven days instead of being blocked after a handful of attempts.

## Recommended Changes

1. **Lower the threshold for administrative account monitoring.** An external login to an account like `admin` should generate an alert on the first occurrence, not only after a pattern of failed attempts.
2. **Build a detection for continued activity from a previously flagged account or IP**, so that residual activity after an initial compromise, like the two extra days observed here, is caught automatically rather than found during a manual timeline review.
3. **Monitor authentication activity across all ports, not only the well-known port for a given service.** This campaign would have been caught two days earlier on Linux if SSH monitoring had not assumed all activity would appear on port 22.
4. **Enable account lockout policies on both Windows and Linux** after three failed login attempts.
5. **Tag related alerts with a shared incident identifier** when they fire close together, such as the Firewall Brute Force Success and Windows Admin External Login alerts in this case, so an analyst sees them as one investigation from the start rather than two separate alerts to piece together manually.

## Process Improvements

- **Detection coverage should be validated against a "first attempt" scenario, not only a repeated pattern.** Several of the detections in this project rely on a threshold, such as five failed logins, which means the very first attempt in a new campaign will never be caught by that rule alone. Complementary detections that flag any successful login from a new or unrecognized source IP would help close this gap.
- **Historical log review should be a standard step in every investigation, not an optional one.** The two additional days of Linux activity were only found because the investigation extended its review window back further than the alert's original trigger point. Making that extended review a default step, rather than something added after the fact, would catch this kind of residual activity sooner.

---
