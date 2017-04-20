<properties
    pageTitle="Kopieren oder Verschieben von Daten auf Speicher mit AzCopy | Microsoft Azure"
    description="Mithilfe des Dienstprogramms AzCopy Daten verschieben oder kopieren oder Blob, Tabelle und Inhalt. Kopieren von Daten in Azure Storage aus lokalen Dateien oder Daten innerhalb oder zwischen Speicherkonten. Einfache Migration Ihrer Daten zu Azure-Speicher."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="micurd"/>

# <a name="transfer-data-with-the-azcopy-command-line-utility"></a>Daten mit AzCopy-Befehlszeilenprogramm

## <a name="overview"></a>Übersicht

AzCopy ist ein Windows-Befehlszeilenprogramm zum Kopieren von Daten zu und von Microsoft Azure BLOB-Datei und Tabelle Speicher mit einfachen Befehlen mit optimaler Leistung entwickelt. Sie können Daten von einem Objekt in ein anderes kopieren, innerhalb Ihres Speicherkontos oder zwischen Speicherkonten.

> [AZURE.NOTE] Dieses Handbuch wird davon ausgegangen, dass Sie bereits kennen [Azure-Speicher](https://azure.microsoft.com/services/storage/). Wenn nicht, der [Einführung in Azure Storage](storage-introduction.md) Dokumentation hilfreich. Vor allem müssen Sie erstellen [ein Speicherkonto](storage-create-storage-account.md#create-a-storage-account) Verwendung AzCopy.

## <a name="download-and-install-azcopy"></a>Herunterladen und Installieren von AzCopy

### <a name="windows"></a>Windows

Herunterladen der [neuesten Version von AzCopy](http://aka.ms/downloadazcopy).

### <a name="maclinux"></a>Mac-Linux

AzCopy ist nicht verfügbar für Mac-Linux-Betriebssysteme. Azure-CLI ist jedoch eine Alternative zum Kopieren von Daten zu und von Azure-Speicher. Gelesen Sie weitere [Verwendung der Azure-CLI mit Azure-Speicher](storage-azure-cli.md) .

## <a name="writing-your-first-azcopy-command"></a>Schreiben den ersten Befehl ein AzCopy

Die grundlegende Syntax für AzCopy Befehle ist:

    AzCopy /Source:<source> /Dest:<destination> [Options]

Öffnen Sie ein Befehlsfenster, und navigieren Sie zum Installationsverzeichnis auf Ihrem Computer - AzCopy, die `AzCopy.exe` ausführbare Datei befindet. Bei Bedarf können Sie den Installationsort AzCopy zum Systempfad hinzufügen. Befindet sich AzCopy `%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy` oder `%ProgramFiles%\Microsoft SDKs\Azure\AzCopy`.

Die folgenden Beispiele veranschaulichen verschiedene Szenarios zum Kopieren von Daten zu und von Microsoft Azure-Blobs, Dateien und Tabellen. Siehe Abschnitt [AzCopy Parameter](#azcopy-parameters) eine ausführliche Erläuterung der Parameter in jeder Probe verwendet.

## <a name="blob-download"></a>BLOB: herunterladen

### <a name="download-single-blob"></a>Einzelnes Blob herunterladen

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:"abc.txt"

Beachten Sie, dass den Ordner `C:\myfolder` nicht vorhanden AzCopy erstellen und downloaden `abc.txt ` in den neuen Ordner.

### <a name="download-single-blob-from-secondary-region"></a>Sekundäre Region einzelnes Blob herunterladen

    AzCopy /Source:https://myaccount-secondary.blob.core.windows.net/mynewcontainer /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Beachten Sie, dass Sie Lesezugriff Geo redundante Speicherung aktiviert haben.

### <a name="download-all-blobs"></a>Alle Blobs herunterladen

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /S

Angenommen Sie, die folgenden Blobs im angegebenen Container befinden:  

    abc.txt
    abc1.txt
    abc2.txt
    vd1\a.txt
    vd1\abcd.txt

Nachdem der Downloadvorgang Verzeichnis `C:\myfolder` enthalten die folgenden Dateien:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\vd1\a.txt
    C:\myfolder\vd1\abcd.txt

Wenn Sie keine Option angeben `/S`, keine Blobs heruntergeladen.

### <a name="download-blobs-with-specified-prefix"></a>Herunterladen von Blobs mit angegebenen Präfix

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /Pattern:a /S

Angenommen Sie, die folgenden Blobs im angegebenen Container befinden. Alle Blobs beginnen mit dem Präfix `a` heruntergeladen:

    abc.txt
    abc1.txt
    abc2.txt
    xyz.txt
    vd1\a.txt
    vd1\abcd.txt

Nach dem Downloadvorgang den Ordner `C:\myfolder` enthalten die folgenden Dateien:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

Das Präfix gilt für das virtuelle Verzeichnis bildet den ersten Teil des blobnamens. Im obigen Beispiel entspricht das virtuelle Verzeichnis nicht das angegebene Präfix, nicht heruntergeladen. Außerdem, wenn die Option `\S` nicht angegeben ist, AzCopy Blobs wird nicht gedownloadet.

### <a name="set-the-last-modified-time-of-exported-files-to-be-same-as-the-source-blobs"></a>Die letzte Änderung Zeit exportierten Dateien als Quell-Blobs sein

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT

Sie können auch anhand ihrer letzten Änderung Downloadvorgang Blobs ausschließen. Beispielsweise sollen Blobs, deren letzte Zeit Änderung, ist-Version als die Zieldatei hinzufügen der `/XN` Option:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XN

Oder sollen Blobs, deren letzter Zeitpunkt Änderung, derselben oder älter als die Zieldatei hinzufügen der `/XO` Option:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:key /MT /XO

## <a name="blob-upload"></a>BLOB: hochladen

### <a name="upload-single-file"></a>Datei hochladen

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:"abc.txt"

Wenn der angegebenen Container nicht vorhanden ist, wird AzCopy erstellt, und die Datei hochladen.

### <a name="upload-single-file-to-virtual-directory"></a>Hochladen einer Datei auf Virtuelles Verzeichnis

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer/vd /DestKey:key /Pattern:abc.txt

Wenn das angegebene virtuelle Verzeichnis nicht vorhanden ist, lädt AzCopy die Datei, um das virtuelle Verzeichnis im Namen enthalten (*z.B.* `vd/abc.txt` im obigen Beispiel).

### <a name="upload-all-files"></a>Alle Dateien hochladen

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /S

Angeben `/S` lädt den Inhalt des angegebenen Verzeichnisses BLOB-Speicher rekursiv, d. h., alle Unterordner und Dateien sowie hochgeladen werden. Angenommen beispielsweise folgenden Dateien befinden sich im Ordner `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nach der Uploadvorgang wird der Container die folgenden Dateien enthalten:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Wenn Sie keine Option angeben `/S`, AzCopy lädt nicht rekursiv. Nach der Uploadvorgang wird der Container die folgenden Dateien enthalten:

    abc.txt
    abc1.txt
    abc2.txt

### <a name="upload-files-matching-specified-pattern"></a>Hochladen von Dateien angegebenen Muster

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:a* /S

Angenommen, die folgenden Dateien befinden sich im Ordner `C:\myfolder`:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt
    C:\myfolder\xyz.txt
    C:\myfolder\subfolder\a.txt
    C:\myfolder\subfolder\abcd.txt

Nach der Uploadvorgang wird der Container die folgenden Dateien enthalten:

    abc.txt
    abc1.txt
    abc2.txt
    subfolder\a.txt
    subfolder\abcd.txt

Wenn Sie keine Option angeben `/S`, AzCopy lädt nur Blobs, die sich nicht in einem virtuellen Verzeichnis befinden:

    C:\myfolder\abc.txt
    C:\myfolder\abc1.txt
    C:\myfolder\abc2.txt

### <a name="specify-the-mime-content-type-of-a-destination-blob"></a>Geben Sie den MIME-Inhaltstyp der Ziel-blob

Standardmäßig legt AzCopy den Inhaltstyp der Ziel-Blob, `application/octet-stream`. Ab Version 3.1.0 Sie können explizit angeben den Inhaltstyp mit der Option `/SetContentType:[content-type]`. Diese Syntax wird der Inhaltstyp für alle Blobs in einem Uploadvorgang.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType:video/mp4

Bei Angabe `/SetContentType` ohne Wert, dann AzCopy setzt jedes Blob oder Inhaltstyp nach der Erweiterung der Datei.

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.blob.core.windows.net/myContainer/ /DestKey:key /Pattern:ab /SetContentType

## <a name="blob-copy"></a>BLOB: Kopieren

### <a name="copy-single-blob-within-storage-account"></a>Einzelnes Blob in Konto kopieren

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceKey:key /DestKey:key /Pattern:abc.txt

Beim Kopieren eines BLOBs in ein Speicherkonto ist ein [serverseitige](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) Kopiervorgang ausgeführt.

### <a name="copy-single-blob-across-storage-accounts"></a>Einzelnes Blob Storage Konten kopieren

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Beim Kopieren eines BLOBs Konten Speicher erfolgt [serverseitige](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-asynchronous-cross-account-copy-blob.aspx) kopieren.

### <a name="copy-single-blob-from-secondary-region-to-primary-region"></a>Einzelnes Blob aus sekundären Kopie primärer Bereich

    AzCopy /Source:https://myaccount1-secondary.blob.core.windows.net/mynewcontainer1 /Dest:https://myaccount2.blob.core.windows.net/mynewcontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt

Beachten Sie, dass Sie Lesezugriff Geo redundante Speicherung aktiviert haben.

### <a name="copy-single-blob-and-its-snapshots-across-storage-accounts"></a>Kopieren Sie einzelnes Blob und der Momentaufnahmen Speicher Konten

    AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt /Snapshot

Nach dem Kopiervorgang wird der Zielcontainer Blob und der Momentaufnahmen enthalten. Angenommen, das Blob im obigen Beispiel zwei Snapshots, enthalten der Container die folgenden Blob und Snapshots:

    abc.txt
    abc (2013-02-25 080757).txt
    abc (2014-02-21 150331).txt

### <a name="synchronously-copy-blobs-across-storage-accounts"></a>Blobs synchron Speicher Konten kopieren

AzCopy standardmäßig kopiert Daten zwischen zwei Endpunkten Speicher asynchron. Daher läuft der Kopiervorgang im Hintergrund freie Bandbreite, die keine SLA, wie schnell ein Blob kopiert werden, und AzCopy wird in regelmäßigen Abständen überprüfen kopieren, bis der Kopiervorgang abgeschlossen oder fehlgeschlagen ist.

Die `/SyncCopy` Option wird sichergestellt, dass der Kopiervorgang gleichmäßige Geschwindigkeit erhalten. AzCopy führt die synchrone Kopie herunterladen die Blobs aus der angegebenen Quelle in den lokalen Speicher kopieren und sie in Ziel-BLOB-Speicher hochladen.

    AzCopy /Source:https://myaccount1.blob.core.windows.net/myContainer/ /Dest:https://myaccount2.blob.core.windows.net/myContainer/ /SourceKey:key1 /DestKey:key2 /Pattern:ab /SyncCopy

`/SyncCopy`möglicherweise zusätzliche Ausgang Kosten gegenüber asynchrone Kopie erzeugen, wird empfohlen, diese Option in Azure-VM verwenden in der gleichen Region wie Ihr Speicher Quellkonto Ausgang Kosten zu vermeiden.

## <a name="file-download"></a>Datei: herunterladen

### <a name="download-single-file"></a>Datei herunterladen

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/myfolder1/ /Dest:C:\myfolder /SourceKey:key /Pattern:abc.txt

Wenn die angegebene Quelle Azure Dateifreigabe, Sie müssen den genauen Dateinamen angeben (*z.B.* `abc.txt`) in einer Datei herunterladen oder Option `/S` , alle Dateien in der Freigabe rekursiv. Versucht, ein Muster und Option an `/S` zusammen führt zu einem Fehler.

### <a name="download-all-files"></a>Alle Dateien

    AzCopy /Source:https://myaccount.file.core.windows.net/myfileshare/ /Dest:C:\myfolder /SourceKey:key /S

Beachten Sie, dass leere Ordner nicht heruntergeladen werden.

## <a name="file-upload"></a>Datei: hochladen

### <a name="upload-single-file"></a>Datei hochladen

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:abc.txt

### <a name="upload-all-files"></a>Alle Dateien hochladen

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /S

Beachten Sie, dass leere Ordner nicht hochgeladen werden.

### <a name="upload-files-matching-specified-pattern"></a>Hochladen von Dateien angegebenen Muster

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.file.core.windows.net/myfileshare/ /DestKey:key /Pattern:ab* /S

## <a name="file-copy"></a>Datei: Kopieren

### <a name="copy-across-file-shares"></a>Kopieren Sie für Dateifreigaben

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S

### <a name="copy-from-file-share-to-blob"></a>Kopieren von BLOB-Dateifreigabe

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare/ /Dest:https://myaccount2.blob.core.windows.net/mycontainer/ /SourceKey:key1 /DestKey:key2 /S

Beachten Sie, dass asynchrone Kopieren von Dateispeicher auf Seite Blob nicht unterstützt wird.

### <a name="copy-from-blob-to-file-share"></a>Kopieren von BLOBs auf Dateifreigabe

    AzCopy /Source:https://myaccount1.blob.core.windows.net/mycontainer/ /Dest:https://myaccount2.file.core.windows.net/myfileshare/ /SourceKey:key1 /DestKey:key2 /S

### <a name="synchronously-copy-files"></a>Synchron Dateien

Sie können die `/SyncCopy` option Daten vom Dateispeicherort Datenspeicher vom Dateispeicherort Blob-Speicher und BLOB-Speicher Datenspeicher synchron kopieren und AzCopy die Daten in den lokalen Speicher heruntergeladen wird auf Ziel erneut hochladen.

    AzCopy /Source:https://myaccount1.file.core.windows.net/myfileshare1/ /Dest:https://myaccount2.file.core.windows.net/myfileshare2/ /SourceKey:key1 /DestKey:key2 /S /SyncCopy

Beim Kopieren von Dateispeicher auf BLOB-Speicher der BLOB-Standardtyp Block-Blob, Benutzer können die Option `/BlobType:page` Ziel-BLOB-Typ ändern.

Beachten Sie, dass `/SyncCopy` zusätzliche Kosten asynchrone Kopie vergleichen Ausgang generiert, wird empfohlen, diese Option in der Azure-VM verwenden in der gleichen Region wie Ihr Speicher Quellkonto Ausgang Kosten zu vermeiden.

## <a name="table-export"></a>Tabelle: Exportieren

### <a name="export-table"></a>Tabelle exportieren

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key

AzCopy schreibt eine manifest-Datei in den angegebenen Zielordner. Die Manifestdatei wird in den Importvorgang erforderlichen Datendateien suchen und Validierung der Daten verwendet. Die Manifestdatei verwendet standardmäßig die folgende Benennungskonvention:

    <account name>_<table name>_<timestamp>.manifest

Benutzer können auch die Option `/Manifest:<manifest file name>` Namen der Manifestdatei fest.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /Manifest:abc.manifest

### <a name="split-export-into-multiple-files"></a>Aufgeteilt in mehrere Dateien exportieren

    AzCopy /Source:https://myaccount.table.core.windows.net/mytable/ /Dest:C:\myfolder /SourceKey:key /S /SplitSize:100

AzCopy verwendet einen *Volumenindex* im Namen geteilten Daten auf um mehrere Dateien zu unterscheiden. Volumenindex besteht aus zwei Teilen, einen *Index der Schlüsselbereich* und *Teilen Dateiindex*. Beide Indizes sind nullbasiert.

Der Index der Schlüsselbereich 0 sein, wenn Benutzer keine Option `/PKRS`.

Angenommen, AzCopy zwei Dateien generiert, nachdem der Benutzer die Option gibt `/SplitSize`. Die resultierenden Daten Dateinamen könnten:

    myaccount_mytable_20140903T051850.8128447Z_0_0_C3040FE8.json
    myaccount_mytable_20140903T051850.8128447Z_0_1_0AB9AC20.json

Hinweis der minimale möglichen Wert für die Option `/SplitSize` 32 MB. Ist das angegebene Ziel-BLOB-Speicher AzCopy wird die Datendatei nach der Blob-Beschränkung (200GB) erreicht, die option unabhängig davon, ob `/SplitSize` vom Benutzer angegeben wurde.

### <a name="export-table-to-json-or-csv-data-file-format"></a>In JSON oder CSV-Dateiformat exportieren

AzCopy standardmäßig exportiert Tabellen in JSON-Dateien. Sie können die Option `/PayloadFormat:JSON|CSV` Tabellen als JSON oder CSV-Format exportieren.

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PayloadFormat:CSV

Bei der Angabe der Nutzlast CSV AzCopy erzeugt außerdem eine Schemadatei Endung `.schema.csv` für jede Datei.

### <a name="export-table-entities-concurrently"></a>Tabelle Personen gleichzeitig exportieren

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:C:\myfolder\ /SourceKey:key /PKRS:"aa#bb"

AzCopy starten gleichzeitige Vorgänge um Entitäten exportieren, wenn der Benutzer Option `/PKRS`. Jeder Vorgang exportiert eine Partition Schlüsselbereich.

Beachten Sie, dass die Anzahl der gleichzeitigen Operationen auch Option gesteuert `/NC`. AzCopy wird die Anzahl der Prozessoren der Standardwert `/NC` Tabelle Elemente kopieren, wenn `/NC` wurde nicht angegeben. Wenn der Benutzer die Option angibt `/PKRS`, AzCopy verwendet den kleineren der beiden Werte - Partition Schlüsselbereiche gegenüber implizit oder explizit angegebenen gleichzeitige Vorgänge - zu bestimmen Sie die Anzahl der gleichzeitigen Operationen starten. Geben Sie weitere Informationen `AzCopy /?:NC` in der Befehlszeile.

### <a name="export-table-to-blob"></a>Exportieren Sie Tabelle BLOB

    AzCopy /Source:https://myaccount.table.core.windows.net/myTable/ /Dest:https://myaccount.blob.core.windows.net/mycontainer/ /SourceKey:key1 /Destkey:key2

AzCopy generiert eine JSON-Datendatei in den BLOB-Container mit Namenskonvention:

    <account name>_<table name>_<timestamp>_<volume index>_<CRC>.json

Die generierte Datei JSON entspricht dem Format Nutzlast für minimale Metadaten. Ausführliche Informationen über dieses Format Nutzlast anzeigen Sie [Nutzlast Format für Dienstvorgänge Tabelle](http://msdn.microsoft.com/library/azure/dn535600.aspx)

Beachten Sie, dass beim Exportieren von Tabellen BLOBs AzCopy Tabellenentitäten auf lokale temporäre Dateien downloaden und uploaden die Entitäten in den Blob wird. Diese temporären Dateien werden in den Journalordner-Datei mit dem Standardpfad eingefügt "<code>%LocalAppData%\Microsoft\Azure\AzCopy</code>", Option angeben und z [Journal-Dateiordner] die Erfassung ändern Ordnerspeicherort Datei daher den Speicherort temporärer Dateien ändern. Größe der temporären Dateien entscheidet die Tabellenentitäten und die Größe, die mit der Option /SplitSize angegeben, obwohl die temporäre Datei im lokalen Datenträger sofort gelöscht wird, wenn das BLOB hochgeladen wurde, stellen sicher, dass Sie ausreichenden lokalen Speicherplatz diese temporären Dateien speichern, bevor sie gelöscht werden.

## <a name="table-import"></a>Tabelle: Importieren

### <a name="import-table"></a>Tabelle importieren

    AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.core.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace

Die Option `/EntityOperation` gibt an, wie Elemente in die Tabelle einfügen. Mögliche Werte sind:

- `InsertOrSkip`: Überspringt eine vorhandene Entität oder ein neues Element eingefügt, wenn in der Tabelle nicht vorhanden ist.
- `InsertOrMerge`: Führt eine vorhandene Entität oder ein neues Element eingefügt, wenn in der Tabelle nicht vorhanden ist.
- `InsertOrReplace`: Eine vorhandene Entität ersetzt oder eine neue Entität eingefügt, wenn in der Tabelle nicht vorhanden ist.

Beachten Sie, dass keine Option angegeben werden können `/PKRS` im Import-Szenario. Im Gegensatz zum Export-Szenario, in dem Geben Sie Option `/PKRS` zum gleichzeitigen Betrieb AzCopy standardmäßig beginnt gleichzeitige Vorgänge eine Tabelle importieren. Die Standardanzahl der gleichzeitigen Operationen gestartet ist gleich der Anzahl der Prozessoren. Sie können jedoch eine andere Anzahl von gleichzeitig mit der Option `/NC`. Geben Sie weitere Informationen `AzCopy /?:NC` in der Befehlszeile.

Beachten Sie, dass AzCopy unterstützt nur für JSON nicht CSV importieren. AzCopy nicht unterstützt Tabelle Einfuhren aus benutzerdefinierten JSON und Manifestdateien. Beide Dateien müssen ein Export AzCopy Tabelle stammen. Zur Vermeidung von Fehlern nicht exportierte JSON ändern oder Manifestdatei.

### <a name="import-entities-to-table-using-blobs"></a>Importieren von Entitäten auf die Tabelle mit blobs

Angenommen ein BLOB-Container enthält folgende: ein JSON-Datei, die eine Azure-Tabelle und die zugehörigen Manifestdatei.

    myaccount_mytable_20140103T112020.manifest
    myaccount_mytable_20140103T112020_0_0_0AF395F1DC42E952.json

Führen Sie den folgenden Befehl zum Importieren von Entitäten in eine Tabelle der Manifestdatei im BLOB-Container:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer /Dest:https://myaccount.table.core.windows.net/mytable /SourceKey:key1 /DestKey:key2 /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:"InsertOrReplace"

## <a name="other-azcopy-features"></a>Weitere AzCopy Funktionen

### <a name="only-copy-data-that-doesnt-exist-in-the-destination"></a>Nur Daten Sie, die im Ziel vorhanden ist

Die `/XO` und `/XN` Parameter können ältere oder neuere Quellressourcen kopiert, bzw. ausschließen. Möchten Sie nur Quellressourcen kopieren, die nicht im Ziel vorhanden sind, können Sie beide Parameter im Befehl AzCopy angeben:

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /XO /XN

    /Source:C:\myfolder /Dest:http://myaccount.file.core.windows.net/myfileshare /DestKey:<destkey> /S /XO /XN

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:http://myaccount.blob.core.windows.net/mycontainer1 /SourceKey:<sourcekey> /DestKey:<destkey> /S /XO /XN

Hinweis: Dies ist nicht unterstützt wird die Quelle oder das Ziel einer Tabelle.

### <a name="use-a-response-file-to-specify-command-line-parameters"></a>Mithilfe einer Antwortdatei Befehlszeilenparameter an

    AzCopy /@:"C:\responsefiles\copyoperation.txt"

Sie können AzCopy Befehlszeilenparameter in einer Antwortdatei enthalten. AzCopy verarbeitet die Parameter in der Datei als ob sie in der Befehlszeile angegeben wurden direkten Ersatz mit dem Inhalt der Datei ausführen.

Annehmen eine Antwortdatei `copyoperation.txt`, die folgenden Zeilen enthält. Jeder AzCopy Parameter kann in einer einzigen Zeile angegeben werden

    /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

oder auf separaten Zeilen:

    /Source:http://myaccount.blob.core.windows.net/mycontainer
    /Dest:C:\myfolder
    /SourceKey:<sourcekey>
    /S
    /Y

AzCopy schlägt fehl, wenn den Parameter auf zwei Zeilen aufgeteilt wie folgt für die `/sourcekey` Parameter:

    http://myaccount.blob.core.windows.net/mycontainer
    C:\myfolder
    /sourcekey:
    <sourcekey>
    /S
    /Y

### <a name="use-multiple-response-files-to-specify-command-line-parameters"></a>Verwenden Sie mehrere Antwortdateien Befehlszeilenparameter an

Angenommen eine Antwortdatei mit dem Namen `source.txt` einen Quellcontainer angibt:

    /Source:http://myaccount.blob.core.windows.net/mycontainer

Und eine Antwortdatei mit dem Namen `dest.txt` , die einen Ordner im Dateisystem angibt:

    /Dest:C:\myfolder

Und eine Antwortdatei mit dem Namen `options.txt` gibt, die Optionen für AzCopy:

    /S /Y

Um diese Antwortdateien AzCopy aufrufen, die in einem Verzeichnis befinden sich `C:\responsefiles`, verwenden Sie diesen Befehl:

    AzCopy /@:"C:\responsefiles\source.txt" /@:"C:\responsefiles\dest.txt" /SourceKey:<sourcekey> /@:"C:\responsefiles\options.txt"   

AzCopy verarbeitet dieser Befehl, wie einzelne Parameter in der Befehlszeile enthalten:

    AzCopy /Source:http://myaccount.blob.core.windows.net/mycontainer /Dest:C:\myfolder /SourceKey:<sourcekey> /S /Y

### <a name="specify-a-shared-access-signature-sas"></a>Geben Sie eine SAS (SAS)

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1 /Dest:https://myaccount.blob.core.windows.net/mycontainer2 /SourceSAS:SAS1 /DestSAS:SAS2 /Pattern:abc.txt

Sie können auch einen SAS im Container URI:

    AzCopy /Source:https://myaccount.blob.core.windows.net/mycontainer1/?SourceSASToken /Dest:C:\myfolder /S

### <a name="journal-file-folder"></a>Journal-Ordner

Jedes Mal, wenn ein zum AzCopy Befehl, überprüft, ob eine Erfassung im Standardordner vorhanden ist oder ob in einem Ordner vorhanden, die Sie über diese Option angegeben. Wenn die Protokolldatei nicht an beiden Stellen vorhanden ist, behandelt den Vorgang als neue AzCopy und generiert eine neue Journaldatei.

Die Journal-Datei vorhanden ist, wird AzCopy prüfen, ob die Eingabe Befehlszeile der Befehlszeile in die Journal-Datei entspricht. Wenn die zwei Zeilen übereinstimmen, wird AzCopy unvollständigen Vorgang fortgesetzt. Wenn sie nicht übereinstimmen, werden Sie aufgefordert, die Journaldatei einen neuen Vorgang starten oder den aktuellen Vorgang abbrechen überschreiben.

Wenn Sie den Standardspeicherort für die Journal-Datei verwenden möchten:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z

Option nicht `/Z`, oder geben Sie Option `/Z` ohne den Ordnerpfad wie oben gezeigt, AzCopy erstellt die Journaldatei der Standardspeicherort ist `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`. Wenn die Journal-Datei bereits vorhanden ist, setzt AzCopy den Vorgang anhand der Journaldatei.

Wenn Sie einen benutzerdefinierten Speicherort für die Protokolldatei angeben möchten:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Z:C:\journalfolder\

Dieses Beispiel erstellt die Journal-Datei, wenn es nicht bereits vorhanden ist. Wenn es vorhanden ist, setzt AzCopy den Vorgang anhand der Journaldatei.

Wenn Sie eine AzCopy Operation fortsetzen möchten:

    AzCopy /Z:C:\journalfolder\

Dieses Beispiel setzt den letzten Vorgang nicht abschließen kann.

### <a name="generate-a-log-file"></a>Erstellen einer Protokolldatei

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V

Wenn Sie Option `/V` ohne einen Dateipfad, das ausführliche Protokoll, dann AzCopy erstellt die Protokolldatei der Standardspeicherort ist `%SystemDrive%\Users\%username%\AppData\Local\Microsoft\Azure\AzCopy`.

Andernfalls können Sie eine Protokolldatei in einem benutzerdefinierten Verzeichnis erstellen:

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /V:C:\myfolder\azcopy1.log

Beachten Sie, dass wenn Sie einen relativen Pfad folgende Option angeben `/V`, wie `/V:test/azcopy1.log`, dann das ausführliche Protokoll im aktuellen Arbeitsverzeichnis in einem Unterordner mit dem Namen `test`.

### <a name="specify-the-number-of-concurrent-operations-to-start"></a>Geben Sie die Anzahl der gleichzeitigen Operationen starten

Option `/NC` gibt die Anzahl der gleichzeitig kopieren. AzCopy startet eine bestimmte Anzahl gleichzeitiger Vorgänge zu Übertragung Datendurchsatz. Tabellenvorgänge ist die Anzahl der gleichzeitigen Operationen gleich der Anzahl der Prozessoren haben Sie. Blob und Dateivorgänge, die Anzahl der gleichzeitigen Operationen ist gleich 8 Mal die Anzahl der Prozessoren haben Sie. Wenn Sie über ein Netzwerk mit niedriger Bandbreite AzCopy ausführen, können Sie eine niedrigere Zahl/NC zu Versagen von Ressourcen angeben.

### <a name="run-azcopy-against-azure-storage-emulator"></a>Azure Speicheremulator AzCopy ausführen

Der [Speicheremulator Azure](storage-use-emulator.md) für Blobs können AzCopy ausgeführt werden:

    AzCopy /Source:https://127.0.0.1:10000/myaccount/mycontainer/ /Dest:C:\myfolder /SourceKey:key /SourceType:Blob /S

und:

    AzCopy /Source:https://127.0.0.1:10002/myaccount/mytable/ /Dest:C:\myfolder /SourceKey:key /SourceType:Table

## <a name="azcopy-parameters"></a>AzCopy Parameter

Parameter für AzCopy werden nachfolgend beschrieben. Sie können die folgenden Befehle von der Befehlszeile aus zu eingeben, mit AzCopy:

- Ausführliche Befehlszeilenhilfe für AzCopy:`AzCopy /?`
- Detaillierte Hilfe AzCopy Parameter:`AzCopy /?:SourceKey`
- Beispiele für Befehlszeilen:`AzCopy /?:Samples`

### <a name="sourcesource"></a>/ Source: "Quelle"

Gibt die Quelldaten kopiert. Die Quelle kann ein Dateisystemverzeichnis BLOB-Container, ein virtuelles Verzeichnis Blob, Dateifreigabe Speicher, Speicher Verzeichnisse oder Azure Tabelle.

**Für:** BLOBs, Dateien, Tabellen

### <a name="destdestination"></a>/ Dest: "Ziel"

Gibt das Ziel für den Kopiervorgang. Das Ziel kann ein Dateisystemverzeichnis BLOB-Container, ein virtuelles Verzeichnis Blob, Dateifreigabe Speicher, Speicher Verzeichnisse oder Azure Tabelle sein.

**Für:** BLOBs, Dateien, Tabellen

### <a name="patternfile-pattern"></a>/ Pattern: "Dateimuster"

Gibt ein Dateimuster, der die zu kopierenden Dateien angibt. Das Verhalten des /Pattern-Parameters bestimmt den Speicherort der Quelldaten und das Vorhandensein der rekursiven Modus die Option. Rekursiver Modus wird über die Option/s angegeben.

Ist die angegebene Quelle ein Verzeichnis im Dateisystem, Platzhalter gültig sind und Dateimuster mit Dateien im Verzeichnis abgeglichen. Wenn die Option/s angegeben wird, dann AzCopy entspricht auch das angegebene Muster für alle Dateien im Unterordner unterhalb des Verzeichnisses.

Ist die angegebene Quelle einen BLOB-Container oder virtuelle Verzeichnis werden Platzhalter nicht angewendet. Wenn die Option/s angegeben wird, dann AzCopy angegebene Dateimuster als BLOB-Präfix interpretiert. Wenn die Option/s angegeben ist, dann AzCopy das Dateimuster genaue BLOB-Namen entspricht.

Wenn die angegebene Quelle Azure Dateifreigabe, Sie geben den genauen Dateinamen an (z. B. abc.txt) um eine einzelne Datei kopieren, oder geben Sie Option/s zum Kopieren aller Dateien in der Freigabe rekursiv. Versuch an einem Muster und die Option führt/s zusammen zu einem Fehler.

AzCopy wird die Groß-/Kleinschreibung entsprechen, wenn das Source einen BLOB-Container oder BLOB-virtuelle Verzeichnis und Kleinschreibung entsprechen in allen anderen Fällen verwendet.

Das Datei Standardmuster verwendet, wenn kein Dateimuster ist *.* für einen Speicherort im Dateisystem oder einem leeren Präfix für ein Azure-Speicherort. Angeben mehrerer Dateimuster wird nicht unterstützt.

**Für:** BLOBs, Dateien

### <a name="destkeystorage-key"></a>/ DestKey: "Storage-Key"

Gibt den Schlüssel Storage-Konto für die Zielressource.

**Für:** BLOBs, Dateien, Tabellen

### <a name="destsassas-token"></a>/ DestSAS: "Sas-Token"

Gibt eine freigegebene Access Signatur (SAS) mit Schreib- und Leserechte für das Ziel (falls zutreffend). Umgeben Sie SAS mit doppelten Anführungszeichen können sie Befehlszeile Sonderzeichen enthält.

Ist die Zielressource eine BLOB-Container, eine Dateifreigabe oder eine Tabelle, Sie können entweder diese Option gefolgt von SAS-Token oder SAS als Teil des Ziel-BLOB-Container, Dateifreigabe oder Tabelle URI ohne diese Option angeben.

Wenn Quelle und Ziel beide Blobs sind, muss das Ziel-Blob innerhalb desselben Speicher als Quell-Blob befinden.

**Für:** BLOBs, Dateien, Tabellen

### <a name="sourcekeystorage-key"></a>/ SourceKey: "Storage-Key"

Gibt den Speicher kontoschlüssel für die Ressource Quelle.

**Für:** BLOBs, Dateien, Tabellen

### <a name="sourcesassas-token"></a>/ SourceSAS: "Sas-Token"

Gibt eine SAS mit Berechtigungen zum Lesen und Liste der Quelle (falls zutreffend). Umgeben Sie SAS mit doppelten Anführungszeichen können sie Befehlszeile Sonderzeichen enthält.

Wenn Source-Ressource ein BLOB-Container ist und einen Schlüssel weder SAS bereitgestellt werden BLOB-Container über anonymen Zugriff gelesen.

Wenn die Quelle eine Dateifreigabe oder eine Tabelle, ein Schlüssel oder eine SAS vorzulegen.

**Für:** BLOBs, Dateien, Tabellen

### <a name="s"></a>/ S

Rekursiver Modus für Kopiervorgänge gibt. Im rekursiven Modus werden alle Blobs oder Dateien, die die angegebene Datei, einschließlich der Unterordner Muster AzCopy kopieren.

**Für:** BLOBs, Dateien

### <a name="blobtypeblock--page--append"></a>/ BlobType: "Block" | "Seite" | "append"

Gibt an, ob das Ziel-Blob ein Seitenblob oder ein Blob anfügen. Diese Option gilt nur, wenn Sie Blob hochladen. Andernfalls wird ein Fehler generiert. Wenn diese Option nicht angegeben, wird standardmäßig das Ziel ist ein Blob erstellt AzCopy ein.

**Für:** BLOBs

### <a name="checkmd5"></a>/ CheckMD5

Einen MD5-Hash für die heruntergeladenen Daten berechnet und überprüft, ob der MD5-Hash in dem Blob gespeichert oder Datei Content-MD5-Eigenschaft entspricht den berechneten Hashwert. MD5-Kontrollkästchen ist standardmäßig deaktiviert, daher müssen Sie diese Option, um die MD5-Überprüfung beim Herunterladen von Daten.

Beachten Sie, dass Azure-Speicher nicht garantiert, dass der MD5-Hash Blob oder Datei gespeichert ist. Es obliegt der Client MD5 aktualisiert das Blob oder der Datei geändert wird.

AzCopy wird immer die Content-MD5-Eigenschaft für ein Azure Blob oder der Datei nach dem Hochladen auf den Dienst.  

**Für:** BLOBs, Dateien

### <a name="snapshot"></a>/ Snapshot

Gibt an, ob Snapshots übertragen. Diese Option ist nur gültig, wenn die Quelle ein Blob ist.

Übertragene Blob-Snapshots werden in diesem Format umbenannt: Herausforderer Blob-Name (Snapshot-Zeit)

Standardmäßig werden Snapshots nicht kopiert.

**Für:** BLOBs

### <a name="vverbose-log-file"></a>/ V [verbose Log-Datei]

Ausgaben Ausführliche Statusnachrichten in eine Protokolldatei.

Standardmäßig heißt die ausführliche Protokolldatei AzCopyVerbose.log in `%LocalAppData%\Microsoft\Azure\AzCopy`. Wenn Sie einen vorhandenen Speicherort für diese Option angeben, wird das ausführliche Protokoll an diese Datei angefügt.  

**Für:** BLOBs, Dateien, Tabellen

### <a name="zjournal-file-folder"></a>/ Z [Journal-Dateiordner]

Gibt einen Journalordner für einen Vorgang fortsetzen.

AzCopy unterstützt immer wieder eine Operation abgebrochen wurde.

Wenn diese Option nicht angegeben, oder ohne einen Ordnerpfad angegeben wird, wird AzCopy die Journaldatei am Standardspeicherort erstellen ist LocalAppData%\Microsoft\Azure\AzCopy.

Jedes Mal, wenn ein zum AzCopy Befehl, überprüft, ob eine Erfassung im Standardordner vorhanden ist oder ob in einem Ordner vorhanden, die Sie über diese Option angegeben. Wenn die Protokolldatei nicht an beiden Stellen vorhanden ist, behandelt den Vorgang als neue AzCopy und generiert eine neue Journaldatei.

Die Journal-Datei vorhanden ist, wird AzCopy prüfen, ob die Eingabe Befehlszeile der Befehlszeile in die Journal-Datei entspricht. Wenn die zwei Zeilen übereinstimmen, wird AzCopy unvollständigen Vorgang fortgesetzt. Wenn sie nicht übereinstimmen, werden Sie aufgefordert, die Journaldatei einen neuen Vorgang starten oder den aktuellen Vorgang abbrechen überschreiben.

Nach dem erfolgreichen Abschluss des Vorgangs wird die Journal-Datei gelöscht.

Beachten Sie, dass einen Vorgang eine Journaldatei fortsetzen von einer vorherigen Version von AzCopy erstellt wird nicht unterstützt.

**Für:** BLOBs, Dateien, Tabellen

### <a name="parameter-file"></a>/@:"parameter-file"

Gibt eine Datei an, die Parameter enthält. AzCopy verarbeitet die Parameter in der Datei, als ob sie in der Befehlszeile angegeben wurden.

In einer Antwortdatei können Sie mehrere Parameter in einer einzigen Zeile angeben oder jeden Parameter in einer eigenen Zeile anzugeben. Beachten Sie, dass ein einzelner Parameter kann nicht über mehrere Zeilen erstrecken.

Antwortdateien können Kommentarzeilen enthalten, die mit dem Symbol # beginnen.

Sie können mehrere Antwortdateien. Beachten Sie jedoch, dass AzCopy geschachtelte Antwortdateien nicht unterstützt.

**Für:** BLOBs, Dateien, Tabellen

### <a name="y"></a>/ Y

Unterdrückt alle AzCopy bestätigen.

**Für:** BLOBs, Dateien, Tabellen

### <a name="l"></a>/ L

Gibt eine Auflistung-Operation. keine Daten.

AzCopy interpretiert wird, der mit dieser Option Simulation zum Ausführen von der Befehlszeile aus ohne die option/l und zählen, wie viele Objekte kopiert, Option im ausführlichen Protokoll v gleichzeitig überprüfen die Objekte kopiert werden können.

Das Verhalten dieser Option auch den Speicherort der Quelldaten und das Vorhandensein der rekursiven Modus Option/s Datei Muster Option und /Pattern bestimmt.

AzCopy erfordert eine Liste und Lesen Berechtigung dieses Quellspeicherort bei Verwendung dieser Option.

**Für:** BLOBs, Dateien

### <a name="mt"></a>HT

Legt die Datei zuletzt geändert Zeit als Quell-Blob oder der Datei übereinstimmen.

**Für:** BLOBs, Dateien

### <a name="xn"></a>/ XN

Schließt eine neuere Quelle Ressource. Die Ressource wird nicht kopiert, wenn die Uhrzeit der letzten Änderung der Quelle übereinstimmt oder neuer als Ziel.

**Für:** BLOBs, Dateien

### <a name="xo"></a>/ XO

Schließt eine ältere Quelle Ressource. Die Ressource wird nicht kopiert, wenn die Uhrzeit der letzten Änderung der Quelle identisch ist oder älter als Ziel.

**Für:** BLOBs, Dateien

### <a name="a"></a>/ A

Lädt nur Dateien, die das Archivattribut gesetzt.

**Für:** BLOBs, Dateien

### <a name="iarashcnetoi"></a>/ IA: [RASHCNETOI]

Lädt nur Dateien, die alle angegebenen Attribute festlegen.

Verfügbare Attribute enthalten:

- R = Schreibgeschützte Dateien
- A = zur Archivierung bereite Dateien
- S = Systemdateien
- H = Versteckte Dateien
- C = komprimierte Dateien
- N = normale Dateien
- E = verschlüsselte Dateien
- T = temporäre Dateien
- O = Offlinedateien
- I = nicht indizierten Dateien

**Für:** BLOBs, Dateien

### <a name="xarashcnetoi"></a>/ XA: [RASHCNETOI]

Schließt Dateien ein, die alle angegebenen Attribute festlegen.

Verfügbare Attribute enthalten:

- R = Schreibgeschützte Dateien
- A = zur Archivierung bereite Dateien
- S = Systemdateien
- H = Versteckte Dateien
- C = komprimierte Dateien
- N = normale Dateien
- E = verschlüsselte Dateien
- T = temporäre Dateien
- O = Offlinedateien
- I = nicht indizierten Dateien

**Für:** BLOBs, Dateien

### <a name="delimiterdelimiter"></a>/ Trennzeichen: "Trennzeichen"

Gibt das Trennzeichen zum Trennen der virtuellen Verzeichnisse in ein Blob.

Standardmäßig verwendet AzCopy / als Trennzeichen. AzCopy unterstützt gemeinsamen Zeichen (z. B. @, # oder %) als Trennzeichen. Möchten Sie eines dieser Sonderzeichen in der Befehlszeile enthalten, schließen Sie den Dateinamen in Anführungszeichen.

Diese Option gilt nur für das Herunterladen von Blobs.

**Für:** BLOBs

### <a name="ncnumber-of-concurrent-operations"></a>/ NC: "Anzahl-der-gleichzeitige-Operationen"

Gibt die Anzahl der gleichzeitigen Operationen.

AzCopy standardmäßig beginnt eine bestimmte Anzahl gleichzeitiger Vorgänge zu Übertragung Datendurchsatz. Beachten Sie, dass viele gleichzeitige Vorgänge in einer Umgebung mit geringer Bandbreite kann des Netzwerks überschwemmen und verhindern, dass die Vorgänge vollständig abgeschlossen. Gleichzeitige Vorgänge basierend auf der aktuell verfügbaren Netzwerkbandbreite zu drosseln.

Die obere Grenze für gleichzeitige Vorgänge ist 512.

**Für:** BLOBs, Dateien, Tabellen

### <a name="sourcetypeblob--table"></a>/ SourceType: "Blob" | "Tabelle"

Gibt an, dass die `source` ist ein Blob in der lokalen, im Speicheremulator verfügbar.

**Für:** BLOBs, Tabellen

### <a name="desttypeblob--table"></a>/ DestType: "Blob" | "Tabelle"

Gibt an, dass die `destination` ist ein Blob in der lokalen, im Speicheremulator verfügbar.

**Für:** BLOBs, Tabellen

### <a name="pkrskey1key2key3"></a>/ PKRS: "key1 #key2 #key3 #..."

Teilt den Schlüsselbereich Partition zum Exportieren von Daten parallel die erhöht die Geschwindigkeit des Exportvorgangs aktivieren.

Wenn diese Option nicht angegeben ist, verwendet AzCopy einen einzelnen Thread Tabellenentitäten exportieren. Angenommen, der Benutzer gibt /PKRS an: "aa #bb" und AzCopy beginnt drei gleichzeitige Vorgänge.

Jeder Vorgang exportiert eine der drei Schlüsselbereiche von Partition wie folgt:

  [erste Partitionsschlüssel, aa)

  [aa, bb)

  [letzten Partitionsschlüssel bb]

**Für:** Tabellen

### <a name="splitsizefile-size"></a>/ SplitSize: "Dateigröße"

Gibt die exportierte Datei Teilen Größe in MB, den minimal zulässigen Wert 32.

Wenn diese Option nicht angegeben ist, wird AzCopy Daten in Datei exportieren.

Wenn die Tabellendaten in ein Blob exportiert und Größe der exportierten Datei die 200 GB für BLOB-Größe erreicht werden AzCopy die exportierte Datei aufgeteilt, auch wenn diese Option nicht angegeben ist.

**Für:** Tabellen

### <a name="entityoperationinsertorskip--insertormerge--insertorreplace"></a>/ EntityOperation: "InsertOrSkip" | "InsertOrMerge" | "InsertOrReplace"

Gibt das Tabelle Daten importieren.

- InsertOrSkip – überspringt eine vorhandene Entität oder ein neues Element eingefügt, wenn in der Tabelle nicht vorhanden ist.

- InsertOrMerge - führt eine vorhandene Entität oder ein neues Element eingefügt, wenn in der Tabelle nicht vorhanden ist.

- InsertOrReplace - ersetzt eine vorhandene Entität oder ein neues Element eingefügt, wenn in der Tabelle nicht vorhanden ist.

**Für:** Tabellen

### <a name="manifestmanifest-file"></a>/ Manifest: "Manifest-Datei"

Gibt die manifest-Datei die Tabelle Export-und Importvorgang.

Diese Option ist beim Exportvorgang optional, AzCopy eine Manifestdatei mit vordefinierten Namen generiert, wenn diese Option nicht angegeben ist.

Diese Option ist während des Importvorgangs zum Suchen von Dateien erforderlich.

**Für:** Tabellen

### <a name="synccopy"></a>/ SyncCopy

Gibt an, ob synchron Blobs oder Dateien zwischen zwei Endpunkten Azure-Speicher kopieren.

AzCopy standardmäßig verwendet serverseitige asynchrone Kopie. Geben Sie diese Option, um eine synchrone Kopie ausführen, die Blobs oder Dateien in den lokalen Speicher heruntergeladen und dann in den Azure-Speicher hochgeladen.

Beim Kopieren von Dateien im BLOB-Speicher im Dateispeicher oder BLOB-Speicher oder Datei speichern, können Sie diese Option.

**Für:** BLOBs, Dateien

### <a name="setcontenttypecontent-type"></a>/ SetContentType: "Content-Type"

Gibt den MIME-Inhaltstyp Ziel Blobs oder Dateien.

AzCopy legt den Inhaltstyp für ein Blob oder der Datei standardmäßig auf Application/Octet-Stream fest. Den Inhaltstyp für alle Blobs oder Dateien lassen sich explizit einen Wert für diese Option angeben.

Wenn Sie diese Option nicht angeben, wird AzCopy jedes Blob oder Inhaltstyp nach der Erweiterung der Datei festgelegt.

**Für:** BLOBs, Dateien

### <a name="payloadformatjson--csv"></a>/ PayloadFormat: "JSON" | "CSV"

Gibt das Format der exportierten Datendatei Tabelle.

Wenn diese Option nicht angegeben ist, Exportdatei standardmäßig AzCopy Tabelle Daten im JSON-Format.

**Für:** Tabellen

## <a name="known-issues-and-best-practices"></a>Bekannte Probleme und bewährte Methoden

### <a name="limit-concurrent-writes-while-copying-data"></a>Einzuschränken Sie gleichzeitige Schreibvorgänge beim Kopieren der Daten

Beim Kopieren von Blobs oder Dateien mit AzCopy Denken Sie daran, dass eine andere Anwendung Daten ändert, während Sie es kopieren. Wenn möglich, sicherzustellen Sie, dass die kopierten Daten während des Kopiervorgangs nicht geändert wird. Beispielsweise beim Kopieren von zugeordneten Azure virtuellen Computer einer virtuelle Festplatte unbedingt derzeit nicht auf der virtuellen Festplatte schreiben. Hierzu lässt Leasing Ressource kopiert werden. Alternativ können Sie zunächst einen Snapshot der virtuellen Festplatte und kopieren Sie dann den Snapshot.

Wenn Sie nicht, dass andere Programme auf Blobs oder Dateien schreiben verhindern können, während sie kopiert werden, dann denken Sie daran, dass zum Zeitpunkt des Jobs kopierten Ressourcen mehr vollständige Parität mit der Quelle möglicherweise.

### <a name="run-one-azcopy-instance-on-one-machine"></a>Eine AzCopy-Instanz auf einem Computer ausführen.
AzCopy soll die Nutzung der Maschine Ressource zur Beschleunigung der Datenübertragung zu maximieren, empfiehlt es sich, nur eine AzCopy-Instanz auf einem Computer ausführen, und geben Sie die Option `/NC` benötigen Sie mehr gleichzeitige Vorgänge. Geben Sie weitere Informationen `AzCopy /?:NC` in der Befehlszeile.

### <a name="enable-fips-compliant-md5-algorithms-for-azcopy-when-you-use-fips-compliant-algorithms-for-encryption-hashing-and-signing"></a>FIPS-konformen MD5-Algorithmus für AzCopy aktivieren Wenn Sie "mit FIPS-konformen Algorithmus für Verschlüsselung, hashing und Signatur".
AzCopy standardmäßig verwendet .NET MD5 Implementierung MD5 berechnet, wenn Sie Objekte kopieren, aber es gibt einige Anforderungen, die AzCopy FIPS-kompatible MD5 aktivieren.

Erstellen Sie eine App.config.Datei `AzCopy.exe.config` -Eigenschaft `AzureStorageUseV1MD5` und mit AzCopy.exe.

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
      <appSettings>
        <add key="AzureStorageUseV1MD5" value="false"/>
      </appSettings>
    </configuration>

Für die Eigenschaft "AzureStorageUseV1MD5" • True - verwendet der Standardwert AzCopy .NET MD5-Implementierung.
• Falsch – AzCopy verwendet die FIPS-konforme MD5-Algorithmus.

Beachten Sie, dass die FIPS-konforme Algorithmen wird standardmäßig auf Ihrem Windows-Computer, geben Sie secpol.msc im Fenster ausführen und Überprüfen dieser Schalter zur Sicherheitsrichtlinie -> lokale Richtlinie-Sicherheit > Optionen -> Systemkryptografie: FIPS-konformen Algorithmus für Verschlüsselung, hashing und Signatur.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure-Speicher und AzCopy finden Sie in den folgenden Ressourcen.

### <a name="azure-storage-documentation"></a>Azure Dokumentation:

- [Einführung in Azure-Speicher](storage-introduction.md)
- [Wie BLOB-Speicher von .NET](storage-dotnet-how-to-use-blobs.md)
- [Verwendung von .NET Dateispeicher](storage-dotnet-how-to-use-files.md)
- [Wie Tabellenspeicher von .NET](storage-dotnet-how-to-use-tables.md)
- [Erstellen, verwalten oder löschen Sie ein Speicherkonto](storage-create-storage-account.md)

### <a name="azure-storage-blog-posts"></a>Azure Storage Blogbeiträgen:
- [Einführung in Azure-Speicher Datenvorschau Bewegung Bibliothek](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)
- [AzCopy: Einführung in die synchrone Kopie und benutzerdefinierten Inhaltstyp](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/01/13/azcopy-introducing-synchronous-copy-and-customized-content-type.aspx)
- [AzCopy: Ankündigung der allgemeinen Verfügbarkeit von AzCopy 3.0 plus Vorabversion von AzCopy 4.0 Tabelle und Datei](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/10/29/azcopy-announcing-general-availability-of-azcopy-3-0-plus-preview-release-of-azcopy-4-0-with-table-and-file-support.aspx)
- [AzCopy: Für umfangreiche Szenarien optimiert](http://go.microsoft.com/fwlink/?LinkId=507682)
- [AzCopy: Unterstützung für Lesezugriff Geo redundante Speicherung](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/04/07/azcopy-support-for-read-access-geo-redundant-account.aspx)
- [AzCopy: Datenübertragung startfähige Modus mit SAS-token](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/09/07/azcopy-transfer-data-with-re-startable-mode-and-sas-token.aspx)
- [AzCopy: Verwendung von Cross-Konto kopieren Blob](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/04/01/azcopy-using-cross-account-copy-blob.aspx)
- [AzCopy: Hochladen/Herunterladen für Azure-Blobs](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/12/03/azcopy-uploading-downloading-files-for-windows-azure-blobs.aspx)
