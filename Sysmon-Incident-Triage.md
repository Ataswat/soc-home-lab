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
This event was triggered when the Windows Event Viewer was launched (`mmc.exe`), which created a new process.

#### Sysmon Event ID 1 – Process Create

- **Timestamp:** 6/25/2025 10:19:06 PM  
- **Parent Process:** svchost.exe  
- **Process Created:** mmc.exe (Event Viewer)  
- **Image Path:** C:\Windows\System32\mmc.exe  
- **User:** Win10-LAB\vboxuser  

![Sysmon mmc.exe Event](https://raw.githubusercontent.com/Ataswat/soc-home-lab/main/sysmon_event_mmc.png)


### 2. Filter Key Event IDs  
Filter for relevant events:  
- **1** – Process creation  
- **3** – Network connection  
- **11** – File created  
- **13/14** – Registry object changes  
- **22** – DNS query  

### 3. Investigate Suspicious Indicators  
**For Process Creation (ID 1):**  
- Unusual command lines (e.g., PowerShell with `-enc`, `-nop`)  
- Rare parent-child process relationships (e.g., Word spawning cmd.exe)  

**For Network Connections (ID 3):**  
- Connections to known malicious IPs/domains  
- Outbound traffic over uncommon ports  

**For File Creation (ID 11):**  
- Executables dropped in `C:\Users\Public\`, `%TEMP%`, or `%AppData%`  

**For Registry Changes (ID 13/14):**  
- Persistence via `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`  

**For DNS Queries (ID 22):**  
- Domains tied to malware or C2 servers  

### 4. Correlate Events Across Timestamps  
- Match suspicious DNS → connection → process  
- Establish full attack timeline  

### 5. Escalate or Contain  
- Document findings  
- Isolate affected system  
- Notify incident response if needed  
