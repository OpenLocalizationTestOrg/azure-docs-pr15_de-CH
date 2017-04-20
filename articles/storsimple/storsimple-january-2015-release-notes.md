<properties 
   pageTitle="0,2 Versionshinweise StorSimple 8000 aktualisieren | Microsoft Azure"
   description="Beschreibt neue Features und Fixes, offene Probleme und Abhilfen verfügbar für Januar 2015 Microsoft Azure StorSimple Release (Update 0,2)."
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
   ms.date="05/16/2016"
   ms.author="v-sharos" />


# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>0,2 Versionshinweise StorSimple 8000-Serie Update - Januar 2015

## <a name="overview"></a>Übersicht

Die folgenden Versionsinformationen stellen öffnen kritisch für Januar 2015 Version von Microsoft Azure StorSimple. Sie enthalten auch eine Liste der StorSimple Software und Firmware-Updates in dieser Version enthalten. Dies ist die zweite Version nach StorSimple 8000-Serie Freigabeversion Juli 2014 allgemein verfügbar gemacht wurde.
  
Dieses Update ändert nicht die Softwareversion des Geräts von Oktober Update. Version 6.3.9600.17312 weiterhin. Das Bild im Bild virtuelles Gerät wurde in dieser Version geändert. Daher werden alle neuen virtuellen Geräte nach 1/20/2015 erstellt die Version als 6.3.9600.17361 angezeigt.  

Überprüfen Sie die folgende Informationen in den Versionshinweisen für das Update von Januar 2015.

> [AZURE.IMPORTANT]  
>
>- Dieses Update ist über Windows Update nicht verfügbar und kann nicht wie andere Updates installiert werden. Das Gerät wird dieses Update nicht erhalten, auch wenn Updates anwenden mit klassischen Azure-Portal. Dieses Update gilt nur für virtuelle Geräte nach dem 20. Januar 2015 erstellt. 
> 
>- Der Januar-Ausgabe von StorSimple enthält keine Updates auf das physische Gerät StorSimple. Sie können noch verfügbare Windows Updates auf Virtuelles Gerät, einschließlich der neuesten Sicherheitsupdates anwenden, aber Sie sehen keine Änderung der Version für das physische Gerät StorSimple.

## <a name="whats-new-in-the-january-release"></a>Was ist neu im Januar

Dieses Update enthält ein Update für die Volumes auf dem virtuellen Gerät offline gehen. (Siehe [Probleme in dieser Version behoben](#issues-fixed-in-the-january-release).)  

Das Update enthält keine neuen Features oder Funktionen.  

## <a name="issues-fixed-in-the-january-release"></a>Probleme in der Januar-Ausgabe

Die folgende Tabelle beschreibt das Problem, das in diesem Update behoben.

|Nein.|Funktion|Problem|Gilt für Gerät|Virtuelles Gerät gilt 
|---|-------|-----|--------------------------|-------------------------
|1|Datenträger offline gehen|Wenn hohe Cloud Wartezeiten Minuten bestehen gehen StorSimple virtuelles Gerät Volumes offline auf den Hosts. Dieses Update erhöht die Schwelle für Cloud Wartezeiten, so dass Situationen, in denen die Volumes auf Hosts offline gehen würde.|Nein|Ja  

## <a name="known-issues-in-the-january-release"></a>Bekannte Probleme im Januar

Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Version.
 
|Nein.|Funktion|Problem|Kommentare-Lösung|Gilt für Gerät|Virtuelles Gerät gilt 
|---|-------|-----|-------------------|--------------------------|-------------------------
|1| Funktion|  In einigen Fällen beim Ausführen einer Factory zurücksetzen StorSimple-Gerät möglicherweise und diese Meldung: **auf Factory zurückgesetzt wird (Phase 8).** Dies geschieht, wenn Sie STRG + C drücken, während das Cmdlet ausgeführt wird.| Drücken Sie STRG + C nicht nach dem Initiieren einer Factory zurücksetzen. Wenn Sie bereits in diesem Zustand sind, wenden Sie sich Microsoft Support für nächste Schritte.|Ja|Nein|
|2|Datenträger-quorum| In seltenen Fällen Wenn die Mehrzahl der Datenträger im EBOD Gehäuse 8600 Gerät getrennt werden wiederum kein Quorum Datenträger werden dann der Speicherpool offline. Offline bleibt, auch wenn die Datenträger wiederhergestellt werden.|Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft Support, für nächste Schritte.|Ja |Nein
|3|Cloud-Snapshot-Fehler|In seltenen Fällen möglicherweise Cloud Snapshot mit dem Fehler **maximale backup Limit erreicht**. Dies tritt auf, wenn 255 online Klone auf demselben Gerät aus dem gleichen Originaldatenträger überschreiten die gelöscht wurde.||Ja|Ja
|4|Falsche Controller-ID|Wenn ein Controller ausgetauscht kann Controller 0 als Domänencontroller 1 angezeigt. Während des controlleraustauschs beim Laden des Bildes aus der Peerknoten kann die Controller-ID zunächst Peer Controller ID angezeigt  In seltenen Fällen kann dieses Verhalten auch nach einem Neustart des Systems angezeigt.|Es ist keine Benutzeraktion erforderlich. Dies löst sich nach Abschluss der Controller ersetzt.|Ja|Nein 
|5| Device-monitoring-Diagrammen|In der StorSimple Manager-Dienst Überwachung Diagramme Geräte funktionieren nicht, wenn grundlegende oder NTLM-Authentifizierung in der Konfiguration des Proxyservers für das Gerät aktiviert ist.|Ändern der Web Proxy-Konfigurations für das Gerät StorSimple Manager-Dienst registriert werden, damit die Authentifizierung auf NONE festgelegt ist. Führen Sie dazu die Windows PowerShell für StorSimple Set-HcsWebProxy-Cmdlet.|Ja|Ja
|6| Speicherkonten|Das Speicherkonto löschen mit der Speicherdienst ist ein nicht unterstütztes Szenario. Dies führt zu einer Situation in dem Daten abgerufen werden können.|| Ja |  Ja
|7|Geräte-failover| Mehrere Failovers eines Containers Volume vom selben Quellgerät für verschiedene Geräte wird nicht unterstützt.| Failover von einem inaktiven Gerät auf mehreren Geräten machen volumecontainer für das erste Gerät Failover Besitzrechte Daten verlieren. Nach einem Failover dieser volumecontainer angezeigt oder Verhalten sich anders, wenn Sie im klassischen Azure-Portal anzeigen.|Ja|Nein
|8| Installation|Während StorSimple Adapter für SharePoint-Installation müssen Sie zu einem Gerät IP in Reihenfolge für die Installation erfolgreich abgeschlossen.||Ja|Nein
|9| Web-proxy|Verfügt der Web Proxy-Konfiguration angegebene Protokoll HTTPS, Ihr Gerät Service-Kommunikation beeinflusst werden und das Gerät offline gehen. Supportpakete werden auch dabei erhebliche Ressourcen auf Ihrem Gerät generiert werden.|Stellen Sie sicher, dass die Web-Proxy-URL angegebene Protokoll HTTP. Informationen Sie weitere zum [Konfigurieren der Webproxy für Ihr Gerät](storsimple-configure-web-proxy.md).|Ja |Nein
|10|Web-proxy|  Konfigurieren und aktivieren Webproxy auf einem registrierten Gerät müssen Sie aktiven Controller auf Ihrem Gerät zu starten.|| Ja |Nein
|11|Hohe Cloud Latenz und hoher i/o-Arbeitslast|Stößt Ihr StorSimple Gerät aus Cloud hohe Wartezeiten (Reihenfolge von Sekunden) und hoher i/o-Arbeitslast, Gerät Volumes in eine herabgesetzte und e / schlägt mit einer Fehlermeldung "Gerät nicht bereit".|Sie müssen manuell Gerätecontroller oder ein Gerät Failover zum Beheben des Problems durchführen.|Ja|Nein

## <a name="physical-device-updates-in-the-january-release"></a>Gerät Veröffentlichen im Januar

Dieses Update enthält keine Änderungen auf dem Gerät StorSimple.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Serial attached SCSI (SAS) Controller und Firmware-Updates im Januar freigeben

Diese Version enthält keine Updates Serial attached SCSI (SAS) Controller oder der Firmware. Treiber-Update wurde im Oktober 2014 Release. 

## <a name="virtual-device-updates-in-the-january-release"></a>Virtuelles Gerät Veröffentlichen im Januar

Diese Version enthält ein aktualisiertes Bild für virtuelles Gerät. Alle virtuellen Geräte erstellt nach dem 20. Januar 2015 werden die Softwareversion als 6.3.9600.17361 angezeigt.

 
