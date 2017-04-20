<properties
    pageTitle="Bereitstellen und Verwalten von Backup für Windows Server-Client mithilfe von PowerShell | Microsoft Azure"
    description="Informationen Sie zum Bereitstellen und Verwalten von Azure Backup mit PowerShell"
    services="backup"
    documentationCenter=""
    authors="saurabhsensharma"
    manager="shivamg"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="saurabhsensharma;markgal;jimpark;nkolli;trinadhk"/>


# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a>Bereitstellen und Verwalten von Azure für Windows Server-Windows-Clients mit PowerShell Sicherung

> [AZURE.SELECTOR]
- [ARM](backup-client-automation.md)
- [Classic](backup-client-automation-classic.md)

Dieser Artikel beschreibt wie Sie PowerShell Azure Backup auf Windows Server oder einem Windows-Client einrichten und Verwalten von Backup und Recovery.

## <a name="install-azure-powershell"></a>Azure PowerShell installieren

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

Dieser Artikel konzentriert sich auf Azure Resource Manager (ARM)-PowerShell-Cmdlets, mit denen Sie ein Depot Recovery Services in einer Ressourcengruppe verwenden.

Im Oktober 2015 wurde Azure PowerShell 1.0 veröffentlicht. Diese Version der 0.9.8 erfolgreich freigeben und einige bedeutende Änderungen, insbesondere dem Benennungsmuster Cmdlets. 1.0 Cmdlets folgen dem Benennungsmuster {Verb}-AzureRm {Substantiv;} während der 0.9.8 Namen enthalten keine **Rm** (beispielsweise New-AzureRmResourceGroup anstelle von New AzureResourceGroup). Bei Azure PowerShell 0.9.8 müssen Sie zuerst den Ressourcen-Manager-Modus mit dem Befehl **Switch AzureMode AzureResourceManager** aktivieren. Dieser Befehl ist nicht in 1.0 oder höher erforderlich.

Möchten Sie die Skripts für die 0.9.8 geschrieben Umgebung in Umgebung 1.0 oder höher sollten sorgfältig testen Sie die Skripts in einer vorab vor der Verwendung in der Produktion zu unerwarteten Folgen.

[PowerShell Version herunterladen](https://github.com/Azure/azure-powershell/releases) (erforderliche Mindestversion ist: 1.0.0)


[AZURE.INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Depots Recovery services

Die folgenden Schritte führen Sie durch das Erstellen eines Depots Recovery Services. Ein Depot Recovery Services unterscheidet sich ein Depot Backup.

1. Bei Verwendung von Azure Backup zum ersten Mal verwenden Sie **Register-AzureRMResourceProvider** -Cmdlet, Azure Recovery Service Provider mit Ihrem Abonnement registrieren.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

2. Recovery Services Vault ist eine Ressource ARM müssen innerhalb einer Ressourcengruppe zu platzieren. Sie können eine vorhandene Ressourcengruppe verwenden oder einen neuen erstellen. Wenn Sie eine neue Ressourcengruppe erstellen, geben Sie den Namen und Speicherort für die Ressourcengruppe.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```

3. Verwenden Sie das Cmdlet **Neu AzureRmRecoveryServicesVault** neue Depot erstellen. Achten Sie an demselben Speicherort für das Depot für die Ressourcengruppe verwendet wurde.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```

4. Geben Sie den Speicherredundanz verwenden; Sie können [Lokal redundanter Speicher (LRS)](../storage/storage-redundancy.md#locally-redundant-storage) oder [Geo redundanten Speicher (GRS)](../storage/storage-redundancy.md#geo-redundant-storage). Das folgende Beispiel zeigt, dass die Option BackupStorageRedundancy - TestVault GeoRedundant ist.

    > [AZURE.TIP] Viele Azure Backup-Cmdlets erfordern Recovery Services Vault-Objekt als Eingabe. Aus diesem Grund empfiehlt es sich, das Backup Recovery Services Vault-Objekt in einer Variablen speichern.

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Anzeigen von Depots in einem Abonnement
Verwenden Sie **Get-AzureRmRecoveryServicesVault** alle +++ Liste in das aktuelle Abonnement anzeigen. Sie können diesen Befehl verwenden, zu überprüfen, ob ein neues Depot erstellt wurde und welche Depots im Abonnement verfügbar sind.

Führen Sie den Befehl Get-AzureRmRecoveryServicesVault und alle Depots im Abonnement aufgelistet.

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="installing-the-azure-backup-agent"></a>Azure Backup-Agent installieren
Bevor Sie Azure Backup-Agent installieren, müssen Sie das Installationsprogramm heruntergeladen und vorhanden auf dem Windows-Server. Sie erhalten die neueste Version des Installers aus dem [Microsoft Download Center](http://aka.ms/azurebackup_agent) oder Recovery Services Depot Dashboardseite. Speichern Sie an einem leicht zugänglichen Speicherort wie das Installationsprogramm * C:\Downloads\*.

Um den Agenten zu installieren, führen Sie den folgenden Befehl in einer erhöhten PowerShell-Konsole:

```
PS C:\> MARSAgentInstaller.exe /q
```

Dies installiert den Agenten mit den Standardoptionen. Die Installation dauert einige Minuten im Hintergrund. Wenn Sie nicht die Option */nu* angeben, wird das Fenster **Windows Update** am Ende der Installation nach Updates suchen geöffnet. Nach der Installation wird der Agent in der Liste der installierten Programme angezeigt.

Zum Anzeigen die Liste der installierten Programme gehen **Control**Panel > **Programme** > **Programme und Funktionen**.

![Agent installiert](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a>Installationsoptionen

Wenn Sie alle verwenden Optionen über die Befehlszeile den folgenden Befehl:

```
PS C:\> MARSAgentInstaller.exe /?
```

Die Optionen sind verfügbar:

| Option | Details | Standard |
| ---- | ----- | ----- |
| / q | Installation im stillen Modus | - |
| p: "Speicherort" | Pfad zum Installationsordner für Azure Backup-Agent. | C:\Program Files\Microsoft Azure Services Wiederherstellungsagenten |
| / s: "Speicherort" | Pfad für den Cacheordner Azure Backup-Agent. | C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch |
| / m | Microsoft Update anmelden | - |
| /Nu | Überprüfen Sie nach Abschluss der Installation keine Updates | - |
| / d | Microsoft Azure Recovery Services Agent deinstalliert | - |
| /pH | Proxy-Hostadresse | - |
| /PO | Proxy Server-Portnummer | - |
| /PU | Proxy Server-Benutzername | - |
| / PW | Proxy-Kennwort | - |


## <a name="registering-windows-server-or-windows-client-machine-to-a-recovery-services-vault"></a>Windows Server oder Windows-Clientcomputer zu einem Depot Recovery Services registrieren

Nach der Erstellung Recovery Services Vault herunterladen Sie der neuesten Agent und Vault-Anmeldeinformationen und Lage wie C:\Downloads speichern.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

Führen Sie auf Windows Server oder Windows-Clientcomputer [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) -Cmdlet Registrieren des Computers mit der.

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

> [AZURE.IMPORTANT] Verwenden Sie relative Pfade nicht die Datei Depot an. Sie müssen einen absoluten Pfad als Eingabe für das Cmdlet angeben.

## <a name="networking-settings"></a>Netzwerk-Standardeinstellungen
Wird die Verbindung des Windows-Computers mit dem Internet über einen Proxyserver können die Proxyeinstellungen auch der Agent bereitgestellt werden. In diesem Beispiel ist kein Proxyserver so wir ausdrücklich alle Proxy-Informationen löschen.

Bandbreite kann auch mit den Optionen gesteuert ```work hour bandwidth``` und ```non-work hour bandwidth``` für bestimmte Tage der Woche.

Festlegen von Details Proxy und Bandbreite erfolgt mithilfe des [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) -Cmdlet:

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a>Verschlüsselung
Backup-Daten an Azure Backup gesendet werden verschlüsselt, um Vertraulichkeit der Daten. Die verschlüsselungspassphrase ist "Password" zum Entschlüsseln der Daten zum Zeitpunkt der Wiederherstellung.

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
Server properties updated successfully
```

> [AZURE.IMPORTANT] Bleiben Sie die Passphrase Informationen sicher Nachdem sie festgelegt wurde. Wiederherstellen von Azure ohne diese Passphrase wird nicht können.

## <a name="back-up-files-and-folders"></a>Sichern von Dateien und Ordnern
Alle Backups von Windows-Servern und Clients auf Azure Backup unterliegen einer Richtlinie. Die Richtlinie besteht aus drei Teilen:

1. Ein **Sicherungszeitplan** , die angibt, bei Backups und mit dem Dienst synchronisiert werden müssen.
2. Ein **Aufbewahrungszeitplan** , die angibt, wie lange in Azure Wiederherstellungspunkte beibehalten.
3. **Einschluss/Ausschluss-Spezifikation** , die bestimmt, was gesichert werden soll.

In diesem Dokument da wir Sicherung automatisieren sind gehen wir davon aus, dass nichts konfiguriert wurde. Zunächst erstellen eine neue backup-Richtlinie mit dem [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) -Cmdlet.

```
PS C:\> $newpolicy = New-OBPolicy
```

Zu diesem Zeitpunkt ist die Richtlinie leer und anderen Cmdlets definieren, welche Elemente eingeschlossen oder ausgeschlossen, Backups Ausführung benötigt und die Sicherungskopien gespeichert werden.

### <a name="configuring-the-backup-schedule"></a>Konfigurieren den Sicherungszeitplan
Die erste 3 Teile einer Richtlinie ist backup Zeitplan mithilfe des [Neu-OBSchedule](https://technet.microsoft.com/library/hh770401) -Cmdlets. Der Sicherungszeitplan definiert bei Backups werden müssen. Beim Erstellen eines Zeitplans müssen 2 Eingabeparameter angeben:

- **Tage der Woche** , die Sicherung ausgeführt werden soll. Sie können den Sicherungsauftrag an nur einem Tag oder jeden Wochentag oder eine beliebige Kombination zwischen ausführen.
- **Zeiten** bei die Sicherung ausgeführt werden soll. Sie können bis zu 3 verschiedene Tageszeiten bei die Sicherung ausgelöst wird.

Beispielsweise können Sie eine Sicherungsrichtlinie konfigurieren, die jeden Samstag und Sonntag 16 Uhr ausgeführt.

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

Sicherungszeitplan muss eine Richtlinie zugeordnet werden, und dies kann mithilfe des [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) -Cmdlets.

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a>Konfigurieren einer Aufbewahrungsrichtlinie
Die Aufbewahrungsrichtlinie definiert, wie lange Sicherungsaufträge erstellt Wiederherstellungspunkte gespeichert werden. Erstellung neue Aufbewahrungsrichtlinie mit dem [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) -Cmdlet können Sie die Anzahl der Tage angeben, die Backup-und Recovery-Punkte mit Azure Backup beibehalten. Im folgenden Beispiel wird eine Aufbewahrungsrichtlinie 7 Tage.

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

Die Aufbewahrungsrichtlinie muss mithilfe des [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405)-Cmdlet Hauptrichtlinie zugeordnet werden:

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-to-be-backed-up"></a>Einschließen und Ausschließen von Dateien gesichert werden
Ein ```OBFileSpec``` -Objekt definiert die Dateien enthalten und eine Sicherung ausgeschlossen. Dies ist ein Satz von Regeln, geschützte Dateien und Ordner auf einem Computer Bereich. Sie können so viele Einschluss oder Ausschluss Regeln nach Bedarf Datei und eine Richtlinie zuordnen. Beim Erstellen eines neuen OBFileSpec-Objekts können Sie:

- Geben Sie Dateien und Ordner werden
- Geben Sie Dateien und Ordner ausgeschlossen werden
- Geben Sie rekursive Sicherung der Daten in einem Ordner (oder), ob der obersten Ebene Dateien im angegebenen Ordner gesichert werden sollen, an.

Letzteres wird mit dem nicht - rekursive Flag im Befehl neu OBFileSpec erreicht.

Im folgenden Beispiel wir sichern Volume C: und D: und OS-Binärdateien in den Windows-Ordner und temporäre Ordner ausschließen. Dazu erstellen wir zwei Dateispezifikationen mit dem [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) -Cmdlet - für Aufnahme und Ausschluss. Wenn Dateiangaben erstellt wurden, sind sie der Richtlinie mithilfe des [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) -Cmdlet zugeordnet.

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-the-policy"></a>Anwenden der Richtlinie
Jetzt das Richtlinienobjekt ist abgeschlossen und hat einen zugeordneten Sicherungszeitplan Aufbewahrungsrichtlinie und eine Aufnahme/Ausschlussliste Dateien. Diese Richtlinie kann jetzt für Azure Backup verwendet werden. Bevor Sie übernehmen die neu erstellte Richtlinie sicherzustellen, dass keine vorhandenen backup-Richtlinien Server mithilfe des Cmdlets [Entfernen OBPolicy](https://technet.microsoft.com/library/hh770415) zugeordnet. Entfernen der Richtlinie werden zur Bestätigung aufgefordert. Mit Bestätigung Überspringen der ```-Confirm:$false``` mit dem Cmdlet.

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

Die Richtlinie Objekt erfolgt mithilfe des [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) -Cmdlet. Dies wird auch zur Bestätigung aufgefordert. Mit Bestätigung Überspringen der ```-Confirm:$false``` mit dem Cmdlet.

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

Sie können die Details der vorhandenen backup-Richtlinie mit dem Cmdlet [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) anzeigen. Sie können Detailinformationen weiter über das Cmdlet " [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) " Sicherungszeitplan und das Cmdlet [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) für die Aufbewahrungsrichtlinien

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a>Eine Ad-hoc-Sicherung
Sobald eine Sicherungsrichtlinie festgelegt wurde erfolgt die Sicherung pro Zeitplan. Auslösen einer Ad-hoc-Sicherung kann auch mit dem [Start-OBBackup](https://technet.microsoft.com/library/hh770426) -Cmdlet:

```
PS C:> Get-OBPolicy | Start-OBBackup
Taking snapshot of volumes...
Preparing storage...
Estimating size of backup items...
Estimating size of backup items...
Transferring data...
Verifying backup...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a>Wiederherstellen von Daten von Azure Backup
Dieser Abschnitt führt Sie durch die Schritte zur Automatisierung von Azure Backup Recovery von Daten. Dies umfasst die folgenden Schritte aus:

1. Wählen Sie das Quell-volume
2. Wählen Sie einen Sicherungspunkt wiederherstellen
3. Wählen Sie ein Element wiederherstellen
4. Trigger der Wiederherstellung

### <a name="picking-the-source-volume"></a>Entnahme des Quell-Volumes
Um ein Element von Azure Sicherung wiederherstellen, müssen Sie die Ursache des Elements. Da wir die Befehle im Kontext der Windows-Server oder einem Windows-Client ausführen, wird der Computer bereits identifiziert. Im nächsten Schritt die Quelle ist für das Volume enthalten. Eine Liste der Volumes oder Datenquellen auf diesem Computer gesichert kann abgerufen werden durch das Cmdlet " [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) " ausführen. Dieser Befehl gibt ein Array aller Quellen von diesem Server/Client gesichert.

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-to-restore"></a>Sicherungsort wiederherstellen auswählen
Die Liste der backup kann abgerufen werden durch das Cmdlet " [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) " mit entsprechenden Parametern ausführen. In unserem Beispiel wir wählen Sie die neueste backup für das Quellvolume *D:* und Wiederherstellen eine bestimmte Datei verwenden.

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
Das Objekt ```$rps``` ist ein Array von Punkten backup. Das erste Element ist die neueste und n-te Element ist die älteste. Wählen Sie den aktuellen Punkt verwenden wir ```$rps[0]```.

### <a name="choosing-an-item-to-restore"></a>Auswählen eines Elements wiederherstellen
Verwenden, um die Datei oder den Ordner wiederherstellen identifizieren rekursiv das Cmdlet " [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) ". Auf diese Weise die Ordnerhierarchie ausschließlich durchsucht die ```Get-OBRecoverableItem```.

In diesem Beispiel wollen wir die Datei *finances.xls* wiederherstellen wir können verweisen, das Objekt mit ```$filesFolders[1]```.

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

Sie können auch suchen Elemente wiederherstellen mit der ```Get-OBRecoverableItem``` Cmdlet. In unserem Beispiel zum *finances.xls* suchen erhalten wir ein Handle auf die Datei, indem dieser Befehl:

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a>Auslösen der Wiederherstellung
Um die Wiederherstellung auszulösen, müssen wir Wiederherstellungsoptionen angeben. Dies erfolgt mithilfe des Cmdlets [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) . In diesem Beispiel nehmen wir an, wir *C:\temp*Dateien wiederherstellen möchten. Auch angenommen, wir bereits vorhandene Ordner *C:\temp*überspringen möchten. Um diese Wiederherstellungsoption zu erstellen, verwenden Sie den folgenden Befehl ein:

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

Jetzt Wiederherstellen mit dem Befehl [Start OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) für das ausgewählte auslösen ```$item``` aus der Ausgabe der ```Get-OBRecoverableItem``` Cmdlet:

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a>Azure Backup-Agent deinstallieren
Deinstallieren von Azure Backup-Agent kann mit dem folgenden Befehl erfolgen:

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

Agent-Binärdateien vom Computer deinstalliert hat einige folgen zu berücksichtigen:

- Den Dateifilter aus dem Computer entfernt und Verfolgung von Änderungen beendet.
- Alle Informationen vom Computer entfernt, aber die Informationen weiterhin im Dienst gespeichert werden.
- Alle Sicherungszeitpläne entfernt, und keine weitere Sicherung.

Jedoch Daten in Azure bleibt gespeichert und bleiben nach der Retention Policy-Setup Sie. Ältere Punkte werden automatisch veraltete.

## <a name="remote-management"></a>Remote-management
Das Management Agent Azure Backup, Richtlinien und Datenquellen kann remote über PowerShell durchgeführt werden. Computer, der Remote verwaltet werden muss ordnungsgemäß vorbereitet werden.

Standardmäßig ist der WinRM-Dienst für den manuellen Start konfiguriert. Starttyp auf *automatisch* eingestellt werden, und der Dienst gestartet werden soll. Überprüfen der WinRM-Dienst ausgeführt wird, sollte der Wert der Status-Eigenschaft *ausgeführt*werden.

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

PowerShell sollte für Remoting konfiguriert werden.

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

Der Computer kann jetzt remote - verwaltet werden beginnend mit der Installations des Agenten. Das folgende Skript beispielsweise den Agent auf den Remotecomputer kopiert und installiert.

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter Azure Backup für Windows Server-Client

- [Einführung in Azure Backup](backup-introduction-to-azure-backup.md)
- [Sichern von Windows Server](backup-configure-vault.md)
