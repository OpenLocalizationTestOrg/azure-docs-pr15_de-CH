<properties 
   pageTitle="Verwenden Sie den Scheitelpunkt Ausführung auf See Datentools für Visual Studio | Microsoft Azure" 
   description="Informationen Sie zum Eckpunkt Ausführung Ansicht Prüfung See Datenanalyse Projekten verwenden." 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/13/2016"
   ms.author="jgao"/>

# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Verwenden Sie den Scheitelpunkt Ausführung auf See Datentools für Visual Studio

Informationen Sie zum Eckpunkt Ausführung Ansicht Prüfung See Datenanalyse Projekten verwenden.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Grundkenntnisse Data Lake-Tools für Visual Studio U-SQL-Skript zu verwenden.  Siehe [Tutorial: Entwickeln U-SQL-Skripts mit Daten See Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Öffnen Sie Scheitelpunkt Ausführung

Für ein bestimmtes Projekt können Sie den Link "Scheitelpunkt Ausführung anzeigen" in der unteren linken Ecke klicken. Sie können Profile laden aufgefordert und dauert einige Zeit abhängig von der Netzwerkkonnektivität.

![Datenanalyse See Werkzeuge Scheitelpunkt Ausführung anzeigen](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Scheitelpunkt Ausführung verstehen

Nach der Eingabe der Scheitelpunkt Ausführung anzeigen, sind drei Teile:

![Datenanalyse See Werkzeuge Scheitelpunkt Ausführung anzeigen](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

- Vertex-Auswahl: links wird der Scheitelpunkt Selektor.  Sie können die Scheitelpunkte von Features auswählen (z. B. Top 10 Daten lesen oder Stufe auswählen)

    Die am häufigsten verwendeten Filter gehört die Scheitelpunkte kritischen Weg. Kritisch ist der längste Pfad U-SQL-Auftrag. Es ist nützlich für die Optimierung Ihrer Aufträge durch Überprüfen der Scheitelpunkt am längsten dauert.

- Oberen mittleren Bereich:

    ![Datenanalyse See Werkzeuge Scheitelpunkt Ausführung anzeigen](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

    Diese Ansicht zeigt auch den Ausführungsstatus der Scheitelpunkte. Konvertiert die Zeit entsprechend lokalen und anderen Status in unterschiedlichen Farben dargestellt.

- Mitte unten:

    ![Datenanalyse See Werkzeuge Scheitelpunkt Ausführung anzeigen](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

    - Prozessname: Der Name der Vertex-Instanz. Besteht aus unterschiedlichen Bauteilen StageName | VertexName | VertexRunInstance. Scheitelpunkt [62] .v1 SV7_Split steht z. B. für die zweite ausgeführte Instanz (.v1, Index beginnt mit 0) Scheitelpunkt Zahl 62 Phase SV7_Split.
    - Insgesamt Daten schreibgeschützt: Die Daten wurden durch diesen Vertex Lese-/Schreibzugriff.
    - Status/Beendigungsstatus: Den endgültigen Status Beendigung der Scheitelpunkt.
    - : Exit-Code-Fehler der Fehler bei der Scheitelpunkt.
    - Erstellung Grund: Warum wurde der Scheitelpunkt erstellt.
    - Ressource Wartezeit-Prozess Wartezeit-PN Warteschlange Latenz: die Zeit für den Scheitelpunkt auf Ressourcen warten Daten verarbeiten und in der Warteschlange verbleiben.
    - Prozess-Ersteller-GUID: GUID für den aktuellen laufenden Vertex oder seinen Ersteller.
    - Version: die N-te Instanz ausgeführten Scheitelpunkt (System möglicherweise neue Instanzen eines Scheitelpunkts planen, für viele Gründe, z. B. Failover Redundanz usw. zu berechnen.)
    - Version erstellt.
    - Prozess erstellen Start Time-Prozess in Warteschlange Zeit-Prozess Start Time-Prozess abgeschlossen Zeit: Beginn des Vorgangs Scheitelpunkt erstellen; Wann beginnt der Scheitelpunkt Warteschlange; Wenn bestimmte Vertex-Prozess beginnt. Wenn bestimmte Scheitelpunkt abgeschlossen ist.

## <a name="next-steps"></a>Nächste Schritte

- Um einen Überblick der Datenanalyse See Übersicht [Azure Data Lake Analytics](data-lake-analytics-overview.md).
- Finden Sie zunächst Anwendungsentwicklung U-SQL [entwickeln U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL finden Sie unter [Einstieg in Azure Data Lake Analytics U-SQL-Sprache](data-lake-analytics-u-sql-get-started.md).
- Management-Aufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics verwenden Azure-Portal](data-lake-analytics-manage-use-portal.md).
- Diagnose-Informationen protokollieren finden Sie unter [Accessing Diagnoseprotokolle für Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
- Eine komplexe Abfrage finden Sie unter [Analysieren Website Protokolle mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Job-Details finden Sie unter [Projekt-Browser verwenden und Auftrag für Azure Data Lake Analytics Aufträge](data-lake-analytics-data-lake-tools-view-jobs.md)