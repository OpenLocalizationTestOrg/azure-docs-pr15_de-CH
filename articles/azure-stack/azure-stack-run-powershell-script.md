<properties
    pageTitle="Bereitstellen von Azure Stapel POC | Microsoft Azure"
    description="Informationen Sie zum Azure Stapel POC vorbereiten und das PowerShell-Skript Azure Stapel POC bereitstellen."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="erikje"/>

# <a name="deploy-azure-stack-poc"></a>Bereitstellen von Azure Stapel POC
POC Stapel Azure bereitstellen, müssen Sie [Computer Bereitstellung vorbereiten](#prepare-the-deployment-machine) und anschließend [das Bereitstellungsskript PowerShell ausführen](#run-the-powershell-deployment-script).

## <a name="download-and-extract-microsoft-azure-stack-poc-tp2"></a>Herunterladen und Extrahieren von Microsoft Azure Stapel POC TP2

Bevor Sie beginnen, stellen Sie sicher, dass Sie mindestens 85 GB Speicherplatz.

1. Download von Azure Stapel POC TP2 besteht aus einer Zip-Datei mit den folgenden 12 Dateien insgesamt ca. 20 GB:
    - 1 MicrosoftAzureStackPOC.EXE
2. Der Lizenzvertrag und Informationen des Self-Extractor-Assistenten, und klicken Sie auf **Weiter**.
3. Die Datenschutzrichtlinie Bildschirm und Informationen des Self-Extractor-Assistenten, und klicken Sie auf **Weiter**.
4. Wählen Sie das Ziel für die Dateien extrahiert werden, klicken Sie auf **Weiter**.
    - Der Standardwert ist: <drive letter>:\<Ordner > \Microsoft Azure Stapel POC
5. Der Bildschirm Ziel und Informationen des Self-Extractor-Assistenten und klicken Sie auf **extrahieren** , um die CloudBuilder.vhdx (~44.5 GB) und ThirdPartyLicenses.rtf-Dateien zu extrahieren.

> [AZURE.NOTE] Nach dem Extrahieren der Dateien können Sie die Zip-Datei Speicherplatz auf dem Computer löschen. Oder verschieben Sie die Zip-Datei an einen anderen Speicherort benötigen, sollten Sie Sie erneut bereitstellen müssen die Zip-Dateien erneut herunterladen.

## <a name="prepare-the-deployment-machine"></a>Vorbereiten des Bereitstellung Computers

1. Stellen Sie sicher, können physisch mit dem Computer Bereitstellung verbunden oder physischen Konsole (z. B. KVM zugreifen). So benötigen Sie nach der Bereitstellung Computer in Schritt 9 neu gestartet.

2. Überprüfen der Bereitstellung Computer die [Mindestanforderungen](azure-stack-deploy.md)erfüllt. [Bereitstellung Checker für Azure Stapel Technical Preview 2](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) können Sie um Ihre Bedürfnisse zu bestätigen.

3. Melden Sie sich als lokaler Administrator an Ihrem Computer POC.

4. Kopieren Sie die Datei CloudBuilder.vhdx in C:\CloudBuilder.vhdx.

    > [AZURE.NOTE] Wenn Sie nicht das empfohlene Skript verwenden, um die POC-Hostcomputer (Schritt 5 – Schritt 7) vorzubereiten, geben Sie alle Lizenzschlüssel auf der Aktivierungsseite. Eine Testversion von Windows Server 2016 Bild gehört und Ablaufdatum Fehlermeldungen verursacht Lizenzschlüssel eingeben.

5. Auf dem Computer POC das Skript folgende PowerShell Azure Stapel TP2 Supportdateien herunterladen:

    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/'
    $LocalPath = 'c:\AzureStack_TP2_SupportFiles'

    # Create folder
    New-Item $LocalPath -type directory

    # Download files
    ( 'BootMenuNoKVM.ps1', 'PrepareBootFromVHD.ps1', 'Unattend.xml', 'unattend_NoKVM.xml') | foreach { Invoke-WebRequest ($uri + $_) -OutFile ($LocalPath + '\' + $_) } 
    ```

    Dieses Skript heruntergeladen Azure Stapel TP2 Unterstützung durch den $LocalPath-Parameter angegebenen Ordner.
    
6. Öffnen Sie eine erhöhte PowerShell-Konsole, und ändern Sie das Verzeichnis, in dem die Dateien kopiert.
    - 11 (N ist 1 bis 11) MicrosoftAzureStackPOC-N.BIN
7. Mit der rechten Maustaste auf den MicrosoftAzureStackPOC.EXE > als Administrator ausführen.

8. Führen Sie das Skript PrepareBootFromVHD.ps1. Dieses Skript und die Dateien für die unbeaufsichtigte Installation sind mit anderen zusammen mit diesem Build Unterstützungsskripts verfügbar.
    Es gibt fünf Parameter für das PowerShell-Skript:
    - CloudBuilderDiskPath (erforderlich) – der Pfad CloudBuilder.vhdx auf dem HOST.
    - DriverPath (optional) – ermöglicht das Hinzufügen zusätzlicher Treiber für den Host in virtuellen HD.
    - ApplyUnattend (optional) – Geben Sie diese Befehlszeilenoption zum Automatisieren der Konfigurations des Betriebssystems. Wenn angegeben, muss der Benutzer AdminPassword konfiguriert das Betriebssystem beim Start (sofern zugehörige unattend_NoKVM.xml Datei erfordert) bereitstellen.
    Wenn Sie diesen Parameter nicht verwenden, wird die generische unattend.xml-Datei ohne weitere Anpassung verwendet. Sie benötigen KVM vollständige Anpassung nach dem Neustart.
    - AdminPassword (optional) – nur verwendet, wenn Sie den ApplyUnattend-Parameter muss mindestens sechs Zeichen.
    - VHDLanguage (optional) – gibt die VHD Sprache standardmäßig auf "En-US".
    Das Skript wird dokumentiert und enthält Beispiel ist die häufigste Verwendung ist:
    
        `.\PrepareBootFromVHD.ps1 -CloudBuilderDiskPath C:\CloudBuilder.vhdx -ApplyUnattend`
    
        Wenn diese genaue Befehl geben Sie AdminPassword aufgefordert.

9. Wenn das Skript abgeschlossen ist, den Neustart zu bestätigen. Wenn andere Benutzer angemeldet sind, schlägt dieser Befehl fehl. Wenn der Befehl fehlschlägt, führen Sie den folgenden Befehl ein:`Restart-Computer -force` 

10. Dem Neustart des Servers in das Betriebssystem CloudBuilder.vhdx, die Bereitstellung wird fortgesetzt.

> [AZURE.IMPORTANT] Azure Stapel benötigt Zugriff auf das Internet entweder direkt oder über einen transparenten Proxy. TP2 POC-Bereitstellung unterstützt genau eine NIC für Netzwerke. Wenn Sie mehrere Netzwerkkarten verfügen, stellen Sie sicher, dass nur aktiviert ist (und alle anderen deaktiviert), vor dem Ausführen des Bereitstellungsskripts im nächsten Abschnitt.

## <a name="run-the-powershell-deployment-script"></a>Führen Sie das Bereitstellungsskript PowerShell

1. Melden Sie sich als lokaler Administrator an Ihrem Computer POC. Verwenden Sie die im vorherigen Schritt angegebenen Anmeldeinformationen.

2. Öffnen Sie eine erhöhte PowerShell-Konsole.

3. In PowerShell, führen Sie folgenden Befehl:`cd C:\CloudDeployment\Configuration`

4. Führen Sie den Bereitstellungsbefehl:`.\InstallAzureStackPOC.ps1`

5. **Eingeben des Kennworts** aufgefordert werden Geben Sie ein Kennwort ein und bestätigen Sie es. Dies ist das Kennwort für die virtuellen Maschinen. Achten Sie darauf, dass die Aufzeichnung.

6. Geben Sie die Anmeldeinformationen für Ihr Konto Azure Active Directory. Dieser Benutzer muss globaler Administrator im Verzeichnis Mandanten.

7. Der Bereitstellungsprozess dauert ein paar Stunden, während der Neustart des Systems automatisch einmal.

    > [AZURE.IMPORTANT] Wenn Sie den Fortschritt der Bereitstellung überwachen möchten, melden Sie sich als Azurestack\AzureStackAdmin an. Wenn Sie als lokaler Administrator anmelden, nachdem der Computer der Domäne hinzugefügt wird, wird den Fortschritt der Bereitstellung nicht angezeigt werden. Bereitstellung erneut, stattdessen melden Sie sich als Azurestack\AzureStackAdmin zu überprüfen, ob er läuft nicht.

    Wenn die Bereitstellung erfolgreich ist, zeigt die PowerShell-Konsole: **abgeschlossen: Aktion "Verteilung"**.

    Wenn die Bereitstellung fehlschlägt, können Sie [es](azure-stack-rerun-deploy.md)erneut versuchen. Sie können [erneut](azure-stack-redeploy.md) von Grund auf neu.

### <a name="deployment-script-examples"></a>Bereitstellungsbeispiele Skript

Wenn Ihre Identität AAD nur AAD Verzeichnis zugeordnet:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred

Wenn Ihre Identität AAD größer als AAD Verzeichnis zugeordnet ist:

    cd C:\CloudDeployment\Configuration
    $adminpass = ConvertTo-SecureString "<LOCAL ADMIN PASSWORD>" -AsPlainText -Force
    $aadpass = ConvertTo-SecureString "<AAD GLOBAL ADMIN ACCOUNT PASSWORD>" -AsPlainText -Force
    $aadcred = New-Object System.Management.Automation.PSCredential ("<AAD GLOBAL ADMIN ACCOUNT> example: user@AADDirName.onmicrosoft.com>", $aadpass)
    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred -AADDirectoryTenantName "<SPECIFIC AAD DIRECTORY example: AADDirName.onmicrosoft.com>"

Wenn Ihre Umgebung DHCP aktiviert ist, müssen Sie die folgenden zusätzlichen Parameter auf eine der Optionen oben (Verwendungsbeispiel bereitgestellt) einschließen:

    .\InstallAzureStackPOC.ps1 -AdminPassword $adminpass -AADAdminCredential $aadcred
    -NatIPv4Subnet 10.10.10.0/24 -NatIPv4Address 10.10.10.3 -NatIPv4DefaultGateway 10.10.10.1


### <a name="installazurestackpocps1-optional-parameters"></a>InstallAzureStackPOC.ps1 optionale Parameter

| Parameter | Erforderlich oder Optional | Beschreibung |
| --------- | ----------------- | ----------- |
| AADAdminCredential | Optionale | Azure Active Directory-Benutzernamen und Kennwort festgelegt. Azure Anmeldeinformationen kann ein Organisations-ID oder ein Microsoft Account. Um Microsoft Account Anmeldeinformationen verwenden, weggelassen Sie werden im Cmdlet. Dieser Parameter ausgelassen fordert das Popup Azure Authentifizierung während der Bereitstellung. Authentifizierung und aktualisieren Token während der Bereitstellung wird erstellt. |
| AADDirectoryTenantName | Erforderlich | Legt das Verzeichnis Mieter. Verwenden Sie diesen Parameter an ein bestimmtes Verzeichnis hat AAD Konto Berechtigungen zum Verwalten mehrerer Verzeichnisse. Vollständiger Name ein AAD Verzeichnis Mieter im Format <directoryName>. onmicrosoft.com. |
| AdminPassword | Erforderlich | Das lokale Administratorkonto und alle anderen Benutzerkonten wird auf allen virtuellen Computern als Teil des POC-Bereitstellung erstellt. Dieses Kennwort muss das aktuelle lokale Administratorkennwort auf dem Host. |
| AzureEnvironment | Optionale | Auswählen der Azure-Umgebung mit dem Azure Stapel Bereitstellung registriert werden soll. Optionen *Öffentlichen Azure* *Azure - China* *Azure - US-Regierung*. |
| EnvironmentDNS | Optionale | Ein DNS-Server wird als Teil der Bereitstellung Azure Stapel erstellt. Um Computer innerhalb der Lösung zum Auflösen von Namen außerhalb der Stempel zu ermöglichen, Bereitstellen Sie Ihre vorhandenen Infrastruktur DNS-server In einen DNS-Server leitet unbekannte Namensauflösungsanfragen, die an diesem Server. |
| NatIPv4Address | Erforderlich für DHCP NAT-Unterstützung | Legt eine statische IP-Adresse für MAS-BGPNAT01. Verwenden Sie diesen Parameter nur, wenn DHCP für den Internetzugriff eine gültige IP-Adresse zuweisen kann. |
| NatIPv4DefaultGateway | Erforderlich für DHCP NAT-Unterstützung | Legt das Standard-Gateway für MAS-BGPNAT01 mit der statischen IP-Adresse verwendet. Verwenden Sie diesen Parameter nur, wenn DHCP für den Internetzugriff eine gültige IP-Adresse zuweisen kann.  |
| NatIPv4Subnet | Erforderlich für DHCP NAT-Unterstützung | IP-Subnetz Präfix für DHCP über NAT-Unterstützung. Verwenden Sie diesen Parameter nur, wenn DHCP für den Internetzugriff eine gültige IP-Adresse zuweisen kann.  |
| PublicVLan | Optionale | Legt die VLAN-ID Verwenden Sie diesen Parameter nur Host und MAS BGPNAT01 VLAN-ID Zugriff auf physische Netzwerk (und Internet) konfigurieren müssen. Zum Beispiel`.\InstallAzureStackPOC.ps1 –Verbose –PublicVLan 305` |
| Erneut ausführen | Optionale | Verwenden Sie dieses Flag Bereitstellung erneut.  Alle vorherigen Eingaben verwendet. Zuvor bereitgestellte Daten erneut eingeben werden nicht unterstützt, da mehrere eindeutige Werte generiert und für die Bereitstellung verwendet. |
| Zeitserver | Optionale | Verwenden Sie diesen Parameter, wenn Sie einen bestimmten Server angeben müssen. |

## <a name="next-steps"></a>Nächste Schritte

[Verbinden mit Azure Stapel](azure-stack-connect-azure-stack.md)
