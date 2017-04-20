<properties 
    pageTitle="Beispieldaten in Azure blob-Speicher | Microsoft Azure" 
    description="Beispieldaten in Azure BLOB-Speicher" 
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

#<a name="heading"></a>Beispieldaten in Azure blob-Speicher


Dieses Dokument beschreibt Samplingdaten programmgesteuert downloaden und sampling mit Prozeduren, die in Python geschrieben in Azure BLOB-Speicher gespeichert.

**Warum Beispieldaten?**
Dataset zu analysieren groß ist, ist es in der Regel sollten Sie Daten auf eine kleinere aber repräsentativ und überschaubarer Größe reduzieren Stichproben. Dies erleichtert die Daten verstehen, Untersuchung und Featureentwicklung. In der Cortana-Analyseprozess wird schnell Prototypen Datenverarbeitung und maschinelles lernen Modelle ermöglichen.

Das **Menü** unten Links zu Themen, die beschreiben, wie Sie Daten aus verschiedenen Speicherausfällen Beispiel. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Diese Aufgabe Sampling ist ein Schritt im [Team Daten Wissenschaft Prozess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Herunterladen und Sie Beispieldaten
1. Herunterladen Sie Daten von Azure BLOB-Speicher der BLOB-Service von folgenden Python-Beispielcode: 

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

2. Lesen von Daten in einem Panda Datenrahmen aus über heruntergeladene Datei.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Ab-Beispiel die Daten mit der `numpy` `random.choice` wie folgt:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Jetzt können Sie mit der obigen Daten Beispiel 1 Prozent für weitere Untersuchung und Funktion Generation arbeiten.

##<a name="heading"></a>Daten und Computer in Azure Lesen

Im folgenden Beispielcode können Sie Stichproben der Daten und direkt in Azure ML:

1. Datenrahmen in eine lokale Datei schreiben

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Hochladen der lokalen Datei in eine Azure Blob mit dem folgenden Beispielcode:

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
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. Lesen Sie Daten von Azure Blob mit Azure ML [Daten importieren](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) , wie in der folgenden Abbildung dargestellt:
 
![Reader-blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 
