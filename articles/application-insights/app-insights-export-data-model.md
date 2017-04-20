<properties 
    pageTitle="Anwendung Insights-Datenmodell" 
    description="Beschreibt die Eigenschaften von fortlaufenden Export in JSON exportiert und als Filter verwendet." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Anwendung Einblicke Export-Datenmodell

Diese Tabelle listet Eigenschaften des Telemetrie von [Application Insights](app-insights-overview.md) -SDKs auf das Portal gesendet. Sie sehen diese Eigenschaften aus dem [Kontinuierlichen exportieren](app-insights-export-telemetry.md).
Sie werden auch in Eigenschaftenfiltern [Metrik Explorer](app-insights-metrics-explorer.md) und [Diagnose suchen](app-insights-diagnostic-search.md).

Punkte zu beachten:

* `[0]`in diesen Tabellen gibt einen Punkt im Pfad Sie einen Index einfügen müssen; Dies ist jedoch immer 0.
* Zeitdauer in Zehntelsekunden Mikrosekunden, so 10000000 sind == 1 Sekunde.
* Datums- und Uhrzeitangaben sind sind UTC, und im ISO-format`yyyy-MM-DDThh:mm:ss.sssZ`

Es gibt mehrere [Beispiele](app-insights-export-telemetry.md#code-samples) , in denen ihre Verwendung veranschaulicht.



## <a name="example"></a>Beispiel

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Kontext

Alle Arten von Telemetriedaten begleitet ein Kontextabschnitt. Nicht alle Felder werden mit jeder Datenpunkt.



|Pfad|Typ|Notizen|
|---|---|---|
| Context.Custom.Dimensions [0]  | Objekt]  | Zeichenfolge mit Schlüssel-Wert-Paare Festlegen benutzerdefinierter Eigenschaftenparameter. Maximale Länge 100 Werte maximale Länge 1024. Mehr als 100 eindeutige Werte die Eigenschaft kann durchsucht aber nicht Segmentierungsalgorithmus verwendet. Max. 200 Schlüssel pro Ikey.  |
| Context.Custom.Metrics [0]  | Objekt]  | Schlüssel-Wert-Paare in benutzerdefinierte Maßeinheiten Parameter und TrackMetrics festgelegt. Max. 100, können Werte numerisch sein. |
| context.data.eventTime | Zeichenfolge | UTC |
| context.data.isSynthetic | boolescher Wert | Anforderung wird aus Bot oder Web Test angezeigt. |
| context.data.samplingRate | Anzahl | Prozentsatz der Telemetrie vom Portal an SDK generiert. Bereich 0,0 100,0.|
| Context.Device | Objekt | Client-Gerät |
| Context.Device.Browser | Zeichenfolge | IE, Chrome... |
| context.device.browserVersion | Zeichenfolge | Chrome 48,0... |
| context.device.deviceModel | Zeichenfolge | |
| context.device.deviceName | Zeichenfolge | |
| Context.Device.ID | Zeichenfolge | |
| Context.Device.Locale | Zeichenfolge | En-GB, de-DE... |
| Context.Device.Network | Zeichenfolge | |
| context.device.oemName | Zeichenfolge | |
| context.device.osVersion | Zeichenfolge | Host-Betriebssystem |
| context.device.roleInstance | Zeichenfolge | ID des Server-Hosts |
| context.device.roleName | Zeichenfolge | |
| Context.Device.Type | Zeichenfolge | PC, Browser... |
| Context.Location | Objekt | Clientip abgeleitet. |
| Context.Location.City | Zeichenfolge | Abgeleitet von Clientip, sofern bekannt  |
| Context.Location.ClientIP | Zeichenfolge | Letzte Achteck ist 0 anonymisiert. |
| Context.Location.Continent | Zeichenfolge | |
| Context.Location.Country | Zeichenfolge | |
| Context.Location.Province | Zeichenfolge | Bundesland oder Kanton |
| Context.Operation.ID | Zeichenfolge | Elemente mit gleichen Vorgangs-Id werden als verknüpfte im Portal angezeigt. In der Regel die Kennung. |
| Context.Operation.Name | Zeichenfolge | URL oder Anforderung |
| context.operation.parentId | Zeichenfolge | Ermöglicht geschachtelten verwandte Elemente. |
| Context.Session.ID | Zeichenfolge | ID einer Gruppe von Vorgängen aus derselben Quelle. 30 Minuten ohne Operation signalisiert das Ende einer Sitzung. |
| context.session.isFirst | boolescher Wert | |
| context.user.accountAcquisitionDate | Zeichenfolge | |
| context.user.anonAcquisitionDate | Zeichenfolge | |
| context.user.anonId | Zeichenfolge | |
| context.user.authAcquisitionDate | Zeichenfolge | [Authentifizierte Benutzer](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | boolescher Wert | |
| internal.data.documentVersion | Zeichenfolge | |
| Internal.Data.ID | Zeichenfolge | |



## <a name="events"></a>Ereignisse

Benutzerdefinierte Ereignisse generiert durch [Trackevent()"](app-insights-api-custom-events-metrics.md#track-event) 


|Pfad|Typ|Notizen|
|---|---|---|
| Ereignisanzahl [0] | ganze Zahl | 100 /[(Abtastrate)](app-insights-sampling.md) . Beispiel 4 =&gt; 25 %. |
| Ereignisname [0] | Zeichenfolge | Name des Ereignisses.  Max. Länge 250. |
| [0] Ereignis-url | Zeichenfolge | |
| Ereignis [0] urlData.base | Zeichenfolge | |
| Ereignis [0] urlData.host | Zeichenfolge | |

## <a name="exceptions"></a>Ausnahmen

Berichte- [Ausnahmen](app-insights-asp-net-exceptions.md) auf dem Server und im Browser. 


|Pfad|Typ|Notizen|
|---|---|---|
| BasicException [0] assembly | Zeichenfolge | |
| Anzahl der BasicException [0] | ganze Zahl | 100 /[(Abtastrate)](app-insights-sampling.md) . Beispiel 4 =&gt; 25 %. |
| BasicException [0] exceptionGroup | Zeichenfolge | |
| BasicException [0] exceptionType | Zeichenfolge | |Zeichenfolge | |
| BasicException [0] failedUserCodeMethod | Zeichenfolge | |
| BasicException [0] failedUserCodeAssembly | Zeichenfolge | |
| BasicException [0] handledAt | Zeichenfolge | |
| BasicException [0] hasFullStack | boolescher Wert | |
| BasicException [0]-id | Zeichenfolge | |
| BasicException [0]-Methode | Zeichenfolge | |
| BasicException [0] Nachricht | Zeichenfolge | Meldung der Ausnahme. Max. Länge 10k.|
| BasicException [0] outerExceptionMessage | Zeichenfolge | |
| BasicException [0] outerExceptionThrownAtAssembly | Zeichenfolge | |
| BasicException [0] outerExceptionThrownAtMethod | Zeichenfolge | |
| BasicException [0] outerExceptionType | Zeichenfolge | |
| BasicException [0] outerId | Zeichenfolge | |
| BasicException [0] ParsedStack [0] assembly | Zeichenfolge | |
| BasicException [0] ParsedStack [0] Dateiname | Zeichenfolge | |
| BasicException [0] ParsedStack [0] auf | ganze Zahl | |
| BasicException [0] ParsedStack [0] Zeile | ganze Zahl | |
| BasicException [0] ParsedStack [0]-Methode | Zeichenfolge | |
| BasicException [0] Stapel | Zeichenfolge | Max. Länge 10k|
| BasicException [0] typeName | Zeichenfolge | |



## <a name="trace-messages"></a>Verfolgen von Nachrichten

[TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)und [Protokollierung Adapter](app-insights-asp-net-trace-logs.md)gesendet.


|Pfad|Typ|Notizen|
|---|---|---|
| Message [0] loggerName | Zeichenfolge ||
| Meldungsparameter [0] | Zeichenfolge ||
| raw Message [0] | Zeichenfolge | Die Lognachricht maximale Länge 10k. |
| Message [0] severityLevel | Zeichenfolge | |



## <a name="remote-dependency"></a>Remote-Abhängigkeit

TrackDependency gesendet. Mit der Leistung von Berichten und Nutzung von [Abhängigkeiten Aufrufe](app-insights-asp-net-dependencies.md) auf dem Server und AJAX Ruft im Browser.

|Pfad|Typ|Notizen|
|---|---|---|
| asynchrone RemoteDependency [0] | boolescher Wert | |
| RemoteDependency [0] baseName | Zeichenfolge |  |
| RemoteDependency [0] commandName | Zeichenfolge | Beispielsweise "Home/Index" |
| Anzahl der RemoteDependency [0] | ganze Zahl | 100 /[(Abtastrate)](app-insights-sampling.md) . Beispiel 4 =&gt; 25 %. |
| RemoteDependency [0] dependencyTypeName | Zeichenfolge | HTTP, SQL... |
| RemoteDependency [0] durationMetric.value | Anzahl | Zeit vom Anruf bis zum Abschluss der Antwort Abhängigkeit |
| RemoteDependency [0]-id | Zeichenfolge | |
| RemoteDependency [0] name | Zeichenfolge | URL. Max. Länge 250.|
| Rückgabewert von RemoteDependency [0] | Zeichenfolge | HTTP-Abhängigkeit |
| RemoteDependency [0] erfolgreich | boolescher Wert | |
| Typ RemoteDependency [0] | Zeichenfolge | HTTP, Sql... |
| RemoteDependency [0]-url | Zeichenfolge |  Max. Länge 2000 |
| RemoteDependency [0] urlData.base | Zeichenfolge | Max. Länge 2000 |
| RemoteDependency [0] urlData.hashTag | Zeichenfolge | |
| RemoteDependency [0] urlData.host | Zeichenfolge | Max. Länge 200 |


## <a name="requests"></a>Anfragen

[TrackRequest](app-insights-api-custom-events-metrics.md#track-request)gesendet. Standardmodule können Berichte Serverantwortzeit gemessen am Server. 


|Pfad|Typ|Notizen|
|---|---|---|
| Anzahl [0] | ganze Zahl | 100 /[(Abtastrate)](app-insights-sampling.md) . Beispiel: 4 =&gt; 25 %. |
| Anforderung [0] durationMetric.value | Anzahl | Zeit von Anforderung zu antworten. 1e7 == 1 s |
| [0] Kennung | Zeichenfolge | Vorgangs-id |
| Anforderungsnamen [0] | Zeichenfolge | GET/POST + Basis-Url.  Max. Länge 250 |
| [0] ResponseCode anfordern | ganze Zahl | HTTP-Antwort an Client gesendet |
| [0] Erfolg anfordern | boolescher Wert | Standard == (ResponseCode &lt; 400) |
| [0] Url anfordern | Zeichenfolge | Einschließlich nicht |
| Anforderung [0] urlData.base | Zeichenfolge | |
| Anforderung [0] urlData.hashTag | Zeichenfolge |  |
| Anforderung [0] urlData.host | Zeichenfolge | |


## <a name="page-view-performance"></a>Seitenansicht-Leistung

Vom Browser gesendet. Misst die Verarbeitungszeit für eine Seite Benutzer die Anforderung abgeschlossen (ohne asynchronen AJAX-Aufrufe) angezeigt.

Client-Betriebssystem und Browserversion anzeigen Kontextwerte. 


|Pfad|Typ|Notizen|
|---|---|---|
| ClientPerformance [0] clientProcess.value | ganze Zahl | Uhrzeit Ende empfangen den HTML-Code die Seite angezeigt. |
| ClientPerformance [0] name | Zeichenfolge | |
| ClientPerformance [0] networkConnection.value | ganze Zahl | Zeit zu einem Netzwerk herzustellen. |
| ClientPerformance [0] receiveRequest.value | ganze Zahl | Uhrzeit Ende Senden der Anforderung an den HTML-Code als Antwort empfangen. |
| ClientPerformance [0] sendRequest.value | ganze Zahl | Uhrzeit der HTTP-Anfrage durchgeführt. |
| ClientPerformance [0] total.value | ganze Zahl | Zeit zum Senden der Anforderung an die Seite ab. |
| ClientPerformance [0]-url | Zeichenfolge | URL dieser Anforderung |
| ClientPerformance [0] urlData.base | Zeichenfolge | |
| ClientPerformance [0] urlData.hashTag | Zeichenfolge | |
| ClientPerformance [0] urlData.host | Zeichenfolge | |
| ClientPerformance [0] urlData.protocol | Zeichenfolge | |

## <a name="page-views"></a>Seitenaufrufe

Per trackPageView() oder [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|Pfad|Typ|Notizen|
|---|---|---|
| Anzahl [0] | ganze Zahl | 100 /[(Abtastrate)](app-insights-sampling.md) . Beispiel 4 =&gt; 25 %. |
| Ansicht [0] durationMetric.value | ganze Zahl | Wert optional in trackPageView() oder startTrackPage() - stopTrackPage(). Nicht identisch mit der ClientPerformance-Werte. |
| Ansichtsname [0] | Zeichenfolge | Seitentitel.  Max. Länge 250 |
| [0] Url anzeigen | Zeichenfolge | |
| Ansicht [0] urlData.base | Zeichenfolge | |
| Ansicht [0] urlData.hashTag | Zeichenfolge | |
| Ansicht [0] urlData.host | Zeichenfolge | |



## <a name="availability"></a>Verfügbarkeit

[Verfügbarkeit von Webtests](app-insights-monitor-web-app-availability.md)erstellt.

|Pfad|Typ|Notizen|
|---|---|---|
| Verfügbarkeit [0] availabilityMetric.name | Zeichenfolge | Verfügbarkeit |
| Verfügbarkeit [0] availabilityMetric.value | Anzahl |1.0 oder 0,0 |
| Anzahl der Verfügbarkeit [0] | ganze Zahl | 100 /[(Abtastrate)](app-insights-sampling.md) . Beispiel 4 =&gt; 25 %. |
| Verfügbarkeit [0] dataSizeMetric.name | Zeichenfolge | |
| Verfügbarkeit [0] dataSizeMetric.value | ganze Zahl | |
| Verfügbarkeit [0] durationMetric.name | Zeichenfolge | |
| Verfügbarkeit [0] durationMetric.value | Anzahl | Dauer des Tests. 1e7 == 1 s |
| Mitteilung zur Verfügbarkeit [0] | Zeichenfolge | Fehler-Diagnose |
| Verfügbarkeit [0] Ergebnis | Zeichenfolge | Bestanden/nicht bestanden |
| Verfügbarkeit [0] runLocation | Zeichenfolge | Geo-Quelle der HTTP-Anforderung |
| Verfügbarkeit [0] testName | Zeichenfolge | |
| Verfügbarkeit [0] testRunId | Zeichenfolge | |
| Verfügbarkeit [0] testTimestamp | Zeichenfolge | |




## <a name="metrics"></a>Metriken

Durch TrackMetric() generiert.

Der Metrikwert wurde in context.custom.metrics[0 gefunden]

Zum Beispiel:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Metrische Werte

Metrikwerte in metrischen Berichte und anderswo, werden mit einer standard-Objekt-Struktur gemeldet. Zum Beispiel:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Aktuell - aber möglicherweise dazu in Zukunft – alle Werte von den Standardmodulen SDK gemeldet `count==1` und die `name` und `value` Felder sind hilfreich. Schreiben Sie Ihre eigenen Aufrufe TrackMetric ist der einzige Fall, andere wären, wäre in dem anderen Parameter festgelegt. 

Die anderen Felder dient Metriken im SDK zu Datenverkehr zum Portal zusammengefasst werden sollen. Mehrere aufeinander folgende Werte z. B. kann Mittelwert, vor dem Senden jedes Metrikbericht. Klicken Sie berechnen, min, Max, Standardabweichung und Aggregatwert (Summe oder Durchschnitt) und Anzahl der Anzahl der im Bericht dargestellten Werte festlegen. 

In der obigen Tabelle haben wir selten verwendete Felder Count, min, Max, StdDev und SampledValue angegeben.

Vorab aggregieren Metriken können Sie [Sampling](app-insights-sampling.md) anstelle Telemetrie reduziert werden soll.


### <a name="durations"></a>Dauer

Anders Dauer in Zehntel einer Mikrosekunde vertreten 10000000.0 1 Sekunde bedeutet.



## <a name="see-also"></a>Siehe auch

* [Anwendung Einblicke](app-insights-overview.md) 
* [Kontinuierliche exportieren](app-insights-export-telemetry.md)
* [Code-Beispiele](app-insights-export-telemetry.md#code-samples)


