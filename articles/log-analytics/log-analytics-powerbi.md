<properties
   pageTitle="Protokoll Analysedaten in Power BI exportieren | Microsoft Azure"
   description="Power BI ist eine cloudbasierte Business Analytics-Dienst von Microsoft, die umfassende Visualisierung und Berichte für verschiedene Datenmengen Analyse bereitstellt.  Protokollanalyse exportieren kontinuierlich Daten aus dem OMS-Repository in Power BI damit die Visualisierung und Analyse-Tools nutzen können.  Dieser Artikel beschreibt, wie Abfragen Protokollanalyse konfigurieren, die automatisch in regelmäßigen Abständen in Power BI zu exportieren."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="export-log-analytics-data-to-power-bi"></a>Power BI Protokollanalyse-Datenexport

[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) ist eine cloudbasierte Business Analytics-Dienst von Microsoft, die umfassende Visualisierung und Berichte für verschiedene Datenmengen Analyse bereitstellt.  Protokollanalyse exportieren automatisch Daten aus dem OMS-Repository in Power BI damit die Visualisierung und Analyse-Tools nutzen können.

Beim Konfigurieren von Power BI mit Protokollanalyse erstellen Sie Protokolldateien Abfragen, die entsprechenden Datensätze in Power BI ihre Ergebnisse exportieren.  Abfrage und Exportieren weiterhin automatisch nach einem Zeitplan ausgeführt, das Dataset mit den neuesten Daten Protokollanalyse Stand.

![Protokollanalyse Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI Zeitpläne

*Power BI Zeitplan* enthält eine protokollsuche, die exportiert Daten aus dem OMS-Repository zu einem entsprechenden Dataset in Power BI und einen Zeitplan, der definiert, wie oft diese Suche ausgeführt, um das Dataset auf dem aktuellen Stand zu halten.

Die Felder im Dataset werden Datensätze durchsucht Protokoll übereinstimmen.  Wenn verschiedene Datensätze zurückgegeben werden Dataset alle Eigenschaften aller Datensatztypen enthalten sind.  

> [AZURE.NOTE] In diesem Zusammenhang empfiehlt es sich, eine Suchabfrage Protokoll verwenden, die unformatierte Daten durchführen einer Konsolidierung Befehle wie [Measure](log-analytics-search-reference.md#measure)zurückgibt.  Aggregation und Berechnungen möglich in Power BI den unformatierten Daten.

## <a name="connecting-oms-workspace-to-power-bi"></a>Power BI OMS Arbeitsbereich herstellen

Bevor Sie Power BI aus Protokollanalyse exportieren können, müssen Sie die Power BI-Konto mithilfe des folgenden Verfahrens OMS Arbeitsbereich verbinden.  

1. Der OMS-Konsole klicken Sie **Einstellungen** .
2. **Konten**auswählen
3. Klicken Sie im Abschnitt **Informationen zum Arbeitsbereich** **mit Power BI-Konto verbinden**.
4. Geben Sie die Anmeldeinformationen für Ihr Konto Power BI.

## <a name="create-a-power-bi-schedule"></a>Power BI Zeitplan erstellen

Erstellen eines Zeitplans Power BI für jedes Dataset mithilfe des folgenden Verfahrens.

1. Der OMS-Konsole klicken Sie **Protokoll suchen** .
2. Geben Sie eine neue Abfrage oder wählen Sie eine gespeicherte Suche aus, die Daten zurückgibt, **Power BI**exportieren.  
3. Klicken Sie auf **Power BI** , am oberen Rand der Seite **Power BI** -Dialogfeld geöffnet.
4. Die Informationen in der folgenden Tabelle, und klicken Sie auf **Speichern**.

| Eigenschaft | Beschreibung |
|:--|:--|
| Name | Name, der beim Anzeigen der Liste der Power BI-Zeitpläne angezeigt. |
| Gespeicherte Suche | Die protokollsuche ausführen.  Wählen Sie die aktuelle Abfrage oder eine vorhandene gespeicherte Suche aus der Dropdownliste auswählen. |
| Zeitplan | Wie häufig führen Sie die gespeicherte Suche und Power BI-DataSet exportieren.  Der Wert muss zwischen 15 Minuten und 24 Stunden liegen. |
| DataSet-Name | Der Name des Datasets in Power BI.  Es existiert nicht erstellt und aktualisiert, wenn sie vorhanden ist. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Anzeigen und Entfernen von Power BI Zeitpläne

Zeigt eine Liste der vorhandenen Power BI Zeitpläne mit der folgenden Prozedur.

1. Der OMS-Konsole klicken Sie **Einstellungen** .
2. Wählen Sie **Power BI**.

Neben den Details des Zeitplans wie oft der Zeitplan in der letzten Woche ausgeführt hat und den Status der letzten Synchronisierung angezeigt.  Wenn die Synchronisierung Fehler auftreten, können Sie den Link, um ein Protokoll nach Datensätzen mit Details zum Ausführen klicken.

Auf das **X** in der **Spalte entfernen**, um einen Zeitplan zu entfernen.  Auswählen **aus**, um einen Zeitplan zu deaktivieren.  Sie ändern einen Zeitplan entfernen, und bei neu.

![Power BI Zeitpläne](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Beispiel Exemplarische Vorgehensweise
Im folgenden Abschnitt führt durch ein Beispiel Power BI Zeitplan erstellen und mit dem Dataset Erstellen eines einfachen Berichts.  In diesem Beispiel alle Leistungsdaten für eine Gruppe von Computern in Power BI exportiert und ein Liniendiagramm wird erstellt, um eine Auslastung angezeigt.

### <a name="create-log-search"></a>Protokollsuche erstellen
Zunächst wird ein Protokoll für die Daten, die wir wollen das Dataset erstellen.  In diesem Beispiel verwenden wir eine Abfrage, die alle Leistungsdaten für Computer mit dem Namen zurückgibt, die mit *Srv*.  

![Power BI Zeitpläne](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Power BI Suche erstellen
Wir klicken **Power BI** Power BI öffnen und die erforderlichen Informationen.  Wir möchten diese Suche einmal pro Stunde ausgeführt und erstellen ein Dataset namens *Contoso Perf*.  Da wir bereits die Suche öffnen, das die Daten erstellt wir, wir übernehmen Sie den Standardnamen *verwenden aktuelle Suchabfrage* für **Gespeicherte Suche**.

![Power BI-Suche](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Überprüfen Sie Power BI-Suche
Überprüfen, dass den Zeitplan korrekt erstellt werden die Liste der Power BI sucht unter **Einstellungen** Kachel OMS-Dashboard anzeigen.  Wir warten Sie einige Minuten und diese Ansicht aktualisieren, solange es meldet, dass die Synch.

![Power BI-Suche](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Überprüfen Sie das Dataset im Power BI
Unser Konto [powerbi.microsoft.com](http://powerbi.microsoft.com/) und Scroll für **Datensätze** am unteren Rand des linken Ausschnitts anmelden.  Wir sehen, dass *Contoso Perf* Dataset aufgeführt ist, dass der Export erfolgreich ausgeführt wurde.

![Power BI dataset](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Erstellen Sie Bericht dataset
Wir **Contoso Perf** Dataset auswählen und im Bereich **Felder** auf der rechten Seite die Felder anzuzeigen, die Teil dieses Dataset auf **Ergebnisse** klicken.  Erstellen Sie ein Liniendiagramm, prozessorauslastung für jeden Computer gehen wir.

1. Wählen Sie die Zeile diagrammvisualisierung.
2. **Objektname** melden **Filter Ebene** ziehen und **Prozessoren**überprüfen.
3. Ziehen Sie **CounterName** **Filter Ebene** gemeldet und überprüfen Sie **Prozessorzeit**.
4. Ziehen Sie **Gegenwert** **Werte**.
5. Ziehen Sie **Computer** zu **Legende**.
6. Ziehen Sie **TimeGenerated** **Achse**

Wir sehen, dass das resultierende Liniendiagramm mit den Daten aus unserem Dataset angezeigt wird.

![Power BI Liniendiagramm](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Speichern Sie den Bericht
Wir speichern Sie den Bericht, indem Sie auf die Schaltfläche am oberen Bildschirmrand und überprüfen, dass sie jetzt im linken Fensterbereich im Abschnitt Berichte aufgeführt.

![Power BI-Berichten](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Abfragen erstellen, die auf Power BI exportiert werden kann.
- Erfahren Sie mehr über [Power BI](http://powerbi.microsoft.com) Visualisierung Protokollanalyse Exporte Grundlage erstellen.
