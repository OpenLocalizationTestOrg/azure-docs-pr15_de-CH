<properties 
    pageTitle="SQL Server gespeicherte Prozedur-Aktivität" 
    description="Erfahren Sie SQL Server gespeicherte Prozedur Aktivität Verwendung zum Aufrufen einer gespeicherten Prozedur in einer Azure SQL-Datenbank oder Azure SQL Data Warehouse von Data Factory-Pipeline." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>SQL Server gespeicherte Prozedur-Aktivität
> [AZURE.SELECTOR]
[Struktur](data-factory-hive-activity.md)  
[Schweine](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Gespeicherte Prozedur](data-factory-stored-proc-activity.md)
[Daten See Analytics U-SQL](data-factory-usql-activity.md)
[benutzerdefinierte .NET](data-factory-use-custom-activities.md)

SQL Server gespeicherte Prozedur Aktivität in einer Data Factory- [Pipeline](data-factory-create-pipelines.md) können zum Aufrufen einer gespeicherten Prozedur in einem der folgenden Datenspeicher: 


- SQL Azure-Datenbank 
- Azure SQL Datawarehouse  
- SQL Server-Datenbank in Ihrem Unternehmen oder ein Azure-VM. Sie müssen Daten Management Gateway auf dem Computer, der die Datenbank hostet oder auf einem separaten Computer zu Wettbewerb um Ressourcen in der Datenbank installieren. Daten-Management Gateway ist eine Software verbindet Quellen-Daten Datenquellen abgespritzt in Azure VMs Clouddienste sicheren und verwalteten lokalen. Siehe [Verschieben von Daten zwischen lokalen und](data-factory-move-data-between-onprem-and-cloud.md) Artikel über Data Management Gateway. 

Dieser Artikel baut auf [Daten Transformationsaktivitäten](data-factory-data-transformation-activities.md) Artikel stellt einen allgemeinen Überblick Datentransformations- und unterstützten Transformationsaktivitäten.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

### <a name="sample-table-and-stored-procedure"></a>Beispiel für die Tabelle und eine gespeicherte Prozedur
1. Erstellen Sie folgende **Tabelle** in der Azure SQL-Datenbank mit SQL Server Management Studio oder andere Tools, die Sie mit. Die Spalte datumuhrzeitstempel ist das Datum und die Zeit, die entsprechende ID generiert wird. 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    ID ist eindeutig identifiziert und die Spalte datumuhrzeitstempel Datum und Uhrzeit, wann die entsprechende ID generiert wird.
    ![Beispieldaten](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] Dieses Beispiel verwendet Azure SQL-Datenbank aber genauso für Azure SQL Data Warehouse und SQL Server-Datenbank. 
2. Erstellen Sie die folgende **gespeicherte Prozedur** , die Daten in die **Sampletable**eingefügt.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **Name** und **Gehäuse** des Parameters (in diesem Beispiel DateTime) muss der Parameter in der Pipeline/Aktivität JSON angegeben. Definition der gespeicherten Prozedur sicher, dass **@** als Präfix für den Parameter verwendet.
    
### <a name="create-a-data-factory"></a>Erstellen einer Data factory  
4. Melden Sie sich bei [Azure-Portal](https://portal.azure.com/). 
5. Klicken Sie im linken Menü auf **neu** und anschließend auf **Intelligenz + Analytik**, **Data Factory**.
    
    ![Neue Data factory](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  Geben Sie in das **neue Data Factory** -Blade **SProcDF** ein. Azure Data Factory-Namen sind **global eindeutig**. Sie müssen die Daten Factory Name, um die erfolgreiche Erstellung der Factory voranstellen.

    ![Neue Data factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  Wählen Sie Ihre **Azure-Abonnement**. 
4.  Führen Sie für **Ressourcengruppe**die folgenden Schritte aus: 
    1.  Klicken Sie auf **neu erstellen** , und geben Sie einen Namen für die Ressourcengruppe.
    2.  Klicken Sie auf **vorhandene** und wählen Sie eine vorhandene Ressourcengruppe.  
5.  **Speicherort** der Data Factory auswählen.
6.  Wählen Sie **Pin Dashboard aus** , sodass Sie Factory Daten im Schaltpult nächsten sehen anmelden. 
6.  Klicken Sie auf das **neue Data Factory** auf **Erstellen** .
6.  Data Factory im **Dashboard** der Azure-Portal angezeigt. Nachdem die Data Factory erfolgreich erstellt wurde, anzeigen die Factory Datenseite der Inhalt der Data factory
    ![Data Factory-Homepage](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Einen verknüpfte Azure SQL-Dienst erstellen  
Nach dem Erstellen der Data Factory, erstellen Sie einen Dienst Azure SQL verknüpft, die Ihre Azure SQL-Datenbank mit Daten Factory verknüpft. Diese Datenbank enthält die Sampletable Tabelle und Sp_sample gespeicherten Prozedur.

7.  Klicken Sie auf **Autor und** auf dem Blatt **Data Factory** für **SProcDF** Data Factory-Editor starten.
2.  Klicken Sie auf der Befehlsleiste auf **neuen Daten zu speichern** , und wählen Sie **Azure SQL-Datenbank**. JSON-Skript zum Erstellen eines Azure SQL verknüpft Service im Editor sollte angezeigt werden. 

    ![Neuer Datenspeicher](media/data-factory-stored-proc-activity/new-data-store.png)
4. Im Skript JSON ändern: 
    1. Ersetzen Sie ** &lt;Servername&gt; ** mit dem Namen des Servers Azure SQL-Datenbank.
    2. Ersetzen Sie ** &lt;Databasename&gt; ** mit der Datenbank in der Tabelle und die gespeicherte Prozedur erstellt.
    3. Ersetzen Sie ** &lt; username@servername ** mit dem Benutzerkonto, das Zugriff auf die Datenbank hat.
    4. Ersetzen Sie ** &lt;Kennwort&gt; ** mit dem Kennwort für das Benutzerkonto. 

    ![Neuer Datenspeicher](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. Klicken Sie auf der Befehlsleiste verknüpften Dienst bereitstellen **Bereitstellen** . Bestätigen Sie, dass der AzureSqlLinkedService in der Strukturansicht auf der linken Seite angezeigt. 

    ![Strukturansicht mit verknüpften](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Erstellen Sie eine ausgabedataset
6. Klicken Sie auf **... Weitere** auf der Symbolleiste klicken Sie auf **Neues Dataset**, und klicken Sie auf **SQL Azure**. **Neues Dataset** auf der Befehlsleiste und select **SQL Azure**.

    ![Strukturansicht mit verknüpften](media/data-factory-stored-proc-activity/new-dataset.png)
7. Das folgende Skript JSON in JSON-Editor kopieren und einfügen.

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. Klicken Sie auf der Befehlsleiste Dataset bereitstellen **Bereitstellen** . Bestätigen Sie, dass das Dataset in der Strukturansicht angezeigt. 

    ![Strukturansicht mit verknüpften Diensten](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Erstellen Sie eine Pipeline mit SqlServerStoredProcedure
Nun erstellen Sie eine Rohrleitung mit einer SqlServerStoredProcedure.
 
9. Klicken Sie auf **... Weitere** Befehl und auf **neue**. 
9. Kopieren der folgenden JSON-Ausschnitts. **StoredProcedureName** auf **Sp_sample**festgelegt. Name und Gehäuse des **DateTime** -Parameters müssen den Namen und die Schreibweise des Parameters in der gespeicherten Prozedur übereinstimmen.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Wenn Sie für einen Parameter null übergeben müssen, verwenden Sie die Syntax: "param1": null (Kleinbuchstaben). 
9. Klicken Sie auf der Symbolleiste bereitstellen die Pipeline **Bereitstellen** .  

### <a name="monitor-the-pipeline"></a>Überwachen der pipeline

6. Klicken Sie auf **X** -Blades Data Factory-Editor schließen und navigieren zu dem Blatt Data Factory, und klicken Sie auf **Diagramm**.

    ![Diagramm-Kachel](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. Im **Diagramm**sehen Sie eine Übersicht der Rohrleitungen und Datasets in diesem Lernprogramm verwendet. 

    ![Diagramm-Kachel](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. Doppelklicken Sie in der Diagrammansicht Dataset **Sprocsampleout**. Sie sehen die Segmente bereit. Da ein Segment für jede Stunde zwischen der Start- und Endzeit von JSON entsteht sollte fünf Segmente vor.

    ![Diagramm-Kachel](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. Wenn ein Slice **bereit** ist, führen *Sampletable *Wählen* ** Abfrage SQL Azure-Datenbank zu überprüfen, ob die Daten in der Tabelle gespeicherte Prozedur eingefügt wurde.

    ![Ausgabedaten](./media/data-factory-stored-proc-activity/output.png)

    Ausführliche Informationen zur Überwachung von Azure Data Factory-Pipelines finden Sie unter [Überwachen der Pipeline](data-factory-monitor-manage-pipelines.md) .  

> [AZURE.NOTE] In diesem Beispiel hat der SprocActivitySample keine Eingaben. Wenn Sie diese Aktivität mit einer Aktivität vor (d. h. vor der Verarbeitung) verketten möchten, die Ausgaben der übergeordneten Aktivität als Eingaben in dieser Aktivität verwendet werden. In diesem Fall führt diese Aktivität erst upstream Aktivität abgeschlossen und die Ausgaben der upstream-Aktivitäten verfügbar (betriebsbereit sind). Eingaben werden nicht direkt als Parameter an die gespeicherte Prozedur Aktivität verwendet

## <a name="json-format"></a>JSON-format
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>JSON-Eigenschaften

Eigenschaft | Beschreibung | Erforderlich
-------- | ----------- | --------
Name | Name der Aktivität | Ja
Beschreibung | Beschreibung der Verwendungszweck der Aktivität | Nein
Typ | SqlServerStoredProcedure | Ja
Eingaben | Optional. Wenn Sie eine Eingabe-Dataset angeben, verfügbar sein muss (im Status "Bereit") für die gespeicherte Prozedur Aktivität ausgeführt. Eingabe-Dataset kann nicht als Parameter in der gespeicherten Prozedur genutzt werden. Es dient nur die Abhängigkeit vor dem Start der Aktivität gespeicherten Prozedur überprüfen. | Nein
Ausgaben | Geben Sie eine ausgabedataset für eine gespeicherte Prozedur ein. Ausgabedataset legt den **Zeitplan** für die gespeicherte Prozedur Aktivität (stündlich, wöchentlich, monatlich, etc.). <br/><br/>Das ausgabedataset verwenden **verknüpften Serviceartikel** , die auf einer Azure SQL-Datenbank oder einer Azure SQL Data Warehouse oder eine SQL Server-Datenbank in der gespeicherte Prozedur, die ausgeführt werden soll. <br/><br/>Das ausgabedataset dient als eine Möglichkeit, das Ergebnis der gespeicherten Prozedur für die spätere Verarbeitung durch eine andere Aktivität ([Verkettung Aktivitäten](data-factory-scheduling-and-execution.md#chaining-activities)) in der Pipeline übergeben. Allerdings schreibt Data Factory automatisch die Ausgabe einer gespeicherten Prozedur in dieses Dataset nicht. Es wird die gespeicherte Prozedur in einer SQL-Tabelle geschrieben, das ausgabedataset verweist. <br/><br/>In einigen Fällen kann das ausgabedataset ein **dummy Dataset**nur verwendet, um den Zeitplan für die Ausführung der gespeicherten Prozedur Aktivität festlegen. | Ja
storedProcedureName | Geben Sie den Namen der gespeicherten Prozedur in SQL Azure-Datenbank oder Azure SQL Data Warehouse verknüpften Serviceartikel dargestellt wird, das die Ausgabetabelle. | Ja
storedProcedureParameters | Geben Sie Werte für Parameter von gespeicherten Prozeduren. Benötigen Sie für einen Parameter null übergeben, verwenden Sie die Syntax: "param1": Null (Kleinschreibung). Anzeigen Sie im folgende Beispiel erfahren Sie mithilfe dieser Eigenschaft| Nein

## <a name="passing-a-static-value"></a>Ein statischer Wert 
Nun betrachten in der Tabelle mit einem statischen Wert 'Document Sample' bezeichnet eine weitere Spalte mit dem Namen "Szenario" hinzufügen.

![Beispieldaten 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Jetzt, übergeben Sie Szenario-Parameter und der Wert aus der gespeicherten Prozedur werden. Abschnitt TypeProperties im voranstehenden Beispiel sieht wie im folgenden Codeausschnitt:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

