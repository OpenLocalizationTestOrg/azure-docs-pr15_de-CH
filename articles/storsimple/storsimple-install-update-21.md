<properties
   pageTitle="Installieren Sie Update 2.2 auf Ihrem Gerät StorSimple | Microsoft Azure"
   description="Erläutert das StorSimple 8000-Serie Update 2.2 StorSimple 8000-Serie installiert."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/02/2016"
   ms.author="alkohli" />

# <a name="install-update-22-on-your-storsimple-device"></a>Installieren Sie Update 2.2 auf Ihrem Gerät StorSimple

## <a name="overview"></a>Übersicht

In diesem Lernprogramm erläutert das Update 2.2 auf einem Gerät StorSimple eine frühere Softwareversion über die klassischen Azure-Portal und die Update-Methode installieren. Die Update-Methode wird verwendet, wenn Gateway an eine Schnittstelle Daten 0 StorSimple-Gerät konfiguriert und Sie versuchen, eine Version 1 Software vor dem Update zu aktualisieren.

Update 2.2 enthält Gerätesoftware, WMI und iSCSI-Updates. Aktualisieren von Version 2.1 müssen das Softwareupdate Gerät angewendet werden. Aktualisieren von einer Version vor dem Update 2 auch müssen Sie LSI-Treiber, Spaceport Storport und Festplatten-Firmware-Updates anwenden. Die Gerätesoftware, WMI iSCSI, LSI-Treiber, Spaceport und Storport Updates unterbrechungsfreie Updates und kann über den Azure-Verwaltungsportal angewendet werden. Festplatten-Firmware-Updates disruptive Updates und können nur über die Windows PowerShell-Schnittstelle des Gerätes angewendet werden. 

> [AZURE.IMPORTANT]

> - Eine Reihe von automatischen und manuellen Vorabprüfungen erfolgt vor der Installation Datenspeichergeräten in Hardware-Zustand und Netzwerkkonnektivität zu bestimmen. Kontrollen vor werden nur ausgeführt, wenn die Updates von klassischen Azure-Portal installieren.
> - Wir empfehlen Software und Treiber-Updates über die klassischen Azure-Portal installieren. Sie sollten nur auf die Windows PowerShell-Schnittstelle des Geräts (Updates installieren) gehen, schlägt die Überprüfung vor dem Update Gateway im Portal. Je nach Version von aktualisieren, dauert die Updates 1,5 bis 2,5 Stunden installieren. Wartung-Modus-Updates müssen über die Windows PowerShell-Schnittstelle des Geräts installiert werden. Wartung-Modus Updates disruptive Updates sind, führt diese Ausfallzeiten für Ihr Gerät.
> - Mit den optionalen StorSimple Snapshot-Manager sicher, dass Sie die Snapshot-Manager Version 2.2 Update aktualisiert haben, vor der Aktualisierung des Geräts.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-the-azure-classic-portal"></a>Installieren Sie Update 2.2 über klassischen Azure-portal

Die folgenden Schritte Ihr Gerät [2.2 aktualisieren](storsimple-update21-release-notes.md)aktualisieren.


> [AZURE.NOTE]
Anwenden von Update 2 oder höher (einschließlich Update 2.1), können Microsoft zusätzliche Diagnoseinformationen vom Gerät abrufen. Wenn unser Betriebsteam Geräte, die Probleme haben bezeichnet, sind wir daher besser ausgestattet, um Informationen vom Gerät Probleme diagnostizieren und. Akzeptieren Sie Update 2 oder höher, können Sie diesen proaktiven Support zu.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Stellen Sie sicher, dass Ihr Gerät **StorSimple 8000 Serie Update 2.2 (6.3.9600.17708)**ausgeführt wird. **Datum letzte Aktualisierung** sollte ebenfalls geändert werden. 

    Wenn Sie von einer Version vor Update 2 aktualisieren, Sie sehen auch die Wartung-Modus-Updates verfügbar sind (diese Meldung kann weiterhin für bis zu 24 Stunden nach der Installation der Updates angezeigt werden).

    Wartung-Modus Updates sind unterbrechungsfrei Updates, die Gerät Ausfallzeiten führen und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden. In einigen Fällen beim Ausführen von Update 1.2 Festplatten-Firmware noch aktuell in diesem Fall möglicherweise benötigen Wartung Modus Updates installieren.

    Aktualisieren von Update 2 sollte Ihr Gerät jetzt auf dem neuesten Stand sein. Sie können die übrigen Schritte überspringen.

13. Die Produktupdates Wartung Modus mithilfe der Schritte in [Hotfixes downloaden](#to-download-hotfixes) suchen und Herunterladen von KB3121899, welche installiert Laufwerk Firmware-Updates (die Updates sollten nun installiert werden).

13. Folgen Sie den Schritten [Installieren und Maintenance Mode-Hotfixes überprüfen](#to-install-and-verify-maintenance-mode-hotfixes) die Wartung-Modus-Updates installieren. 

  

## <a name="install-update-22-as-a-hotfix"></a>2.2 aktualisieren als Hotfix installieren

Verwenden Sie dieses Verfahren, wenn die Gateway-Überprüfung, beim Versuch fehlschlagen, Updates mithilfe der Azure-Verwaltungsportal zu installieren. Die Überprüfung fehlschlägt, ein Gateway eine 0-DATA-Netzwerkschnittstelle zugewiesen und Ihrem Gerät eine Version vor Update 1 ausgeführt wird.

Softwareversionen, die Methode Hotfix aktualisiert werden können, sind:

- 0.1, 0.2, 0.3 aktualisieren
- 1 1.1, 1.2 aktualisieren
- Update 2 2.1 

> [AZURE.IMPORTANT]
>
> - Das Gerät Version (GA) Version ausgeführt wird, wenden Sie sich an [Microsoft Support](storsimple-contact-microsoft-support.md) bei der Aktualisierung.

Die Update-Methode umfasst die folgenden drei Schritte:

- Updates von Microsoft Update-Katalog herunterladen
- Installieren und den normalen Modus Hotfixes überprüfen.
- Installieren und Überprüfen von Hotfix Modus Wartung (nur bei 2 Software vor dem Update aktualisiert).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Herunterladen Sie Updates für ein Gerät Update 2.1-software

**Läuft Ihr Gerät 2.1 aktualisieren**, müssen Sie nur das Device Update KB3179904 herunterladen. Installieren Sie nur die Binärdatei "alle Hcsmdssoftwareudpate" vorangestellt. Installieren der Cis und MDS Agentupdate vorangestellt `all-cismdsagentupdatebundle`. Andernfalls führt zu einem Fehler. Dies ist eine unterbrechungsfreie Aktualisierung, e/a nicht unterbrochen und das Gerät keinen Ausfallzeiten.


#### <a name="download-updates-for-a-device-running-update-2-software"></a>Laden Sie Updates für ein Gerät unter Update 2

**Wenn Ihr Gerät Update 2 läuft**, müssen Sie herunterladen und installieren die folgenden Updates in der vorgeschriebenen Reihenfolge:

| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren  | Installieren |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3179904 | Softwareupdate & #42;  |  Reguläre <br></br>Unterbrechungsfreie     | ca. 45 Minuten |
| 2.      | KB3146621 | iSCSI-Paket | Reguläre <br></br>Unterbrechungsfreie  | ~ 20 Minuten |
| 3.      | KB3103616 | WMI-Paket |  Reguläre <br></br>Unterbrechungsfreie      | ca. 12 Minuten |


 & #42;  *Hinweis Software Update besteht aus zwei binäre Dateien: Gerät Softwareupdate beginnen mit `all-hcsmdssoftwareupdate` und der Cis und Mds vorangestellt `all-cismdsagentupdatebundle`. Das Softwareupdate Gerät muss vor der Cis und Mds-Agent installiert. Neustart auch aktive Steuerung über die `Restart-HcsController` Cmdlet nach dem Anwenden des Cis und MDS-Agents aktualisieren (und vor dem Anwenden der verbleibenden aktualisiert).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Laden Sie Updates für ein Gerät 2 Software vor dem Update

**Läuft Ihr Gerät Versionen 0,2 und 0,3, 1.0, 1.1**, downloaden Sie und installieren die LSI-Treiber und Firmware aktualisieren Software, iSCSI und WMI-Updates. Dieses Update ist bereits installiert, wenn Sie Update 1.2 oder 2 ausführen. 
 
| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren  | Installieren |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3121900 | LSI-Treiber und firmware             |  Reguläre <br></br>Unterbrechungsfreie      | ~ 20 Minuten |


<br></br>
**Wenn das Gerät Version 0.2, 0.3 1.0, 1.1 und 1.2 ausgeführt wird**, müssen Sie herunterladen und Installieren der Spaceport und der Storport-Update. Ausführen von Update 2 werden bereits installiert.

| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren  | Installieren |
|--------|-----------|-------------------------|------------- |-------------|
| 5.      | KB3090322 | Spaceport korrigieren </br> Windows Server 2012 R2 |  Reguläre <br></br>Unterbrechungsfreie      | ~ 20 Minuten |
| 6.      | KB3080728 | Storport-Update </br> Windows Server 2012 R2 |  Reguläre <br></br>Unterbrechungsfreie      | ~ 20 Minuten |



<br></br>
Sie müssen auch Laufwerk Firmware-Updates installieren. Sie können überprüfen, ob benötigen die Festplatten-Firmware-Updates mit den `Get-HcsFirmwareVersion` Cmdlet. Wenn diese Firmwareversionen ausgeführt werden: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, müssen Sie nicht diese Updates zu installieren.


| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren  | Installieren |
|--------|-----------|-------------------------|------------- |-------------|
| 7.      | KB3121899 | Festplatten-firmware              |  Wartung <br></br>Unterbrechungsfreie      | ca. 30 Minuten |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Dieses Verfahren muss nur einmal durchgeführt werden, um Updates 2.2 anzuwenden. Klassische Azure-Portal können Sie nachfolgende Updates anwenden.
> - Aktualisieren von Update 2 liegt die gesamte Installationszeit 1,5 Stunden.
> - Bevor Sie mit diesem Verfahren die Installation unbedingt Gerätecontroller online sind und alle Hardwarekomponenten fehlerfrei sind.

Führen Sie die folgenden Schritte zum Herunterladen und Installieren von Updates.

[AZURE.INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Update 2.1 Version](storsimple-update21-release-notes.md).
