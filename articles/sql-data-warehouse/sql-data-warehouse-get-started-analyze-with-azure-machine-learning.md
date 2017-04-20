<properties
   pageTitle="Analysieren von Daten mit Azure Machine Learning | Microsoft Azure"
   description="Verwenden Sie Azure Machine Learning eine vorhersehbare Maschine Lernmodell auf Azure SQL Data Warehouse gespeicherten Daten basiert."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="kevinvngo"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/14/2016"
   ms.author="kevin;barbkess;sonyama"/>

# <a name="analyze-data-with-azure-machine-learning"></a>Analysieren von Daten mit Azure maschinelles lernen

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure maschinelles lernen](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [Sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Dieses Lernprogramms Azure Machine Learning eine vorhersehbare Maschine Lernmodell auf Azure SQL Data Warehouse gespeicherten Daten basiert. Insbesondere erstellt diese Vorhersagen, wenn ein Kunde wahrscheinlich ein Fahrrad oder eine zielgruppenorientierten Marketingkampagne für Adventure Works sortiertes Fachgeschäft vor Ort.

> [AZURE.VIDEO integrating-azure-machine-learning-with-azure-sql-data-warehouse]


## <a name="prerequisites"></a>Erforderliche Komponenten
Um dieses Lernprogramm durchgehen, müssen wie folgt vor:

- Ein SQL Data Warehouse vorinstalliert mit Beispieldaten AdventureWorksDW. Um diese Bestimmung finden Sie unter [Erstellen einer SQL Data Warehouse][] und Laden Sie die Beispieldaten. Wenn Sie bereits ein Datawarehouse haben jedoch keinen Beispieldaten können Sie [Beispieldaten manuell laden][].

## <a name="1-get-data"></a>1 Daten abrufen
Die Daten sind in der dbo.vTargetMail-Ansicht in der Datenbank AdventureWorksDW. Um diese Daten zu lesen:

1. Melden Sie [Azure Machine Learning Studio an][] und auf meinen Experimenten.
2. Klicken Sie auf **+ neu** und wählen Sie **Leere Experiment**.
3. Geben Sie einen Namen für den Test: Marketing ausgerichtet.
4. Ziehen Sie das **Reader** -Modul aus dem Module auf der Leinwand.
5. Geben Sie die Details Ihrer SQL Data Warehouse-Datenbank im Eigenschaftenbereich.
6. Geben Sie die **Abfrage** um die Daten zu lesen.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Ausführen des Experiments durch **klicken unter experimentcanvas** .
![Führen Sie das experiment][1]


Nach Beendigung des Experiments erfolgreich auf der Ausgangs-Port am unteren Rand das Reader-Modul und **Visualisierung** der importierten Daten wählen.
![Importierte Daten][3]


## <a name="2-clean-the-data"></a>2. Bereinigen der Daten
Löschen Sie zum Bereinigen der Daten einige Spalten für das Modell nicht relevant sind. Dazu:

1. Ziehen Sie das Modul **Projekt Spalten** auf der Leinwand.
2. Klicken Sie auf **Launch Spaltenauswahl** Eigenschaften, welche Spalten sollen zu löschen.
![Project-Spalten][4]

3. Zwei Spalten ausschließen: CustomerAlternateKey und GeographyKey.
![Entfernen Sie überflüssige Spalten][5]


## <a name="3-build-the-model"></a>3. Erstellen des Modells
Es teilt Daten 80-20: 80 % auf einen Computer lernen Modell und 20 % Modell testen Schulen. Wir machen Problems binäre Klassifizierung der "Zwei-Klassen" Algorithmen verwenden.

1. Ziehen Sie das Modul **Teilen** auf der Leinwand.
2. Geben Sie für Teil Zeilen im ersten ausgabedataset im Eigenschaftenbereich 0,8.
![Schulung und Satz Daten teilen][6]
3. Ziehen Sie das Modul **Zwei Klassen erhöht Entscheidungsstruktur** auf der Leinwand.
4. Ziehen Sie das Modul **Zug Modell** auf der Leinwand und geben Sie Eingaben an. Klicken Sie **Start Spaltenauswahl** im Eigenschaftenbereich.
      - Erste Eingabe: ML-Algorithmus.
      - Zweite Eingabe: Daten den Algorithmus zu schulen.
![Schließen Sie Train Model-Modul][7]
5. Wählen Sie die Spalte **BikeBuyer** der Spalte vorherzusagen.
![Wählen Sie Spalte vorherzusagen][8]


## <a name="4-score-the-model"></a>4. Bewerten des Modells
Jetzt werden wir testen, wie das Modell Daten durchführt. Vergleichen wir den Algorithmus unserer Wahl mit einen anderen Algorithmus zu sehen, die besser führt.

1. Ziehen Sie **Score Model** -Modul auf der Leinwand.
    Erste Eingabe: zweite Eingabe Modell trainiert: Testdaten ![Ergebnis des Modells][9]
2. Ziehen Sie **Zwei Klassen Bayes Maschine** in Canvas experimentieren. Wir vergleichen die Leistung dieser Algorithmus im Vergleich zu zwei Klassen erhöht Entscheidungsstruktur behandelt.
3. Kopieren Sie und fügen Sie die Module Zug und Bewertung Modell im Bereich.
4. Ziehen Sie das Modul **Modell evaluieren** auf der Leinwand zwei Algorithmen vergleichen.
5. **Ausführen** des Experiments.
![Führen Sie das experiment][10]
6. Das Ausgangs-Port am unteren Rand das Modul Modell evaluieren klicken Sie und visualisieren.
![Visualisieren der Ergebnisse][11]

Die Metriken sind die ROC Kurve Genauigkeit Rückruf Diagramm, und heben Sie Kurve. Diese Metriken betrachten, sehen wir, dass das erste Modell besser als die zweite durchgeführt. Das erste Modell vorhergesagt, sich der Ausgang des Modells Bewertung auf und dann auf visualisieren.
![Resultat visualisieren][12]

Sie sehen zwei weitere Spalten, die das testdataset hinzugefügt.

- Erzielte Wahrscheinlichkeit: die Wahrscheinlichkeit, dass ein Kunde ein fahrradkäufer.
- Erzielte Etiketten: die Klassifizierung dazu Modell – fahrradkäufer (1) oder nicht (0). Diese wahrscheinlichkeitsschwellenwert für die Kennzeichnung auf 50 % festgelegt und kann angepasst werden.

Vergleich der Spalte BikeBuyer (Istwert) mit bewertet (Vorhersage) können Sie sehen, wie das Modell ausgeführt wurde. Als Nächstes können Sie dieses Modell Vorhersagen für neue Kunden und dieses Modell als Webdienst veröffentlichen oder Ergebnisse zurück in SQL Data Warehouse schreiben.

## <a name="next-steps"></a>Nächste Schritte

Über vorbeugende Maschine lernen Modelle finden Sie unter Einführung [in Computer lernen Azure][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Einführung in Computer lernen Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Laden Sie Beispieldaten manuell]: sql-data-warehouse-load-sample-databases.md
[Erstellen Sie ein SQL Datawarehouse]: sql-data-warehouse-get-started-provision.md
