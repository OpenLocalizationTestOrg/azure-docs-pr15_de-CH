<properties
   pageTitle="Installieren Sie Update 2 auf dem Gerät StorSimple | Microsoft Azure"
   description="Erläutert das StorSimple 8000-Serie Update 2 StorSimple 8000-Serie installiert."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="install-update-2-on-your-storsimple-device"></a>Installieren Sie Update 2 auf dem StorSimple-Gerät

## <a name="overview"></a>Übersicht

In diesem Lernprogramm erläutert das Update 2 auf einem StorSimple Gerät unter einer früheren Softwareversion über die klassischen Azure-Portal installiert. Das Lernprogramm umfasst auch die Schritte für die Aktualisierung beim Gateway an eine Schnittstelle Daten 0 StorSimple-Gerät konfiguriert und Sie versuchen, eine Version 1 Software vor dem Update zu aktualisieren.

Update 2 enthält Gerät Softwareupdates, LSI Treiberupdates und Festplatten-Firmware-Updates. Die Gerätesoftware LSI Updates unterbrechungsfreie Updates und Azure klassischen Portal angewendet werden. Festplatten-Firmware-Updates disruptive Updates und können nur über die Windows PowerShell-Schnittstelle des Gerätes angewendet werden.

> [AZURE.IMPORTANT]

> -  Update 2 möglicherweise nicht angezeigt sofort, da wir einen phasenbasierten Rollout Updates ausführen. Suche nach Updates in einigen Tagen erneut Aktualisierung bald werden.
> - Eine Reihe von automatischen und manuellen Vorabprüfungen erfolgt vor der Installation Datenspeichergeräten in Hardware-Zustand und Netzwerkkonnektivität zu bestimmen. Kontrollen vor werden nur ausgeführt, wenn die Updates von klassischen Azure-Portal installieren.
> - Wir empfehlen Software und Treiber-Updates über die klassischen Azure-Portal installieren. Sie sollten nur auf die Windows PowerShell-Schnittstelle des Geräts (Updates installieren) gehen, schlägt die Überprüfung vor dem Update Gateway im Portal. Die Updates dauert 4 bis 7 Stunden installieren (einschließlich Windows-Updates). Wartung-Modus-Updates müssen über die Windows PowerShell-Schnittstelle des Geräts installiert werden. Wartung-Modus Updates disruptive Updates sind, führt diese Ausfallzeiten für Ihr Gerät.
> - Mit den optionalen StorSimple Snapshot-Manager sicher, dass Sie vor der Aktualisierung des Geräts die Snapshot-Manager Version Update 2 aktualisiert haben.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>Update 2 über klassischen Azure-Portal installieren

Die folgenden Schritte die Device [Update 2](storsimple-update2-release-notes.md)aktualisieren.


> [AZURE.NOTE]
Update 2 ermöglicht Microsoft zusätzliche Diagnoseinformationen vom Gerät abrufen. Wenn unser Betriebsteam Geräte, die Probleme haben bezeichnet, sind wir daher besser ausgestattet, um Informationen vom Gerät Probleme diagnostizieren und. Update 2 akzeptieren, können Sie diesen proaktiven Support zu.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Stellen Sie sicher, dass Ihr Gerät **StorSimple 8000 Serie Update 2 (6.3.9600.17673)**ausgeführt wird. **Datum letzte Aktualisierung** sollte ebenfalls geändert werden. Sie werden auch sehen, dass Wartung Modus gibt (diese Meldung kann weiterhin für bis zu 24 Stunden nach der Installation der Updates angezeigt werden).

    Wartung-Modus Updates sind unterbrechungsfrei Updates, die Gerät Ausfallzeiten führen und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden. In einigen Fällen beim Ausführen von Update 1.2 Festplatten-Firmware noch aktuell in diesem Fall möglicherweise benötigen Wartung Modus Updates installieren.

13. Die Produktupdates Wartung Modus mithilfe der Schritte in [Hotfixes downloaden](#to-download-hotfixes) suchen und Herunterladen von KB3121899, welche installiert Laufwerk Firmware-Updates (die Updates sollten nun installiert werden).

13. Folgen Sie den Schritten [Installieren und Maintenance Mode-Hotfixes überprüfen](#to-install-and-verify-maintenance-mode-hotfixes) die Wartung-Modus-Updates installieren.


## <a name="install-update-2-as-a-hotfix"></a>Update 2 als Hotfix installieren

Verwenden Sie dieses Verfahren, wenn die Gateway-Überprüfung, beim Versuch fehlschlagen, Updates mithilfe der Azure-Verwaltungsportal zu installieren. Die Überprüfung fehlschlägt, ein Gateway eine 0-DATA-Netzwerkschnittstelle zugewiesen und Ihrem Gerät eine Version vor Update 1 ausgeführt wird.

Software-Versionen, die mit der Update-Methode aktualisiert werden können sind Update 0,1 Update 0,2 und 0,3 Update, Update 1, Update 1.1 und Update 1.2. Die Update-Methode umfasst die folgenden drei Schritte:

- Updates von Microsoft Update-Katalog herunterladen
- Installieren und den normalen Modus Hotfixes überprüfen.
- Installieren und Wartung Modus Hotfix überprüfen.

Update 2 als Hotfix installieren, müssen Sie herunterladen und installieren die folgenden Updates:

| Reihenfolge  | KB        | Beschreibung                    | Aktualisieren  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3121901 | Software-Aktualisierung         |  Reguläre     |
| 2      | KB3121900 | LSI-Treibers              |  Reguläre     |
| 3      | KB3080728 | Storport-Update </br> Windows Server 2012 R2 |  Reguläre     |
| 4      | KB3090322 | Spaceport korrigieren </br> Windows Server 2012 R2 |  Reguläre     |
| 5      | KB3121899 | Festplatten-firmware           | Wartung  |


> [AZURE.IMPORTANT]
>
> - Das Gerät Version (GA) Version ausgeführt wird, wenden Sie sich an [Microsoft Support](storsimple-contact-microsoft-support.md) bei der Aktualisierung.
> - Dieses Verfahren muss nur einmal durchgeführt werden, um Update 2 anzuwenden. Klassische Azure-Portal können Sie nachfolgende Updates anwenden.
> - Jeder Update-Installation dauert etwa 20 Minuten. Gesamte Installation liegt 2 Stunden.
> - Stellen Sie mithilfe dieses Verfahrens die Installation sicher, dass beide Gerätecontroller online sind und alle Hardwarekomponenten fehlerfrei.

Führen Sie die folgenden Schritte aus, um dieses Update als Hotfix anwenden.

[AZURE.INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]



## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das [Update 2-Version](storsimple-update2-release-notes.md).
