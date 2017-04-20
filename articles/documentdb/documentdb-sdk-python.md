<properties 
    pageTitle="DocumentDB Python-API und SDK | Microsoft Azure" 
    description="Lernen Sie die Python-API und SDK einschließlich Veröffentlichungsdaten Pensionsdaten und Änderungen zwischen jeder DocumentDB Python SDK-Version." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-APIs und SDKs

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>DocumentDB Python-API und SDK

<table>
<tr><td>**SDK herunterladen**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**API-Dokumentation**</td><td>[Python-API-Dokumentation](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**SDK-Installationshinweise**</td><td>[Informationen zur Installation von Python SDK](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Contribute SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit der Python-SDK](documentdb-python-application.md)</td></tr>
<tr><td>**Aktuelle unterstützte Plattform**</td><td>[Python 2.7](https://www.python.org/downloads/) und [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Versionshinweise

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Unterstützung für Python 3.5 hinzugefügt.
- Unterstützung für Verbindungspooling mit Anfragen-Modul.
- Unterstützung für Session-Konsistenz.
- Unterstützung für TOP-ORDERBY Abfragen für partitionierte Sammlungen.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Unterstützung Drosselung Anfragen hinzugefügt wiederholen. (Drosselung Anfragen eine Anforderung Satz groß Ausnahme Fehlercode 429.) Standardmäßig versucht DocumentDB neun Mal für jede Anforderung bei Fehlercode 429 RetryAfter Zeit im Antwortheader berücksichtigt. Eine feste Retry-Intervall kann jetzt festgelegt werden als Teil der RetryOptions-Eigenschaft für das Objekt ConnectionPolicy RetryAfter Zeit zwischen den Wiederholungsversuchen vom Server zurückgegebenen ignorieren soll. DocumentDB nun wartet bis zu 30 Sekunden für jede Anforderung beschränkt (unabhängig von der Wiederholungsanzahl) und gibt die Antwort mit Fehler 429. Diesmal kann auch Überschreiben in RetryOptions Eigenschaft ConnectionPolicy-Objekts.

- DocumentDB gibt jetzt X-ms-Gas-Retry-Count und x-ms-throttle-retry-wait-time-ms, wie der Antwortheader bei jeder Anforderung die Schubkontrolle kennzeichnen wiederholen, Anzahl und der kummulative die Anforderung zwischen den Wiederholungsversuchen gewartet.

- RetryPolicy-Klasse und die entsprechende Eigenschaft (Retry_policy) der Document_client-Klasse entfernt und stattdessen führte eine RetryOptions-Klasse die RetryOptions-Eigenschaft für ConnectionPolicy-Klasse, mit der einige Standardoptionen wiederholen überschreiben.

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - Unterstützung für mit mehreren Datenbankkonten.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- Unterstützung für Zeit, Live(TTL)-Funktion für Dokumente.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Fehlerbehebungen im Zusammenhang mit Server Seite Partitionierung, um Sonderzeichen im Pfad Partitionkey ermöglichen.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Implementierte [partitionierte Sammlungen](documentdb-partition-data.md) und [benutzerdefinierte Leistung](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Hinzufügen von Hash und Konfliktlöser gerne mit Sharding über mehrere Partitionen partitionieren.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Implementieren Sie Upsert. Neue UpsertXXX Methoden hinzugefügt Upsert.
- Implementieren Sie ID-basierte Routing. Keine öffentliche API ändert, alle internen ändert.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Unterstützt räumlichen Index.
- ID-Eigenschaft für alle Ressourcen überprüft. IDs für Ressourcen dürfen nicht ?, # \, Zeichen oder mit einem Leerzeichen enden.
- ResourceResponse hinzugefügt neue Header "Index Transformation Bearbeitung".

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- Volltextindizierung implementiert V2-Richtlinie.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Proxy-Verbindung unterstützt.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA-SDK.

## <a name="release--retirement-dates"></a>Version & Ruhestand Datumsangaben
Microsoft bietet Benachrichtigung mindestens **12 Monate** vor Renteneintritt eine SDK um Übergangs auf eine neuere bzw. unterstützt.

Neue Features und Funktionen und Optimierungen werden nur aktuelle SDK hinzugefügt, so wird empfohlen, dass Sie immer die neueste SDK-Version so früh wie möglich aktualisieren. 

Antrag auf DocumentDB mit einem zurückgezogenen SDK wird vom Dienst abgelehnt.

> [AZURE.WARNING]
Alle Azure DocumentDB SDK für Python-Versionen vor Version **1.0.0** werden am **29. Februar 2016**zurückgezogen. 

<br/>

| Version | Veröffentlichungsdatum | Pensionsdatum 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 29. September 2016 |---
| [1.9.0](#1.9.0) | 07 Juli 2016 |---
| [1.8.0](#1.8.0) | 14. Juni 2016 |---
| [1.7.0](#1.7.0) | 26 April 2016 |---
| [1.6.1](#1.6.1) | 08 April 2016 |---
| [1.6.0](#1.6.0) | 29. März 2016 |---
| [1.5.0](#1.5.0) | 03 Januar 2016 |---
| [1.4.2](#1.4.2) | 06 Oktober 2015 |---
| [1.4.1](#1.4.1) | 06 Oktober 2015 |---
| [1.2.0](#1.2.0) | 06 August 2015 |---
| [1.1.0](#1.1.0) | 09 Juli 2015 |---
| [1.0.1](#1.0.1) | 25 Mai 2015 |---
| [1.0.0](#1.0.0) | 07 April 2015 |---
| 0.9.4-prelease | 14. Januar 2015 | 29. Februar 2016
| 0.9.3-prelease | 09 Dezember 2014 | 29. Februar 2016
| 0.9.2-prelease | 25 November 2014 | 29. Februar 2016
| 0.9.1-prelease | 23 September 2014 | 29. Februar 2016
| 0.9.0-prelease | 21. August 2014 | 29. Februar 2016

## <a name="faq"></a>Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zu DocumentDB finden Sie unter [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Seite. 
