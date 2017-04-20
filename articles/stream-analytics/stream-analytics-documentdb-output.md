<properties
    pageTitle="JSON-Ausgabe für Stream Analytics | Microsoft Azure"
    description="Erfahren Sie, wie Stream Analytics Azure DocumentDB als JSON-Ausgabe für die Archivierung und niedriger Latenz Abfragen auf unstrukturierte JSON-Daten festlegen."
    keywords="JSON-Ausgabe"
    documentationCenter=""
    services="stream-analytics,documentdb"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

# <a name="target-azure-documentdb-for-json-output-from-stream-analytics"></a>Ziel Azure DocumentDB für die JSON-Ausgabe von Stream Analytics

Stream Analytics können [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) JSON Ausgabe aktivieren Datenabfragen Archivierung und niedriger Latenz auf unstrukturierte JSON-Daten abzielen. Erfahren Sie, wie diese am besten implementieren.

Wer mit DocumentDB nicht vertraut sind, betrachten Sie Schritte [DocumentDBs Lernpfad](https://azure.microsoft.com/documentation/learning-paths/documentdb/) .

## <a name="basics-of-documentdb-as-an-output-target"></a>Grundlagen von DocumentDB als Ausgabeziel
Azure DocumentDB Ausgabe im Stream Analytics ermöglicht schreiben die Verarbeitung der Ergebnisse als JSON-Ausgabe in Ihrem DocumentDB Sammlung(en) Stream. Stream Analytics erstellt Sammlungen in der Datenbank keine stattdessen müssen sie im Voraus erstellen. Deshalb sind Verrechnungskosten DocumentDB Sammlungen transparent und damit die Leistung, Konsistenz und Ihre Sammlungen direkt mit [DocumentDB APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx)optimiert werden kann. Empfiehlt eine DocumentDB Datenbank pro Auftrag streaming auf Ihre Sammlungen für streaming Auftrag logisch getrennt.

DocumentDB Auflistung Optionen werden nachfolgend beschrieben.

## <a name="tune-consistency-availability-and-latency"></a>Konsistenz, Verfügbarkeit und Wartezeit optimieren

Um Ihre Anwendung Anforderungen kann DocumentDB Optimieren der Datenbank und Sammlungen und stellen Kompromisse zwischen Konsistenz, Verfügbarkeit und Wartezeit. Je nach was lesen Konsistenz Belieben Szenario für Lesen und Schreiben Latenz Konsistenz auf Ihrem Datenbank-Konto auswählen können. Außerdem kann standardmäßig DocumentDB synchronen Indizierung bei jedem CRUD-Vorgang zu Ihrer Sammlung. Dies ist eine andere nützliche Option Schreib-Lese-Performance in DocumentDB steuern. Weitere Informationen zu diesem Thema lesen Sie den Artikel [ändern Ihre Datenbank und Abfrage Konsistenz](../documentdb/documentdb-consistency-levels.md) .

## <a name="choose-a-performance-level"></a>Wählen Sie Leistung

DocumentDB Sammlungen können Ebene 3 unterschiedliche Performance (S1, S2 oder S3) erstellt werden, die den verfügbaren Durchsatz für CRUDs auf diese Auflistung zu bestimmen. Darüber hinaus werden Indizierung/Consistency Ebenen auf die Leistung beeinflusst. Finden Sie in [diesem Artikel](../documentdb/documentdb-performance-levels.md) verstehen diese Leistungsmerkmale im Detail.

## <a name="upserts-from-stream-analytics"></a>Upserts von Stream Analytics

Stream Analytics Integration mit DocumentDB können Sie einfügen oder Aktualisieren von Datensätzen in der DocumentDB-Sammlung basierend auf einer bestimmten Dokument-ID-Spalte. Dies wird auch als ein *Upsert*bezeichnet.

Stream Analytics verwendet optimistischen Upsert Ansatz, wo Updates nur beim Einfügen aufgrund einer Dokument-ID fehlschlägt erfolgen. Dieses Update erfolgt durch Stream Analytics als PATCH, so können partielle Updates Dokument, z. B. Hinzufügen neuer Eigenschaften oder eine vorhandene Eigenschaft inkrementell durchgeführt. Beachten Sie, dass die Werte von Arrayeigenschaften in Ihrem Ergebnis JSON-Dokument in das gesamte Array immer überschrieben, d. h. nicht zusammengeführt.

## <a name="data-partitioning-in-documentdb"></a>Daten in DocumentDB-Partitionierung

DocumentDB Sammlungen können Daten anhand der Abfragemuster und Leistung benötigt Ihre Anwendung partitionieren. Jede Auflistung dürfen bis zu 10GB Daten (maximal) und derzeit besteht keine Möglichkeit zu skalieren (overflow) eine Auflistung. Skalierung, Stream Analytics können mehrere Sammlungen mit einem bestimmten Präfix geschrieben (Siehe Nutzungsdetails unten). Stream Analytics verwendet die konsistente [Hash Partition Resolver](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.partitioning.hashpartitionresolver.aspx) Strategie basierend auf den vom Benutzer bereitgestellten PartitionKey Spalte Ausgabedatensätze zu partitionieren. Die Anzahl der Collections mit dem angegebenen Präfix zum Startzeitpunkt des Auftrags streaming als die Ausgabe Partitionsanzahl, der Auftrag in parallel schreibt verwendet (DocumentDB Sammlungen = Ausgabe Partitionen). Für eine Sammlung mit verzögerte Indizierung S3 nur, um 0,4 Schreibdurchsatz MB/s erwarten fügt. Verwenden mehrere Sammlungen können Sie höheren Durchsatz und höhere Kapazität.

Wenn die Anzahl der Partitionen in Zukunft erhöht werden soll, müssen Sie zu Ihrem Auftrag, Partitionieren der Daten aus vorhandenen Sammlungen in neue Stream Analytics-Auftrag starten. Ausführliche PartitionResolver und neu partitionieren Beispielcode wird in einem folgeartikel enthalten. [Partitionierung in DocumentDB](../articles/documentdb-partition-data.md#developing-a-partitioned-application) Artikel enthält auch Details dazu.

## <a name="documentdb-settings-for-json-output"></a>DocumentDB-Einstellung für die JSON-Ausgabe

DocumentDB als Ausgabe in Stream Analytics erstellen erzeugt eine Aufforderung Informationen wie folgt. Dieser Abschnitt erläutert die Definition der Eigenschaften.

![Documentdb Stream Analytics Ausgabebild](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output.png)  

-   **Ausgabealias** – Alias verweisen diese Ausgabe in Ihrer Abfrage ASA  
-   **Kontoname** – der Name oder die Endpunkt-URI des DocumentDB-Kontos.  
-   **Konto-Schlüssel** – freigegebene Tastenkombination für das DocumentDB-Konto.  
-   **Datenbank** -Datenbankname der DocumentDB.  
-   **Auflistung Muster** -Auflistung Namensmuster Sammlungen verwendet werden. Das Format der Auflistung kann mithilfe des Tokens optionale {Partition} Partitionen von 0 beginnt erstellt werden. Folgendes Beispiel gültige Eingaben sind:  
   1\) MyCollection – eine Sammlung namens "MyCollection" bestehen.  
   2\) MyCollection {Partition} – diese Sammlungen bestehen – "MyCollection0", "MyCollection1" und "MyCollection2".  
-   **Partitionsschlüssel** – den Namen des Feldes im Ausgabeereignisse verwendet, um den Schlüssel für die Partitionierung Ausgabe über Sammlungen. Für die Sammlung Ausgabe kann beliebige Ausgabespalte verwendet, z. B. IDs.  
-   **Dokument-ID** – Optional. Der Name des Feldes in ausgabeereignissen an den Primärschlüssel, die INSERT-, den oder Update Vorgänge basieren.  
