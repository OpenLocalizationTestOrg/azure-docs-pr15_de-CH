<properties 
    pageTitle="Untersuchen von Azure BLOB-Speicher mit Panda | Microsoft Azure" 
    description="Wie Sie Daten untersuchen, die in Azure BLOB-Container mit Panda gespeichert." 
    services="machine-learning,storage" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Untersuchen von Azure BLOB-Speicher mit Panda

Dieses Dokument beschreibt die Daten untersuchen, die in Azure BLOB-Container mit [Panda](http://pandas.pydata.org/) Python-Paket gespeichert ist.

Das folgende **Menü** enthält Links zu Themen über die Verwendung von Tools zu Daten aus verschiedenen Speicher. Diese Aufgabe ist ein Schritt im [Wissenschaftlichen Daten]().

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Erforderliche Komponenten
Dieser Artikel setzt voraus:

* Ein Azure Storage-Konto erstellt. Anleitung, finden Sie unter [Azure Storage-Konto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account)
* Die Daten in ein Konto Azure BLOB-Speicher gespeichert. Anleitung, finden Sie unter [Verschieben Daten zwischen Azure-Speicher](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Die Daten in einem Datensatz Panda laden
Untersuchen und Bearbeiten eines Datasets müssen sie zuerst BLOB-Quelle in eine lokale Datei heruntergeladen werden die in einem Datensatz Panda geladen werden kann. Hier werden die Schritte für dieses Verfahren:

1. Download die Daten von Azure blob mit der folgenden Python Code BLOB-Dienst. Bestimmte Werte ersetzen Sie der Variablen im folgenden Code: 

        from azure.storage.blob import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))


2. Lesen Sie die Daten in einem Panda Datenrahmen aus der Datei.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Jetzt können Sie die Daten und Funktionen auf Dataset generieren.

##<a name="blob-dataexploration"></a>Durchsuchen von Daten mithilfe von Panda-Beispiele

Hier sind einige Beispiele für Daten mit Pandas:

1. Überprüfen Sie die **Anzahl der Zeilen und Spalten** 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **Prüfen** der ersten oder letzten **Zeilen** in das folgende Dataset:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Überprüfen Sie den **Datentyp** jeder Spalte importiert wurde, wie im folgenden Beispielcode
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. **Grundlegende Statistiken** für die Spalten im DataSet wie folgt überprüfen
 
        dataframe_blobdata.describe()
    
5. Die Anzahl der Einträge für die einzelnen Spaltenwerte folgendermaßen aussehen

        dataframe_blobdata['<column_name>'].value_counts()

6. **Fehlende Werte zählen** oder die Anzahl der Einträge in jeder Spalte mit den folgenden Beispielcode

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Haben Sie **fehlende Werte** für eine bestimmte Spalte in den Daten, können Sie diese wie folgt löschen:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Ersetzen Sie fehlende Werte ist mit dem Modus wird:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Erstellen Sie einen **Histogramm** Plot mit variabler Anzahl von Lagerfächern Verteilung einer Variablen darstellen 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. **Korrelationen** zwischen Variablen ein Streudiagramm oder integrierte Korrelationsfunktion betrachten

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
