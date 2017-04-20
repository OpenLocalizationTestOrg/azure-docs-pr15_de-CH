## <a name="column-mapping-with-translator-rules"></a>Übersetzer Regeln zuordnen
Zuordnen lässt sich festlegen, wie die Spalten in der"" Übersichtskarte der Tabelle Spalten in "Structure" Senke Tabelle angegebenen angegeben. Die **ColumnMapping** -Eigenschaft ist in Abschnitt **TypeProperties** der Aktivität kopieren.

Spaltenzuordnung unterstützt die folgenden Szenarien:

- Alle Spalten in der Tabelle "Struktur" werden alle Spalten in der Tabelle Empfänger "Struktur" zugeordnet.
- Eine Teilmenge der Spalten in der Tabelle "Struktur" werden alle Spalten in der Tabelle Empfänger "Struktur" zugeordnet.

Folgende Fehler sind und führt zu einer Ausnahme:

- Weniger Spalten oder weitere Spalten in die "Struktur" Senke Tabelle als in der Zuordnung angegeben.
- Doppelte Zuordnung.
- SQL-Abfrageergebnis muss kein Spaltenname, der in der Zuordnung angegeben ist.

## <a name="column-mapping-samples"></a>Spalte Zuordnung Beispiele
> [AZURE.NOTE] Die folgenden Beispiele sind für SQL Azure und Azure Blob jedoch für beliebige Datenspeicher, die rechteckige Datasets unterstützt. Sie müssen Dataset und verknüpfte Dienstdefinitionen in folgenden Beispielen auf Daten in der entsprechenden Datenquelle anzupassen. 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Beispiel 1 – Zuordnen von Azure SQL Azure-BLOB-Spalte
In diesem Beispiel die Eingabetabelle hat eine Struktur und verweist auf eine SQL-Tabelle in einer Azure SQL-Datenbank.

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

In diesem Beispiel die Ausgabetabelle hat eine Struktur und verweist auf ein Blob in Azure Blob-Speicher.

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

Nachfolgend finden Sie die JSON für die Aktivität. Spalten aus Quelle Spalten Senke (**ColumnMappings**) mit **Translator** -Eigenschaft zugeordnet.

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**Spalte Zuordnung Flow:**

![Spalte Zuordnung Fluss](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Beispiel 2 – mit SQL Abfrage von Azure SQL Azure BLOB-Spalte
In diesem Beispiel wird eine SQL-Abfrage zum Extrahieren von Daten aus SQL Azure, statt einfach den Tabellennamen und den Spaltennamen im Abschnitt "Structure". 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

In diesem Fall werden die Ergebnisse der Abfrage zuerst in "Struktur" angegebenen Spalten zugeordnet. Anschließend werden Spalten Quelle "Struktur" Sink "Struktur" Spalten mit Vorschriften ColumnMappings zugeordnet.  Angenommen, die Abfrage 5 Spalten, zwei weitere Spalten und die in die "Struktur" gibt.

**Spalte Zuordnung Fluss**

![Spalte Zuordnung Flow-2](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)







