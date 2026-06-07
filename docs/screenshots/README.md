# Screenshots — Evidence of Working SOC Pipeline

## Required Screenshots to Add Here

Upload the following screenshots to this folder and they will appear in the docs automatically.

### 1. alerts-firing.png
**Where to take it:** Kibana → Security → Alerts
**What it should show:** All 10 alerts visible — SSH Brute Force (High), Reverse Shell (High), Suspicious Sudo Usage (Medium x8)
**Why it matters:** Proves your detection rules caught real attack simulations

### 2. detection-rules-page.png
**Where to take it:** Kibana → Security → Rules → Detection Rules (SIEM)
**What it should show:** All 5 rules listed, all showing "Succeeded" in Last response column
**Why it matters:** Shows professional SIEM rule authoring with MITRE mapping

### 3. soc-dashboard.png
**Where to take it:** Kibana → Dashboards → Project Sentinel — SOC Overview
**What it should show:** All 4 panels — event count metric, failed login line chart, top IPs table, severity pie chart
**Why it matters:** Shows you can build operational dashboards a Tier 1 analyst would use

### 4. ssh-brute-force-alert.png
**Where to take it:** Kibana → Security → Alerts → click on SSH BRUTE Force Detection alert
**What it should show:** Full alert detail — timestamp, rule name, severity, risk score, raw event data
**Why it matters:** Shows you can investigate and triage individual alerts

### 5. reverse-shell-alert.png
**Where to take it:** Kibana → Security → Alerts → click on Reverse shell detection alert
**What it should show:** Full alert detail showing process.args with bash -i pattern
**Why it matters:** Your most impressive detection — shows advanced process monitoring

## How to Add Screenshots

1. Take screenshots on your Mac (Cmd+Shift+4)
2. Go to github.com/MackydLow/Project-Sentinel
3. Navigate to docs/screenshots/
4. Click Add file → Upload files
5. Upload all screenshots with the exact filenames above
