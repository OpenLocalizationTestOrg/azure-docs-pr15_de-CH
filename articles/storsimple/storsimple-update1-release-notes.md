<properties 
   pageTitle="Versionshinweise StorSimple 8000-Serie Update 1.2 | Microsoft Azure"
   description="Beschreibt neue Features, Probleme und Abhilfen für StorSimple 8000-Serie Update 1.2."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-12-release-notes"></a>StorSimple 8000-Serie Update 1.2-Versionsinformationen  

## <a name="overview"></a>Übersicht

Die folgenden Versionshinweise Beschreiben der neuen Features und kritisch öffnen StorSimple 8000-Serie Update 1.2 identifizieren. Sie enthalten auch eine Liste von StorSimple Software, Treiber und Laufwerk Firmware-Updates in dieser Version enthalten. 

Update 1.2 kann auf jedem StorSimple-Gerät mit Version (GA), Update 0.1, 0.2 Update oder Aktualisierungssoftware 0,3 angewendet. Update 1.2 ist nicht verfügbar, wenn Ihr Gerät Update 1 oder Update 1.1 ausgeführt wird. Läuft Ihr Gerät Version (GA), bitte [wenden Sie sich an Microsoft Support Services](storsimple-contact-microsoft-support.md) , um Unterstützung bei der Installation dieses Updates.

Die folgende Tabelle listet die Gerät-Softwareversionen Updates 1, 1.1 und 1.2 entspricht.

| Aktualisierung läuft... | Dies ist Ihre Version der Gerätesoftware. |
|---------------------|---------------------------------------|
| Update 1.2          | 6.3.9600.17584                        |
| 1.1 aktualisieren          | 6.3.9600.17521                        |
| Update 1.0          | 6.3.9600.17491                        |

Überprüfen Sie die Informationen in den Release Notes vor die Aktualisierung in der StorSimple-Lösung bereitstellen. Weitere Informationen finden Sie unter [Update 1.2 auf dem StorSimple-Gerät](storsimple-install-update-1.md)installieren. 

>[AZURE.IMPORTANT]
> 
- Es dauert ca. 5 bis 10 Stunden installieren (einschließlich Windows-Updates). 
- Update 1.2 hat Software, LSI-Treibers und Festplatten-Firmware-Updates. Um zu installieren, führen Sie die Schritte [Update 1.2 auf dem StorSimple-Gerät installieren](storsimple-install-update-1.md).
- Neue Versionen nicht auftreten Updates sofort, da wir einen phasenbasierten Rollout Updates ausführen. Suche nach Updates in einigen Tagen erneut wird diese bald verfügbar.


## <a name="whats-new-in-update-12"></a>Was ist neu im Update 1.2

Diese Features wurden zunächst mit Update 1 veröffentlicht, die eine begrenzte Anzahl von Benutzern verfügbar war. Mit dem Update 1.2 sieht die meisten Benutzer StorSimple den folgenden neuen Funktionen und Optimierungen:

- **Migration von 5000-7000-Serie, 8000-Serie Geräte** dieser Version führt eine neue Migrationsfunktion, die die StorSimple 5000-7000 Serie Appliance Benutzer ihre Daten eine StorSimple 8000-Serie physischen Gerät oder virtuelle Appliance migrieren kann. Die Migrationsfunktion hat zwei wichtigsten Verkaufsargumente:                                                                  

    - **Business Continuity**durch Aktivieren der Migration vorhandener Daten auf Geräte 5000-7000-Serie, 8000-Serie.
    - **Verbesserte Funktion Angebote der 8000-Serie Geräte**wie effizient zentrales Management mehrerer Appliances über StorSimple Manager-Dienst besser Hardwarekategorie und aktualisierte Firmware, virtuelle Appliances, Datentransfer und Funktionen in der Wegweiser für die Zukunft.

    Finden Sie die [Migrationshandbuch](http://www.microsoft.com/download/details.aspx?id=47322) Einzelheiten einer StorSimple 5000-7000-Serie ein Gerät der 8000-Serie migrieren. 

- **Verfügbarkeit in Azure Regierungsportal** StorSimple steht in Azure Government-Portal. Siehe [StorSimple-Gerät in Azure Regierungsportal](storsimple-deployment-walkthrough-gov.md)bereitstellen.

- **Unterstützung für andere Cloud-Dienstanbieter** – die andere Cloud-Dienstanbieter unterstützt werden Amazon S3 Amazon S3 Ressourceneinträge, HP und OpenStack (Beta).

- **Aktualisierung auf neueste Speicher-APIs** – wurde auf die neuesten Azure Storage Service-APIs mit dieser Version StorSimple aktualisiert. StorSimple 8000-Serie Geräte, die Software-vor Update 1 Versionen (Version 0.1, 0,2 und 0,3) Version älter als 17 Juli 2009 Azure-Speicher Service APIs. Wie in der aktualisierten [Ankündigung zum Entfernen von Speicher-Service-Versionen](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)1. August 2016 wird diese APIs veraltet. Unbedingt StorSimple 8000-Serie Update 1 vor dem 1. August 2016 anwenden. Wenn Sie dies nicht tun, werden StorSimple Geräte nicht ordnungsgemäß funktioniert.

- **Unterstützung für Zone redundanten Speicher (ZRS)** – die Aktualisierung auf die neueste Version der Speicher-APIs StorSimple 8000-Serie Zone redundanten Speicher (ZRS) neben lokal redundanter Speicher (LRS) und Geo-redundant Storage (GRS) unterstützt. Finden Sie in diesem [Artikel Azure Redundanz Speicheroptionen](../storage/storage-redundancy.md) ZRS.

- **Anfängliche Bereitstellung und Update-Erfahrung verbessert** – wurden In dieser Version, die Installation und Update-Prozesse weiterentwickelt. Installation durch den Setup-Assistenten wurde verbessert, um dem Benutzer Feedback zu geben, die Netzwerk- und Firewall-Einstellungen falsch sind. Zusätzliche Diagnose Cmdlets wurden bereitgestellt, um Ihnen bei der Problembehandlung Netzwerke des Geräts. Siehe die [Problembehandlung Bereitstellung Artikel](storsimple-troubleshoot-deployment.md) Weitere Informationen zu den neuen Diagnose-Cmdlets zur Problembehandlung.

## <a name="issues-fixed-in-update-12"></a>Probleme in Update 1.2

Die folgende Tabelle enthält eine Zusammenfassung der Updates 1.1, 1.2 und 1 behobene Probleme.    


| Nein. | Funktion | Problem | In Update behobene Probleme | Gilt für Gerät | Virtuelles Gerät gilt |
|-----|---------|-------|-----------------|---------------------------------|--------------------------------|
| 1 | Windows PowerShell für StorSimple | Beim Benutzer StorSimple-Gerät Remotezugriff mithilfe von Windows PowerShell für StorSimple und starten den Setup-Assistenten ist ein Absturz, sobald Daten 0 IP wurde. Dieses Problem wurde in Update 1 behoben. | Update 1 | Ja | Ja |
| 2 | Funktion | In einigen Fällen ein Zurücksetzen Factory durchgeführt StorSimple Gerät stecken wurde als diese Meldung angezeigt: **auf Factory zurückgesetzt wird (Phase 8)**. Dies passiert, wenn STRG + C gedrückt wird, während das Cmdlet ausgeführt wurde. Dieser Fehler wurde behoben.| Update 1 | Ja | Nein |
| 3 | Funktion | Nach dem Zurücksetzen einer fehlgeschlagene zwei Controllerfactory konnten Sie geräteregistrierung fortzusetzen. Dies führte zu einer nicht unterstützten Konfiguration. Update 1 eine Fehlermeldung und Registrierung auf einem Gerät blockiert, eine fehlgeschlagene Factory zurückgesetzt wurde. | Update 1 | Ja | Nein |
| 4 | Funktion | In einigen Fällen wurden falsche positive Konflikt Warnungen ausgelöst. Falsche Konflikt Alerts werden auf Geräten mit Update 1 nicht mehr generiert. | Update 1 | Ja | Nein |
| 5 | Funktion | Factory zurücksetzen vor Abschluss unterbrochen wurde, wird das Gerät im Wiederherstellungsmodus eingegeben und lässt Sie StorSimple auf Windows PowerShell nicht. Dieser Fehler wurde behoben. | Update 1 | Ja | Nein |
| 6 | Disaster recovery | Disaster wurde Recovery (DR) behoben wobei DR während der Erkennung von Backups auf dem Zielgerät fehlschlägt. | Update 1 | Ja | Ja |
| 7 | Überwachen der LEDs | In bestimmten Fällen zeigte die LEDs auf der Rückseite der Appliance überwachen nicht korrekten Status an. Die blaue LED wurde deaktiviert. Daten 0 und DATA 1-LEDs wurden blinken sogar wenn diese Schnittstellen nicht konfiguriert wurden. Wurde das Problem behoben und Überwachung LEDs zeigen jetzt den richtigen Status.  | Update 1 | Ja | Nein |
| 8 | Überwachen der LEDs | In bestimmten Fällen deaktiviert nach der Installation von Update 1 blaue Licht auf aktive Domänencontroller dadurch erschwert active Controller identifizieren. Dieses Problem wurde in dieser Patchversion behoben.| Update 1.2 | Ja | Nein |
| 9 | Netzwerk-Schnittstellen | In früheren Versionen konnte ein StorSimple nicht routbare Gateway konfiguriert offline gehen. In dieser Version wurde die Routingmetrik für Daten 0 erstellt die niedrigste; daher, auch wenn andere Netzwerkschnittstellen Cloud aktiviert sind, der clouddatenverkehr vom Gerät Daten 0 weitergeleitet. | Update 1 | Ja | Ja | 
| 10 | Backups | Fehler in Update 1 Backups nach 24 Tagen Fehler verursacht wurde in den Patch Update 1.1 behoben. | 1.1 aktualisieren | Ja | Ja |
| 11 | Backups | Fehler in früheren Versionen führte für Cloud-Snapshots mit geringer Leistung. Dieser Fehler wurde in dieser Patchversion behoben.| Update 1.2 | Ja | Ja |
| 12 | Updates | Ein Fehler, der gemeldet einer fehlgeschlagenen Aktualisierung der Domänencontroller in den Wiederherstellungsmodus wechselt zu Update 1 wurde in diese Patch-Version behoben.| Update 1.2 | Ja | Ja |


## <a name="known-issues-in-update-12"></a>Bekannte Probleme im Update 1.2

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Version.

| Nein. | Funktion | Problem | Kommentare-Lösung | Gilt für Gerät | Virtuelles Gerät gilt |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Datenträger-quorum | In seltenen Fällen Wenn die Mehrzahl der Datenträger im EBOD Gehäuse 8600 Gerät getrennt werden wiederum kein Quorum Datenträger werden dann der Speicherpool offline. Offline bleibt, auch wenn die Datenträger wiederhergestellt werden. | Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft Support, für nächste Schritte. | Ja | Nein |
| 2 | Falsche Controller-ID | Wenn ein Controller ausgetauscht kann Controller 0 als Domänencontroller 1 angezeigt. Während des controlleraustauschs beim Laden des Bildes aus der Peerknoten kann die Controller-ID zunächst Peer Controller ID angezeigt In seltenen Fällen kann dieses Verhalten auch nach einem Neustart des Systems angezeigt. | Es ist keine Benutzeraktion erforderlich. Dies löst sich nach Abschluss der Controller ersetzt. | Ja | Nein |
| 3 | Speicherkonten | Das Speicherkonto löschen mit der Speicherdienst ist ein nicht unterstütztes Szenario. Dies führt zu einer Situation in dem Daten abgerufen werden können. | Ja | Ja |
| 4 | Geräte-failover | Mehrere Failovers eines Containers Volume vom selben Quellgerät für verschiedene Geräte wird nicht unterstützt. Geräte-Failover von einem inaktiven Gerät auf mehreren Geräten machen volumecontainer für das erste Gerät Failover Besitzrechte Daten verlieren. Nach einem Failover dieser volumecontainer angezeigt oder Verhalten sich anders, wenn Sie im klassischen Azure-Portal anzeigen. | | Ja | Nein |
| 5 | Installation | Während StorSimple Adapter für SharePoint-Installation müssen Sie zu einem Gerät IP in Reihenfolge für die Installation erfolgreich abgeschlossen.    | | Ja | Nein |
| 6 | Web-proxy | Verfügt der Web Proxy-Konfiguration angegebene Protokoll HTTPS, Ihr Gerät Service-Kommunikation beeinflusst werden und das Gerät offline gehen. Supportpakete werden auch dabei erhebliche Ressourcen auf Ihrem Gerät generiert werden. | Stellen Sie sicher, dass die Web-Proxy-URL angegebene Protokoll HTTP. Weitere Informationen finden Sie auf den [WebProxy für Ihr Gerät konfigurieren](storsimple-configure-web-proxy.md). | Ja | Nein |
| 7 | Web-proxy | Konfigurieren und aktivieren Webproxy auf einem registrierten Gerät müssen Sie aktiven Controller auf Ihrem Gerät zu starten. | | Ja | Nein |
| 8 | Hohe Cloud Latenz und hoher i/o-Arbeitslast | Stößt Ihr StorSimple Gerät aus Cloud hohe Wartezeiten (Reihenfolge von Sekunden) und hoher i/o-Arbeitslast, Gerät Volumes in eine herabgesetzte und e / schlägt mit einer Fehlermeldung "Gerät nicht bereit". | Sie müssen manuell Gerätecontroller oder ein Gerät Failover zum Beheben des Problems durchführen. | Ja | Nein |
| 9 | Azure PowerShell | Beim Verwenden der StorSimple-Cmdlet Get-AzureStorSimpleStorageAccountCredential **& #124; Select-Object zunächst 1 - warten** markieren Sie das erste Objekt, ein neues **VolumeContainer** -Objekts erstellen das Cmdlet gibt alle Objekte zurück. | Umschließen Sie das Cmdlet Klammern wie folgt: **(Get Azure StorSimpleStorageAccountCredential) & #124; Select-Object zunächst 1 - warten** | Ja | Ja |
| 10| Migration | Mehrere volumecontainer für Migration übergeben werden, wird ETA für letzte Sicherung nur für den ersten volumecontainer. Außerdem kann parallel Migration starten, nachdem die ersten 4 Backups im ersten volumecontainer migriert wurden. | Wir empfehlen eine volumecontainer gleichzeitig migrieren. | Ja | Nein |
| 11| Migration | Volumes werden nach der Wiederherstellung nicht in die Sicherungsrichtlinie oder virtuelle Datenträger hinzugefügt. | Sie müssen diese Volumes backup-Richtlinie hinzufügen, um Backups zu erstellen. | Ja | Ja |
| 12| Migration | Nach Abschluss die Migration muss die migrierten Datencontainer Gerät 5000-7000-Serie nicht zugreifen. | Wir empfehlen die migrierten Daten-Container löschen, nachdem die Migration abgeschlossen ist. | Ja | Nein |
| 13| Klonen und DR | Ein StorSimple Gerät Update 1 nicht duplizieren oder Disaster Recovery an einem Gerät 1 vor dem Update unter. | Sie müssen das Zielgerät 1 diese Operationen aktualisieren aktualisieren | Ja | Ja |
| 14 | Migration | Sicherung der Konfiguration für die Migration möglicherweise auf ein Gerät der Serie 5000-7000 bei Volume-Gruppen mit keine zugeordneten Volumes. | Löschen Sie die leere Volumegruppen mit keine zugeordneten Volumes, und wiederholen Sie die Sicherungskopie.| Ja | Nein |

## <a name="physical-device-updates-in-update-12"></a>Physisches Geräteupdates Update 1.2

Wenn Patch 1.2 auf ein physisches Gerät (mit Versionen vor Update 1) angewendet wird, die Software-Version in 6.3.9600.17584 geändert.

## <a name="controller-and-firmware-updates-in-update-12"></a>Controller und Firmware-Updates in Update 1.2

Diese Version aktualisiert der Treiber und die Festplatten-Firmware auf Ihrem Gerät.
 
- Weitere Informationen zu SAS-Controller-Update finden Sie unter [Update 1 für LSI SAS-Controller in Microsoft Azure StorSimple](https://support.microsoft.com/kb/3043005). 

- Weitere Informationen zu Festplatten-Firmware aktualisieren Sie, finden Sie [Festplatten-Firmware Update 1 für Microsoft Azure StorSimple Appliance](https://support.microsoft.com/kb/3063416).
 
## <a name="virtual-device-updates-in-update-12"></a>Virtuelles Geräteupdates Update 1.2

Dieses Update kann auf das virtuelle Gerät angewendet werden. Neue virtuelle Geräte müssen erstellt werden. 

## <a name="next-steps"></a>Nächste Schritte

- [Update 1.2 auf Ihrem Gerät installieren](storsimple-install-update-1.md).
 
