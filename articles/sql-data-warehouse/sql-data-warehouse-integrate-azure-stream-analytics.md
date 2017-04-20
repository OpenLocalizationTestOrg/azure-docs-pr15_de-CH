<properties
   pageTitle="Verwenden von Azure Stream Analytics mit SQL Datawarehouse | Microsoft Azure"
   description="Tipps zur Verwendung von Azure Stream Analytics Azure SQL Data Warehouse Lösungen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="use-azure-stream-analytics-with-sql-data-warehouse"></a>Verwenden von Azure Stream Analytics mit SQL Datawarehouse

Azure Stream Analytics ist ein vollständig verwalteter Dienst niedriger Latenz hoch verfügbare, skalierbare komplexe Verarbeitung über Daten in der Cloud bereitstellen. [Einführung in Azure Stream Analytics][]lesen können Sie die Grundlagen kennen. Sie erhalten dann eine End-to-End-Lösung durch den [Einsatz von Azure Stream Analytics][] Lernprogramm mit Stream Analytics erstellen.

In diesem Artikel erfahren Sie, wie Azure SQL Data Warehouse-Datenbank für Ihre Aufträge Dampf Analytics als eine Ereignissenke Ausgabe verwendet.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die folgenden Schritte im Lernprogramm [Erste Schritte mit Azure Stream Analytics][] zuerst ausgeführt.  

1. Erstellen Sie ein Ereignis Hub
2. Konfigurieren Sie und Ereignis-Generator Anwendung
3. Bereitstellung eines Stream Analytics-Auftrags
4. Job-Eingabe und Abfrage angeben

Erstellen einer Azure SQL Data Warehouse-Datenbank

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Auftrag Ausgabe: Azure SQL Data Warehouse-Datenbank

### <a name="step-1"></a>Schritt 1

Dem Stream Analytics-Auftrag auf **Ausgabe** vom oberen Rand der Seite und klicken Sie dann auf **Ausgabe hinzufügen**.

### <a name="step-2"></a>Schritt 2

Wählen Sie SQL-Datenbank, und klicken Sie auf Weiter.

![][add-output]

### <a name="step-3"></a>Schritt 3
Geben Sie auf der nächsten Seite die folgenden Werte:

- *Ausgabealias*: Geben Sie einen Anzeigenamen für diese Auftragsausgabe.
- *Abonnement*:
    - Abonnement ist SQL Data Warehouse-Datenbank im selben Abonnement als Stream Analytics-Auftrags wählen Sie SQL Datenbank aus.
    - Ein weiteres Abonnement ist die Datenbank in ein anderes Abonnement wählen Sie SQL Datenbank aus.
- *Datenbank*: Geben Sie den Namen einer Zieldatenbank.
- *Server-Name*: Geben Sie für die Datenbank nur angegeben. Azure-Verwaltungsportal können Sie dies.

![][server-name]

- *Benutzername*: Geben Sie den Benutzernamen eines Kontos, das über Schreibberechtigungen für die Datenbank verfügt.
- *Kennwort*: Geben Sie das Kennwort für das angegebene Benutzerkonto.
- *Tabelle*: Geben Sie den Namen der Zieltabelle in der Datenbank.

![][add-database]

### <a name="step-4"></a>Schritt 4

Klicken Sie Kontrollkästchen dieser Auftragsausgabe hinzufügen und überprüfen, ob der Stream Analytics eine Verbindung zur Datenbank herstellen können.

![][test-connection]

Wenn die Verbindung zur Datenbank erfolgreich ist, sehen Sie eine Benachrichtigung am unteren Rand des Portals. Sie können unten zum Testen der Verbindung mit der Datenbank Verbindung testen klicken.

## <a name="next-steps"></a>Nächste Schritte

Eine Übersicht über die Integration finden Sie unter [SQL Data Warehouse-Integration (Übersicht)][].

Weitere Hinweise zur Entwicklung finden Sie unter [SQL Data Warehouse Development Overview][].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Einführung in Azure Stream Analytics]: ../stream-analytics/stream-analytics-introduction.md
[Erste Schritte mit Azure Stream Analytics]: ../stream-analytics/stream-analytics-get-started.md
[SQL Data Warehouse-Anwendungsentwicklung, Überblick]:  ./sql-data-warehouse-overview-develop.md
[SQL Data Warehouse-Integration (Übersicht)]:  ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/
