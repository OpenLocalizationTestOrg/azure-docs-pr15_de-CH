## <a name="fileshare-dataset-type-properties"></a>Dateifreigabe Dataset Eigenschaften

Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Datasets finden Sie [Datasets erstellen](../articles/data-factory/data-factory-create-datasets.md) . Abschnitte wie Struktur, Verfügbarkeit und Richtlinien eines Datasets JSON sind für alle Dataset-Typen. 

Abschnitt **TypeProperties** unterscheidet sich für jeden Datensatz. Es enthält Informationen für den Typ Dataset. Abschnitt TypeProperties für ein Dataset Typ **FileShare** Dataset hat die folgenden Eigenschaften:

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
folderPath | Sub-Pfad zu dem Ordner. Verwenden Sie Escapezeichen ' \ ' für spezielle Zeichen in der Zeichenfolge. Beispiele finden Sie unter [Beispiel Service und Dataset-Definitionen verknüpft](#sample-linked-service-and-dataset-definitions) .<br/><br/>Kombinieren diese Eigenschaft mit **PartitionBy** basierend auf Pfade zu Start/Ende Datum / Uhrzeit. | Ja
Dateiname | Geben Sie den Namen der Datei in **FolderPath** soll die Tabelle auf eine bestimmte Datei im Ordner. Wenn Sie einen Wert für diese Eigenschaft nicht angeben, zeigt die Tabelle für alle Dateien im Ordner.<br/><br/>Wenn Dateiname für ein ausgabedataset nicht angegeben wird, wäre der Name der generierten Datei im folgenden dieses Format: <br/><br/>Daten. <Guid>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Nein
partitionedBy | PartitionedBy kann verwendet werden, dynamische FolderPath Dateinamen zeitreihendaten an. Z. B. FolderPath Stunde Daten parametrisiert. | Nein
Format | Folgende Format unterstützt: **TextFormat**, **AvroFormat**, **JsonFormat**und **OrcFormat**. **Typeigenschaft unter Format** auf einen der folgenden Werte festgelegt. [TextFormat angeben](#specifying-textformat), [AvroFormat angeben](#specifying-avroformat), [JsonFormat angeben](#specifying-jsonformat)und [Festlegen OrcFormat](#specifying-orcformat) Abschnitte für Details anzeigen Wenn Sie Dateien kopieren möchten-wird zwischen dateibasierte Speicher (binären Kopie) überspringen Formatabschnitt sowohl Eingabe- und Dataset-Definitionen. | Nein
fileFilter | Gibt einen Filter an eine Teilmenge statt alle Dateien in den Ordnerpfad verwendet werden.<br/><br/>Zulässige Werte: `*` (mehrere Zeichen) und `?` (einzelnes Zeichen).<br/><br/>Beispiel 1:`"fileFilter": "*.log"`<br/>Beispiel 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> FileFilter gilt für Dateifreigabe Eingabedatasets. Diese Eigenschaft ist nicht mit bietet.  | Nein
| Komprimierung | Geben Sie Art und Grad der Komprimierung der Daten. Typen werden unterstützt: **GZip**, **Deflate**, **BZip2** und unterstützte sind: **Optimal** und **schnell**. Derzeit werden Einstellungen für Daten in **AvroFormat** oder **OrcFormat**nicht unterstützt. Weitere Informationen siehe [Komprimierung unterstützt](#compression-support) .  | Nein |
| useBinaryTransfer | Angeben, ob binären Übertragungsmodus verwenden. True, false ASCII zur binären Modus. Standardwert: True. Diese Eigenschaft kann nur verwendet werden, wenn verknüpfte Diensttyp zugeordnet ist: FTP-Server. | Nein | 
 

> [AZURE.NOTE] Dateiname und FileFilter können nicht gleichzeitig verwendet werden.

### <a name="using-partionedby-property"></a>Mithilfe der PartionedBy-Eigenschaft

Wie im vorherigen Abschnitt erwähnt, können Sie dynamische FolderPath Dateinamen Serie Daten mit PartitionedBy. Sie können dazu mit Data Factory Makros und die Systemvariable SliceStart, SliceEnd, die zum logischen Zeitraum einen bestimmten Datenslice anzugeben. 

Time Series Datasets planen und Segmenten finden Sie unter [Erstellen von Datasets](../articles/data-factory/data-factory-create-datasets.md) [Planung & Ausführung](../articles/data-factory/data-factory-scheduling-and-execution.md)und [Pipelines erstellen](../articles/data-factory/data-factory-create-pipelines.md) Artikel. 

#### <a name="sample-1"></a>Beispiel 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In diesem Beispiel {Segment} mit dem angegebenen Wert "Data Factory" Systemvariablen SliceStart im Format (YYYYMMDDHH) ersetzt. Die SliceStart verweist auf das Slice Startzeit. Der Ordnerpfad ist für jedes Segment. Beispiel: Wikisampledataout/Wikidatagateway/2014100103 oder Wikisampledataout/Wikidatagateway/2014100104.

#### <a name="sample-2"></a>Beispiel 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

In diesem Beispiel werden Jahr, Monat, Tag und Zeitpunkt der SliceStart in separaten Variablen extrahiert, die von Ordnerpfad und Dateiname verwendet werden.
