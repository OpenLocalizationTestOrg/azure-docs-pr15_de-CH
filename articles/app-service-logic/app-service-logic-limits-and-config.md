<properties
    pageTitle="Logik App Grenzen und Konfiguration | Microsoft Azure"
    description="Übersicht über die Service und Konfigurationswerte für Logik-Apps."
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="jeffhollan"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>

# <a name="logic-app-limits-and-configuration"></a>Logik App Grenzen und Konfiguration

Hier werden Informationen über die aktuellen und Konfigurationsdetails für Azure Logik Apps.

## <a name="limits"></a>Grenzen

### <a name="http-request-limits"></a>HTTP-Anforderung Grenzwerte

Grenzwerte für einen einzelnen HTTP-Connector bzw. sind

#### <a name="timeout"></a>Zeitlimit

|Name|Grenzwert|Notizen|
|----|----|----|
|Timeout bei Anforderung|1 minute|Ein [asynchroner](app-service-logic-create-api-app.md) oder [until-Schleife](app-service-logic-loops-and-scopes.md) kompensieren kann nach Bedarf|

#### <a name="message-size"></a>Nachrichtengröße

|Name|Grenzwert|Notizen|
|----|----|----|
|Nachrichtengröße|50 MB|Einige Verbinder und APIs unterstützen 50 MB.  Anforderung Trigger unterstützt bis zu 25MB|
|Ausdruck Evaluierungssoftware beschränkt|131.072 Zeichen|`@concat()`, `@base64()`, `string` nicht länger|

#### <a name="retry-policy"></a>Richtlinie wiederholen

|Name|Grenzwert|Notizen|
|----|----|----|
|Wiederholungsversuche|4|Mit der [Richtlinienparameter wiederholen](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) können|
|Max. Verzögerung|1 Stunde|Mit der [Richtlinienparameter wiederholen](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) können|
|Min. Verzögerung|20 Min.|Mit der [Richtlinienparameter wiederholen](https://msdn.microsoft.com/en-us/library/azure/mt643939.aspx) können|

### <a name="run-duration-and-retention"></a>Dauer des Testlaufs und Aufbewahrung

Dies sind die Grenzwerte für eine logische Anwendung ausführen.

|Name|Grenzwert|Notizen|
|----|----|----|
|Dauer des Testlaufs|90 Tage||
|-Aufbewahrung|90 Tage|Dies ist von der zur Startzeit|
|Min Serienintervall|15 Sek.||
|Max. Serienintervall|500 Tage||


### <a name="looping-and-debatching-limits"></a>Schleifen und vorgelagerte Grenzwerte

Grenzwerte für eine logische Anwendung auszuführen sind.

|Name|Grenzwert|Notizen|
|----|----|----|
|ForEach-Elemente|5.000|Der [Abfragevorgang](../connectors/connectors-native-query.md) können größere Arrays Bedarf filtern|
|Bis Iterationen|10.000||
|SplitOn Elemente|5.000||
|ForEach-Parallelität|20|Sequenzielle Foreach soll, durch Hinzufügen von `"operationOptions": "Sequential"` , die `foreach` Aktion|


### <a name="throughput-limits"></a>Durchsatz Grenzwerte

Grenzwerte für eine logische app-Instanz sind. 

|Name|Grenzwert|Notizen|
|----|----|----|
|Trigger pro Sekunde|100|Workflows können mehrere Apps verteilt werden, Bedarf|

### <a name="definition-limits"></a>Definition Grenzen

Grenzwerte für eine logische app Definition sind.

|Name|Grenzwert|Notizen|
|----|----|----|
|Aktionen in ForEach|1|Sie können geschachtelte Workflows erweitern diese nach Bedarf hinzufügen.|
|Aktionen pro workflow|60|Sie können geschachtelte Workflows erweitern diese nach Bedarf hinzufügen.|
|Zulässige Aktion Verschachtelungstiefe|5|Sie können geschachtelte Workflows erweitern diese nach Bedarf hinzufügen.|
|Flows pro Region pro Abonnement|1000||
|Trigger pro workflow|10||
|Max. Zeichen pro Ausdruck|8.192||
|Max. `trackedProperties` Größe in Zeichen|16.000|
|`action`/`trigger`Anzahl|80||
|`description`Beschränkung|256||
|`parameters`Grenzwert|50||
|`outputs`Grenzwert|10||

## <a name="configuration"></a>Konfiguration

### <a name="ip-address"></a>IP-Adresse

Anrufe von einem [Connector](../connectors/apis-list.md) kommen aus der unten angegebenen Adresse.

Aufrufe von einer Logik app direkt (d. h. über [HTTP](../connectors/connectors-native-http.md) oder [HTTP + stolz](../connectors/connectors-native-http-swagger.md)) kommen aus dem [Azure Datacenter IP-Bereiche](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

|Logik App Region|Ausgehendes IP|
|-----|----|
|Australien OST|40.126.251.213|
|Australien Südost|40.127.80.34|
|Brasilien Süd|191.232.38.129|
|Zentrale Indien|104.211.98.164|
|USA|40.122.49.51|
|Ostasien|23.99.116.181|
|Osten der USA|191.237.41.52|
|USA 2 OST|104.208.233.100|
|Japan OST|40.115.186.96|
|Japan West|40.74.130.77|
|Norden der USA – zentral|65.52.218.230|
|Nordeuropa|104.45.93.9|
|Südlichen zentralen USA|104.214.70.191|
|Südostasien|13.76.231.68|
|Süd Indien|104.211.227.225|
|Westeuropa|40.115.50.13|
|West Indien|104.211.161.203|
|Westen der USA|104.40.51.248|


## <a name="next-steps"></a>Nächste Schritte  

- Zunächst mit Logik Apps Lernprogramm die [Logik App erstellen](app-service-logic-create-a-logic-app.md) .  
- [Ansicht-Beispiele und Szenarien](app-service-logic-examples-and-scenarios.md)
- [Automatisieren Sie Geschäftsprozesse mit Logik Apps](http://channel9.msdn.com/Events/Build/2016/T694) 
- [Integrieren Sie Ihre Systeme mit Logik Apps](http://channel9.msdn.com/Events/Build/2016/P462)