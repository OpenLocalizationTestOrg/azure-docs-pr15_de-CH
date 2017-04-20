<properties
   pageTitle="Verwenden von Azure maschinelles Lernen mit SQL Datawarehouse | Microsoft Azure"
   description="Lernprogramm für mit Azure Machine Learning Azure SQL Data Warehouse Lösungen."
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

# <a name="use-azure-machine-learning-with-sql-data-warehouse"></a>Verwenden von Azure maschinelles Lernen mit SQL Datawarehouse

Azure Machine Learning ist eine vollständig verwaltete Vorhersageanalytik Service, den Vorhersagemodelle für Daten im SQL Data Warehouse erstellen und dann als Webdienste bereit verbrauchen veröffentlicht. Erfahren Sie die Grundlagen von Vorhersageanalysen und durch [Einführung in Computer lernen Azure][]lesen.  Sie können dann Informationen zum Erstellen, trainieren, bewerten und Testen einer Maschine Lernmodell [Erstellen Tutorial experimentieren][]mit.

In diesem Artikel erfahren Sie, wie im folgenden mit [Azure Machine Learning Studio][]:

- Lesen von Daten aus Ihrer Datenbank erstellen, Trainieren und Bewerten eines Vorhersagemodells
- Schreiben von Daten in der Datenbank


## <a name="read-data-from-sql-data-warehouse"></a>Lesen von Daten aus SQL Data Warehouse

Es liest Daten aus der Product-Tabelle in der Datenbank AdventureWorksDW.

### <a name="step-1"></a>Schritt 1

Starten Sie einen neuen Test durch Klicken auf + neu am unteren Fensterrand Machine Learning Studio wählen Sie EXPERIMENTIEREN und dann leere experimentieren. Wählen Sie Experiment Standardnamen am oberen Rand der Leinwand und benennen Sie ihn in einen aussagekräftigeren Namen, zum Beispiel Fahrrad preisvorhersage.

### <a name="step-2"></a>Schritt 2

Reader-Modul in der Palette von Datasets und Module auf der linken Seite der Leinwand Experiment suchen. Ziehen Sie das Modul auf die Leinwand experimentieren.
![][drag_reader]

### <a name="step-3"></a>Schritt 3

Wählen Sie das Reader-Modul und Eigenschaftenbereich ausfüllen.

1. Wählen Sie SQL Azure-Datenbank als Datenquelle.
2. Servername: Geben Sie den Servernamen ein. [Azure-Portal][] können Sie diesen finden.

![][server_name]

3. Datenbankname: Geben Sie den Namen einer Datenbank auf dem Server nur angegeben.
4. Benutzerkontoname Server: Geben Sie den Benutzernamen eines Kontos, das über Zugriffsberechtigungen für die Datenbank verfügt.
5. Server-Benutzerkennwort: Geben Sie das Kennwort für das angegebene Benutzerkonto.
6. Akzeptieren Sie alle Serverzertifikat: Diese Option (weniger sicher) Wenn Sie überspringen das Websitezertifikat überprüfen, bevor Sie Daten lesen möchten.
7. Abfrage: Geben Sie eine SQL-Anweisung, die die Daten beschreiben möchten. In diesem Fall werden wir Daten mithilfe der folgenden Abfrage Produkttabelle gelesen werden.


```SQL
SELECT ProductKey, EnglishProductName, StandardCost,
        ListPrice, Size, Weight, DaysToManufacture,
        Class, Style, Color
FROM dbo.DimProduct;
```

![][reader_properties]

### <a name="step-4"></a>Schritt 4

1. Führen Sie das Experiment unter Experiment Leinwand auf Ausführen klicken.
2. Nach Abschluss des Experiments haben das Reader-Modul ein grünes Häkchen an, dass es erfolgreich abgeschlossen wurde. Beachten Sie auch Status in der oberen rechten Ecke ausgeführt hat.

![][run]

3. Die importierten Daten auf der Ausgangs-Port am unteren Rand des Autos Datasets und wählen Sie visualisieren.


## <a name="create-train-and-score-a-model"></a>Erstellen Sie, Trainieren Sie und bewerten Sie ein Modell

Jetzt können Sie Dataset zu verwenden:

- Erstellen eines Modells: Daten und Funktionen definieren
- Trainieren Sie das Modell: auswählen und anwenden ein Learning-Algorithmus
- Bewerten und Testen des Modells: Vorhersagen neuer Fahrrad Preis


![][model]

Erfahren Sie mehr über das Erstellen, Trainieren Sie, Bewertung und Testen Sie einem Computer lernen Modell verwenden [Erstellen experimentieren Tutorial][].

## <a name="write-data-to-azure-sql-data-warehouse"></a>Schreiben von Daten in Azure SQL Data Warehouse

Wir schreiben das Resultset auf ProductPriceForecast-Tabelle in der Datenbank AdventureWorksDW.

### <a name="step-1"></a>Schritt 1

Das Writer-Modul in der Palette von Datasets und Module auf der linken Seite der Leinwand Experiment suchen. Ziehen Sie das Modul auf die Leinwand experimentieren.

![][drag_writer]

### <a name="step-2"></a>Schritt 2

Wählen Sie das Writer-Modul und Eigenschaftenbereich ausfüllen.

1. Wählen Sie als Datenziel Azure SQL-Datenbank.
2. Servername: Geben Sie den Servernamen ein. [Azure-Portal][] können Sie diesen finden.
3. Datenbankname: Geben Sie den Namen einer Datenbank auf dem Server nur angegeben.
4. Benutzerkontoname Server: Geben Sie den Benutzernamen eines Kontos, das über Schreibberechtigungen für die Datenbank verfügt.
5. Server-Benutzerkennwort: Geben Sie das Kennwort für das angegebene Benutzerkonto.
6. Übernehmen Sie alle Serverzertifikat (unsicher): Wählen Sie diese Option, wenn Sie das Zertifikat anzeigen möchten.
7. Durch Kommas getrennte Liste von Spalten gespeichert werden: eine Liste der Datasets oder Ergebnis Spalten, die Sie ausgeben möchten.
8. Name der Datentabelle: Geben Sie den Namen der Datentabelle.
9. Durch Kommas getrennte Liste von Datatable-Spalten: die Spaltennamen in der neuen Tabelle angeben. Listen Sie die gleiche Anzahl Spalten hier, die für die Ausgabetabelle definieren die Spaltennamen können anders aus dem Quell sein
10. Anzahl der Zeilen pro SQL Azure Vorgang: Konfigurieren Sie die Anzahl der Zeilen, die in einer SQL-Datenbank auf einmal geschrieben werden.

![][writer_properties]

### <a name="step-3"></a>Schritt 3

1. Führen Sie das Experiment unter Experiment Leinwand auf Ausführen klicken.
2. Wenn das Experiment abgeschlossen ist, haben alle Module ein grünes Häkchen an, dass sie erfolgreich.

## <a name="next-steps"></a>Nächste Schritte

Weitere Hinweise zur Entwicklung finden Sie unter [SQL Data Warehouse Development Overview][].

<!--Image references-->

[drag_reader]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-reader.png
[server_name]: ./media/sql-data-warehouse-integrate-azure-machine-learning/dw-server-name.png
[reader_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-reader-properties.png
[run]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-finished-running.png
[model]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-create-train-score-model.png
[drag_writer]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-drag-writer.png
[writer_properties]: ./media/sql-data-warehouse-integrate-azure-machine-learning/ml-writer-properties.png

<!--Article references-->

[SQL Data Warehouse-Anwendungsentwicklung, Überblick]: ./sql-data-warehouse-overview-develop.md
[Experiment Lernprogramm erstellen]: https://azure.microsoft.com/documentation/articles/machine-learning-create-experiment/
[Einführung in Computer lernen Azure]: https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Azure Machine Learning Studio]: https://studio.azureml.net/Home
[Azure-portal]: https://portal.azure.com/

<!--MSDN references-->

<!--Other Web references-->

[Azure Machine Learning documentation]: http://azure.microsoft.com/documentation/services/machine-learning/
