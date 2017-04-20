<properties
   pageTitle="Verwenden von Azure Data Factory mit SQL Datawarehouse | Microsoft Azure"
   description="Tipps zur Verwendung von Azure Data Factory (ADF) mit Azure SQL Data Warehouse Lösungen."
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
   ms.date="08/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-azure-data-factory-with-sql-data-warehouse"></a>Verwenden von Azure Data Factory mit SQL Datawarehouse

Azure Data Factory bietet eine vollständig verwaltete Methode für die Übertragung von Daten und Ausführen von gespeicherten Prozeduren auf SQL Data Warehouse orchestriert.  Dadurch können Sie einfacher einrichten und Planen komplexe Transformation extrahieren und laden (ETL) Verfahren mit SQL Data Warehouse. Einen umfassenderen Überblick über Azure Data Factory finden Sie in der [Dokumentation zu Azure Data Factory][].

## <a name="data-movement"></a>Datentransfer

Azure können Daten Datentransfers zwischen lokalen Quellen und anderen Azure Services.  Allgemeine, aktuelle Integration von Azure Data Factory unterstützt Verschieben von Daten in und aus den folgenden Speicherorten:

+ Azure BLOB-Speicher
+ SQL Azure-Datenbank
+ Lokale SQL Server
+ SQL Server auf IaaS

Informationen zum Einrichten einer kopieraktivität finden Sie unter [Daten mit Azure Data Factory][]

## <a name="stored-procedures"></a>Gespeicherte Prozeduren
 In wie sie verwendet werden kann, um die Datenübertragung zu planen, werden Azure Data Factory auch zum Koordinieren der Ausführung von gespeicherten Prozeduren verwendet.  Dies können komplexere Pipelines erstellt werden und Azure Data Factory Fähigkeit rechnerische SQL Data Warehouse Nutzung erweitert.

## <a name="next-steps"></a>Nächste Schritte
Eine Übersicht über die Integration finden Sie unter [SQL Data Warehouse-Integration (Übersicht)][].
Weitere Hinweise zur Entwicklung finden Sie unter [SQL Data Warehouse Development Overview][].

<!--Image references-->

<!--Article references-->

[Daten Sie mit Azure Data Factory]: ../data-factory/data-factory-data-movement-activities.md
[SQL Data Warehouse-Anwendungsentwicklung, Überblick]: ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse-Integration (Übersicht)]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory-Dokumentation]:https://azure.microsoft.com/documentation/services/data-factory/

