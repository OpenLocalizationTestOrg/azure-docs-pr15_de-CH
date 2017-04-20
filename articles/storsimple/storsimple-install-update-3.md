<properties
   pageTitle="Installieren Sie Update 3 auf Ihrem Gerät StorSimple | Microsoft Azure"
   description="Erläutert das StorSimple 8000-Serie Update 3 StorSimple 8000-Serie installiert."
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
   ms.date="10/05/2016"
   ms.author="alkohli" />

# <a name="install-update-3-on-your-storsimple-device"></a>Installieren Sie Update 3 auf Ihrem Gerät StorSimple

## <a name="overview"></a>Übersicht

In diesem Lernprogramm erläutert das Update 3 auf einem Gerät StorSimple eine frühere Softwareversion über die klassischen Azure-Portal und die Update-Methode installieren. Die Update-Methode wird verwendet, wenn Gateway an eine Schnittstelle Daten 0 StorSimple-Gerät konfiguriert und Sie versuchen, eine Version 1 Software vor dem Update zu aktualisieren.

Update 3 enthält Gerätesoftware, LSI-Treiber und Firmware Storport und Spaceport aktualisiert. Update 2 oder einer früheren Version aktualisieren, werden Sie auch sein iSCSI WMI und in bestimmten Fällen Festplatten-Firmware-Updates erforderlich. Die Gerätesoftware, WMI iSCSI, LSI-Treiber, Spaceport und Storport Updates unterbrechungsfreie Updates und kann über den Azure-Verwaltungsportal angewendet werden. Festplatten-Firmware-Updates disruptive Updates und können nur über die Windows PowerShell-Schnittstelle des Gerätes angewendet werden. 

> [AZURE.IMPORTANT]

> - Eine Reihe von automatischen und manuellen Vorabprüfungen erfolgt vor der Installation Datenspeichergeräten in Hardware-Zustand und Netzwerkkonnektivität zu bestimmen. Kontrollen vor werden nur ausgeführt, wenn die Updates von klassischen Azure-Portal installieren.
> - Wir empfehlen Software und Treiber-Updates über die klassischen Azure-Portal installieren. Sie sollten nur auf die Windows PowerShell-Schnittstelle des Geräts (Updates installieren) gehen, schlägt die Überprüfung vor dem Update Gateway im Portal. Je nach Version von aktualisieren, dauert die Updates 1,5 bis 2,5 Stunden installieren. Wartung-Modus-Updates müssen über die Windows PowerShell-Schnittstelle des Geräts installiert werden. Wartung-Modus Updates disruptive Updates sind, führt diese Ausfallzeiten für Ihr Gerät.
> - Mit den optionalen StorSimple Snapshot-Manager sicher, dass Sie vor der Aktualisierung des Geräts die Snapshot-Manager Version Update 2 aktualisiert haben.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-the-azure-classic-portal"></a>Installieren Sie Update 3 über klassischen Azure-portal

Die folgenden Schritte die Aktualisierung Ihres Geräts mit [3 aktualisieren](storsimple-update3-release-notes.md).


> [AZURE.NOTE]
Anwenden von Update 2 oder höher (einschließlich Update 2.1), können Microsoft zusätzliche Diagnoseinformationen vom Gerät abrufen. Wenn unser Betriebsteam Geräte, die Probleme haben bezeichnet, sind wir daher besser ausgestattet, um Informationen vom Gerät Probleme diagnostizieren und. Akzeptieren Sie Update 2 oder höher, können Sie diesen proaktiven Support zu.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Stellen Sie sicher, dass Ihr Gerät **StorSimple 8000 Serie Update 3 (6.3.9600.17759)**ausgeführt wird. **Datum letzte Aktualisierung** sollte ebenfalls geändert werden. 

    Wenn Sie von einer Version vor Update 2 aktualisieren, Sie sehen auch die Wartung-Modus-Updates verfügbar sind (diese Meldung kann weiterhin für bis zu 24 Stunden nach der Installation der Updates angezeigt werden).

    Wartung-Modus Updates sind unterbrechungsfrei Updates, die Gerät Ausfallzeiten führen und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden. In einigen Fällen beim Ausführen von Update 1.2 Festplatten-Firmware noch aktuell in diesem Fall möglicherweise benötigen Wartung Modus Updates installieren.

    Aktualisieren von Update 2 hingegen sollte Ihr Gerät jetzt auf dem neuesten Stand sein. Sie können die übrigen Schritte überspringen.

13. Die Produktupdates Wartung Modus mithilfe der Schritte in [Hotfixes downloaden](#to-download-hotfixes) suchen und Herunterladen von KB3121899, welche installiert Laufwerk Firmware-Updates (die Updates sollten nun installiert werden).

13. Folgen Sie den Schritten [Installieren und Maintenance Mode-Hotfixes überprüfen](#to-install-and-verify-maintenance-mode-hotfixes) die Wartung-Modus-Updates installieren. 

  

## <a name="install-update-3-as-a-hotfix"></a>Update 3 als Hotfix installieren

Verwenden Sie dieses Verfahren, wenn die Gateway-Überprüfung, beim Versuch fehlschlagen, Updates mithilfe der Azure-Verwaltungsportal zu installieren. Die Überprüfung fehlschlägt, ein Gateway eine 0-DATA-Netzwerkschnittstelle zugewiesen und Ihrem Gerät eine Version vor Update 1 ausgeführt wird.

Softwareversionen, die Methode Hotfix aktualisiert werden können, sind:

- 0.1, 0.2, 0.3 aktualisieren
- 1 1.1, 1.2 aktualisieren
- 2, 2.1, 2.2 aktualisieren 

> [AZURE.IMPORTANT]
>
> - Das Gerät Version (GA) Version ausgeführt wird, wenden Sie sich an [Microsoft Support](storsimple-contact-microsoft-support.md) bei der Aktualisierung.

Die Update-Methode umfasst die folgenden drei Schritte:

1.  Updates von Microsoft Update-Katalog herunterladen

2.  Installieren und den normalen Modus Hotfixes überprüfen.

3.  Installieren und Überprüfen von Hotfix Modus Wartung (nur bei 2 Software vor dem Update aktualisiert).


#### <a name="download-updates-for-your-device"></a>Updates für Ihr Gerät herunterladen

**Wenn Ihr Gerät Update 2.1 oder 2.2 ausgeführt wird**, müssen Sie herunterladen und installieren die folgenden Updates in der vorgeschriebenen Reihenfolge:

| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren  | Installieren |
|--------|-----------|-------------------------|------------- |-------------|
| 1.      | KB3186843 | Softwareupdate & #42;  |  Reguläre <br></br>Unterbrechungsfreie     | ca. 45 Minuten |
| 2.      | KB3186859 | LSI-Treiber und firmware             |  Reguläre <br></br>Unterbrechungsfreie      | ~ 20 Minuten |
| 3.      | KB3121261 | Fix Storport und Spaceport </br> Windows Server 2012 R2 |  Reguläre <br></br>Unterbrechungsfreie      | ~ 20 Minuten |

& #42;  *Hinweis Software Update besteht aus zwei binäre Dateien: Gerät Softwareupdate beginnen mit `all-hcsmdssoftwareupdate` und der Cis und Mds vorangestellt `all-cismdsagentupdatebundle`. Das Softwareupdate Gerät muss vor der Cis und Mds-Agent installiert. Neustart auch aktive Steuerung über die `Restart-HcsController` Cmdlet nach dem Anwenden des Cis und Mds-Agents aktualisieren (und vor dem Anwenden der verbleibenden aktualisiert).* 


**Wenn Ihr Gerät Update 0.1, 0.2, 0.3, 1.0, 1.1, 1.2 oder 2.0 ausgeführt wird**, müssen Sie herunterladen und installieren die folgenden Hotfixes neben Software, LSI-Treiber und Firmware-Updates (siehe in der obigen Tabelle), in der vorgeschriebenen Reihenfolge:

| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren  | Installieren |
|--------|-----------|-------------------------|------------- |-------------|
| 4.      | KB3146621 | iSCSI-Paket | Reguläre <br></br>Unterbrechungsfreie  | ~ 20 Minuten |
| 5.      | KB3103616 | WMI-Paket |  Reguläre <br></br>Unterbrechungsfreie      | ca. 12 Minuten |


<br></br>

**Wenn Ihr Gerät Version 0.2, 0.3 1.0, 1.1 und 1.2 ausgeführt wird**, müssen Sie Laufwerk Firmware-Updates auf alle Updates, die in der vorhergehenden Tabelle gezeigt installieren. Sie können überprüfen, ob benötigen die Festplatten-Firmware-Updates mit den `Get-HcsFirmwareVersion` Cmdlet. Wenn diese Firmwareversionen ausgeführt werden: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, müssen Sie nicht diese Updates zu installieren.


| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren  | Installieren |
|--------|-----------|-------------------------|------------- |-------------|
| 6.      | KB3121899 | Festplatten-firmware              |  Wartung <br></br>Unterbrechungsfreie      | ca. 30 Minuten |
 
<br></br>

> [AZURE.IMPORTANT]
>
> - Dieses Verfahren muss nur einmal durchgeführt werden, um Update 3 anzuwenden. Klassische Azure-Portal können Sie nachfolgende Updates anwenden.
> - Von Update 2.2 aktualisieren, liegt die gesamte Installationszeit 1,1 Stunden.
> - Bevor Sie mit diesem Verfahren die Installation unbedingt Gerätecontroller online sind und alle Hardwarekomponenten fehlerfrei sind.

Führen Sie die folgenden Schritte zum Herunterladen und Installieren von Updates.

[AZURE.INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das [Update 3 enthalten](storsimple-update3-release-notes.md).
