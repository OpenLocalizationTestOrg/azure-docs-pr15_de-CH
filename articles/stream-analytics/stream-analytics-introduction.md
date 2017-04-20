<properties 
    pageTitle="Einführung in Stream Analytics | Microsoft Azure" 
    description="Erfahren Sie mehr über Stream Analytics ein verwalteter Dienst, Analysieren von Daten über das Internet der Dinge (IoT) in Echtzeit." 
    keywords="Analytics und managed Services, Verarbeitung, streaming Analytics, was Stream Analytics"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="what-is-stream-analytics"></a>Was ist Stream Analytics?

Azure Stream Analytics ist eine vollständig verwaltete, kostengünstige Echtzeit-Ereignis verarbeiten, mit der Tiefe Einblicke aus entsperren. Stream Analytics erleichtert die in Echtzeit auf Daten aus Geräten, Sensoren, Websites, social Media, Applikationen, Infrastruktur und mehr analytischen Berechnungen einrichten.

Mit wenigen Mausklicks in Azure-Portal Stream Analytics-Auftrag angeben die Eingabequelle Streamingdaten Ausgabe Senke für die Ergebnisse Ihrer Arbeit erstellen und eine Datentransformation in eine SQL-ähnliche Sprache. Sie können überwachen und Skalierung/Geschwindigkeit Ihres Auftrags im Azure-Portal Skalieren von ein paar Kilobyte mindestens ein Gigabyte pro Sekunde verarbeiteten Ereignisse.

Stream Analytics mehrjährige Microsoft Research Arbeit streaming-Engines für zeitkritische Verarbeitung sowie Integrationen intuitive Spezifikationen dieser Sprache optimiert.

## <a name="what-can-i-use-stream-analytics-for"></a>Was kann ich Stream Analyse verwenden?
Daten fließen mit hoher Geschwindigkeit über das Netzwerk heute. Organisationen, die verarbeiten und Bearbeiten dieser Daten in Echtzeit, drastisch verbessern Effizienz und im Markt differenzieren. Szenarien streaming Echtzeitanalysen finden Sie in allen Branchen: personalisierte Echtzeit Aktienhandel Analyse und Warnungen angebotenen Unternehmen; Echtzeit-Erkennung; Daten- und Schutzdienste; zuverlässige Erfassung und Analyse der Daten von den Sensoren und Aktuatoren eingebettet in physischen Objekten (Internet der Dinge oder IoT); Web Clickstream-Analyse; und Kundenpflege (CRM)-Anwendung Alerts bei Kundenzufriedenheit innerhalb beschädigt ist. Unternehmen suchen die flexible, zuverlässige und kostengünstige so diese Analyse in Echtzeit Ereignisstream geprägten modernen Geschäftswelt erfolgreich.

## <a name="key-capabilities-and-benefits"></a>Schlüsselfunktionen und Vorteile
-   **Bedienung:** Stream Analytics unterstützt einen einfachen, deklarativen Query-Modell zum Beschreiben von Transformationen. Optimierung für einfache Bedienung Stream Analytics verwendet eine T-SQL-Variante und beseitigt die Notwendigkeit für Kunden mit den technischen Aspekten der Stream Systeme. [Stream Analytics Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx) in Abfrage-Browser-Editor verwenden, erhalten Sie IntelliSense AutoVervollständigen können Sie schnell und einfach implementieren Time Series Abfragen, einschließlich zeitliche Basis Joins, Fenstermodus Aggregate zeitliche Filter und andere gemeinsame Vorgänge wie Joins, Aggregate, Projektionen und Filter. Darüber hinaus kann im Browser Abfragetests gegen eine Beispieldatendatei schnelle und iterative Entwicklung.  

-   **Skalierbar:** Stream Analytics kann große Durchsatz von bis zu 1GB pro Sekunde. Integration in [Azure Ereignis Hubs](https://azure.microsoft.com/services/event-hubs/) und [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) die Lösung Millionen Ereignisse pro zweite aus Geräten Navigationsdaten, Aufnahme und Protokolldateien zu nennen. Um dies zu erreichen, nutzt Stream Analytics Partitionierung Funktion der Veranstaltung 1 MB/s pro Partition führen können. Benutzer können die Berechnung in mehrere logische Schritte innerhalb der Abfragedefinition partitionieren, jeweils mit weiter erhöhen der Skalierbarkeit partitioniert.  

-   **Zuverlässigkeit, Wiederholbarkeit und schnelle Recovery:** Managed Service in der Cloud Stream Analytics Datenverlusten und Business Continuity bei Ausfällen durch integrierte Funktionen. Die Möglichkeit, intern Zustand beibehalten bietet die reproduzierbare Ergebnisse sicherzustellen, ist es möglich, Ereignisse archivieren erneut verarbeiten, immer die gleichen Ergebnisse erhalten. Dadurch kann Kunden zurück und Berechnungen bei der Ursachenanalyse, was-wenn-Analysen usw. überprüfen.  

-   **Niedrige Kosten:** Als Cloud-Dienst ist Stream Analytics optimiert, damit Benutzer sehr kostengünstig Überblick und echtzeitanalyselösungen verwalten. Der Dienst wird für die Quellenbesteuerung je Einheit Streaming Nutzung und Daten vom System erstellt. Nutzung wird basierend auf dem Volume der verarbeiteten Ereignisse und Compute Leistung innerhalb des Clusters bereitgestellt, die jeweiligen Stream Analytics Projekte abgeleitet.  

-   **Verweisen auf Daten:** Stream Analytics bietet Benutzern die Möglichkeit zu geben Daten. Möglicherweise Daten oder einfach nicht-Streaming-Daten, die weniger häufig mit der Zeit ändert. Das System vereinfacht die Verwendung von Referenzdaten behandelt wie alle anderen eingehenden Ereignis mit anderen Ereignisstreams aufgenommen in Echtzeit Transformationen durchführen.  

-   **Benutzerdefinierte Funktionen:** Stream Analytics bietet Integration mit Azure Machine Learning Funktionsaufrufe in Machine Learning-Dienst als Teil einer Abfrage Stream Analytics definieren. Dies erweitert das Stream Analytics vorhandenen Azure Machine Learning Solutions nutzen. Weitere Informationen hierzu finden Sie [Computerlernen Integration Tutorial](stream-analytics-machine-learning-integration-tutorial.md).

-   **Verbindung:** Stream Analytics Verbindung direkte Azure Ereignis Hubs und Azure IoT Hubs Stream Ingestion und Azure BLOB-Dienst Daten aufnehmen. Ergebnisse können geschrieben werden aus Stream Analytics Azure Storage Blobs oder Tabellen, Azure SQL DB, Azure See Datenspeichern, DocumentDB, Ereignis-Hubs, Azure Service Bus Topics oder Warteschlangen und Power BI, kann dann visualisiert, weitere Workflows verarbeitet, Batch Analytics über [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) verwendet oder als eine Reihe von Ereignissen erneut verarbeitet. Bei Ereignis-Hubs ist es können mehrere Stream Analytics sowie anderen Datenquellen Verarbeitung Motoren ohne streaming Art der Berechnung.  

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
Sie haben einen verwalteten Service für streaming-Daten über das Internet der Dinge Analytics Stream Analytics eingeführt. Weitere Informationen zu diesem Dienst finden Sie unter:

- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

