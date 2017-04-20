<properties
   pageTitle="Installieren Sie Update 1.2 auf Ihrem Gerät StorSimple | Microsoft Azure"
   description="Erläutert das StorSimple 8000-Serie Update 1.2 StorSimple 8000-Serie installiert."
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
   ms.date="08/22/2016"
   ms.author="alkohli" />

# <a name="install-update-12-on-your-storsimple-device"></a>Installieren Sie Update 1.2 auf Ihrem Gerät StorSimple

## <a name="overview"></a>Übersicht

In diesem Lernprogramm erläutert das Update 1.2 auf einem StorSimple-Gerät installieren, eine Version vor Update 1 ausgeführt wird. Das Lernprogramm umfasst zusätzliche Schritte für die Aktualisierung einen Gateway für eine Netzwerkschnittstelle Daten 0 StorSimple-Gerät konfiguriert.

Update 1.2 enthält Gerät Softwareupdates und Treiberupdates LSI Disk Firmware-Updates. Software und Treiberupdates LSI unterbrechungsfreie Updates und können über den Azure-Verwaltungsportal angewendet. Festplatten-Firmware-Updates disruptive Updates und können nur über die Windows PowerShell-Schnittstelle des Gerätes angewendet werden.

Je nach Version Ihr Gerät ausgeführt wird, um festzustellen, ob Update 1.2 angewendet wird. Navigieren im **Blick** Abschnitt des Geräts **Dashboard**können Sie die Softwareversion des Geräts überprüfen.

</br>

| Wenn Software Version...   | Was geschieht im Portal?                              |
|---------------------------------|--------------------------------------------------------------|
| Release - GA                    | Wenn Sie Release-Version (GA) ausführen, gelten Sie nicht dieses Update. Bitte [wenden Sie sich an Microsoft Support](storsimple-contact-microsoft-support.md) , um das Gerät zu aktualisieren.|
| 0,1 aktualisieren                      | Portal gilt Update 1.2.                                |
| 0,2 aktualisieren                      | Portal gilt Update 1.2.                                |
| 0,3 aktualisieren                      | Portal gilt Update 1.2.                                |
| Update 1                        | Dieses Update ist nicht verfügbar.                           |
| 1.1 aktualisieren                      | Dieses Update ist nicht verfügbar.                           |

</br>

> [AZURE.IMPORTANT]

> -  Auftreten Update 1.2 nicht sofort, da wir einen phasenbasierten Rollout Updates ausführen. Suche nach Updates in einigen Tagen erneut Aktualisierung bald werden.
> - Dieses Update enthält eine Reihe von automatischen und manuellen Vorabprüfungen Datenspeichergeräten in Hardware-Zustand und Netzwerkkonnektivität zu bestimmen. Kontrollen vor werden nur ausgeführt, wenn die Updates von klassischen Azure-Portal installieren.
> - Wir empfehlen Software und Treiber-Updates über die klassischen Azure-Portal installieren. Sie sollten nur auf die Windows PowerShell-Schnittstelle des Geräts (Updates installieren) gehen, schlägt die Überprüfung vor dem Update Gateway im Portal. Die Updates dauert 5 bis 10 Stunden installieren (einschließlich Windows-Updates). Wartung-Modus-Updates müssen über die Windows PowerShell-Schnittstelle des Geräts installiert werden. Wartung-Modus Updates disruptive Updates sind, führt diese Ausfallzeiten für Ihr Gerät.

[AZURE.INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Installieren Sie Update 1.2 über klassischen Azure-portal

Die folgenden Schritte die Aktualisierung Ihres Geräts 1.2 [Aktualisieren](storsimple-update1-release-notes.md). Verwenden Sie dieses Verfahren nur, wenn ein Gateway auf Daten 0 Netzwerkschnittstelle auf dem Gerät konfiguriert.

[AZURE.INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

12. Stellen Sie sicher, dass Ihr Gerät **StorSimple 8000 Serie Update 1.2 (6.3.9600.17584)**ausgeführt wird. **Datum letzte Aktualisierung** sollte ebenfalls geändert werden. Sie werden auch sehen, dass Wartung Modus gibt (diese Meldung kann weiterhin für bis zu 24 Stunden nach der Installation der Updates angezeigt werden).

    Wartung-Modus Updates sind unterbrechungsfrei Updates, die Gerät Ausfallzeiten führen und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden.

    ![Seite Wartung] (./media/storsimple-install-update-1/InstallUpdate12_10M.png "Seite Wartung")

13. Die Produktupdates Wartung Modus mithilfe der Schritte in [Hotfixes downloaden]( #to-download-hotfixes) suchen und Herunterladen von KB3063416, welche installiert Laufwerk Firmware-Updates (die Updates sollten nun installiert werden).

13. Folgen Sie den Schritten [Installieren und Maintenance Mode-Hotfixes überprüfen](#to-install-and-verify-maintenance-mode-hotfixes) die Wartung-Modus-Updates installieren.

14. Navigieren Sie im klassischen Azure-Portal zu der Seite **Wartung** und am unteren Rand der Seite, klicken Sie auf **Updates überprüfen** überprüfen Sie Windows Update, und klicken Sie auf **Updates installieren**. Sie sind fertig, nachdem alle Updates erfolgreich installiert wurden.



## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Installieren Sie Update 1.2 auf einem Gerät mit einer Gateway-Konfiguration für eine Schnittstelle 0-Daten

Verwenden Sie dieses Verfahren nur, wenn die Gateway-Überprüfung, beim Versuch fehlschlagen, Updates mithilfe der Azure-Verwaltungsportal zu installieren. Die Überprüfung fehlschlägt, ein Gateway eine 0-DATA-Netzwerkschnittstelle zugewiesen und Ihrem Gerät eine Version vor Update 1 ausgeführt wird. Wenn das Gerät kein Gateway an eine Schnittstelle 0-Daten haben, können Sie das Gerät direkt vom klassischen Azure-Portal aktualisieren. Siehe [Installation 1.2 über Azure-Verwaltungsportal zu aktualisieren](#install-update-1.2-via-the-azure-classic-portal).

Software-Versionen, die mit dieser Methode aktualisiert werden können sind Update 0,1, Update 0,2 und 0,3 Updates.


> [AZURE.IMPORTANT]
>
> - Das Gerät Version (GA) Version ausgeführt wird, wenden Sie sich an [Microsoft Support](storsimple-contact-microsoft-support.md) bei der Aktualisierung.
> - Dieses Verfahren muss nur einmal durchgeführt werden, um Update 1.2 anzuwenden. Klassische Azure-Portal können Sie nachfolgende Updates anwenden.

Wenn das Gerät vor dem Update 1 Software ausgeführt wird und ein Gateway für eine Netzwerkschnittstelle Daten 0 festlegen, können Sie Update 1.2 folgt anwenden:

- **Option 1**: Update herunterladen und anwenden mit der `Start-HcsHotfix` Cmdlet von Windows PowerShell-Schnittstelle des Geräts. Dies ist die empfohlene Methode. **Verwenden Sie diese Methode Update 1.2 anwenden, wenn Ihr Gerät Update 1.0 oder 1.1 Update ausgeführt wird.**

- **Option 2**: Entfernen der Gatewaykonfiguration und Installation direkt von der Azure-Verwaltungsportal.


In den folgenden Abschnitten werden Informationen für jeden bereitgestellt.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Option 1: Verwendung Windows PowerShell für StorSimple Update 1.2 als Hotfix anwenden

Verwenden Sie dieses Verfahren nur, wenn Sie Update 0.1, 0.2, 0.3 und Ihr Gateway fehlgeschlagenen beim Versuch, Updates von klassischen Azure-Portal installieren. Ausführen von Software Release (GA) bitte [Microsoft Support](storsimple-contact-microsoft-support.md) , um Ihr Gerät zu aktualisieren.

Update 1.2 als Hotfix installieren, müssen Sie herunterladen und installieren die folgenden Updates:

| Reihenfolge  | KB        | Beschreibung             | Aktualisieren  |
|--------|-----------|-------------------------|------------- |
| 1      | KB3063418 | Software-Aktualisierung         |  Reguläre     |
| 2      | KB3043005 | LSI SAS-Controller-update |  Reguläre     |
| 3      | KB3063416 | Festplatten-firmware           | Wartung  |

Überprüfen Sie bevor Sie mit diesem Verfahren das Update installieren Folgendes:

- Beide Gerätecontroller sind online.

Die folgenden Schritte Update 1.2 anwenden. **Die Updates konnte rund 2 Stunden dauern (ca. 30 Minuten für Software Minuten Treiber 45 für Festplatten-Firmware).**

[AZURE.INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]


## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Option 2: Verwenden der Azure-Verwaltungsportal Update 1.2 nach dem Entfernen der Gateway-Konfigurations anwenden

Dieses Verfahren gilt nur für StorSimple-Geräte, die eine Version vor Update 1 ausgeführt und ein Gateway für eine Netzwerkschnittstelle Daten 0 festgelegt. Sie müssen die Gateway-Einstellung vor Anwendung des Updates deaktivieren.

Die Aktualisierung kann einige Stunden dauern. Wenn Hosts in verschiedenen Subnetzen befinden, führen Entfernen der Gateway-Konfigurations auf den iSCSI-Schnittstellen Ausfallzeiten. Wir empfehlen, Daten 0 für iSCSI-Verkehr, um die Ausfallzeiten reduzieren konfigurieren.

Führen Sie die folgenden Schritte zum Deaktivieren der Schnittstelle und das Gateway und wenden Sie das Update.

[AZURE.INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[AZURE.INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das [Update 1.2 Version](storsimple-update1-release-notes.md).
