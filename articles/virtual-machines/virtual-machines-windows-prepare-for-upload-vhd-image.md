<properties
    pageTitle="Vorbereiten einer virtuellen Windows Azure hochladen | Microsoft Azure"
    description="Empfohlene Vorgehensweisen für das Vorbereiten einer virtuellen Windows Azure hochladen"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Vorbereiten einer virtuellen Windows Azure hochladen
Eine Windows VM lokal in Azure hochladen, müssen Sie die virtuelle Festplatte (VHD) korrekt vorbereiten. Es gibt mehrere empfohlene Schritte ausführen, bevor Sie eine virtuelle Festplatte in Azure hochladen. Dieser Artikel veranschaulicht das Vorbereiten einer Windows virtuellen Microsoft Azure hochladen und außerdem erfahren Sie, [wann und wie Sysprep verwenden](#step23).

## <a name="prepare-the-virtual-disk"></a>Vorbereiten des virtuellen Laufwerks

>[AZURE.NOTE] 
> Azure unterstützt nur [virtuelle Computer Generation 1](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) im VHD-Format. Neuere VHDX-Format wird in Azure nicht unterstützt. 
>
> Die virtuelle Festplatte muss eine feste Größe nicht dynamisch. Ggf. ausführlich beschrieben VHDX oder dynamischen Datenträger konvertieren. Die maximale zulässige Größe für die virtuelle Festplatte beträgt 1.023 GB.


Stellen Sie sicher, dass die VHD-Datei auf dem lokalen Server funktionsfähig ist. Beheben Sie eventuelle Fehler innerhalb der virtuellen Maschine selbst vor oder in Azure hochladen.

Benötigen Sie das erforderliche Format für Azure virtuelle Festplatte konvertieren, verwenden Sie eine der Methoden in den folgenden Abschnitten notiert haben. Sichern Sie die VM vor Konvertierung Laufwerk oder Sysprep ausgeführt.

### <a name="convert-using-hyper-v-manager"></a>Konvertierung mit Hyper-V-Manager
- Hyper-V-Manager öffnen und Ihren lokalen Computer auf der linken Seite. Klicken Sie im Menü oben auf **Aktion** > **Festplatte bearbeiten**.
    - Auf dem Bildschirm **virtuelle Festplatte suchen** nach und wählen Sie das virtuelle Laufwerk.
    - Wählen Sie auf der nächsten Seite **Konvertieren**
        - Möchten Sie VHDX konvertieren, wählen Sie **VHD** und auf **Weiter**
        - Benötigen Sie eine dynamische Festplatte konvertieren wählen Sie **fester Größe** , und klicken Sie auf **Weiter**

    - Suchen Sie und wählen Sie **Pfad für die VHD-Datei**.
    - Klicken Sie auf **Fertig stellen** , um schließen.

### <a name="convert-using-powershell"></a>Konvertierung mit PowerShell
Sie können ein virtuelles Laufwerk mit dem [Konvertieren VHD-PowerShell-Cmdlets](http://technet.microsoft.com/library/hh848454.aspx)konvertieren. Im folgenden Beispiel werden wir konvertieren aus einem VHDX virtuelle Festplatte und Konvertieren von einer dynamischen Typ fest:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>VMware VMDK Diskettenformat konvertieren
Haben Sie ein Windows-VM-Image im [VMDK-Dateiformat](https://en.wikipedia.org/wiki/VMDK)konvertieren Sie in eine virtuelle Festplatte mit dem [Microsoft Virtual Machine-Konverter](https://www.microsoft.com/download/details.aspx?id=42497). Lesen Sie den Blog [ein VMware VMDK zu Hyper-V virtuelle Festplatte konvertieren](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) Weitere Informationen.

## <a name="prepare-windows-configuration-for-upload"></a>Windows-Konfiguration für den Upload vorzubereiten

> [AZURE.NOTE] Führen Sie die folgenden Befehle mit [Administratorrechten](https://technet.microsoft.com/library/cc947813.aspx).

1. Entfernen Sie alle statischen beständige Route in der Routingtabelle

    - Führen Sie zum Anzeigen der Routentabelle `route print`.
    - Überprüfen Sie **Routen für** Bereiche. Verwenden Sie [Route löschen](https://technet.microsoft.com/library/cc739598.apx) zum Entfernen ist eine beständige Route.

2. Entfernen Sie WinHTTP Proxy:

    ```
    netsh winhttp reset proxy
    ```

3. Konfigurieren Sie die Festplatte SAN-Richtlinie für [Onlineall](https://technet.microsoft.com/library/gg252636.aspx):

    ```
    diskpart san policy=onlineall
    ```

4. Verwenden Sie koordinierte Weltzeit (UTC) Zeit für Windows, und legen Sie den Starttyp des Windows-Zeitdienst (w32time) **automatisch**:

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Konfigurieren von Windows-Diensten
5. Stellen Sie sicher, dass die folgenden Windows-Dienste auf **Windows-Standardwerte**festlegen. Sie sind in der folgenden Liste angegeben Autostart-Optionen konfiguriert. Sie können diese Befehle Start Einstellungen ausführen:

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Konfiguration von Remote Desktop
6. Sind alle selbstsignierte Zertifikate in Remote Desktop Protocol (RDP) Listener gebunden, entfernen:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Weitere Informationen zum Konfigurieren von Zertifikaten für RDP-Listener finden Sie unter [Listener Zertifikat Konfigurationen von Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. [KeepAlive](https://technet.microsoft.com/library/cc957549.aspx) -Werte für RDP-Dienst zu konfigurieren:

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. Konfigurieren Sie den Authentifizierungsmodus für RDP-Dienst:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. Aktivieren Sie RDP-Dienst der Registrierung die folgenden Unterschlüssel hinzu:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>Windows-Firewall-Regeln konfigurieren
10. Die drei Firewallprofile (Domäne, privat und öffentlich) Durchlassen Sie WinRM und aktivieren Sie PowerShell Remote Service:

    ```
    Enable-PSRemoting -force
    ```

11. Stellen Sie sicher, dass die folgenden Gast-Betriebssystem Firewall Regeln:

    - Eingehende

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Eingehenden und ausgehenden

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Ausgehende

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>Zusätzliche Konfigurationsschritte für Windows
12. Ausführen `winmgmt /verifyrepository` zu bestätigen, dass das Repository (Windows Management Instrumentation, WMI) konsistent ist. Wenn das Repository beschädigt ist, finden Sie [in diesem Blogbeitrag](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Stellen Sie sicher, dass die (Boot Configuration Data, BCD) folgende Einstellungen:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Entfernen Sie zusätzliche Transport Driver Interface Filter, wie Software, die TCP-Pakete analysiert.
15. Um sicherzustellen, dass der Datenträger ist fehlerfrei, führen die `CHKDSK /f` Befehl.
16. Deinstallieren Sie anderer Drittanbietersoftware und Fahrer physischen Komponenten oder anderen Virtualisierungstechnologie.
17. Stellen Sie sicher, dass eine Anwendung nicht Port 3389. Dieser Port ist für den RDP-Dienst in Azure verwendet. Sie können die `netstat -anob` Befehl Ports überprüfen, die von Clientanwendungen verwendet werden.
18. Wenn die Windows virtuelle Festplatte, die Sie hochladen möchten, ein Domänencontroller ist, führen Sie [diese Schritte](https://support.microsoft.com/kb/2904015) zur Vorbereitung der Festplatte.
19. Neustart der VM zu Windows noch fehlerfrei ist erreichbar mit RDP-Verbindung.
20. Zurücksetzen Sie aktuelle lokale Administratorkennwort und stellen Sie sicher, dass Sie dieses Konto verwenden können, über die RDP-Verbindung an Windows anmelden.  Das Richtlinienobjekt "Anmelden über Remotedesktopdienste zulassen" diese Berechtigungen gesteuert. Dieses Objekt befindet sich unter "Computer-Einstellungen\Sicherheitseinstellungen\Lokale Richtlinien\Zuweisen von Benutzerrechten."


## <a name="install-windows-updates"></a>Windows installieren
22. Installieren Sie die neuesten Updates für Windows. Wenn dies nicht möglich ist, müssen Sie die folgenden Updates installiert sind:

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VMs nicht von einem Netzwerkausfall wiederherzustellen und treten Probleme mit beschädigten Daten

    - [KB3115224](https://support.microsoft.com/kb/3115224) Verbesserte Zuverlässigkeit für virtuelle Computer, die auf einem Windows Server 2012 R2 oder Windows Server 2012 Server ausgeführt werden

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Sicherheitsupdate für Microsoft Windows eine Erhöhung von Berechtigungen Adresse: 8. März 2016

    - [KB3063075](https://support.microsoft.com/kb/3063075) Viele ID 129 Ereignisse protokolliert, wenn Sie einen virtuellen Computer Windows Server 2012 R2 in Microsoft Azure ausführen

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VMs nicht von einem Netzwerkausfall wiederherzustellen und treten Probleme mit beschädigten Daten

    - [KB3114025](https://support.microsoft.com/kb/3114025) Geringe Leistung, wenn Windows 8.1 oder Server 2012 R2 Azure Verzeichnis zugreifen

    - [KB3033930](https://support.microsoft.com/kb/3033930) Hotfix erhöht 64 KB für RIO Puffer pro Prozess für den Dienst in Windows Azure

    - [KB3004545](https://support.microsoft.com/kb/3004545) Sie können virtuelle Computer zugreifen, die Hosting-Services über eine VPN-Verbindung in Windows Azure gehostet werden

    - [KB3082343](https://support.microsoft.com/kb/3082343) Standortübergreifende VPN-Konnektivität geht verloren, wenn Azure Standort-zu-Standort-VPN-Tunnel Windows Server 2012 R2 RRAS verwenden

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Sicherheitsupdate für Microsoft Windows eine Erhöhung von Berechtigungen Adresse: 8. März 2016

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: Beschreibung des Sicherheitsupdates für CSRSS: 12 April 2016
    - [KB2904100](https://support.microsoft.com/kb/2904100) System stürzt während der Festplatte in Windows<a id="step23"></a>
23. Wenn Sie ein Bild aus mehreren Computern bereitstellen möchten, müssen mit das Abbild verallgemeinern `sysprep` die virtuellen Festplatte in Azure hochladen. Sie müssen nicht ausführen `sysprep` für die Verwendung einer speziellen virtuellen Festplatte. Weitere Informationen zum Erstellen eines generalisierten Abbilds finden Sie in folgenden Artikeln:

    - [Erstellen Sie VM-Image aus einer vorhandenen Azure VM mit dem Ressourcen-Manager-Bereitstellungsmodell](virtual-machines-windows-create-vm-generalized.md)
    - [Eine vorhandene Azure VM mit der klassischen Bereitstellung erstellen Sie einer VM-image](virtual-machines-windows-classic-capture-image.md)
    - [Sysprep-Unterstützung für Serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Empfohlene zusätzliche Konfigurationen

Folgendes wirken nicht virtuelle Festplatte hochladen. Jedoch wird dringend empfohlen, die sie konfiguriert haben.

- [Azure Virtual Machines Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)installieren. Nach der Installation des Agents können Sie VM-Erweiterungen. VM-Erweiterungen implementiert die meisten kritischen Funktionen mit Ihrer virtuellen Computer wie das Zurücksetzen von Kennwörtern, RDP konfigurieren möchten, und viele andere.

- Das Dump-Protokoll kann bei der Problembehandlung von Windows-abstürzen. Aktivieren der Dump Log-Auflistung:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Nach der Erstellung der VM in Azure konfigurieren Sie System definierte Größe Auslagerungsdatei auf Laufwerk D:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Nächste Schritte

- [Hochladen Sie ein Windows-VM-Image für Ressourcenmanager Bereitstellung in Azure](virtual-machines-windows-upload-image.md)
