# Sysmon Incident Triage SOP – Windows SOC Lab

## Purpose
This document outlines the steps to triage suspicious events in a Windows environment using Sysmon logs and Event Viewer.

## Objective
Help analysts investigate alerts by:
- Identifying suspicious behavior
- Correlating logs
- Making an escalation decision

---

## Step-by-Step Triage Workflow

### 1. Identify Alert Context
- What triggered this review? (e.g., high CPU, alert, user report)
- Log source: Event Viewer > Applications and Services Logs > Microsoft > Windows > Sysmon > Operational

---

### 2. Filter Key Event IDs
Filter for relevant events:
- **1** – Process creation
- **3** – Network connection
- **7** – DLL/Image loaded
- **11** – File creation
- **13/14** – Registry changes
- **22** – DNS queries

---

### 3. Investigate Event ID 1 – Process Creation
- Analyze the **command line**
- Check for:
  - PowerShell with `-enc`, `-nop`, or `bypass`
  - WMI (`wmic.exe`) or LOLBins
  - Unsigned or unusual paths (`C:\Users\Public\`, `%TEMP%`)

✅ Example:
```
powershell.exe -nop -w hidden -enc <base64>
```

---

### 4. Investigate Event ID 3 – Network Connection
- External IPs or ports like `4444`, `8080`, `3389`
- Look for connections to:
  - Suspicious domains (use VirusTotal)
  - Unusual ports
  - IPs not related to known infrastructure

---

### 5. Investigate Event ID 11 – File Creation
- Was a file dropped after process execution?
- Path is important:
  - `%TEMP%`, `C:\Users\Public\Downloads\`
- File extension (e.g., `.bat`, `.vbs`, `.ps1`, `.exe`)

---

### 6. Registry Change – Event ID 13/14
- Look for persistence techniques:
  - `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`
  - `RunOnce`, `Services`, `Image File Execution Options`

---

### 7. DNS Query – Event ID 22
- Domain requested by the process?
- Investigate domain reputation:
  - Check WHOIS, VirusTotal, domain age

---

## Triage Decision
After reviewing all logs:
- 🔵 **Benign:** Normal activity (document and close)
- 🟡 **Suspicious:** Needs escalation (Tier 2, IR team)
- 🔴 **Malicious:** Evidence of compromise (document fully)

---

## Reporting Format
Create `.md` or `.txt` report with:
```
Date:
Event ID(s):
Process name:
Command line:
Network IP/domain:
Analysis:
Action taken:
```

---

## Final Note
Every event is context-dependent. Correlate multiple logs before concluding. Save samples/screenshots in GitHub or offline if needed.
