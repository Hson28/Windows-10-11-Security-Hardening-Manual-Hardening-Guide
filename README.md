# Windows Hardening & Säkerhet 🛡️

![Windows](https://img.shields.io/badge/Windows-10%2F11-blue?style=flat-square&logo=windows) ![Security](https://img.shields.io/badge/Security-Hardening-green?style=flat-square) ![Powershell](https://img.shields.io/badge/Language-PowerShell-blue?style=flat-square&logo=powershell)

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

## 🤝 Bidra
Har du förslag på fler säkerhetsåtgärder eller förbättringar av skripten?
1.  Forka detta repository.
2.  Skapa en ny branch (`git checkout -b feature/ny-sakerhet`).
3.  Commit:a dina ändringar.
4.  Push:a till branchen.
5.  Öppna en Pull Request.

## 📄 Licens
Distribueras under MIT License. Se `LICENSE` för mer information.
