<properties 
   pageTitle="Versionshinweise StorSimple 8000-Serie Update 2.2 | Microsoft Azure"
   description="Beschreibt neue Features, Probleme und Abhilfen für StorSimple 8000-Serie Update 2.2."
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
   ms.date="07/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000-Serie Update 2.2-Versionsinformationen  

## <a name="overview"></a>Übersicht

Die folgenden Versionshinweise Beschreiben der neuen Features und öffnen kritisch für StorSimple 8000-Serie Update 2.2 identifizieren. Sie enthalten auch eine Liste der in dieser Version enthaltene StorSimple-Softwareupdates. 

2.2 aktualisieren kann jederzeit StorSimple Update 2.1 Version (GA) oder Update 0,1 durchgehend angewendet werden. Die Geräteversion Update 2.2 zugeordnet ist 6.3.9600.17708.

Überprüfen Sie die Informationen in den Release Notes vor die Aktualisierung in der StorSimple-Lösung bereitstellen.

>[AZURE.IMPORTANT]
> 
> - 2.2 Update hat nur Softwareupdates. Es dauert etwa 1,5 bis 2 Stunden installieren. 

> - Ausführen von Update 2.1 empfiehlt Microsoft Update 2.2 so schnell wie möglich anzuwenden.

> - Neue Versionen nicht auftreten Updates sofort, da wir einen phasenbasierten Rollout Updates ausführen. Warten Sie einige Tage, und dann nach Updates als diese wird bald verfügbar.


## <a name="whats-new-in-update-22"></a>Neues Update 2.2

Folgende wichtige Verbesserung in Update 2.2 wurden.

 
- **Automatische Optimierung der Wiedergewinnung** – beim Löschen von Daten auf Thin Provisioning müssen Blöcke belegter Speicher freigegeben werden. Diese Version verbessert Speicherplatz der Rückgewinnung von der Cloud, was immer schneller im Vergleich zu früheren Versionen verfügbaren Speicherplatz.


- **Verbesserte Performance Snapshot** -Update 2.2 verbessert die Verarbeitungszeit für eine Wolke snapshot in bestimmten Szenarien, wo große Mengen verwendet werden und ist minimal, keine Daten Abwanderung. Ein Szenario, das von dieser Erweiterung profitieren wäre Datenträger archivieren.


- **Absichern von Supportpaket sammeln** – verbesserte Support-Paket gesammelt und in dieser Version hochgeladen wurden. 


- **Verbesserte Zuverlässigkeit aktualisieren** – diese Version hat ein Update Zuverlässigkeit führen Fehlerkorrekturen.

  
 

## <a name="issues-fixed-in-update-22"></a>2.2 Update behobene Probleme

Die folgenden Tabellen enthalten eine Zusammenfassung der Updates 2.2 und 2.1 behobene Probleme.    

| Nein | Funktion                                    | Problem                                                                                                                                                                                                                                                                                        | Gilt für Gerät | Virtuelles Gerät gilt |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Server-performance                      | In der früheren Version beobachtet hostseitige Leistungsprobleme während der Erstellung des lokalen Volumes und während der Konvertierung eines gestuften Volumes auf lokalen Volumes. Diese Probleme wurden in dieser Version verbessert die Performance des Host bei der Erstellung und Konvertierung Volume somit behoben.                                                                        | Ja                        | Nein                        |
| 2  | Lokale volumes                     | In seltenen Fällen würde das System abstürzen, wenn ein lokales Volume erstellen. Dieser Fehler wurde in dieser Version behoben.                                                                                                                                                               | Ja                        | Nein                        |
| 3  | Tiering                                    | Wenn die Metadaten für die StorSimple Cloud (8010 und 8020) in die Cloud gestuft sind Gelegentliche Abstürze. Dieses Problem wurde in dieser Version behoben.                                                                                                                              | Nein                         | Ja                       |
| 4  | Snapshot-Erstellung                          | Es gab Probleme mit der Erstellung von inkrementellen Snapshots in Szenarien mit und minimale Änderung keine Daten. Diese Probleme wurden in dieser Version behoben.                                                                                                                 | Ja                        | Ja                       |
| 5  | Openstack Authentifizierung                   | Bei Verwendung von Openstack als Cloud-Dienstanbieter führen Benutzer in eine unregelmäßige Fehler bei der Authentifizierung JSON-Parser, einem Absturz führte. Dieser Fehler wird in dieser Version.                                                                                                                              | Ja                        | Nein                        |
| 6  | Host-Seite kopieren                             | In früheren Versionen der Software war ein seltener Fehler auf ODX beim Kopieren von Daten von einem Volume auf ein anderes Volume angezeigt. Dadurch würde eine Controller-Failover und das System konnte möglicherweise in Wiederherstellungsmodus. Dieser Fehler wird in dieser Version. | Ja                        | Nein       |
| 7  | Windows-Verwaltungsinstrumentation (WMI) | In früheren Versionen der Software wurden mehrere Instanzen des Web Proxy-Fehler mit Ausnahme "<ManagementException> Anbieter Ladefehler". Dieses Problem wurde auf einen Speicherverlust WMI und behoben.                                                               | Ja                        | Nein                        |
| 8  | Update                                     | In bestimmten seltenen Fällen in früheren Versionen der Software erhalten der Benutzer "CisPowershellHcsscripterror" beim Scannen oder Updates installieren. Dieses Problem wurde in dieser Version behoben.                                                                                        | Ja                        | Ja                       |
| 9  | Support-Paket                            | In dieser Version wurden verbessert das Supportpaket gesammelt und hochgeladen.                                                                                                                                                                                                      | Ja                        | Ja                                    |


## <a name="known-issues-in-update-22"></a>Bekannte Probleme im Update 2.2

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
| 20 |Lokale volumes | Wenn tiered Volumina (erstellt und geklonte mit Update 1.2 oder früher) auf ein lokales Volume konvertieren und das Gerät Speicherplatz ist Cloud ausfallen, können die Clone(s) beschädigt werden.| Dieses Problem tritt nur bei Volumes, die erstellt und mit geklonten vor dem Update 2.1 Software. Dies sollte selten Szenario.|
| 21 | Volume-Konvertierung | ACRs auf einem Volume zugeordnet, während ein Volume ausgeführt wird nicht aktualisiert (Abstufung auf lokal fixiert oder umgekehrt). Aktualisieren der ACRs könnte zu Datenverlusten führen. | Gegebenenfalls aktualisieren Sie ACRs vor der Konvertierung Volume und machen Sie weitere ACR Updates während die Konvertierung ausgeführt wird. |

## <a name="controller-and-firmware-updates-in-update-22"></a>Controller und Firmware-Updates 2.2 aktualisieren

Diese Version wurde nur Software-Updates. Wenn Sie von einer Version vor Update 2 aktualisieren, Sie müssen Treiber Storport, Spaceport, und (in einigen Fällen) Festplatten-Firmware-Updates auf Ihrem Gerät.
 
Weitere Informationen zur Installation der Treiber Storport, Spaceport und Laufwerk Firmware-Updates finden Sie auf Ihrem Gerät StorSimple [Update 2.2 installieren](storsimple-install-update-21.md) .

 
## <a name="virtual-device-updates-in-update-22"></a>Virtuelles Geräteupdates 2.2 aktualisieren

Dieses Update kann auf das virtuelle Gerät angewendet werden. Neue virtuelle Geräte müssen erstellt werden. 

## <a name="next-step"></a>Nächstes

Informationen auf dem Gerät StorSimple [Update 2.2 installieren](storsimple-install-update-21.md) .
