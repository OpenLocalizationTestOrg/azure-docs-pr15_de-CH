<properties
    pageTitle="DocumentDB Node.js-API und SDK | Microsoft Azure"
    description="Lernen Sie die Node.js-API und SDK einschließlich Veröffentlichungsdaten Pensionsdaten und Änderungen zwischen jeder DocumentDB Node.js SDK-Version."
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-APIs und SDKs

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>DocumentDB Node.js-API und SDK

<table>
<tr><td>**SDK herunterladen**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**API-Dokumentation**</td><td>[Node.js-API-Dokumentation](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**SDK-Installationshinweise**</td><td>[Installationshinweise](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Contribute SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**Beispiele**</td><td>[Node.js-Beispiele](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Gestartete Get-Lernprogramm**</td><td>[Erste Schritte mit Node.js-SDK](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Web app Lernprogramm**</td><td>[Erstellen einer Node.js Webanwendung DocumentDB](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Aktuelle unterstützte Plattform**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>Versionshinweise

###<a name="1.10.0"/>1.10.0</a>

- Unterstützung für cross-Partition parallele Abfragen.
- Unterstützung für TOP-ORDER BY-Abfragen für partitionierte Sammlungen.

###<a name="1.9.0"/>1.9.0</a>

- Unterstützung Drosselung Anfragen hinzugefügt wiederholen. (Drosselung Anfragen eine Anforderung Satz groß Ausnahme Fehlercode 429.) Standardmäßig versucht DocumentDB neun Mal für jede Anforderung bei Fehlercode 429 RetryAfter Zeit im Antwortheader berücksichtigt. Eine feste Retry-Intervall kann jetzt festgelegt werden als Teil der RetryOptions-Eigenschaft für das Objekt ConnectionPolicy RetryAfter Zeit zwischen den Wiederholungsversuchen vom Server zurückgegebenen ignorieren soll. DocumentDB nun wartet bis zu 30 Sekunden für jede Anforderung beschränkt (unabhängig von der Wiederholungsanzahl) und gibt die Antwort mit Fehler 429. Diesmal kann auch Überschreiben in RetryOptions Eigenschaft ConnectionPolicy-Objekts.

- DocumentDB gibt jetzt X-ms-Gas-Retry-Count und x-ms-throttle-retry-wait-time-ms, wie der Antwortheader bei jeder Anforderung die Schubkontrolle kennzeichnen wiederholen, Anzahl und der kummulative die Anforderung zwischen den Wiederholungsversuchen gewartet.

- Die RetryOptions-Klasse wurde hinzugefügt, die RetryOptions-Eigenschaft für die ConnectionPolicy-Klasse, mit der einige Standardoptionen wiederholen überschreiben.

###<a name="1.8.0"/>1.8.0</a>

 - Unterstützung für mit mehreren Datenbankkonten.

###<a name="1.7.0"/>1.7.0</a>

- Unterstützung für Zeit, Live(TTL)-Funktion für Dokumente.

###<a name="1.6.0"/>1.6.0</a>

- Implementierte [partitionierte Sammlungen](documentdb-partition-data.md) und [benutzerdefinierte Leistung](documentdb-performance-levels.md).

###<a name="1.5.6"/>1.5.6</a>

- RangePartitionResolver.resolveForRead Fehler, keine Links durch eine fehlerhafte Concat Ergebnisse zurückgegeben wurde.

###<a name="1.5.5"/>1.5.5</a>

- Feste HashParitionResolver resolveForRead(): Wenn keine Partitionsschlüssel angegeben wurde Ausnahme, anstatt eine Liste aller registrierten links.

###<a name="1.5.4"/>1.5.4</a>

- Fixes [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - Ausgabe ausschließlich HTTPS Agent: Ändern des globalen Agents für DocumentDB Zwecke. Verwenden Sie dedizierte Agenten für alle Anfragen der Lib.

###<a name="1.5.3"/>1.5.3</a>

- Updates ausstellen [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - ordnungsgemäß Handle Striche in Media-Ids.

###<a name="1.5.2"/>1.5.2</a>

- Updates ausstellen [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - Listener Speicherverluste Warnung EventEmitter.

###<a name="1.5.1"/>1.5.1</a>

- Updates erteilen [92](https://github.com/Azure/azure-documentdb-node/issues/90) - Ordner umbenennen Hash Hash für Groß-und Kleinschreibung Systeme.

### <a name="1.5.0"/>1.5.0</a>

- Sharding unterstützt von Hash und Partition Konfliktlöser hinzufügen.

### <a name="1.4.0"/>1.4.0</a>

- Implementieren Sie Upsert. Neue UpsertXXX Methoden "documentclient".

### <a name="1.3.0"/>1.3.0</a>

- Versionsnummern mit anderen SDKs bringen übersprungen.

### <a name="1.2.2"/>1.2.2</a>

- Geteilte Q verspricht Wrapper zum neuen Repository.
- Package-Datei Npm Registrierung aktualisieren.

### <a name="1.2.1"/>1.2.1</a>

- Geräte-ID basierend weiterleiten.
- Behebt Problem [Nr. 49](https://github.com/Azure/azure-documentdb-node/issues/49) - Methode current() aktuelle Eigenschaft widerspricht.

### <a name="1.2.0"/>1.2.0</a>

- Unterstützung für Geodaten Index hinzugefügt.
- ID-Eigenschaft für alle Ressourcen überprüft. IDs für Ressourcen dürfen nicht?, #, & #47; #47; Zeichen oder mit einem Leerzeichen enden.
- ResourceResponse hinzugefügt neue Header "Index Transformation Bearbeitung".

### <a name="1.1.0"/>1.1.0</a>

- Volltextindizierung implementiert V2-Richtlinie.

### <a name="1.0.3"/>1.0.3</a>

- Problem Nr. 40 (https://github.com/Azure/azure-documentdb-node/issues/40) - implementiert Eslint und schwierige Konfigurationen im Core und Promise SDK.

### <a name="1.0.2"/>1.0.2</a>

- Problem [Nr. 45](https://github.com/Azure/azure-documentdb-node/issues/45) - Versprechen Wrapper enthält keinen Header mit Fehler.

### <a name="1.0.1"/>1.0.1</a>

- Implementierte Möglichkeit zum Abfragen von Konflikten ReadConflicts, ReadConflictAsync und QueryConflicts.
- Aktualisierte API-Dokumentation.
- Ausgabe [Nr. 41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync-Fehler.

### <a name="1.0.0"/>1.0.0</a>

- GA-SDK.

## <a name="release--retirement-dates"></a>Version & Endtermine
Microsoft bietet Benachrichtigung mindestens **12 Monate** vor Renteneintritt eine SDK um Übergangs auf eine neuere bzw. unterstützt.

Neue Features und Funktionen und Optimierungen werden nur aktuelle SDK hinzugefügt, so wird empfohlen, dass Sie immer die neueste SDK-Version so früh wie möglich aktualisieren.

Antrag auf DocumentDB mit einem zurückgezogenen SDK wird vom Dienst abgelehnt.

> [AZURE.WARNING]
Alle Versionen von Azure DocumentDB SDK Node.js vor Version **1.0.0** werden am **29. Februar 2016**zurückgezogen.

<br/>

| Version | Veröffentlichungsdatum | Pensionsdatum
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 03 Oktober 2016 |---
| [1.9.0](#1.9.0) | 07 Juli 2016 |---
| [1.8.0](#1.8.0) | 14. Juni 2016 |---
| [1.7.0](#1.7.0) | 26 April 2016 |---
| [1.6.0](#1.6.0) | 29. März 2016 |---
| [1.5.6](#1.5.6) | 08 März 2016 |---
| [1.5.5](#1.5.5) | 02 Februar 2016 |---
| [1.5.4](#1.5.4) | 01 Februar 2016 |---
| [1.5.2](#1.5.2) | 26. Januar 2016 |---
| [1.5.2](#1.5.2) | 22. Januar 2016 |---
| [1.5.1](#1.5.1) | 4. Januar 2016 |---
| [1.5.0](#1.5.0) | 31. Dezember 2015 |---
| [1.4.0](#1.4.0) | 06 Oktober 2015 |---
| [1.3.0](#1.3.0) | 06 Oktober 2015 |---
| [1.2.2](#1.2.2) | 10. September 2015 |---
| [1.2.1](#1.2.1) | 15. August 2015 |---
| [1.2.0](#1.2.0) | 05 August 2015 |---
| [1.1.0](#1.1.0) | 09 Juli 2015 |---
| [1.0.3](#1.0.3) | 04 Juni 2015 |---
| [1.0.2](#1.0.2) | 23. Mai 2015 |---
| [1.0.1](#1.0.1) | 15. Mai 2015 |---
| [1.0.0](#1.0.0) | 08 April 2015 |---
| 0.9.4-Prerelease | 06 April 2015 | 29. Februar 2016
| 0.9.3-Prerelease | 14. Januar 2015 | 29. Februar 2016
| 0.9.2-Prerelease | 18 Dezember 2014 | 29. Februar 2016
| 0.9.1-Prerelease | 22. August 2014 | 29. Februar 2016
| 0.9.0-Prerelease | 21. August 2014 | 29. Februar 2016


## <a name="faq"></a>Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zu DocumentDB finden Sie unter [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Seite.
