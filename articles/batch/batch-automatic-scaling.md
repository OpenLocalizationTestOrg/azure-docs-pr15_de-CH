<properties
    pageTitle="Automatisch skalieren compute-Knoten in einem Pool Azure Batch | Microsoft Azure"
    description="Aktivieren der automatischen Skalierung Cloud-Pool die Anzahl der Compute-Knoten im Pool dynamisch angepasst."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>Skalieren Sie Compute-Knoten in einem Pool Azure Batch automatisch

Mit der automatischen Skalierung kann Azure Batch-Dienst dynamisch hinzufügen oder Entfernen von Compute-Knoten in einem Pool basierend auf Parametern definieren. Sie können potenziell sparen Zeit und Geld automatisch anpassen Compute Strom von der Anwendung verwendeten Knoten wie des Auftrags Aufgabe erfordert hinzufügen und entfernen, wenn sie verringern.

Aktivieren der automatischen Skalierung auf ein Compute-Knoten durch Zuordnen einer *Formel skalieren* , die definiert, wie mit [PoolOperations.EnableAutoScale] [ net_enableautoscale] -Methode in der Bibliothek [Batch.NET](batch-dotnet-get-started.md) . Batch-Dienst verwendet dann diese Formel die Anzahl von Compute-Knoten bestimmt, die erforderlich sind, um Ihre Arbeit auszuführen. Stapel entspricht Dienstmetrik Beispieldaten in regelmäßigen Abständen erfasst und passt die Anzahl von Compute-Knoten im Pool in einem konfigurierbaren Intervall auf Basis der Formel.

Sie können die automatische Skalierung ein Pool erstellt oder in einem vorhandenen Pool. Sie können auch eine vorhandene Formel in einem Pool ändern, "Skalieren" aktiviert ist. Batch ermöglicht das Auswerten von Formeln vor Pools zuweisen sowie überwachen den Status der automatischen Skalierung ausgeführt wird.

## <a name="automatic-scaling-formulas"></a>Automatische Skalierung Formeln

Automatische Skalierung Formel ist eine Zeichenfolge, die Sie definieren, die eine oder mehrere Anweisungen enthält und erhält einen Pool [AutoScaleFormula] [ rest_autoscaleformula] Element (Batch REST) oder [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] Eigenschaft (Stapelverarbeitung). Wenn ein Pool zugewiesen, verwendet der Dienst Batch Formel Zielanzahl von Compute-Knoten im Pool für das nächste Intervall Verarbeitung (mehr dazu später Intervalle) bestimmt. Formula String können nicht 8 KB Größe überschreiten, bis zu 100 Anweisung Zeilenumbrüche und Kommentare enthalten, die durch Semikolons getrennt sind.

Sie können automatische Skalierung Formeln mit Batch skalieren "Sprache" vorstellen. Formeln sind frei formulierte Ausdrücke, die Dienst definierten Variablen (definiert durch den Batch-Dienst) und benutzerdefinierte Variablen (Variablen, die Sie definieren) enthalten können. Sie können verschiedene Vorgänge auf diese Werte mithilfe von integrierten Typen, Operatoren und Funktionen ausführen. Eine Anweisung könnte z. B. die folgende Form aufweisen:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formeln enthalten in der Regel mehrere Anweisungen, die Operationen für Werte sind Aussagen. Angenommen, wir rufen Sie zunächst einen Wert für `variable1`, an eine Funktion zum Auffüllen übergeben `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Diese Aussagen in Ihrer Formel Ihr Ziel ist Compute-Knoten, die Pool-Skalierung **der **Node**-Anzahl gewidmet** . Diese Zahl kann höher sein niedriger oder gleich der aktuellen Anzahl der Knoten im Pool. Stapelverarbeitung wertet einen Pool Autoscale Formel in bestimmten Intervallen ([Automatische Skalierung Intervalle](#automatic-scaling-interval) werden unten erläutert). Dann wird die Node-Anzahl im Pool die Anzahl anpassen, die zum Zeitpunkt der Bewertung Autoscale Formel angibt.

Als ein Beispiel gibt diese Formel zweizeilige skalieren, dass die Anzahl der Knoten nach der Anzahl der aktiven Aufgaben maximal 10 Computeknoten angepasst werden:

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

Die nächsten Abschnitten dieses Artikels erläutern die verschiedenen Einheiten, aus denen Autoscale Formeln, einschließlich Variablen, Operatoren, Operationen und Funktionen besteht. Sie finden heraus, wie zu verschiedenen Compute Ressourcen- und Metriken in Batch. Diese Metriken können basierend auf Ressource: Einsatz und Vorgang Status des Pools Knotenanzahl auf intelligente Weise anpassen. Anschließend erfahren, wie Sie eine Formel erstellen und Aktivieren der automatischen Skalierung in einem Pool mit Batch REST und .NET APIs. Wir werden einige Formeln Ende.

> [AZURE.IMPORTANT] Jede Azure Batch-Konto ist auf maximal Kerne (und daher Compute-Knoten), die für die Verarbeitung verwendet werden kann. Der Batch-Dienst erstellt Knoten nur bis, Core. Daher kann nicht Zielanzahl von Compute-Knoten erreicht, die von einer Formel angegeben ist. Informationen anzeigen und Ihr Konto Kontingente finden Sie [Kontingente und Grenzwerte für Azure Batch-Dienst](batch-quota-limit.md) .

## <a name="variables"></a>Variablen

**Dienst definiert** und **benutzerdefinierte** Variablen können in Formeln skalieren. Dienst definierten Variablen sind in der Batch-Dienst integriert einige schreibgeschützt sind und einige sind schreibgeschützt. Benutzerdefinierte Variablen sind Variablen, die *Sie* definieren. Im oben genannten zweizeilige Beispielformel `$TargetDedicated` ist ein Dienst definiert und Variablen `$averageActiveTaskCount` ist eine benutzerdefinierte Variable.

Die Tabelle anzeigen schreibgeschützt und schreibgeschützt vom Batch-Dienst definierten Variablen

Sie können **Abrufen** und **festlegen** der Werte dieser Variablen Dienst definiert die Anzahl von Compute-Knoten in einem Pool zu:

| Lese-/ Schreibzugriff Dienst definierten Variablen | Beschreibung |
| --- | --- |
| $TargetDedicated | Die **vorgegebene** Anzahl **dedizierter Compute-Knoten** für den Pool. Dies ist die Anzahl von Computeknoten Pool skaliert werden soll. Es ist eine Zahl "Target", da ein Pool die Node-Anzahl erreicht werden. Dies kann auftreten, wenn die Node-Anzahl erneut nachfolgende Autoscale Bewertung geändert wird, bevor der Pool das erste Ziel erreicht hat. Es kann auch auftreten, wenn Batch Konto Knoten oder Core Kontingent erreicht wird, bevor die Node-Anzahl erreicht ist. |
| $NodeDeallocationOption | Die Aktion, die bei Compute-Knoten aus einem Pool entfernt werden. Mögliche Werte sind:<ul><li>**Requeue**- Vorgänge sofort beendet und werden in der Warteschlange platziert, sodass sie verschoben.<li>**Beenden**- Vorgänge sofort beendet und aus der Warteschlange entfernt.<li>**Taskcompletion**– wartet für laufende Aufgaben beenden und der Knoten aus dem Pool entfernt.<li>**Retaineddata**– wartet alle lokalen Vorgang beibehalten Daten auf dem Knoten bereinigt werden, bevor der Knoten aus dem Pool entfernt.</ul> |

Sie können den Wert dieser Variablen Dienst definiert Anpassungen Metriken aus der Batch-Dienst basieren **erhalten** :

| Schreibgeschützte Dienst definierten Variablen | Beschreibung |
| --- | --- |
| $CPUPercent | Prozent CPU-Auslastung. |
| $WallClockSeconds | Die Anzahl der Sekunden verwendet. |
| $MemoryBytes | Die durchschnittliche Anzahl von Megabyte verwendet. |
| $DiskBytes | Die durchschnittliche Anzahl von Gigabyte auf lokalen Datenträgern verwendet. |
| $DiskReadBytes | Die Anzahl der gelesenen Bytes. |
| $DiskWriteBytes | Die Anzahl der geschriebenen Bytes. |
| $DiskReadOps | Die Anzahl der gelesenen Vorgängen durchgeführt. |
| $DiskWriteOps | Die Anzahl der Datenträger-Schreibvorgänge ausgeführt werden. |
| $NetworkInBytes | Die Anzahl der eingehenden Bytes. |
| $NetworkOutBytes | Die Anzahl der ausgehenden Bytes. |
| $SampleNodeCount | Die Anzahl von Compute-Knoten. |
| $ActiveTasks | Die Anzahl der Aufgaben in einem aktiven Zustand. |
| $RunningTasks | Die Anzahl der Aufgaben ausgeführt. |
| $PendingTasks | Die Summe der $ActiveTasks und $RunningTasks. |
| $SucceededTasks | Die Anzahl der Aufgaben, die erfolgreich abgeschlossen. |
| $FailedTasks | Die Anzahl der Aufgaben ist fehlgeschlagen. |
| $CurrentDedicated | Die aktuelle Anzahl der dedizierte compute-Knoten. |

> [AZURE.TIP] Schreibgeschützt, Dienst definierten Variablen, die oben dargestellt sind *Objekte* , die verschiedene Methoden jeweils Datenzugriff. Weitere Informationen finden Sie unter [Daten beziehen](#getsampledata) .

## <a name="types"></a>Typen

Diese **Typen** werden in einer Formel.

- Doppelte
- doubleVec
- doubleVecList
- Zeichenfolge
- Timestamp - Zeitstempel ist eine zusammengesetzte Struktur mit den folgenden Elementen:

    - Jahr
    - Monat (1-12)
    - Tag (1-31)
    - Wochentag (im Format Zahl, z. B. 1 für Montag)
    - Stunde (im 24-Stunden-Zahlenformat z. B. 13 bedeutet 13.00 Uhr)
    - Minute (00-59)
    - Sekunde (00-59)
- TimeInterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>Vorgänge

Diese **Vorgänge** sind über die oben aufgeführten Typen zulässig.

| Vorgang                             | Unterstützte Operatoren   | Ergebnistyp   |
| ------------------------------------- | --------------------- | ------------- |
| doppelte *Operator* double              | +, -, *, /            | Doppelte            |
| *Operator* Double timeinterval        | *                     | TimeInterval      |
| DoubleVec *Operator* double           | +, -, *, /            | doubleVec         |
| DoubleVec *Operator* doubleVec        | +, -, *, /            | doubleVec         |
| TimeInterval *Operator* double        | *, /                  | TimeInterval      |
| TimeInterval *Operator* timeinterval  | +, -                  | TimeInterval      |
| TimeInterval *Operator* Zeitstempel     | +                     | Zeitstempel         |
| Timestamp *Operator* timeinterval     | +                     | Zeitstempel         |
| Timestamp *Operator* Zeitstempel        | -                     | TimeInterval      |
| *Operator*double                      | -, !                  | Doppelte            |
| *Operator*timeinterval                | -                     | TimeInterval      |
| doppelte *Operator* double              | < < =, ==, > =, >,! =  | Doppelte            |
| *Operator* -Zeichenfolge              | < < =, ==, > =, >,! =  | Doppelte            |
| Timestamp *Operator* Zeitstempel        | < < =, ==, > =, >,! =  | Doppelte            |
| TimeInterval *Operator* timeinterval  | < < =, ==, > =, >,! =  | Doppelte            |
| doppelte *Operator* double              | & & & #124; & #124;      | Doppelte            |

Bei Double ternären Operator (`double ? statement1 : statement2`) ungleich NULL ist **true**und NULL ist **falsch**.

## <a name="functions"></a>Funktionen

Vordefinierte **Funktionen** stehen für die Verwendung eine automatische Skalierung Formel definieren.

| Funktion                          | Rückgabetyp   | Beschreibung
| --------------------------------- | ------------- | --------- |
| AVG(doubleVecList)                | Doppelte        | Gibt den Mittelwert aller Werte in der DoubleVecList zurück.
| Len(doubleVecList)                | Doppelte        | Gibt die Länge des Vektors aus der DoubleVecList erstellt wird.
| LG(Double)                        | Doppelte        | Gibt das Protokoll der doppelten Basis 2 zurück.
| LG(doubleVecList)                 | doubleVec     | Gibt das componentwise Protokoll Basis 2 die DoubleVecList. Ein vec(double) muss explizit für den Parameter übergeben werden. Andernfalls wird die doppelte lg(double) Version.
| LN(Double)                        | Doppelte        | Den natürlichen Logarithmus des Double zurückgegeben.
| LN(doubleVecList)                 | doubleVec     | Gibt das componentwise Protokoll Basis 2 die DoubleVecList. Ein vec(double) muss explizit für den Parameter übergeben werden. Andernfalls wird die doppelte lg(double) Version.
| Log(Double)                       | Doppelte        | Gibt das Protokoll der doppelten Basis 10 zurück.
| Log(doubleVecList)                | doubleVec     | Gibt das componentwise Protokoll Basis 10 der DoubleVecList. Ein vec(double) muss explizit für die doppelte Parameter übergeben werden. Andernfalls wird die doppelte log(double) Version.
| Max(doubleVecList)                | Doppelte        | Gibt den größten Wert in der DoubleVecList.
| Min(doubleVecList)                | Doppelte        | Gibt den kleinsten Wert in der DoubleVecList.
| Norm(doubleVecList)               | Doppelte        | Gibt zwei Norm Vektor, der von der DoubleVecList erstellt wird.
| Quantil (DoubleVec V, double p) | Doppelte        | Gibt das Quantil Element des Vektors V.
| Zufallszahl)                            | Doppelte        | Gibt einen zufälligen Wert zwischen 0,0 und 1,0.
| Range(doubleVecList)              | Doppelte        | Gibt die Differenz zwischen min und max-Werte in der DoubleVecList.
| Std(doubleVecList)                | Doppelte        | Gibt die Standardabweichung der Stichprobe der Werte in der DoubleVecList zurück.
| Stop()                            |               | Beendet die Auswertung des Ausdrucks Skalierung.
| SUM(doubleVecList)                | Doppelte        | Gibt die Summe aller Komponenten der DoubleVecList zurück.
| Uhrzeit (DateTime string = "")          | Zeitstempel     | Gibt den Zeitstempel der aktuellen Uhrzeit, wenn keine Parameter übergeben werden, Zeitstempel der DateTime-Zeichenfolge übergeben wird. Unterstützte DateTime Formate sind W3C DTF und RFC 1123.
| Val (DoubleVec V, doppelte i)        | Doppelte        | Gibt den Wert des Elements am Speicherort i im Vektor V mit startIndex NULL ist.

In der obigen Tabelle beschriebenen Funktionen akzeptieren eine Liste als Argument. Durch Kommas getrennte Liste ist eine beliebige Kombination der *und *DoubleVec** . Zum Beispiel:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

Ein einzelnes *DoubleVec* vor der Auswertung der *DoubleVecList* -Wert konvertiert. Beispielsweise wenn `v = [1,2,3]`, dann `avg(v)` entspricht dem Aufruf von `avg(1,2,3)`. Aufrufen von `avg(v, 7)` entspricht dem Aufruf von `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Daten abrufen

Skalieren Formeln bearbeiten Metriken (Samples), die vom Batch-Dienst bereitgestellt wird. Eine Formel wächst oder schrumpft Poolgröße basierend auf den Werten, die vom Dienst erhält. Die oben beschriebenen Variablen Dienst definiert sind Objekte, die verschiedene Methoden um auf Daten zuzugreifen, die diesem Objekt zugeordnet ist. Der folgende Ausdruck zeigt z. B. eine Anforderung zum Abrufen der letzten fünf Minuten der CPU-Auslastung:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| -Methode | Beschreibung |
| --- | --- |
| GetSample() | Die `GetSample()` -Methode gibt einen Vektor von Beispieldaten.<br/><br/>Ein Beispiel ist 30 Sekunden Metrikdaten. Beispiele sind also alle 30 Sekunden. Aber wie unten aufgeführt, entsteht eine Verzögerung zwischen der bei der Erfassung der Stichprobe und sie eine Formel zur. So können nicht alle Beispiele für einen bestimmten Zeitraum eine Formel ausgewertet werden.<ul><li>`doubleVec GetSample(double count)`<br/>Gibt die Anzahl der Samples, die neuesten Beispiele erhältlich, die gesammelt wurden.<br/><br/>`GetSample(1)`Gibt das letzte Beispiel verfügbare. Für Metriken wie `$CPUPercent`, aber dies sollte nicht verwendet werden weil es unmöglich zu wissen, *Wenn im Beispiel wurden* . Möglicherweise kürzlich oder wegen Systemprobleme möglicherweise älter. Es ist besser, in diesem Fall verwenden Sie ein Zeitintervall wie unten dargestellt.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Gibt einen Zeitrahmen für das Erfassen von Daten. Gegebenenfalls wird auch den Prozentsatz der Samplings, die in den angeforderten Zeitraum verfügbar sein müssen.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`20 Proben zurück sind alle Beispiele für die letzten zehn Minuten CPUPercent Geschichte. Historie der letzten Minute nicht verfügbar war, würde nur 18 Beispiele jedoch zurückgegeben. In diesem Fall:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`würde fehlschlagen, da nur 90 % der Proben zur Verfügung stehen.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`wäre erfolgreich.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Gibt einen Zeitrahmen für die Datensammlung mit eine Startzeit und eine Endzeit.<br/><br/>Wie bereits erwähnt, gibt es eine Verzögerung zwischen der bei der Erfassung der Stichprobe und sie eine Formel zur. Dies muss als bei Verwendung der `GetSample` Methode. Siehe `GetSamplePercent` unten.|
| GetSamplePeriod() | Gibt den Zeitraum der Proben in Historie Beispieldataset zurück. |
| Count() | Gibt die Gesamtzahl der Samplings in metrischen Geschichte. |
| HistoryBeginTime() | Gibt den Zeitstempel der ältesten verfügbaren Probe für die Metrik. |
| GetSamplePercent() |Gibt den Prozentsatz der verfügbaren Beispiele für eine angegebene Zeitspanne. Zum Beispiel:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Da die `GetSample` -Methode schlägt fehl, wenn der Prozentsatz der Samplings zurückgegeben wird weniger `samplePercent` angegeben, können Sie die `GetSamplePercent` -Methode zunächst überprüft. Dann können Sie eine alternative Aktion ausführen, wenn nicht genügend Beispiele vorhanden sind, ohne automatische Skalierung Auswertung anhalten.|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Beispiele, Beispiel Prozentsatz und die *GetSample()* -Methode

Core-Vorgang einer Formel skalieren ist Vorgangs- und Daten und passen Sie Größe basierend auf diesen Daten. Daher ist es wichtig, ein klares Verständnis der Interaktion Autoscale Formeln mit Metrikdaten oder "Samples".

**Beispiele**

Der Batch-Dienst in regelmäßigen Abständen *Proben* Vorgangs- und Metriken und macht sie skalieren Formeln zur. In diesen Beispielen werden alle 30 Sekunden vom Batch-Dienst aufgezeichnet. Allerdings ist in der Regel Latenz, die verursacht eine Verzögerung zwischen Wenn diese Beispiele wurden und sie werden (und von gelesen werden) skalieren Formeln. Außerdem aufgrund verschiedener Faktoren wie Netzwerk- oder andere Infrastrukturprobleme Beispiele möglicherweise nicht für einen bestimmten Zeitraum aufgezeichnet wurde. Dadurch "fehlenden" Beispiele.

**Prozentsatz für die Stichprobe**

Wenn `samplePercent` übergeben wird die `GetSample()` -Methode oder die `GetSamplePercent()` -Methode aufgerufen wird, "Prozent" bezieht sich auf einen Vergleich zwischen insgesamt *möglichen* Anzahl von Proben, die vom Batch-Dienst aufgezeichnet werden und die Anzahl der Samples, die tatsächlich *verfügbaren* zur Formel skalieren.

Sehen wir uns eine 10-minütige Timespan als Beispiel. Da Beispiele alle 30 Sekunden in einem Zeitraum 10 Minuten aufgezeichnet werden wäre insgesamt maximal Beispiele, die die Stapelverarbeitung erfasst 20 Proben (2 pro Minute). Jedoch aufgrund der inhärente Latenz des Berichterstellungsmechanismus oder ein anderes Problem in der Azure-Infrastruktur möglicherweise nur 15 Beispiele zur Autoscale Formel zum Lesen verfügbar sind. Dies bedeutet, dass für diesen Zeitraum 10 Minuten sind nur **75 Prozent** der Gesamtzahl der Samplings, die erfasst die Formel zur Verfügung.

**GetSample() und Beispiel-Bereiche**

Skalierungsgröße Formeln werden vergrößern und Verkleinern der Pools - Knoten hinzufügen oder Entfernen von Knoten. Da Knoten Geld kostet, möchten Sie sicherstellen, dass Formeln einer intelligenten Analysemethode verwenden, die auf Daten basiert. Wir empfehlen daher einen Trend Typenanalyse in Formeln verwenden. Dieser Typ vergrößern und Verkleinern der Pools anhand eines *Bereichs* Proben.

Dazu verwenden Sie `GetSample(interval look-back start, interval look-back end)` ein **Vektor** von Beispielen zurück:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Wenn Zeile Batch ausgewertet wird, wird ein Zellbereich Samples als Vektor von Werten zurückgegeben. Zum Beispiel:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Den Vektor der Proben gesammelt haben, können Sie Funktionen wie `min()`, `max()`, und `avg()` erfassten Bereich sinnvolle Werte ableiten.

Für zusätzliche Sicherheit können Sie eine Formel Bewertung *nicht* erzwingen, wenn kleiner als ein bestimmter Prozentsatz für die Stichprobe für einen bestimmten Zeitraum verfügbar ist. Wenn Sie eine Formel nicht veranlassen, weisen Batch weitere Bewertung der Formel einzustellen, ist der Prozentsatz der Proben nicht verfügbar und Poolgröße keine Änderung vorgenommen. Erforderlichen Prozentsatz der Proben für die Auswertung erfolgreich, geben sie als dritten Parameter für `GetSample()`. Hier wird eine Anforderung von 75 % der Proben angegeben:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Es ist auch wichtig Verzögerung erwähnten Beispiel Verfügbarkeit immer einen Blick zurück Startzeit angeben, die älter als eine Minute. Deswegen dauert ungefähr eine Minute Beispiele über das System übernehmen so Proben im Bereich `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` häufig nicht zur Verfügung. Wiederum können Parameter Prozentsatz der `GetSample()` eine bestimmten Beispiel Prozentsatz Anforderung erzwingen.

> [AZURE.IMPORTANT] Wir **empfehlen** , dass Sie *nur *verlassen ** auf `GetSample(1)` in Ihrem Autoscale Formeln. Ist `GetSample(1)` im Wesentlichen sagt Batch-Dienst "mir im letzte Beispiel haben, egal wie lange Sie es." Da nur ein Beispiel ist und möglicherweise einer älteren Beispiel kann es nicht das Gesamtbild der aktuellen Aufgabe oder Ressourcenstatus repräsentativ sein. Verwenden Sie `GetSample(1)`, sicher, dass sie Teil einer größeren Anweisung und nicht die nur Daten, die die Formel verwendet.

## <a name="metrics"></a>Metriken

**Ressourcen-** und **Aufgabendaten** Metriken können Sie eine Formel definieren. Sie anpassen die Node-Anzahl dedizierte im Pool basierend auf Metrikdaten abrufen und auswerten. Jede Metrik entnehmen Sie [Variablen](#variables) Abschnitt Weitere Informationen.

<table>
  <tr>
    <th>Metrik</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td><b>Ressource</b></td>
    <td><p><b>Ressourcenmetriken</b> basieren auf die CPU-Bandbreite und Speicher Verwendung von Compute-Knoten sowie die Anzahl der Knoten.</p>
        <p> Dieser Dienst definierten Variablen sind nützlich für Anpassungen basierend auf Knoten:</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Dieser Dienst definierten Variablen sind nützlich für Anpassungen basierend auf Knoten Ressourcenverwendung:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Aufgabe</b></td>
    <td><p><b>Metriken Aufgabe</b> basierend auf dem Status der Aufgaben wie aktiv, und abgeschlossen. Die folgenden Service definierten Variablen sind nützlich für Pool-Größe Anpassungen Aufgabe Metriken anhand:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Schreiben einer Formel automatisch skalieren

Erstellen eine Formel automatisch Skalieren von Anweisungen, die oben aufgeführten Komponenten bilden Sie diese Anweisung in eine vollständige Formel kombinieren. In diesem Abschnitt erstellen wir eine Formel wird automatisch skalieren, die reale Skalierung entscheiden ausführen können.

Zunächst definieren wir an unsere neue Skalierungsgröße Formel. Die Formel muss:

1. **Erhöhen** der Zielnummer von Compute-Knoten in einem Pool ist CPU-Auslastung hoch.
2. **Verringern** der Zielanzahl der Compute-Knoten in einem Pool, wenn CPU-Auslastung niedrig ist.
3. Beschränken Sie die **maximale** Anzahl von Knoten immer auf 400.

*Erhöhen Sie* die Anzahl der Knoten bei hoher CPU-Auslastung, definieren wir die Anweisung, die eine benutzerdefinierte Variable gefüllt (`$totalNodes`) mit einem Wert, die 110 Prozent der aktuellen Ziel Knoten jedoch nur minimale durchschnittliche CPU-Auslastung während der letzten 10 Minuten wurde über 70 %. Andernfalls verwenden Sie dedizierten Zeitwert.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

Zum *verringern* der Anzahl der Knoten bei niedriger CPU-Auslastung legt die nächste Anweisung in Formel dasselbe `$totalNodes` Variable 90 Prozent der aktuellen Node-Anzahl wurde die durchschnittliche CPU-Auslastung in den letzten 60 Minuten unter 20 Prozent. Verwenden Sie andernfalls den aktuellen Wert der `$totalNodes` , die wir in der Anweisung oben aufgefüllt.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Jetzt maximal Ziel dedizierte Compute-Knoten auf ein **maximum** von 400:

```
$TargetDedicated = min(400, $totalNodes)
```

Die vollständige Formel lautet:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Automatische Skalierung aktiviert-Pool erstellen

Zum Erstellen eines neuen Pools mit Skalierung aktiviert können Sie eine der folgenden Methoden:

**Batch .NET**

1. Erstellen Sie den Pool mit [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx).
1. Legen Sie die Eigenschaft [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) auf `true`.
1. Legen Sie die Eigenschaft [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) der Formel skalieren.
1. (Optional) Legen Sie die [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx) -Eigenschaft (Standard ist 15 Minuten).
1. Übernehmen Sie mit [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) oder [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx).

**Batch REST-API**

* [Hinzufügen eines Pools mit einem Konto](https://msdn.microsoft.com/library/azure/dn820174.aspx): Geben Sie die `enableAutoScale` und `autoScaleFormula` Elemente in der REST-API-Anforderung konfigurieren Automatische Skalierung für einen Pool beim Erstellen.

Der folgende Codeausschnitt erstellt einen Pool skalieren aktiviert mit [Batch.NET] [ net_api] Bibliothek. Der Pool Autoscale Formel festgelegt der Node-Anzahl auf 5 montags und 1 für jeden Tag der Woche. [Automatische Skalierung Intervall](#automatic-scaling-interval) wird auf 30 Minuten festgelegt. In dieser und anderen C# Ausschnitte in diesem Artikel wird "MyBatchClient" eine ordnungsgemäß initialisierte Instanz [BatchClient][net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

Batch REST-API und .NET SDK können anderen [Stapel SDKs](batch-technical-overview.md#batch-development-apis), [Batch-PowerShell-Cmdlets](batch-powershell-cmdlets-get-started.md)und [Batch CLI](batch-cli-get-started.md) Sie automatische Skalierung arbeiten.

> [AZURE.IMPORTANT] Wenn skalieren aktiviert-Pool erstellen, müssen Sie **nicht** angeben der `targetDedicated` Parameter. Auch möchten Sie manuell einen Pool skalieren aktiviert Größe (z. B. mit [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]), müssen Sie erste **deaktiviert** automatische Skalierung im Pool, Ändern der Größe.

### <a name="automatic-scaling-interval"></a>Automatische Skalierung Intervall

Standardmäßig passt der Batch-Dienst Poolgröße Formel die Skalierungsgröße alle **15 Minuten**. Dieses Intervall lässt, jedoch mit den folgenden Eigenschaften:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (Batch .NET)
* [AutoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST-API)

Das kleinste Intervall beträgt fünf Minuten, und der Maximalwert ist 168 Stunden. Wenn ein Intervall außerhalb dieses Bereichs angegeben wird, wird der Batch-Dienst einen Fehler Bad Request (400) zurück.

> [AZURE.NOTE] Automatische Skalierung kann derzeit nicht Veränderungen in weniger als einer Minute reagieren jedoch eher soll die Größe des Pools schrittweise beim Ausführen einer Arbeitslast anpassen.

## <a name="enable-autoscaling-on-an-existing-pool"></a>Skalierung auf einem vorhandenen Pool aktivieren

Wenn Sie mithilfe des Parameters *TargetDedicated* bereits einen Pool mit einer festgelegten Anzahl von Compute-Knoten erstellt haben, können Sie weiterhin automatische Skalierung im Pool. Jeder Batch-SDK enthält beispielsweise eine Operation "Enable Autoscale":

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] (Batch .NET)
*  [Aktivieren der automatischen Skalierung in einem Pool] [ rest_enableautoscale] (REST-API)

Wenn Sie automatische Skalierung auf einem vorhandenen Pool aktivieren, gilt Folgendes:

* Ist automatische Skalierung derzeit im Pool **deaktiviert** die Anforderung "Enable Autoscale" Ausgabe angeben *muss* eine gültige Autoscale Formel, Ausgabe die Anforderung. Sie können *optional* ein skalieren Bewertung Intervall festlegen. Wenn Sie kein Intervall angeben, wird der Standardwert 15 Minuten verwendet.

* Wenn skalieren derzeit im Pool **aktiviert** ist, können eine Formel skalieren oder Bewertung Intervall angeben. Sie können nicht beide Eigenschaften auslassen.

  * Wenn Sie ein neues Autoscale Bewertung Intervall angeben, der vorhandenen Zeitplan zum Auswertung beendet und ein neuer Zeitplan gestartet. Der neue Zeitplan ist Start die Zeit, die Ausgabe die Anforderung "Skalieren aktivieren".

  * Wenn Sie Formel skalieren oder Bewertung Intervall, Batch-Dienst auslassen verwendet den aktuellen Wert dieser Einstellung.

> [AZURE.NOTE] Wenn Pool erstellt wurde ein Wert für den Parameter *TargetDedicated* angegeben wurde, wird sie ignoriert, wenn die automatische Skalierung Formel ausgewertet wird.

Dieser Codeausschnitt C# verwendet [.NET Stapel] [ net_api] Bibliothek Skalierung auf einem vorhandenen Pool zu aktivieren:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Aktualisieren einer Formel automatisch skalieren

Verwenden Sie die gleichen "Skalieren aktivieren" Anforderung zum *Aktualisieren* der Formel auf einem vorhandenen Pool skalieren aktiviert (z. B. mit [EnableAutoScale] [ net_enableautoscale] in Batch .NET). Es gibt keine spezielle "Skalieren" aufgetreten. Beispielsweise wenn automatische Skalierung beim Ausführen des folgenden Codes bereits auf "Myexistingpool" aktiviert ist, seine Formel skalieren erhält mit dem Inhalt des `myNewFormula`.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Aktualisierungsintervall skalieren

Wie bei der Aktualisierung einer Formel skalieren dieselbe [EnableAutoScale] [ net_enableautoscale] Methode ändern des Intervalls Autoscale Bewertung eines vorhandenen Pools skalieren aktiviert. Wenn Sie beispielsweise bereits skalieren aktiviert ist 60 Minuten für einen Pool Autoscale Bewertung Intervall fest:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Auswerten einer Formel automatisch skalieren

Eine Formel können vor einem Pool zuweisen. So führen Sie eine "Testlauf" Formel auswerten von seinem bevor Sie die Formel in die Produktion anzeigen.

Zum Auswerten einer Formel skalieren müssen Sie zuerst die **Automatische Skalierung aktivieren** im Pool eine **gültige Formel**. Möchten Sie Testen einer Formel in einem Pool, die automatische Skalierung aktiviert noch nicht, können die einzeilige Formel `$TargetDedicated = 0` beim Aktivieren von Skalierung. Verwenden Sie dann eine der folgenden Formel auswerten zu testen:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) oder [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    Diese Stapelverarbeitung .NET Methoden erfordern die ID von einem vorhandenen Pool sein und skalieren Formel auswerten. Die Ergebnisse sind in der zurückgegebenen [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) -Instanz enthalten.

* [Automatische Skalierung Formel auswerten](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    Geben Sie in dieser Anforderung REST-API die Pool-ID im URI und skalieren Formel im *AutoScaleFormula* -Element des Anforderungstexts. Die Antwort des Vorgangs enthält alle Informationen, die mit der Formel verknüpft werden kann.

In diesem [Stapel .NET] [ net_api] Codeausschnitt bewerten wir eine Formel vor auf [CloudPool][net_cloudpool]. Wenn der Pool keine automatische Skalierung aktiviert haben, ermöglichen wir es zuerst.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Erfolgreichen Auswertung der Formel in diesem Codeausschnitt führt eine Ausgabe ähnlich der folgenden:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Informationen Sie über automatische Skalierung ausgeführt wird

Um sicherzustellen, dass die Formel erwartungsgemäß, sollten Sie regelmäßig die Ergebnisse der Skalierung "läuft" Stapel für den Pool ausführt. Einen Verweis auf den Pool also (oder aktualisieren) und überprüfen Sie die Eigenschaften der letzten skalieren ausgeführt.

In Batch .NET hat die [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) -Eigenschaft mehrere Eigenschaften, die Informationen über die neuesten automatischen Skalierung durchgeführt im Pool vom Batch-Dienst ausführen.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

In der REST-API gibt die [Informationen über eine Get](https://msdn.microsoft.com/library/dn820165.aspx) -Anforderung Informationen zu Pool, einschließlich der neuesten automatischen Skalierung Informationen im [AutoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun)ausgeführt.

Der folgende C#-Codeausschnitt verwendet die Stapelverarbeitung .NET Bibliothek um Informationen über die letzte automatische Skalierung Pool "MyPool" ausgeführt:

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Ausgabe des vorhergehenden Ausschnitts:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Skalierungsgröße Formeln

Betrachten wir nun einige Formeln, die verschiedene Methoden zum Anpassen des computeressourcen in einem Pool anzeigen.

### <a name="example-1-time-based-adjustment"></a>Beispiel 1: Zeitliche Anpassung

Vielleicht möchten Sie die Poolgröße basierend auf den Wochentag und die Tageszeit erhöhen oder verringern die Anzahl der Knoten im Pool entsprechend anpassen.

Diese Formel ruft zunächst die aktuelle Zeit. Wochentag (1-5) ist und innerhalb von Stunden (8 Uhr bis 6 Uhr) 20 Knoten Poolgröße Ziel fest. Andernfalls wird es mit 10 Knoten festgelegt.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Beispiel 2: Aufgabenbasierte Anpassung

In diesem Beispiel wird die Größe des Pools basierend auf der Anzahl der Aufgaben in der Warteschlange angepasst. Beachten Sie, dass Kommentare und Zeilenumbrüche in Formel Zeichenfolgen verwendet werden.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Beispiel 3: Kontoführung für parallele Aufgaben

Dies ist ein weiteres Beispiel, das basierend auf der Anzahl der Aufgaben Poolgröße passt. Diese Formel berücksichtigt [MaxTasksPerComputeNode] [ net_maxtasks] -Wert, der für den Pool eingestellt. Dies ist besonders nützlich in Situationen, wo das Pool [parallelen Taskausführung](batch-parallel-node-tasks.md) aktiviert wurde.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Beispiel 4: Festlegen einer anfänglichen Pool-Größe

Dieses Beispiel zeigt einen C#-Codeausschnitt mit einer Formel skalieren, die Größe des Pools auf eine bestimmte Anzahl von Knoten für einen anfänglichen Zeitraum fest. Dann die Poolgröße basierend auf der Anzahl der ausgeführten und aktive passt Aufgaben nach Ablauf des ersten Zeitraums.

Die Formel im folgenden Codeausschnitt:

- Legt die anfängliche Poolgröße vier Knoten.
- Die Poolgröße wird nicht innerhalb der ersten 10 Minuten des Pools Lebenszyklus angepasst werden.
- Nach 10 Minuten erhält der Maximalwert die Anzahl der laufenden als auch aktive Tasks innerhalb der letzten 60 Minuten.
  - Wenn beide 0 Werte (d. h., dass keine Vorgänge ausgeführt oder innerhalb der letzten 60 Minuten) ist die Größe des Pools auf 0 festgelegt.
  - Wenn entweder Wert größer als NULL ist, wird keine Änderung vorgenommen.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Nächste Schritte

* [Maximieren Azure Batch berechnen Ressourcenverwendung mit Aufgaben gleichzeitig Knoten](batch-parallel-node-tasks.md) enthält Informationen, wie Sie mehrere Aufgaben gleichzeitig auf den Compute-Knoten im Pool ausgeführt werden können. Neben Skalierung kann dieses Feature Auftragsdauer für einige Arbeitslasten sparen senken helfen.

* Ein anderes Effizienz Booster sicher, dass die Batch-Anwendung auf optimale Weise Batch-Dienst fragt. In [Azure Batch-Dienst effizient Abfragen](batch-efficient-list-queries.md)erfahren Sie die Datenmenge beschränken, die den Draht schneidet den Status der potenziell Tausenden von Compute-Knoten oder Aufgaben abzufragen.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
