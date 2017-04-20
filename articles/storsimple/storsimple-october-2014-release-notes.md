<properties 
    pageTitle="0,1 Versionshinweise StorSimple 8000 aktualisieren | Microsoft Azure"
    description="Beschreibt neue Features und Fixes, offene Probleme und Abhilfen verfügbar für Oktober 2014 Microsoft Azure StorSimple Release (Update 0,1)."
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

# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000-Serie Update 0,1 Versionshinweise – Oktober 2014  

## <a name="overview"></a>Übersicht

Die folgenden Versionshinweise identifizieren öffnen kritisch StorSimple 8000-Serie Update veröffentlicht im Oktober 2014 0,1. Sie enthalten auch eine Liste der StorSimple Software und Firmware-Updates in dieser Version enthalten. Dies ist die erste Version StorSimple 8000-Serie Freigabeversion wurde im Juli 2014 zur Verfügung gestellt und auf Version 6.3.9600.17312 entspricht.  

Wir empfehlen, Sie suchen alle verfügbaren Updates anwenden, sobald Sie das Gerät installieren. Sie können auch aktivieren automatische Updates herunterladen und Installieren von Updates mit hoher Priorität von Microsoft, sobald sie freigegeben werden. Weitere Informationen finden Sie unter [StorSimple-Gerät](storsimple-update-device.md)aktualisieren.  

Überprüfen Sie die Informationen in den Release Notes vor der Bereitstellung von Updates in der StorSimple-Lösung.  

>[AZURE.IMPORTANT]
> 
-   Verwenden Sie StorSimple Manager-Dienst und nicht Windows PowerShell für StorSimple Oktober Updates zu installieren.  
-   Die Updates dauern in der Regel ungefähr 3 Stunden.  
-   Der Oktoberausgabe von StorSimple enthält keine Updates für das virtuelle Gerät StorSimple. Sie können noch verfügbare Windows-Updates, einschließlich der neuesten Sicherheitsupdates anwenden, aber Sie sehen keine Änderung der Version für das virtuelle Gerät.  

Stellen Sie sicher, dass die folgenden erforderlichen Komponenten vor der Aktualisierung des Geräts StorSimple erfüllt sind.  

- Stellen Sie sicher, dass beide Gerätecontroller ausgeführt werden, bevor Sie nach Updates suchen. Beide Controller nicht ausgeführt wird, schlägt die Überprüfung fehl. Um sicherzustellen, dass der Domänencontroller ordnungsgemäß, navigieren Sie auf der Seite **Wartung** zu **Hardwarestatus** . Falls Komponenten, **müssen Ihre Aufmerksamkeit**, Kontakt Microsoft Support vor dem fortfahren.  
- Sicherstellen Sie, dass die feste IP-Adressen für beide Controller 0 Controller 1 routingfähig und sind mit dem Internet verbinden können, wie diese verwendet werden, für die Updates für das Gerät warten. [Verbindung prüfen-Cmdlet](https://technet.microsoft.com/library/hh849808.aspx) können Sie ping eine bekannte Adresse außerhalb des Netzwerks wie outlook.com, um sicherzustellen, dass der Controller mit dem externen Netzwerk verbunden ist.  
- Sicherstellen Sie, dass die erforderlichen ausgehende Ports auf dem StorSimple-Gerät für die ausgehende Kommunikation. Weitere Informationen finden Sie unter [Netzwerk an das Gerät StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
- Deaktivieren Sie die Version der Gerätesoftware älter als 6.3.9600.17312 (Oktober 2014 Update) Daten 2 und Daten 3 Ports aktivierter vor der Aktualisierung. Wenn Sie Daten 2 und Daten 3 Anschlüsse aktiviert, wenn das Update anwenden lassen, kann Controllers Gerät Wiederherstellungsmodus zu führen. Beachten Sie, dass beim Deaktivieren der Netzwerkschnittstellen zugeordneten Volumes offline geschaltet und die e/a für die Dauer der Aktualisierung unterbrochen.  

## <a name="whats-new-in-the-october-release"></a>Neuigkeiten in der Oktober-Ausgabe

Dieses Update enthält folgenden zählen:

- Den Dienst StorSimple Manager Benutzeroberfläche können nun Ihre Gerätecontroller verwalten. Die Management-Aktionen neu starten, Herunterfahren, oder auf einem Domänencontroller aktivieren. Weitere Informationen finden Sie [StorSimple verwalten Gerätecontroller](storsimple-manage-device-controller.md).  
- Sie können die WAN-bandbreitenverteilung der Wochentag und Uhrzeit Kombinationen planen. Dadurch können Sie eine bessere Nutzung der WAN-Bandbreite Spitzenzeiten. Andere bandbreitenvorlagen dürfen für Datenträger-Container. Weitere Informationen zum Wechseln Sie [die StorSimple bandbreitenvorlagen verwalten](storsimple-manage-bandwidth-templates.md)  
- Sie können e-Mail-Benachrichtigung der Administratoren und andere vorhandene oder möglicherweise bevorstehende Probleme proaktiv benachrichtigt. Weitere Informationen finden Sie auf [Einstellungen konfigurieren](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Probleme in der Oktober-Ausgabe


Die folgende Tabelle enthält eine Zusammenfassung der in diesem Update behobene Probleme.  

| Nein. | Funktion | Problem | Gilt für Gerät | Virtuelles Gerät gilt |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Netzwerk-Schnittstellen | Netzwerkschnittstellen Daten 2 und Daten 3 wurden in der vorherigen Version der Software ersetzt. Dies wurde in diesem Update behoben. Deaktivieren Sie die Einstellung, und deaktivieren Sie dieser Netzwerkschnittstellen, bevor Sie das Update installieren. Nach der Installation des Updates müssen Sie diese Schnittstellen neu konfigurieren. | Ja | Nein |
| 2 | Support-Paket | In der vorherigen Version Wenn Windows PowerShell **Export HcsSupportPackage** -Cmdlet zum Abrufen der Protokolle Baseboard Management Controller (BMC) ausgeführt wurde der Fehler mit der folgenden Warnung: "der Vorgang wurde erfolgreich auf diesem Domänencontroller jedoch auf dem Peer-Domänencontroller aufgrund der folgenden Fehler fehlgeschlagen. Überprüfen Sie, ob der Peer fehlerfrei ist und ob der aktuelle Knoten dem Peer herstellen kann." Dieses Problem wurde nun behoben. | Ja | Nein |
| 3 | Geräte-failover | In der vorherigen Version wurde eine Chance Dateninkonsistenz Wenn ein **Backup entdecken** bei einem Failover Gerät ist fehlgeschlagen. Dieses Problem wurde nun behoben. | Ja | Nein |
| 4 | Geräte-failover | In der vorherigen Version nach einem Failover Gerät Backups sichtbar waren jedoch der zugehörigen Container nicht auf das Zielgerät. Dieses Problem wurde nun behoben. | Ja | Nein |
| 5 | Geräte-failover | In der Vorgängerversion gab es ein Fehler in der Enumeration der Cloud Backups während der Registrierung-Wiederherstellung zu Datenkonflikten führen konnte gäbe Cloud-Konnektivitätsprobleme. | Ja | Nein |
| 6 | Firmware-Aktualisierung | In der vorherigen Version die Geräte-Firmware-Aktualisierung fehlgeschlagen und angegeben, dass die Cmdlets nicht erkennen und daher Fehler bei die Aktualisierung einen Fehler angezeigt. Der Domänencontroller hat im Wiederherstellungsmodus. Dieses Problem wurde nun behoben. | Ja | Nein |
| 7 | Installation | Fehler durch das Gerät nicht ordnungsgemäß bei der Installation abgebildet wurden behoben. | Ja | Nein |
| 8 | Funktion | Sie können nun optional Firmware überprüft Funktion überspringen. Dies ist eine Änderung gegenüber der vorherigen Version. | Ja | Nein |
| 9 | Funktion | In der vorherigen Version beim Ausführen eines Cmdlets Factory zurücksetzen Firmware Version überprüft nur für einige Hardwarekomponenten wurden. Zusätzliche Firmware überprüft wurden nach dem ersten Neustart des Prozesses die zurücksetzen Fehler verursachen könnte. Dieses Update wird sichergestellt, dass alle Firmware Kontrolle durchgeführt wird, wenn die Factory Cmdlet zurückgesetzt wird und vor dem ersten Neustart. | Ja | Nein |
| 10 | Storage-Konto Schlüsselrotation | **Invoke-HcsmServiceDataEncryptionKeyChange** -Cmdlet zum Konto Speicherschlüssel jetzt drehen fordert den Benutzer auf den Verschlüsselungsschlüssel Daten eingeben. Dies ist eine Änderung gegenüber der vorherigen Version der Verschlüsselungsschlüssel Daten als ein Inline-Parameter übergeben wurde. | Ja | Nein |
| 11 | Failback innerhalb von 24 Stunden | Die Bereinigung des Quellgeräts während der Wiederherstellung nicht klar, dass Failback nicht geschehen. Dies wurde in dieser Version behoben. | Ja | Nein |

## <a name="known-issues-in-the-october-release"></a>Bekannte Probleme in der Oktober-Ausgabe

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Version.

| Nein. | Funktion | Problem | Kommentare-Lösung | Gilt für Gerät | Virtuelles Gerät gilt |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Funktion | In einigen Fällen beim Ausführen einer Factory zurücksetzen StorSimple-Gerät möglicherweise und diese Meldung angezeigt: **auf Factory zurückgesetzt wird (Phase 8)**. Dies geschieht, wenn Sie STRG + C drücken, während das Cmdlet ausgeführt wird. | Drücken Sie STRG + C nicht nach dem Initiieren einer Factory zurücksetzen. Wenn Sie bereits in diesem Zustand sind, wenden Sie sich Microsoft Support für nächste Schritte. | Ja | Nein |
| 2 | Funktion | Führen Sie keine Funktion ein StorSimple GA bis Oktober 2014 aktualisiert wird freigegeben. | Dieser Vorgang funktioniert nur, wenn ein Patch installiert ist. Wenden Sie sich an Microsoft Support zu diesem Patch erforderlich. | Ja | Nein | 
| 3 | Datenträger-quorum | In seltenen Fällen Wenn die Mehrzahl der Datenträger im EBOD Gehäuse 8600 Gerät getrennt werden wiederum kein Quorum Datenträger werden dann der Speicherpool offline. Offline bleibt, auch wenn die Datenträger wiederhergestellt werden. | Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft Support, für nächste Schritte. | Ja | Nein |
| 4 | Cloud-Snapshot-Fehler | In seltenen Fällen möglicherweise Cloud Snapshot mit dem Fehler **maximale backup Limit erreicht**. Dies tritt auf, wenn 255 online Klone auf demselben Gerät aus dem gleichen Originaldatenträger überschreiten die gelöscht wurde. | | Ja | Ja |
| 5 | Falsche Controller-ID | Wenn ein Controller ausgetauscht kann Controller 0 als Domänencontroller 1 angezeigt. Während des controlleraustauschs beim Laden des Bildes aus der Peerknoten kann die Controller-ID zunächst Peer Controller ID angezeigt In seltenen Fällen kann dieses Verhalten auch nach einem Neustart des Systems angezeigt. |Es ist keine Benutzeraktion erforderlich. Dies löst sich nach Abschluss der Controller ersetzt. | Ja | Nein |
| 6 | Device-monitoring-Diagrammen | In der StorSimple Manager-Dienst Überwachung Diagramme Geräte funktionieren nicht, wenn grundlegende oder NTLM-Authentifizierung in der Konfiguration des Proxyservers für das Gerät aktiviert ist. | Ändern der Web Proxy-Konfigurations für das Gerät StorSimple Manager-Dienst registriert werden, damit die Authentifizierung auf NONE festgelegt ist. Führen Sie dazu die Windows PowerShell für StorSimple Set-HcsWebProxy-Cmdlet. | Ja | Ja |
| 7 | Speicherkonten | Das Speicherkonto löschen mit der Speicherdienst ist ein nicht unterstütztes Szenario. Dies führt zu einer Situation in dem Daten abgerufen werden können. | | Ja | Ja |
| 8 | Geräte-failover | Mehrere Failovers eines Containers Volume vom selben Quellgerät für verschiedene Geräte wird nicht unterstützt. | Failover von einem inaktiven Gerät auf mehreren Geräten machen volumecontainer für das erste Gerät Failover Besitzrechte Daten verlieren. Nach einem Failover dieser volumecontainer angezeigt oder Verhalten sich anders, wenn Sie im klassischen Azure-Portal anzeigen. | Ja | Nein |
| 9 | Installation | Während StorSimple Adapter für SharePoint-Installation müssen Sie zu einem Gerät IP in Reihenfolge für die Installation erfolgreich abgeschlossen.    | | Ja | Nein |
| 10 | Web-proxy | Verfügt der Web Proxy-Konfiguration angegebene Protokoll HTTPS, Ihr Gerät Service-Kommunikation beeinflusst werden und das Gerät offline gehen. Supportpakete werden auch dabei erhebliche Ressourcen auf Ihrem Gerät generiert werden. | Stellen Sie sicher, dass die Web-Proxy-URL angegebene Protokoll HTTP. Weitere Informationen zum [Konfigurieren der Webproxy für Ihr Gerät](storsimple-configure-web-proxy.md). | Ja | Nein |
| 11 | Web-proxy | Konfigurieren und aktivieren Webproxy auf einem registrierten Gerät müssen Sie aktiven Controller auf Ihrem Gerät zu starten. | | Ja | Nein |
| 12 | Hohe Cloud Latenz und hoher i/o-Arbeitslast | Stößt Ihr StorSimple Gerät aus Cloud hohe Wartezeiten (Reihenfolge von Sekunden) und hoher i/o-Arbeitslast, Gerät Volumes in eine herabgesetzte und e / schlägt mit einer Fehlermeldung "Gerät nicht bereit". | Sie müssen manuell Gerätecontroller oder ein Gerät Failover zum Beheben des Problems durchführen. | Ja | Nein |

## <a name="physical-device-updates-in-the-october-release"></a>Gerät Veröffentlichen im Oktober

Wenn diese Updates zu einem physischen Gerät angewendet werden, wird die Software-Version in 6.3.9600.17312 geändert. Sofern nicht anders angegeben, werden diese Versionsinformationen gelten für alle Modelle des StorSimple-Geräts. Weitere Informationen zu diesen Updates finden Sie unter [Oktober 2014 physikalischen Geräts Software für Microsoft Azure StorSimple Gerät aktualisieren](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Serial attached SCSI (SAS) Controller und Firmware-Updates im Oktober freigeben

Diese Funktion aktualisiert die Treiber und die Firmware auf dem SAS-Controller des physischen Geräts. Weitere Informationen zu den SAS-Controller, finden Sie [Oktober 2014 LSI SAS-Controller in Microsoft Azure StorSimple aktualisieren](http://support.microsoft.com/kb/2987020).   

Diese Version auch eine kumulative Firmware-Aktualisierung, die Zuverlässigkeit mit dem Gerät Probleme. Weitere Informationen zur Firmware-Aktualisierung anzeigen Sie [Oktober 2014 Firmware-Aktualisierung für Microsoft Azure StorSimple Appliance](http://support.microsoft.com/kb/2987015)  

## <a name="virtual-device-updates-in-the-october-release"></a>Virtuelles Gerät Veröffentlichen im Oktober

Diese Version enthält keine Updates für das virtuelle Gerät. Installation dieses Updates wird die Softwareversion eines virtuellen Geräts nicht geändert.
 
