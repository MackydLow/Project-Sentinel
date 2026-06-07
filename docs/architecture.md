# Architecture — Project Sentinel

## Overview

A full SOC detection pipeline built on a single Kali Linux VM running in UTM on Apple Silicon (M-series Mac). All components communicate locally over localhost.

## System Specs

- **Host:** MacBook Air M-series
- **VM:** Kali Linux (ARM64) via UTM
- **RAM allocated:** 4GB
- **Storage:** 27GB
- **Network:** Host-only mode (isolated) for attack simulations

## Pipeline

Attack Tools Log Sources Pipeline SIEM (Hydra, nmap) ---> /var/log/auth.log --> Filebeat ---> Elasticsearch /var/log/syslog --> Auditbeat ---> Kibana Network traffic --> Suricata ---> Detection Rules System metrics --> Metricbeat ---> Alerts Dashboard

## Components

| Component | Version | Port | Purpose |
|-----------|---------|------|---------|
| Elasticsearch | 8.19.16 | 9200 | Data store and search engine |
| Kibana | 8.19.16 | 5601 | SIEM UI, dashboards, detection rules |
| Filebeat | 8.19.16 | — | Ships auth and system logs |
| Auditbeat | 8.19.16 | — | Ships process and syscall events |
| Metricbeat | 8.19.16 | — | Ships system metrics |
| Suricata | Latest | — | Network IDS, generates alerts |

## Screenshot

![Architecture Overview](screenshots/detection-rules-page.png)
> Screenshot showing all 5 detection rules active in Kibana SIEM

## Key Design Decisions

- Security enabled on Elasticsearch to unlock Kibana SIEM detection engine
- SSL disabled intentionally (home lab — not production)
- Host-only networking used during attack simulations to prevent accidental external traffic
- Auditbeat used for process monitoring (required for reverse shell detection)
