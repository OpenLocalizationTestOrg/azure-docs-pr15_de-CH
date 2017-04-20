<properties
   pageTitle="Verteilte Daten und Tabellenoptionen für SQL Data Warehouse und Parallel Data Warehouse Systeme massiv parallele Verarbeitung (MPP) | Microsoft Azure"
   description="Erfahren Sie, wie Daten massiv parallele Verarbeitung (MPP) und Optionen für Tabellen in Azure SQL Data Warehouse und Parallel Data Warehouse verteilen verteilt werden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="barbkess"/>


# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Verteilte Daten und verteilte Tabellen für massiv parallele Verarbeitung (MPP)

Erfahren Sie, wie Daten in Azure SQL Data Warehouse und Parallel Data Warehouse sind Microsoft massiv parallele Verarbeitung (MPP) Systeme verteilt. Entwerfen das Datawarehouse verteilte Daten effektiv verwenden können Sie Vorteile der MPP-Architektur Verarbeitung der Abfrage zu. Datenbank Design Wahl haben einen erheblichen Einfluss auf die Leistung verbessert.  

>[AZURE.NOTE] Azure SQL Data Warehouse und Parallel Data Warehouse verwenden das gleichen Design massiv parallele Verarbeitung (MPP) haben einige Unterschiede aufgrund der zugrunde liegenden Plattform. SQL Data Warehouse ist eine Plattform als Service (PaaS), die in Azure ausgeführt wird. Parallel Data Warehouse wird auf Analytics Plattform System (APS) ist eine lokale Anwendung, die Windows Server ausgeführt wird.

## <a name="what-is-distributed-data"></a>Was ist verteilte Daten?

Bezieht sich im SQL Data Warehouse und Parallel Data Warehouse verteilte Daten auf Benutzerdaten, die an mehreren Standorten MPP-System gespeichert sind. Alle Lagerplätze fungiert als unabhängige Speicher und Verarbeitungseinheit, die Abfragen auf dem Teil der Daten. Verteilter ist Abfragen parallel zu hohe Leistung ausgeführt.

Datawarehouse ordnet jede Zeile einer Benutzertabelle zum Verteilen von Daten zentral verteilte zu.  Sie können Tabellen mit Hash Verteilungsmethode oder einer Round-Robin-Methode verteilen. Die Verteilungsmethode wird in der CREATE TABLE-Anweisung angegeben. 

## <a name="hash-distributed-tables"></a>Distributed Hash Tabellen
  
Das folgende Diagramm veranschaulicht, wie eine vollständige (nicht verteilten Tabelle) als distributed Hash Tabelle gespeichert wird. Eine deterministische Funktion ordnet jede Zeile einer Verteilerliste gehören. In der Tabellendefinition ist eine der Spalten als verteilungsspalte festgelegt. Die Hash-Funktion verwendet den Wert in der verteilungsspalte eine jede Zeile zuweisen.

Gibt für die Auswahl einer Spalte Verteilung Leistungsaspekte wie Unterscheidbarkeit, Daten Neigung und die Arten von Abfragen auf dem System ausgeführt.
  
![Verteilte Tabelle] (media/sql-data-warehouse-distributed-data/hash-distributed-table.png "Verteilte Tabelle")  
  
-   Jede Zeile gehört zu einem.  
  
-   Deterministische Hashalgorithmus ordnet jede Zeile einer Verteilung.  
  
-   Die Anzahl der Zeilen pro Verteilung hängt wie Größen Tabellen dargestellt.

## <a name="round-robin-distributed-tables"></a>Verteilte Roundrobin-Tabellen

Eine verteilte Roundrobin-Tabelle verteilt die Zeilen eine sequenziell jede Zeile zuweisen. Schnell zum Laden von Daten in eine Tabelle Round Robin, aber Leistung möglicherweise langsamer.  Verknüpfen einer Round-Robin-Tabelle in der Regel erfordert Neuordnung Zeilen um die Abfrage zu Zeit ein richtiges Ergebnis zu aktivieren.

## <a name="distributed-storage-locations-are-called-distributions"></a>Verteilte Speicherorte heißen Distributionen

Jeder verteilte Standort wird eine Verteilung aufgerufen. Wenn eine Abfrage parallel ausgeführt wird, führt jede Verteilung eine SQL-Abfrage auf die Daten. SQL Data Warehouse verwendet SQL-Datenbank, um die Abfrage auszuführen. Parallel Data Warehouse verwendet SQL Server zum Ausführen der Abfrage. Shared-Nothing-Architektur ist für dezentrales Skalieren parallel computing erreichen.

### <a name="can-i-view-the-distributions"></a>Kann die Distributionen anzeigen?

Jede Verteilung hat eine Verteilung ID und sichtbar in Systemansichten SQL Data Warehouse und Parallel Data Warehouse beziehen. Distributions-ID können Sie die Leistung und andere Probleme beheben. Finden Sie eine Liste der Systemansichten [MPP-Systemansicht](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>Unterschied zwischen einer Verteilung und Compute-Knoten

Eine Verteilung ist die grundlegende Einheit für verteilte Speicherung und Verarbeitung parallele Abfragen. Compute-Knoten werden Distributionen gruppiert. Jeder Compute-Knoten verfolgt mindestens Distributionen.  

-   Analytics Plattformsystem verwendet Compute-Knoten als zentrale Komponente der Hardware-Architektur und Dezentrales Skalieren. Er verwendet immer acht Distributionen jeden Compute-Knoten im Diagramm für verteilte Hash Tabellen dargestellt. Die Anzahl von Compute-Knoten und damit die Anzahl der Verteilung die Anzahl von Compute-Knoten bestimmt das Gerät erworben. Beim Kauf von acht Computeknoten erhalten Sie 64 Distributionen (8 Compute X 8 Verteilung/Knoten). 

-   SQL Data Warehouse verfügt über eine feste Anzahl von 60 Distributionen und flexiblen Anzahl von Compute-Knoten. Die Datenverarbeitungsknoten werden mit Azure Datenverarbeitung und Speicher-Ressourcen implementiert. Die Anzahl von Compute-Knoten können Back-End-Dienst Arbeitslast und die Datenverarbeitung (DWUs) für das Datawarehouse angeben. Ändert die Anzahl der Compute-Knoten ändert sich auch die Anzahl der pro Compute-Knoten. 

### <a name="can-i-view-the-compute-nodes"></a>Kann die Compute-Knoten anzeigen?

Jeder Compute-Knoten hat eine Knoten-ID und wird im SQL Data Warehouse und Parallel Data Warehouse beziehen Systemansichten angezeigt.  Sie sehen den Compute-Knoten anhand der Spalte knoten_id Ansichten mit sys.pdw_nodes. Finden Sie eine Liste der Systemansichten [MPP-Systemansicht](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Replizierte Tabellen für Parallel Datawarehouse 
  
Betrifft: Parallel Datawarehouse

Parallel Data Warehouse bietet neben verteilte Tabellen, eine Option Tabellen repliziert. Eine *replizierte Tabelle* ist eine Tabelle, die vollständig auf jeder Compute-Knoten gespeichert. Replizieren einer Tabelle wird die Zeilen zwischen den Compute-Knoten übertragen, bevor die Tabelle in einer Verknüpfung oder Aggregation. Replizierte Tabellen sind nur mit kleinen Tabellen aufgrund der zusätzlichen erforderlichen vollständige Tabelle auf jeder Compute-Knoten zu speichern.  
  
Das folgende Diagramm zeigt eine replizierte Tabelle auf jeder Compute-Knoten gespeichert. Die replizierte Tabelle werden über alle Festplatten den Compute-Knoten zugewiesen. Diese Strategie Datenträger erfolgt mit Hilfe von SQL Server-Dateigruppen.  
  
![Replizierte Tabelle] (media/sql-data-warehouse-distributed-data/replicated-table.png "Replizierte Tabelle") 
  
## <a name="next-steps"></a>Nächste Schritte
  
Verteilte Tabellen effektiv finden Sie unter [Verteilen von Tabellen im SQL Data Warehouse](sql-data-warehouse-tables-distribute.md)  
  



  
