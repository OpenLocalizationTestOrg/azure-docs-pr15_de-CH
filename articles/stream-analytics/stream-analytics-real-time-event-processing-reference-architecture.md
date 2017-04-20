<properties 
    pageTitle="Echtzeit-Ereignis mit Stream Analytics Verarbeitung | Microsoft Azure" 
    description="Erfahren Sie, wie eine Reihe von Azure Services in Echtzeit verarbeiten und Analytics zusammenarbeiten kann." 
    keywords="Echtzeit-Verarbeitung, Verarbeitung, Referenzarchitektur"
    services="stream-analytics,event-hubs,storage,sql-database" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="stream-analytics" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Referenzarchitektur: in Echtzeit mit Microsoft Azure Stream Analytics Ereignis

Die Referenzarchitektur für Echtzeit-Ereignis mit Azure Stream Analytics soll einen allgemeinen Entwurf für die Bereitstellung von Echtzeit-Platform as a Service (PaaS)-Verarbeitung Lösung mit Microsoft Azure.

## <a name="summary"></a>Zusammenfassung

Traditionell Analytics Solutions Funktionen wie ETL (Extract, Transform laden) und Datawarehousing basieren, in dem Daten vor der Analyse gespeichert werden. Veränderliche Anforderungen mehr schnell ankommenden Daten treiben vorhandene Modell Grenzwert. Analysieren von Daten in Streams vor Speicher verschieben ist eine Lösung, und während es keine neue Funktion, die Methode nicht weit angenommen in allen Branchen. 

Microsoft Azure bietet einen umfassenden Katalog Analytics Technologien unterstützt eine Reihe von verschiedenen Lösungsszenarios und Vorschriften. Auswählen der Azure-Dienste für eine End-to-End-Lösung bereitstellen kann eine Herausforderung der vielfältigen angeboten werden. Dieses Dokument dient zum Beschreiben der Funktionen und Interoperabilität der verschiedenen Azure Services, die eine Ereignis-Streaming-Lösung unterstützen. Darüber hinaus einige Szenarien, in denen Kunden von diesem Ansatz profitieren.

## <a name="contents"></a>Inhalt

- Zusammenfassung
- Einführung in Echtzeit-Analysen
- Nutzen von Echtzeitdaten in Azure
- Häufige Szenarien für Echtzeit-Analysen
- Architektur und Komponenten
    - Datenquellen
    - Datenintegration Ebene
    - Echtzeit-Analysen Ebene
    - Datenspeicherschicht
    - Präsentation / Layer-Verbrauch
- Abschluss

**Autor:** Charles Feddersen, Solution Architect Einblicke Rechenzentrum Excellence, Microsoft Corporation

**Veröffentlicht:** Januar 2015

**Version:** 1.0

**Herunterladen:** [Mit Microsoft Azure Stream Analytics Echtzeit-Ereignis](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)


## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)

 
