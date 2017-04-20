## <a name="invoking-stored-procedure-for-sql-sink"></a>Aufrufen von gespeicherten Prozedur für SQL Auffangen

Beim Kopieren von Daten in SQL Server oder Azure SQL-SQL Server-Datenbank angegeben Benutzer gespeicherte Prozedur konfiguriert und mit zusätzlichen Parametern aufgerufen werden kann. 

Eine gespeicherte Prozedur kann genutzt werden, wenn integrierte Kopiermechanismus nicht den Zweck dienen. Dies ist in der Regel genutzt (Zusammenführen Spalten Nachschlagen zusätzliche Werte einfügen in mehrere Tabellen...) muss vor dem letzten Einfügen von Daten in der Zieltabelle werden zusätzliche Verarbeitung. 

Sie können eine gespeicherte Prozedur Wahl aufrufen. Das folgende Beispiel veranschaulicht, wie eine gespeicherte Prozedur verwendet ein einfaches Einfügen in eine Tabelle in der Datenbank. 

**Ausgabedataset**

In diesem Beispiel geben soll: SqlServerTable. Stellen sie AzureSqlTable mit einer Azure SQL-Datenbank. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
Definieren Sie wie folgt im Abschnitt SqlSink Kopie Aktivität JSON. Zum Aufrufen einer gespeicherten Prozedur beim Einfügen von Daten werden Eigenschaften SqlWriterStoredProcedureName und SqlWriterTableType.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

Definieren Sie in der Datenbank die gespeicherte Prozedur mit demselben Namen wie SqlWriterStoredProcedureName. Aus der angegebenen Quelle und Einfügen verarbeitet in der Ausgabetabelle. Beachten Sie, dass der Parametername der gespeicherten Prozedur TableName gemäß Tabelle JSON-Datei identisch sein sollte.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

Definieren Sie in der Datenbank den Tabelle mit demselben Namen wie SqlWriterTableType. Beachten Sie, dass das Schema der Art der Eingabedaten zurückgegebene Schema übereinstimmen muss.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

Das Feature gespeicherte Prozedur nutzt [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).