<properties 
   pageTitle="Versionsinformationen StorSimple 8000 freigeben | Microsoft Azure"
   description="Beschreibt neue Features, offene Probleme und Abhilfen verfügbar für Juli 2014 Microsoft Azure StorSimple Version."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>StorSimple 8000-Serie freigeben Versionsinformationen - Juli 2014 

## <a name="overview"></a>Übersicht

Die Versionsinformationen folgen stellen wichtige Tagesordnungspunkte für StorSimple-8000-Serie allgemein verfügbar (GA) Version von Microsoft Azure StorSimple Juli 2014. Diese Version entspricht Softwareversion 6.3.9600.17215.  

Sofern nicht anders angegeben, werden diese Versionsinformationen gelten für alle Modelle des StorSimple-Geräts. Die Versionsinformationen werden kontinuierlich aktualisiert. kritische Probleme erfordern eine Lösung gefunden werden, werden sie hinzugefügt. Bevor Sie Ihre Microsoft Azure StorSimple-Lösung bereitstellen, beachten Sie die folgenden Informationen.  

## <a name="known-issues-in-this-release"></a>Bekannte Probleme in dieser Version
Die folgende Tabelle enthält eine Zusammenfassung der bekannten Probleme in dieser Version.  
 
| Nein. | Funktion | Problem | Kommentare-Lösung | Gilt für Gerät | Virtuelles Gerät gilt |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Funktion | In einigen Fällen beim Ausführen einer Factory zurücksetzen StorSimple-Gerät möglicherweise und diese Meldung angezeigt: **auf Factory zurückgesetzt wird (Phase 8)**. Dies geschieht, wenn Sie STRG + C drücken, während das Cmdlet ausgeführt wird. | Drücken Sie STRG + C nicht nach dem Initiieren einer Factory zurücksetzen. Wenn Sie bereits in diesem Zustand sind, wenden Sie sich Microsoft Support für nächste Schritte. | Ja | Nein |
| 2 | Datenträger-quorum | In seltenen Fällen Wenn die Mehrzahl der Datenträger im EBOD Gehäuse 8600 Gerät getrennt werden wiederum kein Quorum Datenträger werden dann der Speicherpool offline. Offline bleibt, auch wenn die Datenträger wiederhergestellt werden. | Sie müssen das Gerät neu starten. Wenn das Problem weiterhin besteht, wenden Sie sich an den Microsoft Support, für nächste Schritte. | Ja | Nein |
| 3 | Cloud-Snapshot-Fehler | In seltenen Fällen möglicherweise Cloud Snapshot mit dem Fehler **maximale backup Limit erreicht**. Dies tritt auf, wenn 255 online Klone auf demselben Gerät aus dem gleichen Originaldatenträger überschreiten die gelöscht wurde. | | Ja | Ja |
| 4 | Falsche Controller-ID | Wenn ein Controller ausgetauscht kann Controller 0 als Domänencontroller 1 angezeigt. Während des controlleraustauschs beim Laden des Bildes aus der Peerknoten kann die Controller-ID zunächst Peer Controller ID angezeigt In seltenen Fällen kann dieses Verhalten auch nach einem Neustart des Systems angezeigt. | Es ist keine Benutzeraktion erforderlich. Dies löst sich nach Abschluss der Controller ersetzt. | Ja | Nein |
| 5 | Device-monitoring-Diagrammen | In der StorSimple Manager-Dienst Überwachung Diagramme Geräte funktionieren nicht, wenn grundlegende oder NTLM-Authentifizierung in der Konfiguration des Proxyservers für das Gerät aktiviert ist. | Ändern der Web Proxy-Konfigurations für das Gerät StorSimple Manager-Dienst registriert werden, damit die Authentifizierung auf NONE festgelegt ist. Führen Sie dazu die Windows PowerShell für StorSimple Set-HcsWebProxy-Cmdlet. | Ja | Ja |
| 6 | Speicherkonten | Das Speicherkonto löschen mit der Speicherdienst ist ein nicht unterstütztes Szenario. Dies führt zu einer Situation in dem Daten abgerufen werden können. | | Ja | Ja |
| 7 | Failback | Ein Failback innerhalb von 24 Stunden Disaster Recovery (DR) wird nicht unterstützt. | | Ja | Nein |
| 8 | Geräte-failover | Mehrere Failovers eines Containers Volume vom selben Quellgerät für verschiedene Geräte wird nicht unterstützt. Failover von einem inaktiven Gerät auf mehreren Geräten machen volumecontainer für das erste Gerät Failover Besitzrechte Daten verlieren. Nach einem Failover dieser volumecontainer angezeigt oder Verhalten sich anders, wenn Sie im klassischen Azure-Portal anzeigen. | | Ja | Nein |
| 9 | Installation | Während StorSimple Adapter für SharePoint-Installation müssen Sie bieten eine IP Gerät die Installation erfolgreich abgeschlossen. | | Ja | Nein |
| 10 | Netzwerk-Schnittstellen | Netzwerkschnittstellen Daten 2 und Daten 3 wurden in der Software ersetzt. | Kontaktieren Sie Microsoft Support möchten Sie diese Schnittstellen zu konfigurieren. | Ja | Nein |


 
