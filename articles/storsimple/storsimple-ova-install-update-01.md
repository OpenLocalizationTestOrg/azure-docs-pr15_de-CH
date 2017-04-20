<properties 
   pageTitle="Installieren von Updates auf Virtuelles Array StorSimple | Microsoft Azure"
   description="Beschreibt, wie Virtual Array StorSimple Webbenutzeroberfläche Methode Portal und Hotfix Updates installieren"
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
   ms.date="09/07/2016"
   ms.author="alkohli" />

# <a name="install-updates-on-your-storsimple-virtual-array"></a>Installieren der virtuellen StorSimple-Array

## <a name="overview"></a>Übersicht

Dieser Artikel beschreibt die Schritte auf das virtuelle StorSimple Array über lokale Webbenutzeroberfläche und klassischen Azure-Portal installiert. Software-Updates oder Hotfixes StorSimple Virtual Array aktualisieren müssen. 

Denken Sie daran, die ein Update oder Hotfix installieren das Gerät wird neu gestartet. StorSimple Virtual Array ein einzelner Knoten ist, alle i/o bei unterbrochen und das Gerät Erfahrungen Ausfallzeiten. 

Bevor Sie ein Update installieren, wir empfehlen Sie die Volumes oder Freigaben offline auf dem Host erste und dann das Gerät. Mögliche Datenverluste wird minimiert.

> [AZURE.IMPORTANT] Wenn Sie Update 0,1 oder GA Software-Versionen ausführen, müssen Sie Hotfix Methode über lokale Webbenutzeroberfläche Sie Update 0,3 verwenden. Ausführen von Update 0,2 empfiehlt sich die Installation der Updates über klassischen Azure-Portal.

## <a name="use-the-local-web-ui"></a>Die lokale Web-Benutzeroberfläche verwenden 
 
Wenn die lokale Web-Benutzeroberfläche verwenden, umfasst zwei Schritte:

- Downloaden Sie das Update oder hotfix
- Installieren Sie das Update oder hotfix

### <a name="download-the-update-or-the-hotfix"></a>Downloaden Sie das Update oder hotfix

Die folgenden Schritte das Softwareupdate von Microsoft Update-Katalog herunterladen.

#### <a name="to-download-the-update-or-the-hotfix"></a>Downloaden Sie das Update oder hotfix

1. Starten Sie Internet Explorer, und navigieren Sie zu [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Zum ersten Mal auf diesem Computer die Microsoft Update-Katalog verwenden, klicken Sie auf Aufforderung das Microsoft Update-Katalog Add-on installieren **Installieren** .
  
3. Geben Sie im Suchfeld der Microsoft Update-Katalog die Knowledge Base (KB) Anzahl Hotfix herunterladen möchten. Geben Sie **3182061** Update 0,3 ein und dann auf **Suchen**.

    Die Update-Liste angezeigt wird, z. B. **StorSimple Virtual Array Update 0,3**.

    ![Katalog durchsuchen](./media/storsimple-ova-install-update-01/download1.png)

4. Klicken Sie auf **Hinzufügen**. Das Update wird zum Warenkorb hinzugefügt.

5. Klicken Sie auf **Warenkorb ansehen**.

6. Klicken Sie auf **Download**. Geben Sie oder an einen lokalen Speicherort Downloads auf **Durchsuchen** . Die Updates sind am angegebenen Speicherort gedownloadet und in einen Unterordner mit dem gleichen Namen wie die Aktualisierung. Der Ordner kann auch auf einer Netzwerkfreigabe kopiert werden, die vom Gerät erreichen.

7. Kopierten Ordner öffnen, erscheint eine eigenständige Updatepaket Microsoft `WindowsTH-KB3011067-x64`. Diese Datei wird verwendet, um Update oder Hotfix installieren.


### <a name="install-the-update-or-the-hotfix"></a>Installieren Sie das Update oder hotfix

Stellen Sie vor Update oder Hotfix-Installation sicher, dass Sie das Update oder Hotfix heruntergeladen entweder lokal auf dem Host haben oder über eine Netzwerkfreigabe. 

Verwenden Sie diese Methode auf einem GA-Gerät installieren oder aktualisieren 0,1 Softwareversionen. Weniger als 2 Minuten dauert. Die folgenden Schritte Update oder Hotfix installieren.


#### <a name="to-install-the-update-or-the-hotfix"></a>Das Update oder Hotfix installieren

1. Wechseln Sie in den lokalen Webbenutzeroberfläche **Wartung** > **Softwareupdate**.

    ![Gerät aktualisieren](./media/storsimple-ova-install-update-01/update1m.png)

2. **Dateipfad aktualisieren**Geben Sie den Dateinamen für das Update oder den Hotfix. Auch finden die Installationsdatei Update oder Hotfix Sie auf einer Netzwerkfreigabe. Klicken Sie auf **Übernehmen**.

    ![Gerät aktualisieren](./media/storsimple-ova-install-update-01/update2m.png)

3.  Eine Warnung wird angezeigt. Angesichts dieser ist ein einzelner Knoten nach der Aktualisierung, das Gerät wird neu gestartet und Ausfälle. Klicken Sie auf Kontrollkästchen.

    ![Gerät aktualisieren](./media/storsimple-ova-install-update-01/update3m.png)

4. Das Update wird gestartet. Nachdem das Gerät erfolgreich aktualisiert wurde, startet ihn neu. Lokale Benutzeroberfläche ist nicht verfügbar in diesem Zeitraum.

    ![Gerät aktualisieren](./media/storsimple-ova-install-update-01/update5m.png)

5. Nachdem der Neustart abgeschlossen ist, werden Sie zur Seite **Anmelden** . Um sicherzustellen, dass die Gerätesoftware, im lokalen Web-Benutzeroberfläche wurde gehen zur **Wartung** > **Softwareupdate**. Die angezeigten Softwareversion sollte **10.0.0.0.0.10288.0** Update 0,3.

    > [AZURE.NOTE] Wir melden die Softwareversionen in lokalen Webbenutzeroberfläche geringfügig und klassischen Azure-Portal. Beispielsweise meldet der lokalen Webbenutzeroberfläche **10.0.0.0.0.10288** und klassische Azure-Portal meldet **10.0.10288.0** für dieselbe Version. 

    ![Gerät aktualisieren](./media/storsimple-ova-install-update-01/update6m.png)





## <a name="use-the-azure-classic-portal"></a>Verwenden der Azure-Verwaltungsportal

0,2 ausgeführt, sollten Sie Updates über das klassische Azure-Portal installieren. Portal Verfahren muss der Benutzer überprüfen, herunterladen und Installieren der Updates. Dieses Verfahren dauert ca. 7 Minuten. Die folgenden Schritte Update oder Hotfix installieren.

[AZURE.INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

Nach Abschluss der Installation (wie Auftragsstatus 100 %), gehen Sie zu **Geräte > Verwaltung > Softwareupdates**. Die angezeigten Softwareversion sollte 10.0.10288.0 sein.

![Gerät aktualisieren](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [virtuelle StorSimple Array verwalten](storsimple-ova-web-ui-admin.md).
