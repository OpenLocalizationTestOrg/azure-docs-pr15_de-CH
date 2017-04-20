<properties
    pageTitle="Azure-Speicher mit Azure CLI | Microsoft Azure"
    description="Informationen Sie zum Verwenden der Azure-Befehlszeilenschnittstelle (CLI Azure) mit Azure Storage Konten und Azure-Blobs mit Dateien arbeiten. Azure-CLI ist eine plattformübergreifende tool "
    services="storage"
    documentationCenter="na"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="using-the-azure-cli-with-azure-storage"></a>Verwenden von Azure CLI mit Azure-Speicher

## <a name="overview"></a>Übersicht

Azure-CLI bietet eine Reihe von open Source plattformübergreifende Befehle für die Arbeit mit der Azure-Plattform. Es bietet im Wesentlichen die gleiche Funktionalität in [Azure-Portal](https://portal.azure.com) als auch umfangreiche Datenzugriffsfunktionen gefunden.

In diesem Handbuch erforschen wir, wie Sie [Azure-Befehlszeilenschnittstelle (CLI Azure)](../xplat-cli-install.md) eine Vielzahl von Entwicklungs- und Verwaltungsaufgaben mit Azure-Speicher. Wir empfehlen, downloaden und installieren oder auf die neueste Azure CLI aktualisieren vor der Verwendung dieses Handbuchs.

Dieses Handbuch setzt voraus, dass Sie die grundlegenden Konzepte der Azure-Speicher verstehen. Das Handbuch enthält eine Reihe von Skripts veranschaulichen die Verwendung der Azure-CLI mit Azure-Speicher. Achten Sie darauf, dass Sie basierten auf der Konfiguration vor dem Ausführen jedes Skript Skriptvariablen aktualisieren.

> [AZURE.NOTE] Das Handbuch enthält Azure CLI-Befehl und Skript Beispiele für klassische Speicherkonten. [Verwendung der Azure-CLI für Mac, Linux und Windows Azure Resource Management](../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) Azure-CLI für Ressourcenmanager Speicherkonten Befehle angezeigt.

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Erste Schritte mit Azure Storage und Azure-CLI in 5 Minuten

Ubuntu Beispiele verwendet jedoch anderen Betriebssystemen ähnlich ausführen.

**Neue Azure:** Microsoft Azure-Abonnement und eine Microsoft-Konto mit diesem Abonnement zu erhalten. Informationen zu Azure Kaufoptionen finden Sie [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/), [Erwerben](https://azure.microsoft.com/pricing/purchase-options/)und [Bietet Member](https://azure.microsoft.com/pricing/member-offers/) (für Mitglieder von MSDN, Microsoft Partner Network und BizSpark und andere Microsoft-Programme).

Weitere Informationen zu Azure-Abonnements finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Nach dem Erstellen einer Microsoft Azure-Abonnement und Konto:**

1. Downloaden und Installieren der Azure-CLI dabei in [Azure CLI installieren](../xplat-cli-install.md).
2. Installierte Azure CLI können Sie mit Azure Befehl über die Befehlszeilenschnittstelle (Bash Terminal Befehlszeile) Azure-CLI-Befehle zugreifen. Typ `azure` Befehl sollte die folgende Ausgabe angezeigt.

    ![Azure Befehlsausgabe][Image1]

3. Geben Sie in der Befehlszeilenschnittstelle `azure storage` der Azure-Speicher Befehle und erhalten einem Eindruck von den Funktionen der Azure-CLI bietet. Befehlsnamen **-h** Parameter eingeben (z. B. `azure storage share create -h`) Befehlssyntax angezeigt.
4. Jetzt geben wir Ihnen ein einfaches Skript, das grundlegende Azure-CLI-Befehle auf Azure-Speicher anzeigt. Das Skript fragt zunächst zwei Variablen für das Speicherkonto und den Schlüssel festgelegt. Dann das Skript in diesem neuen Speicherkonto einen neuen Container erstellen und Hochladen eine vorhandenen Bilddatei (Blob) in diesem Container. Nachdem das Skript alle Blobs im Container enthält, werden die Image-Datei in das Zielverzeichnis gedownloadet, die auf dem lokalen Computer existiert.

        #!/bin/bash
        # A simple Azure storage example

        export AZURE_STORAGE_ACCOUNT=<storage_account_name>
        export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

        export container_name=<container_name>
        export blob_name=<blob_name>
        export image_to_upload=<image_to_upload>
        export destination_folder=<destination_folder>

        echo "Creating the container..."
        azure storage container create $container_name

        echo "Uploading the image..."
        azure storage blob upload $image_to_upload $container_name $blob_name

        echo "Listing the blobs..."
        azure storage blob list $container_name

        echo "Downloading the image..."
        azure storage blob download $container_name $blob_name $destination_folder

        echo "Done"

5. Öffnen Sie auf Ihrem lokalen Computer Ihren bevorzugten Texteditor (z. B. Vim). Geben Sie das obige Skript in den Texteditor.

6. Jetzt müssen Sie basierten auf Ihren Konfigurationen Skriptvariablen aktualisieren.

    - **< Storage_account_name >** Der Vorname in das Skript oder geben Sie einen neuen Namen für das Speicherkonto. **Wichtig:** Der Name des Speicherkontos muss in Azure eindeutig sein. Es muss auch Kleinbuchstaben sein!

    - **< Storage_account_key >** Die Zugriffstaste für das Speicherkonto.

    - **< Container_name >** Vorname in das Skript oder geben Sie einen neuen Namen für den Container.

    - **< Image_to_upload >** Geben Sie einen Pfad zu einem Bild auf dem lokalen Computer wie: "~ / images/HelloWorld.png".

    - **< Destination_folder >** Geben Sie einen Pfad zu einem lokalen Verzeichnis aus dem Azure-Speicher wie heruntergeladene Dateien speichern: "~/downloadImages".

7. Drücken Sie nach der Aktualisierung erforderlichen Variablen in Vim Tastenkombinationen "Esc:, Wq!" um das Skript zu speichern.

8. Um dieses Skript ausführen, geben Sie einfach den Namen der Skriptdatei Bash-Konsole. Nach der Ausführung dieses Skripts müssen Sie einen lokalen Ordner, der die gedownloadete Datei enthält. Der folgende Screenshot zeigt eine Beispiel für die Ausgabe:

Nach der Ausführung des Skripts müssen Sie einen lokalen Ordner, der die gedownloadete Datei enthält.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Speicher mit der Azure-CLI verwalten

### <a name="connect-to-your-azure-subscription"></a>Verbinden Sie mit der Azure-Abonnement

Während die meisten Befehle Speicher ohne Azure-Abonnement funktioniert, empfehlen wir Ihnen Verbindung zu Ihrem Abonnement von Azure-CLI. Konfigurieren der Azure-Befehlszeilenschnittstelle arbeiten Ihr Abonnement gehen Sie in [Verbindung mit Azure-Abonnement von Azure-CLI](../xplat-cli-connect.md).

### <a name="create-a-new-storage-account"></a>Erstellen eines neuen Speicherkontos

Um Azure-Speicher zu verwenden, benötigen Sie ein Speicherkonto. Sie können ein neues Azure Storage-Konto nach der Konfiguration des Computers zum Abonnement erstellen.

        azure storage account create <account_name>

Der Namen Ihres Speicherkontos muss zwischen 3 und 24 Zeichen und Zahlen und nur Kleinbuchstaben.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Ein Azure-Speicher Standardkonto in Umgebungsvariablen festgelegt

Sie können mehrere Speicherkonten in Ihrem Abonnement verfügen. Wählen sie, und in den Umgebungsvariablen für alle Speicher-Befehle in der gleichen Sitzung festlegen. Dadurch führen Sie Befehle Speicher Azure CLI ohne das Speicherkonto und explizit.

        export AZURE_STORAGE_ACCOUNT=<account_name>
        export AZURE_STORAGE_ACCESS_KEY=<key>

Verbindungszeichenfolge verwendet ein Speicherkonto standardmäßig festgelegt. Erstens erhalten Sie die Verbindungszeichenfolge vom Befehl:

        azure storage account connectionstring show <account_name>

Ausgabe-Verbindungszeichenfolge kopieren und Umgebungsvariablen festgelegt:

        export AZURE_STORAGE_CONNECTION_STRING=<connection_string>

## <a name="create-and-manage-blobs"></a>Erstellen und Verwalten von blobs

Azure BLOB-Speicher ist ein Dienst zum Speichern großer Mengen an unstrukturierten Daten wie Text oder binär, die überall in der Welt über HTTP oder HTTPS zugegriffen werden kann. In diesem Abschnitt wird davon ausgegangen, dass Sie bereits mit den Azure BLOB-Speicher-Konzepten vertraut sind. Ausführliche Informationen finden Sie unter [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md) und [BLOB-Konzepte](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Erstellen eines Containers

Jedes Blob in Azure-Speicher muss sich in einem Container. Erstellen Sie eine private Container mit der `azure storage container create` Befehl:

        azure storage container create mycontainer

> [AZURE.NOTE] Es gibt drei anonymen Lesezugriff: **Off** **Blob**und **Container**. Anonymen Zugriff auf Blobs festgelegt Parameters Berechtigung **aus**. Standardmäßig der neue Container ist privat und kann nur vom Kontoinhaber zugegriffen werden. Um anonyme öffentliche Lesezugriff auf BLOB-Ressourcen, aber nicht auf Container Metadaten oder zur Liste der Blobs im Container Parameters Berechtigung auf **Blob**zu ermöglichen. Um öffentliche Lesezugriff auf BLOB-Ressourcen zu ermöglichen, legen Container Metadaten und die Liste der Blobs im Container Parameters Berechtigung **Container**. Weitere Informationen finden Sie unter [Verwalten anonymen Lesezugriff auf Container und Blobs](storage-manage-access-to-resources.md).

### <a name="upload-a-blob-into-a-container"></a>Ein Container einen Blob hochladen

Block-Blobs und Seitenblobs unterstützt Azure BLOB-Speicher. Weitere Informationen finden Sie unter [Understanding Block Blobs, Anhängen Blobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Blobs in einem Container hochladen, können Sie die `azure storage blob upload`. Standardmäßig lädt dieser Befehl die lokalen Dateien an ein. Für das Blob geben, können Sie die `--blobtype` Parameter.

        azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob

### <a name="download-blobs-from-a-container"></a>Herunterladen von Blobs aus einem container

Im folgenden Beispiel wird veranschaulicht, wie Container Blobs herunterladen.

        azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'

### <a name="copy-blobs"></a>Kopieren des blobs

Sie können asynchron Blobs innerhalb oder zwischen Speicherkonten und Regionen.

Im folgenden Beispiel wird veranschaulicht, wie Blobs von einem Speicherkonto kopieren. In diesem Beispiel erstellen wir einen Container Blobs öffentlich sind anonym zugegriffen werden.

    azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

    azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

    azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer

Dieses Beispiel führt eine asynchrone Kopie. Überwacht den Status der einzelnen Kopiervorgang mit der `azure storage blob copy show` Vorgang.

Beachten Sie, dass die Quell-URL für den Kopiervorgang entweder öffentlich zugänglich oder SAS (SAS) Zeichen enthalten.

### <a name="delete-a-blob"></a>Löschen eines BLOBs

Um ein Blob zu löschen, verwenden Sie den folgenden Befehl:

        azure storage blob delete mycontainer myBlockBlob2

## <a name="create-and-manage-file-shares"></a>Erstellen und Verwalten von Dateifreigaben

Azure Dateispeicher bietet freigegebenen Speicher für Applikationen mit SMB-Standardprotokoll. Microsoft Azure-Computer und Cloud-Services sowie lokale Anwendung können Daten über bereitgestellte Freigaben freigeben. Sie können Dateien und Daten über die CLI Azure verwalten. Weitere Informationen zu Azure Dateispeicher finden Sie unter [Erste Schritte mit Windows Azure Dateispeicher](storage-dotnet-how-to-use-files.md) oder [wie Azure Dateispeicher mit Linux](storage-how-to-use-files-linux.md).

### <a name="create-a-file-share"></a>Erstellen einer Dateifreigabe

Ein Azure-Datei ist eine SMB-Datei in Azure. Alle Verzeichnisse und Dateien müssen in einer Dateifreigabe erstellt. Ein Konto kann eine unbegrenzte Anzahl von Freigaben enthalten, und eine Freigabe kann eine unbegrenzte Anzahl von Dateien, die Kapazitätsgrenzen des Speicherkontos speichern. Das folgende Beispiel erstellt eine Dateifreigabe mit der Bezeichnung **Myshare**.

        azure storage share create myshare

### <a name="create-a-directory"></a>Erstellen Sie ein Verzeichnis

Ein Verzeichnis bereit Azure Dateifreigabe optionale hierarchische Struktur. Das folgende Beispiel erstellt ein Verzeichnis namens **MyDir** in der Dateifreigabe.

        azure storage directory create myshare myDir

Beachten Sie, dass Verzeichnispfad kann *z. B.*mehrere Ebenen enthalten **eine / b**. Allerdings müssen Sie sicherstellen, dass alle übergeordneten Verzeichnisse vorhanden ist. Beispielsweise Pfad **eine / b**, müssen Sie zunächst Verzeichnis **ein** und dann Verzeichnis **b**erstellen.

### <a name="upload-a-local-file-to-directory"></a>Upload einer lokalen Datei in Verzeichnis

Das folgende Beispiel lädt eine Datei **~/temp/samplefile.txt** im Verzeichnis **MyDir** . Bearbeiten Sie den Pfad verweist auf eine gültige Datei auf dem lokalen Computer:

        azure storage file upload '~/temp/samplefile.txt' myshare myDir

Beachten Sie, dass eine Datei in der Freigabe bis zu 1 TB groß sein kann.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Listet die Dateien im Stammverzeichnis der Freigabe oder Verzeichnis

Sie können die Dateien und Unterverzeichnisse im Stamm einer Freigabe oder einem Verzeichnis mit dem folgenden Befehl auflisten:

        azure storage file list myshare myDir

Beachten Sie, dass der Name des Verzeichnisses für den Vorgang Angebot optional. Wenn nicht angegeben, listet der Befehl den Inhalt des Stammverzeichnisses der Freigabe.

### <a name="copy-files"></a>Dateien kopieren

Ab Version 0.9.8 von Azure-Befehlszeilenschnittstelle können Sie eine Datei eine Blob oder ein Blob in einer Datei, einer anderen Datei kopieren. Hier demonstrieren wir diese CLI-Befehlen Kopieren Operationen. Eine Datei in das neue Verzeichnis zu kopieren:

    azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

Einen Blob in ein Dateiverzeichnis zu kopieren:

    azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString

## <a name="next-steps"></a>Nächste Schritte

Hier sind einige Artikeln und Ressourcen zur Vertiefung Azure-Speicher.

- [Azure-Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/)
- [Azure Storage REST-API-Referenz](https://msdn.microsoft.com/library/azure/dd179355.aspx)


[Image1]: ./media/storage-azure-cli/azure_command.png
