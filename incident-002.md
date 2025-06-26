# Incident 002 – Suspicious Encoded PowerShell Execution

## 🕵️ Step-by-Step Triage Workflow

### 1. Identify Alert Context
This alert was triggered by an obfuscated PowerShell command using Base64 encoding. The `powershell.exe` process was observed spawning from the user context.

#### Sysmon Event ID 1 – Process Create

- **Timestamp:** 6/25/2025 10:50:09 PM  
- **Parent Process:** `cmd.exe` (Command Prompt)  
- **Process Created:** `powershell.exe -enc UwB0AGEAcgB0AC0AUwBsAGUAZQBwACAAOwAA==`  
- **Image Path:** `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`  
- **User:** `Win10-LAB\vboxuser`  
- **CommandLine:** `powershell.exe -enc UwB0AGEAcgB0AC0AUwBsAGUAZQBwACAAOwAA==`

![Sysmon PowerShell Encoded Event](https://raw.githubusercontent.com/Ataswat/soc-home-lab/main/sysmon_event_id1_powershell_encoded.png)

---

### 2. Filter Key Event IDs
Filter for relevant Event IDs:

- `1` – Process creation  
- `3` – Network connection (if any follow-on activity is seen)  
- `11` – File created (was there a dropped payload?)

---

### 3. Analyze the Behavior
- The use of the `-enc` flag suggests possible obfuscation.
- Decode the Base64 string and review its intent.
- No obvious follow-up behavior (e.g., network activity) was recorded in this snapshot.

---

### 4. Decode Base64 Payload
```powershell
[System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String("UwB0AGEAcgB0AC0AUwBsAGUAZQBwACAAOwAA=="))
