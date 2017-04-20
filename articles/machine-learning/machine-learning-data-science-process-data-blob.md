<properties 
    pageTitle="Azure BLOB-Daten mit erweiterten Analytics | Microsoft Azure" 
    description="Daten in Azure BLOB-Speicher." 
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
    ms.date="09/19/2016"
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Daten Sie Azure BLOB-mit modernste Analytik

Dieses Dokument beschreibt Kennenlernen Daten und Generieren von Funktionen in Azure BLOB-Speicher gespeicherten Daten. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Laden Sie die Daten in einem Datenrahmen Panda
Untersuchen und Bearbeiten eines Datasets müssen sie BLOB-Quelle in eine lokale Datei heruntergeladen werden die in einem Frame Panda Daten geladen werden kann. Hier werden die Schritte für dieses Verfahren:

1. Download die Daten von Azure blob mit dem folgenden Beispiel Python Code BLOB-Dienst. Ersetzen Sie die Variable im Code unten mit bestimmten Werten: 

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


##<a name="blob-dataexploration"></a>Durchsuchen von Daten

Hier sind einige Beispiele für Daten mit Pandas:

1. Überprüfen Sie die Anzahl der Zeilen und Spalten 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Überprüfen Sie die ersten oder letzten Zeilen im Dataset wie folgt:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Überprüfen Sie den Datentyp jeder Spalte importiert wurde, wie im folgenden Beispielcode
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Statistik der grundlegenden für die Spalten im DataSet wie folgt
 
        dataframe_blobdata.describe()
    
5. Die Anzahl der Einträge für die einzelnen Spaltenwerte folgendermaßen aussehen

        dataframe_blobdata['<column_name>'].value_counts()

6. Fehlende Werte zählen oder die Anzahl der Einträge in jeder Spalte mit den folgenden Beispielcode

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Haben Sie fehlende Werte für eine bestimmte Spalte in den Daten können Sie diese wie folgt löschen:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Ersetzen Sie fehlende Werte ist mit dem Modus wird:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Erstellen Sie einen Histogramm Plot mit variabler Anzahl von Lagerfächern Verteilung einer Variablen darstellen 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Korrelationen zwischen Variablen ein Streudiagramm oder integrierte Korrelationsfunktion betrachten

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="blob-featuregen"></a>KE-Erzeugung
    
Wir können mit Python wie folgt erstellen:

###<a name="blob-countfeature"></a>Indikatorwert basierte Funktion generieren

Kategorieliste Funktionen können wie folgt erstellt werden:

1. Untersuchen Sie die Verteilung der Kategoriespalte:
    
        dataframe_blobdata['<categorical_column>'].value_counts()

2. Generieren Sie Indikatorwerte für jede Spaltenwerte

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Verknüpfen Sie die Spalte mit den ursprünglichen Daten 
 
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Entfernen Sie die ursprüngliche Variable:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    
###<a name="blob-binningfeature"></a>Binning Funktion generieren

Zum Generieren von gebinnten Funktionen, wir wie folgt vorgehen:

1. Eine Folge der Spalten eine numerische Spalte bin
 
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
        
2. Konvertiert eine Folge von booleschen Variablen binning

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    
3. Schließlich dummy-Variablen an die ursprüngliche Datenrahmen beitreten

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool) 


##<a name="sql-featuregen"></a>Zurückschreiben von Daten in Azure Blob und in Azure Machine Learning verwenden

Haben die Daten untersucht und erstellt die erforderlichen Features, können Sie die Daten hochladen (Sampling oder Featurized) ein Azure blob und in Azure Machine Learning folgendermaßen nutzen: zusätzliche Funktionen in Azure Machine Learning Studio sowie erstellt werden können. 
1. Datenrahmen in lokale Datei schreiben

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Hochladen der Daten in Azure Blob wie folgt:

        from azure.storage.blob import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Daten aus dem Blob Azure Machine Learning [Importdaten] lesen können[ import-data] Modul wie im folgenden Bildschirm dargestellt:
 
![Reader-blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
