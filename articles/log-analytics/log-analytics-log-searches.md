<properties
    pageTitle="Protokollanalyse sucht anmelden | Microsoft Azure"
    description="Protokolldatei suchen können und Computerdaten aus mehreren Quellen in Ihrer Umgebung entsprechen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-searches-in-log-analytics"></a>Log durchsucht Protokollanalyse

Kern Protokollanalyse ist die Protokoll-Suchfunktion ermöglicht kombinieren und Computerdaten aus mehreren Quellen in Ihrer Umgebung entsprechen. Solutions sind auch angetrieben Protokoll suchen bringen Sie Metriken Umgebung Problems gedreht.

Auf der Seite suchen können Sie eine Abfrage erstellen und dann beim Suchen, Sie können die Ergebnisse filtern Facet Steuerelemente mit. Erweiterte Abfragen transformieren, Filter und Berichte erstellen Sie Ihrer Ergebnisse.

Allgemeine Protokoll Suchabfragen auf den meisten Seiten der Projektmappe angezeigt. In der Konsole OMS klicken Sie Kacheln oder anderen Elementen Details über das Element anzeigen Protokoll suchen Bohren.

In diesem Lernprogramm gehen wir Beispiele Protokoll Suche alle Grundlagen behandelt.

Wir beginnen mit einfachen und praktische Beispiele und dann darauf aufbauen, so dass Sie Kenntnisse in praktischen Anwendungsfälle wie die Syntax verwenden, um die gewünschten Erkenntnisse aus den Daten extrahieren.

Nachdem Sie Techniken vertraut haben, können Sie der [Protokollanalyse anmelden Suchverweis](log-analytics-search-reference.md)überprüfen.

## <a name="use-basic-filters"></a>Einfache Filter

Erstes bekannt ist, dass der erste Teil einer Suche Abfragen vor "|" vertikale Zeichen ist immer ein *Filter*. Sie können davon als WHERE-Klausel TSQL - *Teilmenge der Daten aus dem Datenspeicher OMS* bestimmt. Suche im Datenspeicher ist weitgehend zur Angabe der Merkmale der Daten die zu extrahieren, so ist es natürlich mit der WHERE-Klausel eine Abfrage starten würden.

Grundlegende Filter verwendeten sind *Schlüsselwörter*wie 'Fehler' oder 'Timeout' oder einen Computernamen ein. Diese Typen von Abfragen im Allgemeinen die verschiedene Formen innerhalb desselben Resultsets zurück Deswegen Protokollanalyse verschiedene *Typen* enthält.


### <a name="to-conduct-a-simple-search"></a>Eine einfache Suche durchführen
1. Klicken Sie im Portal OMS auf **Protokoll suchen**.  
    ![Suche](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. Geben Sie in das Abfragefeld `error` und dann auf **Suchen**.  
    ![Fehler beim Suchen](./media/log-analytics-log-searches/oms-search-error.png)  
    Die Abfrage für `error` in der folgenden Abbildung zurückgegebenen **100.000 Datensätze (erfassten Protokollmanagement)** , 18 **ConfigurationAlert** Datensätze (generiert durch Konfiguration Bewertung) und 12 **ConfigurationChange** Datensätze (durch das Änderungsprotokoll aufgezeichnet).   
    ![Suchergebnisse](./media/log-analytics-log-searches/oms-search-results01.png)  

Diese Filter sind nicht wirklich Objekt Basistypklassen. *Typ* ist nur ein Tag oder eine Eigenschaft oder eine Zeichenfolge/Name/Kategorie, die Daten zugeordnet ist. Einige Dokumente im System werden als **Typ: ConfigurationAlert** und einige **Typ: Perf**oder **Typ: Ereignis**markiert und so weiter. Jedes Suchergebnis, Dokument, Datensatz oder Eintrag zeigt die Basiseigenschaften und deren Werte für jede dieser Daten und können Sie die Feldnamen im Filter angeben, um nur die Datensätze abzurufen, hat das Feld, der Wert.

*Ist wirklich nur ein Feld, das alle Datensätze* ist es von einem anderen Feld. Dies wurde anhand des Werts des Felds Art. Dieser Datensatz wird eine unterschiedliche Form haben. Übrigens **Typ Perf =**, oder **Type = Ereignis** ist auch die Syntax, die Sie lernen, Leistungsdaten oder Ereignissen abgefragt müssen.

Ein Doppelpunkt (:) oder ein Gleichheitszeichen (=) können hinter dem Feldnamen und der Wert. **Ereignistyp:** und **Type = Ereignis** wird dieselbe Bedeutung, Sie können die gewünschte Formatvorlage.

Wenn also der Typ = Perf Datensätze enthalten ein Feld namens "CounterName" und schreiben eine Abfrage wie `Type=Perf CounterName="% Processor Time"`.

Diese geben Performance-Daten Ihnen ist der Name des Leistungsindikators "Prozessorzeit (%)".

### <a name="to-search-for-processor-time-performance-data"></a>Zeit Prozessorleistungsdaten suchen
- Geben Sie im Feld Abfrage suchen`Type=Perf CounterName="% Processor Time"`

Sie können auch spezifischere und **InstanceName = _ 'Summe'** ist die Abfrage, die eine Windows-Leistungsindikator. Sie können auch eine Facette und ein anderes **Feld-Wert**auswählen. Filter wird automatisch der Filter in der Abfrage angezeigt. Dies ist in der folgenden Abbildung sehen. Beschreibt, wo Sie klicken hinzufügen **InstanceName: _Total"** der Abfrage ohne etwas einzugeben.

![Suche facet](./media/log-analytics-log-searches/oms-search-facet.png)

Die Abfrage wird jetzt`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

In diesem Beispiel müssen Sie nicht angeben **Type = Perf** dieses Ergebnis zu. Da die Felder CounterName und InstanceName nur für Datensätze vom Typ vorhanden = Leistung, die Abfrage ist ausreichend, den gleichen Ergebnissen wie länger, vorherige:
```
CounterName="% Processor Time" InstanceName="_Total"
```

Ist als miteinander *und* alle Filter in der Abfrage ausgewertet werden. Weitere Felder hinzufügen Sie Kriterien erhalten, bestimmte und verfeinert Ergebnisse.

Die Abfrage `Type=Event EventLog="Windows PowerShell"` entspricht `Type=Event AND EventLog="Windows PowerShell"`. Alle Ereignisse, die in und aus dem Windows PowerShell-Ereignisprotokoll erfasst wurden zurückgegeben. Wenn Sie Filter mehrmals hinzufügen, indem wiederholt dieselbe Facette wird das Problem rein kosmetischen - noch gibt dieselben Ergebnisse zurück, da implizite und Operator immer gibt es möglicherweise die Suchleiste durcheinander

Einen Operator nicht explizit verwenden können Sie einfach den impliziten Operator und umkehren. Zum Beispiel:

`Type:Event NOT(EventLog:"Windows PowerShell")`oder einem entsprechenden `Type=Event EventLog!="Windows PowerShell"` alle Ereignisse von anderen Protokollen, die nicht das Windows PowerShell-Protokoll zurück.

Oder Sie können booleschen Operators wie 'Oder'. Die folgende Abfrage gibt Datensätze für die im Ereignisprotokoll eine Anwendung oder ein System ist.

```
EventLog=Application OR EventLog=System
```

Die obige Abfrage erhalten Einträge für beide Protokolle in das gleiche Ergebnis Sie.

Jedoch entfernen oder den impliziten verlassen und Platz wird dann die folgende Abfrage keine Ergebnisse da kein Ereignisprotokolleintrag beide Protokolle gehört. Jeder Eintrag im Ereignisprotokoll wurden in eines der beiden Protokolle geschrieben.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Zusätzliche Filter

Die folgende Abfrage gibt Posten 2 Ereignisprotokolle für alle Computer, die Daten gesendet wurden.

```
EventLog=Application OR EventLog=System
```

![Suchergebnisse](./media/log-analytics-log-searches/oms-search-results03.png)

Wählen eines der Felder Filter wird die Abfrage an einen bestimmten Computer, ausgenommen alle anderen, einschränken. Die Abfrage würde wie folgt aussehen.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Die folgenden, durch die implizite und entspricht

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Jede Abfrage wird in die explizite Reihenfolge ausgewertet. Beachten Sie die Klammer.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Wie das Feld Ereignisprotokoll können Abrufen von Daten für mehrere Computer hinzufügen oder. Zum Beispiel:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Auch dadurch die folgende Abfrage zurückgegeben **% CPU-Zeit** für den ausgewählten Computern.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```


### <a name="boolean-operators"></a>Boolesche Operatoren
Datetime und numerische Felder können Sie suchen Werte *größer als* *weniger als*, und *weniger oder gleich*. Können Sie einfache Operatoren wie >, <>, =, < =,! = in der Suchleiste Abfrage.


Sie können ein bestimmtes Ereignisprotokoll für einen bestimmten Zeitraum Abfragen. Beispielsweise wird durch folgenden mnemonische 24 Stunden ausgedrückt.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Mit booleschen operator
- Geben Sie im Feld Abfrage suchen`EventLog=System TimeGenerated>NOW-24HOURS"`  
    ![mit booleschen Suche](./media/log-analytics-log-searches/oms-search-boolean.png)

Zwar Sie das Zeitintervall grafisch steuern können und meistens dazu möchten sind Vorteile auch einen Zeitfilter direkt in die Abfrage. Beispielsweise funktioniert hervorragend mit Dashboards, die Zeit für jede Kachel unabhängig von *globalen* Zeit Auswahl auf der Dashboardseite überschrieben werden kann. Weitere Informationen finden Sie unter [Zeit Angelegenheiten im Dashboard](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Beim Filtern nach Zeit dabei, dass Sie Ergebnisse der *Schnittpunkt* der beiden Zeiträume: die OMS-Portal (S1) angegeben und in der Abfrage (S2) angegeben.

![Schnittmenge](./media/log-analytics-log-searches/oms-search-intersection.png)

Dies bedeutet, wenn die Zeiträume, z.B. nicht in OMS-Portal schneiden, in dem Sie **Diese Woche** und in der Abfrage Sie **letzte Woche definieren**gibt keine Schnittmenge und erhalten Sie keine Ergebnisse.

Vergleichsoperatoren verwendet für das Feld TimeGenerated sind auch in anderen Situationen hilfreich. Beispiel mit numerischen Feldern.

Betrachten Sie Konfiguration Bewertung Warnungen haben folgende Schweregrad:

- 0 = Informationen
- 1 = Warnung
- 2 = kritisch

Sie können für Warnung und kritische Alarme und auch ausschließen, Information, mit der folgenden Abfrage:

```
Type=ConfigurationAlert  Severity>=1
```


Sie können auch Bereichsabfragen. Dies bedeutet, dass den Anfang und das Ende Wertebereich in einer Sequenz bereitstellen kann. Beispielsweise ggf. Ereignisse aus dem Ereignisprotokoll Operations Manager EventID größer oder gleich 2100 aber nicht größer als 2199 ist würde dann die folgende Abfrage sie zurückgeben.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


>[AZURE.NOTE] Sie verwenden Range-Syntax ist Trennzeichen Feldwert: Doppelpunkt (:) und *nicht* das Gleichheitszeichen (=). Das oberen und unteren Ende des Bereichs in Klammern einschließen und mit zwei Punkten (.) getrennt.

## <a name="manipulate-search-results"></a>Bearbeiten von Suchergebnissen

Wenn Sie Daten suchen, sollten Sie Suchabfragen optimieren und eine gute Kontrolle über die Ergebnisse. Beim Abrufen von Ergebnissen können Sie Befehle zum Transformieren anwenden.

Befehle in Protokollanalyse suchen *müssen* folgen einem senkrechten Strich (|). Ein Filter muss immer der erste Teil einer Abfragezeichenfolge. Definiert die Daten, mit dem Sie arbeiten, und dann "leitet" Diese Ergebnisse in einem Befehl. Die Pipe können Sie zusätzliche Befehle hinzufügen. Dies ist der Windows PowerShell-Pipeline lose vergleichbar.

Im Allgemeinen versucht Suchsprache Protokollanalyse PowerShell Stil und Richtlinien zu ähnlich den IT-Experten und Lernen erleichtern.

Befehle haben Namen Verben, sodass Sie leicht erkennen können, was sie tun.  

### <a name="sort"></a>Sortieren

Der Sortierbefehl können Sie die Sortierreihenfolge durch ein oder mehrere Felder definieren. Auch wenn Sie diese Standardeinstellung verwenden, wird eine Zeit absteigender Reihenfolge erzwungen. Die aktuellsten Ergebnisse sind immer am Anfang der Suchergebnisse. Dies bedeutet, dass beim Ausführen einer Suche mit `Type=Event EventID=1234` ist was für Sie wirklich ausgeführt wird:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Deshalb ist die Art der Erfahrung in Protokollen vertraut sind. Angenommen, in der Windows-Ereignisanzeige.

Sortieren können, verändert sich die Ergebnisse zurückgegeben werden. Die folgenden Beispiele zeigen Funktionsweise.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Die oben genannten Beispiele zeigen die Funktionsweise von Befehlen – ändern Sie die Form der Filter zurückgegebenen Ergebnisse.

### <a name="limit-and-top"></a>Grenzwert und oben
Eine andere weniger bekannter Befehl ist begrenzt. Limit ist ein PowerShell wie Verb. Grenzwert entspricht den TOP-Befehl. Die folgenden Abfragen werden die gleichen Ergebnisse zurückgegeben.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Mit oben
- Geben Sie im Feld Abfrage suchen`Type=Event EventID=600 | Top 1`   
    ![Suche oben](./media/log-analytics-log-searches/oms-search-top.png)

Im oben gezeigten Bild sind 358 Tausend Datensätze mit EventID = 600. Felder, Facetten und Filter links zeigen immer die Ergebnisse *vom Filter Teil* der Abfrage ausgegeben, der Teil vor dem Pipe-Zeichen. Im **Ergebnisbereich** gibt nur das neueste 1 Ergebnis, da der Beispielbefehl geformt und die Ergebnisse transformiert.

### <a name="select"></a>Wählen Sie

SELECT-Befehl verhält sich wie Select-Object in PowerShell. Gefilterte Ergebnisse, die nicht alle ihre ursprünglichen Eigenschaften zurückgegeben. Stattdessen wählt nur die Eigenschaften, die Sie angeben.

#### <a name="to-run-a-search-using-the-select-command"></a>Eine Suche mit dem select-Befehl

1. Geben Sie in suchen, `Type=Event` und dann auf **Suchen**.
2. Klicken Sie auf **+ mehr** Ergebnisse alle Eigenschaften anzeigen, die Ergebnisse enthalten.
3. Einige explizit wählen und die Abfrage ändern `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Suchen](./media/log-analytics-log-searches/oms-search-select.png)

Befehl besonders ist Wenn Suchergebnisse steuern und wählen nur die Teile der Daten, die für Ihre Untersuchung unerheblich, die häufig den vollständigen Datensatz Dies empfiehlt sich auch Datensätze verschiedener besitzen *einige* gemeinsame Eigenschaften jedoch nicht *Alle* Eigenschaften sind. Sie generieren, die eine Tabelle besser aussieht oder funktioniert gut, wenn in eine CSV-Datei exportiert und dann in Excel bearbeitet.



## <a name="use-the-measure-command"></a>Verwenden Sie den messbefehl

Maßnahme ist der vielseitigste Befehle Protokollanalyse sucht. Dadurch werden Ihre Daten und die Ergebnisse nach einem bestimmten Feld gruppiert statistische *Funktionen* zuweisen. Gibt es mehrere statistische Funktionen Maßnahme.

### <a name="measure-count"></a>Count() messen

Die erste statistische Funktion, und eines der am einfachsten zu verstehen ist die Funktion *count()* .

Ergibt eine Suchabfrage wie `Type=Event`, als auch auf der linken Seite der Suchergebnisse Facets bezeichnet Filter anzeigen. Der Filter anzeigen eine Verteilung von Werten nach einem bestimmten Feld Ergebnisse Suche ausgeführt

![Suche Measure Anzahl](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Z. B. in der obigen Abbildung sehen Sie **das Feld** , und es zeigt, dass fast 739 Tausend Ereignisse in den Ergebnissen 68 eindeutig und unterschiedliche Werte für das Feld **Computer** Datensätze. Die Kachel zeigt nur die Top 5 die 5 häufigsten Werte in den Feldern **Computer** geschrieben werden), sortiert die Anzahl der Dokumente, die dieser bestimmten Wert in diesem Feld enthalten. Im Bild sehen Sie, dass fast 369 Tausend Ereignisse – 90 Tausend OpsInsights04.contoso.com Computer 83 tausend Computer DB03.contoso.com stammen und usw..


Möchten Sie die Kachel nur nur die oberen 5 zeigt alle Werte anzeigen?

Das ist was messbefehl mit der Count()-Funktion. Diese Funktion verwenden keine Parameter. Geben Sie einfach das Feld Gruppieren nach – **Feld** in diesem Fall soll:

`Type=Event | Measure count() by Computer`

![Suche Measure Anzahl](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

**Computer** ist jedoch nur ein Feld *in* jede Daten – gibt keine relationalen Datenbanken und gibt **keine separaten Computerobjekt** überall. Nur die Werte *in* den Daten beschreiben die Entität generiert werden und verschiedene andere Merkmale und Aspekte der Daten – damit Begriff *Facet*. Allerdings können Sie genauso gut von anderen Feldern gruppieren. Da die ursprünglichen Ergebnisse fast 739 Tausend Ereignisse, die an den messbefehl geleitet werden auch ein Feld namens **EventID**verfügen, können Sie das gleiche Verfahren nach diesem Feld Gruppieren und Anzahl Ereignisse EventID anwenden:

```
Type=Event | Measure count() by EventID
```

Wenn Sie interessieren sich nicht die tatsächliche Anzahl, die einen bestimmten Wert enthalten, aber stattdessen soll nur eine Liste der Werte selbst, Sie einen *Select* -Befehl am Ende und nur können wählen Sie die erste Spalte:

```
Type=Event | Measure count() by EventID | Select EventID
```

Sie können komplizierte und bereits in der Abfrage sortieren oder Sie können nur die Spalten im Raster zu klicken.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Mit Measure Anzahl

- Geben Sie im Feld Abfrage suchen`Type=Event | Measure count() by EventID`
- Append `| Select EventID` am Ende der Abfrage.
- Abschließend fügen `| Sort EventID asc` am Ende der Abfrage.


Es gibt einige wichtige Punkte beachten und hervorheben:

Erstens sind die Ergebnisse nicht die ursprünglichen raw Ergebnisse mehr. Stattdessen sind sie aggregierte Ergebnisse – im Wesentlichen Gruppen von Ergebnissen. Kein Problem, aber Sie sollten wissen, dass Sie sehr unterschiedliche Form Daten unterscheidet sich von der ursprünglichen raw Form interagieren, die durch Aggregation-statistische Funktion dynamisch erstellt wird.

Zweitens gibt **Measure Anzahl** derzeit nur die ersten 100 unterschiedliche Ergebnisse zurück. Diese Beschränkung gilt nicht für die statistischen Funktionen. Daher müssen Sie normalerweise genaueren Filter zuerst verwenden, um bestimmte Elemente suchen bevor Maßnahme count() anwenden.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Verwenden Sie die Funktionen Max und min mit dem messbefehl

Es gibt verschiedene Szenarios **Messen Max()** **Maßnahme Min()** nützlich und sind. Aber da jede Funktion gegenüber, werden wir Max() veranschaulichen und probieren Sie Min() selbst.

Abfrage für Sicherheitsereignisse haben sie eine **Level** -Eigenschaft variieren kann. Zum Beispiel:

```
Type=SecurityEvent
```

![Suche Measure Anzahl start](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Wenn den höchsten Wert für alle anzuzeigenden können Ereignisse häufig Computer nach Feld gruppieren Sie

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![Max. Maß Computersuche](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Es wird angezeigt, für die Computer, die **auf** Datensätze, die meisten haben mindestens Stufe 8, viele hatte eine 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![Max. Maß Suchzeit generiert computer](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Funktioniert gut mit Zahlen, aber es funktioniert auch mit DateTime-Felder. Es empfiehlt sich, den letzten oder neuesten Zeitstempel für alle Daten, die für jeden Computer indiziert überprüfen. Zum Beispiel: Wann trat das neueste Sicherheitsereignis für jeden Computer?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Verwenden Sie die Avg-Funktion mit dem messbefehl

Die Avg() statistische Funktion mit können Sie den Durchschnittswert für ein Feld und Gruppieren von Ergebnissen nach demselben oder einem anderen Feld berechnen. Dies ist nützlich in verschiedenen Fällen wie Leistungsdaten.

Wir beginnen mit. Beachten Sie, dass OMS derzeit Leistungsindikatoren für Windows- und Linux-Computern erfasst.

Um *Alle* Daten zu suchen, ist die grundlegende Abfrage:

```
Type=Perf
```

![Suche Avg starten](./media/log-analytics-log-searches/oms-search-avg01.png)

Sie sehen ist Protokollanalyse drei zeigt: Liste zeigt zeigt die Datensätze hinter Diagramme; Tabelle der Tabellenansicht Leistungsindikatordaten angezeigt wird; und Metriken, die Diagramme für die Leistungsindikatoren.

Im Bild oben gibt es zwei Felder, die Folgendes angeben:

- Der erste Satz gibt Windows Leistungsindikatorname Objektnamen und Instanznamen in den Abfragefilter. Dies sind die Felder verwenden Sie vermutlich am häufigsten als Facetten-Filter
- **Gegenwert** ist der eigentliche Wert des Zählers. In diesem Beispiel wird der Wert *75*.
- **TimeGenerated** ist 12:51 im 24-Stunden-Format.

Hier ist eine Ansicht der Metrik in einem Diagramm.

![Suche Avg starten](./media/log-analytics-log-searches/oms-search-avg02.png)

Measure Avg() können Sie lesen Datensatz Perf-Shape, und zum anderen Suchtechniken Lektüre dieses Typs von numerischen Daten, aggregieren.

Hier ist ein einfaches Beispiel:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![Durchschnittliche Samplevalue suchen](./media/log-analytics-log-searches/oms-search-avg03.png)

In diesem Beispiel wählen Sie die CPU-Gesamtzeit Leistung Zähler und durchschnittliche vom Computer. Möchten Sie die Ergebnisse der letzten 6 Stunden einzuschränken, verwenden das Filtersteuerelement Zeit oder in der Abfrage angeben:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Mit Avg-Funktion mit dem messbefehl
- Geben Sie im Suchfeld Abfrage `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.


Sie sammeln und Korrelieren von Daten *auf* Computern. Angenommen Sie, haben Sie eine Reihe von Hosts in eine Farm, wo jeder Knoten entspricht anderen sie einfach alle Geben Sie Arbeit und laden sollte ungefähr ausgeglichen. Sie erhalten ihre Indikatoren, in einem mit folgender Abfrage und Durchschnittswerte für die gesamte Farm. Sie können durch Auswählen der Computer mit dem folgenden Beispiel starten:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Jetzt haben Sie den Computer nur soll zwei Key Performance Indicators (KPIs): CPU-Auslastung und % freien Speicherplatz. So wird dieser Teil der Abfrage:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Jetzt können Sie Computer und mit dem folgenden Beispiel:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Haben eine sehr spezielle Auswahl kann der **Maßnahme Avg()** Befehl zurück den Durchschnitt nicht Computer, sondern in der Farm einfach durch Gruppierung von CounterName. Zum Beispiel:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Dadurch werden nützliche Kompaktansicht ein paar Ihrer Umgebung KPIs.

![Suche Avg gruppieren](./media/log-analytics-log-searches/oms-search-avg04.png)


Einfach können die Suchabfrage in einem Dashboard. Beispielsweise konnte Sie die Suchabfrage speichern und erstellen ein Dashboard mit dem Namen *Web Farm KPIs*. Erfahren Sie mehr über die Verwendung von Dashboards finden Sie unter [Erstellen eines benutzerdefinierten Dashboards Protokollanalyse](log-analytics-dashboards.md).

![Suche Avg dashboard](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Verwenden Sie die Summenfunktion mit dem messbefehl

Die Summe-Funktion ähnelt anderen Funktionen des Befehls messen. Ein Beispiel zur Verwendung die Summenfunktion [W3C IIS Protokolle](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx)Suchen in Microsoft Azure betriebliche Informationen sehen.

Max() und Min() können mit Zahlen, Zeitangaben und Textzeichenfolgen. Textzeichenfolgen alphabetisch sortiert und Sie zuerst und zuletzt.

Sie können nicht jedoch Sum() mit nicht numerische Felder verwenden. Dies gilt auch für Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Quantil-Funktion mit dem messbefehl

Quantil-Funktion ähnelt Avg() und Sum(), nur für numerische Felder verwendet werden kann. Können Sie alle Quantil zwischen 1 und 99 in einem numerischen Feld ein. Sie können auch Befehle **Perzentil** und **Pct** . Es folgen einige Beispiele:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Verwenden Sie die Where Befehl

Der Befehl funktioniert wie ein Filter, aber in der Pipeline auf Weitere Ergebnisse filtern, die von einem messbefehl – erzeugt angewendet werden und nicht raw Ergebnisse, gefiltert am Anfang einer Abfrage.

Zum Beispiel:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Hinzufügen einer anderen Pipe "|" Zeichen und der Befehl nur Computern abzurufen, deren durchschnittliche CPU über 80 % mit dem folgenden Beispiel:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Wenn Sie Microsoft System Center - Operations Manager kennen können Sie sich vorstellen wo Befehl Management Pack ausgedrückt. Wenn im Beispiel eine Regel, wäre der erste Teil der Abfrage der Datenquelle und der Befehl wäre die Erkennung.

Können Sie die Abfrage als Kachel in **Mein Dashboard**Überwachung Art sehen, wenn Computer CPUs überlastet sind. Erfahren Sie mehr über Dashboards finden Sie unter [Erstellen eines benutzerdefinierten Dashboards Protokollanalyse](log-analytics-dashboards.md). Sie können auch mit der app Dashboards verwenden. Weitere Informationen finden Sie unter [OMS-Mobile-Anwendung ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). In Kacheln unten zwei der folgenden Abbildung sehen Sie der Monitor angezeigt und als Zahl. Im Wesentlichen sollten Sie immer die Zahl Null und die Liste leer. Andernfalls zeigt eine Warnung an. Bei Bedarf können sie einen Blick auf die Computer unter Druck.

![Mobile Dashboards](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Verwenden Sie den in-operator

Der Operator *IN* mit *NOT IN* können Sie Subsearches, suchen, die einer anderen als Argument Suche verwenden. Sie sind in geschweiften Klammern {} in einem anderen *primären* oder *äußere* Suche enthalten. Das Ergebnis einer Subsearch häufig eine Liste der unterschiedlichen Ergebnisse wird dann als Argument in der primären Suche verwendet.

Subsearches können Sie Teilmengen von Daten entsprechen, beschreiben können direkt im Suchbegriff, aber die von der Suche generiert werden können. Z. B. Wenn Sie einen *Computer Sicherheitsupdates fehlen*alle Ereignisse zu verwenden, müssen Sie eine Subsearch entwerfen, die dieser *Computer Sicherheitsupdates fehlen* zunächst identifiziert vor Ereignissen gehören die Hosts gefunden.

So können Sie mit der folgenden Abfrage *Computer derzeit erforderlichen Sicherheitsupdates fehlen* express:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![IM Suchbeispiel](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Haben die Liste können Sie die Suche als innere Suche zugeführt die Liste der Computer einen äußeren (primäre) suchen, an denen Ereignisse für diesen Computer. Dazu innere Suche in Klammern einschließen und die Ergebnisse als mögliche Werte für ein Filterfeld äußeren suchen IN-Operator mit Fütterung. Die Abfrage würde ähneln:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![IM Suchbeispiel](./media/log-analytics-log-searches/oms-search-in02-revised.png)


Außerdem Beachten der Zeitfilter im inneren Bereich verwendet, da der Update-System einen Snapshot aller Computer innerhalb von 24 Stunden dauert. Die innere Abfrage können einfacher und präziser Sie nur einen Tag suchen. Die äußere Suche verwendet stattdessen die Auswahl in der Benutzeroberfläche Abrufen der Ereignisse der letzten 7 Tage. Weitere Informationen zu zeitoperatoren finden Sie unter [boolesche Operatoren](#boolean-operators) .

Da Sie wirklich nur die Suchergebnisse innere als Filter für die äußere Wert, können Sie weiterhin Befehle im äußeren Bereich. Beispielsweise können Sie der oben aufgeführten Ereignisse mit anderen Maßnahme weiterhin gruppieren:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![IM Suchbeispiel](./media/log-analytics-log-searches/oms-search-in03-revised.png)


In der Regel sollen die innere Abfrage schnell ausgeführt werden, da der Protokollanalyse dienstseitigen Timeouts verfügt und wenige Ergebnisse zurückgegeben. Wenn die innere Abfrage mehr Ergebnisse zurückgibt, wird die Liste abgeschnitten, die konnte die äußere Suche zu falschen Ergebnissen führen.

Eine andere Regel ist, dass die interne Suche derzeit *aggregierte* Ergebnisse. Es muss also einen *Measure* -Befehl enthalten. Sie können nicht derzeit in einer äußeren Suche Ergebnisse.

Nur ein IN-Operator kann auch muss der letzte Filter in der Abfrage. Mehrere Operatoren IN nicht oder würde dies im Wesentlichen verhindert die Ausführung mehrerer Subsearches: wichtig ist die einzige Sub-inneren Suche kann jede äußere suchen.

Selbst mit diesen IN kann viele Arten von korrelierten sucht und ähnlich wie Computer, definieren Sie Anwendern oder Dateien – was die Felder Daten enthalten. Es folgen weitere Beispiele:

**Alle Updates fehlen Computer dem Einstellung für automatische Updates deaktiviert ist**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Alle Fehlerereignisse von Computern mit SQL Server (=, SQL-Bewertung ausgeführt hat)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Alle Sicherheitsereignisse von Computern, die Domänencontroller (=, AD Bewertung ausgeführt wurde)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Die haben andere Konten auf demselben Computer angemeldet, BACONLAND\jochan Konto angemeldet hat?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Unterschiedliche Befehl

Wie der Name schon sagt, stellt dieser Befehl eine Liste der unterschiedlichen Werte für ein Feld. Es ist erstaunlich einfach, aber sehr nützlich. Dasselbe kann auch wie folgt mit Maßnahme count() erreicht werden.

```
Type=Event | Measure count() by Computer
```

![Beispiel unterschiedliche suchen](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Wenn Sie jedoch nur eine Liste der unterschiedlichen Werte und nicht die Anzahl der Dokumente, die Werte und DISTINCT bereitstellen kann optimiert und einfacher zu lesen Sie Ausgabe und kürzere Syntax wie unten gezeigt.

```
Type=Event | Distinct Computer
```
![Beispiel unterschiedliche suchen](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Countdistinct-Funktion mit dem messbefehl
Countdistinct-Funktion zählt die Anzahl von unterschiedlichen Werten in jeder Gruppe. Beispielsweise kann verwendet werden, um die Anzahl von eindeutigen Computern für jeden:

```
* | measure countdistinct(Computer) by Type
```

![OMS-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Verwenden Sie den Befehl Maßnahme Intervall
Mit in Echtzeit-Leistungsdaten gesammelt und jeder Leistungsindikator Protokollanalyse darstellen. Eingabe der Abfrage **Typ: Perf** Tausende von metrischen Diagramme zurück, basierend auf der Anzahl von Leistungsindikatoren und Servern in Ihrer Umgebung Protokollanalyse. Bei Bedarf metrische Aggregation können Sie betrachten insgesamt Metriken in Ihrer Umgebung zu einem hohen und tiefen Einblick in detailliertere Daten nach Bedarf.

Angenommen Sie möchten wissen, was die durchschnittliche CPU auf allen Computern. Durchschnittliche CPU für jeden Computer betrachten nicht hilfreich möglicherweise Ergebnisse geglättet abrufen können. Um weitere Informationen zu suchen, können Sie aggregieren Ergebnis eine kleinere Stücke Fenster und Suchen in einem über verschiedene Dimensionen hinweg. Beispielsweise können Sie stündlich durchschnittliche CPU-Nutzung auf allen Computern wie folgt ausführen:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![durchschnittlich Intervall messen](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Standardmäßig werden diese Ergebnisse in einem Diagramm mit mehreren interaktiven Serie angezeigt.  Dieses Diagramm unterstützt Serie umschalten (mit Skalierung y-Achse), Zoomen und bewegen.  Anzeigeoption Tabelle steht weiterhin zum Anzeigen der unformatierten Daten bei Bedarf.

Sie können auch nach anderen Feldern gruppieren. In diesem Beispiel suche alle % Leistungsindikatoren für einen bestimmten Computer und möchte wissen, was die stündlich 70 Perzentile jeder Zähler ist:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Zu beachten ist, dass diese Abfragen nicht auf Leistungsindikatoren. Sie können sie einer Metrik zuweisen. In diesem Beispiel Suche ich W3C IIS-Protokolle. Ich möchte wissen, was die maximale Zeit in 5-Minuten-Intervall für die Verarbeitung jeder Anforderung dauert:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Verwenden Sie mehrere Aggregate in einer Abfrage
Sie können mehrere aggregate-Klausel in einem messbefehl.  Jeder kann Alias unabhängig sein.  Wenn kein Alias angegeben ist werden der resultierende Feldname, die Aggregatfunktion verwendet (z. B. "avg(CounterValue)" für avg(CounterValue)).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS-multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Hier ist ein weiteres Beispiel:
 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Protokoll suchen finden Sie unter:

- Verwenden Sie [Felder in Protokollanalyse](log-analytics-custom-fields.md) Protokoll suchen erweitert.
- Überprüfen der [Protokollanalyse Suchverweis protokollieren](log-analytics-search-reference.md) aller Suchfelder und Facets für Protokollanalyse.
