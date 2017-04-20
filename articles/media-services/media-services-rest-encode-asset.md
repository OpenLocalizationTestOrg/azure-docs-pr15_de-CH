<properties 
    pageTitle="Das Codieren einer Anlage mit Media Encoder Standard | Microsoft Azure" 
    description="Erfahren Sie mehr über das Media Encoder Standard verwenden, um Medieninhalte auf Media Services codieren. REST-API verwendet." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>Das Codieren einer Anlage mit Media Encoder Standard


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
- [REST](media-services-rest-encode-asset.md)
- [Portal](media-services-portal-encode.md)

##<a name="overview"></a>Übersicht
Um digitale Videos über das Internet bereitstellen müssen Sie die Medien komprimieren. Digitale Videodateien sehr groß sind und möglicherweise zu groß, über das Internet oder für Ihren Kunden Geräte ordnungsgemäß angezeigt. Beim Codieren werden Video und Audio komprimieren, damit Ihre Kunden Ihre Medien anzeigen können.

Codierung Aufträge sind eine der häufigsten Verarbeitungsvorgänge in Media Services. Sie Arbeitsplätze Codierung, um Mediendateien aus einer Codierung in eine andere zu konvertieren. Wenn Sie codieren, können Sie den Media Services Encoder integrierten (Media Encoder Standard). Sie können auch einen Encoder von Media Services Partner bereitgestellt. Drittanbieter-Encoder sind Azure Marketplace erhältlich. Sie können die Details der Codierung Aufgaben mithilfe von vordefinierten Zeichenfolgen für den Encoder definiert oder vordefinierte Konfigurationsdateien. Folgende Vorgaben, die verfügbar sind, finden Sie unter [Aufgabe Vorgaben für Media Encoder Standard](http://msdn.microsoft.com/library/mt269960).

Jeder Auftrag können eine oder mehrere Aufgaben je nach der Art der Verarbeitung, die Sie erreichen möchten. Durch die REST-API können Sie Aufträge und die Aufgaben auf zwei Arten erstellen:

- Aufgaben werden können durch Vorgänge Navigationseigenschaft Auftrag Entitäten oder
- durch OData-Stapelverarbeitung.


Es wird empfohlen, Kodieren Sie die Aufsteckkarte Dateien immer in eine adaptive Bitrate MP4 festlegen und den Satz in das gewünschte Format der [Dynamischen Verpackung](media-services-dynamic-packaging-overview.md)konvertieren. Um dynamische Verpackung nutzen zu können, müssen Sie zunächst mindestens bei Bedarf Streaming-Einheit für Streaming-Endpunkt abrufen, aus denen Lieferung des Inhalts werden soll. Weitere Informationen finden Sie unter [Skalierung Media Services](media-services-portal-manage-streaming-endpoints.md).

Ist die Ausgabe Anlage Speicher verschlüsselt, müssen Anlagen Lieferung Richtlinie konfigurieren. Informationen finden Sie unter [Konfigurieren von Anlage Lieferung Richtlinie](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE]Sie zunächst verweisen Medienprozessoren sicher, dass Sie das richtige Medium Prozessor-ID Weitere Informationen finden Sie unter [Medienprozessoren erhalten](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>Erstellen Sie ein Projekt mit einer einzelnen Aufgabe Codierung

>[AZURE.NOTE] Beim Arbeiten mit Media Services REST-API berücksichtigen die folgenden Aspekte:
>
>Wenn Entitäten in Media Services zugreifen, müssen Sie bestimmten Feldern und Werten in der HTTP-Anfragen festlegen. Weitere Informationen finden Sie unter [Setup für Media Services REST API-Entwicklung](media-services-rest-how-to-use.md).

>Nach dem erfolgreichen Verbindungsaufbau zu https://media.windows.net erhalten Sie eine 301-Umleitung einer anderen Media Services URI angegeben. Nachfolgende Aufrufe der neuen URI Siehe [Herstellen einer Verbindung mit Media Services REST-API verwenden](media-services-rest-connect-programmatically.md)müssen.
>
>Wenn JSON und um das **__metadata** -Schlüsselwort in der Anforderung (z. B., verweist auf ein verknüpftes Objekt) müssen Sie **Accept** -Header auf [JSON ausführlichen Format](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)festlegen: akzeptieren: Application/Json; Odata = verbose.

Im folgende Beispiel wird das Erstellen und Buchen eines Auftrags mit einem Vorgang legen Sie ein Video mit einer bestimmten Auflösung und Qualität codiert veranschaulicht. Beim Codieren Media Encoder Standard können angegebene Task Konfiguration Vorgaben [hier](http://msdn.microsoft.com/library/mt269960).

Anforderung:

POST https://media.windows.net/API/Jobs HTTP/1.1 Content-Type: Application/Json; Odata = verbose annehmen: Application/Json; Odata = ausführlichen DataServiceVersion: 3.0 MaxDataServiceVersion: X-ms-Version 3.0: 2.11 Autorisierung: Träger <token value> 
 X-ms-Client-Anfrage-Id: 00000000-0000-0000-0000-000000000000 Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Antwort:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>Die Ausgabe Anlage Namen

Das folgende Beispiel zeigt, wie Zweck der Ausgabe-Attribut festgelegt:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Hinweise

- TaskBody Eigenschaften müssen XML-literal verwenden, legen Sie die Anzahl der Eingabe oder Ausgabe von Anlagen, die vom Task verwendet werden. Aufgabenthema enthält die XML-Schemadefinition für XML.
- Definition der TaskBody jeder inneren Wert für <inputAsset> und <outputAsset> JobInputAsset(value) oder JobOutputAsset(value) festgelegt werden.
- Eine Aufgabe kann mehrere Ausgabe-Anlagen haben. Eine Joboutputasset(x)-Objekt kann nur einmal als Ausgabe einer Aufgabe in einem Projekt verwendet werden.
- Sie können JobInputAsset oder JobOutputAsset als Vermögenswert Eingabe eines Vorgangs angeben.
- Aufgaben müssen keine Schleife bilden.
- Der Wertparameter an JobInputAsset oder JobOutputAsset darstellt den Indexwert für eine Anlage. Die tatsächlichen Vermögenswerte werden in der Eigenschaften InputMediaAssets und OutputMediaAssets auf Auftragsdefinition Entität definiert. 
- Da Media Services OData v3 erstellt wird, wird die einzelnen Ressourcen in InputMediaAssets und OutputMediaAssets Navigation Eigenschaftsauswahlen mit einem "__metadata: Uri" Name-Wert-Paar.
- InputMediaAssets ordnet eine oder mehrere Anlagen in Media Services erstellt haben. OutputMediaAssets werden vom System erstellt. Sie verweisen nicht auf eine vorhandene Anlage.
- OutputMediaAssets können mit dem Zweck der Ausgabe Attribut benannt werden. Dieses Attribut ist nicht vorhanden, wenn der Name der OutputMediaAsset werden die inneren Text der Wert der <outputAsset> Element ist mit einem Suffix Auftragsname Wert oder die Auftrags-Id-Wert (in dem Fall, in dem die Name-Eigenschaft ist nicht definiert). Setzt einen Wert für "Beispiel" Zweck der Ausgabe, würden die OutputMediaAsset Name-Eigenschaft ebenfalls nach "Sample" festgelegt werden. Wenn Sie keinen Wert für Zweck der Ausgabe festlegen, aber hat den Auftragsnamen an "NewJob" festgelegt, würde der OutputMediaAsset Name, "_NewJob JobOutputAsset (Wert)" sein. 


##<a name="create-a-job-with-chained-tasks"></a>Erstellen eines Auftrags mit verketteten Aufgaben

In vielen Anwendungsszenarien Entwickler eine Reihe von Aufgaben erstellen möchten. In Media Services können Sie eine Reihe von verketteten Aufgaben erstellen. Jede Aufgabe führt verschiedene Schritte und andere Medienprozessoren verwenden kann. Verketteten Aufgaben können an anderen Ausführen einer linearen Sequenz von Aufgaben für die Anlage eine Anlage aus einer Aufgabe übergeben. Jedoch müssen die Aufgaben in einem Projekt nicht in einer Sequenz. Beim Erstellen einer verketteten Aufgabe werden verkettete **ITask** -Objekte in ein einzelnes **IJob** Objekt erstellt.

>[AZURE.NOTE] Es gibt maximal 30 Aufgaben pro Projekt. Benötigen Sie mehr als 30 Aufgaben verketten, erstellen Sie mehrere Einzelvorgänge der Aufgaben.


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Hinweise

Verketten von Task aktivieren:

- Ein Auftrag muss mindestens 2 Aufgaben
- Es muss mindestens eine Aufgabe, deren Eingabe Ausgabe eines anderen Vorgangs im Auftrag.

## <a name="use-odata-batch-processing"></a>OData-Stapelverarbeitung verwenden 

Im folgenden Beispiel wird veranschaulicht, wie mit OData Stapelverarbeitung einen Auftrag und Aufgaben. Informationen über Stapelverarbeitung finden Sie in der [Stapelverarbeitung Open Data Protocol (OData)](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Erstellen Sie einen Druckauftrag eine Auftragsvorlage


Bei der Verarbeitung mehrerer Ressourcen mit allgemeinen Aufgaben sind JobTemplates geben die Aufgabe Standardvorgaben Reihenfolge von Aufgaben usw..

Im folgenden Beispiel wird veranschaulicht, wie eine Auftragsvorlage mit definierten TaskTemplate Inline erstellt. Die TaskTemplate verwendet Media Encoder Standard als das MediaProcessor der Bilddatei codieren. andere MediaProcessors kann jedoch auch verwendet werden. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]Im Gegensatz zu anderen Entitäten Media Services müssen Sie einen neuen GUID-Bezeichner für jede TaskTemplate definieren und es in die TaskTemplateId und die ID-Eigenschaft im Hauptteil Anforderung. Content Identifikationsschema muss in Azure Media Services Entitäten identifizieren beschriebene Schema. JobTemplates kann auch aktualisiert werden. Stattdessen müssen Sie eine aktualisierten Änderungen erstellen.
 

Wenn erfolgreich, wird die folgende Antwort zurückgegeben:
    
    HTTP/1.1 201 Created
    
    . . .


Im folgenden Beispiel wird veranschaulicht, wie beim Erstellen eines Auftrags eine Auftragsvorlage Id verweisen:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Wenn erfolgreich, wird die folgende Antwort zurückgegeben:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Nächste Schritte
Jetzt wissen Sie wie beim Erstellen eines Auftrags zum Codieren einer Assset, gehen Sie [Wie zu überprüfen Projektfortschritt mit Media Services](media-services-rest-check-job-progress.md) -Thema.


##<a name="see-also"></a>Siehe auch

[Medienprozessoren abrufen](media-services-rest-get-media-processor.md)
