<properties
    pageTitle="Exemplarische Vorgehensweise Azure Ereignis Hubs Archiv | Microsoft Azure"
    description="Beispiel, Azure Python SDK zeigen-Ereignis Hubs Archiv Funktion verwendet."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="event-hubs-archive-walkthrough-python"></a>Ereignis Hubs Archiv Walkthrough: Python

Ereignis Hubs Archiv ist ein neues Feature von Ereignis-Hubs, die automatisch zu Streamdaten in Ihrem Ereignis Hub ein Konto Azure BLOB-Speicher ermöglicht. Dies erleichtert die Stapelverarbeitung auf Echtzeit-Daten ausführen. Dieser Artikel beschreibt das Ereignis Hubs Archiv mit Python verwenden. Weitere Informationen zum Ereignis Hubs Archiv finden Sie im [Artikel](event-hubs-archive-overview.md).

Dieses Beispiel verwendet die Azure Python SDK veranschaulichen die Verwendung des Archivfeatures. Der sender.py sendet simulierten Umgebung Telemetrie an Event Hubs im JSON-Format. Hub-Ereignis konfiguriert die Archiv-Funktion zum Schreiben dieser Daten BLOB-Speicher im Stapel. Die archivereader.py liest diese Blobs und erstellt eine Datei anhängen pro Gerät und schreibt die Daten in CSV-Dateien.

Was erfolgt

1.  Erstellen eines Kontos Azure BLOB-Speicher und einen BLOB-Container, mit Azure-portal

2.  Erstellen Sie einen Namespace Event Hub der Azure-portal

3.  Die Archivierung aktiviert, Azure-Portal mit erstellen Sie Event-Hub

4.  Senden von Daten an den Ereignis-Hub mit einem Python-Skript

5.  Die Dateien aus dem Archiv gelesen und mit einer anderen Python-Skript bearbeiten

Erforderliche Komponenten

1.  Python 2.7.x

2.  Ein Azure-Abonnement

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Ein Azure Storage-Konto erstellen

1.  Melden Sie sich bei [Azure-Portal][].

2.  Klicken Sie im linken Navigationsbereich des Portals auf neu, klicken Sie auf Daten + Speicher, und klicken Sie dann auf Konto.

3.  Füllen Sie die Felder in der Blade-Storage-Konto, und klicken Sie auf **Erstellen**.

    ![][1]

4.  Klicken Sie auf **Blobs**, nachdem die **Bereitstellung erfolgreich war** , klicken Sie auf das neue Speicherkonto und **Essentials** Blade-Meldung. Wenn das **Blob** Blatt geöffnet wird, klicken Sie auf **+ Container** oben. Container- **Archiv**benennen und Blade **BLOB-Dienst** zu schließen.

5.  Klicken Sie in dem linken Blade **Zugriffstasten** und kopieren Sie den Namen des Speicherkontos und Wert **key1**. Speichern Sie diese Werte in Editor oder einem anderen temporären Speicherort.

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Erstellen Sie Python-Skript zum Senden von Ereignissen an Event Hub

1.  Öffnen Sie Ihre bevorzugten Python-Editor wie [Visual Studio][].

2.  Erstellen Sie ein Skript namens **sender.py**. Dieses Skript sendet 200 Ereignisse mit der Ereignis-Hub. Einfache Umgebung Literatur in JSON gesendet werden.

3.  Fügen Sie folgenden Code in sender.py:

    ```
    import uuid
    import datetime
    import random
    import json
    from azure.servicebus import ServiceBusService
    
    sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
    devices = []
    for x in range(0, 10):
        devices.append(str(uuid.uuid4()))
    
    for y in range(0,20):
        for dev in devices:
            reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
            s = json.dumps(reading)
            sbs.send\_event('myhub', s)
        print y
    ```
4.  Aktualisieren des vorherigen Codes der Namespacename und Werte, die Sie erhalten, wenn Ereignis Hubs Namespace erstellt.

## <a name="create-a-python-script-to-read-your-archive-files"></a>Erstellen Sie Python-Skript lesen die Archivdateien

1.  Füllen Sie das Blade und klicken Sie auf **Erstellen**.

2.  Erstellen Sie ein Skript namens **archivereader.py**. Dieses Skript liest die Archivdateien und erstellen Sie eine Datei pro Gerät zum Schreiben der Daten für dieses Gerät.

3.  Fügen Sie folgenden Code in archivereader.py:

    ```
    import os
    import string
    import json
    import avro.schema
    from avro.datafile import DataFileReader, DataFileWriter
    from avro.io import DatumReader, DatumWriter
    from azure.storage.blob import BlockBlobService
    
    def processBlob(filename):
        reader = DataFileReader(open(filename, 'rb'), DatumReader())
        dict = {}
        for reading in reader:
            parsed\_json = json.loads(reading["Body"])
            if not 'id' in parsed\_json:
                return
            if not dict.has\_key(parsed\_json['id']):
            list = []
            dict[parsed\_json['id']] = list
        else:
            list = dict[parsed\_json['id']]
            list.append(parsed\_json)
        reader.close()
        for device in dict.keys():
            deviceFile = open(device + '.csv', "a")
            for r in dict[device]:
                deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\\n')

    def startProcessing(accountName, key, container):
        print 'Processor started using path: ' + os.getcwd()
        block\_blob\_service = BlockBlobService(account\_name=accountName, account\_key=key)
        generator = block\_blob\_service.list\_blobs(container)
        for blob in generator:
            if blob.properties.content\_length != 0:
                print('Downloaded a non empty blob: ' + blob.name)
                cleanName = string.replace(blob.name, '/', '\_')
                block\_blob\_service.get\_blob\_to\_path(container, blob.name, cleanName)
                processBlob(cleanName)
                os.remove(cleanName)
            block\_blob\_service.delete\_blob(container, blob.name)
    startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'archive')
    ```

4.  Fügen Sie die entsprechenden Werte für Ihren Speicher Benutzernamen und Schlüssel im Aufruf unbedingt `startProcessing`.

## <a name="run-the-scripts"></a>Führen Sie die Skripts

1.  Öffnen Sie eine Befehlszeile, die Python in seinem Pfad, und führen Sie diese Befehle Python Pakete installieren:

    ```
    pip install azure-storage
    pip install azure-servicebus
    pip install avro
    ```
  
    Wenn Sie eine frühere Version einer Azure-Speicher oder Azure Sie mithilfe müssen der **-upgrade** Option

    Sie müssen auch die folgenden (bei den meisten Systemen nicht erforderlich):

    ```
    pip install cryptography
    ```

2.  Wechseln Sie zum Sie sender.py und archivereader.py gespeichert, und führen Sie diesen Befehl:

    ```
    start python sender.py
    ```
    
    Dies startet eine neue Python Absender führen.

3. Jetzt Warten des Archivs ausgeführt. Geben Sie folgenden Befehl in der ursprünglichen Befehlsfenster ein:

    ```
    python archivereader.py
    ```

Dieses Archiv Prozessor verwendet das lokale Verzeichnis alle Blobs von Konto/Behälter herunterladen. Es werden alle verarbeitet, die nicht leer und die Ergebnisse als CSV-Dateien in das lokale Verzeichnis schreiben.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Ereignis Hubs auf die folgenden Links:

- [Übersicht über Ereignis Hubs archivieren][]
- Eine vollständige [Anwendung mit Event Hubs][].
- Das Beispiel [Dezentrales Ereignis mit Ereignis verarbeitet][] .
- [Übersicht über Hubs][]
 

[Azure-portal]: https://portal.azure.com/
[Übersicht über Ereignis Hubs archivieren]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]: https://azure.microsoft.com/en-us/documentation/articles/storage-create-storage-account/
[Visual Studio-Code]: https://code.visualstudio.com/
[Übersicht über Hubs]: event-hubs-overview.md
[beispielanwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Ereignis mit Ereignis verarbeitet skalieren]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
