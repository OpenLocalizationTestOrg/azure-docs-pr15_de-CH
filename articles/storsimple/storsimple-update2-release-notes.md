<properties 
   pageTitle="Versionshinweise StorSimple 8000-Serie Update 2 | Microsoft Azure"
   description="Beschreibt neue Features, Probleme und Abhilfen für StorSimple 8000-Serie Update 2."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
 <tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-2-release-notes"></a>StorSimple 8000-Serie Update 2-Versionsinformationen  

## <a name="overview"></a>Übersicht

Die folgenden Versionshinweise Beschreiben der neuen Features und öffnen kritisch für StorSimple 8000-Serie Update 2 identifizieren. Sie enthalten auch eine Liste von StorSimple Software, Treiber und Laufwerk Firmware-Updates in dieser Version enthalten. 

Update 2 kann jederzeit StorSimple Update 1.2 Version (GA) oder Update 0,1 durchgehend angewendet werden. Update 2 zugeordnete Geräteversion ist 6.3.9600.17673.

Überprüfen Sie die Informationen in den Release Notes vor die Aktualisierung in der StorSimple-Lösung bereitstellen.

>[AZURE.IMPORTANT]
> 
- Es dauert ungefähr 4 bis 7 Stunden (einschließlich der Windows-Updates) installieren. 
- Update 2 hat Software, LSI-Treibers und SSD Firmware-Updates.
- Neue Versionen nicht auftreten Updates sofort, da wir einen phasenbasierten Rollout Updates ausführen. Warten Sie einige Tage, und dann nach Updates als diese wird bald verfügbar.


## <a name="whats-new-in-update-2"></a>Was ist neu im Update 2

Update 2 führt die folgenden neuen Funktionen.

- **Lokal fixiert Volumes** – wurden In früheren Versionen der StorSimple 8000-Serie Datenblöcke Ebenen in die Cloud basierend auf der Verwendung. Gab es keine Möglichkeit sicherzustellen, dass Blöcke auf bleiben. Update 2 Wenn Sie ein Volume erstellen können Sie ein Volume festlegen als lokales und primäre Daten von diesem Volume nicht in die Cloud gestuft werden. Snapshots lokale Volumes werden noch in die Cloud für die Sicherung kopiert, sodass Cloud für Data Mobility und Disaster Recovery-Zwecke verwendet werden kann. Außerdem können Sie den Datenträgertyp ändern (also Convert tiered-Volumes auf lokale Volumes und Volumes konvertieren lokal fixierten Ebenen). 

- **StorSimple virtuelles Gerät verbessert** – positioniert zuvor StorSimple 8000-Serie virtuelle Gerät als Disaster Recovery oder Entwicklung/Test. Es war nur ein virtuelles Gerät (Modell 1100). Update 2 führt zwei virtuelle Modelle: 

     - 8010 (früher: 1100) – keine Änderung; hat eine Kapazität von 30 TB und Azure Standardspeicher verwendet.
     - 8020-hat eine Kapazität von 64 TB und Azure Premium Speicher zur Verbesserung der Leistung verwendet.

    Es gibt eine einzelne VHD für beide Modelle virtuelles Gerät (8010/8020). Beim Starten von virtuellen Geräts erkennt die Plattform-Parameter und wendet die richtige Version.

- **Verbesserte Netzwerk** -Update 2 verbessert die folgenden Netzwerke:

     - NICs können Failover möglich, fällt eine Netzwerkkarte für die Cloud aktiviert werden.
     - Routing verbessert mit festen Metriken für Cloud aktiviert blockiert.
     - Online-Wiederholung ausgefallene Ressourcen vor einem Failover.
     - Neue Alarme für Dienstausfälle.

- **Verbesserte aktualisieren** – Update 1.2 und früher StorSimple 8000-Serie über zwei Kanäle aktualisiert wurde: Windows Update für clustering, iSCSI, usw. und Microsoft Update für Binärdateien und Firmware.
    Update 2 verwendet Microsoft Update für alle-Pakete Update. Dies sollte weniger Zeit Patches oder bei Failover führen. 

- **Firmware-Updates** die folgenden Firmware-Updates sind enthalten:
    - LSI: lsi_sas2.sys Produktversion 2.00.72.10
    - Nur SSD (keine Updates HDD): XMGG, XGEG, KZ50, F6C2 und VR08

- **Proaktiver Support** – Update 2 ermöglicht Microsoft zusätzliche Diagnoseinformationen vom Gerät abrufen. Wenn unser Betriebsteam Geräte, die Probleme haben identifiziert, können wir besser vom Gerät und Probleme diagnostizieren. **Durch das Update 2 akzeptieren können zu diesen proaktiven Support**.    
 

## <a name="issues-fixed-in-update-2"></a>Probleme in Update 2

Die folgenden Tabellen enthalten eine Zusammenfassung der Updates 2 behobene Probleme.    

| Nein. | Funktion | Problem | Gilt für Gerät | Virtuelles Gerät gilt |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Netzwerk-Schnittstellen | Nach einer Aktualisierung auf Update 1 zufolge der StorSimple Manager-Dienst Fehler Ports Data2 und Data3 auf einem Domänencontroller. Dieses Problem wurde behoben. | Ja | Nein |
| 2 | Updates | Nach einer Aktualisierung auf Update 1 akustischen Alarm Alarme im klassischen Azure-Portal auf mehreren Geräten ist aufgetreten. Dieses Problem wurde behoben. | Ja | Nein |
| 3 | Openstack Authentifizierung | Bei Verwendung von Openstack als Cloud Service Provider konnte Sie Fehlermeldung, dass die Authentifizierungszeichenfolge Cloud zu lang war. Dies wurde behoben. | Ja | Nein |


## <a name="known-issues-in-update-2"></a>Bekannte Probleme im Update 2

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Version.

| Nein. | Funktion | Problem | Kommentare / Abhilfe | Gilt für Gerät | Virtuelles Gerät gilt |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Datenträger-quorum | In seltenen Fällen Wenn die Mehrzahl der Datenträger im EBOD Gehäuse 8600 Gerät getrennt werden wiederum kein Quorum Datenträger geht der Speicherpool offline. Offline bleibt, auch wenn die Datenträger wiederhergestellt werden. | Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft Support, für nächste Schritte. | Ja | Nein |
| 2 | Falsche Controller-ID | Wenn ein Controller ausgetauscht kann Controller 0 als Domänencontroller 1 angezeigt. Während des controlleraustauschs beim Laden des Bildes aus der Peerknoten kann die Controller-ID zunächst Peer Controller ID angezeigt In seltenen Fällen kann dieses Verhalten auch nach einem Neustart des Systems angezeigt. | Es ist keine Benutzeraktion erforderlich. Dies löst sich nach Abschluss der Controller ersetzt. | Ja | Nein |
| 3 | Speicherkonten | Das Speicherkonto löschen mit der Speicherdienst ist ein nicht unterstütztes Szenario. Dies führt zu einer Situation in dem Daten abgerufen werden können.|  | Ja | Ja |
| 4 | Geräte-failover | Mehrere Failovers eines Containers Volume vom selben Quellgerät für verschiedene Geräte wird nicht unterstützt. Failover von einem inaktiven Gerät auf mehreren Geräten machen volumecontainer für das erste Gerät Failover Besitzrechte Daten verlieren. Nach einem Failover dieser volumecontainer angezeigt oder Verhalten sich anders, wenn Sie im klassischen Azure-Portal anzeigen. | | Ja | Nein |
| 5 | Installation | Während StorSimple Adapter für SharePoint-Installation müssen Sie zu einem Gerät IP in Reihenfolge für die Installation erfolgreich abgeschlossen.    | | Ja | Nein |
| 6 | Web-proxy | Verfügt der Web Proxy-Konfiguration angegebene Protokoll HTTPS, Ihr Gerät Service-Kommunikation beeinflusst werden und das Gerät offline gehen. Supportpakete werden auch dabei erhebliche Ressourcen auf Ihrem Gerät generiert werden. | Stellen Sie sicher, dass die Web-Proxy-URL angegebene Protokoll HTTP. Weitere Informationen finden Sie auf den [WebProxy für Ihr Gerät konfigurieren](storsimple-configure-web-proxy.md). | Ja | Nein |
| 7 | Web-proxy | Konfigurieren und aktivieren Webproxy auf einem registrierten Gerät müssen Sie aktiven Controller auf Ihrem Gerät zu starten. | | Ja | Nein |
| 8 | Hohe Cloud Latenz und hoher i/o-Arbeitslast | Stößt Ihr StorSimple Gerät aus Cloud hohe Wartezeiten (Reihenfolge von Sekunden) und hoher i/o-Arbeitslast, Gerät Volumes in eine herabgesetzte und e / schlägt mit einer Fehlermeldung "Gerät nicht bereit". | Sie müssen manuell Gerätecontroller oder ein Gerät Failover zum Beheben des Problems durchführen. | Ja | Nein |
| 9 | Azure PowerShell | Beim Verwenden der StorSimple-Cmdlet Get-AzureStorSimpleStorageAccountCredential **& #124; Select-Object zunächst 1 - warten** markieren Sie das erste Objekt, ein neues **VolumeContainer** -Objekts erstellen das Cmdlet gibt alle Objekte zurück. | Umschließen Sie das Cmdlet Klammern wie folgt: **(Get Azure StorSimpleStorageAccountCredential) & #124; Select-Object zunächst 1 - warten** | Ja | Ja |
| 10| Migration | Mehrere volumecontainer für Migration übergeben werden, wird ETA für letzte Sicherung nur für den ersten volumecontainer. Außerdem kann parallel Migration starten, nachdem die ersten 4 Backups im ersten volumecontainer migriert wurden. | Wir empfehlen eine volumecontainer gleichzeitig migrieren. | Ja | Nein |
| 11| Migration | Volumes werden nach der Wiederherstellung nicht in die Sicherungsrichtlinie oder virtuelle Datenträger hinzugefügt. | Sie müssen diese Volumes backup-Richtlinie hinzufügen, um Backups zu erstellen. | Ja | Ja |
| 12| Migration | Nach Abschluss die Migration muss die migrierten Datencontainer Gerät 5000-7000-Serie nicht zugreifen. | Wir empfehlen die migrierten Daten-Container löschen, nachdem die Migration abgeschlossen ist. | Ja | Nein |
| 13| Klonen und DR | Ein StorSimple Gerät Update 1 nicht duplizieren oder Wiederherstellung auf ein Gerät unter 1 Software vor dem Update. | Sie müssen das Zielgerät 1 diese Operationen aktualisieren aktualisieren | Ja | Ja |
| 14 | Migration | Sicherung der Konfiguration für die Migration möglicherweise auf ein Gerät der Serie 5000-7000 bei Volume-Gruppen mit keine zugeordneten Volumes. | Löschen Sie die leere Volumegruppen mit keine zugeordneten Volumes, und wiederholen Sie die Sicherungskopie.| Ja | Nein |
| 15 | Azure PowerShell-Cmdlets und lokale volumes | Sie können keine lokalen Volumes über Azure PowerShell-Cmdlets erstellen. (Ein Volume über Azure PowerShell erstellten wird gestuft werden.) |Verwenden des StorSimple Manager-Dienstes immer lokale Volumes konfigurieren.| Ja | Nein |
| 16 |Speicherplatz für lokale volumes | Wenn Sie ein lokales Volume löschen, kann Platz für neue Volumes nicht sofort aktualisiert. StorSimple Manager-Dienst wird im lokalen Speicherplatz ca. stündlich aktualisiert.| Warten Sie eine Stunde vor das neue Volume erstellen. | Ja | Nein |
| 17 | Lokale volumes | Der Wiederherstellungsauftrag macht temporäre Snapshotsicherung der Sicherungskatalog jedoch nur für die Dauer der Wiederherstellungsauftrag. Darüber hinaus stellt eine virtuelle Laufwerkgruppe mit Präfix **TmpCollection** auf der Seite **Sicherung** jedoch nur für die Dauer der Wiederherstellungsauftrag. | Dieses Verhalten kann auftreten, wenn der Wiederherstellungsauftrag nur lokale Volumes oder eine Kombination aus lokal fixierte und tiered-Volumes fixiert wurde. Wenn der Wiederherstellungsauftrag mehrstufige Volumes enthält, wird dieses Verhalten nicht auftreten. Es ist kein Benutzereingriff erforderlich. | Ja | Nein |
| 18 | Lokale volumes | Stornieren Sie einen Wiederherstellungsauftrag und Controller-Failover tritt unmittelbar anschließend Wiederherstellungsauftrag **konnte** **abgebrochen**angezeigt wird. Wenn ein Wiederherstellungsauftrag fehlschlägt und einem Controller Failover sofort anschließend Wiederherstellungsauftrag statt **Fehler** **abgebrochen** angezeigt. | Dieses Verhalten kann auftreten, wenn der Wiederherstellungsauftrag nur lokale Volumes oder eine Kombination aus lokal fixierte und tiered-Volumes fixiert wurde. Wenn der Wiederherstellungsauftrag mehrstufige Volumes enthält, wird dieses Verhalten nicht auftreten. Es ist kein Benutzereingriff erforderlich. | Ja | Nein |
| 19 |Lokale volumes | Wenn Sie einen Wiederherstellungsauftrag abbrechen oder eine Wiederherstellung fehlschlägt und dann einem Controller Failover, zusätzliche Wiederherstellungsauftrag auf der Seite **Projekte erscheint** . | Dieses Verhalten kann auftreten, wenn der Wiederherstellungsauftrag nur lokale Volumes oder eine Kombination aus lokal fixierte und tiered-Volumes fixiert wurde. Wenn der Wiederherstellungsauftrag mehrstufige Volumes enthält, wird dieses Verhalten nicht auftreten. Es ist kein Benutzereingriff erforderlich. | Ja | Nein |
| 20 |Lokale volumes | Wenn tiered Volumina (erstellt und geklonte mit Update 1.2 oder früher) auf ein lokales Volume konvertieren und das Gerät Speicherplatz ist Cloud ausfallen, können die Clone(s) beschädigt werden.| Dieses Problem tritt nur bei Volumes, die erstellt und mit geklonten vor dem Update 2 Software. Dies sollte selten Szenario.|
| 21 | Volume-Konvertierung | ACRs auf einem Volume zugeordnet, während ein Volume ausgeführt wird nicht aktualisiert (Abstufung auf lokal fixiert oder umgekehrt). Aktualisieren der ACRs könnte zu Datenverlusten führen. | Gegebenenfalls aktualisieren Sie ACRs vor der Konvertierung Volume und machen Sie weitere ACR Updates während die Konvertierung ausgeführt wird. |

## <a name="controller-and-firmware-updates-in-update-2"></a>Controller und Firmware-Updates in Update 2

Diese Version aktualisiert der Treiber und die Festplatten-Firmware auf Ihrem Gerät.
 
- Weitere Informationen zu LSI-Firmware aktualisieren Sie, finden Sie im Microsoft Knowledge Base-Artikel 3121900. 
- Weitere Informationen zu Festplatten-Firmware aktualisieren Sie, finden Sie im Microsoft Knowledge Base-Artikel 3121899.
 
## <a name="virtual-device-updates-in-update-2"></a>Virtuelles Geräteupdates Update 2

Dieses Update kann auf das virtuelle Gerät angewendet werden. Neue virtuelle Geräte müssen erstellt werden. 

## <a name="next-step"></a>Nächstes

Informationen auf dem Gerät StorSimple [Update 2 installieren](storsimple-install-update-2.md) .
