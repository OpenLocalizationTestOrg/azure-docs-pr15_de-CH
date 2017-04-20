<properties
    pageTitle="Replizieren Sie virtuelle VMware-Maschinen in Azure Site Recovery mit Azure Automation DSC | Microsoft Azure"
    description="Beschreibt das Azure Automation DSC Azure Site Recovery Mobilität Service und Azure Agent für virtuelle/physische Computer automatisch auf Azure bereitgestellt."
    services="site-recovery"
    documentationCenter=""
    authors="krnese"
    manager="lorenr"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="krnese"/>

# <a name="replicate-vmware-virtual-machines-to-azure-by-using-site-recovery-with-azure-automation-dsc"></a>Replizieren Sie virtuelle VMware-Maschinen mit Site Recovery Azure Automation DSC in Azure

Operations Management Suite bieten wir eine umfassende Backup- und Disaster Recovery-Lösung, die Sie als Teil Ihres Business Continuity-Plans verwenden können.

Wir begannen mit Hyper-V Replica Reise mit Hyper-V. Aber wir haben eine heterogene Setup Unterstützung haben Kunden mehrere Hypervisoren und Plattformen in ihre Wolken erweitert.

Wenn Sie VMware Arbeitslasten oder physischen Server heute ausführen, führt ein Verwaltungsserver Azure Site Recovery-Komponenten in Ihrer Umgebung in Azure ist die Kommunikation und Datenreplikation mit Azure behandelt.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>Mithilfe von Automatisierung DSC bereitstellen Sie Site Recovery Mobilität service
Zunächst folgt eine kurze Aufschlüsselung der diesem Verwaltungsserver.

Der Verwaltungsserver führt mehrere Serverrollen. Eine dieser Rollen ist *Konfiguration*koordiniert Kommunikation und Daten-Replikation und Recovery-Prozesse.

Darüber hinaus ist *Prozess* Rolle als Gateway Replikation. Diese Rolle geschützten Datenquelle Maschinen replizierten Daten empfängt, caching, Komprimierung und Verschlüsselung optimiert und sendet Azure Storage-Konto. Die Funktionen für die Prozessrolle gehört auch push-Installation des Dienstes Mobilität geschützt Maschinen und automatische Erkennung der VMware-VMs.

Ist ein Failback von Azure, behandelt die *master* Zielrolle Replikationsdaten als Teil dieses Vorgangs.

Für geschützte Computer setzen wir auf *mobilitätsservice*. Diese Komponente wird für jede Maschine (VM VMware oder physische Server) bereitgestellt, die Sie in Azure replizieren möchten. Schreibvorgänge auf dem Computer erfasst und leitet sie an den Verwaltungsserver (Prozessrolle).

Beim Umgang mit Business Continuity ist wichtig zu verstehen, die Auslastung Ihrer Infrastruktur und Komponenten. Sie können dann die für die Recovery Time Objective (RTO) und Recovery Point Objective (RPO) erfüllen. In diesem Kontext ist der Mobilität-Taste, um sicherzustellen, dass Ihre Arbeitslasten wie erwartet.

Wie können Sie optimal, sicherstellen, dass Sie zuverlässige protected Setup mit einigen Operations Management Suite-Komponenten?

Dieser Artikel enthält ein Beispiel für die Verwendung Azure Automatisierung gewünschten Zustand Konfiguration (DSC), mit Site Recovery sicherstellen, dass:

- Mobilitätsservice und Azure VM-Agent werden auf Windows-Computern bereitgestellt, die Sie schützen möchten.
- Mobilitätsservice und Azure VM Agent laufen immer bei Azure Replikation soll.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Ein Repository für die Einrichtung speichern
- Ein Repository zum Speichern erforderlichen Kennwort mit dem Management Server registrieren

 > [AZURE.NOTE] Für jeden Verwaltungsserver wird eine eindeutige Passphrase generiert. Wenn Sie mehrere Management-Server bereitstellen möchten, müssen Sie sicherstellen, dass das korrekte Kennwort in der Datei passphrase.txt gespeichert ist.

- Windows Management Framework (WMF) 5.0 auf den Computern Schutz (Voraussetzung für Automation DSC soll)

 > [AZURE.NOTE] DSC für Windows-Computer verwenden, die WMF 4.0 installiert haben, finden Sie im Abschnitt [DSC in einer getrennten Umgebung verwenden](#Use DSC in disconnected environments).

Mobility-Dienst über die Befehlszeile installiert werden und mehrere Argumente akzeptiert. Deshalb müssen Sie die Binärdateien (nach Extrahieren aus dem Setup) und an einer Stelle, wo Sie abrufen können mit DSC-Konfiguration, zu speichern.

## <a name="step-1-extract-binaries"></a>Schritt 1: Extrahieren Binärdateien

1. Wechseln Sie zum Extrahieren der Dateien, die Sie für diese Konfiguration müssen in das folgende Verzeichnis auf dem Verwaltungsserver:

    **\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**

    MSI-Datei sollte in diesem Ordner angezeigt werden:

    **Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**

    Verwenden Sie folgenden Befehl, um das Installationsprogramm zu extrahieren:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe/q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**

2. Wählen Sie alle Dateien, und eine komprimierte ZIP-komprimierten Ordner an.

Sie haben nun die Binärdateien, die Sie zum Automatisieren der Installation des Dienstes Mobilität mit Automatisierung DSC.

### <a name="passphrase"></a>Passphrase

Anschließend müssen Sie bestimmen, wo diese ZIP-Ordner erstellt werden soll. Wie später das Kennwort gespeichert, das für die Installation benötigt, können Sie ein Konto Azure-Speicher. Der Agent wird dann mit dem Verwaltungsserver als Teil des Prozesses registriert.

Die Passphrase haben bei der Bereitstellung des Verwaltungsservers kann in eine Textdatei als passphrase.txt gespeichert werden.

Platzieren Sie ZIP-Ordner und das Kennwort in einem dedizierten Container Azure Speicherkonto.

![Ordner](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Möchten Sie diese Dateien auf einer Netzwerkfreigabe, möchten. Nur müssen sicherstellen, dass DSC-Ressource, die Sie später verwenden werden Zugriff und Setup und Kennwort erhalten.

## <a name="step-2-create-the-dsc-configuration"></a>Schritt 2: Erstellen der DSC-Konfigurations

Das Setup hängt WMF 5.0. WMF 5.0 muss für den Computer die Konfiguration durch Automatisierung DSC erfolgreich installieren vorhanden sein.

Die Umgebung wird Beispiel DSC Konfiguration verwendet:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Die Konfiguration wird Folgendes:

- Die Variablen informiert der Konfiguration wo Sie die Binärdateien für Mobilität und Azure VM-Agent wo Sie die Passphrase, und wo die Ausgabe.
- Importieren der Konfiguration die xPSDesiredStateConfiguration DSC-Ressource die Verwendung `xRemoteFile` die Dateien aus dem Repository herunterladen.
- Die Konfiguration erstellt ein Verzeichnis die Binärdateien gespeichert werden soll.
- Archivressource extrahiert Dateien aus der ZIP-Ordner.
- Das Paket Installationsressource installiert Mobility-Dienst aus der UNIFIEDAGENT. EXE Installer mit bestimmten Argumenten. (Die Variablen, die die Argumente erstellen müssen auf Ihre Umgebung geändert werden.)
- Das Paket AzureAgent Ressource installiert Azure VM-Agent auf jedem virtuellen Computer empfohlen, die in Azure ausgeführt wird. Azure VM-Agent ermöglicht auch nach einem Failover der VM Erweiterungen.
- Service oder mehrere Ressourcen werden sichergestellt, dass die zugehörige Mobilität und Azure Dienste immer ausgeführt werden.

Speichern Sie die Konfiguration als **ASRMobilityService**.

>[AZURE.NOTE] Denken Sie daran, zerebraler Kinderlähmung in Ihrer Konfiguration entsprechend den tatsächlichen Verwaltungsserver so, dass der Agent ordnungsgemäß angeschlossen und das korrekte Kennwort ersetzen.

## <a name="step-3-upload-to-automation-dsc"></a>Schritt 3: Automatisierung DSC hochladen

Da die vorgenommenen Konfiguration der DSC ein benötigtes DSC Ressourcenmodul (xPSDesiredStateConfiguration) importieren, müssen Sie dieses Modul in der Automatisierung vor dem upload der DSC-Konfigurations importieren.

Melden Sie sich bei Ihrem Konto Automatisierung, wechseln Sie zu **Ressourcen** > **Module**, und klicken Sie auf **Galerie durchsuchen**.

Hier können Sie für das Modul suchen und Ihrem Konto importieren.

![Importmodul](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Wenn Sie dies abgeschlossen haben, gehen Sie auf Ihrem Computer, wo Sie Azure Ressourcenmanager Module installiert und fahren Sie mit die neu erstellte DSC-Konfiguration importieren.

### <a name="import-cmdlets"></a>Cmdlets importieren

Melden Sie sich in PowerShell in Azure-Abonnement an. Die Cmdlets entsprechend Ihrer Umgebung und Ihre Kontoinformationen Automatisierung in einer Variablen zu ändern:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Hochladen Sie mit dem folgenden Cmdlet die Konfiguration auf Automatisierung DSC:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>Kompilieren Sie die Konfiguration in Automation DSC

Anschließend müssen Sie die Konfiguration in Automation DSC kompilieren, damit Sie beginnen können, Knoten zu registrieren. Erreichen das folgende Cmdlet ausführen:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Dies kann einige Minuten dauern, da Sie grundsätzlich die Konfiguration für die gehosteten Dienst DSC bereitstellen.

Nach dem Kompilieren der Konfigurations können Sie die Auftragsinformationen mithilfe von PowerShell (Get-AzureRmAutomationDscCompilationJob) oder mithilfe der [Azure-Portal](https://portal.azure.com/)abrufen.

![Aufgabe abrufen](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Sie haben jetzt erfolgreich veröffentlicht und DSC-Konfiguration auf Automatisierung DSC hochgeladen.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Schritt 4: Integrierte Maschinen Automatisierung DSC
>[AZURE.NOTE] Eine Voraussetzung für die Durchführung dieses Szenarios ist, dass Windows-Computer mit der neuesten Version des WMF aktualisiert werden. Sie können downloaden und installieren die richtige Version für Ihre Plattform aus dem [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).

Sie werden nun eine Metaconfig für DSC erstellen, die auf Knoten angewendet wird. Um erfolgreich mit diesem müssen Sie die Endpunkt-URL und den Primärschlüssel für die automatisierungskonto in Azure abzurufen. Sie finden diese Werte unter **Schlüsseln** für **Alle** Blades für Automation-Konto.

![Schlüsselwerte](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

In diesem Beispiel müssen Sie einen Windows Server 2012 R2 physischen Server, den Sie mithilfe von Site Recovery schützen möchten.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Prüfen Sie, ob ausstehende Dateioperationen in der Registrierung umbenennen

Zunächst Automatisierung DSC-Endpunkt des Servers zugeordnet wird empfohlen, für alle ausstehenden Datei Rename Operations in der Registrierung überprüfen. Sie können verhindern, dass Setup aufgrund eines ausstehenden Neustarts abgeschlossen.

Führen Sie das folgende Cmdlet um sicherzustellen, dass es keine ausstehenden Neustarts auf dem Server:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Wenn leere zeigt, sind Sie OK, um fortzufahren. Wenn dies nicht der Fall ist, durch einen Neustart des Servers während eines Wartungszeitfensters sollte adressieren.

Wenden Sie die Konfiguration auf dem Server, PowerShell Integrated Scripting Environment (ISE) starten, und führen Sie das folgende Skript. Dies ist im Wesentlichen eine DSC lokale Konfiguration, die weisen lokale Konfigurations-Manager-Modul Automatisierung DSC-Dienst registriert werden und die spezifische Konfiguration (ASRMobilityService.localhost) abrufen.

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Diese Konfiguration verursacht lokale Konfigurations-Manager-Modul mit Automatisierung DSC registrieren. Darüber hinaus ermittelt die Funktionsweise der Engine, was sie tun sollten, ist eine Konfiguration Drift (ApplyAndAutoCorrect) und wie sie mit der Konfiguration fortgesetzt werden soll, wenn ein Neustart erforderlich ist.

Nach dem Ausführen dieses Skripts sollten die Startknoten Automatisierung DSC registrieren.

![Node-Registrierung durchgeführt](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Wenn Sie Azure-Portal zurückkehren, sehen Sie der neu registrierte Knoten jetzt im Portal angezeigt wird.

![Registrierten Knoten im portal](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Auf dem Server führen Sie das folgende PowerShell-Cmdlet, um sicherzustellen, dass der Knoten ordnungsgemäß registriert wurde:

```powershell
Get-DscLocalConfigurationManager
```

Nachdem die Konfiguration abgerufen und auf den Server angewendet wurde, können Sie dies mit dem folgenden Cmdlet überprüfen:

```powershell
Get-DscConfigurationStatus
```

Die Ausgabe zeigt, dass der Server die Konfiguration erfolgreich abgerufen wurde:

![Ausgabe](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Darüber hinaus hat Dienstinstallation Mobilität eine eigene Protokolldatei *Systemlaufwerk*\ProgramData\ASRSetupLogs finden

Das wars. Sie haben jetzt erfolgreich bereitgestellt und registriert Mobility-Dienst auf dem Computer mit Site Recovery geschützt werden soll. DSC wird sichergestellt, dass immer die erforderlichen Dienste ausgeführt werden.

![Erfolgreiche Bereitstellung](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Nachdem der Verwaltungsserver die erfolgreiche Bereitstellung erkennt, können Schutz konfigurieren und Aktivieren der Replikation auf dem Computer mit Site Recovery.

## <a name="use-dsc-in-disconnected-environments"></a>DSC in einer getrennten Umgebung verwenden

Wenn Ihr Computer mit dem Internet verbunden sind, können Sie immer noch abhängig DSC bereitstellen und Konfigurieren der Mobilität auf Arbeitslast, die Sie schützen möchten.

Instanziieren eigene DSC-Pull-Server in Ihrer Umgebung im Wesentlichen die gleiche Funktionalität bereitstellen, die Automatisierung DSC ab. Kunden ziehen also die Konfiguration DSC-Endpunkt (nachdem es registriert wurde). Eine weitere Option ist jedoch müssen manuell DSC-Konfiguration auf Ihrem Computer entweder lokal oder Remote.

Beachten Sie, dass in diesem Beispiel hinzugefügten Parameter für den Computernamen. Die remote jetzt auf einer Remotefreigabe, die zugänglich sein sollten von den Computern befinden, die Sie schützen möchten. Ende des Skripts erlässt die Konfiguration und startet dann die DSC-Konfiguration auf dem Zielcomputer anwenden.

### <a name="prerequisites"></a>Erforderliche Komponenten

Stellen Sie sicher, dass xPSDesiredStateConfiguration PowerShell Modul. Für Windows-Rechner, WMF 5.0 installiert ist, können Sie das xPSDesiredStateConfiguration-Modul mit dem folgenden Cmdlet auf den Zielcomputern installieren:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Sie können auch herunterladen und speichern Sie das Modul bei müssen Sie auf Windows-Computern verteilen, die WMF 4.0. Führen Sie dieses Cmdlet auf einem Computer ist PowerShellGet (WMF 5.0) vorhanden:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Auch WMF 4.0 sicher, dass [Windows 8.1 aktualisieren KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) auf den Computern installiert ist.

Die folgende Konfiguration kann auf Windows-Computern abgelegt werden, die WMF 5.0 und WMF 4.0:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Eigene DSC Pull-Server im Unternehmensnetzwerk Funktionen imitieren, die man von Automatisierung DSC instanziieren, finden Sie unter [Einrichten von einem Webserver Pull DSC](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Optional: Bereitstellen Sie DSC-Konfiguration mithilfe einer Vorlage Ressourcenmanager Azure

Dieser Artikel konzentriert sich auf Erstellen Sie eine eigene Konfiguration DSC automatisch Service Mobilität und Azure VM Agent - Bereitstellung und stellen Sie sicher, die auf den Computern ausgeführt werden, die Sie schützen möchten. Wir haben auch eine Azure-Ressourcen-Manager-Vorlage, die diese DSC-Konfiguration auf einer neuen oder vorhandenen Azure Automation-Konto bereitstellen. Eingabeparameter verwendet die Vorlage Automatisierung Ressourcen erstellen, die Variablen für Ihre Umgebung.

Nach dem Bereitstellen der Vorlage können Sie einfach mit Schritt 4 in diesem Leitfaden an Bord Ihrer Computer verweisen.

Die Vorlage wird Folgendes:

1. Vorhandenes automatisierungskonto verwenden oder eine neue erstellen
2. Dauern Sie Eingabeparameter:
    - ASRRemoteFile – der Speicherort der Dienstinstallation Mobilität haben
    - ASRPassphrase – der Speicherort die Datei passphrase.txt haben
    - ASRCSEndpoint – Ihr Management-Server die IP-Adresse
3. XPSDesiredStateConfiguration-PowerShell-Modul importieren
4. Erstellen und Kompilieren der DSC-Konfigurations

Die vorangehenden Schritte erfolgt in der richtigen Reihenfolge, Onboarding für den Schutz Ihrer Computer starten können.

Die Vorlage mit einer Anleitung für die Bereitstellung auf [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC)befindet.

Stellen Sie die Vorlage mithilfe von PowerShell bereit:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Bereitstellen der Mobilität Dienst-Agents können Sie für die virtuellen Computer [Aktivieren der Replikation](site-recovery-vmware-to-azure.md#step-6-replicate-applications) .
