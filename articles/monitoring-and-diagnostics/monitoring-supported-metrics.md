<properties
    pageTitle="Azure Monitor Metriken - unterstützten Metriken pro Ressourcentyp | Microsoft Azure"
    description="Liste der Metriken für jeden Ressourcentyp mit Azure."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="supported-metrics-with-azure-monitor"></a>Unterstützte Metriken mit Azure

Azure Monitor bietet verschiedene Metriken, einschließlich im Portal Diagramm, durch die REST-API zugreifen oder diese Abfragen interagieren mit PowerShell oder CLI. Es folgt eine vollständige Liste aller Metriken mit Azure Monitor Metrisch Pipeline.

> [AZURE.NOTE] Andere möglicherweise in das Portal oder über legacy-APIs verfügbar. Diese Liste enthält nur über die public Preview konsolidierten Azure Monitor Metrisch Pipeline public Preview-Metriken.

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|CoreCount|Anzahl der Kerne|Anzahl|Insgesamt|Gesamtzahl der Kerne im Batch-Konto|
|TotalNodeCount|Node-Count|Anzahl|Insgesamt|Gesamtzahl der Knoten im Batch-Konto|
|CreatingNodeCount|Erstellen von Knoten|Anzahl|Insgesamt|Anzahl der erstellten Knoten|
|StartingNodeCount|Starten Sie Knoten|Anzahl|Insgesamt|Anzahl der Knoten ab|
|WaitingForStartTaskNodeCount|Wartezeit für Start Task Node-Count|Anzahl|Insgesamt|Anzahl der Knoten wartet die Aufgabe abgeschlossen|
|StartTaskFailedNodeCount|Start Taskfehler Knoten|Anzahl|Insgesamt|Anzahl der Knoten, in dem die Aufgabe fehlgeschlagen|
|IdleNodeCount|Knoten im Leerlauf|Anzahl|Insgesamt|Anzahl der Knoten im Leerlauf|
|OfflineNodeCount|Offline Node-Count|Anzahl|Insgesamt|Anzahl der Knoten offline|
|RebootingNodeCount|Neustarten von Knoten|Anzahl|Insgesamt|Anzahl der Knoten neu gestartet|
|ReimagingNodeCount|Erstellen eines neuen Image Knoten|Anzahl|Insgesamt|Anzahl reimaging Knoten|
|RunningNodeCount|Knoten ausführen|Anzahl|Insgesamt|Anzahl der ausgeführten Knoten|
|LeavingPoolNodeCount|Pool-Node-Count verlassen|Anzahl|Insgesamt|Anzahl der Knoten aus dem Pool|
|UnusableNodeCount|Unbrauchbar Node-Count|Anzahl|Insgesamt|Anzahl nicht verwendbar|
|TaskStartEvent|Aufgabe starten-Ereignisse|Anzahl|Insgesamt|Gesamtzahl der angefangene Vorgänge|
|TaskCompleteEvent|Vollständige Aufgabenereignisse|Anzahl|Insgesamt|Gesamtanzahl der Vorgänge, die abgeschlossen wurden|
|TaskFailEvent|Fehler Aufgabenereignisse|Anzahl|Insgesamt|Gesamtanzahl der Vorgänge, die in einem fehlerhaften Zustand abgeschlossen haben|
|PoolCreateEvent|Pool Ereignisse erstellen|Anzahl|Insgesamt|Gesamtanzahl der erstellten pools|
|PoolResizeStartEvent|Pool-Größe starten-Ereignisse|Anzahl|Insgesamt|Gesamtzahl der Pool-Größe, die gestartet wurden|
|PoolResizeCompleteEvent|Pool-Größe Complete-Ereignisse|Anzahl|Insgesamt|Gesamtzahl der Pool-Größe, die abgeschlossen wurden|
|PoolDeleteStartEvent|Pool löschen starten-Ereignisse|Anzahl|Insgesamt|Gesamtzahl der Pool gelöscht, die gestartet wurden|
|PoolDeleteCompleteEvent|Pool löschen Complete-Ereignisse|Anzahl|Insgesamt|Gesamtzahl der Pool gelöscht, die abgeschlossen wurden|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|connectedclients|Verbundene Clients|Anzahl|Maximale||
|totalcommandsprocessed|Gesamtanzahl der Vorgänge|Anzahl|Insgesamt||
|CacheHits|Cache-Treffer|Anzahl|Insgesamt||
|cachemisses|Cachefehler|Anzahl|Insgesamt||
|getcommands|Ruft ab|Anzahl|Insgesamt||
|setcommands|Legt|Anzahl|Insgesamt||
|evictedkeys|Entfernten Schlüssel|Anzahl|Insgesamt||
|totalkeys|Schlüssel insgesamt|Anzahl|Maximale||
|expiredkeys|Abgelaufener Schlüssel|Anzahl|Insgesamt||
|usedmemory|Verwendeter Speicher|Bytes|Maximale||
|usedmemoryRss|Verwendeter Speicher RSS|Bytes|Maximale||
|serverLoad|Serverlast|Prozent|Maximale||
|cacheWrite|Cache-Schreiben|BytesPerSecond|Maximale||
|cacheRead|Cache-lesen|BytesPerSecond|Maximale||
|percentProcessorTime|CPU|Prozent|Maximale||
|connectedclients0|Verbundenen Clients (Splitter 0)|Anzahl|Maximale||
|totalcommandsprocessed0|Gesamtanzahl der Vorgänge (Splitter 0)|Anzahl|Insgesamt||
|cachehits0|Cache-Treffer (Splitter 0)|Anzahl|Insgesamt||
|cachemisses0|Cachefehler (Splitter 0)|Anzahl|Insgesamt||
|getcommands0|Ruft ab (Splitter 0)|Anzahl|Insgesamt||
|setcommands0|Legt (Splitter 0)|Anzahl|Insgesamt||
|evictedkeys0|Entfernten Schlüssel (Splitter 0)|Anzahl|Insgesamt||
|totalkeys0|Schlüssel (Knoten 0) insgesamt|Anzahl|Maximale||
|expiredkeys0|Abgelaufener Schlüssel (Splitter 0)|Anzahl|Insgesamt||
|usedmemory0|Verwendeter Speicher (Splitter 0)|Bytes|Maximale||
|usedmemoryRss0|Verwendeter Speicher RSS (Splitter 0)|Bytes|Maximale||
|serverLoad0|Serverlast (Splitter 0)|Prozent|Maximale||
|cacheWrite0|Cache Schreiben (Splitter 0)|BytesPerSecond|Maximale||
|cacheRead0|Cache-lesen (Splitter 0)|BytesPerSecond|Maximale||
|percentProcessorTime0|CPU (Splitter 0)|Prozent|Maximale||
|connectedclients1|Verbundenen Clients (Splitter 1)|Anzahl|Maximale||
|totalcommandsprocessed1|Gesamtanzahl der Vorgänge (Splitter 1)|Anzahl|Insgesamt||
|cachehits1|Cache-Treffer (Splitter 1)|Anzahl|Insgesamt||
|cachemisses1|Cachefehler (Splitter 1)|Anzahl|Insgesamt||
|getcommands1|Ruft ab (Splitter 1)|Anzahl|Insgesamt||
|setcommands1|Legt (Splitter 1)|Anzahl|Insgesamt||
|evictedkeys1|Entfernten Schlüssel (Splitter 1)|Anzahl|Insgesamt||
|totalkeys1|Schlüssel (Knoten 1) insgesamt|Anzahl|Maximale||
|expiredkeys1|Abgelaufener Schlüssel (Splitter 1)|Anzahl|Insgesamt||
|usedmemory1|Verwendeter Speicher (Splitter 1)|Bytes|Maximale||
|usedmemoryRss1|Verwendeter Speicher RSS (Splitter 1)|Bytes|Maximale||
|serverLoad1|Serverlast (Splitter 1)|Prozent|Maximale||
|cacheWrite1|Cache Schreiben (Splitter 1)|BytesPerSecond|Maximale||
|cacheRead1|Cache-lesen (Splitter 1)|BytesPerSecond|Maximale||
|percentProcessorTime1|CPU (Splitter 1)|Prozent|Maximale||
|connectedclients2|Verbundenen Clients (Splitter 2)|Anzahl|Maximale||
|totalcommandsprocessed2|Gesamtanzahl der Vorgänge (Splitter 2)|Anzahl|Insgesamt||
|cachehits2|Cache-Treffer (Splitter 2)|Anzahl|Insgesamt||
|cachemisses2|Cachefehler (Splitter 2)|Anzahl|Insgesamt||
|getcommands2|Ruft ab (Splitter 2)|Anzahl|Insgesamt||
|setcommands2|Legt (Splitter 2)|Anzahl|Insgesamt||
|evictedkeys2|Entfernten Schlüssel (Splitter 2)|Anzahl|Insgesamt||
|totalkeys2|Schlüssel (Knoten 2) insgesamt|Anzahl|Maximale||
|expiredkeys2|Abgelaufener Schlüssel (Splitter 2)|Anzahl|Insgesamt||
|usedmemory2|Verwendeter Speicher (Splitter 2)|Bytes|Maximale||
|usedmemoryRss2|Verwendeter Speicher RSS (Splitter 2)|Bytes|Maximale||
|serverLoad2|Serverlast (Splitter 2)|Prozent|Maximale||
|cacheWrite2|Cache Schreiben (Splitter 2)|BytesPerSecond|Maximale||
|cacheRead2|Cache-lesen (Splitter 2)|BytesPerSecond|Maximale||
|percentProcessorTime2|CPU (Splitter 2)|Prozent|Maximale||
|connectedclients3|Verbundenen Clients (Splitter 3)|Anzahl|Maximale||
|totalcommandsprocessed3|Gesamtanzahl der Vorgänge (Splitter 3)|Anzahl|Insgesamt||
|cachehits3|Cache-Treffer (Splitter 3)|Anzahl|Insgesamt||
|cachemisses3|Cachefehler (Splitter 3)|Anzahl|Insgesamt||
|getcommands3|Ruft ab (Splitter 3)|Anzahl|Insgesamt||
|setcommands3|Legt (Splitter 3)|Anzahl|Insgesamt||
|evictedkeys3|Entfernten Schlüssel (Splitter 3)|Anzahl|Insgesamt||
|totalkeys3|Schlüssel (Knoten 3) insgesamt|Anzahl|Maximale||
|expiredkeys3|Abgelaufener Schlüssel (Splitter 3)|Anzahl|Insgesamt||
|usedmemory3|Verwendeter Speicher (Splitter 3)|Bytes|Maximale||
|usedmemoryRss3|Verwendeter Speicher RSS (Splitter 3)|Bytes|Maximale||
|serverLoad3|Serverlast (Splitter 3)|Prozent|Maximale||
|cacheWrite3|Cache Schreiben (Splitter 3)|BytesPerSecond|Maximale||
|cacheRead3|Cache-lesen (Splitter 3)|BytesPerSecond|Maximale||
|percentProcessorTime3|CPU (Splitter 3)|Prozent|Maximale||
|connectedclients4|Verbundenen Clients (Splitter 4)|Anzahl|Maximale||
|totalcommandsprocessed4|Gesamtanzahl der Vorgänge (Splitter 4)|Anzahl|Insgesamt||
|cachehits4|Cache-Treffer (Splitter 4)|Anzahl|Insgesamt||
|cachemisses4|Cachefehler (Splitter 4)|Anzahl|Insgesamt||
|getcommands4|Ruft ab (Splitter 4)|Anzahl|Insgesamt||
|setcommands4|Legt (Splitter 4)|Anzahl|Insgesamt||
|evictedkeys4|Entfernten Schlüssel (Splitter 4)|Anzahl|Insgesamt||
|totalkeys4|Schlüssel insgesamt (4 Knoten)|Anzahl|Maximale||
|expiredkeys4|Abgelaufener Schlüssel (Splitter 4)|Anzahl|Insgesamt||
|usedmemory4|Verwendeter Speicher (Splitter 4)|Bytes|Maximale||
|usedmemoryRss4|Verwendeter Speicher RSS (Splitter 4)|Bytes|Maximale||
|serverLoad4|Serverlast (Splitter 4)|Prozent|Maximale||
|cacheWrite4|Cache Schreiben (Splitter 4)|BytesPerSecond|Maximale||
|cacheRead4|Cache-lesen (Splitter 4)|BytesPerSecond|Maximale||
|percentProcessorTime4|CPU (Splitter 4)|Prozent|Maximale||
|connectedclients5|Verbundenen Clients (Splitter 5)|Anzahl|Maximale||
|totalcommandsprocessed5|Gesamtanzahl der Vorgänge (Splitter 5)|Anzahl|Insgesamt||
|cachehits5|Cache-Treffer (Splitter 5)|Anzahl|Insgesamt||
|cachemisses5|Cachefehler (Splitter 5)|Anzahl|Insgesamt||
|getcommands5|Ruft ab (Splitter 5)|Anzahl|Insgesamt||
|setcommands5|Legt (Splitter 5)|Anzahl|Insgesamt||
|evictedkeys5|Entfernten Schlüssel (Splitter 5)|Anzahl|Insgesamt||
|totalkeys5|Schlüssel insgesamt (5 Knoten)|Anzahl|Maximale||
|expiredkeys5|Abgelaufener Schlüssel (Splitter 5)|Anzahl|Insgesamt||
|usedmemory5|Verwendeter Speicher (Splitter 5)|Bytes|Maximale||
|usedmemoryRss5|Verwendeter Speicher RSS (Splitter 5)|Bytes|Maximale||
|serverLoad5|Serverlast (Splitter 5)|Prozent|Maximale||
|cacheWrite5|Cache Schreiben (Splitter 5)|BytesPerSecond|Maximale||
|cacheRead5|Cache-lesen (Splitter 5)|BytesPerSecond|Maximale||
|percentProcessorTime5|CPU (Splitter 5)|Prozent|Maximale||
|connectedclients6|Verbundenen Clients (Splitter 6)|Anzahl|Maximale||
|totalcommandsprocessed6|Gesamtanzahl der Vorgänge (Splitter 6)|Anzahl|Insgesamt||
|cachehits6|Cache-Treffer (Splitter 6)|Anzahl|Insgesamt||
|cachemisses6|Cachefehler (Splitter 6)|Anzahl|Insgesamt||
|getcommands6|Ruft ab (Splitter 6)|Anzahl|Insgesamt||
|setcommands6|Legt (Splitter 6)|Anzahl|Insgesamt||
|evictedkeys6|Entfernten Schlüssel (Splitter 6)|Anzahl|Insgesamt||
|totalkeys6|Schlüssel insgesamt (6 Knoten)|Anzahl|Maximale||
|expiredkeys6|Abgelaufener Schlüssel (Splitter 6)|Anzahl|Insgesamt||
|usedmemory6|Verwendeter Speicher (Splitter 6)|Bytes|Maximale||
|usedmemoryRss6|Verwendeter Speicher RSS (Splitter 6)|Bytes|Maximale||
|serverLoad6|Serverlast (Splitter 6)|Prozent|Maximale||
|cacheWrite6|Cache Schreiben (Splitter 6)|BytesPerSecond|Maximale||
|cacheRead6|Cache-lesen (Splitter 6)|BytesPerSecond|Maximale||
|percentProcessorTime6|CPU (Splitter 6)|Prozent|Maximale||
|connectedclients7|Verbundenen Clients (Splitter 7)|Anzahl|Maximale||
|totalcommandsprocessed7|Gesamtanzahl der Vorgänge (Splitter 7)|Anzahl|Insgesamt||
|cachehits7|Cache-Treffer (Splitter 7)|Anzahl|Insgesamt||
|cachemisses7|Cachefehler (Splitter 7)|Anzahl|Insgesamt||
|getcommands7|Ruft ab (Splitter 7)|Anzahl|Insgesamt||
|setcommands7|Legt (Splitter 7)|Anzahl|Insgesamt||
|evictedkeys7|Entfernten Schlüssel (Splitter 7)|Anzahl|Insgesamt||
|totalkeys7|Schlüssel (Knoten 7) insgesamt|Anzahl|Maximale||
|expiredkeys7|Abgelaufener Schlüssel (Splitter 7)|Anzahl|Insgesamt||
|usedmemory7|Verwendeter Speicher (Splitter 7)|Bytes|Maximale||
|usedmemoryRss7|Verwendeter Speicher RSS (Splitter 7)|Bytes|Maximale||
|serverLoad7|Serverlast (Splitter 7)|Prozent|Maximale||
|cacheWrite7|Cache Schreiben (Splitter 7)|BytesPerSecond|Maximale||
|cacheRead7|Cache-lesen (Splitter 7)|BytesPerSecond|Maximale||
|percentProcessorTime7|CPU (Splitter 7)|Prozent|Maximale||
|connectedclients8|Verbundenen Clients (Splitter 8)|Anzahl|Maximale||
|totalcommandsprocessed8|Gesamtanzahl der Vorgänge (Splitter 8)|Anzahl|Insgesamt||
|cachehits8|Cache-Treffer (Splitter 8)|Anzahl|Insgesamt||
|cachemisses8|Cachefehler (Splitter 8)|Anzahl|Insgesamt||
|getcommands8|Ruft ab (Splitter 8)|Anzahl|Insgesamt||
|setcommands8|Legt (Splitter 8)|Anzahl|Insgesamt||
|evictedkeys8|Entfernten Schlüssel (Splitter 8)|Anzahl|Insgesamt||
|totalkeys8|Schlüssel (8 Knoten) insgesamt|Anzahl|Maximale||
|expiredkeys8|Abgelaufener Schlüssel (Splitter 8)|Anzahl|Insgesamt||
|usedmemory8|Verwendeter Speicher (Splitter 8)|Bytes|Maximale||
|usedmemoryRss8|Verwendeter Speicher RSS (Splitter 8)|Bytes|Maximale||
|serverLoad8|Serverlast (Splitter 8)|Prozent|Maximale||
|cacheWrite8|Cache Schreiben (Splitter 8)|BytesPerSecond|Maximale||
|cacheRead8|Cache-lesen (Splitter 8)|BytesPerSecond|Maximale||
|percentProcessorTime8|CPU (Splitter 8)|Prozent|Maximale||
|connectedclients9|Verbundenen Clients (Splitter 9)|Anzahl|Maximale||
|totalcommandsprocessed9|Gesamtanzahl der Vorgänge (Splitter 9)|Anzahl|Insgesamt||
|cachehits9|Cache-Treffer (Splitter 9)|Anzahl|Insgesamt||
|cachemisses9|Cachefehler (Splitter 9)|Anzahl|Insgesamt||
|getcommands9|Ruft ab (Splitter 9)|Anzahl|Insgesamt||
|setcommands9|Legt (Splitter 9)|Anzahl|Insgesamt||
|evictedkeys9|Entfernten Schlüssel (Splitter 9)|Anzahl|Insgesamt||
|totalkeys9|Schlüssel (Knoten 9) insgesamt|Anzahl|Maximale||
|expiredkeys9|Abgelaufener Schlüssel (Splitter 9)|Anzahl|Insgesamt||
|usedmemory9|Verwendeter Speicher (Splitter 9)|Bytes|Maximale||
|usedmemoryRss9|Verwendeter Speicher RSS (Splitter 9)|Bytes|Maximale||
|serverLoad9|Serverlast (Splitter 9)|Prozent|Maximale||
|cacheWrite9|Cache Schreiben (Splitter 9)|BytesPerSecond|Maximale||
|cacheRead9|Cache-lesen (Splitter 9)|BytesPerSecond|Maximale||
|percentProcessorTime9|CPU (Splitter 9)|Prozent|Maximale||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|NumberOfCalls|API-Aufrufe insgesamt|Anzahl|Insgesamt|Gesamtzahl der API-Aufrufe.|
|NumberOfFailedCalls|Gesamtanzahl der fehlgeschlagenen API-Aufrufe|Anzahl|Insgesamt|Gesamtanzahl der fehlgeschlagenen API-Aufrufe.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|Prozent CPU|Prozent CPU|Prozent|Durchschnitt|Der Prozentsatz der reservierten Compute-Einheiten, die derzeit von der virtuellen Computer verwendet werden|
|Im Netzwerk|Im Netzwerk|Bytes|Insgesamt|Die Anzahl der vom virtuellen Computer (eingehender Datenverkehr) auf allen Netzwerkschnittstellen empfangenen bytes|
|Netzwerk|Netzwerk|Bytes|Insgesamt|Die Anzahl der Bytes in allen Netzwerkschnittstellen von virtuellen (ausgehender Datenverkehr)|
|Lesen der Bytes|Lesen der Bytes|Bytes|Insgesamt|Lesen vom Datenträger im Überwachungszeitraum Bytes insgesamt|
|Schreiben der Bytes|Schreiben der Bytes|Bytes|Insgesamt|Gesamtanzahl der Bytes im Überwachungszeitraum Datenträger geschrieben|
|Gelesen/Sek.|Gelesen/Sek.|CountPerSecond|Durchschnitt|Lesevorgänge IOPS|
|Schreiben Operationen/Sek.|Schreiben Operationen/Sek.|CountPerSecond|Durchschnitt|Schreibvorgänge IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|Prozent CPU|Prozent CPU|Prozent|Durchschnitt|Der Prozentsatz der reservierten Compute-Einheiten, die derzeit von der virtuellen Computer verwendet werden|
|Im Netzwerk|Im Netzwerk|Bytes|Insgesamt|Die Anzahl der vom virtuellen Computer (eingehender Datenverkehr) auf allen Netzwerkschnittstellen empfangenen bytes|
|Netzwerk|Netzwerk|Bytes|Insgesamt|Die Anzahl der Bytes in allen Netzwerkschnittstellen von virtuellen (ausgehender Datenverkehr)|
|Lesen der Bytes|Lesen der Bytes|Bytes|Insgesamt|Lesen vom Datenträger im Überwachungszeitraum Bytes insgesamt|
|Schreiben der Bytes|Schreiben der Bytes|Bytes|Insgesamt|Gesamtanzahl der Bytes im Überwachungszeitraum Datenträger geschrieben|
|Gelesen/Sek.|Gelesen/Sek.|CountPerSecond|Durchschnitt|Lesevorgänge IOPS|
|Schreiben Operationen/Sek.|Schreiben Operationen/Sek.|CountPerSecond|Durchschnitt|Schreibvorgänge IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|Prozent CPU|Prozent CPU|Prozent|Durchschnitt|Der Prozentsatz der reservierten Compute-Einheiten, die derzeit von der virtuellen Computer verwendet werden|
|Im Netzwerk|Im Netzwerk|Bytes|Insgesamt|Die Anzahl der vom virtuellen Computer (eingehender Datenverkehr) auf allen Netzwerkschnittstellen empfangenen bytes|
|Netzwerk|Netzwerk|Bytes|Insgesamt|Die Anzahl der Bytes in allen Netzwerkschnittstellen von virtuellen (ausgehender Datenverkehr)|
|Lesen der Bytes|Lesen der Bytes|Bytes|Insgesamt|Lesen vom Datenträger im Überwachungszeitraum Bytes insgesamt|
|Schreiben der Bytes|Schreiben der Bytes|Bytes|Insgesamt|Gesamtanzahl der Bytes im Überwachungszeitraum Datenträger geschrieben|
|Gelesen/Sek.|Gelesen/Sek.|CountPerSecond|Durchschnitt|Lesevorgänge IOPS|
|Schreiben Operationen/Sek.|Schreiben Operationen/Sek.|CountPerSecond|Durchschnitt|Schreibvorgänge IOPS|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Telemetrie Nachricht senden versucht|Anzahl|Insgesamt|Anzahl der Geräte in die Cloud Telemetrie Nachrichten versucht IoT Hub gesendet werden|
|d2c.telemetry.Ingress.Success|Telemetrie Nachrichten|Anzahl|Insgesamt|Anzahl der Geräte Cloud Telemetrie Nachrichten erfolgreich IoT hub|
|c2d.Commands.Egress.Complete.Success|Befehle abgeschlossen|Anzahl|Insgesamt|Anzahl der Cloud gerätebefehle erfolgreich vom Gerät|
|c2d.Commands.Egress.Abandon.Success|Befehle abgebrochen|Anzahl|Insgesamt|Anzahl der Cloud gerätebefehle vom Gerät abgebrochen|
|c2d.Commands.Egress.Reject.Success|Abgelehnte Befehle|Anzahl|Insgesamt|Anzahl der Cloud gerätebefehle vom Gerät abgelehnt|
|devices.totalDevices|Geräte insgesamt|Anzahl|Insgesamt|Anzahl der Geräte registriert IoT hub|
|devices.connectedDevices.allProtocol|Angeschlossene Geräte|Anzahl|Insgesamt|Anzahl an IoT Hub angeschlossene Geräte|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|INREQS|Anfragen|Anzahl|Insgesamt|Ereignis-Hub eingehende Nachrichtendurchsatz für einen namespace|
|SUCCREQ|Erfolgreiche Anfragen|Anzahl|Insgesamt|Erfolgreiche Anfragen insgesamt für einen namespace|
|FAILREQ|Fehlgeschlagene Anfragen|Anzahl|Insgesamt|Fehlgeschlagene Anfragen insgesamt für einen namespace|
|SVRBSY|Server ausgelastet-Fehler|Anzahl|Insgesamt|Beschäftigt Serverfehler gesamt für einen namespace|
|SITZ|Interner Serverfehler|Anzahl|Insgesamt|Interner Serverfehler für einen Namespace gesamt|
|MISCERR|Andere Fehler|Anzahl|Insgesamt|Fehlgeschlagene Anfragen insgesamt für einen namespace|
|INMSGS|Eingehende Nachrichten|Anzahl|Insgesamt|Gesamte eingehende Nachrichten für einen namespace|
|OUTMSGS|Ausgehende Nachrichten|Anzahl|Insgesamt|Ausgehende Nachrichten für einen Namespace gesamt|
|EHINMBS|Eingehende Bytes pro Sekunde|BytesPerSecond|Insgesamt|Ereignis-Hub eingehende Nachrichtendurchsatz für einen namespace|
|EHOUTMBS|Ausgehende Bytes pro Sekunde|BytesPerSecond|Insgesamt|Ausgehende Nachrichten für einen Namespace gesamt|
|EHABL|Rückstand Nachrichten archivieren|Anzahl|Insgesamt|Hub Archiv Nachrichten im Rückstand für einen namespace|
|EHAMSGS|Archivieren Sie Nachrichten|Anzahl|Insgesamt|Event Hub archivierten Nachrichten in einem namespace|
|EHAMBS|Archiv Nachrichtendurchsatz|BytesPerSecond|Insgesamt|Ereignis-Hub archivierte Nachrichtendurchsatz in einem namespace|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|RunsStarted|Führt gestartet|Anzahl|Insgesamt|Anzahl der Workflow wird gestartet.|
|RunsCompleted|Testläufe abgeschlossen|Anzahl|Insgesamt|Anzahl der Workflow wird abgeschlossen.|
|RunsSucceeded|Erfolgreich ausgeführt|Anzahl|Insgesamt|Anzahl der Workflow wird erfolgreich ausgeführt.|
|RunsFailed|Konnte nicht ausgeführt|Anzahl|Insgesamt|Anzahl der Workflow führt fehlgeschlagen.|
|RunsCancelled|Führt abgebrochen|Anzahl|Insgesamt|Anzahl der Workflow wird abgebrochen.|
|RunLatency|Wartezeit ausführen|Sekunden|Durchschnitt|Latenz des abgeschlossenen Workflows ausgeführt wird.|
|RunSuccessLatency|Wartezeit für erfolgreich ausgeführt|Sekunden|Durchschnitt|Wartezeit für erfolgreiche Workflow ausgeführt wird.|
|RunThrottledEvents|Drosselung Ereignisse ausführen|Anzahl|Insgesamt|Anzahl der Workflowaktion oder Trigger gedrosselt Ereignisse.|
|RunFailurePercentage|Prozentsatz Fehler führen|Prozent|Insgesamt|Prozentsatz der Workflow führt fehlgeschlagen.|
|ActionsStarted|Aktionen gestartet |Anzahl|Insgesamt|Anzahl der Workflowaktionen gestartet.|
|ActionsCompleted|Aktionen |Anzahl|Insgesamt|Anzahl der Workflowaktionen abgeschlossen.|
|ActionsSucceeded|Aktionen erfolgreich |Anzahl|Insgesamt|Anzahl der Aktivitäten war erfolgreich.|
|ActionsFailed|Aktionen fehlgeschlagen|Anzahl|Insgesamt|Anzahl der Workflow-Aktionen ist fehlgeschlagen.|
|ActionsSkipped|Aktionen übersprungen |Anzahl|Insgesamt|Workflowaktionen übersprungen.|
|ActionLatency|Aktion-Wartezeit |Sekunden|Durchschnitt|Wartezeit der abgeschlossenen Aktivitäten.|
|ActionSuccessLatency|Aktion erfolgreich Wartezeit |Sekunden|Durchschnitt|Wartezeit für erfolgreiche Aktivitäten.|
|ActionThrottledEvents|Aktion gedrosselt Ereignisse|Anzahl|Insgesamt|Anzahl der Workflowaktion gedrosselt Ereignisse...|
|TriggersStarted|Trigger gestartet |Anzahl|Insgesamt|Anzahl der Workflow-Trigger gestartet.|
|TriggersCompleted|Trigger abgeschlossen |Anzahl|Insgesamt|Anzahl der Workflow Trigger abgeschlossen.|
|TriggersSucceeded|Trigger erfolgreich |Anzahl|Insgesamt|Anzahl der Workflow Trigger war erfolgreich.|
|TriggersFailed|Fehler bei Trigger |Anzahl|Insgesamt|Anzahl der Workflow Trigger ist fehlgeschlagen.|
|TriggersSkipped|Trigger übersprungen|Anzahl|Insgesamt|Workflow Trigger übersprungen.|
|TriggersFired|Trigger ausgelöst |Anzahl|Insgesamt|Anzahl der Workflow Trigger ausgelöst.|
|TriggerLatency|Trigger-Wartezeit |Sekunden|Durchschnitt|Wartezeit der abgeschlossenen Workflows Trigger.|
|TriggerFireLatency|Trigger Feuer Wartezeit |Sekunden|Durchschnitt|Latenz wird Workflow Trigger ausgelöst.|
|TriggerSuccessLatency|Trigger Erfolg Wartezeit |Sekunden|Durchschnitt|Wartezeit von erfolgreich Workflow Trigger.|
|TriggerThrottledEvents|Drosselung Ereignisse auslösen|Anzahl|Insgesamt|Anzahl der Workflow Trigger gedrosselt Ereignisse.|
|BillableActionExecutions|Berechenbare Aktion wie|Anzahl|Insgesamt|Anzahl der Workflow Aktion wie immer berechnet.|
|BillableTriggerExecutions|Berechenbare Trigger-Ausführung|Anzahl|Insgesamt|Anzahl der Workflow Trigger wie immer berechnet.|
|TotalBillableExecutions|Gesamte abrechenbare Ausführung|Anzahl|Insgesamt|Anzahl der Workflow-Ausführung immer berechnet.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|Durchsatz|Durchsatz|BytesPerSecond|Durchschnitt||

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|SearchLatency|Wartezeit bei der Suche|Sekunden|Durchschnitt|Suche durchschnittliche Wartezeit für den Suchdienst|
|SearchQueriesPerSecond|Suchanfragen pro Sekunde|CountPerSecond|Durchschnitt|Suchvorgänge pro Sekunde für den Suchdienst|
|ThrottledSearchQueriesPercentage|Eingeschränkte Suche Abfragen Prozentsatz|Prozent|Durchschnitt|Prozentsatz der Suchvorgänge für den Suchdienst gedrosselt wurde|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|CPUXNS|CPU-Verwendung pro namespace|Prozent|Maximale|Service Bus Premium Namespace CPU Verwendung Metrik|
|WSXNS|Größe der Speicherverbrauch pro namespace|Prozent|Maximale|Service Bus Premium Namespace Speicher Verwendung Metrik|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|
|physical_data_read_percent|Daten e/a-Prozentsatz|Prozent|Durchschnitt|Daten e/a-Prozentsatz|
|log_write_percent|Protokoll i/o-Prozentsatz|Prozent|Durchschnitt|Protokoll i/o-Prozentsatz|
|dtu_consumption_percent|DTU in Prozent|Prozent|Durchschnitt|DTU in Prozent|
|Speicher|Gesamtgröße der Datenbanken|Bytes|Maximale|Gesamtgröße der Datenbanken|
|connection_successful|Erfolgreiche Verbindung|Anzahl|Insgesamt|Erfolgreiche Verbindung|
|connection_failed|Verbindungsfehler|Anzahl|Insgesamt|Verbindungsfehler|
|blocked_by_firewall|Von der Firewall blockiert|Anzahl|Insgesamt|Von der Firewall blockiert|
|Deadlock|Deadlocks|Anzahl|Insgesamt|Deadlocks|
|storage_percent|Datenbank Größe in Prozent|Prozent|Maximale|Datenbank Größe in Prozent|
|xtp_storage_percent|Im Arbeitsspeicher OLTP-Speicher-percent(Preview)|Prozent|Durchschnitt|Im Arbeitsspeicher OLTP-Speicher-percent(Preview)|
|workers_percent|Arbeitskräfte in Prozent|Prozent|Durchschnitt|Arbeitnehmer %|
|sessions_percent|Sessions Prozent|Prozent|Durchschnitt|Sessions Prozent|
|dtu_limit|DTU-Begrenzung|Anzahl|Durchschnitt|DTU-Begrenzung|
|dtu_used|DTU verwendet|Anzahl|Durchschnitt|DTU verwendet|
|service_level_objective|SLO der Datenbank|Anzahl|Insgesamt|SLO der Datenbank|
|dwu_limit|Dwu-Begrenzung|Anzahl|Maximale|Dwu-Begrenzung|
|dwu_consumption_percent|DWU in Prozent|Prozent|Durchschnitt|DWU in Prozent|
|dwu_used|DWU verwendet|Anzahl|Durchschnitt|DWU verwendet|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|cpu_percent|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|
|physical_data_read_percent|Daten e/a-Prozentsatz|Prozent|Durchschnitt|Daten e/a-Prozentsatz|
|log_write_percent|Protokoll i/o-Prozentsatz|Prozent|Durchschnitt|Protokoll i/o-Prozentsatz|
|dtu_consumption_percent|DTU in Prozent|Prozent|Durchschnitt|DTU in Prozent|
|storage_percent|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|
|workers_percent|Arbeitnehmer %|Prozent|Durchschnitt|Arbeitnehmer %|
|sessions_percent|Sessions Prozent|Prozent|Durchschnitt|Sessions Prozent|
|eDTU_limit|eDTU-Begrenzung|Anzahl|Durchschnitt|eDTU-Begrenzung|
|storage_limit|Speicherkapazität|Bytes|Durchschnitt|Speicherkapazität|
|eDTU_used|eDTU verwendet|Anzahl|Durchschnitt|eDTU verwendet|
|storage_used|Verwendeter Speicher|Bytes|Durchschnitt|Verwendeter Speicher|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|ResourceUtilization|SU % Auslastung|Prozent|Maximale|SU % Auslastung|
|InputEvents|Ereignisse|Anzahl|Insgesamt|Ereignisse|
|InputEventBytes|Eingabeereignis Bytes|Bytes|Insgesamt|Eingabeereignis Bytes|
|LateInputEvents|Spät Eingabeereignisse|Anzahl|Insgesamt|Spät Eingabeereignisse|
|OutputEvents|Ausgabeereignisse|Anzahl|Insgesamt|Ausgabeereignisse|
|ConversionErrors|Fehler bei der Konvertierung|Anzahl|Insgesamt|Fehler bei der Konvertierung|
|Fehler|Laufzeitfehler|Anzahl|Insgesamt|Laufzeitfehler|
|DroppedOrAdjustedEvents|Reihenfolge von Ereignissen|Anzahl|Insgesamt|Reihenfolge von Ereignissen|
|AMLCalloutRequests|Funktion Anfragen|Anzahl|Insgesamt|Funktion Anfragen|
|AMLCalloutFailedRequests|Fehlgeschlagene Funktion Anfragen|Anzahl|Insgesamt|Fehlgeschlagene Funktion Anfragen|
|AMLCalloutInputEvents|Funktion Ereignisse|Anzahl|Insgesamt|Funktion Ereignisse|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|CpuPercentage|CPU-Prozentsatz|Prozent|Durchschnitt|CPU-Prozentsatz|
|MemoryPercentage|Speicher in Prozent|Prozent|Durchschnitt|Speicher in Prozent|
|DiskQueueLength|Warteschlangenlänge des Datenträgers|Anzahl|Insgesamt|Warteschlangenlänge des Datenträgers|
|HttpQueueLength|HTTP-Länge|Anzahl|Insgesamt|HTTP-Länge|
|BytesReceived|Daten In|Bytes|Insgesamt|Daten In|
|BytesSent|Daten|Bytes|Insgesamt|Daten|

## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Insgesamt|CPU-Zeit|
|Anfragen|Anfragen|Anzahl|Insgesamt|Anfragen|
|BytesReceived|Daten In|Bytes|Insgesamt|Daten In|
|BytesSent|Daten|Bytes|Insgesamt|Daten|
|Http2xx|HTTP-2xx|Anzahl|Insgesamt|HTTP-2xx|
|Http3xx|HTTP-3xx|Anzahl|Insgesamt|HTTP-3xx|
|Http401|HTTP-Fehler 401|Anzahl|Insgesamt|HTTP-Fehler 401|
|Http403|HTTP 403|Anzahl|Insgesamt|HTTP 403|
|Http404|HTTP 404|Anzahl|Insgesamt|HTTP 404|
|Http406|HTTP 406|Anzahl|Insgesamt|HTTP 406|
|Http4xx|HTTP-4xx|Anzahl|Insgesamt|HTTP-4xx|
|Http5xx|HTTP-Server-Fehler|Anzahl|Insgesamt|HTTP-Server-Fehler|
|MemoryWorkingSet|Arbeit|Bytes|Insgesamt|Arbeit|
|AverageMemoryWorkingSet|Durchschnittliche Arbeitsspeicher Arbeitssatz|Bytes|Durchschnitt|Durchschnittliche Arbeitsspeicher Arbeitssatz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrik|Metrische Anzeigenamen|Einheit|Typ|Beschreibung|
|---|---|---|---|---|
|CpuTime|CPU-Zeit|Sekunden|Insgesamt|CPU-Zeit|
|Anfragen|Anfragen|Anzahl|Insgesamt|Anfragen|
|BytesReceived|Daten In|Bytes|Insgesamt|Daten In|
|BytesSent|Daten|Bytes|Insgesamt|Daten|
|Http2xx|HTTP-2xx|Anzahl|Insgesamt|HTTP-2xx|
|Http3xx|HTTP-3xx|Anzahl|Insgesamt|HTTP-3xx|
|Http401|HTTP-Fehler 401|Anzahl|Insgesamt|HTTP-Fehler 401|
|Http403|HTTP 403|Anzahl|Insgesamt|HTTP 403|
|Http404|HTTP 404|Anzahl|Insgesamt|HTTP 404|
|Http406|HTTP 406|Anzahl|Insgesamt|HTTP 406|
|Http4xx|HTTP-4xx|Anzahl|Insgesamt|HTTP-4xx|
|Http5xx|HTTP-Server-Fehler|Anzahl|Insgesamt|HTTP-Server-Fehler|
|MemoryWorkingSet|Arbeit|Bytes|Insgesamt|Arbeit|
|AverageMemoryWorkingSet|Durchschnittliche Arbeitsspeicher Arbeitssatz|Bytes|Durchschnitt|Durchschnittliche Arbeitsspeicher Arbeitssatz|
|AverageResponseTime|Durchschnittliche Antwortzeit|Sekunden|Durchschnitt|Durchschnittliche Antwortzeit|

## <a name="next-steps"></a>Nächste Schritte

- [Metriken in Azure Monitor Informationen](./monitoring-overview.md#monitoring-sources)
- [Alerts Kennzahlen erstellen](./insights-receive-alert-notifications.md)
- [Exportieren Sie Metriken, Speicher, Ereignis-Hub oder Protokollanalyse](./monitoring-overview-of-diagnostic-logs.md)
