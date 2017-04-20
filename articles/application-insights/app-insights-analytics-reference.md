<properties 
    pageTitle="Auf Analytics Anwendung Erkenntnisse | Microsoft Azure" 
    description="Referenz für Anweisungen Analytics mächtiges Werkzeug Anwendung Erkenntnisse. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>Referenz für die Analyse

[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion [Anwendung](app-insights-overview.md)Erkenntnisse. Diese Seiten beschreiben die Abfragesprache Analytics.

> [AZURE.NOTE] [Testen Analytics auf die simulierten Daten](https://analytics.applicationinsights.io/demo) Wenn Ihre Anwendung Daten noch Anwendung Erkenntnisse mitsendet.

## <a name="index"></a>Index


**Lassen Sie** [lassen Sie](#let-clause)


**Abfragen und Operatoren** [Anzahl](#count-operator)  |  [Auswerten](#evaluate-operator) | [Erweitern](#extend-operator) | [Join](#join-operator) | [Limit](#limit-operator) | [Mvexpand](#mvexpand-operator) | [analysiert](#parse-operator) | [Projekt](#project-operator) | [Projekt entfernt](#project-away-operator) | [Bereich](#range-operator) | [reduzieren](#reduce-operator) | [Rendern Richtlinie](#render-directive) | [restrict-Bedingung](#restrict-clause) | [Art](#sort-operator) | [zusammenfassen](#summarize-operator) | [nehmen](#take-operator) | [oben](#top-operator) | [oben geschachtelt](#top-nested-operator) | [Union](#union-operator) | [,](#where-operator) | [Where in](#where-in-operator)

**Aggregationen** [Alle](#any)  |  [Argmax](#argmax) | [Argmin](#argmin) | [Avg](#avg) | [Buildschema](#buildschema) | [Anzahl](#count) | [ZÄHLENWENN](#countif) | [Dcount](#dcount) | [Dcountif](#dcountif) | [Makelist](#makelist) | [Makeset](#makeset) | [maximale](#max) | [min](#min) | [Quantil](#percentile) | [Perzentile](#percentiles) | [Percentilesw](#percentilesw) | [Percentilew](#percentilew) | [Stdev](#stdev) | [Summe](#sum) | [Abweichung](#variance)

**Skalare** [Boolesche Literale](#boolean-literals)  |  [Boolesche Operatoren](#boolean-operators) | [Umwandlungen](#casts) | [Vergleiche skalare](#scalar-comparisons) | [Gettype](#gettype) | [Hash](#hash) | [iff](#iff) | [Isnotnull](#isnotnull) | [Isnull](#isnull) | [Notnull](#notnull) | [Toscalar](#toscalar)

**Zahlen** [Arithmetische Operatoren](#arithmetic-operators)  |  [Numerische Literale](#numeric-literals) | [abs](#abs) | [Bin](#bin) | [exp](#exp) | [Boden](#floor) | [Gamma](#gamma) | [Protokoll](#log) | [Rand](#rand) | [Wurzel](#sqrt) | [Todouble](#todouble) | [Toint](#toint) | [Tolong](#tolong)

**Datum und Uhrzeit** [Datum und Uhrzeit](#date-and-time-expressions)  |  [Datum und Zeitliterale](#date-and-time-literals) | [vor](#ago) | [Datepart](#datepart) | [Dayofmonth](#dayofmonth) | [Dayofweek](#dayofweek) | [Dayofyear](#dayofyear) | [Endofday](#endofday) | [Endofmonth](#endofmonth) | [Endofweek](#endofweek) | [namens](#endofyear) | [Getmonth](#getmonth) | [Getyear](#getyear) | [jetzt](#now) | [Startofday](#startofday) | [Startofmonth](#startofmonth) | [Startofweek](#startofweek) | [Startofyear](#startofyear) | [Todatetime](#todatetime) | [Totimespan](#totimespan) | [Weekofyear](#weekofyear)

**Zeichenfolge** [GUIDs](#guids)  |  [Verborgen Zeichenfolgenliterale](#obfuscated-string-literals) | [Zeichenfolgenliterale](#string-literals) | [Zeichenfolgenvergleiche](#string-comparisons) | [Countof](#countof) | [extrahieren](#extract) | [Isempty](#isempty) | [Isnotempty](#isnotempty) | [Notempty](#notempty)| [Parseurl](#parseurl) | [Ersetzen](#replace) | [Teilen](#split) | [Strcat](#strcat) | [Strlen](#strlen) | [Teilzeichenfolge](#substring) | [Tolower](#tolower) | [Toupper](#toupper)

**Arrays, Objekten und dynamisch** [Array- und Literale](#array-and-object-literals)  |  [Dynamisches Objektfunktionen](#dynamic-object-functions) | [dynamische Objekte können Klauseln](#dynamic-objects-in-let-clauses) | [JSON Pfad Ausdrücke](#json-path-expressions) | [Namen](#names) | [Arraylength](#arraylength) | [Extractjson](#extractjson) | [Parsejson](#parsejson) | [Bereich](#range) | [Todynamic](#todynamic) | [Treepath](#treepath)



## <a name="let"></a>Lassen Sie

### <a name="let-clause"></a>Let-Klausel

**Tabellarisch let - Benennung einer Tabelle**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Skalar let - Wert benennen**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda let - Benennung einer Funktion**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

Eine Let-Klausel bindet einen [Namen](#names) tabellarische Ergebnis, Skalarwert oder Funktion. Die Klausel ist ein Präfix einer Abfrage und die Bindung ist die Abfrage. (Lassen Sie nicht zu Namen Dinge, die Sie später in der Sitzung bereitstellen.)

**Syntax**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *Typ:* `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *Plain_query:* Eine Abfrage Let-Klausel nicht vorangestellt.

**Beispiele**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


Selbst-join:

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>Abfragen und Operatoren

Eine Abfrage über Ihre Telemetrie besteht aus eine Quelldatenstrom, gefolgt von einer Pipeline Filter. Zum Beispiel:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Jeder Filter das Pipezeichen vorangestellt `|` ist der *Operator*mit Parametern. Die Eingabe des Operators ist die Tabelle, die das Ergebnis der vorhergehenden Pipeline. In den meisten Fällen sind Parameter [skalaren Ausdrücken](#scalars) über die Spalten der Eingabe. In einigen Fällen Parameter sind die Namen der Eingabespalten und in einigen Fällen ist des Parameters eine zweite Tabelle. Das Ergebnis einer Abfrage ist immer eine Tabelle, auch wenn es nur eine Spalte und eine Zeile.

Abfragen können einzelne Zeilenumbrüche enthalten, aber durch eine Leerzeile beendet. Sie enthalten Kommentare zwischen `//` und Ende der Zeile.

Eine Abfrage kann eine oder mehrere [Klauseln lassen](#let-clause), vorangestellt werden die skalare, Tabellen oder Funktionen, die in der Abfrage definiert.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`Bei Abfragebeispiele wird zum Bezeichnen der obigen Tabelle Pipeline oder Quelle.
> 

### <a name="count-operator"></a>Count-operator

Die `count` Operator gibt die Anzahl der Datensätze (Zeilen) in der Eingabe aufzeichnen.

**Syntax**

    T | count

**Argumente**

* *T*: Daten in der Tabelle, deren Datensätze gezählt werden sollen.

**Zurück**

Diese Funktion gibt eine Tabelle mit einem einzelnen Datensatz und Spalte vom Typ `long`. Der Wert der einzige Zelle ist die Anzahl der Datensätze in *T*. 

**Beispiel**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>Operator auswerten

`evaluate`ist ein Erweiterungsmechanismus, der spezielle Algorithmen für Abfragen angefügt werden können.

`evaluate`muss der letzte Operator die Abfragepipeline (außer einer `render`). Es darf nicht in einem Funktionsrumpf erscheinen.

[Auswerten Autocluster](#evaluate-autocluster) | [Auswerten Warenkorb](#evaluate-basket) | [Auswerten Diffpatterns](#evaluate-diffpatterns) | [Extractcolumns bewerten](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>Autocluster bewerten

     T | evaluate autocluster()

AutoCluster allgemeine Muster der einzelnen Attribute (Dimensionen) in den Daten sucht und reduzieren die Ergebnisse der ursprünglichen Abfrage (ob 100 oder 100 k Zeilen ist) auf eine kleine Anzahl von Mustern. AutoCluster wurde entwickelt, um Fehler (Ausnahmen, Abstürze) analysieren, doch kann potenziell auf alle gefilterten arbeiten. 

**Syntax**

    T | evaluate autocluster( arguments )

**Zurück**

AutoCluster gibt eine (in der Regel klein) Muster, das Erfassen von Daten mit gemeinsamen Werte über mehrere diskrete Attribute zurück. Jedes Muster wird durch eine Zeile in die Ergebnisse dargestellt. 

Die ersten beiden Spalten sind Anzahl und Prozentsatz der Zeilen aus der ursprünglichen Abfrage, die mit dem Muster erfasst werden. Die verbleibenden Spalten aus der ursprünglichen Abfrage sind und ihr Wert einen bestimmten Wert aus der Spalte oder ' *' bedeutet Werte. 

Beachten Sie, dass das Muster nicht disjunkt sind: möglicherweise überlappen und in der Regel decken nicht alle ursprünglichen Zeilen. Einige Zeilen können nicht in ein Muster fallen.

**Tipps**

* Mit `where` und `project` in der eingegebenen Daten reduzieren, um nur was Sie interessiert.
* Wenn Sie eine interessante Zeile finden, möchten es Drilldown durch Hinzufügen der spezifischen Werte der `where` Filter.

**Argumente (optional)**

* `output=all | values | minimal` 

    Das Format der Ergebnisse. Anzahl und Prozent-Spalten werden immer in den Ergebnissen angezeigt. 

 * `all`-alle Spalten aus der Eingabe werden
 * `values`-Spalten mit Filter ' *' in den Ergebnissen
 * `minimal`-Filter auch Spalten für alle Zeilen in der ursprünglichen Abfrage identisch. 


* `min_percent=`*doppelte* (Standardwert: 1)

    Der Mindestprozentsatz Abdeckung der generierten Zeilen.

    Beispiel:`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*int* (Standard: 25) 

    Die Anzahl der Samen bestimmt die Anzahl der ersten lokalen Suche Punkte des Algorithmus. In einigen Fällen, abhängig von der Struktur der Daten erhöht die Anzahl der Samen der Anzahl oder Qualität der Ergebnisse durch verbesserte Suche Raum langsamer Abfrage Kompromiss. Das Argument Num_seeds hat punktbasierte Ergebnisse in beiden Richtungen 5 verringern vernachlässigt Leistungssteigerungen erzielen und erhöhen über 50 weitere Muster selten generiert.

    Beispiel:`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < double < 1*+ (Standard: 0,5)

    Gibt Ihnen Kontrolle über die Balance zwischen generischen (hoher Schutz) und informative (viele gemeinsame Werte). Steigende Size_weight in der Regel weniger Muster und jedes Muster tendenziell einen höheren Prozentsatz abdecken. Verringern Size_weight in der Regel erzeugt Weitere Muster kleiner Prozentsatz Abdeckung mit mehr Werte. Die Formel wird intern eine geometrische zwischen generischen normalisierte Bewertung und informative Bewertung mit Size_weight und 1 Size_weight als das Gewicht. 

    Beispiel:`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*column_name*

    Jede Zeile in der Eingabe angegebene Gewicht (in der Standardeinstellung hat jede Zeile eine Gewichtung von "1"), der eine Gewichtungsspalte werden häufig in Konto Sampling oder Buckets/Aggregation von Daten, die in jeder Zeile bereits eingebettet ist.

    Beispiel:`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>Warenkorb auswerten

     T | evaluate basket()

Warenkorb findet alle häufige Muster der einzelnen Attribute (Dimensionen) Daten und kehrt alle häufige Muster, die den Schwellenwert der Häufigkeit der ursprünglichen Abfrage übergeben. Warenkorb wird garantiert alle häufige Muster in den Daten aber nicht unbedingt polynomische Laufzeit. Zur Laufzeit der Abfrage kann wird in die Anzahl der Zeilen in einigen Fällen jedoch exponentiell in die Anzahl der Spalten (Dimensionen). Warenkorb basiert auf Zufallszahl Algorithmus ursprünglich für Warenkorb Analyse Datamining. 

**Zurück**

Alle Muster in mehr als einer angegebenen Bruchteil (Standard 0,05) Ereignisse angezeigt werden.

**Argumente (optional)**


* `threshold=`*0,015 < double < 1* (Standard: 0,05) 

    Legt das minimale Verhältnis von Zeilen gelten häufig (Muster mit kleineren nicht zurückgegeben).

    Beispiel:`T | evaluate basket("threshold=0.02")`


* `weight_column=`*column_name*

    Jede Zeile in der Eingabe angegebene Gewicht (in der Standardeinstellung hat jede Zeile eine Gewichtung von "1"), der eine Gewichtungsspalte werden häufig in Konto Sampling oder Buckets/Aggregation von Daten, die in jeder Zeile bereits eingebettet ist.

    Beispiel: T | Auswerten von basket("weight_column=sample_Count")


* `max_dims=`*1 < Int* (Standardwert: 5)

    Legt die Anzahl der Dimensionen nicht korrelierte pro Warenkorb beschränkt, die Abfrage Runtime verringert.


* `output=minimize` | `all` 

    Das Format der Ergebnisse. Anzahl und Prozent-Spalten werden immer in den Ergebnissen angezeigt.

 * `minimize`-Spalten mit Filter ' *' in den Ergebnissen.
 * `all`-alle Spalten aus der Eingabe werden ausgegeben.




#### <a name="evaluate-diffpatterns"></a>Auswerten von diffpatterns

     requests | evaluate diffpatterns("split=success")

Diffpatterns vergleicht zwei Daten dieselbe Struktur und sucht nach Mustern diskrete Attribute (Dimensionen), die Unterschiede zwischen den beiden Datensätzen zu kennzeichnen. Diffpatterns wurde entwickelt, um Fehler (z. B. durch Vergleichen von Fehler Fehler in einem bestimmten Zeitraum) analysieren jedoch möglicherweise Unterschiede zwei Datensätze mit derselben Struktur gefunden. 

**Syntax**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**Zurück**

Diffpatterns gibt einen (normalerweise kleinen) Satz von Mustern, die verschiedene Teile der Daten in zwei Gruppen (d. h. ein Muster ein großer Prozentsatz der Zeilen im ersten Datensatz und niedrigen Prozentsatz der Zeilen in der zweiten Aufnahme) erfassen. Jedes Muster wird durch eine Zeile in die Ergebnisse dargestellt.

Die ersten vier Spalten sind Anzahl und Prozentsatz der Zeilen aus der ursprünglichen Abfrage, die mit dem Muster in jedem Satz erfasst werden, in der fünften Spalte die Differenz (in absoluten Prozentpunkte) zwischen den beiden. Die verbleibenden Spalten aus der ursprünglichen Abfrage sind und ihr Wert einen bestimmten Wert aus der Spalte oder * Werte von Bedeutung. 

Beachten Sie, dass die Muster unterscheiden: möglicherweise überlappen und in der Regel decken nicht alle ursprünglichen Zeilen. Einige Zeilen können nicht in ein Muster fallen.

**Tipps**

* Verwenden der WHERE- und Projekt in der eingegebenen Daten reduzieren, um nur was Sie interessiert.

* Wenn Sie eine interessante Zeile finden, möchten Sie weiter hinzu, dass bestimmte Werte die Where Filtern Drilldown.

**Argumente**

* `split=`*Spaltenname* (erforderlich)

    Die Spalte muss genau zwei Werte haben. Erstellen Sie ggf. eine Spalte:

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*Zeichenfolge*

    Gibt der Algorithmus an, nur Muster die höheren Prozentsatz im Ziel-Dataset haben, das Ziel muss einer der beiden Werte der Spalte teilen.

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0,015 < double < 1* (Standard: 0,05) 

    Legt die minimale Muster (Verhältnis) Unterschied zwischen den beiden.

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    Das Format der Ergebnisse. Anzahl und Prozent-Spalten werden immer in den Ergebnissen angezeigt. 

 * `minimize`-Spalten mit Filter ' *' in den Ergebnissen
 * `all`-alle Spalten aus der Eingabe werden

* `weight_column=`*column_name*

    Jede Zeile in der Eingabe angegebene Gewicht (standardmäßig jede Zeile hat eine Gewichtung von "1"). Eine häufige Verwendung eine Gewichtungsspalte ist in Konto Sampling oder Buckets/Aggregation von Daten, die in jeder Zeile bereits eingebettet ist.

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>Auswerten von extractcolumns

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Extractcolumns wird verwendet, um eine Tabelle mit mehreren Spalten einfach gestalten, die dynamisch extrahiert aus (halb) strukturierte Spalte(n) basierend auf ihrem Typ. Unterstützt derzeit nur Spalten Json beide dynamische und Serialisierung Jsons.


* `max_columns=`*int* (Standard: 10) 

    Die Anzahl der neu hinzugefügten Spalten ist dynamisch und kann sehr groß (tatsächlich ist die Anzahl der unterschiedlichen Schlüssel alle Json-Datensätze) damit wir eingeschränkt werden muss. Neuen Spalten werden basierend auf der Häufigkeit in absteigender Reihenfolge sortiert und bis Max_columns zur Tabelle hinzugefügt werden.

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*doppelte* (Standard: 10.0) 

    Können neuen Spalten begrenzen, indem Sie Spalten ignorieren, deren Häufigkeit ist niedriger als Min_percent.

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*bool* (Standard: true) 

    True wird der Name der komplexen Spalte extrahierten Spalten Namen als Präfix hinzugefügt.


* `prefix_delimiter=`*Zeichenfolge* (Standard: "_") 

    Wenn Add_prefix = True dieser Parameter definiert das Trennzeichen, das verwendet wird, um den Namen der neuen Spalten verketten.

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*bool* (Standard: false) 

    True werden die ursprünglichen (Json) Spalten in der Ausgabetabelle gehalten.


* `output=query | table` 

    Das Format der Ergebnisse. 

 * `table`-Die Ausgabe wird dieselbe Tabelle angegebenen Eingabespalten + neue Spalten aus der Eingabespalten extrahiert wurden empfangen.
 * `query`-Die Ausgabe ist eine Zeichenfolge, die die Abfrage soll das Ergebnis als Tabelle darstellt. 




### <a name="extend-operator"></a>Operator erweitern

     T | extend duration = stopTime - startTime

Fügen Sie eine oder mehrere berechnete Spalten in eine Tabelle. 


**Syntax**

    T | extend ColumnName = Expression [, ...]

**Argumente**

* *T:* Die Eingabetabelle.
* *Spaltenname:* Der Name einer Spalte hinzufügen. [Groß-/Kleinschreibung und dürfen alphabetische,](#names) numerische und '_' Zeichen. Mit `['...']` oder `["..."]` Zitat Schlüsselwörter oder andere Zeichen.
* *Ausdruck:* Eine Berechnung über die vorhandenen Spalten.

**Zurück**

Eine Kopie der Eingabetabelle mit den angegebenen zusätzlichen Spalten.

**Tipps**

* Mit [`project`](#project-operator) hingegen möchten Sie auch löschen oder Umbenennen von einigen Spalten.
* Verwenden Sie keine `extend` um einen kürzeren Namen ein long-Ausdruck verwenden. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    Systemeigenen Spalten der Tabelle wurden indiziert. der neue Name definiert eine zusätzliche Spalte indiziert ist, die Abfrage wahrscheinlich langsamer ist.

**Beispiel**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>Join-operator

    Table1 | join (Table2) on CommonColumn

Übereinstimmende Werte der angegebenen Spalte zusammengeführt die Zeilen von zwei Tabellen.


**Syntax**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**Argumente**

* *Tabelle 1* - die "Links" der Verknüpfung.
* *Tabelle2* - 'rechts' der Verknüpfung. Geschachtelte Abfrage-Ausdrucks möglich, das eine Tabelle ausgibt.
* *CommonColumn* - eine Spalte mit dem gleichen Namen in beiden Tabellen.
* *Art* - gibt an, wie Zeilen aus den beiden Tabellen übereinstimmen.

**Zurück**

Eine Tabelle mit:

* Eine Spalte für jede Spalte in jeder der beiden Tabellen, einschließlich der übereinstimmenden Schlüsseln. Die Spalten auf der rechten Seite werden Namenskonflikte gibt automatisch umbenannt.
* Eine Zeile für jede Übereinstimmung zwischen den Tabellen. Eine Übereinstimmung wird eine Zeile in einer Tabelle mit demselben Wert für alle ausgewählten der `on` Felder in einer Zeile in der Tabelle. 

* `Kind`nicht angegeben

    Nur eine Zeile von Links für jeden Wert entspricht der `on` Schlüssel. Die Ausgabe enthält eine Zeile für jede Übereinstimmung mit dieser Zeile mit Zeilen aus der rechten Seite.

* `Kind=inner`
 
     In der Ausgabe für jede Kombination von übereinstimmenden Zeilen ist von links und rechts eine Zeile vorhanden.

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     Neben der inneren entspricht gibt eine Zeile für jede Zeile auf der linken (bzw. rechts) auch wenn keine Übereinstimmung vorliegt. In diesem Fall enthalten die Ausgabezellen nicht übereinstimmende NULL.

* `kind=leftanti`

     Gibt alle Datensätze von links, die nicht von rechts entspricht. Die Tabelle hat nur die Spalten auf der linken Seite. 
 
Wenn mehrere Zeilen mit denselben Werten für diese Felder sind, erhalten Sie Zeilen für alle Kombinationen.

**Tipps**

Für optimale Leistung:

* Mit `where` und `project` Verringerung der Anzahl von Zeilen und Spalten in der Tabelle Eingabe vor der `join`. 
* Wenn eine Tabelle immer kleiner ist als die andere ist, als (piped) links der Verknüpfung verwenden.
* Die Spalten für das Spiel beitreten müssen den gleichen Namen. Verwenden Sie den Projekt-Operator ggf. Umbenennen eine Spalte in einer Tabelle.

**Beispiel**

Get erweitert Aktivitäten aus einem Protokoll, in dem einige Einträge markieren den Anfang und Ende einer Aktivität. 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>Limit-operator

     T | limit 5

Gibt bis zur angegebenen Anzahl von Zeilen aus der Tabelle. Es gibt keine Garantie, welche Datensätze zurückgegeben werden. (Verwenden, um bestimmte Datensätze zurückzugeben, [`top`](#top-operator).)

**Alias**`take`

**Syntax**

    T | limit NumberOfRows


**Tipps**

`Take`ist eine einfache und effiziente Möglichkeit, sehen ein Beispiel für die Ergebnisse, wenn Sie interaktiv arbeiten. Beachten Sie, dass bestimmten Zeilen oder erstellen sie in einer bestimmten Reihenfolge nicht garantiert.

Gibt eine implizite Beschränkung für die Anzahl der Zeilen, die an den Client zurückgegeben, benutzen Sie `take`. Um diese Beschränkung aufzuheben, verwenden Sie die `notruncation` Client-Anforderung Option.



### <a name="mvexpand-operator"></a>Mvexpand-operator

    T | mvexpand listColumn 

Erweitert eine Liste aus einem dynamisch typisierte (JSON), so dass jeder Eintrag eine separate Zeile hat. Alle Zellen in einer erweiterten Zeile dupliziert werden. 

(Siehe auch [`summarize makelist`](#summarize-operator) das gegenteilige Funktion ausführt.)

**Beispiel**

Angenommen Sie, die Eingabetabelle ist:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Hello"|{"Key": "Wert"}|
|2|"World"|[0,1, "k", "V"]|

    mvexpand D

Ergebnis ist:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Hello"|{"Key": "Wert"}|
|2|"World"|0|
|2|"World"|1|
|2|"World"|"k"|
|2|"World"|"V"|


**Syntax**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumente**

* *Spaltenname:* Das Ergebnis sind Arrays in der benannten Spalte mehrere Zeilen erweitert. 
* *ArrayExpression:* Gibt ein Array Ausdruck. Wenn dieses Formular verwendet wird, eine neue Spalte hinzugefügt, und der beibehalten.
* *Name:* Einen Namen für die neue Spalte.
* *Typname:* Wandelt den erweiterten Ausdruck für einen bestimmten
* *RowLimit:* Die maximale Anzahl der Zeilen aus jeder ursprünglichen Zeile generiert. Der Standardwert ist 128.

**Zurück**

Mehrere Zeilen für die Werte in jedem Array die benannte Spalte oder im Arrayausdruck.

Erweiterte Spalte verfügt immer über dynamischen Typ. Verwenden einer `todatetime()` oder `toint()` Wenn berechnen oder Aggregieren von Werten.

Zwei Betriebsmodi Eigenschaftensammlung Erweiterungen werden unterstützt:

* `bagexpansion=bag`: Eigenschaftensammlungen sind in einzelnen Eintrag Eigenschaftensammlungen erweitert. Dies ist die Standard-Erweiterung.
* `bagexpansion=array`: Eigenschaftensammlungen sind in zwei Elementen erweitert `[` *Schlüssel*`,`*Wert* `]` array von Strukturen, einheitlichen Zugriff auf Schlüssel und Werte (sowie eine distinct Count-Aggregation beispielsweise über Namen ausführen). 

**Beispiele**


    exceptions | take 1 
  	| mvexpand details[0]

Zeilen für jeden Artikel im Feld Details teilt einen Ausnahmedatensatz.



### <a name="parse-operator"></a>Parse-operator

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

Werte extrahiert aus einer Zeichenfolge. Können einfach oder regulären Ausdrücken verwenden.

**Syntax**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**Argumente**

* `T`Die Eingabetabelle.
* `kind`: 
 * `simple`(Standard): die `Match` Zeichenfolgen sind einfache Zeichenfolgen.
 * `relaxed`: der Text nicht als den Typ einer Spalte analysieren die Spalte auf Null und die Analyse festgelegt wird fortgesetzt 
 * `regex`: die `Match` Zeichenfolgen sind reguläre Ausdrücke.
* `Text`Eine Spalte oder anderen Ausdruck, der ausgewertet und in eine Zeichenfolge konvertiert werden kann.
* *Übereinstimmung:* Der nächste Teil der Zeichenfolge und verwerfen.
* *Spalte:* In dieser Spalte wird den nächsten Teil der Zeichenfolge zuweisen. Die Spalte wird erstellt, wenn er nicht vorhanden ist.
* *Typ:* Analysieren den nächsten Teil der Zeichenfolge angegebenen Typ wie Int, date, double. 


**Zurück**

Die Eingabetabelle erweitert die Liste der Spalten.

Die Elemente in der `with` Klausel sind wiederum mit den Quelltext verglichen. Jedes Element kaut Sie einen Ausschnitt des Quelltextes: 

* Eine Literalzeichenfolge oder Ausdruck verschiebt den entsprechenden Cursor durch die Länge der Übereinstimmung.
* Ein regulärer Ausdruck können in eine Analyse Regex Minimierung Operator '?' so bald wie möglich die folgenden Übereinstimmung verschoben.
* Ein Spaltenname mit analysiert den angegebenen Typ. Wenn Art = entspannt nicht erfolgreich analysieren erklärt das gesamte Muster.
* Ein Spaltenname ein oder vom Typ "String" kopiert die Mindestanzahl von Zeichen zu folgenden Übereinstimmung.
* ' *' Überspringt die Mindestanzahl von Zeichen zu folgenden Übereinstimmung. Können Sie "*" am Anfang und Ende des Musters oder nach Typ als Zeichenfolge oder Zeichenfolge entspricht.

Alle Elemente in einem Muster analysieren müssen ordnungsgemäß übereinstimmen; Andernfalls werden keine Ergebnisse erstellt. Die Ausnahme von dieser Regel wird die Art = entspannt, fällt eine typisierte Variable zu analysieren, die restlichen die Analyse fortgesetzt.

**Beispiele**

*Einfach:*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | Zähler | vorhanden | Jahr
---|---|---|---
1 | 2 | SOCKS | 63

*Entspannt:*

Wenn die Eingabe eine vollständige Übereinstimmung für jede eingegebene Spalte enthält, erzeugt nicht strikte Analyse wie eine einfache Analyse. Aber eines typisierten Spalten korrekt analysieren nicht entspannt analysieren weiterhin den Rest des Musters, eine einfache Analyse beendet und alle Ergebnis nicht verarbeiten.


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | vorhanden | Jahr
---|---|---
1 |  SOCKS | 63


*Regex:*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

Ressource | Segment | Sperren | Freigeben | Vorherige
---|---|---|---|---
Planer | 16 | 02/17/2016 08:41:00 | 02/17/2016 08:41 | 2016-02-17T08:40:00Z

### <a name="project-operator"></a>Projekt-operator

    T | project cost=price*quantity, price

Wählen Sie die Spalten, umbenennen oder löschen und fügen Sie neue berechnete Spalten ein. Die Reihenfolge der Spalten im Resultset wird durch die Reihenfolge der Argumente angegeben. In den Argumenten angegebenen Spalten in der Ergebnismenge enthalten: andere Eingabe gelöscht.  (Siehe auch `extend`.)


**Syntax**

    T | project ColumnName [= Expression] [, ...]

**Argumente**

* *T:* Die Eingabetabelle.
* *Spaltenname:* Der Name einer Spalte in der Ausgabe angezeigt werden. Wenn kein *Ausdruck*vorhanden ist, muss eine Spalte mit diesem Namen in der Eingabe angezeigt. [Groß-/Kleinschreibung und dürfen alphabetische,](#names) numerische und '_' Zeichen. Mit `['...']` oder `["..."]` Zitat Schlüsselwörter oder andere Zeichen.
* *Ausdruck:* Optionale Skalarausdruck Eingabespalten verweisen. 

    Es ist zulässig, eine neue berechnete Spalte mit demselben Namen wie eine vorhandene Spalte in der Eingabe zurück.

**Zurück**

Eine Tabelle mit Spalten als Argumente und so viele Zeilen wie die Eingabetabelle.

**Beispiel**

Das folgende Beispiel zeigt verschiedene Manipulationen möglich mithilfe der `project` Operator. Der Tabelle `T` hat drei Spalten vom Typ `int`: `A`, `B`, und `C`. 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>Projekt entfernt operator

    T | project-away column1, column2, ...

Angegebene Spalten ausschließen. Das Ergebnis enthält alle Eingabespalten mit Ausnahme derjenigen, die Sie benennen.

### <a name="range-operator"></a>Bereichsoperator

    range LastWeek from ago(7d) to now() step 1d

Generiert eine einspaltige Tabelle mit Werten. Beachten Sie, dass es eine Pipelineeingabe nicht. 

|Letzte Woche|
|---|
|2015-12-05 09:10:04.627|
|2015-12-06-09:10:04.627|
|...|
|2015-12-12 09:10:04.627|



**Syntax**

    range ColumnName from Start to Stop step Step

**Argumente**

* *Spaltenname:* Der Name der einzelnen Spalte in der Ausgabetabelle.
* *Gestartet:* Der kleinste Wert in der Ausgabe.
* *Stop:* Der höchste Wert in die Ausgabe (oder eine Grenze der höchste Wert, dieser Wert tritt *Schritt* ) generiert.
* *Schritt:* Die Differenz zwischen zwei aufeinander folgenden Werten. 

Die Argumente müssen Zahlen-, Datums- oder Timespan-Werte sein. Sie können nicht die Spalten einer Tabelle verweisen. (Den Bereich einer Tabelle berechnet, verwenden Sie [Bereich *Funktion*](#range)vielleicht mit dem [Mvexpand-Operator](#mvexpand-operator).) 

**Zurück**

Eine Tabelle mit einer einzelnen Spalte namens *Spaltenname*, deren Werte *Starten*, *Starten Sie* + *Schritt*... bis zu und einschließlich *Beenden*.

**Beispiel**  

```AIQL
range Steps from 1 to 8 step 3
```

Eine Tabelle mit einer Spalte namens `Steps` vom Typ `long` und dessen Werte `1`, `4`, und `7`.

**Beispiel**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Eine Tabelle der Mitternacht vergangenen sieben Tage. Lagerplatz (Stock) Funktion reduziert jedes Mal zu Beginn des Tages.

**Beispiel**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Veranschaulicht, wie die `range` Operator kann zum Erstellen einer kleinen, Ad-hoc-der Dimensionstabelle dann Nullen einzuführen, in denen die Quelldaten keine Werte hat.

### <a name="reduce-operator"></a>Operator reduzieren

    exceptions | reduce by outerMessage

Versucht, ähnliche Datensätze zusammen gruppieren. Für jede Gruppe der Operator gibt die `Pattern` denkt am besten beschreibt, Gruppe und die `Count` der Datensätze in dieser Gruppe.


![](./media/app-insights-analytics-reference/reduce.png)

**Syntax**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumente**

* *Spaltenname:* Die Spalte zu. Es muss vom Typ String sein.
* *Schwellenwert:* Ein Wert im Bereich {von 0.. 1}. Standardwert ist 0,001. Bei großen Eingaben sollten Schwellenwert klein sein. 

**Zurück**

Zwei Spalten, `Pattern` und `Count`. In vielen Fällen werden Muster eine vollständige Wert aus der Spalte. In einigen Fällen können Begriffe identifizieren und Variable Teile mit ' *'.

Beispiel: das Ergebnis des `reduce by city` gehören: 

|Muster | Anzahl |
|---|---|
| SAN * | 5182 |
| St. * | 2846 |
| Moskau | 3726 |
| \*-auf-\* | 2730 |
| Paris | 27163 |


### <a name="render-directive"></a>Richtlinie Rendern

    T | render [ table | timechart  | barchart | piechart ]

Rendern weist der Darstellungsschicht die Tabelle anzeigen. Es sollte das letzte Element der Pipe. Es stellt eine praktische Alternative, mit der Anzeige können Sie eine Abfrage mit einer bestimmten Präsentation speichern.

### <a name="restrict-clause"></a>Restrict-Bedingung 

Gibt den Satz der verfügbaren Operatoren führen Sie Tabellennamen. Zum Beispiel:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>Sort-operator 

    T | sort by country asc, price desc

Sortieren Sie die Zeilen der Tabelle in Reihenfolge von einer oder mehreren Spalten.

**Alias**`order`

**Syntax**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumente**

* *T:* Die Tabelle Eingabe sortiert.
* *Spalte:* Spalte *t* , sortieren. Der Typ der Werte muss numerisch, Datum, Uhrzeit oder Zeichenfolge.
* `asc`Sortieren in aufsteigender Reihenfolge niedrig bis hoch. Der Standardwert ist `desc`, absteigend hoch zu niedrig.

**Beispiel**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Alle Zeilen in Tabelle Spuren, die einen bestimmten `ActivityId`, sortiert nach deren Zeitstempel.

### <a name="summarize-operator"></a>Operator zusammenfassen

Erstellt eine Tabelle, die den Inhalt der Tabelle aggregiert.
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Eine Tabelle mit der Anzahl, durchschnittliche Anforderungsdauer und Städte in jedem Land. Eine Zeile ist in der Ausgabe für jedes Land unterschiedliche vorhanden. Die Ausgabespalten zeigen den Anzahl der durchschnittlichen Dauer, und Land. Andere Eingabespalten werden ignoriert.


    T | summarize count() by price_range=bin(price, 10.0)

Eine Tabelle, die zeigt, wie viele Elemente Preise in jedem Intervall [0,10.0] [10.0,20.0] und so weiter. In diesem Beispiel verfügt über eine Spalte für die Anzahl und eine für den Preis. Andere Eingabespalten werden ignoriert.


**Syntax**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumente**

* *Spalte:* Optionaler Name für eine Ergebnisspalte. Standardmäßig ein abgeleitet vom Ausdruck. [Groß-/Kleinschreibung und dürfen alphabetische,](#names) numerische und '_' Zeichen. Mit `['...']` oder `["..."]` Zitat Schlüsselwörter oder andere Zeichen.
* *Aggregation:* Ein Aufruf einer Aggregatfunktion wie `count()` oder `avg()`, Spaltennamen als Argumente. [Aggregationen](#aggregations)anzeigen
* *GroupExpression:* Ausdruck für Spalten, die eine Reihe von unterschiedlichen Werten bereitstellt. In der Regel wird eine Spaltennamen, die bereits einer beschränkten Anzahl von Werten, bietet oder `bin()` mit einem numerischen oder Zeit als Argument. 

Wenn Sie einen numerischen oder Zeit Ausdruck ohne Bereitstellen `bin()`, Analytics gilt es automatisch mit einem Intervall von `1h` Zeit oder `1.0` für Zahlen.

Wenn Sie keinen *GroupExpression,* wird die gesamte Tabelle in einer einzelnen Ausgabezeile zusammengefasst.



**Zurück**

Eingabezeilen werden angeordnet in Gruppen die gleichen Werte der `by` Ausdrücke. Die angegebenen Aggregationsfunktionen sind über jeder Gruppe erzeugt eine Zeile für jede Gruppe berechnet. Das Ergebnis enthält die `by` Spalten und auch mindestens eine Spalte für jedes Aggregat berechnet. (Einige Aggregationsfunktionen zurückgeben mehrerer Spalten.)

Das Ergebnis hat verschiedene Kombinationen von Zeilen `by` Werte. In Bereichen mit numerischen Werten zusammenfassen, verwenden Sie `bin()` Bereiche auf diskrete Werte reduzieren.

**Hinweis**

Obwohl Sie beliebige Ausdrücke für die Aggregation und die Gruppierungsausdrücke bereitstellen können, ist effizienter einfache Spaltennamen oder anwenden `bin()` einer numerischen Spalte.



### <a name="take-operator"></a>Operator übernehmen

Alias der [Grenze](#limit-operator)


### <a name="top-operator"></a>Top-operator

    T | top 5 by Name desc nulls first

Gibt die ersten *N* Datensätze durch die angegebenen Spalten sortiert.


**Syntax**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**Argumente**

* *NumberOfRows:* Die Anzahl der Zeilen von *T* zurück.
* *Sort_expression:* Ein Ausdruck, um die Zeilen zu sortieren. Es ist normalerweise nur ein Spaltenname. Sie können mehrere Sort_expression angeben.
* `asc`oder `desc` (Standard) scheinbar Steuerelement ob Auswahl tatsächlich aus dem "Bottom" oder "oben" an.
* `nulls first`oder `nulls last` , null-Werte erscheinen. `First`ist die Standardeinstellung für `asc`, `last` ist der Standardwert für `desc`.


**Tipps**

`top 5 by name`Oberflächlich entspricht `sort by name | take 5`. Allerdings schneller und immer gibt Ergebnisse sortiert, während `take` übernimmt keine Garantie.

### <a name="top-nested-operator"></a>Operator Top geschachtelt

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

Ergebnisse hierarchische, wobei jede Ebene einen Drilldown zur vorherigen Ebene ist. Es eignet sich für die Beantwortung von Fragen wie "Was sind die Top 5 Anfragen für jede, was 3 Spitzenleistung Buckets sind und für alle die Top 3 Ländern Anfragen kommen aus?"

**Syntax**

   T | oben geschachtelt der Spalte N durch AGGREGATION [,...]

**Argumente**

* N:int - Anzahl Zeilen zurück oder auf Weiter. In einer Abfrage mit drei Ebenen, wobei N 5, 3 und 3 ist, werden Zeilen insgesamt 45.
* Spalte - Spalte auf Gruppieren nach für die Aggregation. 
* AGGREGATION - eine [Aggregatfunktion](#aggregations) , die für jede Gruppe von Zeilen gelten. Die Ergebnisse dieser Aggregationen bestimmt die oberen Gruppen angezeigt werden.


### <a name="union-operator"></a>Union-operator

     Table1 | union Table2, Table3

Zwei oder mehr Tabellen und gibt alle Zeilen. 

**Syntax**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumente**

* *Tabelle1*, *Tabelle2* ...
 *  Der Name einer Tabelle, wie `requests`, oder eine Tabelle in einer [let-Klausel](#let-clause). oder
 *  Ein Abfrageausdruck wie`(requests | where success=="True")`
 *  Eine Reihe von Tabellen mit Platzhalterzeichen angegeben. Beispielsweise `e*` bilden die Union aller Tabellen, die in vorherigen dürfen Klauseln, deren Name mit "e" mit der Ausnahmentabelle 'begann' definiert.
* `kind`: 
 * `inner`-Das Ergebnis hat die Teilmenge der Spalten der Tabellen gemeinsam.
 * `outer`-Das Ergebnis hat die Spalten, die auftreten eines Eingaben. Zellen, die nicht durch eine Eingabezeile definierten sollen `null`.
* `withsource=`*Spaltenname:* Wenn angegeben, enthalten die Ausgabe eine Spalte namens *Spaltenname* , dessen Wert gibt die Quelltabelle jede Zeile beigetragen hat.

**Zurück**

Sind so viele Zeilen wie in alle Eingabetabellen und so viele Spalten wie sind eindeutig die Eingaben.

**Beispiel**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
Gesamtmenge aller Tabellen, deren Namen mit "Tt" beginnen.


**Beispiel**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
Die Anzahl der verschiedenen Benutzer, die entweder erzeugt eine `exceptions` Ereignis oder `traces` Ereignis am letzten Tag. Im Ergebnis wird der Spalte 'SourceTable' "Abfrage" und "Befehl" angegeben.

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

Hierbei effizienter zu demselben Ergebnis. Jede Tabelle filtern vor der Union.

### <a name="where-operator"></a>dem Operator

     requests | where resultCode==200

Filtert eine Tabelle eine Teilmenge der Zeilen, die ein Prädikat erfüllen.

**Alias**`filter`

**Syntax**

    T | where Predicate

**Argumente**

* *T:* Tabellarische Eingabe, deren Datensätze gefiltert werden sollen.
* *Prädikat:* Ein `boolean` [Ausdruck](#boolean) über die Spalten der *T*. Für jede Zeile im *T*wird ausgewertet.

**Zurück**

Zeilen in *T* für das *Prädikat* `true`.

**Tipps**

Die schnellste Leistung zu erhalten:

* **Verwenden einfache Vergleiche** zwischen Spaltennamen und Konstanten. (Konstante 'Konstante' Tabelle - bedeutet, `now()` und `ago()` sind OK und so Skalare Werte werden mit einem [ `let` Klausel](#let-clause).)

    Beispielsweise `where Timestamp >= ago(1d)` , `where floor(Timestamp, 1d) == ago(1d)`.

* **Einfachste erste Begriffe**: haben Sie mehrere Klauseln verbunden mit `and`, zuerst die Klauseln, die nur eine Spalte umfassen. So `Timestamp > ago(1d) and OpId == EventId` besser umgekehrt.


**Beispiel**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Datensätze, die nicht älter als 1 Stunde und der Quelle "Plumplori" aufgerufen und zwei Spalten mit den gleichen Wert haben. 

Beachten Sie, dass wir den Vergleich zwischen zwei Spalten, den Index nicht nutzen und erzwingt eine setzen.


### <a name="where-in-operator"></a>WHERE-operator

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**Syntax**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**Argumente**

* `col`Eine Spalte in der Tabelle.
* `expr1`...: Eine Liste von skalaren Ausdrücken.

Mit `in` wird verwendet, um nur Zeilen mit einzubeziehen `col` entspricht eine Ausdrücke `expr1...`.

Mit `!in` , in der nur Zeilen einschließen `col` entspricht keiner der Ausdrücke `expr1...`.  


## <a name="aggregations"></a>Aggregationen

Aggregationen sind Funktionen kombinieren Werte in Gruppen [zusammenfassen Vorgang](#summarize-operator)erstellt. In dieser Abfrage ist beispielsweise eine Aggregatfunktion dcount():

    requests | summarize dcount(name) by success

### <a name="any"></a>alle 

    any(Expression)

Nach dem Zufallsprinzip wählt eine Zeile der Gruppe und gibt den Wert des angegebenen Ausdrucks zurück.

Dies empfiehlt sich beispielsweise, wenn eine Spalte eine große Anzahl von ähnlichen Werten (z. B. eine Spalte "Fehlertext") und Sie diese Spalte einen eindeutigen Wert der zusammengesetzten Schlüssel einmal aufnehmen möchten. 

**Beispiel**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>Argmin argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

Sucht eine Zeile in die Gruppe, die *ExprToMaximize*minimiert/maximiert und gibt den Wert des *ExprToReturn* (oder `*` um die gesamte Zeile zurückzugeben).

**Tipp**: übergeben durch Spalten werden automatisch umbenannt. Um sicherzustellen, dass Sie die richtigen Namen verwenden, überprüfen Sie die Ergebnisse mit `take 5` , bevor die Ergebnisse in einem anderen Operator pipe.

**Beispiele**

Zeigen Sie für jeden Anforderungsnamen Wenn die längste Anforderung aufgetreten an:

    requests | summarize argmax(duration, timestamp) by name

Alle Details der längsten Anforderung, nicht nur der Zeitstempel anzeigen:

    requests | summarize argmax(duration, *) by name


Den niedrigsten Wert der jede Metrik der Zeitstempel und andere Daten zu finden:

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>Durchschn.

    avg(Expression)

Berechnet den Mittelwert des *Ausdrucks* in der Gruppe.

### <a name="buildschema"></a>Buildschema

    buildschema(DynamicExpression)

Gibt das minimale Schema, das alle Werte der *DynamicExpression*gibt. 

Spalte Parametertyp sollte `dynamic` – ein Array oder Eigenschaft Tasche. 

**Beispiel**

    exceptions | summarize buildschema(details)

Ergebnis:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Beachten Sie, dass `indexer` markiert, numerischen Index verwenden soll. Für dieses Schema einige gültige Pfade würde (vorausgesetzt diese Beispiel Indizes sind im Bereich):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Beispiel**

Angenommen Sie die Eingabespalte weist drei dynamische Werte:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


Das resultierende Schema wäre:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

Das Schema sagt uns, dass:

* Das Stammobjekt ist ein Container mit vier Eigenschaften X, y, Z und t.
* Die Eigenschaft mit dem Namen "X", die vom Typ "Int" oder vom Typ "String" sein.
* Die Eigenschaft namens "y", die entweder vom Typ "double" oder einen anderen Container eine Eigenschaft namens "w" vom Typ "String".
* Die ``indexer`` Schlüsselwort Gibt an, dass "Z" und "t" Arrays.
* Jedes Element im Array "Z" ist Int oder eine Zeichenfolge.
* "t" ist ein Array von Zeichenfolgen.
* Jede Eigenschaft ist implizit optional und jedem Array ist möglicherweise leer.

##### <a name="schema-model"></a>Schemamodell

Die Syntax der zurückgegebenen Schema ist:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

Sie sind Teil der Schreibmaschine Typangaben als einen dynamischen Wert entspricht. Mit Schreibmaschine wäre das Beispielschema:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>Anzahl

    count([ Predicate ])

Gibt die Anzahl der Zeilen für das *Prädikat* ergibt `true`. Wenn keine- *Prädikat* angegeben, gibt die Gesamtzahl der Datensätze in der Gruppe. 

**Perf-Tipp**: Verwenden Sie `summarize count(filter)` anstelle von`where filter | summarize count()`

> [AZURE.NOTE] Vermeiden Sie count() zum Ermitteln der Anzahl von Anfragen, Ausnahmen oder andere Ereignisse. Die Anzahl der Datenpunkte in Application Insights beibehalten werden kleiner als die Anzahl der ursprünglichen Ereignisse bei der [Probenahme](app-insights-sampling.md) in Betrieb ist. Verwenden Sie stattdessen `summarize sum(itemCount)...`. Die ItemCount-Eigenschaft gibt die Anzahl der ursprünglichen Ereignisse beibehalten Datenpunkte dargestellt werden.

### <a name="countif"></a>ZÄHLENWENN

    countif(Predicate)

Gibt die Anzahl der Zeilen für das *Prädikat* ergibt `true`.

**Perf-Tipp**: Verwenden Sie `summarize countif(filter)` anstelle von`where filter | summarize count()`

> [AZURE.NOTE] Verwenden Sie countif() zum Ermitteln der Anzahl von Anfragen, Ausnahmen oder andere Ereignisse. Die Anzahl der Datenpunkte werden kleiner als die Anzahl der tatsächlichen bei der [Probenahme](app-insights-sampling.md) in Betrieb ist. Verwenden Sie stattdessen `summarize sum(itemCount)...`. Die ItemCount-Eigenschaft gibt die Anzahl der ursprünglichen Ereignisse beibehalten Datenpunkte dargestellt werden.

### <a name="dcount"></a>DCount

    dcount( Expression [ ,  Accuracy ])

Gibt eine Schätzung der Anzahl eindeutiger Werte von *Ausdruck* in der Gruppe. (Verwenden Sie zum Auflisten der unterschiedlichen Werte [`makeset`](#makeset).)

*Genauigkeit*angegeben, steuert der Kompromiss zwischen Geschwindigkeit und Genauigkeit.

 * `0`= mindestens präzise und schnellste Berechnung.
 * `1`Die Standardeinstellung, die Genauigkeit und die Berechnung Salden; ca. 0,8 % Fehler.
 * `2`die genaue und langsamste Berechnung =; etwa 0,4 % Fehler.

**Beispiel**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

Gibt eine Schätzung der Anzahl eindeutiger Werte von *Ausdruck* der Zeilen in der Gruppe für die *Prädikat* true ist. (Verwenden Sie zum Auflisten der unterschiedlichen Werte [`makeset`](#makeset).)

*Genauigkeit*angegeben, steuert der Kompromiss zwischen Geschwindigkeit und Genauigkeit.

 * `0`= mindestens präzise und schnellste Berechnung.
 * `1`Die Standardeinstellung, die Genauigkeit und die Berechnung Salden; ca. 0,8 % Fehler.
 * `2`die genaue und langsamste Berechnung =; etwa 0,4 % Fehler.

**Beispiel**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>MakeList

    makelist(Expr [ ,  MaxListSize ] )

Gibt eine `dynamic` (JSON)-Array mit den Werten der *Ausdruck* in der Gruppe. 

* *MaxListSize* ist eine optionale Ganzzahl Beschränkung für die maximale Anzahl der zurückgegebenen Elemente (Standardwert ist *128*).

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

Gibt eine `dynamic` (JSON) Array Reihe von unterschiedlichen Werten *Ausdruck* in der Gruppe verwendet. (Tipp: verwenden, um nur unterschiedliche Werte zählen [`dcount`](#dcount).)
  
*  *MaxSetSize* ist eine optionale Ganzzahl Beschränkung für die maximale Anzahl der zurückgegebenen Elemente (Standardwert ist *128*).

**Beispiel**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

Siehe auch die [ `mvexpand` Operator](#mvexpand-operator) für die gegenteilige Funktion.


### <a name="max-min"></a>Max, min

    max(Expr)

Berechnet die maximale *Ausdruck*.
    
    min(Expr)

Das kleinste *Ausdruck*berechnet.

**Tipp**: diese Ihnen min oder Max selbst - beispielsweise die oder niedrigster Preis. Aber andere Spalten in der Zeile - z. B. der Name des Lieferanten mit dem niedrigsten Preis - verwenden Sie [Argmin oder Argmax](#argmin-argmax).


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>Quantil Perzentile, Percentilew, percentilesw

    percentile(Expression, Percentile)

Gibt eine Schätzung für *Ausdruck* angegebenen prozentualen in der Gruppe. Die Genauigkeit hängt die Dichte der Bevölkerung im Bereich der Quantil.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

Wie `percentile()`, aber eine Anzahl der Prozentwerte (schneller als jede Quantil einzeln berechnen) berechnet.

    percentilew(Expression, WeightExpression, Percentile)

Gewichtete Quantil. Verwenden Sie diese vorab aggregierten Daten.  `WeightExpression`ist eine Ganzzahl, die angibt, ursprüngliche Zeilen durch jede aggregierten Zeile dargestellt werden.

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

Wie `percentilew()`, aber eine prozentuale Werte berechnet.

**Beispiele**


Der Wert der `duration` mehr als 95 % der festgelegt und kleiner als 5 % der Samplesatz für jeden Anforderung berechnet wird:

    request 
  	| summarize percentile(duration, 95)
      by name

Auslassen "von..." für die gesamte Tabelle berechnet.

Berechnen Sie gleichzeitig mehrere Perzentile für andere Anforderung Namen:

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

Die Ergebnisse zeigen, dass für die Anforderung /Events/Index, 5 % der Anfragen werden beantwortet in weniger als 2.44s, Hälfte in 3.52s, und 5 % langsamer als 6.85s.

Berechnen Sie mehrere Statistiken:

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>Gewichtete Perzentile

Verwenden Sie gewichtete Quantil in Fällen, in denen die Daten vorab aggregiert wurde. 

Angenommen Sie, Ihre Anwendung führt Tausende von Vorgängen pro Sekunde und die Wartezeit möchten. Die einfache Lösung wäre eine Anwendung Einblicke Anforderung oder ein benutzerdefiniertes Ereignis für jeden Vorgang generieren. Dies würde viel Verkehr, obwohl adaptive Sampling senken wirksam wird. Aber eine noch bessere Lösung implementieren möchten: Schreiben Sie Code in Ihrer Anwendung vor dem Senden an Application Insights aggregiert. Zusammenfassung die aggregierte werden regelmäßig reduziert die Datenrate vielleicht einige Punkte pro Minute gesendet.

Der Code erhält einen Stream Maßeinheiten Wartezeit in Millisekunden. Zum Beispiel:
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

Er zählt die Maße in folgenden Lagerplätze:`{ 10, 20, 30, 40, 50, 100 }`

In regelmäßigen Abständen stellt eine Reihe von TrackEvent Aufrufe für jede Gruppe mit benutzerdefinierten Aufruf: 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

Analytics sehen Sie eine solche Gruppe von Ereignissen wie folgt:

`opCount` | `latency`| Bedeutung
---|---|---
8 | 10 | = 8 Einsatzweisen 10ms Lagerplatz
6 | 20 | = 6 Vorgänge am Lagerplatz 20 ms
3 | 30 | = Vorgänge am Lagerplatz 30ms 3
1 | 40 | = 1 Vorgänge 40ms Lagerplatz

Um ein genaues Bild der ursprünglichen Verteilung Ereignis Wartezeiten abzurufen, verwenden wir `percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

Das Ergebnis ist identisch, als hätten wir nur verwendet `percentiles` auf die ursprünglichen Maße.

> [AZURE.NOTE] Gewichtete Perzentile gelten nicht für [abgetastete Daten](app-insights-sampling.md), wobei jede Zeile aufgenommenen Stichprobe der ursprünglichen anstelle eines Lagerplatzes darstellt. Einfache Quantil-Funktionen eignen sich für die abgetasteten Daten.

#### <a name="estimation-error-in-percentiles"></a>Fehler in Perzentile Vorkalkulation

Perzentile Aggregat enthält einen ungefähren Wert mit [T-Digest](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf). 

Die folgenden wichtigen Punkte: 

* Die Grenzen für die Vorkalkulation Fehler je nach Wert der angeforderten Quantil. Die größte Genauigkeit wird Ende [0 bis 100] skalieren Perzentile 0 und 100 sind genaue Mindest- und Höchstwerten der Verteilung. Die Genauigkeit nimmt in die Mitte der Skala. Es ist den Median und auf 1 % begrenzt. 
* Fehler Grenzen werden auf Rang nicht beobachtet. Quantil (X 50) angenommen Xm Wert zurückgegeben. Die Vorkalkulation garantiert 49 % und höchstens 51 % der Werte von X kleiner als Xm. Differenz Xm und mittlere Istwert X ist theoretisch unbegrenzt.

### <a name="stdev"></a>STABW

     stdev(Expr)

Gibt die Standardabweichung der *Ausdruck* für die Gruppe.

### <a name="variance"></a>Abweichung

    variance(Expr)

Gibt die Varianz der *Ausdruck* für die Gruppe.

### <a name="sum"></a>Summe

    sum(Expr)

Gibt die Summe der *Ausdruck* für die Gruppe.                      


## <a name="scalars"></a>Skalare

[Umwandlungen](#casts) | [Vergleichen](#scalar-comparisons)
<br/>
[GetType](#gettype) | [Hash](#hash) | [iff](#iff) |  [Isnull](#isnull) | [Isnotnull](#isnotnull) | [Notnull](#notnull) | [Toscalar](#toscalar)

Die unterstützten Typen sind:

| Typ      | Weitere Namen   | Gleichwertiges .NET |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>Umwandlungen

Sie können von einem Typ in einen anderen umwandeln. Im Allgemeinen die Konvertierung sinnvoll, funktioniert es:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

Überprüfen Sie, ob eine Zeichenfolge in einen bestimmten Typ konvertiert werden kann:

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>Skalare Vergleiche

||
---|---
`<` |Weniger
`<=`|Kleiner oder gleich
`>` |Größer
`>=`|Größer oder gleich
`<>`|Ungleich
`!=`|Ungleich 
`in`| Rechter Operand (dynamische) Arrays und linken Operanden gleich auf eines seiner Elemente.
`!in`| Rechte Operand (dynamische) Arrays und linke Operand entspricht nicht der Elemente.




### <a name="gettype"></a>GetType

**Zurück**

Eine Zeichenfolge, die zugrunde liegenden Speichertyp einziges Argument. Dies ist besonders hilfreich, wenn der Art Werte `dynamic`: in diesem Fall `gettype()` codieren Wert zeigen.

**Beispiele**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>Hash

**Syntax**

    hash(source [, mod])

**Argumente**

* *Quelle*: Quelle skalaren der Hash wird berechnet.
* *Mod*: die modulo-Wert auf den Hashwert.

**Zurück**

Der Xxhash (Long) Wert des angegebenen Skalar modulo angegebenen mod Wert (falls angegeben).

**Beispiele**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>IFF

Die `iff()` -Funktion wertet das erste Argument (Prädikat) und gibt den Wert der zweiten oder dritten Argumente davon ab, ob das Prädikat `true` oder `false`. Das zweite und dritte Argument müssen vom gleichen Typ sein.

**Syntax**

    iff(predicate, ifTrue, ifFalse)


**Argumente**

* *Prädikat:* Ein Ausdruck, der den Wert einer `boolean` Wert.
* *IfTrue:* Ein Ausdruck, der ausgewertet und der Wert von der Funktion zurückgegeben Wenn *Prädikat* ergibt `true`.
* *IfFalse:* Ein Ausdruck, der ausgewertet und der Wert von der Funktion zurückgegeben Wenn *Prädikat* ergibt `false`.

**Zurück**

Diese Funktion gibt den Wert der *IfTrue* ergibt *Prädikat* `true`, oder der Wert der *IfFalse* sonst.

**Beispiel**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>IsNull, Isnotnull, NotNull.

    isnull(parsejson("")) == true

Akzeptiert ein einzelnes Argument und gibt an, ob null ist.

**Syntax**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Zurück**

WAHR oder falsch, je nachdem, ob ist der Wert null oder nicht null.


|x|IsNull(x)
|---|---
| "" | falsch
|"X" | falsch
|parseJSON("")|true
|parseJSON("[]")|falsch
|parseJSON("{}")|falsch

**Beispiel**

    T | where isnotnull(PossiblyNull) | count

Beachten Sie, dass diese Wirkung anders:

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

Wertet eine Abfrage oder einen Ausdruck und gibt das Ergebnis als einzelnen Wert zurück. Diese Funktion ist nützlich für die bereitgestellte Berechnung; Beispielsweise berechnet die Gesamtzahl Ereignisse und verwenden, die als Grundlage.

**Syntax**

    toscalar(query)
    toscalar(scalar)

**Zurück**

Das Argument ausgewertet. Wenn das Argument eine Tabelle ist, gibt die erste Spalte der ersten Zeile. (Empfehlenswert ist das Argument hat nur eine Spalte und Zeile anordnen.)

**Beispiel**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>Boolesche Literale

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>Boolesche Operatoren

    and 
    or 

    

## <a name="numbers"></a>Zahlen

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>Numerische Literale

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>Arithmetische Operatoren

|| |
|---|-------------|
| + | Hinzufügen         |
| - | Subtrahieren    |
| * | Multiplizieren    |
| / | Teilen      |
| % | Modulo      |
||
|`<` |Weniger
|`<=`|Kleiner oder gleich
|`>` |Größer
|`>=`|Größer oder gleich
|`<>`|Ungleich
|`!=`|Ungleich 


### <a name="abs"></a>Abs

**Syntax**

    abs(x)

**Argumente**

* X-Integer, Real oder timespan

**Zurück**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>Lagerplatz, floor

-Werte auf eine Ganzzahl gerundet mehreren bestimmten Lagerplatz Größe. Verwendet eine Menge in die [`summarize by`](#summarize-operator) Abfrage. Verstreute Wertemenge verfügen, werden sie in eine geringere Anzahl an bestimmte Werte gruppiert.

Alias `floor`.

**Syntax**

     bin(value, roundTo)
     floor(value, roundTo)

**Argumente**

* *Wert:* Eine Zahl, Datum oder Zeitraum. 
* *RoundTo:* "Bin Größe." Zahl, Datum oder Zeitraum, der *Wert*dividiert. 

**Zurück**

Das nächste Vielfache des *RoundTo* unter *Wert*.  
 
    (toint((value/roundTo)-0.5)) * roundTo

**Beispiele**

Ausdruck | Ergebnis
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


Der folgende Ausdruck berechnet ein Histogramm der Dauer mit Bucket 1 Sekunde:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>EXP

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>Boden

Ein Alias für [`bin()`](#bin).

### <a name="gamma"></a>Gamma

Die [Gammafunktion](https://en.wikipedia.org/wiki/Gamma_function)

**Syntax**

    gamma(x)

**Argumente**

* *x:* Eine reelle Zahl

Positive Ganzzahlen `gamma(x) == (x-1)!` z. B. `gamma(5) == 4 * 3 * 2 * 1`.

Siehe auch [Loggamma](#loggamma).


### <a name="log"></a>Protokoll

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`eine Zahl > 0 muss sein. Andernfalls wird Null zurückgegeben.

### <a name="loggamma"></a>loggamma


Der natürliche Logarithmus des absoluten Werts der [Gammafunktion](#gamma).

**Syntax**

    loggamma(x)

**Argumente**

* *x:* Eine reelle Zahl


### <a name="rand"></a>Rand

Einen Zufallszahlen-Generator.

* `rand()`-eine reelle Zahl zwischen 0,0 und 1,0
* `rand(n)`-eine ganze Zahl zwischen 0 und n-1




### <a name="sqrt"></a>Wurzel

Die Funktion WURZEL.  

**Syntax**

    sqrt(x)

**Argumente**

* *x:* Eine reelle Zahl > = 0.

**Zurück**

* Eine positive Zahl,`sqrt(x) * sqrt(x) == x`
* `null`Wenn das Argument negativ ist oder nicht in konvertiert eine `real` Wert. 




### <a name="toint"></a>ToInt

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>tolong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>ToDouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>Datum und Uhrzeit


[vor](#ago) | [Dayofmonth](#dayofmonth) | [Dayofweek](#dayofweek) |  [Dayofyear](#dayofyear) |[Datepart](#datepart) | [Endofday](#endofday) | [Endofmonth](#endofmonth) | [Endofweek](#endofweek) | [namens](#endofyear) | [Getmonth](#getmonth)|  [Getyear](#getyear) | [jetzt](#now) | [Startofday](#startofday) | [Startofmonth](#startofmonth) | [Startofweek](#startofweek) | [Startofyear](#startofyear) | [Todatetime](#todatetime) | [Totimespan](#totimespan) | [Weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>Datum und Uhrzeit Literale

|||
---|---
**DateTime**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Wie oft werden immer in UTC. Das Datum weglassen bietet heute eine Zeit.
`now()`|Die aktuelle Zeit.
`now(`-*TimeSpan*`)`|`now()-`*TimeSpan*
`ago(`*TimeSpan*`)`|`now()-`*TimeSpan*
**TimeSpan**|
`2d`|2 Tage
`1.5h`|1,5 Stunden 
`30m`|30 Minuten
`10s`|10 Sekunden
`0.1s`|0,1 Sekunden
`100ms`| 100 Millisekunden
`10microsecond`|
`1tick`|100ns
`time("15 seconds")`|
`time("2")`| 2 Tage
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>Datum und Uhrzeit Ausdrücke

Ausdruck |Ergebnis
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Weniger
`<=`|Kleiner oder gleich
`>` |Größer
`>=`|Größer oder gleich
`<>`|Ungleich
`!=`|Ungleich 




### <a name="ago"></a>vor

Subtrahiert die angegebene Timespan aktuelle UTC-Uhrzeit. Wie `now()`, diese Funktion kann mehrmals in einer Anweisung verwendet werden und die UTC-Uhrzeit verwiesen werden für alle Instanziierungen.

**Syntax**

    ago(a_timespan)

**Argumente**

* *A_timespan*: Intervall zu subtrahierende die aktuelle UTC-Uhrzeit (`now()`).

**Zurück**

    now() - a_timespan

**Beispiel**

Alle Zeilen mit einem Zeitstempel in der letzten Stunde:

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>DatePart

    datepart("Day", datetime(2015-12-14)) == 14

Extrahiert einen angegebenen Teil eines Datums als ganze Zahl.

**Syntax**

    datepart(part, datetime)

**Argumente**

* `part:String`-{"Year", "Monat" "Tag", "Stunde", "Minute", "Second", "Millisecond", "Mikrosekunden", "Nanosekunden"}
* `datetime`

**Zurück**

Long, die den angegebenen Bereich darstellt.


### <a name="dayofmonth"></a>DayOfMonth

    dayofmonth(datetime("2016-05-15")) == 15 

Die Ordinalzahl des Tages im Monat.

**Syntax**

    dayofmonth(a_date)

**Argumente**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>DayOfWeek

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Die ganzzahlige Anzahl von Tagen seit dem vorhergehenden Sonntag als ein `timespan`.

**Syntax**

    dayofweek(a_date)

**Argumente**

* `a_date`: A `datetime`.

**Zurück**

Die `timespan` seit Mitternacht am Anfang der vorhergehenden Sonntag Tage eine ganze Zahl abgerundet.

**Beispiele**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>DayOfYear

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

Die Ordinalzahl des Tages im Jahr.

**Syntax**

    dayofyear(a_date)

**Argumente**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>Endofday, Endofweek, Endofmonth, Namens

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getMonth

Datetime Monatszahl (1-12) erhalten.

**Beispiel**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>getYear

Erhalten Sie das Jahr eines DateTime.

**Beispiel**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>jetzt

    now()
    now(-2d)

Die aktuelle UTC-Uhrzeit optional durch eine angegebene Zeitspanne versetzt. Diese Funktion kann mehrmals in einer Anweisung verwendet und referenzierte Uhrzeit werden für alle Instanzen identisch.

**Syntax**

    now([offset])

**Argumente**

* *Versatz:* Ein `timespan`, die aktuelle UTC-Uhrzeit hinzugefügt. Standard: 0.

**Zurück**

Die aktuelle UTC-Uhrzeit als ein `datetime`.

    now() + offset

**Beispiel**

Bestimmt das Intervall seit dem Ereignis vom Prädikat identifiziert:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>Startofday, Startofweek, Startofmonth, startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>ToDateTime

Alias `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

Überprüfen Sie, ob eine Zeichenfolge ein gültiges Datum:

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>ToTimeSpan

Alias `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>WeekOfYear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

Das Ganzzahlergebnis stellt die Wochennummer standard ISO-8601 dar. Der erste Tag der Woche ist Sonntag und die erste Woche des Jahres ist die Woche, die ersten Donnerstag des Jahres enthält. (Die letzten Tage des Jahres darf daher einige Tage der Woche 1 im nächsten Jahr oder Anfang enthalten einige der Woche 52 und 53 des Vorjahres.)


## <a name="string"></a>Zeichenfolge

[Countof](#countof) | [extrahieren](#extract) | [Extractjson](#extractjson)  | [Isempty](#isempty) | [Isnotempty](#isnotempty) | [Notempty](#notempty) | [Parseurl](#parseurl) | [Ersetzen](#replace) | [Teilen](#split) | [Strcat](#strcat) | [Strlen](#strlen) | [Teilzeichenfolge](#substring) | [Tolower](#tolower) | [Tostring](#tostring) | [Toupper](#toupper)


### <a name="string-literals"></a>Zeichenfolgenliterale

Die Regeln sind identisch mit JavaScript.

Zeichenfolgen können entweder in einzelne oder in doppelte Anführungszeichen eingeschlossen werden. 

Umgekehrter Schrägstrich (`\`), wie Escapezeichen verwendet `\t` (Registerkarte) `\n` (Zeilenvorschub) und Anführungszeichen einschließen.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>Verborgene Zeichenfolgenliterale

Verborgene Zeichenfolgenliterale sind Zeichenfolgen Analytics verdeckt die Zeichenfolge (z. B. beim tracing) ausgeben. Verbergen alle verborgene Zeichen ersetzt durch eine (`*`) Zeichen.

Bilden Sie eine verschleierte Zeichenfolgenliteral voran `h` oder 'H'. Zum Beispiel:

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>Vergleichen von Zeichenfolgen

Operator|Beschreibung|Groß-/Kleinschreibung|True, Beispiel
---|---|---|---
`==`|Ist gleich |Ja| `"aBc" == "aBc"`
`<>` `!=`|Ungleich|Ja| `"abc" <> "ABC"`
`=~`|Ist gleich |Nein| `"abc" =~ "ABC"`
`!~`|Ungleich |Nein| `"aBc" !~ "xyz"`
`has`|Rechte Seite (RS) bezeichnet eine ganze im linken Seite (LHS)|Nein| `"North America" has "america"`
`!has`|Rechte Seite ist keiner vollständigen Begriff LHS|Nein|`"North America" !has "amer"` 
`hasprefix`|Rechte Seite ist ein Präfix eines Begriffs LHS|Nein|`"North America" hasprefix "ame"`
`!hasprefix`|Rechte Seite ist kein Präfix von einem beliebigen Begriff im LHS|Nein|`"North America" !hasprefix "mer"`
`hassuffix`|Rechte Seite ist ein Suffix eines Begriffs LHS|Nein|`"North America" hassuffix "rth"`
`!hassuffix`|Rechte Seite ist kein Suffix jeder Begriff LHS|Nein|`"North America" !hassuffix "mer"`
`contains` | Rechte Seite tritt als Teilzeichenfolge LHS|Nein| `"FabriKam" contains "BRik"`
`!contains`| Rechte Seite tritt nicht in LHS|Nein| `"Fabrikam" !contains "xyz"`
`containscs` | Rechte Seite tritt als Teilzeichenfolge LHS|Ja| `"FabriKam" contains "Kam"`
`!containscs`| Rechte Seite tritt nicht in LHS|Ja| `"Fabrikam" !contains "Kam"`
`startswith`|Rechte Seite ist ein Anfangszeichen LHS.|Nein|`"Fabrikam" startswith "fab"`
`!startswith`|Rechte Seite ist keiner Anfangszeichen LHS.|Nein|`"Fabrikam" !startswith "abr"`
`endswith`|Rechte Seite ist eine terminal Teilzeichenfolge LHS.|Nein|`"Fabrikam" endswith "kam"`
`!endswith`|Rechte Seite ist keiner terminal Teilzeichenfolge LHS.|Nein|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS enthält eine Übereinstimmung für rechte Seite|Ja| `"Fabrikam" matches regex "b.*k"`
`in`|Alle Elemente gleich|Ja|`"abc" in ("123", "345", "abc")`
`!in`|Elemente nicht gleich|Ja|`"bc" !in ("123", "345", "abc")`

Mit `has` oder `in` Wenn Sie das Vorhandensein eines gesamten lexikalische Term - testen ist, ein Symbol oder eine alphanumerische Wort begrenzt alphanumerischen Zeichen, Start oder Ende des Felds. `has`führt schneller `contains`, `startswith` oder `endswith`. Die erste dieser Abfragen schneller ausgeführt:

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Anzahl der Vorkommen einer Teilzeichenfolge in einer Zeichenfolge. Einfache Zeichenfolge entspricht überschneiden; Regex entspricht nicht.

**Syntax**

    countof(text, search [, kind])

**Argumente**

* *Text:* Eine Zeichenfolge.
* *Suchen:* Einfache Zeichenfolge oder *Text*entspricht.
* *Art:* `"normal"|"regex"` Standard `normal`. 

**Zurück**

Die Anzahl die Suchzeichenfolge im Container zugeordnet werden kann. Einfache Zeichenfolge entspricht überschneiden; Regex entspricht nicht.

**Beispiele**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (nicht 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>Extrahieren

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

Eine Übereinstimmung für einen [regulären Ausdruck](#regular-expressions) aus einer Zeichenfolge abrufen. Optional konvertiert dann die extrahierte Teilzeichenfolge in den angegebenen Typ.

**Syntax**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumente**

* *Regex:* Ein [regulärer Ausdruck](#regular-expressions).
* *CaptureGroup:* Eine Positive `int` -Konstante zurück, die Erfassungsgruppe extrahieren. 0 steht für die gesamte Übereinstimmung 1 für den Wert vom ersten "(" Klammer")" im regulären Ausdruck mindestens 2 für nachfolgende Klammern übereinstimmen.
* *Text:* Ein `string` suchen.
* *TypeLiteral:* Ein optionaler Typ Literal (z.B. `typeof(long)`). Wenn angegeben, ist die extrahierte Teilzeichenfolge in diesen Typ konvertiert. 

**Zurück**

Wenn *Regex* *in*Übereinstimmung: die Teilzeichenfolge mit angegebenen Erfassung Gruppe *CaptureGroup*optional in *TypeLiteral*konvertiert.

Wenn keine Übereinstimmung vorhanden ist oder die Konvertierung fehlschlägt: `null`. 

**Beispiele**

Die Zeichenfolge wird `Trace` eine Definition gesucht `Duration`. Die Übereinstimmung konvertiert `real`, dann eine Konstante multipliziert (`1s`), `Duration` vom Typ `timespan`. In diesem Beispiel ist gleich 123.45 Sekunden:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

Dieses Beispiel entspricht `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>IsEmpty, Isnotempty, notempty

    isempty("") == true

True, wenn das Argument eine leere Zeichenfolge oder null.
Siehe auch [Isnull](#isnull).


**Syntax**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Zurück**

Gibt an, ob das Argument Isnull oder eine leere Zeichenfolge ist.

|x|IsEmpty(x)
|---|---
| "" | true
|"X" | falsch
|parseJSON("")|true
|parseJSON("[]")|falsch
|parseJSON("{}")|falsch


**Beispiel**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>ParseURL

URL in seine Bestandteile aufgeteilt.

**Syntax**

    parseurl(urlstring)

**Argumente**

* *Urlstring:* EINE URL.

**Zurück**

Ein Objekt, die Teile als Zeichenfolgen enthält.

**Beispiel**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>Ersetzen

Alle Regex entspricht mit einer anderen Zeichenfolge zu ersetzen.

**Syntax**

    replace(regex, rewrite, text)

**Argumente**

* *Regex:* Der [reguläre Ausdruck](https://github.com/google/re2/wiki/Syntax) *Text*suchen. Sie können Gruppen Erfassung '('Klammern')' enthalten. 
* *Schreiben:* Der Regex Ersatz für jede Übereinstimmung *MatchingRegex*. Mit `\0` das gesamte Spiel auf `\1` für die erste Erfassungsgruppe `\2` für nachfolgende Aufzeichnung Gruppen.
* *Text:* Eine Zeichenfolge.

**Zurück**

*Text* nach Bewertungen *Schreiben*alle Spiele *Regex* ersetzen. Entspricht überlappen nicht.

**Beispiel**

Diese Anweisung:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

Führt zu folgenden Ergebnissen:

| x    | Str | ersetzt|
|---|---|---|
| 1    | 1.000000 ist  | Anzahl: 1.000000|
| 2    | Ist 2.000000  | Anzahl: 2.000000|
| 3    | Ist 3.000000  | Anzahl: 3.000000|
| 4    | Ist 4.000000  | Anzahl: 4.000000|
| 5    | Ist 5.000000  | Anzahl: 5.000000|
 



### <a name="split"></a>Teilen

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Teilt eine Zeichenfolge nach einem bestimmten Trennzeichen und gibt ein Zeichenfolgenarray mit den enthaltenen Teilzeichenfolgen zurück. Optional kann eine bestimmte Teilzeichenfolge zurückgegeben, wenn vorhanden.

**Syntax**

    split(source, delimiter [, requestedIndex])

**Argumente**

* *Quelle*: die Quellzeichenfolge nach angegebenen Trennzeichen aufgeteilt werden.
* *Trennzeichen*: das Trennzeichen, das verwendet wird, um die Quellzeichenfolge aufgeteilt.
* *RequestedIndex*: eine optionale nullbasierten Index `int`. Sofern das zurückgegebene Zeichenfolgenarray enthält angeforderte Teilzeichenfolge wenn vorhanden ist. 

**Zurück**

Ein Zeichenfolgenarray mit Teilzeichenfolgen bestimmte Zeichenfolge, die durch das angegebene Trennzeichen getrennt sind.

**Beispiele**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

Zwischen 1 und 16 Argumente, die Zeichenfolgen verkettet.

### <a name="strlen"></a>strlen

    strlen("hello") == 5

Die Länge einer Zeichenfolge.

### <a name="substring"></a>substring

    substring("abcdefg", 1, 2) == "bc"

Extrahieren einer Teilzeichenfolge aus einer bestimmten Quellzeichenfolge ab einem angegebenen Index. Die Länge des angeforderten Teilzeichenfolge kann optional angegeben werden.

**Syntax**

    substring(source, startingIndex [, length])

**Argumente**

* *Quelle:* Die Quellzeichenfolge Teilzeichenfolge entnommen.
* *StartingIndex:* Die nullbasierte Zeichen Anfangsposition der gewünschten untergeordneten Zeichenfolge.
* *Länge:* Ein optionaler Parameter, der an die angeforderte Anzahl von Zeichen in der Teilzeichenfolge verwendet werden kann. 

**Zurück**

Eine Teilzeichenfolge aus der angegebenen Zeichenfolge. Die Teilzeichenfolge beginnt an Zeichenposition StartingIndex (nullbasiert) und bis zum Ende der Zeichenfolge oder Länge Zeichen angegeben.

**Beispiele**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>ToLower

    tolower("HELLO") == "hello"

Konvertiert eine Zeichenfolge in Kleinbuchstaben.

### <a name="toupper"></a>ToUpper

    toupper("hello") == "HELLO"

Konvertiert eine Zeichenfolge in Großbuchstaben.



### <a name="guids"></a>GUIDs

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>Arrays, Objekten und dynamisch

[Literale](#dynamic-literals) | [Umwandlung](#casting-dynamic-objects) | [Operatoren](#operators) | [Klauseln lassen](#dynamic-objects-in-let-clauses)
<br/>
[ArrayLength](#arraylength) | [Extractjson](#extractjson) | [Parsejson](#parsejson) | [Bereich](#range) | [Treepath](#treepath) | [Todynamic](#todynamic) | [Zip](#zip)


Hier ist das Ergebnis einer Abfrage in einer Anwendung Einblicke Ausnahme. Der Wert im `details` ist ein Array.

![](./media/app-insights-analytics-reference/310.png)

**Indizierung:** Index-Arrays und Objekte wie JavaScript:

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* Aber `arraylength` und anderen Analytics-Funktionen (nicht ".length"!)

**Umwandeln** In einigen Fällen muss ein Element, das aus einem Objekt extrahieren umwandeln, da der Typ variieren kann. Beispielsweise `summarize...to` ein bestimmtes Typs benötigt:

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Literale** Erstellen einer expliziten Arrays oder Eigenschaftensammlung schreiben Sie als JSON-Zeichenfolge aus:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**Mvexpand:** Verwenden Sie, um die Eigenschaften eines Objekts in separaten Zeilen zerlegen, Mvexpand:

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**Treepath:** Alle Pfade in einem komplexen Objekt zu finden:

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**Buildschema:** Minimale Schema gefunden, das alle Werte des Ausdrucks in der Tabelle gibt:

    exceptions | summarize buildschema(details)

Ergebnis:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Beachten Sie, dass `indexer` markiert, numerischen Index verwenden soll. Für dieses Schema einige gültige Pfade würde (vorausgesetzt diese Beispiel Indizes sind im Bereich):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>Array- und Literale

Verwenden Sie zum Erstellen einer dynamischen Literal `parsejson` (Alias `todynamic`) mit einer JSON Zeichenfolgenargument:

* `parsejson('[43, 21, 65]')`-ein Array von Zahlen
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`-ein einzelner Wert des dynamischen Typ mit einer Zahl
* `parsejson('"21"')`-ein einzelner Wert des dynamischen Typ mit einer Zeichenfolge

Beachten Sie, dass im Gegensatz zu JavaScript JSON erfordert die Verwendung von Anführungszeichen (`"`) Zeichenfolgen. Daher ist es im Allgemeinen einfacher JSON-codierte Zeichenfolge mit einfachen Anführungszeichen Literale Angebot (`'`).

Dieses Beispiel erstellt einen dynamischen Wert und die Felder verwendet:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>Dynamisches Objekt-Funktionen

|||
|---|---|
| *Wert* `in` *Array*| True, wenn ein Element des *Arrays* , == *Wert*<br/>`where City in ('London', 'Paris', 'Rome')`
| *Wert* `!in` *Array*| True, wenn kein Element des *Arrays* ist, == *Wert*
|[`arraylength(`Array`)`](#arraylength)| Wenn ein Array nicht NULL
|[`extractjson(`Pfad, Objekt`)`](#extractjson)|Verwendet den Pfad zum Objekt navigieren.
|[`parsejson(`Quelle`)`](#parsejson)| Wandelt eine JSON-Zeichenfolge in ein dynamisches Objekt.
|[`range(`Schritt aus`)`](#range)| Ein Array von Werten
|[`mvexpand`listColumn](#mvexpand-operator) | Repliziert eine Zeile für jeden Wert in einer angegebenen Zelle in einer Liste.
|[`summarize buildschema(`Spalte`)`](#buildschema) |Leitet das Schema aus Spalteninhalt
|[`summarize makelist(`Spalte`)`](#makelist)| Fasst Zeilengruppen und legt die Werte der Spalte im Array.
|[`summarize makeset(`Spalte`)`](#makeset) | Gruppen von Zeilen reduziert und die Werte der Spalte in Array ohne Duplikate.

### <a name="dynamic-objects-in-let-clauses"></a>Dynamische Objekte können Klauseln


[Klauseln können](#let-clause) Speicher dynamische Werte als Zeichenfolgen, diese beiden Klauseln entsprechen und benötigen die `parsejson` (oder `todynamic`) vor:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>ArrayLength

Die Anzahl der Elemente in ein dynamisches Array.

**Syntax**

    arraylength(array)

**Argumente**

* *Array:* Ein `dynamic` Wert.

**Zurück**

Die Anzahl der Elemente im *Array*oder `null` Wenn *Array* kein Array ist.

**Beispiele**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Abrufen eines angegebenen Elements aus einem JSON-Text über ein. Konvertieren Sie die extrahierte Zeichenfolge optional in einen bestimmten Typ.


**Syntax**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**Zurück**

Diese Funktion führt eine JsonPath Abfrage in DataSource eine gültige JSON-Zeichenfolge konvertiert optional je nach das dritte Argument in einen anderen Wert enthält.



**Beispiel**

[Klammern] Notation und Punktnotation entsprechen:

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Tipps zur Leistungssteigerung**

* Where-Klauseln vor anwenden`extractjson()`
* Sollten Sie Übereinstimmung eines regulären Ausdrucks mit [extrahieren Sie](#extract) stattdessen. Dies kann sehr viel schneller ausgeführt und ist wirksam, wenn die JSON aus einer Vorlage erstellt wird.
* Mit `parsejson()` benötigen Sie mehr als einen Wert von JSON extrahieren.
* JSON zur Aufnahme durch die Art der Spalte analysiert und dynamisch in Betracht.

### <a name="json-path-expressions"></a>JSON-Pfad-Ausdrücke

|||
|---|---|
|`$`|Stammobjekt|
|`@`|Aktuelle Objekt|
|`[0]`|Arrayfeldindex|
|`.`oder`[0]` | Untergeordnetes Element|

*(Derzeit nicht wir Platzhalter, Rekursion, Union oder Segmente implementieren.)*




### <a name="parsejson"></a>parseJSON

Interpretiert eine `string` als [JSON-Wert](http://json.org/)) und gibt den Wert als `dynamic`. Es ist mit `extractjson()` Sie mehr als ein Element eines zusammengesetzten Objekts JSON extrahieren möchten.

**Syntax**

    parsejson(json)

**Argumente**

* *Json:* Ein JSON-Dokument.

**Zurück**

Ein Objekt vom Typ `dynamic` *Json*angegeben.

**Beispiel**

Im folgenden Beispiel bei `context_custom_metrics` ist ein `string` , sieht folgendermaßen aus: 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

dann das folgende Fragment den Wert ruft der `duration` Steckplatz im Objekt und davon ruft zwei Steckplätze `duration.value` und  `duration.min` (`118.0` und `110.0`bzw.).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>Bereich

Die `range()` Funktion (nicht zu verwechseln mit der `range` Operator) generiert ein dynamisches Array eine Reihe von Werten gleich.

**Syntax**

    range(start, stop, step)

**Argumente**

* *gestartet:* Der Wert des ersten Elements in das resultierende Array. 
* *stop:* Der Wert des letzten Elements im resultierenden Array oder geringsten Wert, der größer ist als das letzte Element im resultierenden Array und Integer Vielfache von *Schritt* *aus*.
* *Schritt:* Die Differenz zwischen zwei aufeinander folgenden Elemente des Arrays.

**Beispiele**

Das folgende Beispiel gibt `[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

Das folgende Beispiel gibt ein Array aller Tagen im Jahr 2015:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Konvertiert eine Zeichenfolge in einen dynamischen Wert.

### <a name="treepath"></a>TreePath

    treepath(dynamic_object)

Listet die pfadausdrücke, die Blätter in ein dynamisches Objekt identifizieren. 

**Zurück**

Ein Array von Pfad Ausdrücken.

**Beispiele**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Beachten Sie, dass "[0]" Array zeigt jedoch keinen Index durch einen bestimmten Pfad verwendet.

### <a name="zip"></a>ZIP

    zip(list1, list2, ...)

Kombiniert Listen in einer Liste von Tupeln.

* `list1...`: Eine Liste von Werten

**Beispiele**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>Namen

Namen können bis zu 1024 Zeichen lang sein. Sie Groß-/Kleinschreibung und dürfen Buchstaben, Ziffern und unterstrichen (`_`). 

Angebot einen Namen mit ['... '] or [" ... "] andere Zeichen oder ein Schlüsselwort als Namen verwenden. Zum Beispiel:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|["Pfad\\\n\'x\''] | Mit \ als Escapezeichen|
|["d-e.=/f#\n"] | |
|[@'path\file'] | Keine Escapezeichen - \ Literal|
|[@"\nowund dann\"] | |
|[] | Ein verwenden als Namen|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


