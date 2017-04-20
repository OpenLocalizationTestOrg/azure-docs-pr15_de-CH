<properties 
    pageTitle="Verwendung von Azure BLOB-Speicher mit WebJobs SDK" 
    description="Informationen Sie zum Azure BLOB-Speicher WebJobs SDK verwenden. Einen Prozess ausgelöst wird, wenn ein neues Blob in einem Container angezeigt und 'Gift Blobs' behandeln." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Verwendung von Azure BLOB-Speicher mit WebJobs SDK

## <a name="overview"></a>Übersicht

Dieses Handbuch bietet C# Codebeispiele, die zeigen, wie einen Prozess ausgelöst wird, wenn ein Azure Blob erstellt oder aktualisiert wird. Die Codebeispiele verwenden [Webaufträge SDK](websites-dotnet-webjobs-sdk.md) -Version 1.x.

Codebeispiele, die Blobs zu veranschaulichen, finden Sie unter [Warteschlangenspeicher Azure SDK Webaufträge verwenden](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 
        
Das Handbuch setzt voraus wissen [wie ein Webauftrag Projekt in Visual Studio mit Verbindungszeichenfolgen erstellt das Speicherkonto darauf](websites-dotnet-webjobs-sdk-get-started.md) oder [mehrere Storage-Konten](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Wie eine Funktion ausgelöst wird, wenn ein Blob erstellt oder aktualisiert wird

Dieser Abschnitt veranschaulicht, wie die `BlobTrigger` Attribut. 

> [AZURE.NOTE] Das WebJobs SDK scannt Dateien auf neue oder geänderte Blobs. Dieser Prozess ist nicht in Echtzeit; eine Funktion kann nicht mehrere Minuten oder länger ausgelöst werden nach das Blob erstellt wird. Darüber hinaus [Speicher erstellten Protokolle auf einem"besten"](https://msdn.microsoft.com/library/azure/hh343262.aspx) Basis; Es gibt keine Garantie, dass alle Ereignisse erfasst werden. Unter gewissen Umständen können Protokolle fehlen. Wenn die Geschwindigkeit und Zuverlässigkeit Grenzen der BLOB-Trigger nicht für Ihre Anwendung sind, empfiehlt eine warteschlangennachricht erstellen, wenn Sie Blob erstellen und verwenden Sie das [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) -Attribut nicht der `BlobTrigger` Attribut auf die Funktion, die das Blob verarbeitet.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Einzelne Platzhalter für BLOB-Erweiterung  

Das folgende Codebeispiel kopiert Text-Blobs im Container *input* *Output* Container angezeigt:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Der Attributkonstruktor hat einen Zeichenfolgenparameter, der den Containernamen und für BLOB-Namen angibt. In diesem Beispiel ein Blob mit dem Namen *Blob1.txt* im *input* -Container erstellt wird, erstellt die Funktion einen Blob mit dem Namen *Blob1.txt* im *Ausgabe* -Container. 

Sie können ein Muster mit Platzhalter Blob angeben, wie im folgenden Beispiel gezeigt:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Dieser Code kopiert nur Blobs, deren Namen mit"original" beginnen. Beispielsweise wird *Blob1.txt ursprüngliche* *Eingabe* Container in *Kopie Blob1.txt* im Container *Ausgabe* kopiert.

Benötigen Sie ein Muster für BLOB-Namen anzugeben, die geschweifte Klammern in den Namen, verdoppeln Sie die geschweiften Klammern. Beispielsweise soll Blobs im Container *Bilder* , deren Namen wie folgt:

        {20140101}-soundfile.mp3

Verwenden Sie für das Muster:

        images/{{20140101}}-{name}

Im Beispiel wäre Platzhalterwert *namens* *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Separate BLOB-Namen und die Erweiterung Platzhalter

Im folgenden Beispiel ändert die Erweiterung als Blobs kopiert, die im Container *input* *Output* -Container angezeigt werden. Der Code protokolliert die Erweiterung *input* BLOB und die Erweiterung der *Ausgabe* -Blob auf *txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a>Typen, die Blobs gebunden werden können

Sie können die `BlobTrigger` Attribut auf folgenden Typen:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Andere Typen von [ICloudBlobStreamBinder](#icbsb) deserialisiert 

Wenn Sie direkt mit den Azure-Speicher-Konto arbeiten möchten, können Sie auch Hinzufügen einer `CloudStorageAccount` Parameter der Methodensignatur.

Beispiele finden Sie in den [BLOB-Bindung Code im Repository Azure Webaufträge Sdk auf GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Blob Textinhalt von Bindung Zeichenfolge abrufen

Text-Blobs erwarten, `BlobTrigger` können auch auf eine `string` Parameter. Im folgenden Beispiel bindet Text Blob, ein `string` Parameter mit dem Namen `logMessage`. Die Funktion verwendet diesen Parameter der Inhalt des Blob zum Dashboard WebJobs SDK geschrieben. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Abrufen von BLOB-Inhalt mit serialisiert ICloudBlobStreamBinder

Im folgenden Beispiel wird eine Klasse implementiert `ICloudBlobStreamBinder` Aktivieren der `BlobTrigger` Attribut-Blob, das Binden der `WebImage` Typ.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

Die `WebImage` Bindung Code gemäß einer `WebImageBinder` Klasse, die von `ICloudBlobStreamBinder`.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Abrufen des Pfads Blob auslösenden BLOB

Um den Containernamen und BLOB-Namen des Blob, das die Funktion ausgelöst wurde, enthalten eine `blobTrigger` Zeichenfolgenparameter in der Funktionssignatur.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Gift Blobs Behandlung

Wenn ein `BlobTrigger` Funktion fehlschlägt, SDK wird auch für den Fall, dass der Fehler durch einen vorübergehenden Fehler verursacht wurde. Wenn der Inhalt des BLOBs fehlschlägt, schlägt die Funktion jedes Mal versucht das Blob zu. Standardmäßig ruft SDK eine Funktion bis zu 5 Mal für einen angegebenen Blob. Fällt der fünfte Versuch fügt SDK eine Nachricht an eine Warteschlange mit dem Namen *Webjobs Blobtrigger Poison*.

Die maximale Anzahl der Wiederholungsversuche ist konfigurierbar. Dieselbe Einstellung für die [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) wird für Gift Blob Behandlung und Nachrichtenbehandlung Gift Warteschlange verwendet. 

Die Warteschlange für unzustellbare Blobs wird ein JSON-Objekt mit den folgenden Eigenschaften:

* FunctionId (im Format *{Webauftrags Name}*. Funktionen. *{Funktionsname}*Beispiel: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (einen Versionsbezeichner Blob zum Beispiel: "0x8D1DC6E70A277EF")

Im folgenden Codebeispiel die `CopyBlob` Funktion enthält Code, der bewirkt, dass es nicht jedes Mal aufgerufen wird. Nachdem das SDK für die maximale Anzahl der Wiederholungsversuche aufruft und eine Nachricht in der Warteschlange Gift Blob erstellt, wird vom dem `LogPoisonBlob` Funktion. 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }
        
        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

Das SDK deserialisiert die JSON-Nachricht automatisch. Hier wird die `PoisonBlobMessage` Klasse: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>BLOB-Polling-Algorithmus

Das WebJobs SDK scannt alle Container festgelegten `BlobTrigger` Attribute beim Starten der Anwendung. In einem großen Speicherkonto dieser Scan kann einige Zeit dauern, so etwas könnte es vor neuen Blobs befinden und `BlobTrigger` Funktionen ausgeführt werden.

Um neue oder geänderte Blobs nach Anwendung erkennen, liest das SDK regelmäßig aus den Protokollen BLOB-Speicher. BLOB-Protokolle gepuffert werden und nur 10 Minuten physisch geschrieben oder so so erheblichen Verzögerung nach ein Blob erstellt oder, bevor das entsprechende aktualisiert `BlobTrigger` Funktion. 

Eine Ausnahme für Blobs, die Sie mit Erstellen der `Blob` Attribut. WebJobs SDK ein neues Blob erstellt, es geht neue Blob sofort auf alle übereinstimmenden `BlobTrigger` Funktionen. Daher haben Sie eine Kette von BLOB-Eingaben und Ausgaben kann SDK effizient verarbeiten. Aber wenn niedrigen Latenz unter das Blob Verarbeitungsfunktionen für Blobs, die erstellt oder aktualisiert werden soll, empfehlen wir `QueueTrigger` und nicht `BlobTrigger`.

### <a id="receipts"></a>BLOB-Zugänge

Das WebJobs SDK stellt sicher, dass keine `BlobTrigger` Funktion mehrmals für das gleiche neue oder aktualisierte Blob aufgerufen wird. Dies geschieht durch *Blob Zugänge* verwalten, um festzustellen, ob eine angegebene Blob verarbeitet wurde.

BLOB-Zugänge werden in einem Container mit dem Namen *Azure Webaufträge Hosts* angegebene Verbindungszeichenfolge AzureWebJobsStorage Azure Storage-Konto gespeichert. Eine Empfangsbestätigung Blob enthält die folgenden Informationen:

* Die Funktion hieß für Blob ("*{Webauftrags Name}*. Funktionen. *{Funktionsname}*", zum Beispiel:"WebJob1.Functions.CopyBlob")
* Der Containername
* BLOB-Typ ("BlockBlob" oder "PageBlob")
* Der BLOB-name
* Das ETag (einen Versionsbezeichner Blob zum Beispiel: "0x8D1DC6E70A277EF")

Wenn Sie Blob Wiederaufarbeitung erzwingen möchten, können Sie manuell BLOB-Eingang für dieses Blob aus dem Container *Azure Webaufträge Hosts* löschen.

## <a id="queues"></a>Warteschlangen-Artikel Verwandte Themen

Informationen zur Behandlung von BLOB-Verarbeitung durch eine warteschlangennachricht ausgelöst oder WebJobs SDK finden Sie unter Szenarien nicht spezifisch für BLOB-Verarbeitung [Azure Warteschlangenspeicher Webaufträge SDK verwenden](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Die folgenden: Themen in diesem Artikel behandelt

* Async-Funktion
* Mehrere Instanzen
* Ordnungsgemäßes Herunterfahren
* WebJobs SDK Attribute im Hauptteil einer Funktion
* Festlegen Sie die SDK-Verbindungszeichenfolgen im Code.
* Legen Sie Werte für WebJobs SDK Parameter im code
* Konfigurieren Sie `MaxDequeueCount` für die Behandlung von Gift Blob.
* Eine Funktion manuell auslösen
* Schreiben von Protokollen

## <a id="nextsteps"></a>Nächste Schritte

Dieses Handbuch hat Codebeispiele, die veranschaulichen, wie häufige Szenarios für die Arbeit mit Azure-Blobs. Weitere Informationen zur Verwendung von Azure Webaufträge und WebJobs SDK finden Sie unter [Azure Webaufträge empfohlene Ressourcen](http://go.microsoft.com/fwlink/?linkid=390226).
 
