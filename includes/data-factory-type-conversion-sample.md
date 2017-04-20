### <a name="type-conversion-sample"></a>Konvertierung Beispiel
Im folgende Beispiel wird zum Kopieren von Daten von einem Blob zu SQL Azure mit Typumwandlungen.

Angenommen Sie das Blob Dataset im CSV-Format ist und 3 Spalten enthält. Davon ist einer Datetime-Spalte mit einem benutzerdefinierten Datetime Format abgekürzten französische Namen für den Wochentag.

BLOB-Quell-Dataset wie folgt mit Typdefinitionen für die Spalten bestimmen.

    {
        "name": "AzureBlobTypeSystemInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
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
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Angesichts der SQL definieren Typ .NET Zuordnungstabelle oben Azure SQL-Tabelle durch das folgende Schema.

| Spaltenname | SQL-Typ |
| ----------- | -------- |
| Benutzer-ID | bigint |
| Name | Text |
| LastLoginDate | DateTime |

Als Nächstes definieren Azure SQL-Dataset wie folgt Sie. Hinweis: Sie brauchen "Struktur" Abschnitt mit Informationen angeben, da die Informationen bereits im zugrunde liegenden Datenspeicher angegeben.

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

In diesem Fall geschieht Data Factory den Typ automatisch Umwandlungen einschließlich Datetime-Feld mit benutzerdefinierter Datetime Format mit Kultur fr-fr, wenn Daten in SQL Azure BLOB verschoben.


