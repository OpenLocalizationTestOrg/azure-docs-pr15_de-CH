## <a name="specifying-formats"></a>Festlegen von Formaten

Azure Data Factory unterstützt das folgende Format: 

- [Textformat](#specifying-textformat)
- [JSON-Format](#specifying-jsonformat)
- [Avro formatieren](#specifying-avroformat)
- [ORK-Format](#specifying-orcformat)
- [Parkett Format](#specifying-parquetformat)

### <a name="specifying-textformat"></a>TextFormat angeben

Wenn das **TextFormat**konfiguriert ist, können Sie die folgenden **optionalen** Eigenschaften im Abschnitt **Format** angeben.

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | Das Zeichen zum Trennen der Spalten in einer Datei. | Es ist nur ein Zeichen zulässig. Der Standardwert ist Komma (','). <br/><br/>Ein Unicode-Zeichen finden Sie unter [Unicode-Zeichen](https://en.wikipedia.org/wiki/List_of_Unicode_characters) den entsprechenden Code abgerufen. Sie können beispielsweise selten nicht druckbaren Zeichen verwenden, die wahrscheinlich nicht in Ihren Daten vorhanden: Geben Sie "\u0001" Starten der Überschrift (SOH) dar. | Nein |
| rowDelimiter | Zeichen, Zeilen in einer Datei. | Es ist nur ein Zeichen zulässig. Der Standardwert ist einer der folgenden Werte lesen: ["\r\n", "\r" "\n"] und "\r\n" schreiben. | Nein |
| escapeChar | Spezielle Zeichen, einer Spalte im Inhalt der Eingabedatei Begrenzer. <br/><br/>Sie können keine EscapeChar und QuoteChar für eine Tabelle angeben. | Es ist nur ein Zeichen zulässig. Kein Standardwert. <br/><br/>Beispiel: haben Sie Komma (', ') als das Spaltentrennzeichen, aber Sie das Komma im Text möchten (Beispiel: "Hello, World"), als Escape-Zeichen '$' definieren und Zeichenfolge "$Hello, World" in der Quelle. | Nein | 
| quoteChar | Das Zeichen einen Zeichenfolgenwert Angebot. Die Spalten- und Trennzeichen innerhalb der Anführungszeichen würde als Teil der Zeichenfolge behandelt. Diese Eigenschaft gilt sowohl Eingabe- und Datasets.<br/><br/>Sie können keine EscapeChar und QuoteChar für eine Tabelle angeben. | Es ist nur ein Zeichen zulässig. Kein Standardwert. <br/><br/>Beispielsweise haben Komma (', ') das Spaltentrennzeichen aber Komma im Text haben möchten (Beispiel: < Hello, World >), können "(Anführungszeichen) Anführungszeichen und mit der Zeichenfolge"Hello, World"in der Quelle. | Nein |
| nullValue | Ein oder mehrere Zeichen verwendet, um einen Nullwert darzustellen. | Ein oder mehrere Zeichen. Die Standardwerte sind "\N" und "NULL" "\N" zu lesen, schreiben. | Nein |
| encodingName | Angeben des Codierungsnamens. | Ein gültiger Codierungsname. Siehe [Encoding.EncodingName-Eigenschaft](https://msdn.microsoft.com/library/system.text.encoding.aspx). Beispiel: Windows-1250 oder Shift_jis. Der Standardwert ist UTF-8. | Nein | 
| firstRowAsHeader | Gibt an, ob die erste Zeile als Kopfzeile berücksichtigen. Eingabe-Dataset liest Data Factory erste Zeile als Kopfzeile. Für ein ausgabedataset schreibt Daten Factory erste Zeile als Kopfzeile. <br/><br/>Beispielszenarien finden Sie unter [Szenarien für die Verwendung von **FirstRowAsHeader** und **SkipLineCount** ](#scenarios-for-using-firstrowasheader-and-skiplinecount) . | True<br/>False (Standard) | Nein |
| skipLineCount | Gibt die Anzahl der Zeilen, die beim Lesen von Daten aus Dateien zu überspringen. Wenn SkipLineCount und FirstRowAsHeader angegeben werden, werden zunächst die Zeilen übersprungen und die Headerinformationen werden aus der Eingabedatei gelesen. <br/><br/>Beispielszenarien finden Sie unter [Szenarien für die Verwendung von FirstRowAsHeader und SkipLineCount](#scenarios-for-using-firstrowasheader-and-skiplinecount) . | Ganze Zahl | Nein | 
| treatEmptyAsNull | Gibt an, ob beim Lesen von Daten aus einer Eingabedatei null oder eine leere Zeichenfolge als null-Wert behandelt. | True (Standard)<br/>False | Nein |  

#### <a name="textformat-example"></a>TextFormat-Beispiel
Im folgende Beispiel werden einige der Eigenschaften für TextFormat.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

Um ein EscapeChar anstelle QuoteChar verwenden, ersetzen Sie die Zeile mit QuoteChar mit folgenden EscapeChar:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Szenarien für die Verwendung von FirstRowAsHeader und skipLineCount

- Eine Dateiquelle in eine Textdatei kopieren und möchte eine Kopfzeile mit den Schemametadaten (z. B.: SQL-Schema). Geben Sie **FirstRowAsHeader** als True im ausgabedataset für dieses Szenario. 
- Sie eine Textdatei mit einer Kopfzeile eine Senke-Datei kopieren und diese Zeile löschen möchten. Geben Sie **FirstRowAsHeader** als True Eingabedatasets.
- Sie werden aus einer Textdatei kopieren und Zeilen am Anfang überspringen, die keine Daten oder Header enthalten. Geben Sie **SkipLineCount** die Anzahl von Zeilen übersprungen werden. Wenn der Rest der Datei eine Kopfzeile enthält, können Sie **FirstRowAsHeader**angeben. Wenn **SkipLineCount** und **FirstRowAsHeader** angegeben werden, werden zunächst die Zeilen übersprungen und die Headerinformationen werden aus der Eingabedatei gelesen

### <a name="specifying-avroformat"></a>AvroFormat angeben
Wenn das Format auf AvroFormat festgelegt ist, müssen Sie keine Eigenschaften im Abschnitt TypeProperties der Abschnitt Format angeben. Beispiel:

    "format":
    {
        "type": "AvroFormat",
    }

Zum Avro-Formats in einer Tabelle Struktur finden Sie [Apache Hive Lernprogramm](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Beachten Sie folgende Punkte:  

- [Komplexe Datentypen](http://avro.apache.org/docs/current/spec.html#schema_complex) werden nicht unterstützt (Datensätze, Enumerationen, Arrays, Karten, Unions und feste)

### <a name="specifying-jsonformat"></a>JsonFormat angeben

Wenn das Format auf **JsonFormat**festgelegt ist, können Sie die folgenden **optionalen** Eigenschaften im Abschnitt **Format** angeben.

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| filePattern | Anzugeben Sie das Muster der Daten in jeder Datei JSON. Zulässige Werte: **SetOfObjects** und **ArrayOfObjects**. **Der Standardwert** ist: **SetOfObjects**. Siehe folgende Abschnitte für Details dieser Muster.| Nein |
| encodingName | Angeben des Codierungsnamens. Die Liste der gültigen Codierungsnamen finden Sie unter: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) -Eigenschaft. Beispiel: Windows-1250 oder Shift_jis. **Der Standardwert** ist: **UTF-8**. | Nein | 
| nestingSeparator | Zeichen, Schachtelungsebenen trennen. Der Standardwert ist '.' (Punkt). | Nein | 


#### <a name="setofobjects-file-pattern"></a>SetOfObjects Muster

Jede Datei enthält einzelne Objekt oder Zeile-getrennt/verkettet mehrere Objekte. Wenn diese Option in einem ausgabedataset ausgewählt ist, erzeugt kopieraktivität eine einzelne JSON-Datei mit jedem Objekt pro Zeile (Zeile getrennt).

**einzelnes Objekt** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**JSON Zeile getrennt** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**verkettete JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>ArrayOfObjects Muster. 

Jede Datei enthält ein Array von Objekten. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>JsonFormat-Beispiel

Haben Sie eine JSON-Datei mit folgendem Inhalt:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

und in einer Azure SQL-Tabelle folgendermaßen kopieren: 

ID  | Name.First | Name.Middle | Name.Last | Tags
--- | ---------- | ----------- | --------- | ----
1 | John | NULL | Doe | ["Data Factory", "Azure"]

Eingabe-Dataset mit JsonFormat wie folgt definiert: (partielle Definition der einschlägigen Teile)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Die Struktur definiert ist, Kopieraktivität Struktur standardmäßig reduziert und Alles kopieren. 

#### <a name="supported-json-structure"></a>Unterstützte JSON-Struktur
Beachten Sie folgende Punkte: 

- Jedes Objekt mit einer Auflistung von Name-Wert-Paare wird eine Zeile mit Daten in Tabellenform zugeordnet. -Objekte geschachtelt werden können und wie die Struktur in einem Dataset mit Schachtelungstrennzeichen (.) standardmäßig reduzieren können. Siehe vorhergehenden Abschnitt beispielsweise [JsonFormat Beispiel](#jsonformat-example) .  
- Wenn die Struktur nicht im Dataset Data Factory definiert ist, erkennt das Schema aus dem ersten Objekt kopieren-Aktivität und reduzieren das gesamte Objekt. 
- Wenn die JSON-Eingabe ein Array, konvertiert Aktivität kopieren den gesamten Array-Wert in eine Zeichenfolge. Sie können [spaltenzuordnung](#column-mapping-with-translator-rules)oder Filterung überspringen.
- Gibt doppelte Namen auf derselben Ebene, wählt die Kopieraktivität zuletzt.
- Eigenschaft Groß-und Kleinschreibung. Zwei Eigenschaften mit demselben Namen, aber unterschiedliche Gehäuse werden als zwei separate Eigenschaften behandelt. 

### <a name="specifying-orcformat"></a>OrcFormat angeben
Wenn das Format auf OrcFormat festgelegt ist, müssen Sie keine Eigenschaften im Abschnitt TypeProperties der Abschnitt Format angeben. Beispiel:

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] Wenn Sie keine ORK Dateien kopieren **als-ist** zwischen lokalen und Datenspeichern 8 JRE (Java Runtime Environment) auf dem Gatewaycomputer installieren möchten. 64-Bit-Gateway erfordert JRE 64-Bit- und 32-Bit-Gateway 32-Bit-JRE. Sie können beide Versionen [hier](http://go.microsoft.com/fwlink/?LinkId=808605)finden. Wählen Sie die entsprechende.

Beachten Sie folgende Punkte:

-   Komplexe Daten werden nicht unterstützt (Struktur, Karte, Liste, UNION)
-   ORK-Datei enthält drei [Komprimierung Optionen](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): keine, ZLIB, SNAPPY. Data Factory unterstützt das Lesen von Daten aus ORK Datei komprimierte Formate. Verwendet die Komprimierung Codec ist in den Metadaten zum Lesen der Daten. Allerdings wählt beim Schreiben in eine Datei ORK Data Factory ZLIB, Standard für ORK. Derzeit gibt es keine Möglichkeit, dieses Verhalten zu überschreiben. 

### <a name="specifying-parquetformat"></a>ParquetFormat angeben
Wenn das Format auf ParquetFormat festgelegt ist, müssen Sie keine Eigenschaften im Abschnitt TypeProperties der Abschnitt Format angeben. Beispiel:

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] Wenn Sie keine Parkett Dateien kopieren **als-ist** zwischen lokalen und Datenspeichern 8 JRE (Java Runtime Environment) auf dem Gatewaycomputer installieren möchten. 64-Bit-Gateway erfordert JRE 64-Bit- und 32-Bit-Gateway 32-Bit-JRE. Sie können beide Versionen [hier](http://go.microsoft.com/fwlink/?LinkId=808605)finden. Wählen Sie die entsprechende.

Beachten Sie folgende Punkte:

-   Komplexe Typen sind nicht unterstützt (Karte, Liste)
-   Parkett-Datei enthält die folgenden Optionen durch Komprimierung hervorgerufene: NONE, SNAPPY, GZIP und LZO. Data Factory unterstützt das Lesen von Daten aus ORK Datei komprimierte Formate. Komprimierungscodec verwendet zum Lesen der Daten in den Metadaten. Allerdings wählt beim Schreiben in eine Datei Parkett Data Factory SNAPPY, Standard für Parkett Format. Derzeit gibt es keine Möglichkeit, dieses Verhalten zu überschreiben. 
