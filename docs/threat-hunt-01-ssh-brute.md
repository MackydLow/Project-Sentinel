# Threat Hunt Case Study 01 — SSH Brute Force Attack

## Summary

On 2026-06-07 at 10:02 UTC, the SSH Brute Force Detection rule fired on host sentinel-soc. This documents the full investigation from alert triage to containment.

## Alert Details

- **Rule:** SSH Brute Force Detection
- **Severity:** High
- **Risk Score:** 73
- **MITRE:** T1110.001 — Brute Force: Password Guessing
- **Timestamp:** 2026-06-07 @ 11:16 BST

## Investigation Steps

### 1. Alert Triage
Kibana Security → Alerts showed a High severity alert. Rule fired after detecting more than 5 failed SSH authentication attempts matching `message:*failed*` in the system.auth dataset.

### 2. Log Analysis
Filtered Kibana Discover to `event.dataset:system.auth` and `message:*failed*`. Found 6 failed password attempts for user root from 127.0.0.1 within 6 seconds — consistent with automated password spraying.

### 3. Scope Assessment
Checked for any successful logins following the failed attempts. No `event.outcome:success` events found from the same source. Attack was contained — no successful authentication.

### 4. Attacker Behaviour
The attack used 6 common passwords (password, 123456, admin, kali, letmein, qwerty) — a classic low-sophistication spray attack using Hydra v9.7.

## MITRE ATT&CK Mapping

- **Tactic:** Credential Access
- **Technique:** T1110.001 — Brute Force: Password Guessing

## Verdict

Contained. No successful authentication. No lateral movement detected.

## Response

```bash
# Block the source IP with UFW
sudo ufw deny from 127.0.0.1 to any port 22
```

## Lessons Learned

- Detection rule successfully identified the attack within 1 minute of execution
- Rule required tuning — original KQL used structured fields (system.auth.ssh.event) which weren't populated in this Filebeat version. Updated to use raw message field matching.
- Threshold of 5 attempts in 1 minute is appropriate for this environment.
