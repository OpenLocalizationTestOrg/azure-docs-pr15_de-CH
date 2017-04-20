<properties 
    pageTitle="DocumentDB .NET API und SDK | Microsoft Azure" 
    description="Lernen Sie die .NET API und SDK einschließlich Veröffentlichungsdaten Pensionsdaten und Änderungen zwischen jeder DocumentDB .NET SDK-Version." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB-APIs und SDKs 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [REST](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>DocumentDB .NET API und SDK

<table>
<tr><td>**SDK-download**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**API-Dokumentation**</td><td>[.NET API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Beispiele**</td><td>[Codebeispiele für .NET](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Erste Schritte**</td><td>[Erste Schritte mit DocumentDB .NET SDK](documentdb-get-started.md)</td></tr>
<tr><td>**Web app Lernprogramm**</td><td>[Webanwendungsentwicklung mit DocumentDB](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Aktuelle unterstützten framework**</td><td>[Microsoft.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Release Notes

> [AZURE.IMPORTANT] Ab Version 1.9.2 Version erhalten System.NotSupportedException Sie beim Abfragen von partitionierter Sammlungen. Um diesen Fehler zu vermeiden, dass der Hostprozess 64-Bit ist. Für ausführbare Projekte können dies durch Deaktivieren der Option "Bevorzugen 32-Bit" im Fenster Projekt-Eigenschaften auf der Registerkarte erstellen.

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Unterstützung für partitionierte Sammlungen hinzugefügt direkte Verbindung.
  - Verbesserte Leistung für Ebene Konsistenz Alterung begrenzt.
  - Polygon und LineString DataTypes während der Indizierung Richtlinie für räumliche Abfragen geofencing-Auflistung hinzugefügt.
  - LINQ Unterstützung für StringEnumConverter, IsoDateTimeConverter und UnixDateTimeConverter während der Übersetzung Prädikate.
  - Verschiedene SDK Fehlerkorrekturen.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Ein Fehler der folgenden NotFoundException festgelegt: Lese Sitzung ist nicht für die Eingabe Sitzungstoken. Bereich Lesen eines Kontos geografisch verteilte Abfragen ist in einigen Fällen diese Ausnahme aufgetreten.
  - ResponseStream-Eigenschaft in der ResourceResponse-Klasse ermöglicht direkten Zugriff auf den zugrunde liegenden Stream aus einer Antwort ausgesetzt.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - Verändert die Klassen ResourceResponse, FeedResponse, StoredProcedureResponse und MediaResponse die entsprechende öffentliche Schnittstelle implementieren, sodass für testgesteuerte Bereitstellung (TDD) Mocks erstellt werden können.
  - Behoben, die einen ungültige Partition Schlüssel Header beim Serialisieren der Daten mit einer benutzerdefinierten JsonSerializerSettings-Objekt verursacht.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Behoben, die lang andauernden Abfragen fehl mit Fehler verursacht: Authentifizierungstoken ist zurzeit ungültig.
  - Behoben, die den ursprünglichen SqlParameter cross-Partition oben/Order-by-Abfragen entfernt.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Unterstützung für parallele Abfragen für partitionierte Sammlungen.
  - Unterstützung für cross-Partition ORDER BY und TOP-Abfragen für partitionierte Sammlungen.
  - Fixiert die fehlenden Verweise auf DocumentDB.Spatial.Sql.dll und Microsoft.Azure.Documents.ServiceInterop.dll, die erforderlich sind, wenn ein DocumentDB Projekt mit DocumentDB Nuget-Paket verweisen.
  - Parameter mit unterschiedlichen Typen verwenden, bei Verwendung von benutzerdefinierten Funktionen in LINQ behoben. 
  - Behoben Global replizierten Konten, wurden Upsert Aufrufe gerichtet statt schreiben Speicherorten gelesen.
  - Hinzugefügte Methoden der IDocumentClient-Schnittstelle, die fehlen: 
      - UpsertAttachmentAsync-Methode, Mediendatenstroms und Optionen als Parameter akzeptiert
      - CreateAttachmentAsync-Methode, die Optionen als Parameter akzeptiert
      - CreateOfferQuery-Methode, die QuerySpec als Parameter annimmt.
  - Nicht versiegelte öffentliche Klassen, die in der IDocumentClient-Schnittstelle verfügbar gemacht werden.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - Unterstützung für mit mehreren Datenbankkonten.
  - Unterstützung für Wiederholung Drosselung Anfragen.  Benutzer kann die Anzahl der Versuche und die maximale Wartezeit durch die ConnectionPolicy.RetryOptions Eigenschaft anpassen.
  - Eine neue IDocumentClient-Schnittstelle, die Signaturen aller DocumenClient Eigenschaften und Methoden definiert, hinzugefügt.  Als Teil dieser Änderung geändert auch Erweiterungsmethoden, die IQueryable und IOrderedQueryable Methoden für die Klasse "documentclient" selbst erstellen.
  - Hinzugefügt Konfigurationsoption ServicePoint.ConnectionLimit für einen bestimmten DocumentDB Endpunkt Uri festgelegt.  Verwenden Sie ConnectionPolicy.MaxConnectionLimit, um den Standardwert ändern, der auf 50 festgelegt ist.
  - Veraltete IPartitionResolver und deren Implementierung.  Unterstützung für IPartitionResolver ist veraltet. Es wird empfohlen, Sammlungen partitioniert für höhere Speicher- und Durchsatz zu verwenden.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Hinzugefügte Überlastung URI ExecuteStoredProcedureAsync Methode basiert, die RequestOptions als Parameter akzeptiert.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Zeit live (TTL) Unterstützung für Dokumente.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Behoben in Nuget-Paket .NET SDK für Verpackung als Teil einer Azure Cloud Service-Lösung.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Implementierte [partitionierte Sammlungen](documentdb-partition-data.md) und [benutzerdefinierte Leistung](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Feste]** DocumentDB Endpunkt löst Abfragen: "System.Net.Http.HttpRequestException: Fehler beim Kopieren von Inhalt in einem Stream.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Erweiterte LINQ Bereich Vergleich und Unterstützung neue Operatoren für paging bedingte Ausdrücke.
    - Nehmen Sie Operator SELECT TOP Verhalten in LINQ aktivieren
    - CompareTo-Operator Zeichenfolgenvergleiche Bereich aktivieren
    - Bedingte (?) und zusammengefügten Operatoren (?)
  - **[Feste]** ArgumentOutOfRangeException beim Kombinieren der Projektion Modell wo In Linq-Abfrage.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Feste]** Wählen Sie nicht den letzten Ausdruck ist der LINQ-Anbieter als angenommen ohne Projektion und erzeugt wählen * falsch.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Implementierte Upsert hinzugefügt UpsertXXXAsync Methoden
 - Leistungssteigerungen für alle Anfragen
 - LINQ-Anbieter unterstützt bedingten zusammenzufügen und CompareTo-Methoden für Zeichenfolgen
 - **[Feste]** LINQ-Anbieter--> implementieren Contains-Methode auf dieselbe SQL als IEnumerable und Array generieren
 - **[Feste]** BackoffRetryUtility verwendet den gleichen HttpRequestMessage erneut anstatt auf "Wiederholen"
 - **[Veraltet]** UriFactory.CreateCollection--> Verwenden Sie jetzt UriFactory.CreateDocumentCollection
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Feste]** Lokalisierung, wenn nicht En des Kulturinformation wie nl-NL usw. verwenden. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - Routing auf Grundlage der ID
    - Neue UriFactory Hilfe bei Bau ID basiert Ressourcenlinks
    - Neue Überladung zu URI "documentclient"
  - IsValid() und IsValidDetailed() in LINQ für Geodaten
  - LINQ-Anbieter-Unterstützung erweitert
    - **Math** - Abs Acos, ARCSIN Atan Decke, Cos, Exp, Floor, Log, Log10, Pow, runde, Zeichen, Sin, Sqrt Tan Abschneiden
    - **Zeichenfolge** - Concat EndsWith IndexOf Count, ToLower, TrimStart enthält, ersetzen, TrimEnd StartsWith Teilzeichenfolge, ToUpper umkehren
    - **Array** - Concat, enthält Count
    - Operator **IN**

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Unterstützung für Indexierungsrichtlinien ändern
    - Neue ReplaceDocumentCollectionAsync-Methode in "documentclient"
    - Neue IndexTransformationProgress Eigenschaft im ResourceResponse<T> für indexänderungen Prozent Fortschritt
    - DocumentCollection.IndexingPolicy ist jetzt veränderbar
  - Unterstützung für räumliche Indizierung und Abfrage
    - Neuer Microsoft.Azure.Documents.Spatial-Namespace für räumliche Typen serialisieren/Deserialisieren wie Point und Polygon
    - Neue SpatialIndex-Klasse für die Indizierung GeoJSON Daten in DocumentDB
  - **[Feste]** : falsche SQL-Abfrage generiert von Linq Ausdruck [#38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Abhängigkeit von Newtonsoft.Json v5.0.7 
- Änderungen Order By
  - LINQ-Anbieter Unterstützung für OrderBy() oder OrderByDescending()
  - IndexingPolicy Order By unterstützen 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Unterstützung für die Partitionierung von Daten mithilfe von neuen Klassen HashPartitionResolver und RangePartitionResolver und die IPartitionResolver
- DataContract-Serialisierung
- GUID-Unterstützung in LINQ-Anbieter
- UDF-Unterstützung in LINQ

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK

> [AZURE.NOTE]
NuGet-Paket namens zwischen Vorschau und GA wurde Wir verschoben von **Microsoft.Azure.Documents.Client** in **Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- Vorschau-SDKs [veraltet]

## <a name="release--retirement-dates"></a>Version & Endtermine
Microsoft bietet Benachrichtigung mindestens **12 Monate** vor Renteneintritt eine SDK um Übergangs auf eine neuere bzw. unterstützt.

Neue Features und Funktionen und Optimierungen werden nur aktuelle SDK hinzugefügt, so wird empfohlen, immer die neueste SDK-Version so früh wie möglich aktualisieren. 

Antrag auf DocumentDB mit einem zurückgezogenen SDK wird vom Dienst abgelehnt.

> [AZURE.WARNING]
Alle Versionen von Azure DocumentDB SDK für .NET vor Version **1.0.0** werden am **29. Februar 2016**zurückgezogen. 
 
<br/>
 
| Version | Veröffentlichungsdatum | Pensionsdatum 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 27. September 2016 |---
| [1.9.5](#1.9.5) | 01 September 2016 |---
| [1.9.4](#1.9.4) | 24 August 2016 |---
| [1.9.3](#1.9.3) | 15 August 2016 |---
| [1.9.2](#1.9.2) | 23. Juli 2016 |---
| 1.9.1 | Veraltet |---
| 1.9.0 | Veraltet |---
| [1.8.0](#1.8.0) | 14. Juni 2016 |---
| [1.7.1](#1.7.1) | 06 Mai 2016 |---
| [1.7.0](#1.7.0) | 26 April 2016 |---
| [1.6.3](#1.6.3) | 08 April 2016 |---
| [1.6.2](#1.6.2) | 29. März 2016 |---
| [1.5.3](#1.5.3) | 19. Februar 2016 |---
| [1.5.2](#1.5.2) | 14. Dezember 2015 |---
| [1.5.1](#1.5.1) | 23. November 2015 |---
| [1.5.0](#1.5.0) | 05 Oktober 2015 |---
| [1.4.1](#1.4.1) | 25 August 2015 |---
| [1.4.0](#1.4.0) | 13. August 2015 |---
| [1.3.0](#1.3.0) | 05 August 2015 |---
| [1.2.0](#1.2.0) | 06 Juli 2015 |---
| [1.1.0](#1.1.0) | 30. April 2015 |---
| [1.0.0](#1.0.0) | 08 April 2015 |---
| [0.9.3-prelease](#0.9.x-preview) | 12. März 2015 | 29. Februar 2016
| [0.9.2-prelease](#0.9.x-preview) | Januar 2015 | 29. Februar 2016
| [.9.1-prelease](#0.9.x-preview) | 13. Oktober 2014 | 29. Februar 2016
| [0.9.0-prelease](#0.9.x-preview) | 21. August 2014 | 29. Februar 2016

## <a name="faq"></a>Häufig gestellte Fragen
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Siehe auch

Weitere Informationen zu DocumentDB finden Sie unter [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) Seite. 
