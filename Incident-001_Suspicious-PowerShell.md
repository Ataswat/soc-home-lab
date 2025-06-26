# Incident 001 – Suspicious PowerShell Activity

## Alert Summary  
- **Source**: SIEM Alert from Sysmon Event ID 1  
- **Trigger**: Unusual PowerShell process execution by `winword.exe`  
- **Timestamp**: 2025-06-25 14:03:12

---

## Investigation Steps  

### Step 1: Review Sysmon Event ID 1  
- Found process:  
  - `powershell.exe -nop -w hidden -enc aQBmACgA...`  
- Parent process:  
  - `winword.exe` (Microsoft Word)

### Step 2: Review Process Tree  
- Suspicious parent-child chain:  
  - `winword.exe → powershell.exe`  
- Flags:  
  - Encoded PowerShell, hidden window, no profile  
  - Likely macro-based initial access  

### Step 3: Check Network Activity (ID 3)  
- Detected outbound connection to `185.202.1.10` over port 443  
- Reverse DNS: `unknownhost.xyz`  
- Not a known business asset  

---

## Assessment  
This activity resembles **macro-enabled document attack** via phishing.

## Recommendation  
✅ Escalate to IR team  
✅ Isolate host  
✅ Retrieve phishing email sample if available  
✅ Block domain/IP in firewall

---

## Notes  
- User: `j.smith@company.local`  
- Host: `HR-WKS-03`  
- Endpoint did not have EDR alert  
