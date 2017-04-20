<properties 
    pageTitle="SQL-Syntax und SQL-Abfrage für DocumentDB | Microsoft Azure" 
    description="Informationen Sie zu SQL-Syntax, Datenbanken und SQL-Abfragen für DocumentDB NoSQL-Datenbank. SQL kann als eine JSON-Abfragesprache in DocumentDB verwendet." 
    keywords="SQL-Syntax, Sql-Abfrage, Sql-Abfragen, Json-Abfragesprache, Datenbanken und Sql-Abfragen mit Aggregatfunktionen"
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="sql-query-and-sql-syntax-in-documentdb"></a>SQL-Abfrage und SQL-Syntax in DocumentDB
Microsoft Azure DocumentDB unterstützt abfragende Dokumenten SQL (strukturierte Abfragesprache) als Abfragesprache JSON. DocumentDB ist wirklich Schema. Durch sein Engagement JSON-Datenmodell direkt innerhalb des Datenbankmoduls wird die automatische Indizierung der JSON-Dokumente ohne explizite Schema oder Erstellung von sekundären Indizes. 

Beim Entwerfen der Abfragesprache für DocumentDB hatten wir zwei Ziele:

-   Anstatt eine neue Abfragesprache JSON erfinden, wollte SQL unterstützen. SQL ist eine bekannte und beliebte Abfragesprachen. DocumentDB SQL bietet eine formale Programmiermodell für umfangreiche Abfragen über JSON-Dokumente.
-   Als JSON-Dokument-Datenbank direkt in die Datenbank-Engine JavaScript ausführen wollte JavaScript Programmiermodell als Grundlage für unsere Abfragesprache verwenden. JavaScript Typsystem und Auswertung von Ausdrücken Funktionsaufruf ist SQL DocumentDB verankert. Diese wiederum bietet einen natürlichen Programmiermodell für relationale Projektionen, hierarchische Navigation JSON-Dokumente, Self-Joins raumbezogener Abfragen und Aufrufen von benutzerdefinierten Funktionen (UDFs) in JavaScript unter anderem geschrieben. 

Wir glauben, dass diese Funktionen zur Verringerung der Reibung zwischen der Anwendung und der Datenbank und für die Entwicklerproduktivität.

Erste Schritte im folgenden Video, wo Aravind Ramachandran DocumentDB Abfragen Funktionen zeigt, und besuchen Sie unsere [Abfrage Spielplatz](http://www.documentdb.com/sql/demo), wo Sie versuchen, DocumentDB und unser Dataset SQL-Abfragen ausführen sollten.

> [AZURE.VIDEO dataexposedqueryingdocumentdb]

Dann zurück zu diesem Artikel, beginnen wir mit einer SQL-Abfrage-Lernprogramm, das über einige einfache JSON-Dokumente und SQL Befehle führt.

## <a name="getting-started-with-sql-commands-in-documentdb"></a>Erste Schritte mit SQL-Befehlen in DocumentDB
DocumentDB SQL am Arbeitsplatz finden wir beginnen mit einigen einfachen JSON-Dokumente und zeigen einige einfachen Abfragen. Betrachten Sie diese zwei JSON-Dokumente über zwei Familien. Beachten Sie, dass mit DocumentDB überflüssig Schemas oder sekundären Indizes nicht explizit erstellen. Wir müssen lediglich JSON-Dokumente auf eine Auflistung DocumentDB einfügen und anschließend abgefragt. Hier haben wir einen einfachen JSON Adresse und Registrierungsinformationen für Familie Andersen, Eltern, Kinder und Haustiere dokumentieren. Das Dokument hat, Zeichenfolgen, Zahlen, boolesche Werte, Arrays und geschachtelten Eigenschaften. 

**Dokument**  

    {
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }


Hier ist ein zweites Dokument mit einem feinen Unterschied – `givenName` und `familyName` anstelle von `firstName` und `lastName`.

**Dokument**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
                "familyName": "Miller", 
                 "givenName": "Lisa", 
                 "gender": "female", 
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "creationDate": 1431620462,
        "isRegistered": false
    }



Jetzt probieren Sie einige Abfragen für diese Daten an die wichtigsten Aspekte des DocumentDB SQL verstehen. Z. B. die folgende Abfrage liefert Dokumente entspricht das Id-Feld `AndersenFamily`. Ist ein `SELECT *`, die Ausgabe der Abfrage wird das vollständige JSON-Dokument:

**Abfrage**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]


Nun betrachten Sie die JSON-Ausgabe in eine andere Form formatieren müssen. Diese Abfrage projiziert ein neues JSON-Objekt mit zwei ausgewählte Felder Name und Ort, wenn die Stadt denselben Namen wie der Status hat. In diesem Fall entspricht "New York, NY".

**Abfrage**   

    SELECT {"Name":f.id, "City":f.address.city} AS Family 
    FROM Families f 
    WHERE f.address.city = f.address.state

**Ergebnisse**

    [{
        "Family": {
            "Name": "WakefieldFamily", 
            "City": "NY"
        }
    }]


Die nächste Abfrage gibt Vornamen Kinder in der Familie, dessen Id entspricht `WakefieldFamily` den Ort des Wohnorts geordnet.

**Abfrage**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC

**Ergebnisse**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


Wir möchten einige wichtige Aspekte der DocumentDB Abfragesprache die Beispiele, die wir bisher Zeichnen:  
 
-   Seit SQL DocumentDB auf JSON, behandelt Entitäten Zeilen und Spalten geformten Struktur. Daher Sprache können Sie die Knoten der Struktur auf eine beliebige Tiefe wie finden `Node1.Node2.Node3…..Nodem`wie relationale SQL den zweiteiligen Verweis auf `<table>.<column>`.   
-   Strukturierte Abfragesprache verwendet Schema weniger Daten. Daher muss das Typsystem dynamisch gebunden werden. Der gleiche Ausdruck kann verschiedene Dokumente ergeben. Das Ergebnis einer Abfrage ist ein gültiger JSON-Wert, aber ist nicht unbedingt eine feste Schema.  
-   DocumentDB unterstützt nur strenge JSON-Dokumente. Dies bedeutet, dass das Typsystem und Ausdrücke nur mit JSON-Typen beschränkt sind. Finden Sie die [JSON-Spezifikation](http://www.json.org/) für Weitere Informationen.  
-   Eine DocumentDB-Auflistung ist ein Schema frei Container JSON-Dokumente. Relationen Datenentitäten innerhalb und zwischen Dokumenten in einer Auflistung werden implizit durch Eingrenzung und kein Primärschlüssel und Fremdschlüssel-Relationen erfasst. Dies ist ein wichtiger Aspekt angesichts dokumentinterne Joins später in diesem Artikel beschriebenen hinzuweisen.

## <a name="documentdb-indexing"></a>DocumentDB Indizierung

Bevor man in DocumentDB SQL-Syntax ist es Wert Indizierung Design in DocumentDB. 

Die von Datenbankindizes dient Abfragen in ihren verschiedenen Formen und Formen mit minimalen Ressourcen (z. B. CPU und Eingabe/Ausgabe) guten Durchsatz und niedrige Latenz. Die Wahl des richtigen Index zum Abfragen einer Datenbank erfordert oft viel Planung und experimentieren. Dieser Ansatz stellt eine Herausforderung für Datenbanken mit weniger Schemata die Daten nicht strikte Schema entsprechen und schnell entwickelt. 

Bei DocumentDB Indizierung Subsystem, legen wir deshalb die folgenden Ziele:

-   Indizieren von Dokumenten ohne Schema: Teilsystems Indizierung Schema Informationen weder Annahmen zum Schema der Dokumente. 

-   Unterstützung für effiziente, umfassende hierarchische und relationale Abfragen: Index unterstützt DocumentDB Abfragesprache, einschließlich Unterstützung für hierarchische und relationale Projektionen.

-   Unterstützung für konsistente Abfragen angesichts der anhaltend Datenträger schreibt: für hohe schreiben Durchsatz Arbeitslasten mit konsistenten Abfragen Index werden inkrementell, effizient und online angesichts der anhaltend Datenträger schreibt. Konsistente Index Update ist für Abfragen auf Konsistenz dienen, in dem der Benutzer den Dokumentdienst konfiguriert.

-   Unterstützung für mehrere Mandanten: Reservierung Modell für Ressource Governance über Mieter gegeben, Index werden Aktualisierungen Kostenrahmen Systemressourcen (CPU, Arbeitsspeicher und Eingabe-/Ausgabeoperationen pro Sekunde), die pro Replikat. 

-   Speichereffizienz: Kosten-Festplattenspeicher-Overhead des Indexes ist begrenzt und vorhersagbar. Dies ist entscheidend, da DocumentDB Developer Kosten Basis Kompromisse zwischen Index Aufwand in Bezug auf die Leistung von Abfragen zu kann.  

Beispiele zum Konfigurieren der Volltextindizierung Richtlinie für eine Auflistung finden Sie unter [DocumentDB Beispiele](https://github.com/Azure/azure-documentdb-net) auf MSDN. Nun kommen wir in die Details der DocumentDB SQL-Syntax.


## <a name="basics-of-a-documentdb-sql-query"></a>Grundlagen von DocumentDB SQL-Abfrage
Jede Abfrage besteht aus einer SELECT-Klausel und optional von und WHERE-Klauseln pro ANSI SQL-Standards. Für jede Abfrage wird in der Regel die Quelle in der FROM-Klausel aufgelistet. Dann wird der Filter in der WHERE-Klausel auf eine Teilmenge der JSON-Dokumente abrufen angewendet. Schließlich dient die SELECT-Klausel die angeforderte JSON Werte in der Liste auswählen.
    
    SELECT [TOP <top_expression>] <select_list> 
    [FROM <from_specification>] 
    [WHERE <filter_condition>]
    [ORDER BY <sort_specification]    


## <a name="from-clause"></a>FROM-Klausel
Die `FROM <from_specification>` -Klausel ist optional, sofern die Quelle gefiltert oder später in der Abfrage projiziert. Diese Klausel ist die Datenquelle an, auf der die Abfrage ausgeführt werden muss. Häufig ist die gesamte Auflistung der Quelle eine Teilmenge der Auflistung kann stattdessen angegeben. 

Eine Abfrage wie `SELECT * FROM Families` gibt an, dass die gesamte Auflistung der Familien der Quelle über die aufgelistet werden sollen. Eine spezielle Kennung Stamm kann verwendet werden, die anstelle der Sammlungsname Darstellung. Die folgende Liste enthält die Regeln, die pro Abfrage erzwungen werden:

- Die Auflistung kann Alias, wie `SELECT f.id FROM Families AS f` oder `SELECT f.id FROM Families f`. Hier `f` entspricht `Families`. `AS`ist ein optionales Schlüsselwort Alias der Bezeichner.

-   Beachten Sie, dass Alias der ursprünglichen Quelle kann nicht gebunden werden. Beispielsweise `SELECT Families.id FROM Families f` ist syntaktisch ungültig, da der Bezeichner "Familie" nicht mehr aufgelöst werden kann.

-   Alle Eigenschaften, die erfolgen müssen vollqualifiziert sein. Dies wird in Ermangelung Schema strikte Einhaltung Vermeidung mehrdeutiger Bindings erzwungen. Daher `SELECT id FROM Families f` syntaktisch ungültig ist, da die Eigenschaft `id` nicht gebunden ist.
    
### <a name="sub-documents"></a>Untergeordnete Dokumente
Die Quelle kann auch auf eine kleinere Teilmenge reduziert werden. Beispielsweise kann nur eine Teilstruktur in jedem Dokument auflisten, unter Root dann die Quelle werden wie im folgenden Beispiel gezeigt.

**Abfrage**

    SELECT * 
    FROM Families.children

**Ergebnisse**  

    [
      [
        {
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [
              {
                  "givenName": "Fluffy"
              }
            ]
        }
      ],
      [
        {
            "familyName": "Merriam",
            "givenName": "Jesse",
            "gender": "female",
            "grade": 1
        },
        {
            "familyName": "Miller",
            "givenName": "Lisa",
            "gender": "female",
            "grade": 8
        }
      ]
    ]

Im obigen Beispiel wird ein Array als Quelle verwendet, konnte ein Objekt auch als Quelle verwendet werden die im folgenden Beispiel gezeigt wird. Für die Aufnahme in das Ergebnis der Abfrage gelten alle gültiger JSON-Wert (nicht undefiniert), die in der Quelle gefunden werden. Familien haben eine `address.state` Wert im Abfrageergebnis ausgeschlossen.

**Abfrage**

    SELECT * 
    FROM Families.address.state

**Ergebnisse**

    [
      "WA", 
      "NY"
    ]


## <a name="where-clause"></a>WHERE-Klausel
Die WHERE-Klausel (**`WHERE <filter_condition>`**) ist optional. Sie gibt die Bedingung(en), die JSON-Dokumente von der Quelle bereitgestellt erfüllen müssen, um das Ergebnis enthalten sein. JSON-Dokument muss der angegebenen Bedingung auf "true" für das Ergebnis gelten ergeben. Die WHERE-Klausel mit dem Index um absolute kleinste Teilmenge Herkunftsbelege ermitteln, die Teil des Ergebnisses verwendet. 

Die folgende Abfrage fordert Dokumente mit eine Name-Eigenschaft, deren Wert `AndersenFamily`. Jedes Dokument, das eine Name-Eigenschaft nicht besitzt oder wo der Wert `AndersenFamily` ausgeschlossen ist. 

**Abfrage**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


Das vorherige Beispiel zeigte eine einfache Übereinstimmungsvergleiche Abfrage. DocumentDB SQL unterstützt außerdem eine Vielzahl von skalaren Ausdrücken. Die am häufigsten verwendeten sind binär und unäre Ausdrücke. Verweise auf Eigenschaften aus dem Quellobjekt JSON sind auch gültige Ausdrücke. 

Die folgenden binären Operatoren werden unterstützt und können in Abfragen verwendet werden, wie in den folgenden Beispielen dargestellt:  
<table>
<tr>
<td>Arithmetische Operationen</td> 
<td>+,-,*,/,%</td>
</tr>
<tr>
<td>Bitweise</td>    
<td>|, &, ^, <<, >>, >>> (0-Füllung Verschiebung nach rechts) </td>
</tr>
<tr>
<td>Logische</td>
<td>UND, ODER, NICHT</td>
</tr>
<tr>
<td>Vergleich</td> 
<td>=, !=, &lt;, &gt;, &lt;=, &gt;=, <></td>
</tr>
<tr>
<td>Zeichenfolge</td> 
<td>|| (verketten)</td>
</tr>
</table>  

Betrachten wir nun einige Abfragen mit binären Operatoren.

    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1
    
    SELECT * 
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5
    
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5     -- matching grades == 5


Unäre Operatoren +,-, ~ nicht ebenfalls unterstützt und können in Abfragen verwendet werden, wie im folgenden Beispiel gezeigt:

    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1
    
    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5



Neben Binär- und unäre Operatoren Eigenschaftenverweise dürfen. Beispielsweise `SELECT * FROM Families f WHERE f.isRegistered` gibt das JSON-Dokument mit der Eigenschaft `isRegistered` den Wert der Eigenschaft ist gleich der JSON `true` Wert. Alle anderen Werte (false, null, Undefined, `<number>`, `<string>`, `<object>`, `<array>`usw.) führt das Quelldokument aus der Ergebnismenge ausgeschlossen. 

### <a name="equality-and-comparison-operators"></a>Gleichheits-und Vergleichsoperator
Die folgende Tabelle zeigt das Ergebnis Gleichheitsvergleiche in SQL DocumentDB zwischen zwei JSON Typen.
<table style = "width:300px">
   <tbody>
      <tr>
         <td valign="top">
            <strong>Op</strong>
         </td>
         <td valign="top">
            <strong>Nicht definiert</strong>
         </td>
         <td valign="top">
            <strong>NULL</strong>
         </td>
         <td valign="top">
            <strong>Boolescher Wert</strong>
         </td>
         <td valign="top">
            <strong>Anzahl</strong>
         </td>
         <td valign="top">
            <strong>Zeichenfolge</strong>
         </td>
         <td valign="top">
            <strong>Objekt</strong>
         </td>
         <td valign="top">
            <strong>Array</strong>
         </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Nicht definiert<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>NULL<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Boolescher Wert<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Anzahl<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Zeichenfolge<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Objekt<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
         <td valign="top">
Nicht definiert </td>
      </tr>
      <tr>
         <td valign="top">
            <strong>Array<strong>
         </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
Nicht definiert </td>
         <td valign="top">
            <strong>Okay</strong>
         </td>
      </tr>
   </tbody>
</table>

Weitere Vergleichsoperatoren wie >, > =,! =, < und < = die folgenden Regeln angewendet:   

-   Vergleich über Typen führt undefiniert.
-   Vergleich zwischen zwei Objekte oder zwei arrays führt undefiniert.   

Ist das Ergebnis der skalaren Ausdruck im Filter nicht definiert ist, das entsprechende Dokument nicht direkt im Ergebnis, da Undefined logisch 'true' entsprechen nicht.

### <a name="between-keyword"></a>ZWISCHEN Schlüsselwort
Das BETWEEN-Schlüsselwort können Sie Abfragen Wertebereiche wie in ANSI SQL express. ZWISCHEN kann Zeichenfolgen oder Zahlen verwendet werden.

Diese Abfrage gibt z. B. alle Familie Dokumente in der ersten untergeordneten Klasse zwischen 1 bis 5 (beide einschließlich) zurück. 

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade BETWEEN 1 AND 5

Im Gegensatz zu in ANSI-SQL können auch die BETWEEN-Klausel in der FROM-Klausel wie im folgenden Beispiel Sie.

    SELECT (c.grade BETWEEN 0 AND 10)
    FROM Families.children[0] c

Schnellere Abfrage-Ausführungszeit Denken Sie daran, eine Indizierung Richtlinie erstellen, die einen Bereich Index für numerische Eigenschaften/Pfade verwendet, die in die BETWEEN-Klausel gefiltert werden. 

Der Hauptunterschied zwischen DocumentDB und ANSI-SQL mit ist gemischten Eigenschaften Bereichsabfragen express, z. B. möglicherweise "Qualität" eine Zahl (5) in einigen Dokumenten und Zeichenfolgen in anderen ("grade4"). In diesen Fällen wird wie in JavaScript ein Vergleich zwischen zwei verschiedenen Ergebnissen in "undefined" und dem Dokument übersprungen.

### <a name="logical-and-or-and-not-operators"></a>Logische (AND, OR und NOT) Operatoren
Logische Operatoren werden für boolesche Werte. Die logischen Tabellen Wahrheit für diese Operatoren werden in der folgenden Tabelle angezeigt.

ODER|True|False|Nicht definiert
---|---|---|---
True|True|True|True
False|True|False|Nicht definiert
Nicht definiert|True|Nicht definiert|Nicht definiert

UND|True|False|Nicht definiert
---|---|---|---
True|True|False|Nicht definiert
False|False|False|False
Nicht definiert|Nicht definiert|False|Nicht definiert

NICHT|  |
---|---
True|False
False|True
Nicht definiert|Nicht definiert

### <a name="in-keyword"></a>IN-Schlüsselwort
IN-Schlüsselwort kann verwendet werden, zu überprüfen, ob ein Wert in einer Liste angegebenen übereinstimmt. Diese Abfrage gibt z. B. alle Familie Dokumente ist die Id "WakefieldFamily" oder "AndersenFamily". 
 
    SELECT *
    FROM Families 
    WHERE Families.id IN ('AndersenFamily', 'WakefieldFamily')

Dieses Beispiel gibt alle Dokumente ist der Status der angegebenen Werte.

    SELECT *
    FROM Families 
    WHERE Families.address.state IN ("NY", "WA", "CA", "PA", "OH", "OR", "MI", "WI", "MN", "FL")

### <a name="ternary--and-coalesce--operators"></a>Ternär (?) und Operatoren Coalesce (?)
Bedingte Ausdrücke wie verbreiteten Programmiersprachen wie C# und JavaScript erstellen können Ternär und zusammengesetzten Operatoren verwendet werden. 

Operators Ternär (?) kann sehr nützlich sein, wenn neue JSON-Eigenschaften dynamisch erstellen. Jetzt z. B. können Sie Abfragen zum Klassifizieren von Klasse Ebenen in Menschen lesbare Form wie Anfänger/fortgeschrittene/Advanced wie folgt schreiben.
 
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel 
     FROM Families.children[0] c

Sie können auch die Aufrufe an den Operator wie in der folgenden Abfrage verschachteln.
 
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high")  AS gradeLevel 
    FROM Families.children[0] c

Wie werden mit anderen Abfrageoperatoren fehlen die referenzierten Eigenschaften im bedingten Ausdruck in jedem Dokument oder die verglichenen unterschiedlich sind diese Dokumente in den Abfrageergebnissen ausgeschlossen.

Der Operator Coalesce (?) lässt sich effizient überprüft das Vorhandensein einer Eigenschaft (auch bekannt als definiert) in einem Dokument. Dies ist nützlich für semistrukturierten Abfragen oder gemischte Datentypen. Diese Abfrage gibt z. B. "Nachname" gegebenenfalls oder "Nachname" Wenn es nicht vorhanden ist.

    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f

### <a name="quoted-property-accessor"></a>Anführungszeichen Eigenschaftenaccessor
Sie können auch Eigenschaften mit den angegebenen Eigenschaftenoperator zugreifen `[]`. Beispielsweise `SELECT c.grade` und `SELECT c["grade"]` entsprechen. Diese Syntax ist nützlich, wenn Sie müssen eine Eigenschaft, die Leerzeichen, Sonderzeichen enthält oder geschieht mit dem gleichen Namen wie ein SQL-Schlüsselwort oder reserviertes Wort.

    SELECT f["lastName"]
    FROM Families f
    WHERE f["id"] = "AndersenFamily"


## <a name="select-clause"></a>SELECT-Klausel
Die SELECT-Klausel (**`SELECT <select_list>`**) ist obligatorisch und gibt an, welche Werte werden von der Abfrage abgerufen wie ANSI SQL. Auf Quelldokumente gefilterte Teilmenge der Projektionsphase angegebenen JSON-Werte werden abgerufen, wobei ein JSON-Objekt erstellt, für jede Eingaben auf weitergegeben werden. 

Das folgende Beispiel zeigt eine normale SELECT-Abfrage. 

**Abfrage**

    SELECT f.address
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "address": {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }
    }]


### <a name="nested-properties"></a>Geschachtelte Eigenschaften
Im folgenden Beispiel wir zwei geschachtelte Eigenschaften projiziert `f.address.state` und `f.address.city`.

**Abfrage**

    SELECT f.address.state, f.address.city
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "state": "WA", 
      "city": "seattle"
    }]


Projektion unterstützt auch JSON-Ausdrücke, wie im folgenden Beispiel gezeigt.

**Abfrage**

    SELECT { "state": f.address.state, "city": f.address.city, "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle", 
        "name": "AndersenFamily"
      }
    }]


Betrachten Sie die Rolle des `$1` hier. Die `SELECT` Klausel muss ein JSON-Objekt erstellen und da kein Schlüssel angegeben ist, verwenden wir implizite Argument Variablennamen mit `$1`. Diese Abfrage gibt z. B. zwei implizite argumentvariablen mit der Bezeichnung `$1` und `$2`.

**Abfrage**

    SELECT { "state": f.address.state, "city": f.address.city }, 
           { "name": f.id }
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "$1": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "$2": {
        "name": "AndersenFamily"
      }
    }]


### <a name="aliasing"></a>Aliasing
Jetzt erweitern wir das Beispiel oben mit expliziten Aliasing Werte. Das Schlüsselwort für Aliasing ist. Ist optional, wie beim Projizieren des zweiten Wert als `NameInfo`. 

Bei eine Abfrage zwei Eigenschaften mit demselben Namen, muss Aliasing verwendet werden, eine oder beide der Eigenschaften umbenennen, damit sie das voraussichtliche Ergebnis auseinander gehalten werden.

**Abfrage**

    SELECT 
           { "state": f.address.state, "city": f.address.city } AS AddressInfo, 
           { "name": f.id } NameInfo
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
      "AddressInfo": {
        "state": "WA", 
        "city": "seattle"
      }, 
      "NameInfo": {
        "name": "AndersenFamily"
      }
    }]


### <a name="scalar-expressions"></a>Skalare Ausdrücke
Neben Eigenschaftenverweise unterstützt die SELECT-Klausel skalare Ausdrücke wie Konstanten, arithmetische Ausdrücke, logische Ausdrücke. Hier ist z. B. eine einfache "Hello World"-Abfrage.

**Abfrage**

    SELECT "Hello World"

**Ergebnisse**

    [{
      "$1": "Hello World"
    }]


Hier ist ein komplexeres Beispiel, das ein Skalarausdruck verwendet.

**Abfrage**

    SELECT ((2 + 11 % 7)-2)/3   

**Ergebnisse**

    [{
      "$1": 1.33333
    }]


Im folgenden Beispiel ist das Ergebnis der skalaren Ausdruck einen booleschen Wert.

**Abfrage**

    SELECT f.address.city = f.address.state AS AreFromSameCityState
    FROM Families f 

**Ergebnisse**

    [
      {
        "AreFromSameCityState": false
      }, 
      {
        "AreFromSameCityState": true
      }
    ]


### <a name="object-and-array-creation"></a>Objekt und Array erstellen
Eine weitere wichtige Funktion des DocumentDB SQL ist Array-Objekt erstellen. Beachten Sie im vorherigen Beispiel erstellt ein JSON-Objekt. Ebenso kann eine auch Arrays erstellen, wie in den folgenden Beispielen gezeigt.

**Abfrage**

    SELECT [f.address.city, f.address.state] AS CityState 
    FROM Families f 

**Ergebnisse**  

    [
      {
        "CityState": [
          "seattle", 
          "WA"
        ]
      }, 
      {
        "CityState": [
          "NY", 
          "NY"
        ]
      }
    ]

### <a name="value-keyword"></a>Wert-Schlüsselwort
Das **Wert** -Schlüsselwort ermöglicht das JSON-Wert zurückgeben. Unten gezeigte Abfrage gibt z. B. die skalare `"Hello World"` anstelle von `{$1: "Hello World"}`.

**Abfrage**

    SELECT VALUE "Hello World"

**Ergebnisse**

    [
      "Hello World"
    ]


Die folgende Abfrage gibt den JSON-Wert ohne den `"address"` Bezeichnung in den Ergebnissen.

**Abfrage**

    SELECT VALUE f.address
    FROM Families f 

**Ergebnisse**  

    [
      {
        "state": "WA", 
        "county": "King", 
        "city": "seattle"
      }, 
      {
        "state": "NY", 
        "county": "Manhattan", 
        "city": "NY"
      }
    ]

Das folgende Beispiel erweitert, wie JSON-primitive Werte (Endknotenebene der JSON-Struktur) zurückgegeben. 

**Abfrage**

    SELECT VALUE f.address.state
    FROM Families f 

**Ergebnisse**

    [
      "WA",
      "NY"
    ]


###<a name="-operator"></a>* Operator
Der spezielle Operator (*) wird das Dokument im Projekt-ist. Verwendet, muss er nur projizierten Bereich sein. Während eine Abfrage wie `SELECT * FROM Families f` gilt, `SELECT VALUE * FROM Families f ` und `SELECT *, f.id FROM Families f ` sind ungültig.

**Abfrage**

    SELECT * 
    FROM Families f 
    WHERE f.id = "AndersenFamily"

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

###<a name="top-operator"></a>TOP-Operator
Das TOP-Schlüsselwort kann verwendet werden, um Werte aus einer Abfrage einzuschränken. Wenn oben in Verbindung mit der ORDER BY-Klausel verwendet wird, wird das Resultset auf die ersten N Anzahl der bestellten Werte; andernfalls gibt die erste N Anzahl der Ergebnisse in einer nicht definierten Reihenfolge zurück. Es wird empfohlen in einer SELECT-Anweisung verwenden Sie eine ORDER BY-Klausel mit der TOP-Klausel. Dies ist die einzige Möglichkeit an vorhersehbar oben die Zeilen betroffen sind. 


**Abfrage**

    SELECT TOP 1 * 
    FROM Families f 

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]

TOP kann mit einem konstanten Wert (siehe oben) oder mit einem Variablen Wert parametrisierte Abfragen verwendet werden. Weitere Informationen finden Sie unter parametrisierte Abfragen.

## <a name="order-by-clause"></a>ORDER BY-Klausel
Wie in ANSI-SQL Sie optionale Order By-Klausel beim Abfragen können. Die Klausel kann eines optionalen Arguments der ASC/DESC um die Reihenfolge festzulegen, in der Ergebnisse abgerufen werden müssen. Eine ausführlichere Betrachtung Order By finden Sie in [DocumentDB Reihenfolge von exemplarischen Vorgehensweise](documentdb-orderby.md).

Hier ist z. B. eine Abfrage, die Familien in der Reihenfolge der residenten Namen abruft.

**Abfrage**

    SELECT f.id, f.address.city
    FROM Families f 
    ORDER BY f.address.city
    
**Ergebnisse**
    
    [
      {
        "id": "WakefieldFamily",
        "city": "NY"
      },
      {
        "id": "AndersenFamily",
        "city": "Seattle"   
      }
    ]

Und hier ist eine Abfrage, die als eine Zahl der Epoche gespeichert Familien in der Reihenfolge der Erstellungsdatum abrufen Zeit, z. B. verstrichene Zeit seit dem 1. Januar 1970 in Sekunden.

**Abfrage**

    SELECT f.id, f.creationDate
    FROM Families f 
    ORDER BY f.creationDate DESC
    
**Ergebnisse**
    
    [
      {
        "id": "WakefieldFamily",
        "creationDate": 1431620462
      },
      {
        "id": "AndersenFamily",
        "creationDate": 1431620472  
      }
    ]
    
## <a name="advanced-database-concepts-and-sql-queries"></a>Fortgeschrittene Konzepte und SQL-Abfragen
### <a name="iteration"></a>Iteration
Ein neues Konstrukt wurde über das **IN** -Schlüsselwort DocumentDB SQL unterstützen zum Durchlaufen von JSON-Arrays hinzugefügt. VON Quelle unterstützt Iteration. Beginnen Sie mit dem folgenden Beispiel:

**Abfrage**

    SELECT * 
    FROM Families.children

**Ergebnisse**  

    [
      [
        {
          "firstName": "Henriette Thaulow", 
          "gender": "female", 
          "grade": 5, 
          "pets": [{ "givenName": "Fluffy"}]
        }
      ], 
      [
        {
            "familyName": "Merriam", 
            "givenName": "Jesse", 
            "gender": "female", 
            "grade": 1
        }, 
        {
            "familyName": "Miller", 
            "givenName": "Lisa", 
            "gender": "female", 
            "grade": 8
        }
      ]
    ]

Jetzt sehen wir uns eine andere Abfrage, die Iteration für untergeordnete Elemente in der Auflistung ausführt. Beachten Sie den Unterschied in der Matrix. In diesem Beispiel wird `children` und fasst die Ergebnisse in einem einzigen Array.  

**Abfrage**

    SELECT * 
    FROM c IN Families.children

**Ergebnisse**  

    [
      {
          "firstName": "Henriette Thaulow",
          "gender": "female",
          "grade": 5,
          "pets": [{ "givenName": "Fluffy" }]
      },
      {
          "familyName": "Merriam",
          "givenName": "Jesse",
          "gender": "female",
          "grade": 1
      },
      {
          "familyName": "Miller",
          "givenName": "Lisa",
          "gender": "female",
          "grade": 8
      }
    ]

Diese Weitere lässt sich auf jedes einzelne Array filtern, wie im folgenden Beispiel gezeigt.

**Abfrage**

    SELECT c.givenName
    FROM c IN Families.children
    WHERE c.grade = 8

**Ergebnisse**  

    [{
      "givenName": "Lisa"
    }]

### <a name="joins"></a>Joins
In einer relationalen Datenbank ist die Notwendigkeit, Tabellen sehr wichtig. Es ist die logische Folge standardisierte Schemas entwerfen. Im Gegensatz dazu behandelt denormalisierte Datenmodell Schema Dokumente DocumentDB. Dies ist das logische Äquivalent einer "selbst-join".

Die Syntax der Programmiersprache unterstützt ist < from_source1 > < from_source2 > JOIN-Verknüpfung... JOIN < From_sourceN >. Insgesamt gibt eine Menge von **N**- Tupel (Tupel mit **N** ) zurück. Jedes Tupel hat Werte durch ihre jeweiligen legt alle Auflistungsaliase durchlaufen Anders gesagt ist dies eine vollständige Kreuzprodukt legt die Verknüpfung an.

Die folgenden Beispiele zeigen Funktionsweise der JOIN-Klausel. Im folgenden Beispiel das Ergebnis leer seit das Kreuzprodukt der einzelnen Dokumente aus Quelle und ein leerer Satz leer ist.

**Abfrage**

    SELECT f.id
    FROM Families f
    JOIN f.NonExistent

**Ergebnisse**  

    [{
    }]


Im folgenden Beispiel wird die Verknüpfung zwischen der Dokumentstamm und `children` unter Root. Es ist ein Kreuzprodukt zwischen zwei JSON-Objekte. Die Tatsache, dass Kinder ein Array wirkt nicht im JOIN da wir mit einem einzigen Stammverzeichnis handelt, die Kinder Array. Daher enthält das Ergebnis nur zwei Ergebnisse, da das Kreuzprodukt der einzelnen Dokumente mit dem Array genau nur ein Dokument ergibt.

**Abfrage**

    SELECT f.id
    FROM Families f
    JOIN f.children
 
**Ergebnisse**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]


Das folgende Beispiel zeigt eine konventionelle Verknüpfung:

**Abfrage**

    SELECT f.id
    FROM Families f
    JOIN c IN f.children 

**Ergebnisse**

    [
      {
        "id": "AndersenFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }, 
      {
        "id": "WakefieldFamily"
      }
    ]



Die erste ist zu beachten, die die `from_source` der **JOIN** -Klausel ist ein Iterator. Daher ist der Fluss in diesem Fall wie folgt:  

-   Erweitern Sie jedes untergeordnete Element **c** im Array.
-   Wenden Sie ein Kreuzprodukt mit dem Stamm des Dokuments **f** mit jedes untergeordnete Element **c** zunächst reduziert wurde.
-   Schließlich Projekteigenschaft den Stamm Objekt **f** Name allein. 

Das erste Dokument (`AndersenFamily`) enthält die Ergebnismenge nur ein einzelnes Objekt für dieses Dokument enthält nur ein untergeordnetes Element. Das zweite Dokument (`WakefieldFamily`) enthält zwei untergeordnete Elemente. Daher erzeugt das Kreuzprodukt ein separates Objekt für jedes Kind, wodurch zwei Objekte für jedes untergeordnete Element zu diesem Dokument. Beachten Sie, dass Root-Felder in beiden Dokumenten wie ein Kreuzprodukt gewünscht derselben.

Das wirkliche Hilfsprogramm der Verknüpfung ist Formular Tupel aus dem Kreuzprodukt in einer Form, die sonst nur schwer Projekt. Darüber, wie wir im folgenden Beispiel sehen werden, können Sie auf ein Tupel filtern, können die Benutzer eine Bedingung erfüllt, die Tupel insgesamt ausgewählt haben.

**Abfrage**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
 
**Ergebnisse**

    [
      {
        "familyName": "AndersenFamily", 
        "childFirstName": "Henriette Thaulow", 
        "petName": "Fluffy"
      }, 
      {
        "familyName": "WakefieldFamily", 
        "childGivenName": "Jesse", 
        "petName": "Goofy"
      }, 
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]



In diesem Beispiel wird eine natürliche Erweiterung des vorherigen Beispiels und führt eine doppelte Verknüpfung. So kann das Kreuzprodukt als im folgenden Pseudocode angezeigt werden.

    for-each(Family f in Families)
    {   
        for-each(Child c in f.children)
        {
            for-each(Pet p in c.pets)
            {
                return (Tuple(f.id AS familyName, 
                  c.givenName AS childGivenName, 
                  c.firstName AS childFirstName,
                  p.givenName AS petName));
            }
        }
    }

`AndersenFamily`hat ein Kind ein Haustier hat. Das Kreuzprodukt ergibt also eine Zeile (1*1*1) aus dieser Familie. WakefieldFamily jedoch zwei untergeordnete Elemente hat, jedoch nur eine untergeordnete "Jesse" Haustiere. Jesse hat 2 Haustiere. Somit liefert das Kreuzprodukt 1*1*2 = 2 Zeilen aus dieser Familie.

Das nächste Beispiel gibt ein zusätzlicher Filter auf `pet`. Dies schließt alle Tupel den Pet nicht "Schatten" ist. Beachten Sie, dass wir können Tupel von Arrays, Filter auf Elemente des Tupels, erstellen und eine beliebige Kombination der Elemente. 

**Abfrage**

    SELECT 
        f.id AS familyName,
        c.givenName AS childGivenName,
        c.firstName AS childFirstName,
        p.givenName AS petName 
    FROM Families f 
    JOIN c IN f.children 
    JOIN p IN c.pets
    WHERE p.givenName = "Shadow"

**Ergebnisse**

    [
      {
       "familyName": "WakefieldFamily", 
       "childGivenName": "Jesse", 
       "petName": "Shadow"
      }
    ]


## <a name="javascript-integration"></a>JavaScript-integration
DocumentDB enthält ein JavaScript-basierte Anwendungslogik direkt auf die Sammlungen in gespeicherten Prozeduren und Triggern ausgeführt. Dies ermöglicht sowohl:

-   Möglichkeit zur Höchstleistung Transaktionsvorgänge CRUD und Abfragen für Dokumente in einer Auflistung durch die Tiefe Integration von JavaScript Laufzeit direkt in der Datenbank-Engine. 
-   Eine natürliche Modellierung Kontrollfluss, Variable Bereichsdefinierung Zuordnung und Integration der Ausnahmebehandlung primitive mit Datenbanktransaktionen. Weitere Informationen zu DocumentDB-Unterstützung für JavaScript-Integration finden Sie in JavaScript Server Seite Programmierbarkeit Dokumentation.

###<a name="user-defined-functions-udfs"></a>Benutzerdefinierte Funktionen (UDFs)
Mit den in diesem Artikel bereits definierten Typen unterstützt DocumentDB SQL für benutzerdefinierte Funktion (UDF). Insbesondere werden skalare UDF unterstützt, können die Entwickler oder viele Argumente übergeben und ein einzelnes Argument Ergebnis zurück. Jedes dieser Argumente auf juristische JSON Werte überprüft werden.  

DocumentDB SQL-Syntax wird erweitert, um benutzerdefinierte Logik verwenden diese benutzerdefinierten Funktionen unterstützen. UDFs können mit DocumentDB registriert und dann als Teil einer SQL-Abfrage verwiesen werden. Tatsächlich sollen die UDFs vorzüglich Abfragen aufgerufen werden. Als Folge dieser Wahl UDFs auf das Kontextobjekt keinen mit die anderen JavaScript-Typen (gespeicherte Prozeduren und Trigger). Da Abfragen schreibgeschützt ausgeführt werden, können sie auf primären oder sekundären Replikate ausführen. Daher sollen UDFs auf sekundäre Replikate im Gegensatz zu anderen JavaScript ausführen.

Es folgt ein Beispiel, wie eine UDF-Datei in der Datenbank DocumentDB speziell unter eine registriert werden kann.

   
       UserDefinedFunction regexMatchUdf = new UserDefinedFunction
       {
           Id = "REGEX_MATCH",
           Body = @"function (input, pattern) { 
                       return input.match(pattern) !== null;
                   };",
       };
       
       UserDefinedFunction createdUdf = client.CreateUserDefinedFunctionAsync(
           UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
           regexMatchUdf).Result;  
                                                                             
Im vorherigen Beispiel erstellt eine UDF-Datei, deren Name `REGEX_MATCH`. Akzeptiert zwei Werte der JSON-Zeichenfolge `input` und `pattern` und überprüft, ob das erste Muster im zweiten angegebenen JavaScript string.match()-Funktion.


Wir können jetzt diese UDF in einer Abfrage in eine Projektion. UDFs müssen mit der Groß-/Kleinschreibung "Udf" qualifiziert Wenn Abfragen aus aufgerufen. 

>[AZURE.NOTE] Vor 3/17/2015 unterstützt DocumentDB UDF Aufrufe ohne "Udf." Präfix wie REGEX_MATCH() wählen. Dieser Aufruf Muster ist veraltet.  

**Abfrage**

    SELECT udf.REGEX_MATCH(Families.address.city, ".*eattle")
    FROM Families

**Ergebnisse**

    [
      {
        "$1": true
      }, 
      {
        "$1": false
      }
    ]

Der UDF-Datei kann auch in einem Filter verwendet werden, wie im folgenden Beispiel dargestellt mit dem "Udf" qualifiziert Präfix:

**Abfrage**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE udf.REGEX_MATCH(Families.address.city, ".*eattle")

**Ergebnisse**

    [{
        "id": "AndersenFamily",
        "city": "Seattle"
    }]


Im Wesentlichen UDFs sind gültige skalare Ausdrücke und Projektionen und Filtern verwendet werden. 

Erweitern auf die Leistungsfähigkeit des UDFs, betrachten wir ein weiteres Beispiel mit bedingter Logik:

       UserDefinedFunction seaLevelUdf = new UserDefinedFunction()
       {
           Id = "SEALEVEL",
           Body = @"function(city) {
                switch (city) {
                    case 'seattle':
                        return 520;
                    case 'NY':
                        return 410;
                    case 'Chicago':
                        return 673;
                    default:
                        return -1;
                    }"
            };

            UserDefinedFunction createdUdf = await client.CreateUserDefinedFunctionAsync(
                UriFactory.CreateDocumentCollectionUri("testdb", "families"), 
                seaLevelUdf);
    
    
Unten ist ein Beispiel, in dem das UDF ausführt.

**Abfrage**

    SELECT f.address.city, udf.SEALEVEL(f.address.city) AS seaLevel
    FROM Families f 

**Ergebnisse**

     [
      {
        "city": "seattle", 
        "seaLevel": 520
      }, 
      {
        "city": "NY", 
        "seaLevel": 410
      }
    ]


Wie die vorherigen Beispiele zeigen mit UDFs DocumentDB SQL eine umfangreiche programmierbare Schnittstelle um komplexe Verfahren, bedingte Logik mit Hilfe der integrierten JavaScript-Laufzeitfunktionen macht JavaScript-Sprache integriert.

DocumentDB SQL bietet die Argumente, die UDFs für jedes Dokument in der Quelle an die aktuelle Phase der Verarbeitung des UDFs (WHERE-Klausel oder SELECT-Klausel). Das Ergebnis wird in der gesamten Ausführungspipeline nahtlos integriert. Wenn Eigenschaften bezeichnet UDF Parameter sind nicht verfügbar in JSON-Wert, der Parameter als nicht definiert und daher der UDF-Aufruf vollständig übersprungen. Auch wenn das Ergebnis der UDF-Datei nicht definiert ist, nicht in das Ergebnis eingeschlossen. 

UDFs sind nützliche Tools, komplexe Geschäftslogik als Teil der Abfrage.

### <a name="operator-evaluation"></a>Operator-Bewertung
DocumentDB zeichnet den Vorzug einer JSON, parallelen mit JavaScript und seiner Bewertung Semantik. Während DocumentDB versucht, JavaScript-Semantik JSON-Unterstützung, weicht in einigen Fällen Vorgang Bewertung ab.

In DocumentDB SQL sind anders als in herkömmlichen SQL Werte häufig erst bekannt Werte tatsächlich aus der Datenbank abgerufen werden. Um Abfragen effektiv ausführen zu können, müssen die meisten strikte. 

DocumentDB SQL ausführen nicht implizite Konvertierungen im Gegensatz zu JavaScript. Beispielsweise eine Abfrage wie `SELECT * FROM Person p WHERE p.Age = 21` Dokumente enthalten, deren Wert 21 ist, Age-Eigenschaft entspricht. Alle anderen Dokumente, deren Alter-Eigenschaft Zeichenfolge "21" oder andere möglicherweise unendliche Variationen entspricht, wie "021", "21,0", "0021", "00021", usw. nicht übereinstimmen. Dies steht im Gegensatz zu JavaScript Zeichenfolgenwerte implizit in Zahlen umgewandelt sind (basierend auf Operator, ex: ==). Diese Option ist für effizienten Index DocumentDB SQL entsprechen. 

## <a name="parameterized-sql-queries"></a>Parametrisierte SQL-Abfragen
DocumentDB unterstützt Abfragen mit Parametern angegeben mit der bekannten @ Notation. Parametrisierte SQL bietet robuste behandeln und escaping von Benutzereingaben verhindert unbeabsichtigte Offenlegung von Daten durch SQL-Injektion. 

Sie können z. B. eine Abfrage, die Namen und Adresse Status als Parameter akzeptiert und führen für verschiedene Werte der Nachname und Adresse Status basierend auf Benutzereingaben.

    SELECT * 
    FROM Families f
    WHERE f.lastName = @lastName AND f.address.state = @addressState

Diese Anforderung kann dann DocumentDB als parametrisierte Abfrage wie JSON zugesandt unten.

    {      
        "query": "SELECT * FROM Families f WHERE f.lastName = @lastName AND f.address.state = @addressState",     
        "parameters": [          
            {"name": "@lastName", "value": "Wakefield"},         
            {"name": "@addressState", "value": "NY"},           
        ] 
    }

Das Argument TOP einstellbar parametrisierte Abfragen wie unten.

    {      
        "query": "SELECT TOP @n * FROM Families",     
        "parameters": [          
            {"name": "@n", "value": 10},         
        ] 
    }

Parameterwerte können alle gültigen JSON (Zeichenfolgen, Zahlen, boolesche Werte null auch Arrays oder JSON geschachtelt). Da DocumentDB Schema kleiner ist, werden Parameter auch nicht gegen überprüft.

##<a name="built-in-functions"></a>Integrierte Funktionen
DocumentDB unterstützt auch eine Reihe von integrierten Funktionen für allgemeine Operationen, die in Abfragen benutzerdefinierte Funktionen (UDFs) verwendet werden können.

<table>
<tr>
<td>Mathematische Funktionen</td> 
<td>ABS DECKE EXP, FLOOR, LOG, LOG10, POWER, runde, Zeichen, SQRT, QUADRAT, kürzen, ACOS, ARCSIN, ATAN, ATN2, COS COT Grad, PI, BOGENMAß, SIN und TAN</td>
</tr>
<tr>
<td>Überprüfung von Funktionen</td>    
<td>IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED und IS_PRIMITIVE</td>
</tr>
<tr>
<td>String-Funktionen</td>   
<td>CONCAT, CONTAINS ENDSWITH, INDEX_OF, links, Länge, unten, LTRIM, ersetzen, replizieren, REVERSE, rechts, RTRIM, STARTSWITH, TEILZEICHENFOLGE und oben</td>
</tr>
<tr>
<td>Array-Funktionen</td>    
<td>ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH und ARRAY_SLICE</td>
</tr>
<tr>
<td>Räumliche Funktionen</td>  
<td>ST_DISTANCE, ST_WITHIN, ST_ISVALID und ST_ISVALIDDETAILED</td>
</tr>
</table>  

Wenn Sie derzeit, eine benutzerdefinierte Funktion (UDF verwenden) für die integrierte Funktion steht, sollten Sie die entsprechende integrierte Funktion verwenden, wie es schneller ausgeführt und effizienter. 

### <a name="mathematical-functions"></a>Mathematische Funktionen
Mathematischen Funktionen führen eine Berechnung, in der Regel Eingabewerte, die als Argumente bereitgestellt und einen numerischen Wert zurückgeben. Hier ist eine Tabelle der unterstützten integrierten mathematischen Funktionen.

<table>
<tr>
<td><strong>Verwendung</strong></td>
<td><strong>Beschreibung</strong></td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_abs">ABS (Num_expr)</a></td> 
<td>Gibt den absoluten (positiven) Wert des angegebenen numerischen Ausdrucks zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ceiling">CEILING (Num_expr)</a></td> 
<td>Gibt die kleinste Ganzzahl größer oder gleich dem angegebenen numerischen Ausdruck zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_floor">FLOOR (Num_expr)</a></td> 
<td>Gibt der größten ganze Zahl kleiner oder gleich dem angegebenen numerischen Ausdruck.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_exp">EXP (Num_expr)</a></td> 
<td>Der Exponent des angegebenen numerischen Ausdrucks zurückgegeben.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log">LOG (Num_expr [Base])</a></td> 
<td>Gibt den natürlichen Logarithmus des angegebenen numerischen Ausdrucks oder den Logarithmus der angegebenen Basis</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_log10">LOG10 (Num_expr)</a></td> 
<td>Gibt den logarithmischen Basis 10-Wert des angegebenen numerischen Ausdrucks zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_round">ROUND (Num_expr)</a></td> 
<td>Gibt einen numerischen Wert auf die nächste ganze Zahl gerundet.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_trunc">TRUNC (Num_expr)</a></td> 
<td>Gibt einen numerischen Wert auf die nächste ganze Zahl gekürzt.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sqrt">SQRT (Num_expr)</a></td>   
<td>Gibt die Quadratwurzel des angegebenen numerischen Ausdrucks zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_square">QUADRAT (Num_expr)</a></td>   
<td>Gibt das Quadrat des angegebenen numerischen Ausdrucks zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_power">POWER (Num_expr, Num_expr)</a></td>   
<td>Macht den angegebenen numerischen Ausdruck gibt angegebene Wert.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sign">Zeichen (Num_expr)</a></td>   
<td>Gibt den Zeichen-Wert (1, 0, 1) des angegebenen numerischen Ausdrucks zurück.</td>
</tr>
<tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_acos">ACOS (Num_expr)</a></td>   
<td>Gibt den Winkel im Bogenmaß, dessen Kosinus der angegebene numerische Ausdruck ist; auch Arkuskosinus genannt.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_asin">ARCSIN (Num_expr)</a></td>   
<td>Gibt den Winkel im Bogenmaß, dessen Sinus der angegebene numerische Ausdruck ist. Dies wird auch als Arkussinus bezeichnet.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atan">ATAN (Num_expr)</a></td>   
<td>Gibt den Winkel im Bogenmaß, dessen Tangens der angegebene numerische Ausdruck ist. Dies wird auch als Arkustangens bezeichnet.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_atn2">ATN2 (Num_expr)</a></td>   
<td>Gibt den Winkel im Bogenmaß zwischen der positiven x-Achse und dem Strahl vom Ursprung zum Punkt (y, X), wobei x und y sind die Werte von zwei angegebenen Float-Ausdrücken.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cos">COS (Num_expr)</a></td> 
<td>Gibt den trigonometrischen Kosinus des angegebenen Winkels im Bogenmaß im angegebenen Ausdruck zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_cot">COT (Num_expr)</a></td> 
<td>Gibt den trigonometrischen Kotangens des angegebenen Winkels im Bogenmaß im angegebenen numerischen Ausdrucks zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_degrees">Grad (Num_expr)</a></td> 
<td>Gibt den entsprechenden Winkel in Grad im Bogenmaß angegebenen Winkels zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_pi">PI)</a></td>   
<td>Gibt den konstanten Wert von PI zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_radians">RAD (Num_expr)</a></td> 
<td>Gibt Bogenmaß Eingabe ein numerisches Ausdrucks in Grad.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_sin">SIN (Num_expr)</a></td> 
<td>Gibt den trigonometrischen Sinus des angegebenen Winkels im Bogenmaß im angegebenen Ausdruck zurück.</td>
</tr>
<tr>
<td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_tan">TAN (Num_expr)</a></td> 
<td>Gibt den Tangens des Eingabeausdrucks, in dem angegebenen Ausdruck zurück.</td>
</tr>

</table> 

Beispielsweise können Sie Abfragen wie folgt ausführen:

**Abfrage**

    SELECT VALUE ABS(-4)

**Ergebnisse**

    [4]

Der Hauptunterschied zwischen den DocumentDB im Vergleich zu ANSI SQL ist, dass sie mit weniger Schema und gemischten Schemadaten entwickelt. Haben Sie ein Dokument, in dem die Size-Eigenschaft fehlt oder hat, ein nicht numerischen Wert wie "Unbekannt" ein, dann Dokument wird übersprungen, anstatt einen Fehler.

### <a name="type-checking-functions"></a>Überprüfung von Funktionen
Prüfung Typ-Funktionen können Sie zum Überprüfen des Typs eines Ausdrucks in SQL-Abfragen. Typ überprüfen Funktionen können verwendet werden, um Variablen oder unbekannte Eigenschaften in Dokumenten dynamisch bestimmen. Hier ist eine Tabelle der unterstützten integrierten Funktion.

<table>
<tr>
  <td><strong>Verwendung</strong></td>
  <td><strong>Beschreibung</strong></td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_array">IS_ARRAY (Ausdruck)</a></td>
  <td>Gibt einen booleschen Wert zurück, wenn der Wert ein Array ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_bool">IS_BOOL (Ausdruck)</a></td>
  <td>Gibt einen booleschen Wert zurück, wenn der Typ des Werts einen booleschen Wert zurück.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_null">IS_NULL (Ausdruck)</a></td>
  <td>Gibt einen booleschen Wert zurück, wenn der Wert null ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_number">IS_NUMBER (Ausdruck)</a></td>
  <td>Gibt einen booleschen Wert zurück, wenn der Wert eine Zahl ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_object">IS_OBJECT (Ausdruck)</a></td>
  <td>Gibt einen booleschen Wert zurück, wenn der Typ des Werts ein JSON-Objekt ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_string">IS_STRING (Ausdruck)</a></td>
  <td>Gibt einen booleschen Wert zurück, wenn der Wert eine Zeichenfolge ist.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_defined">IS_DEFINED (Ausdruck)</a></td>
  <td>Gibt einen booleschen Wert zurück, wenn die Eigenschaft einen Wert zugewiesen wurde.</td>
</tr>
<tr>
  <td><a href="https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_is_primitive">IS_PRIMITIVE (Ausdruck)</a></td>
  <td>Gibt einen booleschen Wert zurück, wenn der Typ des Werts Zeichenfolge, Zahl, boolescher Wert oder Null zurück.</td>
</tr>

</table>

Mit diesen Funktionen können Sie Abfragen wie folgt ausführen:

**Abfrage**

    SELECT VALUE IS_NUMBER(-4)

**Ergebnisse**

    [true]

### <a name="string-functions"></a>String-Funktionen
Die folgenden skalaren Funktionen verarbeiten Zeichenfolgen-Eingabewerte und einer Zeichenfolge, numerischen oder einen booleschen Wert zurück. Hier ist eine Tabelle mit integrierten Funktionen:

Verwendung|Beschreibung
---|---
[Länge (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_length)|Gibt die Anzahl der Zeichen im angegebenen Zeichenfolgenausdruck
[CONCAT (Str_expr, Str_expr [, Str_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_concat)|Gibt eine Zeichenfolge aus zwei oder mehr String-Werte verkettet ist.
[SUBSTRING (Str_expr, Num_expr, Num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_substring)|Gibt einen Zeichenfolgenausdruck Teil.
["StartsWith" (Str_expr, Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_startswith)|Gibt ein Boolean, der angibt, ob die erste Ausdruck Zeichenfolge endet mit der zweiten
[ENDSWITH (Str_expr, Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_endswith)|Gibt ein Boolean, der angibt, ob die erste Ausdruck Zeichenfolge endet mit der zweiten
[ENTHÄLT (Str_expr, Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_contains)|Gibt ein Boolean, der angibt, ob die erste Ausdruck Zeichenfolge enthält die zweite.
[INDEX_OF (Str_expr, Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_index_of)|Gibt die Position des ersten Vorkommens des zweiten Zeichenfolgenausdruck innerhalb der ersten angegebenen Zeichenfolgenausdruck oder -1, wenn die Zeichenfolge nicht gefunden wird.
[Links (Str_expr, Num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_left)|Gibt den linken Teil einer Zeichenfolge mit der angegebenen Anzahl von Zeichen.
[RECHTS (Str_expr Num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_right)|Gibt den rechten Teil einer Zeichenfolge mit der angegebenen Anzahl von Zeichen.
[LTRIM (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_ltrim)|Gibt einen Zeichenfolgenausdruck zurück, nachdem führende Leerzeichen entfernt.
[RTRIM (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_rtrim)|Gibt einen Zeichenfolgenausdruck, nachdem alle nachfolgenden Leerzeichen abgeschnitten.
[UNTEN (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_lower)|Gibt einen Zeichenfolgenausdruck nach Zeichen von Großbuchstaben in Kleinbuchstaben.
[OBEN (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_upper)|Gibt einen Zeichenfolgenausdruck nachdem Kleinbuchstaben in Großbuchstaben konvertiert.
[Ersetzen (Str_expr, Str_expr, Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replace)|Ersetzt alle Vorkommen eines angegebenen Zeichenfolgenwerts durch einen anderen Zeichenfolgenwert.
[REPLICATE (Str_expr, Num_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_replicate)|Wiederholt einem Zeichenfolgenwert so oft wie angegeben.
[REVERSE (Str_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_reverse)|Gibt einen Zeichenfolgenwert umgekehrten Reihenfolge zurück.

Mit diesen Funktionen können Sie Abfragen wie folgt ausführen. Beispielsweise können Sie den Namen in Großbuchstaben wie folgt zurückgeben:

**Abfrage**

    SELECT VALUE UPPER(Families.id)
    FROM Families

**Ergebnisse**

    [
        "WAKEFIELDFAMILY", 
        "ANDERSENFAMILY"
    ]

Oder Verketten von Zeichenfolgen wie in diesem Beispiel:

**Abfrage**

    SELECT Families.id, CONCAT(Families.address.city, ",", Families.address.state) AS location
    FROM Families

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
      "location": "NY,NY"
    },
    {
      "id": "AndersenFamily",
      "location": "seattle,WA"
    }]


Zeichenfolgenfunktionen können auch in der WHERE-Klausel zum Filtern der Ergebnisse, wie im folgenden Beispiel verwendet werden:

**Abfrage**

    SELECT Families.id, Families.address.city
    FROM Families
    WHERE STARTSWITH(Families.id, "Wakefield")

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
      "city": "NY"
    }]

### <a name="array-functions"></a>Array-Funktionen
Die folgenden skalaren Funktionen führen eine Operation auf ein Array Eingabewert und numerische Eingabe, Boolean oder Array. Hier ist eine Tabelle integrierte Array-Funktionen:

Verwendung|Beschreibung
---|---
[ARRAY_LENGTH (Arr_expr)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_length)|Gibt die Anzahl der Elemente von dem angegebenen Ausdruck zurück.
[ARRAY_CONCAT (Arr_expr, Arr_expr [, Arr_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_concat)|Gibt ein Array aus zwei oder mehr Arraywerte verkettet ist.
[ARRAY_CONTAINS (Arr_expr, Ausdruck)](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_contains)|Gibt einen booleschen Wert Array angegebenen Wert enthält.
[ARRAY_SLICE (Arr_expr, Num_expr [, Num_expr])](https://msdn.microsoft.com/library/azure/dn782250.aspx#bk_array_slice)|Gibt ein Arrayausdruck Teil.

Arrayfunktionen können verwendet werden, zur Bearbeitung von Arrays in JSON. Hier ist z. B. eine Abfrage, die alle Dokumente zurückgegeben, bei denen die Eltern "Robin Wakefield". 

**Abfrage**

    SELECT Families.id 
    FROM Families 
    WHERE ARRAY_CONTAINS(Families.parents, { givenName: "Robin", familyName: "Wakefield" })

**Ergebnisse**

    [{
      "id": "WakefieldFamily"
    }]

Hier ist ein anderes Beispiel, ARRAY_LENGTH die Anzahl der Kinder pro Familie.

**Abfrage**

    SELECT Families.id, ARRAY_LENGTH(Families.children) AS numberOfChildren
    FROM Families 

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
      "numberOfChildren": 2
    },
    {
      "id": "AndersenFamily",
      "numberOfChildren": 1
    }]

### <a name="spatial-functions"></a>Räumliche Funktionen

DocumentDB unterstützt die folgenden Open Geospatial Consortium (OGC) integrierten Funktionen für Geodaten Abfragen. Weitere Informationen zu Geodaten in DocumentDB unterstützen, siehe [Arbeiten mit Geodaten in Azure DocumentDB](documentdb-geospatial.md). 

<table>
<tr>
  <td><strong>Verwendung</strong></td>
  <td><strong>Beschreibung</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (Point_expr, Point_expr)</td>
  <td>Gibt den Abstand zwischen den beiden GeoJSON Punkt Ausdrücken.</td>
</tr>
<tr>
  <td>ST_WITHIN (Point_expr, Polygon_expr)</td>
  <td>Gibt einen booleschen Ausdruck, ob im ersten Argument angegebene GeoJSON Punkt innerhalb des Polygons GeoJSON im zweiten Argument ist.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Gibt einen booleschen Wert, der angibt, ob der angegebene GeoJSON Punkt oder Polygon Ausdruck gültig ist.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Gibt eine JSON mit einem booleschen Wert Wenn der angegebene GeoJSON Punkt oder Polygon Ausdruck gültig ist und ungültige, außerdem der Grund als Zeichenfolgenwert.</td>
</tr>
</table>

Räumliche Funktionen dienen Nähe Steinbrüchen räumliche Daten ausführen. Hier ist z. B. eine Abfrage, die alle Familie Dokumente zurückgibt, die innerhalb von 30 km von der angegebenen Position mit der integrierten ST_DISTANCE-Funktion. 

**Abfrage**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Ergebnisse**

    [{
      "id": "WakefieldFamily"
    }]

Enthält der räumliche Indizierung die Indizierung Richtlinie werden "Abstand Abfragen" effizient über den Index bereitgestellt. Weitere Informationen über räumliche Indizierung finden Sie im Abschnitt unten. Haben Sie einen räumlichen Index für die angegebenen Pfade, Sie können weiterhin raumbezogener Abfragen von ausführen `x-ms-documentdb-query-enable-scan` Anfrage-Header mit dem Wert "true". In .NET kann dies durch das optionale Argument **FeedOptions** Abfragen mit [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) auf True übergeben. 

ST_WITHIN kann verwendet werden, zu überprüfen, ob ein Punkt innerhalb eines Polygons liegt. Polygone dienen häufig Grenzen wie Postleitzahlen, Grenzen oder natürliche darstellen. Wieder enthält der räumliche Indizierung die Indizierung Richtlinie dann "in" Abfragen effizient über den Index servieren. 

Polygon Argumente im ST_WITHIN darf nur ein einziges Rufzeichen, Polygone müssen also keine Löcher. Überprüfen der [DocumentDB Grenzwerte](documentdb-limits.md) für die maximale Anzahl von Punkten in einem Polygon für eine ST_WITHIN Abfrage zulässig.

**Abfrage**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Ergebnisse**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Ähnlich wie nicht übereinstimmende Typen arbeiten DocumentDB Abfrage, wenn der Speicherort im angegebene Argument ist fehlerhaft oder ungültig und bewertet **nicht definiert** und bewertet Dokument aus den Abfrageergebnissen übersprungen werden. Wenn die Abfrage keine Ergebnisse zurückgibt, ST_ISVALIDDETAILED ausführen, Debuggen, warum Spatail ist ungültig.     

ST_ISVALID und ST_ISVALIDDETAILED kann verwendet werden, zu überprüfen, ob ein räumliches Objekt gültig ist. Die folgende Abfrage überprüft beispielsweise die Gültigkeit eines Punkts mit einem Latitude Bereichswert (-132.8). ST_ISVALID gibt einen booleschen Wert zurück und ST_ISVALIDDETAILED gibt den booleschen Wert und eine Zeichenfolge mit der Grund, warum er ungültig ist.

**Abfrage**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Ergebnisse**

    [{
      "$1": false
    }]

Diese Funktionen können auch verwendet werden, Polygone zu überprüfen. Beispielsweise verwenden Sie hier wir ST_ISVALIDDETAILED ein Polygon überprüfen, die nicht geschlossen ist. 

**Abfrage**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Ergebnisse**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
Das schließt die räumliche Funktionen und die SQL-Syntax für DocumentDB. Jetzt sehen wir uns an wie LINQ-Abfragen arbeiten und Ihre Interaktion mit der Syntax wir bisher.

## <a name="linq-to-documentdb-sql"></a>LINQ to SQL DocumentDB
LINQ ist ein komponentenbasierter, die Berechnung als Abfragen auf Objekte gibt. DocumentDB stellt einen Client Seite Bibliothek Schnittstelle mit LINQ erleichtern eine Konvertierung zwischen JSON und .NET und eine Zuordnung aus einer Teilmenge von LINQ Abfragen an DocumentDB Abfragen. 

Die folgende Abbildung zeigt die Architektur von LINQ-Abfragen mit DocumentDB unterstützen.  DocumentDB-Client verwenden, können Entwickler ein **IQueryable** -Objekt erstellen, das direkt Abfragen Abfrageanbieter DocumentDB übersetzt dann die LINQ-Abfrage in einer Abfrage DocumentDB. Die Abfrage wird dann mit dem DocumentDB-Server zum Abrufen der Ergebnisse im JSON-Format übergeben. Die zurückgegebenen Ergebnisse werden in einen Stream aus auf der Clientseite deserialisiert.

![Architektur unterstützt LINQ Abfragen mit DocumentDB - SQL-Syntax, JSON-Abfragesprache Datenbankkonzepte und SQL-Abfragen][1]
 


### <a name="net-and-json-mapping"></a>.NET und JSON-Zuordnung
Die Zuordnung zwischen JSON-Dokumente ist natürlich - ein JSON-Objekt, wobei der Feldname "Key" Teil des Objekts zugeordnet und Bereich "Wert" rekursiv der Wertteil des Objekts zugeordnet jedes Mitglied Datenfeld zugeordnet ist. Beispiel. JSON-Dokument wie folgt erstellte Familie-Objekt zugeordnet. Und umgekehrt JSON-Dokument wird wieder ein.

**C#-Klasse**

    public class Family
    {
        [JsonProperty(PropertyName="id")]
        public string Id;
        public Parent[] parents;
        public Child[] children;
        public bool isRegistered;
    };
    
    public struct Parent
    {
        public string familyName;
        public string givenName;
    };
    
    public class Child
    {
        public string familyName;
        public string givenName;
        public string gender;
        public int grade;
        public List<Pet> pets;
    };
    
    public class Pet
    {
        public string givenName;
    };
    
    public class Address
    {
        public string state;
        public string county;
        public string city;
    };
    
    // Create a Family object.
    Parent mother = new Parent { familyName= "Wakefield", givenName="Robin" };
    Parent father = new Parent { familyName = "Miller", givenName = "Ben" };
    Child child = new Child { familyName="Merriam", givenName="Jesse", gender="female", grade=1 };
    Pet pet = new Pet { givenName = "Fluffy" };
    Address address = new Address { state = "NY", county = "Manhattan", city = "NY" };
    Family family = new Family { Id = "WakefieldFamily", parents = new Parent [] { mother, father}, children = new Child[] { child }, isRegistered = false };


**JSON**  

    {
        "id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam", 
                "givenName": "Jesse", 
                "gender": "female", 
                "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            { 
              "familyName": "Miller", 
              "givenName": "Lisa", 
              "gender": "female", 
              "grade": 8 
            }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    };



### <a name="linq-to-sql-translation"></a>LINQ to SQL-Übersetzung
DocumentDB-Abfrageanbieter führt am besten Aufwand Zuordnung aus einer LINQ-Abfrage in eine DocumentDB SQL-Abfrage. In der folgenden Beschreibung angenommen hat der Leser von LINQ vertraut sind.

Für das Typsystem unterstützen wir zunächst alle JSON primitive Typen – numerische Typen, Boolean, String und Null. Diese JSON-Typen werden unterstützt. Die folgenden skalaren Ausdrücken werden unterstützt.

-   Konstanten – dazu gehören Konstanten primitive Datentypen zum Zeitpunkt die Abfrage ausgewertet wird.

-   Array-Eigenschaft Indexausdrücke – diese Ausdrücke verweisen auf die Eigenschaft eines Objekts oder ein Arrayelement.

        family.Id;
        family.children[0].familyName;
        family.children[0].grade;
        family.children[n].grade; //n is an int variable

-   Arithmetische Ausdrücke - gehören allgemeine arithmetische Ausdrücke auf numerische und boolesche Werte. Eine vollständige Liste finden Sie in der SQL-Spezifikation.

        2 * family.children[0].grade;
        x + y;

-   Vergleich Zeichenfolgenausdruck - diese vergleichen einen Wert einen Konstanten Zeichenfolgenwert enthalten.  
 
        mother.familyName == "Smith";
        child.givenName == s; //s is a string variable

-   / Objektarray Ausdruck - diese Ausdrücke zurück anonymen Typ oder zusammengesetzten Wert ein Objekt oder ein Objektarray. Diese Werte können geschachtelt werden.

        new Parent { familyName = "Smith", givenName = "Joe" };
        new { first = 1, second = 2 }; //an anonymous type with 2 fields              
        new int[] { 3, child.grade, 5 };

### <a name="list-of-supported-linq-operators"></a>Liste der unterstützten LINQ-Operatoren
Hier wird eine Liste der unterstützten LINQ-Operatoren in der LINQ-Anbieter mit dem DocumentDB .NET SDK enthalten.

-   **Auswählen**: Projektionen übersetzen Objekts einschließlich SQL auswählen
-   **Wo**: Filter, SQL, übersetzen und unterstützt Konvertierung zwischen & &, || und! die SQL-Operatoren
-   **SelectMany**: ermöglicht das Entladen des Arrays SQL JOIN-Klausel. Kann zur Kette/Ausdrücke Arrayelemente Filtern verschachteln
-   **OrderBy und OrderByDescending**: ORDER BY Sortierreihenfolge entspricht:
-   **CompareTo**: Bereich Vergleiche entspricht. Häufig für Zeichenfolgen verwendet werden, da sie nicht mit .NET
-   **Übernehmen**: SQL oben zum Einschränken der Ergebnisse einer Abfrage entspricht
-   **Mathematische Funktionen**: Übersetzung unterstützt. NET Abs Acos, ARCSIN Atan Decke, Cos Exp Floor, Log, Log10, Pow, runde, Zeichen, Sin, Sqrt, Tan, Truncate entsprechende integrierte SQL-Funktionen.
-   **Funktionen**: Übersetzung unterstützt. NET Concat, Contains, EndsWith IndexOf Count, ToLower, TrimStart, ersetzen, Reverse, TrimEnd, StartsWith, SubString, ToUpper entsprechende integrierte SQL-Funktionen.
-   **Array-Funktionen**: Übersetzung unterstützt. NET Concat enthält und auf entsprechenden integrierten SQL-Funktionen.
-   **Geodaten-Erweiterungsfunktionen**: Übersetzung Stubmethoden Abstand IsValid und IsValidDetailed die entsprechenden integrierten SQL-Funktionen unterstützt.
-   **Benutzerdefinierte Funktion Erweiterungsfunktion**: unterstützt Übersetzung Stub-Methode UserDefinedFunctionProvider.Invoke für den entsprechenden Benutzer definierte Funktion.
-   **Sonstiges**: Übersetzung der zusammengesetzte Bedingte Operatoren unterstützt. Enthält Zeichenfolge enthält, ARRAY_CONTAINS oder je nach Kontext IN SQL übersetzen.

### <a name="sql-query-operators"></a>SQL-Operatoren
Es folgen einige Beispiele, die veranschaulichen, wie LINQ-Standardabfrageoperatoren auf DocumentDB Abfragen übersetzt werden.

#### <a name="select-operator"></a>Operator auswählen
Die Syntax lautet `input.Select(x => f(x))`, wobei `f` ist ein skalarer Ausdruck.

**LINQ Lambda-Ausdruck**

    input.Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f



**LINQ Lambda-Ausdruck**

    input.Select(family => family.children[0].grade + c); // c is an int variable


**SQL** 

    SELECT VALUE f.children[0].grade + c
    FROM Families f 



**LINQ Lambda-Ausdruck**

    input.Select(family => new
    {
        name = family.children[0].familyName,
        grade = family.children[0].grade + 3
    });


**SQL** 

    SELECT VALUE {"name":f.children[0].familyName, 
                  "grade": f.children[0].grade + 3 }
    FROM Families f



#### <a name="selectmany-operator"></a>SelectMany-operator
Die Syntax lautet `input.SelectMany(x => f(x))`, wobei `f` ist ein skalarer Ausdruck Auflistung zurück.

**LINQ Lambda-Ausdruck**

    input.SelectMany(family => family.children);

**SQL** 

    SELECT VALUE child
    FROM child IN Families.children



#### <a name="where-operator"></a>Dem Operator
Die Syntax lautet `input.Where(x => f(x))`, wobei `f` ist ein skalarer Ausdruck, der einen booleschen Wert zurückgibt.

**LINQ Lambda-Ausdruck**

    input.Where(family=> family.parents[0].familyName == "Smith");

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith" 



**LINQ Lambda-Ausdruck**

    input.Where(
        family => family.parents[0].familyName == "Smith" && 
        family.children[0].grade < 3);

**SQL** 

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"
    AND f.children[0].grade < 3


### <a name="composite-sql-queries"></a>Zusammengesetzte SQL-Abfragen
Die oben aufgeführten Operatoren können zu leistungsfähiger Abfragen bestehen. Da DocumentDB verschachtelte unterstützt, werden die Komposition verkettet oder geschachtelt.

#### <a name="concatenation"></a>Verkettung 

Die Syntax lautet `input(.|.SelectMany())(.Select()|.Where())*`. Eine verkettete Abfrage mit einem optionalen starten `SelectMany` Abfrage gefolgt von mehreren `Select` oder `Where` Operatoren.


**LINQ Lambda-Ausdruck**

    input.Select(family=>family.parents[0])
        .Where(familyName == "Smith");

**SQL**

    SELECT *
    FROM Families f
    WHERE f.parents[0].familyName = "Smith"



**LINQ Lambda-Ausdruck**

    input.Where(family => family.children[0].grade > 3)
        .Select(family => family.parents[0].familyName);

**SQL** 

    SELECT VALUE f.parents[0].familyName
    FROM Families f
    WHERE f.children[0].grade > 3



**LINQ Lambda-Ausdruck**

    input.Select(family => new { grade=family.children[0].grade}).
        Where(anon=> anon.grade < 3);
            
**SQL** 

    SELECT *
    FROM Families f
    WHERE ({grade: f.children[0].grade}.grade > 3)



**LINQ Lambda-Ausdruck**

    input.SelectMany(family => family.parents)
        .Where(parent => parents.familyName == "Smith");

**SQL** 

    SELECT *
    FROM p IN Families.parents
    WHERE p.familyName = "Smith"



#### <a name="nesting"></a>Verschachteln

Die Syntax lautet `input.SelectMany(x=>x.Q())` Q ist ein `Select`, `SelectMany`, oder `Where` Operator.

Die innere Abfrage wird in einer geschachtelten Abfrage auf jedes Element der äußeren Auflistung angewendet. Wichtig ist, dass die innere Abfrage Felder der Elemente in der äußeren Auflistung wie finden Selbstjoins.

**LINQ Lambda-Ausdruck**

    input.SelectMany(family=> 
        family.parents.Select(p => p.familyName));

**SQL** 

    SELECT VALUE p.familyName
    FROM Families f
    JOIN p IN f.parents


**LINQ Lambda-Ausdruck**

    input.SelectMany(family => 
        family.children.Where(child => child.familyName == "Jeff"));
            
**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = "Jeff"



**LINQ Lambda-Ausdruck**
            
    input.SelectMany(family => family.children.Where(
        child => child.familyName == family.parents[0].familyName));

**SQL** 

    SELECT *
    FROM Families f
    JOIN c IN f.children
    WHERE c.familyName = f.parents[0].familyName


## <a name="executing-sql-queries"></a>Ausführen von SQL-Abfragen
DocumentDB stellt Ressourcen über eine REST-API, die von jeder Sprache, die HTTP/HTTPS-Anforderungen aufgerufen werden kann. Darüber hinaus bietet DocumentDB Programmierbibliotheken für verschiedene gängige Sprachen wie .NET Node.js, JavaScript und Python. Die REST-API und die verschiedenen Bibliotheken unterstützt Abfragen über SQL. .NET SDK unterstützt LINQ neben SQL Abfragen.

Folgende Beispiele zeigen, wie eine Abfrage erstellen und gegen eine Datenbank DocumentDB senden.

### <a name="rest-api"></a>REST-API
DocumentDB bietet eine offene RESTful Programmiermodell über HTTP. Datenbankkonten können mit Azure-Abonnement bereitgestellt werden. DocumentDB-Ressourcenmodell besteht aus einer Ressourcen Datenbankkonto, von denen jeder mit einem logischen und stabile URI adressierbar sind. Eine Reihe von Ressourcen wird in diesem Dokument als Feed bezeichnet. Datenbankkonto besteht aus einer Reihe von Datenbanken, jeweils mehrere Sammlungen enthalten jeweils die wiederum Dokumente, UDF-Dateien und andere Typen enthalten.

Grundlegende Interaktionsmodell mit diesen Ressourcen wird über die HTTP-Verben GET, PUT, POST und Löschen mit ihren standard. Das POST-Verb ist für Erstellung einer neuen Ressource, zum Ausführen einer gespeicherten Prozedur oder Ausgeben einer DocumentDB Abfrage verwendet. Abfragen werden immer nur mit keine Nebeneffekte Lesevorgänge.

Die folgenden Beispiele zeigen einen Beitrag für eine DocumentDB Abfrage gegen eine Auflistung mit zwei Beispieldokumente wir bisher überprüft. Die Abfrage enthält einen einfachen Filter auf die Namenseigenschaft JSON. Beachten Sie die `x-ms-documentdb-isquery` und Content-Type: `application/query+json` zu kennzeichnen, dass der Vorgang eine Abfrage ist.


**Anforderung**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {      
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",     
        "parameters": [          
            {"name": "@familyId", "value": "AndersenFamily"}         
        ] 
    }
    

**Ergebnisse**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }


Das zweite Beispiel zeigt eine komplexe Abfrage, die die Verknüpfung mehrerer Ergebnisse zurückgibt.

**Anforderung**

    POST https://<REST URI>/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json
    
    {      
        "query": "SELECT 
                     f.id AS familyName, 
                     c.givenName AS childGivenName, 
                     c.firstName AS childFirstName, 
                     p.givenName AS petName 
                  FROM Families f 
                  JOIN c IN f.children 
                  JOIN p in c.pets",     
        "parameters": [] 
    }


**Ergebnisse**

    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84
    
    <indented for readability, results highlighted>
    
    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }


Die Ergebnisse einer Abfrage in einer einzelnen Ergebnisse passt nicht, wenn die REST-API gibt ein Fortsetzungstoken durch die `x-ms-continuation-token` Antwort-Header. Clients können durch Einfügen des Header in nachfolgenden Ergebnisse Ergebnisse paginiert. Die Anzahl der Ergebnisse pro Seite auch gesteuert werden durch die `x-ms-max-item-count` Zahl Header.

Die Konsistenz Datenrichtlinien für Abfragen verwalten die `x-ms-consistency-level` Header wie alle REST API-Anfragen. Session-Konsistenz ist auch die neuesten echo `x-ms-session-token` Cookie-Header in der Anforderung. Beachten Sie, dass der abgefragten Auflistung indizieren Richtlinie auch die Konsistenz der Ergebnisse beeinflussen kann. Standardmäßig indizieren Richtlinien für Sammlungen Index ist immer mit dem Inhalt des Dokuments und Ergebnisse entsprechen die Konsistenz für Daten ausgewählt. Wenn die Indizierung Richtlinie Lazy entspannt, können Abfragen veraltete Ergebnisse zurück. Weitere Informationen finden Sie auf [DocumentDB Konsistenz] [consistency-levels].

Wenn die Indizierung konfigurierte Richtlinie die Auflistung die angegebene Abfrage nicht unterstützt, gibt der Server DocumentDB 400 "Bad Request". Für Bereichsabfragen Konfigurationspfade Hash (Gleichheit) suchen und Indizierung explizit ausgeschlossene Pfade zurückgegeben. Die `x-ms-documentdb-query-enable-scan` Header kann angegeben werden, zu der Abfrage einen Scan ausführen, wenn ein Index nicht verfügbar ist.

### <a name="c-net-sdk"></a>C# (.NET) SDK
.NET SDK unterstützt LINQ und SQL Abfragen. Im folgenden Beispiel wird veranschaulicht, wie für die einfache Filterabfrage bereits eingeführt.


    foreach (var family in client.CreateDocumentQuery(collectionLink, 
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(collectionLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(collectionLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in client.CreateDocumentQuery(collectionLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Dieses Beispiel vergleicht zwei Eigenschaften Gleichheit in jedem Dokument und anonyme Projektionen verwendet. 


    foreach (var family in client.CreateDocumentQuery(collectionLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family 
        FROM Families f 
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }
    
    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(collectionLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }
    
    foreach (var family in
        client.CreateDocumentQuery<Family>(collectionLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }


Das nächste Beispiel zeigt ausgedrückt durch LINQ SelectMany-Joins.


    foreach (var pet in client.CreateDocumentQuery(collectionLink,
          @"SELECT p
            FROM Families f 
                 JOIN c IN f.children 
                 JOIN p in c.pets 
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }
    
    // Equivalent in Lambda expressions
    foreach (var pet in
        client.CreateDocumentQuery<Family>(collectionLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }



Client .NET durchläuft automatisch alle Seiten von Abfrageergebnissen in der Foreach-Blöcke wie oben dargestellt. Die Optionen im Abschnitt REST-API eingeführt stehen im .NET SDK mithilfe der `FeedOptions` und `FeedResponse` Klassen in der CreateDocumentQuery-Methode. Die Anzahl der Seiten kann gesteuert werden die `MaxItemCount` Einstellung. 

Können Sie auch explizit steuern Auslagerungsdatei erstellen `IDocumentQueryable` mit dem `IQueryable` Objekt dann anhand der` ResponseContinuationToken` sollten Sie Werte und Weitergabe `RequestContinuationToken` in `FeedOptions`. `EnableScanInQuery`festlegen, Scans aktivieren, wenn die Abfrage durch die Indizierung konfigurierte Richtlinie unterstützt werden kann. Partitionierte Sammlungen können `PartitionKey` zum Ausführen der Abfrage für eine einzelne partition (obwohl DocumentDB automatisch diese aus den Abfragetext extrahiert werden kann), und `EnableCrossPartitionQuery` Abfragen ausführen, die mehrere Partitionen ausgeführt werden. 

Weitere Beispiele mit Abfragen finden Sie unter [.NET DocumentDB Beispiele](https://github.com/Azure/azure-documentdb-net) . 

### <a name="javascript-server-side-api"></a>JavaScript serverseitige API 
DocumentDB enthält ein JavaScript-basierte Anwendungslogik direkt auf die Sammlungen mit gespeicherten Prozeduren und Triggern ausgeführt. Auf einer Ebene registrierte JavaScript-Logik kann dann Datenbankoperationen der Operationen auf der angegebenen Auflistung ausstellen. Diese Vorgänge werden in der ACID-Transaktionen eingeschlossen.

Das folgende Beispiel zeigt die Verwendung der QueryDocuments in der JavaScript-API-Server Abfragen von innerhalb von gespeicherten Prozeduren und Triggern.


    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();
        var collectionLink = collectionManager.getSelfLink()
    
        // create a new document.
        collectionManager.createDocument(collectionLink,
            { name: name, author: author },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);
    
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);
    
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don't need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }

## <a name="aggregate-functions"></a>Aggregatfunktionen

Systemeigene Unterstützung für Aggregatfunktionen wird, aber wenn Sie Anzahl oder Summe in der Funktionsumfang, erzielen Sie dasselbe Ergebnis mit verschiedenen Methoden.  

Lesen Sie Pfad:

- Sie können Aggregatfunktionen durch Abrufen der Daten und Anzahl lokal ausführen. Es ist ratsam, eine billige Abfrageprojektion wie verwenden `SELECT VALUE 1` als Volltext wie `SELECT * FROM c`. Dadurch wird die Anzahl der Dokumente, die auf jeder Seite der Ergebnisse, wodurch zusätzliche Roundtrips an den Dienst bei Bedarf verarbeitet maximieren.
- Eine gespeicherte Prozedur können Sie um Netzwerklatenz wiederholte Roundtrips zu minimieren. Ein gespeicherter Prozedur, der Anzahl die für eine Abfrage angegebenen Filter berechnet finden Sie unter [Count.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/stored-procedures/Count.js). Die gespeicherte Prozedur können Benutzer umfangreiche Geschäftslogik und Aggregationen effizient ausführen kombinieren.

Write-Pfad:

- Ein anderes allgemeines Muster ist die Ergebnisse im Pfad "Write" vorab aggregiert. Dies ist besonders interessant, wenn Abfragen "Lesen", "Schreiben" Anfragen übersteigt. Einmal vorab aggregiert, die Ergebnisse sind mit einer Anforderung gelesen.  Die besten DocumentDB vorab aggregieren ist ein Trigger, der mit "Schreiben" aufgerufen und mit der aktuellen Ergebnisse für die Abfrage, die materialisiert werden Metadatendokument aktualisieren. Beispielsweise überprüfen Sie das [UpdateaMetadata.js](https://github.com/Azure/azure-documentdb-js-server/blob/master/samples/triggers/UpdateMetadata.js) -Beispiel MinSize, MaxSize und TotalSize Metadatendokument für die Auflistung aktualisiert. Im Beispiel kann erweitert werden, um einen Zähler, Summe usw. zu aktualisieren.

##<a name="references"></a>Referenzen
1.  [Einführung in Azure DocumentDB][introduction]
2.  [DocumentDB SQL-Spezifikation](http://go.microsoft.com/fwlink/p/?LinkID=510612)
3.  [DocumentDB .NET-Beispiele](https://github.com/Azure/azure-documentdb-net)
4.  [DocumentDB Konsistenzebenen][consistency-levels]
5.  ANSI SQL 2011 [http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681](http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
6.  JSON- [http://json.org/](http://json.org/)
7.  JavaScript-Spezifikation [http://www.ecma-international.org/publications/standards/Ecma-262.htm](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 
8.  LINQ- [http://msdn.microsoft.com/library/bb308959.aspx](http://msdn.microsoft.com/library/bb308959.aspx) 
9.  Bewertung Abfragetechniken für große Datenbanken [http://dl.acm.org/citation.cfm?id=152611](http://dl.acm.org/citation.cfm?id=152611)
10. Verarbeiten von Abfragen in parallele relationalen Datenbanksystemen, IEEE Computer Society Press, 1994
11. Lu, Ooi, Tan, Verarbeiten von Abfragen in parallele relationalen Datenbanksystemen, IEEE Computer Society Press, 1994.
12. Christopher Olston Benjamin Reed Utkarsh Srivastava, Ravi Kumar, Andrew Tomkins: Schwein Lateinisch: nicht So fremden Sprache für Datenverarbeitung, SIGMOD 2008.
13.     G. Graefe. Überlappend Rahmen für Optimierung. IEEE Daten eng. Stier, 18 Absatz 3: 1995.


[1]: ./media/documentdb-sql-query/sql-query1.png
[introduction]: documentdb-introduction.md
[consistency-levels]: documentdb-consistency-levels.md
 
