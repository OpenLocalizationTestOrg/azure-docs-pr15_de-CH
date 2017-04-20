<properties
   pageTitle="Erstellen eines Pakets StorSimple Support | Microsoft Azure"
   description="Informationen Sie zum Erstellen, entschlüsseln und Support-Paket für Ihr Gerät StorSimple bearbeiten."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="alkohli" />


# <a name="create-and-manage-a-storsimple-support-package"></a>Erstellen Sie und verwalten Sie einer StorSimple Support-Paket

## <a name="overview"></a>Übersicht

Ein StorSimple Support-Paket ist ein einfach zu verwendende Mechanismus, der alle relevanten Protokolle unterstützen Microsoft Support bei der Fehlerbehebung StorSimple Gerät Probleme gesammelt. Die Protokolle werden verschlüsselt und komprimiert.

Dieses enthält eine schrittweise Anleitung zum Erstellen und verwalten Support-Paket.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Erstellen Sie und Hochladen Sie einer Support-Paket im klassischen Azure-portal

Erstellen und Hochladen einer Support-Paket auf der Microsoft Support-Website über die Seite **Wartung** des Dienstes im klassischen Azure-Portal.

> [AZURE.NOTE] Der Upload benötigt Unterstützung Hauptschlüssel. Ihr Supportbetreuer sollten Sie eine e-Mail-Nachricht werden.

Verschlüsselte und komprimierte Supportpaket (CAB-Datei) erstellt und auf der Support-Website hochgeladen. Supporttechniker kann dieses Paket von der Support-Website für die Problembehandlung abrufen.

Führen Sie folgenden Schritte im Verwaltungsportal Supportpaket erstellen.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Ein Supportpaket im klassischen Azure-Portal erstellen

1. Wählen Sie **Geräte** > **Wartung**.

2. Wählen Sie im Abschnitt **Support Package** **Supportpaket erstellen und Hochladen**.

3. Klicken Sie im Dialogfeld **Erstellen und Hochladen Supportpaket** folgendermaßen Sie vor:

    ![Support-Paket erstellen](./media/storsimple-create-manage-support-package/IC740923.png)

    - Geben Sie im Textfeld **Kennwort unterstützt** den Hauptschlüssel. Microsoft-Supporttechniker sollten dieses Kennwort per e-Mail senden.

    - Das Kontrollkästchen Einwilligung Support-Paket automatisch auf der Microsoft Support-Website hochladen.

    - Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-create-manage-support-package/IC740895.png).


## <a name="manually-create-a-support-package"></a>Ein Supportpaket manuell erstellen

In einigen Fällen müssen Sie das Supportpaket über Windows PowerShell für StorSimple manuell erstellen. Zum Beispiel:

- Wenn Sie vertrauliche Informationen aus den Protokolldateien vor mit Microsoft Support entfernen müssen.

- Wenn das Paket durch Konnektivitätsprobleme hochladen Probleme auftreten.

Sie können manuell generierte Supportpaket für Microsoft Support per e-Mail freigeben. Führen Sie die folgenden Schritte zum Erstellen eines unterstützungspakets in Windows PowerShell für StorSimple.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Zum Erstellen eines unterstützungspakets in Windows PowerShell für StorSimple

1. Starten eine Windows PowerShell-Sitzung als Administrator auf dem Remotecomputer, mit dem eine Verbindung mit dem Gerät StorSimple, geben Sie folgenden Befehl ein:

    `Start PowerShell`

2. In Windows PowerShell-Sitzung eine Verbindung mit Konsole SSAdmin des Geräts:

    - Geben Sie an der Befehlszeile ein:

        `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`

    1. Geben Sie im angezeigten Dialogfeld Administratorkennwort Gerät. Das Standardkennwort lautet:

        `Password1`

        ![Das Dialogfeld Anmeldeinformationen PowerShell](./media/storsimple-create-manage-support-package/IC740962.png)

    2. Wählen Sie **OK**.
    1. Geben Sie an der Befehlszeile ein:

        `Enter-PSSession $MS`

3. Geben Sie in der Sitzung, die geöffnet wird den entsprechenden Befehl.

    - Geben Sie für Aktien, die kennwortgeschützt sind:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`

        Sie werden ein Kennwort, einen Pfad zu dem freigegebenen Netzwerkordner und eine verschlüsselungspassphrase aufgefordert (da das Supportpaket verschlüsselt). Support-Paket wird dann im angegebenen Ordner erstellt.

    - Für Aktien, die nicht kennwortgeschützt sind, müssen Sie nicht die `-Credential` Parameter. Geben Sie Folgendes ein:

        `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`

        Support-Paket ist für beide Controller im Netzwerk freigegebenen Ordner erstellt. Es ist eine verschlüsselte komprimierte Datei, die zur Problembehandlung an den Microsoft Support gesendet werden kann. Weitere Informationen finden Sie unter [Microsoft Support kontaktieren](storsimple-contact-microsoft-support.md).


### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>Export-HcsSupportPackage-Cmdlet-Parameter
Die folgenden Parameter können mit dem Export-HcsSupportPackage-Cmdlet.

| Parameter            | Erforderlich oder Optional | Beschreibung                                                                                                                                                             |
|----------------------|-------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `-Path`                 | Erforderlich          | Verwendung den Speicherort des freigegebenen Netzwerkordners in der Support-Paket platziert wird.                                                                 |
| `-EncryptionPassphrase` | Erforderlich          | Verwendung eine Passphrase unterstützen das Supportpaket verschlüsseln.                                                                                                        |
| `-Credential`           | Optionale          | Zugang für freigegebenen Netzwerkordner Anmeldeinformationen verwenden.                                                                                        |
| `-Force`                | Optionale          | Verwenden Sie Verschlüsselung Passphrase Bestätigungsschritt übersprungen.                                                                                                                |
| `-PackageTag`           | Optionale          | Verwenden Sie, um ein Verzeichnis im *Pfad* angeben, in der Support-Paket platziert wird. Der Standardwert ist [Gerätename]-[aktuell und Time:yyyy-MM-dd-HH-mm-ss].       |
| `-Scope`                | Optionale          | Geben Sie als **Cluster** (Standard) zu einem Support-Paket für beide Controller. Wenn Sie ein Paket nur für den aktuellen Controller erstellen möchten, geben Sie **Controller**. |


## <a name="edit-a-support-package"></a>Ein Supportpaket bearbeiten

Nachdem Sie eine Support-Paket erstellt haben, müssen Sie das Paket entfernen vertrauliche Informationen bearbeiten. Dadurch kann Namen, Gerät IP-Adressen und backup Namen von Protokolldateien enthalten.

> [AZURE.IMPORTANT] Sie können nur ein Supportpaket bearbeiten, die über Windows PowerShell für StorSimple generiert wurde. In klassischen Azure-Portal mit StorSimple Manager erstellten Pakets kann nicht bearbeitet werden.

Support-Paket vor dem Hochladen auf der Microsoft Support-Website bearbeiten zunächst entschlüsseln das Supportpaket, die Dateien bearbeiten und erneut verschlüsseln. Führen Sie die folgenden Schritte aus.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Bearbeiten eines unterstützungspakets in Windows PowerShell für StorSimple

1. Generieren Sie Support-Paket, wie zuvor beschrieben in [Unterstützungspaket in Windows PowerShell für StorSimple erstellen](#to-create-a-support-package-in-windows-powershell-for-storsimple).

2. [Download des Skripts](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokal auf dem Client.

3. Importieren Sie das Windows PowerShell-Modul. Geben Sie den Pfad des lokalen Ordners, in dem das Skript heruntergeladen. Um das Modul zu importieren, geben Sie Folgendes ein:

    `Import-module <Path to the folder that contains the Windows PowerShell script>`

4. Die Dateien sind *AES* -Dateien, die komprimiert und verschlüsselt sind. Dekomprimieren und Entschlüsseln eingeben:

    `Open-HcsSupportPackage <Path to the folder that contains support package files>`

    Beachten Sie, dass die Datei-Erweiterung jetzt alle Dateien angezeigt werden.

    ![Support-Paket bearbeiten](./media/storsimple-create-manage-support-package/IC750706.png)

5. Wenn Sie für die verschlüsselungspassphrase aufgefordert werden, geben Sie das Kennwort, das beim Support-Paket verwendet.

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:EncryptionPassphrase: ****

6. Wechseln Sie zu dem Ordner mit den Protokolldateien. Da die Protokolldateien jetzt dekomprimiert und entschlüsselt werden, haben diese ursprüngliche Dateityp. Ändern Sie diese Dateien alle kundenspezifische Informationen wie Namen und Geräteadressen entfernen und speichern.

7. Schließen Sie die Dateien mit Gzip komprimiert und mit AES-256-Verschlüsselung. Dies ist für Geschwindigkeit und Sicherheit Support-Paket über das Netzwerk übertragen. Komprimieren von Dateien verschlüsseln und geben Sie Folgendes ein:

    `Close-HcsSupportPackage <Path to the folder that contains support package files>`

    ![Support-Paket bearbeiten](./media/storsimple-create-manage-support-package/IC750707.png)

8. Bei Aufforderung bieten Sie eine verschlüsselungspassphrase für geänderte Support-Paket.

        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****

9. Notieren Sie das neue Kennwort, Microsoft Support angefordert freizugeben.


### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Beispiel: Bearbeitet Dateien in einem Unterstützungspaket kennwortgeschützten Freigabe

Das folgende Beispiel veranschaulicht die entschlüsseln, bearbeiten und neu verschlüsseln Support-Paket.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [Supportpakete und Geräte in der gerätebereitstellung beheben](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).

- Informationen zum Verwenden [den StorSimple Manager-Dienst das StorSimple-Gerät verwalten](storsimple-manager-service-administration.md).
