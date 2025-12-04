# 📋 Windows Hardening Checklist / Checklista

*[🇬🇧 English version below](#-windows-hardening-checklist)*

Denna checklista beskriver manuella steg och GPO-konfigurationer för att säkra Windows 10/11. Varje punkt innehåller en förklaring till varför åtgärden är viktig.

**OBS:** Testa alltid ändringar i en testmiljö först.

---

## 🇸🇪 Svensk Checklista

### 1. 🔐 BitLocker & Kryptering

- [ ] **Aktivera BitLocker med stark kryptering (XTS-AES 256-bit)**
  * *Var:* `Computer Configuration > Administrative Templates > Windows Components > BitLocker Drive Encryption > Drive Encryption Method and Cipher Strength`.
  * *Åtgärd:* Välj "Enabled" och sätt "Operating System Drives" till **XTS-AES 256-bit**.
  * *ℹ️ Varför:* Standardkrypteringen är ofta 128-bit. Genom att tvinga 256-bit XTS-AES får du det starkaste skyddet mot brute-force-attacker om disken skulle stjälas.

- [ ] **Kräv PIN eller lösenord vid start (Pre-boot authentication)**
  * *Var:* `... > Operating System Drives > Require additional authentication at startup`.
  * *Åtgärd:* Aktivera och konfigurera TPM + PIN.
  * *ℹ️ Varför:* Utan PIN låser datorn upp krypteringsnyckeln automatiskt via TPM-chippet så fort strömmen slås på. En PIN-kod skyddar mot attacker där angriparen försöker läsa minnet direkt (Cold Boot attacks) eller manipulera bootprocessen.

- [ ] **Inaktivera DMA-enheter vid låst dator**
  * *Var:* `Computer Configuration > Administrative Templates > System > Kernel DMA Protection`.
  * *ℹ️ Varför:* Förhindrar att externa enheter (via Thunderbolt/PCIe) kan läsa av systemminnet direkt (DMA-attacker) när datorn är påslagen men låst.

### 2. 📝 Audit Logs (Loggning)

- [ ] **Logga processkapande (Process Creation)**
  * *Var:* `Computer Configuration > Windows Settings > Security Settings > Advanced Audit Policy Configuration > System Audit Policies > Detailed Tracking`.
  * *Åtgärd:* Aktivera "Audit Process Creation" (Success/Failure).
  * *ℹ️ Varför:* Detta är grundläggande för att se *vad* som händer på datorn. Utan detta syns det inte i loggarna när ett program eller ett virus startas.

- [ ] **Inkludera kommandorader i loggen (Viktigt för forensic)**
  * *Var:* `Computer Configuration > Administrative Templates > System > Audit Process Creation`.
  * *Åtgärd:* Aktivera "Include command line in process creation events".
  * *ℹ️ Varför:* Att veta *att* `powershell.exe` startade räcker inte. Du måste veta *vilket skript* eller *vilket kommando* den körde. Denna inställning avslöjar angriparens instruktioner.

- [ ] **Öka storleken på säkerhetsloggen**
  * *Var:* `Event Viewer > Windows Logs > Security (Högerklick > Properties)`.
  * *Åtgärd:* Sätt till minst **102400 KB** (100 MB) eller mer.
  * *ℹ️ Varför:* Standardstorleken är ofta för liten (20 MB). Vid en attack fylls loggen snabbt, och gamla bevis skrivs över (log rotation) innan du hinner se dem.

### 3. ⚙️ Group Policy & Kontosäkerhet

- [ ] **Namnändra lokalt administratörskonto**
  * *Var:* `Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options`.
  * *Åtgärd:* "Accounts: Rename administrator account" (Döp till något neutralt, t.ex. "SupportUser").
  * *ℹ️ Varför:* "Security through obscurity". Det stoppar automatiska script och bottar som specifikt försöker logga in mot användarnamnet "Administrator".

- [ ] **Konfigurera kontolåsning (Account Lockout)**
  * *Var:* `... > Local Policies > Account Lockout Policy`.
  * *Åtgärd:* Sätt "Account lockout threshold" till t.ex. **10 försök**.
  * *ℹ️ Varför:* Förhindrar lösenordsgissning (Brute Force) genom att låsa kontot tillfälligt efter ett antal misslyckade försök.

- [ ] **Inaktivera AutoRun/AutoPlay**
  * *Var:* `Computer Configuration > Administrative Templates > Windows Components > AutoPlay Policies`.
  * *Åtgärd:* "Turn off AutoPlay" på alla enheter.
  * *ℹ️ Varför:* Historiskt sett en av de vanligaste infektionsvägarna. Förhindrar att skadlig kod på USB-minnen körs automatiskt så fort minnet sätts i.

- [ ] **Skärp UAC (User Account Control)**
  * *Var:* `... > Security Options`.
  * *Åtgärd:* "User Account Control: Behavior of the elevation prompt for administrators" -> **Prompt for consent for non-Windows binaries**.
  * *ℹ️ Varför:* Tvingar användaren att godkänna installationer och ändringar. Det förhindrar skadlig kod från att tyst höja sina rättigheter (Privilege Escalation) i bakgrunden.

### 4. 🛠️ Tjänstehärdning (Services) & Nätverk

- [ ] **Inaktivera SMBv1**
  * *PowerShell:* `Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol`
  * *ℹ️ Varför:* SMBv1 är ett föråldrat protokoll med kända, allvarliga säkerhetshål (t.ex. EternalBlue/WannaCry). Det ska aldrig användas i moderna nätverk.

- [ ] **Inaktivera Print Spooler (Om datorn inte skriver ut)**
  * *PowerShell:* `Set-Service -Name Spooler -StartupType Disabled`
  * *ℹ️ Varför:* Print Spooler har historiskt haft många kritiska sårbarheter (t.ex. PrintNightmare) som ger angripare full kontroll över systemet.

- [ ] **Inaktivera Remote Desktop (RDP) om det ej används**
  * *Inställningar:* System > Remote Desktop > Off.
  * *ℹ️ Varför:* RDP är en vanlig ingång för ransomware-attacker. Om du inte behöver fjärrstyra datorn ska porten vara stängd för att minska attackytan.

- [ ] **Blockera inkommande trafik som standard i brandväggen**
  * *Var:* Windows Defender Firewall with Advanced Security.
  * *Åtgärd:* På "Public Profile", sätt "Inbound connections" till **Block**.
  * *ℹ️ Varför:* Säkerställer att inga tjänster exponeras mot osäkra nätverk (som café-wifi) av misstag. Endast svar på trafik du själv initierat tillåts.

---

## 🇬🇧 Windows Hardening Checklist

This checklist details manual steps and GPO configurations to secure Windows 10/11. Each item includes an explanation of its importance.

### 1. 🔐 BitLocker & Encryption

- [ ] **Enable BitLocker with strong encryption (XTS-AES 256-bit)**
  * *Path:* `Computer Configuration > Administrative Templates > Windows Components > BitLocker Drive Encryption > Drive Encryption Method and Cipher Strength`.
  * *Action:* Set to "Enabled" and choose **XTS-AES 256-bit** for Operating System Drives.
  * *ℹ️ Why:* Standard encryption is often 128-bit. Enforcing 256-bit XTS-AES provides the highest level of protection against brute-force attacks if the drive is stolen.

- [ ] **Require PIN or Password at startup**
  * *Path:* `... > Operating System Drives > Require additional authentication at startup`.
  * *Action:* Enable and configure TPM + PIN.
  * *ℹ️ Why:* Without a PIN, the TPM chip automatically unlocks the encryption key upon boot. A PIN protects against Cold Boot attacks and physical tampering with the boot process.

- [ ] **Disable DMA devices when computer is locked**
  * *Path:* `Computer Configuration > Administrative Templates > System > Kernel DMA Protection`.
  * *ℹ️ Why:* Prevents external peripherals (via Thunderbolt/PCIe) from accessing system memory directly (DMA attacks) while the computer is running but locked.

### 2. 📝 Audit Logs

- [ ] **Audit Process Creation**
  * *Path:* `Computer Configuration > Windows Settings > Security Settings > Advanced Audit Policy Configuration > System Audit Policies > Detailed Tracking`.
  * *Action:* Enable "Audit Process Creation" (Success/Failure).
  * *ℹ️ Why:* Fundamental for visibility. Without this, security logs won't show when a program or malware executable is launched.

- [ ] **Include Command Line in logs (Crucial for forensics)**
  * *Path:* `Computer Configuration > Administrative Templates > System > Audit Process Creation`.
  * *Action:* Enable "Include command line in process creation events".
  * *ℹ️ Why:* Knowing *that* `powershell.exe` ran isn't enough. You need to know *what script* or *arguments* were passed. This setting reveals the attacker's actual instructions.

- [ ] **Increase Security Log size**
  * *Path:* `Event Viewer > Windows Logs > Security (Right-click > Properties)`.
  * *Action:* Set to at least **102400 KB** (100 MB).
  * *ℹ️ Why:* Default log sizes are often too small (20 MB). During an attack, logs fill up quickly, and older evidence is overwritten (log rotation) before it can be analyzed.

### 3. ⚙️ Group Policy & Account Security

- [ ] **Rename Local Administrator Account**
  * *Path:* `Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options`.
  * *Action:* "Accounts: Rename administrator account" (e.g., to "SupportUser").
  * *ℹ️ Why:* "Security through obscurity". Stops automated scripts and bots that specifically target the username "Administrator".

- [ ] **Configure Account Lockout**
  * *Path:* `... > Local Policies > Account Lockout Policy`.
  * *Action:* Set "Account lockout threshold" to e.g., **10 attempts**.
  * *ℹ️ Why:* Prevents password guessing (Brute Force) by temporarily locking the account after a set number of failed attempts.

- [ ] **Disable AutoRun/AutoPlay**
  * *Path:* `Computer Configuration > Administrative Templates > Windows Components > AutoPlay Policies`.
  * *Action:* "Turn off AutoPlay" on all drives.
  * *ℹ️ Why:* Prevents malware on USB drives from executing automatically upon insertion. Historically a very common infection vector.

- [ ] **Tighten UAC (User Account Control)**
  * *Path:* `... > Security Options`.
  * *Action:* "User Account Control: Behavior of the elevation prompt for administrators" -> **Prompt for consent for non-Windows binaries**.
  * *ℹ️ Why:* Forces user interaction for installations/changes. Prevents malware from silently elevating privileges in the background without the user noticing.

### 4. 🛠️ Service Hardening & Network

- [ ] **Disable SMBv1**
  * *PowerShell:* `Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol`
  * *ℹ️ Why:* SMBv1 is an obsolete protocol with critical vulnerabilities (e.g., EternalBlue/WannaCry). It should never be used in modern networks.

- [ ] **Disable Print Spooler (If printing is not needed)**
  * *PowerShell:* `Set-Service -Name Spooler -StartupType Disabled`
  * *ℹ️ Why:* The Print Spooler has a history of critical vulnerabilities (e.g., PrintNightmare) allowing remote code execution. Disable it on systems that don't need to print.

- [ ] **Disable Remote Desktop (RDP) if not used**
  * *Settings:* System > Remote Desktop > Off.
  * *ℹ️ Why:* RDP is a primary entry point for ransomware. Keeping the port closed reduces the attack surface significantly.

- [ ] **Block inbound traffic by default in Firewall**
  * *Path:* Windows Defender Firewall with Advanced Security.
  * *Action:* On "Public Profile", set "Inbound connections" to **Block**.
  * *ℹ️ Why:* Ensures no services are accidentally exposed to untrusted networks (like public Wi-Fi). Only traffic you initiate is allowed back in.
