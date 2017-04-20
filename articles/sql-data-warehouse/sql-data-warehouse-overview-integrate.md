<properties
   pageTitle="Integrierte Lösung mit SQL Data Warehouse erstellen | Microsoft Azure"
   description="Tools und Partnern Lösungen, die mit SQL Data Warehouse. "
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="leverage-other-services-with-sql-data-warehouse"></a>Andere Dienste mit SQL Data Warehouse nutzen
Neben seiner Kernfunktionen ermöglicht SQL Data Warehouse viele andere Dienste in Azure neben nutzen.  Insbesondere haben wir derzeit tief Integration mit folgenden Maßnahmen:

+ Power BI
+ Azure Data Factory
+ Azure maschinelles lernen
+ Azure Stream Analytics

Wir arbeiten mit mehr Azure Ökosystem verbinden.

##<a name="power-bi"></a>Power BI
Power BI Integration ermöglicht die Compute und Visualisierung von Power BI SQL Data Warehouse mit dynamischen Berichten nutzen. Power BI-Integration umfasst derzeit:

+ **Direkte Verbindung**: eine erweiterte Verbindung mit logischen Pushdown für SQL Data Warehouse.  Dies ermöglicht schnellere Analyse größer.
+ **In Power BI geöffnet**: ' Power BI öffnen ' übergibt Informationen an Power BI für eine nahtlose Verbindung.

Finden Sie unter [integrieren mit Power BI](./sql-data-warehouse-integrate-power-bi.md) oder [Power BI-Dokumentation](http://blogs.msdn.com/b/powerbi/archive/2015/06/24/exploring-azure-sql-data-warehouse-with-power-bi.aspx) Weitere Informationen.

##<a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory bietet Benutzern eine verwaltete Plattform komplexe extrahieren laden Pipelines erstellen.  Data Warehouse SQL Azure Data Factory Integration umfasst Folgendes:

+ **Gespeicherte Prozeduren**: koordinieren die Ausführung von gespeicherten Prozeduren auf SQL Data Warehouse.
+ **Kopieren**: mit ADF zum Verschieben von Daten in SQL Data Warehouse.  Diesen Vorgang können ADF Standarddaten bewegungsmechanismus oder PolyBase im Hintergrund. 

[Integrieren in Azure Data Factory](./sql-data-warehouse-integrate-azure-data-factory.md) oder [Azure Data Factory Dokumentation](https://azure.microsoft.com/documentation/services/data-factory/) Weitere Informationen anzeigen

##<a name="azure-machine-learning"></a>Azure maschinelles lernen
Azure Machine Learning ist eine vollständig verwaltete Analytics Benutzer komplexe Modelle nutzt eine Reihe von vorhersehbaren Tools erstellen.  SQL Data Warehouse ist eine Quelle und das Ziel für diese Modelle die folgenden Funktionen unterstützt:

+ **Daten:** Skalieren mit T-SQL für SQL Data Warehouse fahren Sie Modelle.
+ **Daten:** Änderungen von keinem Modell in SQL Data Warehouse.

[Integrieren in Azure Machine Learning](./sql-data-warehouse-integrate-azure-machine-learning.md) oder [Azure Machine Learning Dokumentation](https://azure.microsoft.com/services/machine-learning/) Weitere Informationen anzeigen

##<a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics ist eine komplexe, vollständig verwaltete Infrastruktur für die Verarbeitung und Nutzung von Azure Event Hub erzeugten Ereignisdaten.  Integration mit SQL Data Warehouse kann für streaming-Daten effizient verarbeitet und mit relationalen Daten tiefer, erweiterte Analyse gespeichert werden.  

+ **Arbeit Ausgabe:** Senden Sie Ausgabe von Stream Analytics-Aufträge direkt an SQL Data Warehouse.

[Integrieren in Azure Stream Analytics](./sql-data-warehouse-integrate-azure-stream-analytics.md) oder [Azure Stream Analytics Dokumentation](https://azure.microsoft.com/documentation/services/stream-analytics/) Weitere Informationen anzeigen

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop/

[Azure Data Factory]: sql-data-warehouse-integrate-azure-data-factory.md
[Azure Machine Learning]: sql-data-warehouse-integrate-azure-machine-learning.md
[Azure Stream Analytics]: sql-data-warehouse-integrate-azure-stream-analytics.md
[Power BI]: sql-data-warehouse-integrate-power-bi.md
[Partners]: sql-data-warehouse-partner-business-intelligence.md

<!--MSDN references-->

<!--Other Web references-->
