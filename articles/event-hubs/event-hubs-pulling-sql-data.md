<properties
   pageTitle="Ziehen von Daten SQL Azure Ereignis Hubs | Microsoft Azure"
   description="Übersicht der Veranstaltung Importieren von SQL-Beispiel"
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor=""/>

<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="spyros;sethm" />

# <a name="pulling-data-from-sql-into-an-azure-event-hub"></a>Ziehen von Daten aus SQL in Azure Event Hub

Eine normale Architektur für eine Anwendung für die Verarbeitung von Daten umfasst zunächst Azure Event Hub drücken. Ein Szenario IoT wie den Datenverkehr auf verschiedene einer Autobahn oder Überwachungsmaßnahmen Trümpfe wilden Teilnehmer Gaming-Szenario oder einem Unternehmensszenario wie Gebäude Belegung möglicherweise. In diesen Fällen die Daten sind im Allgemeinen externe Objekte Zeitreihen Daten, die Sie sammeln, analysieren, speichern und entsprechend und können einen erheblichen Aufwand in investiert Ausbau der Infrastruktur für diese Prozesse. Was tun Sie, wenn Sie Daten aus wie eine Datenbank als Quelle für Daten möchten und in Verbindung mit der anderen Daten verwenden? Betrachten Sie Azure Stream Analytics, Remote Data Explorer (RDX) oder einem anderen Tool zu analysieren und langsam veränderliche Daten aus Microsoft Dynamics AX oder benutzerdefinierte Facilities-Management-System verwendet werden soll? Zur Lösung dieses Problems haben wir geschrieben und Open-Source-eine kleine Beispiel, das Ändern und bereitstellen, die die Daten aus einer SQL-Tabelle und eine Azure Event Hub verwendet als Eingabe die analytischen Verwendungen drücken. Erkennen Sie, dass dies ist ein seltener Fall und das Gegenteil von was Sie normalerweise mit einem Ereignis-Hub tun. Aber wenn Sie Sie in der Situation Dies müssen Sie finden, finden den Code Sie in der Azure-Samples Gallery [hier](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-import-from-sql/).  

Beachten Sie, dass der Code in diesem Beispiel, ein Beispiel. Es ist **nicht** beabsichtigt, eine produktionsanwendung und nicht versucht wurden für den Einsatz in einer solchen Umgebung machen, streng BASTELN, Entwickler ausgerichtet wird. Sie müssen alle Arten von Sicherheit, Leistung, Funktionalität und Kosten vor End-to-End-Architektur.

## <a name="application-structure"></a>Anwendung

Die Anwendung ist in C# geschrieben und die Datei readme.md im Beispiel enthält alle Informationen müssen Sie erstellen, ändern und veröffentlichen Sie die Anwendung. Die folgenden Abschnitte enthalten eine umfassende Übersicht darüber, was die Anwendung macht.

Wir beginnen mit der Annahme, dass Sie Zugriff auf eine SQL Azure-Tabelle. Sie müssen auch zu Azure Event Hub eingerichtet haben, die Verbindung zum Zugriff erforderliche Zeichenfolge.

Beim Starten der SqlToEventHub Lösung liest eine Konfigurationsdatei (App.config), eine Reihe von Dingen, wie in der Datei readme.md beschrieben. Sind selbsterklärend, wie den Namen der Datentabelle, usw., und keine müssen die Erklärung hierzu. 

Nach die Anwendung die Konfigurationsdatei gelesen, geht es in eine Schleife Lesen der SQL-Tabelle Datensätze an den Hub Ereignis drücken und benutzerdefinierte Ruheintervall wartet, bevor dabei ganz erneut. Hierbei sind einige Punkte zu beachten:

1. Die Anwendung basiert auf Annahme, die dass die SQL-Tabelle wird von einem externen Prozess aktualisiert, und Sie möchten alle und nur die Updates an einen Hub-Ereignis.
2. Die SQL-Tabelle muss ein Feld, das eine eindeutige und zunehmende Zahl, z. B. eine Datensatz-Anzahl. Dies kann so einfach wie ein Feld namens "Id" oder alles, was als was die Datenbank aktualisiert erhöht Fügt Datensätze "Creation_time" oder "Sequence_number". Die Anwendung fest und speichert den Wert des Feldes in jeder Iteration. In jeder nachfolgenden Schleifendurchlauf fragt die Anwendung im Wesentlichen die Tabelle für alle Datensätze, in denen der Wert dieses Felds den Wert überschreitet, der letzten Schleifendurchlauf begeistert. Wir fordern diese letzte Wert "Versatz".
3. Die Anwendung erstellt eine Tabelle "TableOffsets", beginnt er, die Offsets gespeichert. Die Tabelle ist mit der Abfrage erstellt, die "CreateOffsetTableQuery" in der Konfigurationsdatei definiert. 
4. Es gibt mehrere Abfragen zum Arbeiten mit der Offset Tabelle als "OffsetQuery", "UpdateOffsetQuery" und "InsertOffsetQuery" in der Konfigurationsdatei definiert. Sie sollten diese nicht ändern.
5. Schließlich ist die Abfrage "Datenabfrage" in der Konfigurationsdatei definiert die Abfrage ausgeführt wird, die Datensätze aus der SQL-Tabelle ziehen. Derzeit ist Top 1000 Datensätzen in jedem Durchlauf der Schleife für Optimierungszwecke - Wenn z. B. 25.000 Datensätze in der Datenbank seit der letzten Abfrage hinzugefügt wurden dauern eine Weile zum Ausführen der Abfrage. Beschränkt die Abfrage auf 1.000 Datensätze jeweils, werden Abfragen schneller. Top 1000 einfache Auswahl legt nachfolgenden Batches 1.000 Datensätze an den Hub Ereignis.    

## <a name="next-steps"></a>Nächste Schritte

Zum Bereitstellen der Projektmappe Klonen oder SqlToEventHub-Anwendung herunterladen, bearbeiten Sie die Datei App.config erstellt und schließlich veröffentlichen. Nach der Anmeldung können im klassischen Azure-Portal unter Cloud-Dienste ausgeführt wird, und auf Ihrem Haupt-Ereignis überwachen. Beachten Sie, dass die Häufigkeit von zwei Dinge festgelegt werden: die Häufigkeit von Updates für die SQL-Tabelle und Ruheintervall haben Sie in der Konfigurationsdatei für die Anwendung angegeben.