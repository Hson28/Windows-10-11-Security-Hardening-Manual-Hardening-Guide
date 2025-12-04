# Windows Hardening & Säkerhet 🛡️

![Windows](https://img.shields.io/badge/Windows-10%2F11-blue?style=flat-square&logo=windows) ![Security](https://img.shields.io/badge/Security-Hardening-green?style=flat-square) ![Powershell](https://img.shields.io/badge/Language-PowerShell-blue?style=flat-square&logo=powershell)

*[🇬🇧 English version below](#-windows-hardening--security)*

En **teknisk guide** och **detaljerad checklista** för att manuellt säkra och härda (harden) en Windows 10/11-miljö. Detta projekt syftar till att minska attackytan på klienter genom beprövade metoder och konfigurationer.

## 📖 Om Projektet

Att säkra en Windows-miljö kräver mer än bara ett antivirusprogram. Detta repository samlar "best practices" för konfiguration av operativsystemet för att skydda mot moderna hot. Guiden täcker allt från diskkryptering till detaljerad loggning och tjänstehärdning.

Projektet innehåller både manuella instruktioner och PowerShell-skript för automatisering.

## 🚀 Innehåll

Denna guide täcker följande huvudområden:

### 🔐 1. BitLocker & Fysisk Säkerhet
* Konfiguration av BitLocker (XTS-AES 256-bit).
* TPM-krav och PIN-hantering.
* DMA-skydd (Direct Memory Access).

### 📝 2. Audit Logs & Övervakning
* Aktivering av avancerad loggning (Process Creation, Command Line Auditing).
* Konfiguration av loggstorlek och lagring.
* Spårning av inloggningsförsök och privilegieeskallering.

### ⚙️ 3. Group Policy (GPO)
* Säkerhetspolicys för lösenord och kontolåsning.
* Begränsning av administrativa rättigheter.
* Blockering av exekvering från temporära mappar (AppLocker/SRP basics).
* Inaktivering av telemetri och datainsamling.

### 🛠️ 4. Tjänstehärdning (Service Hardening)
* Inaktivering av onödiga Windows-tjänster (t.ex. Xbox Services, Print Spooler om ej nödvändig).
* Nätverkskonfiguration och brandväggsregler.

## 💻 Kom igång

### Förutsättningar
* **OS:** Windows 10 eller Windows 11 (Pro eller Enterprise rekommenderas för fullt GPO-stöd).
* **Behörighet:** Administratörsrättigheter krävs för de flesta moment.

### Installation / Användning

1.  **Kloning av repo:**
    ```powershell
    git clone [https://github.com/ditt-anvandarnamn/windows-hardening.git](https://github.com/ditt-anvandarnamn/windows-hardening.git)
    cd windows-hardening
    ```

2.  **Kör manuell checklista:**
    Öppna `CHECKLIST.md` för att gå igenom stegen manuellt.

3.  **Använd PowerShell-skript (Valfritt):**
    > **Varning:** Granska alltid skript innan du kör dem i din miljö.
    ```powershell
    # Exempel på att köra härdningsskriptet
    .\scripts\harden-windows.ps1
    ```

## ⚠️ Disclaimer

**Används på egen risk.**
Härdning av operativsystem kan ibland påverka funktionaliteten i vissa applikationer eller systemfunktioner.
* Testa alltid konfigurationerna i en virtuell miljö (VM) eller på en testdator innan du rullar ut det i produktion.
* Se till att du har en aktuell backup.

---

# 🛡️ Windows Hardening & Security

A **technical guide** and **detailed checklist** for manually securing and hardening a Windows 10/11 environment. This project aims to reduce the attack surface on clients through proven methods and configurations.

## 📖 About the Project

Securing a Windows environment requires more than just antivirus software. This repository collects "best practices" for operating system configuration to protect against modern threats. The guide covers everything from disk encryption to detailed logging and service hardening.

The project includes both manual instructions and PowerShell scripts for automation.

## 🚀 Contents

This guide covers the following main areas:

### 🔐 1. BitLocker & Physical Security
* BitLocker configuration (XTS-AES 256-bit).
* TPM requirements and PIN management.
* DMA protection (Direct Memory Access).

### 📝 2. Audit Logs & Monitoring
* Enabling advanced logging (Process Creation, Command Line Auditing).
* Configuration of log size and retention.
* Tracking login attempts and privilege escalation.

### ⚙️ 3. Group Policy (GPO)
* Security policies for passwords and account lockouts.
* Restriction of administrative privileges.
* Blocking execution from temporary folders (AppLocker/SRP basics).
* Disabling telemetry and data collection.

### 🛠️ 4. Service Hardening
* Disabling unnecessary Windows services (e.g., Xbox Services, Print Spooler if not needed).
* Network configuration and firewall rules.

## 💻 Getting Started

### Prerequisites
* **OS:** Windows 10 or Windows 11 (Pro or Enterprise recommended for full GPO support).
* **Permissions:** Administrative rights are required for most steps.

### Installation / Usage

1.  **Clone the repo:**
    ```powershell
    git clone [https://github.com/your-username/windows-hardening.git](https://github.com/your-username/windows-hardening.git)
    cd windows-hardening
    ```

2.  **Run manual checklist:**
    Open `CHECKLIST.md` to go through the steps manually.

3.  **Use PowerShell scripts (Optional):**
    > **Warning:** Always review scripts before running them in your environment.
    ```powershell
    # Example of running the hardening script
    .\scripts\harden-windows.ps1
    ```

## ⚠️ Disclaimer

**Use at your own risk.**
System hardening can sometimes affect the functionality of certain applications or system features.
* Always test configurations in a virtual environment (VM) or on a test machine before rolling out to production.
* Ensure you have a current backup.

## 🤝 Contributing
Do you have suggestions for more security measures or improvements to the scripts?
1.  Fork this repository.
2.  Create a new branch (`git checkout -b feature/new-security`).
3.  Commit your changes.
4.  Push to the branch.
5.  Open a Pull Request.

## 📄 License
Distributed under the MIT License. See `LICENSE` for more information.
