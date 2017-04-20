<properties
    pageTitle="Erste Schritte mit Blob speichern und Visual Studio verbunden Services (Webauftrags Projekte) | Microsoft Azure"
    description="Wie Sie BLOB-Speicher in einem Webauftrags verwenden, nachdem eine Verbindung mit einer Azure-Speicher Visual Studio verbunden Services."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Erste Schritte mit Azure Blob Storage und Visual Studio verbunden Services (Webauftrags Projekte)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Übersicht

Dieser Artikel enthält C# Codebeispiele, die zeigen, wie einen Prozess ausgelöst wird, wenn ein Azure Blob erstellt oder aktualisiert wird. Die Codebeispiele verwenden die [Webaufträge SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) -Version 1.x. Wenn Sie ein Speicherkonto Webauftrags Projekt über das Dialogfeld Visual Studio **Verbunden Dienste hinzufügen** hinzufügen, entsprechende Azure Storage NuGet-Paket installiert ist, werden die entsprechenden auf dem Projekt hinzugefügt und Verbindungszeichenfolgen für das Speicherkonto werden in der Datei App.config.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Wie eine Funktion ausgelöst wird, wenn ein Blob erstellt oder aktualisiert wird

Dieser Abschnitt zeigt, wie mit dem **BlobTrigger** -Attribut.

 **Hinweis:** Das WebJobs SDK scannt Dateien auf neue oder geänderte Blobs. Dieser Vorgang ist naturgemäß sehr; eine Funktion kann nicht mehrere Minuten oder länger ausgelöst werden nach das Blob erstellt wird.  Wenn Ihre Anwendung Blobs sofort verarbeiten, ist die empfohlene Methode zum Erstellen einer warteschlangennachricht Blob erstellen und **BlobTrigger** -Attribut auf die Funktion, die das Blob verarbeitet das [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) -Attribut anstelle.

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

## <a name="types-that-you-can-bind-to-blobs"></a>Typen, die Blobs gebunden werden können

Das **BlobTrigger** -Attribut können Sie auf folgenden Typen:

* **Zeichenfolge**
* **TextReader**
* **Stream**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Andere Typen von [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder) deserialisiert

Möchten Sie direkt mit dem Azure-Speicher-Konto arbeiten, können Sie auch die Methodensignatur **CloudStorageAccount** Parameter hinzufügen.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Blob Textinhalt von Bindung Zeichenfolge abrufen

Text Blobs erwarten, können **BlobTrigger** auf **einen Parameter** angewendet werden. Im folgenden Beispiel wird Text Blob an **einen Parameter mit dem Namen **LogMessage**** gebunden. Die Funktion verwendet diesen Parameter der Inhalt des Blob zum Dashboard WebJobs SDK geschrieben.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Abrufen von BLOB-Inhalt mit serialisiert ICloudBlobStreamBinder

Das folgende Codebeispiel verwendet eine Klasse, die **ICloudBlobStreamBinder** um **BlobTrigger** Attribut ein BLOBs **WebImage** Typ gebunden zu aktivieren.

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

**WebImage** Bindung Code wird in einer **WebImageBinder** -Klasse bereitgestellt, die von **ICloudBlobStreamBinder**abgeleitet.

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

## <a name="how-to-handle-poison-blobs"></a>Gift Blobs Behandlung

Fällt eine **BlobTrigger** -Funktion wird das SDK wieder für den Fall, dass der Fehler durch einen vorübergehenden Fehler verursacht wurde. Wenn der Inhalt des BLOBs fehlschlägt, schlägt die Funktion jedes Mal versucht das Blob zu. Standardmäßig ruft SDK eine Funktion bis zu 5 Mal für einen angegebenen Blob. Fällt der fünfte Versuch fügt SDK eine Nachricht an eine Warteschlange mit dem Namen *Webjobs Blobtrigger Poison*.

Die maximale Anzahl der Wiederholungsversuche ist konfigurierbar. Dieselbe Einstellung für die [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) wird für Gift Blob Behandlung und Nachrichtenbehandlung Gift Warteschlange verwendet.

Die Warteschlange für unzustellbare Blobs wird ein JSON-Objekt mit den folgenden Eigenschaften:

* FunctionId (im Format *{Webauftrags Name}*. Funktionen. *{Funktionsname}*Beispiel: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" oder "PageBlob")
* ContainerName
* BlobName
* ETag (einen Versionsbezeichner Blob zum Beispiel: "0x8D1DC6E70A277EF")

Im folgenden Codebeispiel hat die Funktion **CopyBlob** Code, der bewirkt, dass es nicht jedes Mal aufgerufen wird. Nachdem das SDK für die maximale Anzahl der Wiederholungsversuche aufgerufen, wird eine Nachricht in der Warteschlange Gift Blob erstellt und diese Nachricht verarbeitet, von der **LogPoisonBlob** -Funktion.

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

Das SDK deserialisiert die JSON-Nachricht automatisch. Hier ist die **PoisonBlobMessage** -Klasse:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>BLOB-Polling-Algorithmus

Das WebJobs SDK scannt alle Container festgelegten **BlobTrigger** Attribute beim Starten der Anwendung. In einem großen Speicherkonto kann dieser Scan einige Zeit dauern, so etwas könnte es neue Blobs gefunden und **BlobTrigger** Funktionen sind.

Um neue oder geänderte Blobs nach Anwendung erkennen, liest das SDK regelmäßig aus den Protokollen BLOB-Speicher. BLOB-Protokolle gepuffert werden und nur 10 Minuten physisch geschrieben oder so damit möglicherweise erheblichen Verzögerung nach ein Blob erstellt oder, bevor das entsprechende aktualisiert **BlobTrigger** Funktion ausführt.

Es gibt eine Ausnahme für Blobs, die mithilfe des Attributs **Blob** erstellen. Das WebJobs SDK ein neues Blob erstellt, übergibt es neue Blob sofort alle übereinstimmenden **BlobTrigger** Funktionen. Daher haben Sie eine Kette von BLOB-Eingaben und Ausgaben kann SDK effizient verarbeiten. Aber soll das Blob Verarbeitungsfunktionen für Blobs, die erstellt oder aktualisiert mit geringen Latenz, empfehlen wir **QueueTrigger** anstelle von **BlobTrigger**.

### <a name="blob-receipts"></a>BLOB-Zugänge

Das WebJobs SDK stellt sicher, dass keine **BlobTrigger** Funktion mehrmals für das gleiche neue oder aktualisierte Blob aufgerufen wird. Dies geschieht durch *Blob Zugänge* verwalten, um festzustellen, ob eine angegebene Blob verarbeitet wurde.

BLOB-Zugänge werden in einem Container mit dem Namen *Azure Webaufträge Hosts* angegebene Verbindungszeichenfolge AzureWebJobsStorage Azure Storage-Konto gespeichert. Eine Empfangsbestätigung Blob enthält die folgenden Informationen:

* Die Funktion hieß für Blob ("*{Webauftrags Name}*. Funktionen. *{Funktionsname}*", zum Beispiel:"WebJob1.Functions.CopyBlob")
* Der Containername
* BLOB-Typ ("BlockBlob" oder "PageBlob")
* Der BLOB-name
* Das ETag (einen Versionsbezeichner Blob zum Beispiel: "0x8D1DC6E70A277EF")

Wenn Sie Blob Wiederaufarbeitung erzwingen möchten, können Sie manuell BLOB-Eingang für dieses Blob aus dem Container *Azure Webaufträge Hosts* löschen.

## <a name="related-topics-covered-by-the-queues-article"></a>Warteschlangen-Artikel Verwandte Themen

Informationen zur Behandlung von BLOB-Verarbeitung durch eine warteschlangennachricht ausgelöst oder WebJobs SDK finden Sie unter Szenarien nicht spezifisch für BLOB-Verarbeitung [Azure Warteschlangenspeicher Webaufträge SDK verwenden](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Die folgenden: Themen in diesem Artikel behandelt

* Async-Funktion
* Mehrere Instanzen
* Ordnungsgemäßes Herunterfahren
* WebJobs SDK Attribute im Hauptteil einer Funktion
* Festlegen Sie die SDK-Verbindungszeichenfolgen im Code.
* Legen Sie Werte für WebJobs SDK Parameter im code
* Konfigurieren von **MaxDequeueCount** für Gift BLOB-Verarbeitung.
* Eine Funktion manuell auslösen
* Schreiben von Protokollen

## <a name="next-steps"></a>Nächste Schritte

Dieser Artikel hat Codebeispiele, die veranschaulichen, wie häufige Szenarios für die Arbeit mit Azure-Blobs. Weitere Informationen zur Verwendung von Azure Webaufträge und WebJobs SDK finden Sie unter [Azure Webaufträge Dokumentationsressourcen](http://go.microsoft.com/fwlink/?linkid=390226).
