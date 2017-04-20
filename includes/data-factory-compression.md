### <a name="compression-support"></a>Unterstützung der Komprimierung  
Verarbeitung von großen Datenmengen kann e/a und Netzwerkengpässen. Daher komprimierte Daten speichert nicht nur beschleunigt die Datenübertragung über das Netzwerk und Speicherplatz sparen können erhebliche Leistungssteigerungen Groß Datenverarbeitung bringen. Komprimierung ist derzeit für einen dateibasierten Datenspeicher Azure Blob oder lokalen Dateisystem unterstützt.  

> [AZURE.NOTE] Komprimierungsoption werden für Daten in **AvroFormat**, **OrcFormat**oder **ParquetFormat**nicht unterstützt. 

Um Komprimierung für ein Dataset anzugeben, verwenden Sie die Eigenschaft **Compression** im Dataset JSON wie im folgenden Beispiel:   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
Abschnitt **Komprimierung** verfügt über zwei Eigenschaften:  
  
- **Typ:** Komprimierungscodec **GZIP**, **Deflate** oder **BZIP2**.  
- **Stufe:** das Komprimierungsverhältnis **Optimal** oder **am schnellsten**. 
    - **Am schnellsten:** Die Komprimierung sollte Vorgang so schnell wie möglich, auch wenn die Datei nicht optimal komprimiert. 
    - **Optimal**: der Komprimierungsvorgang sollte optimal komprimiert werden, auch wenn der Vorgang mehr Zeit in Anspruch nimmt. 
    
    Weitere Informationen finden Sie [Komprimierung](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) . 

Angenommen, das obige Beispiel Dataset als Ausgabe einer Kopie Aktivität verwendet wird, wird die kopieraktivität Ausgabedaten optimal mit GZIP-Codecs komprimiert und komprimierten Daten in eine Datei namens pagecounts.csv.gz in Azure BLOB-Speicher schreiben.   

Bei Angabe von Eigenschaft Compression in JSON Eingabedatasets kann Pipeline komprimierte Daten lesen kann, und wenn Sie die Eigenschaft in einem ausgabedataset JSON angeben kopieraktivität komprimierte Daten in das Ziel geschrieben. Hier sind einige Beispielszenarien: 

- Lesen GZIP komprimierte Daten von Azure Blob dekomprimieren und schreiben Daten in eine SQL Azure-Datenbank. Sie definieren in diesem Fall Eingabedatasets Azure Blob mit der JSON-Eigenschaft. 
- Lesen von Daten aus einer Textdatei aus lokalen Dateisystem GZip-Format komprimieren und die komprimierten Daten in Azure Blob schreiben. Sie definieren in diesem Fall eine Ausgabe Azure BLOB-Dataset mit den JSON-Eigenschaft.  
- GZIP-komprimierte Daten von Azure Blob lesen, dekomprimieren BZIP2 komprimiert und Ergebnisdaten in Azure Blob schreiben. Sie definieren Eingabedatasets Azure Blob mit vom Komprimierungstyp GZIP und das ausgabedataset mit vom Komprimierungstyp BZIP2 bei.   
