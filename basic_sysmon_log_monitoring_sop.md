# Log Monitoring SOP – Windows SOC Lab

## Purpose
This document outlines how to monitor and investigate suspicious behavior on a Windows 10 virtual machine using Sysmon and Event Viewer.

## Tools Used
- Sysmon v15.15 (System Monitor by Sysinternals)
- Sysmon XML configuration file (custom ruleset)
- Windows Event Viewer

## Key Sysmon Event IDs
| Event ID | Description                        |
|----------|------------------------------------|
| 1        | Process creation                   |
| 3        | Network connection                 |
| 7        | Image loaded                       |
| 11       | File created                       |
| 13/14    | Registry key creation/modification |
| 22       | DNS query                          |

## Log Monitoring Steps
1. Open **Event Viewer** → `Applications and Services Logs` → `Microsoft` → `Windows` → `Sysmon` → `Operational`
2. Right-click `Operational` → **Filter Current Log**
3. Enter relevant Event IDs to filter:  
   `1,3,7,11,13,22`
4. Review log entries for:
   - Unusual command-line activity  
   - Outbound connections to external IPs  
   - DNS lookups to unknown domains  
   - File creation in suspicious directories  
   - Registry modifications in autorun locations

## Investigation Checklist
- **Command line**: Does it show scripting, obfuscation, or downloads?
- **Network**: Is the destination IP internal or external? Known or unknown?
- **Files**: Are they dropped in `C:\Users\Public\`, `%TEMP%`, or similar paths?
- **Registry**: Is persistence attempted via `HKCU\...\Run`?

## Common Suspicious Examples
- `cmd.exe /c whoami` or encoded PowerShell
- IP connection to `192.168.1.100:4444`
- File drop in temp/public folder
- Registry changes outside standard install paths

## Reporting Template
Document any findings in a `.txt` or `.md` file with the following:
- Date and time  
- Event ID  
- Full command line or connection details  
- Your analysis or hypothesis

## Notes
This environment is designed for manual SOC practice. No automated detection or SIEM is used. The focus is on learning how to read and interpret logs manually.
