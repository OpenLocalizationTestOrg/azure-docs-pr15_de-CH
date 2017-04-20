
<properties
    pageTitle="DocumentDB Java API und SDK | Microsoft Azure"
    description="Lernen Sie die Java API und SDK einschließlich Veröffentlichungsdaten Pensionsdaten und Änderungen zwischen jeder DocumentDB Java SDK-Version."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-APIs und SDKs

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>DocumentDB Java API und SDK

<table>
<tr><td>**SDK-Download**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**API-Dokumentation**</td><td>[Java API-Referenzdokumentation](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Contribute SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit Java SDK](documentdb-java-application.md)</td></tr>
<tr><td>**Aktuelle unterstützte Laufzeit**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Release Notes

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Unterstützung für BoundedStaleness Konsistenz auf.
  - Unterstützung für direkte Konnektivität für CRUD-Operationen für partitionierte Sammlungen.
  - Fehler bei der Abfrage einer Datenbank mit SQL.
  - Fehler im Sitzungscache wo Sitzungstoken falsch festgelegt werden.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Unterstützung für cross-Partition parallele Abfragen.
  - Unterstützung für TOP-ORDER BY-Abfragen für partitionierte Sammlungen.
  - Unterstützung für starke Konsistenz.
  - Unterstützung für Namen Anfragen beim direkte Verbindung verwenden.
  - Feste zu ActivityId auf Anforderung Versuche konsistent bleiben.
  - Ein Fehler beim erneuten Erstellen einer Auflistung mit demselben Namen auf den Sitzungscache beziehen.
  - Polygon und LineString DataTypes während der Indizierung Richtlinie für räumliche Abfragen geofencing-Auflistung hinzugefügt.
  - Behobene Probleme mit Java für Java 1.8.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - Fehler in PartitionKeyDefinitionMap Zwischenspeichern Einzelpartition Sammlungen und nicht zusätzliche Schlüssel Anfragen partition abrufen.
  - Fehler nicht wiederholen, wenn ein falsche Partitionswert bereitgestellt wird.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - Unterstützung für mit mehreren Datenbankkonten.
  - Unterstützung für automatische Wiederholung Drosselung Anfragen mit max. Wiederholungsversuche und max Wiederholungszeit anpassen.  Siehe RetryOptions und ConnectionPolicy.getRetryOptions().
  - Veraltete IPartitionResolver Grundlage benutzerdefinierten Partitionierung Code. Verwenden Sie partitionierte Sammlungen höhere Speicher- und Durchsatz.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Unterstützung für die Drosselung hinzugefügte wiederholen.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Zeit live (TTL) Unterstützung für Dokumente.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Implementierte [partitionierte Sammlungen](documentdb-partition-data.md) und [benutzerdefinierte Leistung](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Fehler in HashPartitionResolver Hash-Werte im little-Endian mit anderen SDKs zu generieren.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Hinzufügen von Hash und Konfliktlöser gerne mit Sharding über mehrere Partitionen partitionieren.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Implementieren Sie Upsert. Neue UpsertXXX Methoden hinzugefügt Upsert.
- Implementieren Sie ID-basierte Routing. Keine öffentliche API ändert, alle internen ändert.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Veröffentlichung übersprungen zu Versionsnummer mit anderen SDKs

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Unterstützt räumlichen Index
- ID-Eigenschaft für alle Ressourcen überprüft. IDs für Ressourcen dürfen nicht ?, # \, Zeichen oder mit einem Leerzeichen enden.
- ResourceResponse hinzugefügt neue Header "Index Transformation Bearbeitung".

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- Volltextindizierung implementiert V2-Richtlinie

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- GA SDK

## <a name="release--retirement-dates"></a>Version & Endtermine
Microsoft bietet Benachrichtigung mindestens **12 Monate** vor Renteneintritt eine SDK um Übergangs auf eine neuere bzw. unterstützt.

Neue Features und Funktionen und Optimierungen werden nur aktuelle SDK hinzugefügt, so wird empfohlen, dass Sie immer die neueste SDK-Version so früh wie möglich aktualisieren.

Antrag auf DocumentDB mit einem zurückgezogenen SDK wird vom Dienst abgelehnt.

> [AZURE.WARNING]
Alle Versionen von Azure DocumentDB SDK für Java vor Version **1.0.0** werden am **29. Februar 2016**zurückgezogen.

<br/>

| Version | Veröffentlichungsdatum | Pensionsdatum
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 28 Oktober 2016 |---
| [1.9.0](#1.9.0) | 03 Oktober 2016 |---
| [1.8.1](#1.8.1) | 30. Juni 2016 |---
| [1.8.0](#1.8.0) | 14. Juni 2016 |---
| [1.7.1](#1.7.1) | 30. April 2016 |---
| [1.7.0](#1.7.0) | 27 April 2016 |---
| [1.6.0](#1.6.0) | 29. März 2016 |---
| [1.5.1](#1.5.1) | 31. Dezember 2015 |---
| [1.5.0](#1.5.0) | 04 Dezember 2015 |---
| [1.4.0](#1.4.0) | 05 Oktober 2015 |---
| [1.3.0](#1.3.0) | 05 Oktober 2015 |---
| [1.2.0](#1.2.0) | 05 August 2015 |---
| [1.1.0](#1.1.0) | 09 Juli 2015 |---
| [1.0.1](#1.0.1) | 12. Mai 2015 |---
| [1.0.0](#1.0.0) | 07 April 2015 |---
| 0.9.5-prelease | 09 Mär 2015 | 29. Februar 2016
| 0.9.4-prelease | 17. Februar 2015 | 29. Februar 2016
| 0.9.3-prelease | 13. Januar 2015 | 29. Februar 2016
| 0.9.2-prelease | 19. Dezember 2014 | 29. Februar 2016
| 0.9.1-prelease | 19. Dezember 2014 | 29. Februar 2016
| 0.9.0-prelease | 10. Dezember 2014 | 29. Februar 2016

## <a name="faq"></a>Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zu DocumentDB finden Sie unter [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Seite.
