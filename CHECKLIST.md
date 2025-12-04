# 📋 Windows Hardening Checklist / Checklista

*[🇬🇧 English version below](#-windows-hardening-checklist)*

Denna checklista beskriver manuella steg och GPO-konfigurationer för att säkra Windows 10/11.
**OBS:** Testa alltid ändringar i en testmiljö först.

---

## 🇸🇪 Svensk Checklista

### 1. 🔐 BitLocker & Kryptering
- [ ] **Aktivera BitLocker med stark kryptering (XTS-AES 256-bit)**
  * *Var:* `Computer Configuration > Administrative Templates > Windows Components > BitLocker Drive Encryption > Drive Encryption Method and Cipher Strength`.
  * *Åtgärd:* Välj "Enabled" och sätt "Operating System Drives" till **XTS-AES 256-bit**.
- [ ] **Kräv PIN eller lösenord vid start**
  * *Var:* `... > Operating System Drives > Require additional authentication at startup`.
  * *Åtgärd:* Aktivera och konfigurera TPM + PIN.
- [ ] **Inaktivera DMA-enheter vid låst dator**
  * *Var:* `Computer Configuration > Administrative Templates > System > Kernel DMA Protection`.

### 2. 📝 Audit Logs (Loggning)
- [ ] **Logga processkapande (Process Creation)**
  * *Var:* `Computer Configuration > Windows Settings > Security Settings > Advanced Audit Policy Configuration > System Audit Policies > Detailed Tracking`.
  * *Åtgärd:* Aktivera "Audit Process Creation" (Success/Failure).
- [ ] **Inkludera kommandorader i loggen (Viktigt för forensic)**
  * *Var:* `Computer Configuration > Administrative Templates > System > Audit Process Creation`.
  * *Åtgärd:* Aktivera "Include command line in process creation events".
- [ ] **Öka storleken på säkerhetsloggen**
  * *Var:* `Event Viewer > Windows Logs > Security (Högerklick > Properties)`.
  * *Åtgärd:* Sätt till minst **102400 KB** (100 MB) eller mer.

### 3. ⚙️ Group Policy & Kontosäkerhet
- [ ] **Namnändra lokalt administratörskonto**
  * *Var:* `Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options`.
  * *Åtgärd:* "Accounts: Rename administrator account".
- [ ] **Konfigurera kontolåsning (Account Lockout)**
  * *Var:* `... > Local Policies > Account Lockout Policy`.
  * *Åtgärd:* Sätt "Account lockout threshold" till t.ex. **10 försök**.
- [ ] **Inaktivera AutoRun/AutoPlay**
  * *Var:* `Computer Configuration > Administrative Templates > Windows Components > AutoPlay Policies`.
  * *Åtgärd:* "Turn off AutoPlay" på alla enheter.
- [ ] **Aktivera UAC (User Account Control)**
  * *Var:* `... > Security Options`.
  * *Åtgärd:* "User Account Control: Behavior of the elevation prompt for administrators" -> **Prompt for consent for non-Windows binaries**.

### 4. 🛠️ Tjänstehärdning (Services) & Nätverk
- [ ] **Inaktivera SMBv1 (Skydd mot WannaCry etc.)**
  * *PowerShell:* `Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol`
- [ ] **Inaktivera Print Spooler (Om datorn inte skriver ut)**
  * *PowerShell:* `Set-Service -Name Spooler -StartupType Disabled`
- [ ] **Inaktivera Remote Desktop (RDP) om det ej används**
  * *Inställningar:* System > Remote Desktop > Off.
- [ ] **Blockera inkommande trafik som standard i brandväggen**
  * *Var:* Windows Defender Firewall with Advanced Security.
  * *Åtgärd:* På "Public Profile", sätt "Inbound connections" till **Block**.

---

## 🇬🇧 Windows Hardening Checklist

This checklist details manual steps and GPO configurations to secure Windows 10/11.
**Note:** Always test changes in a controlled environment first.

### 1. 🔐 BitLocker & Encryption
- [ ] **Enable BitLocker with strong encryption (XTS-AES 256-bit)**
  * *Path:* `Computer Configuration > Administrative Templates > Windows Components > BitLocker Drive Encryption > Drive Encryption Method and Cipher Strength`.
  * *Action:* Set to "Enabled" and choose **XTS-AES 256-bit** for Operating System Drives.
- [ ] **Require PIN or Password at startup**
  * *Path:* `... > Operating System Drives > Require additional authentication at startup`.
  * *Action:* Enable and configure TPM + PIN.
- [ ] **Disable DMA devices when computer is locked**
  * *Path:* `Computer Configuration > Administrative Templates > System > Kernel DMA Protection`.

### 2. 📝 Audit Logs
- [ ] **Audit Process Creation**
  * *Path:* `Computer Configuration > Windows Settings > Security Settings > Advanced Audit Policy Configuration > System Audit Policies > Detailed Tracking`.
  * *Action:* Enable "Audit Process Creation" (Success/Failure).
- [ ] **Include Command Line in logs (Crucial for forensics)**
  * *Path:* `Computer Configuration > Administrative Templates > System > Audit Process Creation`.
  * *Action:* Enable "Include command line in process creation events".
- [ ] **Increase Security Log size**
  * *Path:* `Event Viewer > Windows Logs > Security (Right-click > Properties)`.
  * *Action:* Set to at least **102400 KB** (100 MB).

### 3. ⚙️ Group Policy & Account Security
- [ ] **Rename Local Administrator Account**
  * *Path:* `Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options`.
  * *Action:* "Accounts: Rename administrator account".
- [ ] **Configure Account Lockout**
  * *Path:* `... > Local Policies > Account Lockout Policy`.
  * *Action:* Set "Account lockout threshold" to e.g., **10 attempts**.
- [ ] **Disable AutoRun/AutoPlay**
  * *Path:* `Computer Configuration > Administrative Templates > Windows Components > AutoPlay Policies`.
  * *Action:* "Turn off AutoPlay" on all drives.
- [ ] **Enable UAC (User Account Control)**
  * *Path:* `... > Security Options`.
  * *Action:* "User Account Control: Behavior of the elevation prompt for administrators" -> **Prompt for consent for non-Windows binaries**.

### 4. 🛠️ Service Hardening & Network
- [ ] **Disable SMBv1 (Protection against WannaCry etc.)**
  * *PowerShell:* `Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol`
- [ ] **Disable Print Spooler (If printing is not needed)**
  * *PowerShell:* `Set-Service -Name Spooler -StartupType Disabled`
- [ ] **Disable Remote Desktop (RDP) if not used**
  * *Settings:* System > Remote Desktop > Off.
- [ ] **Block inbound traffic by default in Firewall**
  * *Path:* Windows Defender Firewall with Advanced Security.
  * *Action:* On "Public Profile", set "Inbound connections" to **Block**.
