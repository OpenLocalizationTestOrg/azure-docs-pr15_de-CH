<properties 
   pageTitle="Verwalten von Geräten mit Snapshot-Manager StorSimple | Microsoft Azure"
   description="Beschreibt die Verwendung StorSimple Snapshot-Manager-MMC-Snap-in zu StorSimple Geräte verwalten."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>Verwenden Sie StorSimple Snapshot-Manager zu StorSimple Geräte verwalten

## <a name="overview"></a>Übersicht

Sie können Knoten im StorSimple Snapshot-Manager- **Bereich** StorSimple importierte Daten überprüfen und Aktualisieren verbundenen Speichergeräte. Beim Anklicken **der Geräteknoten** können Sie eine Liste der angeschlossenen Geräte und entsprechende Informationen im **Ergebnisbereich** angezeigt werden.

![Angeschlossene Geräte](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Abbildung 1: StorSimple Snapshot-Manager-Gerät** 

**Der Ergebnisbereich** zeigt die folgende Informationen zu jedem Gerät abhängig von Ihrer Auswahl **Anzeigen** . (Weitere Informationen zum Konfigurieren einer Ansicht wechseln Sie [im Menü Ansicht](storsimple-use-snapshot-manager.md#view-menu).


| Ergebnisspalte  |Beschreibung          |
|:----------------|:--------------------| 
| Name            | Der Name des Geräts im klassischen Azure-Portal konfiguriert|
| Modell           | Die Modellnummer des Geräts|
| Version         | Die Version der Software auf dem Gerät installiert |
| Status          | Gibt an, ob das Gerät verfügbar ist. |
| Zuletzt synchronisiert     | Datum und Uhrzeit der letzten des Geräts Synchronisierung |
| Seriennr.      | Die Seriennummer des Geräts |
 
Maustaste **Geräteknoten im **Bereichsfenster** ** können Sie Folgendes auswählen:

- Hinzufügen oder Ersetzen eines Geräts 
- Ein Gerät und Imports überprüfen 
- Aktualisieren von Geräten 

Klicken Sie auf **Geräte** -Knoten und anschließend mit der Maustaste ein Gerätename im **Ergebnisbereich** können Sie Folgendes auswählen:

- Ein Gerät authentifizieren 
- Gerätedetails anzeigen 
- Aktualisieren eines Geräts 
- Löschen einer Konfigurations 
- Ändern eines Gerätekennworts

>[AZURE.NOTE] Alle diese Aktionen stehen auch im **Aktionsbereich** .
 
In diesem Lernprogramm erläutert StorSimple Snapshot-Manager zu und Geräte verwalten die folgenden Aufgaben ausführen:

- Hinzufügen oder Ersetzen eines Geräts 
- Ein Gerät und Imports überprüfen 
- Aktualisieren von Geräten 
- Ein Gerät authentifizieren 
- Gerätedetails anzeigen 
- Ein einzelnes Gerät aktualisieren 
- Löschen einer Konfigurations 
- Ein abgelaufenes Gerätekennwort ändern
- Ersetzen eines ausgefallenen Geräts

>[AZURE.NOTE] Allgemeine Informationen über die StorSimple Snapshot-Manager-Benutzeroberfläche dazu [StorSimple Snapshot-Manager-Benutzeroberfläche](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Hinzufügen oder Ersetzen eines Geräts

Gehen Sie zum Hinzufügen oder Ersetzen eines StorSimple-Geräts.

#### <a name="to-add-or-replace-a-device"></a>Hinzufügen oder Ersetzen eines Geräts

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Klicken Sie im **Bereich** klicken Sie **der Geräteknoten** und dann auf **Gerät konfigurieren**. Das Dialogfeld **Gerät konfigurieren** .

    ![StorSimple-Gerät konfigurieren](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 

3. Wählen Sie im Dropdown- **Gerät** die IP-Adresse des Geräts oder virtuelles Gerät. 

4. Geben Sie im Textfeld **Kennwort** das Kennwort StorSimple Snapshot-Manager, das für das Gerät im klassischen Azure-Portal erstellt. Klicken Sie auf **OK**. StorSimple Snapshot-Manager sucht das Gerät, das Sie identifiziert. 

    - Wenn das Gerät verfügbar ist, fügt StorSimple Snapshot-Manager eine Verbindung. 

    - Wenn das Gerät nicht verfügbar ist, gibt StorSimple Snapshot-Manager eine Fehlermeldung zurück. Klicken Sie auf **OK** , um die Fehlermeldung zu schließen, und klicken Sie auf **Abbrechen** , um das Dialogfeld **Gerät konfigurieren** zu schließen.

## <a name="connect-a-device-and-verify-imports"></a>Ein Gerät und Imports überprüfen

Gehen Sie folgendermaßen vor, um StorSimple Gerät und stellen Sie sicher, dass alle vorhandenen Volume-Gruppen, die Backups verknüpft haben importiert werden.

#### <a name="to-connect-a-device-and-verify-imports"></a>Importiert ein Gerät und überprüfen

1. Um ein Gerät zu StorSimple Snapshot-Manager befolgen hinzufügen oder Ersetzen eines Geräts. Verbindung zu einem Gerät antwortet StorSimple Snapshot-Manager wie folgt:

    - Wenn das Gerät nicht verfügbar ist, gibt StorSimple Snapshot-Manager eine Fehlermeldung zurück. 

   - Wenn das Gerät verfügbar ist, fügt StorSimple Snapshot-Manager eine Verbindung. Wählen Sie das Gerät im **Ergebnisbereich** angezeigt, und das Statusfeld gibt an, dass wird das Gerät **verfügbar**. StorSimple Snapshot-Manager importiert alle Volume-Gruppen für das Gerät konfiguriert, Volume-Gruppen Backups zugeordnet haben. Backup-Policies werden nicht importiert. Volume-Gruppen, die nicht zugeordneten Backups werden nicht importiert.

2. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

3. Den obersten Knoten im **Bereichsfenster** Maustaste und anschließend auf **Umschalten Importe anzeigen**.

    ![Wählen Sie umschalten Einfuhren anzeigen](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 

4. Das Dialogfeld **Umschaltbare Bildschirmanzeige auf Einfuhren** wird der Status des importierten Datenträgers Gruppen und Backups angezeigt. Klicken Sie auf **OK**. 

Nach Volume-Gruppen und Backups importiert können StorSimple Snapshot-Manager Sie verwalten wie Volume-Gruppen und Backups, die erstellt und konfiguriert mit StorSimple Snapshot-Manager verwalten soll. 

## <a name="refresh-connected-devices"></a>Aktualisieren von Geräten

Gehen Sie StorSimple Geräte mit StorSimple Snapshot Manager synchronisiert.

####<a name="to-refresh-connected-devices"></a>Aktualisieren von Geräten

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Klicken Sie im **Bereich** klicken Sie **Geräte**und dann auf **Geräte aktualisieren**. Diese synchronisiert die angeschlossenen Geräten mit StorSimple Snapshot-Manager, Volume-Gruppen und Backups, einschließlich Zusätze anzeigen können. 

    ![StorSimple Geräte aktualisieren](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)
 
**Geräte aktualisieren** Aktion ruft alle Volume-Gruppen und zugeordneten Sicherungskopien von Geräten ab. Im Gegensatz zu **Volumes Scannen** Aktion für Knoten **Volumes** **Geräte aktualisieren** nicht backup Registrierung wiederhergestellt werden.

## <a name="authenticate-a-device"></a>Ein Gerät authentifizieren

Gehen Sie zur Authentifizierung ein StorSimple mit StorSimple Snapshot-Manager.

#### <a name="to-authenticate-a-device"></a>Ein Gerät authentifizieren

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Klicken Sie im **Bereich** **Geräte**.

3. Klicken Sie im **Ergebnisbereich** Maustaste auf das Gerät und dann auf **Authentifizierung**.

4. Das Dialogfeld **authentifizieren** wird angezeigt. Geben Sie das Gerät ein, und klicken Sie auf **OK**.

    ![Das Dialogfeld authentifizieren](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 
 
## <a name="view-device-details"></a>Gerätedetails anzeigen

Gehen Sie die Details eines Geräts StorSimple anzeigen und bei Bedarf synchronisieren das Gerät mit StorSimple Snapshot-Manager.

#### <a name="to-view-and-resynchronize-device-details"></a>Zu Gerät synchronisieren

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager.

2. Klicken Sie im **Bereich** **Geräte**.

3. Klicken Sie im **Ergebnisbereich** Maustaste auf das Gerät und dann auf **Details**. 

4. das Dialogfeld **Gerät** . Dieses Feld zeigt, Name, Modell, Version, Seriennummer, Status, Ziel-iSCSI-qualifizierte Namen (IQN) letzte und Synchronisierungsdatum. 

   - Klicken Sie auf **Synchronisieren** , um das Gerät zu synchronisieren.

   - Klicken Sie auf **OK** oder auf **Abbrechen** , um das Dialogfeld zu schließen.

    ![Gerätedetails](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 
 
## <a name="refresh-an-individual-device"></a>Ein einzelnes Gerät aktualisieren

Gehen Sie folgendermaßen vor, um ein einzelnes Gerät StorSimple mit StorSimple Snapshot-Manager neu zu synchronisieren.

#### <a name="to-refresh-a-device"></a>So aktualisieren Sie ein Gerät

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager. 

2. Klicken Sie im **Bereich** **Geräte**. 

3. Klicken Sie im **Ergebnisbereich** Maustaste auf das Gerät und dann auf **Gerät aktualisieren**. Dies wird das Gerät mit StorSimple Snapshot-Manager synchronisiert. 

## <a name="delete-a-device-configuration"></a>Löschen einer Konfigurations

Gehen Sie folgendermaßen vor, eine einzelne StorSimple Konfiguration von StorSimple Snapshot-Manager gelöscht.

#### <a name="to-delete-a-device-configuration"></a>So löschen Sie eine Konfiguration

1. Klicken Sie auf das Desktopsymbol starten StorSimple Snapshot-Manager. 

2. Klicken Sie im **Bereich** **Geräte**. 

3. Klicken Sie im **Ergebnisbereich** Maustaste auf das Gerät und dann auf **Löschen**. 

4. Die folgende Meldung angezeigt. Klicken Sie auf **Ja,** löschen Sie die Konfiguration oder klicken Sie auf **Nein** , um den Löschvorgang abzubrechen.

    ![Konfiguration löschen](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Ein abgelaufenes Gerätekennwort ändern

Geben Sie ein Kennwort für die Authentifizierung ein Geräts StorSimple mit StorSimple Snapshot-Manager ein. Dieses Kennwort wird bei Verwendung die Windows PowerShell-Schnittstelle an das Gerät konfigurieren. Allerdings kann das Kennwort abläuft. In diesem Fall das klassische Azure-Portal können Sie das Kennwort ändern. Da das Gerät StorSimple Snapshot-Manager konfiguriert wurde, bevor das Kennwort abgelaufen ist, müssen Sie das Gerät StorSimple Snapshot-Manager anschließend erneut authentifizieren. 

#### <a name="to-change-the-expired-password"></a>Das abgelaufene Kennwort zu ändern

1. Starten Sie in der Azure-Verwaltungsportal StorSimple Manager-Dienst

2. Klicken Sie auf **Geräte** > **Konfigurieren** für das Gerät.

3. Bildlauf StorSimple Snapshot-Manager-Bereich. Geben Sie ein Kennwort mit 14-15 Zeichen. Stellen Sie sicher, dass das Kennwort aus Großbuchstaben, Kleinbuchstaben, numerischen und Sonderzeichen enthält.

4. Geben Sie das Kennwort zur Bestätigung ein.

5. Klicken Sie am unteren Rand der Seite **Speichern** .

#### <a name="to-re-authenticate-the-device"></a>Um das Gerät erneut authentifizieren

1. StorSimple Snapshot-Manager zu starten.

2. Klicken Sie im **Bereich** **Geräte**. Im **Ergebnisbereich** wird eine Liste von konfigurierten Geräten angezeigt. 

3. Wählen Sie das Gerät mit der rechten Maustaste und klicken **authentifizieren**.

4. Im Fenster **identifizieren** Geben Sie das neue Kennwort ein. 

5. Wählen Sie Geräte, mit der rechten Maustaste und wählen Sie **Geräte aktualisieren**. Dies wird das Gerät mit StorSimple Snapshot-Manager synchronisiert. 

## <a name="replace-a-failed-device"></a>Ersetzen eines ausgefallenen Geräts

Wenn ein StorSimple fehlschlägt und ersetzt durch ein Standby (Failover), gehen Sie zu verbinden, das neue Gerät zugeordneten Backups.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Nach einem Failover auf ein neues Gerät verbinden

1. Konfigurieren der iSCSI-Verbindungs auf das neue Gerät. Informationen finden Sie unter "Schritt 7: Mount, initialisieren und Format eines Volumes" in [Ihrem lokalen StorSimple-Gerät bereitstellen](storsimple-deployment-walkthrough-u2.md). 

>[AZURE.NOTE] Weist das neue StorSimple-Gerät die IP-Adresse wie der alte, Sie für die Verbindung die alte Konfiguration möglicherweise. 

2. Beenden Sie Management von Microsoft StorSimple:

    1. Starten Sie Server-Manager.

    2. Wählen Sie auf dem Schaltpult Server-Manager im Menü **Extras** **Dienste**. 

    3. Wählen Sie im Fenster **Dienste** **Microsoft StorSimple-Verwaltungsdienst**. 

    4. Klicken Sie im rechten Bereich **Microsoft StorSimple-Verwaltungsdienst**auf **Beenden Sie den Dienst**. 

3. Entfernen Sie die Konfigurationsinformationen für das alte Gerät verknüpft: 

    1. Wechseln Sie im Datei-Explorer zu C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    2. Löschen Sie die Dateien im Ordner "BACatalog". 

4. Management von Microsoft StorSimple Dienst: 

    1. Wählen Sie auf dem Schaltpult Server-Manager im Menü **Extras** **Dienste**. 

    2. Wählen Sie im Fenster **Dienste** **Microsoft StorSimple-Verwaltungsdienst**. 

    3. Klicken Sie im rechten **Microsoft StorSimple-Verwaltungsdienst**auf **Dienst neu starten**. 

5. StorSimple Snapshot-Manager zu starten. 

6. Konfigurieren Sie das neue StorSimple-Gerät die Schritte in Schritt2: Verbinden ein StorSimple [StorSimple Snapshot-Manager bereitstellen](storsimple-snapshot-manager-deployment.md). 

7. Maustaste auf Knoten der obersten Ebene im **Bereich (StorSimple Snapshot-Manager im Beispiel)** und klicken Sie auf **Umschalten Einfuhren anzeigen**. 

8. Eine Meldung wird angezeigt, wenn die importierte Volume-Gruppen und Backups StorSimple Snapshot-Manager angezeigt. Klicken Sie auf **OK**. 

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie, wie Sie [StorSimple Snapshot-Manager zum Verwalten der StorSimple Lösung](storsimple-snapshot-manager-admin.md).
- Erfahren Sie, wie Sie [StorSimple Snapshot-Manager anzeigen und Verwalten von Datenträgern](storsimple-snapshot-manager-manage-volumes.md).

