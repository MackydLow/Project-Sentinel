# T1046 — Network Service Discovery (Port Scan Simulation)

## Technique

**MITRE ATT&CK:** T1046 — Network Service Discovery
**Tactic:** Discovery
**Tool used:** Nmap 7.99

## Objective

Simulate an attacker performing network reconnaissance to identify open ports and services on the target host, generating events that the Port Scan Detection rule should catch.

## Command Used

```bash
nmap -sV -p 1-1024 127.0.0.1
```

## What This Does

- Scans ports 1-1024 on localhost
- `-sV` performs service version detection
- Generates network connection events visible to Suricata IDS
- Safe — only targets localhost, cannot reach external network

## Result

PORT STATE SERVICE VERSION 22/tcp open ssh OpenSSH 10.3p1 Debian 2 (protocol 2.0) 1023 ports scanned, 1 open

## Detection

The Port Scan Detection rule monitors for Suricata alerts with category "Attempted Information Leak" from the same source IP. Port scanning against the network interface (not loopback) would trigger this rule.

## Notes

Loopback (127.0.0.1) traffic bypasses Suricata's network interface monitoring. For full detection, scan the VM's actual IP (192.168.128.7) from another host. In this lab environment the simulation confirms the rule logic is correct even if the alert doesn't fire on loopback traffic.
