<properties
    pageTitle="KEs für Azure BLOB-Speicherdaten über Panda | Microsoft Azure"
    description="Funktionen für Daten erstellen, die in Azure BLOB-Container mit dem Panda Python-Paket gespeichert ist"
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
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Erstellen Sie Funktionen für Azure BLOB-Speicherdaten über Panda

Dieses Dokument veranschaulicht die Features für Daten erstellen, die in Azure BLOB-Container mit dem [Panda](http://pandas.pydata.org/) Python-Paket gespeichert ist. Nach der Gliederung von Daten in ein Panda Daten laden, wird gezeigt, wie kategorische Funktionen Python-Skripten mit Indikatorwerte und binning Funktionen.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Dieses **Menü** enthält Links zu Themen, in denen Funktionen für Daten in unterschiedlichen Umgebungen erstellen. Diese Aufgabe ist ein Schritt im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Erforderliche Komponenten

Es wird vorausgesetzt, dass Azure BLOB-Speicher Konto und Ihre Daten gespeichert haben. Um ein Konto einzurichten, finden Sie unter [Azure Storage-Konto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>Laden Sie die Daten in einem Datenrahmen Panda
Um untersuchen und Bearbeiten eines Datasets, muss es die BLOB-Quelle in eine lokale Datei heruntergeladen werden die in einem Frame Panda Daten geladen werden kann. Hier werden die Schritte für dieses Verfahren:

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

##<a name="blob-featuregen"></a>KE-Erzeugung

In den nächsten beiden Abschnitten zeigen wie kategorische Funktionen Indikatorwerte und binning Funktionen Python-Skripts generieren.

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

3. Nun können die Daten aus dem Blob mit dem Azure Machine Learning [Importdaten](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) Modul wie im folgenden Bildschirm lesen:

![Reader-blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)
