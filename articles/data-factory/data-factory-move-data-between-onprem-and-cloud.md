<properties 
    pageTitle="Verschieben von Daten - Datenverwaltungsgateway | Microsoft Azure"
    description="Ein datengateway zum Verschieben von Daten zwischen lokalen und der Cloud einrichten. Verwenden Sie Daten-Management Gateway in Azure Data Factory Daten verschieben." 
    keywords="datengateway Datenintegration, Verschieben von Daten-Anmeldeinformationen"
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="jingwang"/>

# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Verschieben von Daten zwischen lokalen und Cloud mit Data Management Gateway
Dieser Artikel bietet eine Übersicht über Datenintegration zwischen lokalen Datenspeicher und Cloud-Datenspeichern mit Data Factory. Baut auf [Datenaktivitäten](data-factory-data-movement-activities.md) Artikel und andere Daten Factory zentralen Konzepte Artikel: [Datasets](data-factory-create-datasets.md) und [Rohrleitungen](data-factory-create-pipelines.md). 

## <a name="data-management-gateway"></a>Data Management Gateway
Sie müssen auf dem Computer lokalen Daten nach einem lokalen Datenspeicher aktivieren Data Management Gateway installieren. Das Gateway kann auf demselben Computer wie der Datenspeicher oder auf einem anderen Computer installiert werden, als Gateway mit dem Datenspeicher herstellen kann. 

> [AZURE.IMPORTANT] Siehe [Data Management Gateway](data-factory-data-management-gateway.md) Artikel über Data Management Gateway.   

Die folgende exemplarische Vorgehensweise veranschaulicht die Data Factory mit einer Pipeline zu erstellen, die Daten aus einer lokalen **SQL Server** -Datenbank an eine Azure BLOB-Speicher. Als Teil der exemplarischen Vorgehensweise installieren und Konfigurieren der Datenverwaltungsgateway auf Ihrem Computer. 

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Exemplarische Vorgehensweise: Daten Sie lokal in die cloud
  
## <a name="create-data-factory"></a>Data Factory erstellen
In diesem Schritt verwenden Sie Azure-Portal eine Azure Data Factory-Instanz mit dem Namen **ADFTutorialOnPremDF**erstellt. 

1.  Auf der [Azure-Portal](https://portal.azure.com)anmelden. 
2.  Klicken Sie auf **+ neu**und anschließend auf **Intelligenz + Analytik**, **Data Factory**.

    ![Neu -> DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
2. Geben Sie in das **neue Data Factory** -Blade **ADFTutorialOnPremDF** ein.

    ![Fügen zum Startmenü hinzu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

    > [AZURE.IMPORTANT] 
    > Der Name der Azure Data Factory muss eindeutig sein. Wenn Sie die Fehlermeldung: Ändern Sie den Namen der Data Factory (z. B. YournameADFTutorialOnPremDF) **Data Factory Name "ADFTutorialOnPremDF" ist nicht verfügbar**, und versuchen Sie es erneut erstellen. Verwenden Sie diesen Namen anstelle von ADFTutorialOnPremDF während der verbleibenden Schritte in diesem Lernprogramm.
    > 
    > Der Name der Factory Daten möglicherweise als einen **DNS-** Namen werden in der Zukunft und damit öffentlich sichtbar registriert werden.
3. Wählen Sie den **Azure-Abonnement** Data Factory erstellt werden soll. 
4.  Wählen Sie vorhandene **Ressourcengruppe** oder erstellen Sie eine Ressourcengruppe. Erstellen Sie für das Lernprogramm für eine Ressourcengruppe mit dem Namen: **ADFTutorialResourceGroup**. 
5.  Klicken Sie auf das **neue Data Factory** auf **Erstellen** .

    > [AZURE.IMPORTANT] Um Data Factory-Instanzen erstellen, muss ein Mitglied der [Data Factory](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) Beitragendenrolle auf Abonnement-Ressource sein. 
11. Nach Abschluss der Erstellung Siehe Blatt **Data Factory** , wie in der folgenden Abbildung dargestellt:

    ![Factory-Homepage](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Gateway erstellen
5. Klicken Sie auf Blatt **Data Factory** **Autor und** Kachel einzuführen **Editor** für Data Factory.

    ![Erstellen und Bereitstellen der Kachel](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  Klicken Sie im Data Factory-Editor auf **... Weitere** auf der Symbolleiste und klicken Sie dann auf **Neuer datengateway**. Alternativ können Sie mit der rechten Maustaste **Datengateways** in der Strukturansicht und klicken Sie auf **neue datengateway**. 

    ![Neue datengateway Symbolleiste](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. Blatt **Erstellen** Geben Sie **Adftutorialgateway** **Namen**, und klicken Sie auf **OK**.    

    ![Gateway-Blade erstellen](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
3. Das Blade **Konfigurieren** klicken Sie **direkt auf diesem Computer installieren**. Diese Aktion lädt das Installationspaket für das Gateway installiert, konfiguriert und Gateway auf dem Computer registriert.  

    > [AZURE.NOTE] 
    > Verwenden Sie Internet Explorer oder einem kompatiblen Webbrowser Microsoft ClickOnce.
    > 
    > Bei Verwendung von Chrome wechseln Sie [Chrome Webstore](https://chrome.google.com/webstore/)"ClickOnce" Schlüsselwort suchen und wählen Sie eine ClickOnce-Erweiterung installieren 
    >  
    > Führen Sie dasselbe für Firefox (Installation hinzufügen-in). Klicken Sie **Menü öffnen** auf der Symbolleiste (**drei horizontale Linien** oben rechts), klicken Sie auf **Add-Ons**"ClickOnce" Schlüsselwort suchen Sie, wählen Sie eine ClickOnce-Erweiterung und installieren.    

    ![Gateway - Blade konfigurieren](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Auf diese Weise ist der einfachste Weg (Mausklick) herunterladen, installieren, konfigurieren und registrieren Gateway in einem Schritt. Sie können sehen, dass **Microsoft Data Management Gateway Configuration Manager** -Anwendung auf Ihrem Computer installiert ist. Die ausführbare Datei **ConfigManager.exe** im Ordner finden: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.

    Sie können auch herunterladen und Gateway manuell installieren, indem Sie über die Links in diesem Blatt und registrieren mit dem Schlüssel im Textfeld **Neue Schlüssel** .
    
    Siehe [Data Management Gateway](data-factory-data-management-gateway.md) Artikel für alle Details über das Gateway.

    >[AZURE.NOTE] Sie müssen ein Administrator auf dem lokalen Computer installieren und konfigurieren Data Management Gateway erfolgreich sein. Sie können **Daten Management Gateway Benutzer** lokale Windows-Gruppe weitere Benutzer hinzufügen. Mitglieder dieser Gruppe können Tool Data Management Gateway-Konfigurations-Manager das Gateway konfigurieren. 

5. Warten Sie ein paar Minuten, oder warten Sie, bis die folgende Meldung angezeigt:

    ![Gateway-Installation erfolgreich](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png) 
6. Starten Sie **Data Management Gateway Configuration Manager** -Anwendung auf Ihrem Computer. Geben Sie im **Suchfenster** **Data Management Gateway** Zugriff auf dieses Programm. Die ausführbare Datei **ConfigManager.exe** im Ordner finden: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared** 

    ![Gateway-Konfigurations-Manager](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. Vergewissern, dass Sie `adftutorialgateway is connected to the cloud service` angezeigt. Die Statusleiste unten **verbunden mit Cloud-Dienst** mit einem **grünen Häkchen**angezeigt.

    Auf der Registerkarte **Startseite** können Sie auch die folgenden Operationen durchführen: 
    - **Registrieren Sie** ein Gateway mit einem Schlüssel von Azure-Portal über die Schaltfläche registrieren. 
    - Data Management Gateway Server Service **Beenden** auf Ihrem Gatewaycomputer. 
    - **Planen von Updates** zu einem bestimmten Zeitpunkt des Tages installiert werden. 
    - Ansicht das Gateway **zuletzt aktualisiert**wurde.
    - Geben Sie mit der Aktualisierung der Gateway installiert werden kann. 

8. Wechseln Sie zur Registerkarte **Einstellungen** . Im Abschnitt **Zertifikat** angegebene Zertifikat wird zum Verschlüsseln/Anmeldeinformationen für den lokalen Datenspeicher entschlüsseln, die im Portal angeben. (optional) Klicken Sie auf **Ändern** , um Ihr eigenes Zertifikat verwenden. Das Gateway verwendet das Zertifikat, das vom Dienst Data Factory automatisch generiert wird.

    ![Gateway – Konfigurieren von Zertifikaten](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Sie können auch die folgenden Aktionen auf **der Registerkarte** ausführen: 
    - Zeigen Sie an oder exportieren Sie das Zertifikat vom Gateway verwendet wird.
    - Ändern Sie den HTTPS-Endpunkt vom Gateway verwendet.    
    - Legen Sie einen HTTP-Proxy vom Gateway verwendet werden.   
9. (optional) Wechseln Sie zur Registerkarte **Diagnose** , das Kontrollkästchen Sie **ausführliche Protokollierung aktivieren** Sie ausführliche Protokollierung aktivieren, mit denen Sie Probleme mit dem Gateway beheben möchten. Die Protokollierungsinformationen finden Sie im **Ereignis-Viewer** in **Applikationen und Dienstprotokolle** -> **Data Management Gateway** Knoten. 

    ![Registerkarte "Diagnose"](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    In der Registerkarte **Diagnose** können Sie auch die folgenden Aktionen ausführen: 
    
    - Verwenden Sie **Verbindung testen** Abschnitt an eine lokale Datenquelle mit dem Gateway.
    - Klicken Sie auf **Protokolle anzeigen** , um Log Data Management Gateway in einem Ereignis-Viewer-Fenster angezeigt. 
    - Klicken Sie auf **Protokolle senden** , um eine Zip-Datei mit der letzten sieben Tage an Microsoft zu Fragen der Problembehandlung übertragen. 
10. Wählen Sie auf der Registerkarte **Diagnose** im Abschnitt **Verbindung testen** **SQL Server** für den Typ des Datenspeichers, geben Sie den Namen des Datenbankservers, der Name der Datenbank geben Sie Authentifizierungstyp an, geben Sie Benutzernamen und Kennwort ein und klicken Sie auf **Testen** , testen, ob das Gateway auf die Datenbank zugreifen kann. 
11. Wechseln Sie zum Webbrowser und in **Azure-Portal**, klicken **Sie** auf das **Konfigurieren** und dann auf die **neuen Daten-Gateway** .
6. **Adftutorialgateway** unter **Datengateways** in der Strukturansicht auf der linken Seite sollte angezeigt werden.  Wenn Sie darauf klicken, sollte die zugehörige JSON angezeigt werden. 
    

## <a name="create-linked-services"></a>Erstellen von verknüpften Diensten 
In diesem Schritt erstellen Sie zwei verknüpften Dienste: **AzureStorageLinkedService** und **SqlServerLinkedService**. **SqlServerLinkedService** verknüpft einer lokalen SQL Server-Datenbank und die **AzureStorageLinkedService** verknüpften Serviceartikel Links Azure Blob speichern mit Data Factory. Erstellen eine Pipeline weiter unten in dieser exemplarischen Vorgehensweise, die Daten aus der lokalen SQL Server-Datenbank in Azure Blob-Speicher kopiert. 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Hinzufügen einer verknüpften Serviceartikel mit einer lokalen SQL Server-Datenbank
1.  **Data Factory-Editor**auf der Symbolleiste auf **Speichern neuer Daten** und **SQL Server**wählen. 

    ![Neue verknüpfte SQL Server-Dienst](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.  **JSON-Editor** auf der rechten Seite führen Sie die folgenden Schritte aus: 
    1. Geben Sie für **GatewayName** **Adftutorialgateway**. 
    2. **ConnectionString**führen Sie die folgenden Schritte aus: 
        1. **Servername**Geben Sie den Namen des Servers, der SQL Server-Datenbank hostet.
        2. **Datenbankname**Geben Sie den Namen der Datenbank.
        3. Klicken Sie auf die Schaltfläche **Verschlüsseln** . Diese downloads und startet die Anwendung Anmeldeinformations-Manager.
        
            ![Anmeldeinformationen Manager-Anwendung](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
        5. Klicken Sie im Dialogfeld **Anmeldeinformationen festlegen** Geben Sie Authentifizierungstyp, Benutzernamen und Kennwort an, und klicken Sie auf **OK**. Wenn die Verbindung erfolgreich ist, werden die verschlüsselten Anmeldeinformationen in JSON und das Dialogfeld wird geschlossen. 
        6. Schließen der leere Registerkarte, die im Dialogfeld gestartet, falls nicht automatisch geschlossen und wieder auf die Registerkarte mit dem Azure-Portal. 
  
            Auf dem Gateway sind diese Anmeldeinformationen mit einem Zertifikat der Data Factory Dienst **verschlüsselt** . Möchten Sie das Zertifikat verwenden, das stattdessen Data Management Gateway zugeordnet ist, finden Sie unter [Anmeldeinformationen sicher festgelegt](#set-credentials-and-security).    
    1.  Klicken Sie auf der Befehlsleiste verknüpfte SQL Server-Dienst bereitstellen **Bereitstellen** . Den verknüpften Dienst in der Strukturansicht sollte angezeigt werden. 
        
        ![Verknüpfte SQL Server-Dienst in der Strukturansicht](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)  

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Verknüpften Dienst ein Azure Storage-Konto hinzufügen
 
1. **Data Factory-Editor**auf der Befehlsleiste auf **neuen Daten zu speichern** und anschließend auf **Azure-Speicher**.
2. Geben Sie den Namen Ihres Kontos Azure-Speicher für den **Kontonamen**.
3. Geben Sie den Schlüssel für das Speicherkonto Azure für **kontoschlüssel**.
4. Klicken Sie auf **Deploy** **AzureStorageLinkedService**bereitstellen.
   
 
## <a name="create-datasets"></a>Datasets erstellen
In diesem Schritt erstellen und Datasets, die Eingabe- und für den Kopiervorgang Daten Ausgabe (lokale SQL Server-Datenbank = > Azure BLOB-Speicher). Vor dem Erstellen die Datasets folgendermaßen Sie (detaillierte Schritte die Liste):

- **Emp** in der SQL Server-Datenbank als verknüpfte Dienst Data Factory hinzugefügte Tabelle erstellen und ein paar Beispiele für Einträge in die Tabelle einfügen.
- Erstellen Sie einen BLOB-Container mit dem Namen **Adftutorial** in Azure BLOB-Speicherkonto hinzugefügten als verknüpfte Dienst Data Factory.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>Das Lernprogramm für lokale SQL Server Vorbereiten

1. In der Datenbank der lokalen SQL Server angegebene verknüpfter Dienst (**SqlServerLinkedService**) verwenden folgende SQL-Skript **emp** -Tabelle in der Datenbank erstellen.

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO 
2. Einige Beispiele in die Tabelle einfügen: 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')

### <a name="create-input-dataset"></a>Eingabe-Dataset erstellen

1. Klicken Sie im **Editor für Factory** **... Weitere**klicken Sie auf der Befehlsleiste auf **Neues Dataset** , und klicken Sie auf **SQL Server-Tabelle**. 
2.  Ersetzen Sie JSON im rechten Fensterbereich mit folgendem Text:
        
            {       
                "name": "EmpOnPremSQLTable",
                "properties": {
                    "type": "SqlServerTable",
                    "linkedServiceName": "SqlServerLinkedService",
                    "typeProperties": {
                        "tableName": "emp"
                    },
                    "external": true,
                    "availability": {
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

    Beachten Sie folgende Punkte: 

    - **Typ** wird auf **SqlServerTable**festgelegt.
    - **emp** **TableName** fest.
    - **Nameverknüpfterdienst** wird auf **SqlServerLinkedService** festgelegt (diese verknüpften Serviceartikel wurde dieser exemplarischen Vorgehensweise erstellt werden.).
    - Für Eingabedatasets, die nicht mit einer anderen Rohrleitung in Azure Data Factory erstellt wird, müssen Sie **externe** auf **true**festlegen. Es gibt die Eingabedaten in Azure Data Factory-Dienst hergestellt werden. Optional können Sie externe Daten mit **ExternalData** Element im Abschnitt **Richtlinie** Richtlinien angeben.    

    Details über JSON-Eigenschaften finden Sie unter [Verschieben von Daten in SQL Server](data-factory-sqlserver-connector.md) .
2. Klicken Sie auf der Befehlsleiste Dataset bereitstellen **Bereitstellen** .  


### <a name="create-output-dataset"></a>Ausgabedataset erstellen

1.  Klicken Sie im **Data Factory-Editor**auf der Befehlsleiste auf **Neues Dataset** und **Azure BLOB-Speicher**auf.
2.  Ersetzen Sie JSON im rechten Fensterbereich mit folgendem Text: 

            {
                "name": "OutputBlobTable",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "AzureStorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/outfromonpremdf",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            }
  
    Beachten Sie folgende Punkte: 
    
    - **Typ** wird auf **AzureBlob**festgelegt.
    - **Nameverknüpfterdienst** wird auf **AzureStorageLinkedService** gesetzt (Sie hatten diesen verknüpften Dienst in Schritt2 erstellt).
    - **FolderPath** ist, **Adftutorial-Outfromonpremdf** festgelegt, wobei Outfromonpremdf den Ordner im Container Adftutorial. Erstellen Sie den Container **Adftutorial** , wenn es nicht bereits vorhanden ist. 
    - Die **Verfügbarkeit** wird **stündlich** (**Häufigkeit** **Stunde** und **Intervall** auf **1**festgelegt) fest.  Data Factory-Dienst generiert eine Ausgabe Datenslice stündlich in der Tabelle **emp** Azure SQL-Datenbank. 

    Wenn Sie einen **Dateinamen** für die **Ausgabetabelle**nicht angeben, werden die generierten Dateien im **Ordnerpfad** im folgenden Format benannt: Daten. <Guid>.txt (z. B.:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    Um **Ordnerpfad** und **Dateiname** dynamisch anhand des **SliceStart** festzulegen, verwenden Sie die PartitionedBy-Eigenschaft. Im folgenden Beispiel FolderPath verwendet, Jahr, Monat und Tag SliceStart (Startzeit des Segments verarbeitet) und Dateiname verwendet Stunde aus der SliceStart. Beispielsweise ein Segment für 2014 entsteht-10-20T08:00:00, Wikidatagateway/Wikisampledataout/2014/10/20 Ordnername festgelegt ist und der Dateiname wird auf 08.csv festgelegt. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 
    Details über JSON-Eigenschaften finden Sie unter [Verschieben von Daten in Azure BLOB-Speicher](data-factory-azure-blob-connector.md) .
2.  Klicken Sie auf der Befehlsleiste Dataset bereitstellen **Bereitstellen** . Bestätigen Sie, dass die Datensätze in der Strukturansicht angezeigt.  

## <a name="create-pipeline"></a>Pipeline erstellen
In diesem Schritt erstellen Sie eine **Pipeline** **Kopieren-Aktivität** , die **EmpOnPremSQLTable** als Eingabe verwendet und **OutputBlobTable** ausgegeben.

1.  Klicken Sie in Data Factory-Editor auf **... Weitere**, und klicken Sie auf **neue**. 
2.  Ersetzen Sie JSON im rechten Fensterbereich mit folgendem Text: 
    
            {
                "name": "ADFTutorialPipelineOnPrem",
                "properties": {
                "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
                "activities": [
                {
                    "name": "CopyFromSQLtoBlob",
                    "description": "Copy data from on-prem SQL server to blob",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "EmpOnPremSQLTable"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "OutputBlobTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from emp"
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                    },
                    "Policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "NewestFirst",
                      "style": "StartOfInterval",
                      "retry": 0,
                      "timeout": "01:00:00"
                    }
                  }
                ],
                "start": "2016-07-05T00:00:00Z",
                "end": "2016-07-06T00:00:00Z",
                "isPaused": false
              }
            }

    > [AZURE.IMPORTANT]
    > Ersetzen Sie den Wert der **start** -Eigenschaft mit dem aktuellen Tag und **Ende** mit am nächsten Tag.

    Beachten Sie folgende Punkte:
 
    - Im Abschnitt Aktivitäten ist nur Aktivität, deren **Typ** **Kopieren**festgelegt ist.
    - **Eingabe** für die Aktivität, **EmpOnPremSQLTable** und **Ausgabe** für die Aktivität ist auf **OutputBlobTable**festgelegt.
    - In Abschnitt **TypeProperties** **SQLSource aus** dem **Quelltyp** angegeben und **BlobSink **als die **Senke**angegeben.
    - SQL-Abfrage `select * from emp` für die **SqlReaderQuery** -Eigenschaft des **SQLSource aus**angegeben.

     Start und Ende Datumswerte müssen im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601). Beispiel: 2014-10-14T16:32:41Z. **Die Endzeit** ist optional, aber wir in diesem Lernprogramm verwenden. 
    
    Wenn kein Wert für die **End** -Eigenschaft angeben, wird es als "**Start + 48 Stunden**" berechnet. Führen Sie die Pipeline unbegrenzt Geben Sie **9/9/9999** als Wert für die **End** -Eigenschaft. 
    
    Sie definieren die Dauer in der Datensegmente verarbeitet werden auf Grundlage der **Verfügbarkeit** -Eigenschaften, die für jedes Dataset Azure Data Factory definiert wurden.
    
    Im Beispiel gibt 24 Datenslices jedes Datenslice stündlich erzeugt wird.     
2. Klicken Sie auf der Befehlsleiste auf das Dataset bereitstellen **Bereitstellen** (Tabelle ist eine rechteckige Dataset). Bestätigen Sie, dass die Pipeline unter **Rohrleitungen** Knoten in der Strukturansicht angezeigt.  
5. Klicken Sie nun auf **X** zweimal um Blades wieder Blatt **Data Factory** für **ADFTutorialOnPremDF**zu schließen.

**Herzlichen Glückwunsch!** Erfolgreich erstellt eine Azure Data Factory, verknüpften Diensten, Datasets und einer Pipeline und die Pipeline geplant.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Factory Daten in einer Ansicht anzeigen 
1. Klicken Sie auf **Diagramm** Kachel auf der Startseite für die Factory **ADFTutorialOnPremDF** Daten in **Azure-Portal**. :

    ![Diagramm verknüpfen](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Das Diagramm ähnlich dem folgenden Bild sollte angezeigt werden:

    ![Diagramm anzeigen](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Sie können vergrößern, verkleinern, zoom auf 100 % Zoom anpassen, Rohrleitungen und Datasets positionieren und Datenherkunftsinformationen anzeigen (highlights Upstream- und downstream-Elemente der ausgewählten Elemente).  Doppelklicken Sie auf ein Objekt (Eingabe/Ausgabe-Dataset oder Pipeline) Eigenschaften sehen. 

## <a name="monitor-pipeline"></a>Monitor-pipeline
In diesem Schritt verwenden Sie Azure-Portal überwachen, was in einer Azure Daten. PowerShell-Cmdlets können Sie Datasets und Rohrleitungen überwachen. Informationen zur Überwachung finden Sie unter [Überwachen und Verwalten von Pipelines](data-factory-monitor-manage-pipelines.md).

5. Doppelklicken Sie im Diagramm auf **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable Segmente](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. Beachten Sie, dass alle Daten von slices **bereit** ist die Pipeline Dauer (Startzeit, Endzeit) in der Vergangenheit. Es ist auch die Daten in der SQL Server-Datenbank eingefügt haben und es gibt ständig. Bestätigen Sie, dass keine Segmente in **Slices Problem** unten angezeigt. Klicken Sie alle Segmente **Weitere** am Ende der Liste der Slices. 
7. Klicken Sie auf **OutputBlobTable**, Blatt **Datasets** .

    ![OputputBlobTable Segmente](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
9. Klicken Sie in der Liste auf alle Daten und Blade **Datenslice** sollte angezeigt werden. Aktivität für das Segment wird angezeigt. Sie sehen nur eine Aktivität in der Regel ausgeführt.  

    ![Daten Slice-Blade](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Wenn das Segment nicht im Zustand **bereit** ist, sehen Sie upstream Segmente, die noch nicht blockieren das aktuelle Segment ausführen aus **Upstream Segmente, die nicht bereit sind** .

10. Klicken Sie auf die **Aktivität ausführen** aus der Liste unten, um die **Aktivität**führen Details.

    ![Aktivität Testlaufdetails blade](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

    Finden Sie Informationen wie Durchsatz, Dauer und das übertragen die Daten verwendete Gateway. 
11. Klicken Sie auf **X** , um alle Blades bis schließen 
12. zurück zu home Blade für **ADFTutorialOnPremDF**.
14. (optional) Klicken Sie auf **Pipelines**und Drillthrough-Tabellen (**verbraucht**) oder Ausgabe Datasets (**Produktion**) auf **ADFTutorialOnPremDF**.
15. Mit Tools wie Tools wie [Microsoft Speicher-Explorer](http://storageexplorer.com/) verwenden, ob für jede Stunde eine Blob-Datei erstellt wird.

    ![Azure-Speicher-Explorer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Nächste Schritte

- Siehe [Data Management Gateway](data-factory-data-management-gateway.md) Artikel alle Informationen Data Management Gateway.
- Siehe [Daten von Azure Blob zu SQL Azure](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Informationen Kopieraktivität zum Verschieben von Daten aus einem Datenspeicher Quelle eine Senke Datenspeicher verwenden. 
