<properties
    pageTitle="Azure-Speicher mit Azure PowerShell | Microsoft Azure"
    description="Informationen Sie zum Verwenden von Azure PowerShell-Cmdlets zum Azure-Speicher erstellen und Verwalten von Speicherkonten; Arbeiten Sie mit Blobs, Tabellen, Warteschlangen und Dateien. Konfigurieren und Erstellen von SAS Speicheranalyse Abfragen."
    services="storage"
    documentationCenter="na"
    authors="robinsh"
    manager="carmonm"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="using-azure-powershell-with-azure-storage"></a>Verwenden von Azure PowerShell mit Azure-Speicher

## <a name="overview"></a>Übersicht

Azure PowerShell ist ein Modul, die Cmdlets zum Verwalten von Azure über Windows PowerShell bereitstellt. Es ist eine aufgabenbasierte Befehlszeilenshell und Skriptsprache, die speziell für die Systemadministration. Mit PowerShell können Sie Steuerung und Automatisierung die Verwaltung Ihrer Azure Services und Applikationen. Die Cmdlets können Sie dieselben Aufgaben ausführen wie [Azure-Portal](https://portal.azure.com)können.

In diesem Handbuch erforschen wir, wie [Azure Storage-Cmdlets](https://msdn.microsoft.com/library/azure/mt269418.aspx) verwenden, um eine Vielzahl von Entwicklungs- und Verwaltungsaufgaben mit Azure-Speicher.

Dieses Handbuch setzt voraus, dass Erfahrung [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) mit [Windows PowerShell](http://technet.microsoft.com/library/bb978526.aspx)haben. Das Handbuch enthält eine Reihe von Skripts veranschaulichen die Verwendung von PowerShell mit Azure-Speicher. Aktualisieren Sie basierten auf der Konfiguration vor dem Ausführen jedes Skript Skriptvariablen.

Der erste Abschnitt in diesem Handbuch bietet einen Einblick in Azure Storage und PowerShell. Ausführliche Informationen und eine Anleitung Starten von [Voraussetzung für die Verwendung von Azure PowerShell mit Azure-Speicher](#prerequisites-for-using-azure-powershell-with-azure-storage).


## <a name="getting-started-with-azure-storage-and-powershell-in-5-minutes"></a>Erste Schritte mit Azure PowerShell in 5 Minuten

Dieser Abschnitt veranschaulicht die Azure-Speicher über PowerShell in 5 Minuten zugreifen.

**Neue Azure:** Microsoft Azure-Abonnement und eine Microsoft-Konto mit diesem Abonnement zu erhalten. Informationen zu Azure Kaufoptionen finden Sie [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/), [Erwerben](https://azure.microsoft.com/pricing/purchase-options/)und [Bietet Member](https://azure.microsoft.com/pricing/member-offers/) (für Mitglieder von MSDN, Microsoft Partner Network und BizSpark und andere Microsoft-Programme).

Weitere Informationen zu Azure-Abonnements finden Sie unter [Zuweisen von Administratorrollen in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) .

**Nach dem Erstellen einer Microsoft Azure-Abonnement und Konto:**

1.  Downloaden und Installieren von [Azure PowerShell](http://go.microsoft.com/?linkid=9811175&clcid=0x409).
2.  Start Windows PowerShell Integrated Scripting Environment (ISE): Auf Ihrem lokalen Computer werden Sie im **Startmenü** . Geben Sie **Verwaltung** und klicken Sie auf ausführen. Im Fenster **Verwaltung** mit der rechten Maustaste **Windows PowerShell ISE**, klicken Sie auf **als Administrator ausführen**.
3.  **Klicken Sie in **Windows PowerShell ISE**** > **neu** , um ein neues Skript erstellen.
4.  Jetzt geben wir Ihnen ein einfaches Skript, das grundlegende PowerShell Befehle auf Azure Storage zeigt. Das Skript fragt zunächst Ihre Azure Konto Anmeldeinformationen hinzufügen Ihre Azure PowerShell-Umgebung. Dann wird das Skript den Standardsatz Azure-Abonnement und erstellen ein neues Speicherkonto in Azure. Anschließend das Skript in diesem neuen Speicherkonto einen neuen Container erstellen und Hochladen eine vorhandenen Bilddatei (Blob) in diesem Container. Nachdem das Skript alle Blobs im Container enthält, ein neues Verzeichnis auf Ihrem lokalen Computer erstellen und die Datei herunterladen.
5.  Wählen Sie das Skript zwischen den Hinweisen im Abschnitt folgenden Code **#beginnen** und **#end**. Drücken Sie STRG + C zum Kopieren in die Zwischenablage.

        #begin
        # Update with the name of your subscription.
        $SubscriptionName = "YourSubscriptionName"

        # Give a name to your new storage account. It must be lowercase!
        $StorageAccountName = "yourstorageaccountname"

        # Choose "West US" as an example.
        $Location = "West US"

        # Give a name to your new container.
        $ContainerName = "imagecontainer"

        # Have an image file and a source directory in your local computer.
        $ImageToUpload = "C:\Images\HelloWorld.png"

        # A destination directory in your local computer.
        $DestinationFolder = "C:\DownloadImages"

        # Add your Azure account to the local PowerShell environment.
        Add-AzureAccount

        # Set a default Azure subscription.
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

        # Create a new storage account.
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $Location

        # Set a default storage account.
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

        # Create a new container.
        New-AzureStorageContainer -Name $ContainerName -Permission Off

        # Upload a blob into a container.
        Set-AzureStorageBlobContent -Container $ContainerName -File $ImageToUpload

        # List all blobs in a container.
        Get-AzureStorageBlob -Container $ContainerName

        # Download blobs from the container:
        # Get a reference to a list of all blobs in a container.
        $blobs = Get-AzureStorageBlob -Container $ContainerName

        # Create the destination directory.
        New-Item -Path $DestinationFolder -ItemType Directory -Force  

        # Download blobs into the local destination directory.
        $blobs | Get-AzureStorageBlobContent –Destination $DestinationFolder
        #end

5.  **Windows PowerShell ISE**drücken Sie STRG + V, um das Skript zu kopieren. Klicken Sie auf **Datei** > **Speichern**. Geben Sie im Dialogfeld **Speichern unter** den Namen der Skriptdatei wie "Mystoragescript". Klicken Sie auf **Speichern**.

6.  Jetzt müssen Sie basierten auf Ihren Konfigurationen Skriptvariablen aktualisieren. Sie müssen die Variable **$SubscriptionName** mit eigenen Abonnement aktualisieren. Sie können der anderen Variablen im Skript angegeben oder wie gewünscht zu aktualisieren.

    - **$SubscriptionName:** Sie müssen diese Variable mit eigenen Namen aktualisieren. Führen Sie einen der folgenden drei Arten nach dem Namen Ihres Abonnements:

        ein. **Klicken Sie in **Windows PowerShell ISE**** > **neu** , um ein neues Skript erstellen. Kopieren Sie das folgende Skript in die neue Skriptdatei, und klicken Sie auf **Debuggen** > **Ausführen**. Das folgende Skript fragt zunächst Ihre Azure Anmeldeinformationen hinzufügen Ihre Azure PowerShell-Umgebung berücksichtigen und alle Abonnements, die an lokale PowerShell-Sitzung anzeigen. Notieren Sie sich den Namen des Abonnements, die Sie bei diesem Lernprogramm verwenden möchten:

            Add-AzureAccount
                Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName

        b. Klicken Sie zum Suchen und kopieren die Namen in der [Azure-Portal](https://portal.azure.com)im Hub-Menü auf der linken Seite auf **Abonnements**. Kopieren Sie den Namen des Abonnement, das Sie beim Ausführen der Skripts in diesem Handbuch.

        ![Azure-Portal][Image2]

        c. Die Namen der [Azure-Verwaltungsportal](https://manage.windowsazure.com/)kopieren, scrollen Sie nach unten und klicken Sie auf **Einstellungen** auf der linken Seite des Portals. Klicken Sie auf **Abonnements** , um eine Liste der Abonnements anzuzeigen. Kopieren Sie den Namen des Abonnement, das Sie beim Ausführen der Skripts erhalten in diesem Handbuch.

        ![Azure-Verwaltungsportal][Image1]

    - **$StorageAccountName:** Der Vorname in das Skript oder geben Sie einen neuen Namen für das Speicherkonto. **Wichtig:** Der Name des Speicherkontos muss in Azure eindeutig sein. Es muss auch Kleinbuchstaben sein!

    - **$Location:** Verwenden Sie bestimmte "West US" im Skript oder wählen Sie andere Speicherorte Azure wie ostasiatische US Nordeuropa usw.

    - **$ContainerName:** Vorname in das Skript oder geben Sie einen neuen Namen für den Container.

    - **$ImageToUpload:** Geben Sie einen Pfad zu einem Bild auf dem lokalen Computer wie: "C:\Images\HelloWorld.png".

    - **$DestinationFolder:** Geben Sie einen Pfad zu einem lokalen Verzeichnis aus dem Azure-Speicher wie heruntergeladene Dateien speichern: "C:\DownloadImages".

7.  Nach der Aktualisierung Skriptvariablen in der Datei "mystoragescript.ps1", klicken Sie auf **Datei** > **Speichern**. Klicken Sie auf **Debuggen** > **Ausführen** oder drücken Sie **F5** , um das Skript auszuführen.  

Nach der Ausführung des Skripts müssen Sie einen lokalen Ordner, der die gedownloadete Datei enthält. Der folgende Screenshot zeigt eine Beispiel für die Ausgabe:

![Herunterladen von Blobs][Image3]


> [AZURE.NOTE] Bereich "Erste Schritte mit Azure-Speicher und PowerShell in 5 Minuten" bereitgestellt eine kurze Einführung zur Verwendung von Azure PowerShell mit Azure-Speicher. Ausführliche Informationen und Hinweise empfehlen wir Ihnen die folgenden Abschnitte.

## <a name="prerequisites-for-using-azure-powershell-with-azure-storage"></a>Voraussetzung für die Verwendung von Azure PowerShell mit Azure-Speicher
Sie benötigen ein Azure-Abonnement und Konto in diesem Handbuch angegebenen PowerShell-Cmdlets ausführen, wie oben beschrieben.

Azure PowerShell ist ein Modul, die Cmdlets zum Verwalten von Azure über Windows PowerShell bereitstellt. Informationen zum Installieren und Einrichten von Azure PowerShell Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md). Wir empfehlen, downloaden und installieren oder die neuesten Azure PowerShell-Modul aktualisieren, bevor Sie dieses Handbuch verwenden.

Sie können die Cmdlets in standard Windows PowerShell-Konsole oder der Windows PowerShell Integrated Scripting Environment (ISE) ausführen. Beispielsweise öffnen Sie **Windows PowerShell ISE**das Startmenü, geben Verwaltung und klicken Sie auf ausführen. Im Fenster Verwaltung mit der rechten Maustaste Windows PowerShell ISE, klicken Sie auf als Administrator ausführen.

## <a name="how-to-manage-storage-accounts-in-azure"></a>Speicherkonten in Azure verwalten

### <a name="how-to-set-a-default-azure-subscription"></a>Wie ein Azure-Abonnement festlegen
Azure Storage mit Azure PowerShell verwalten, müssen Sie Ihre Clientumgebung Azure über Azure Active Directory oder zertifikatbasierte Authentifizierung zu authentifizieren. Ausführliche Informationen [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Lernprogramm anzeigen Dieses Handbuch verwendet Azure Active Directory-Authentifizierung.

1.  Geben Sie den folgenden Befehl hinzufügen Ihre Azure in Windows PowerShell ISE PowerShell-Umgebung berücksichtigen:

    `Add-AzureAccount`

2.  Geben Sie im Fenster "Zeichen in zu Microsoft Azure" e-Mail-Adresse und Ihr Konto zugeordnete Kennwort. Azure authentifiziert und die Anmeldeinformationen speichert und schließt das Fenster.

3.  Führen Sie dann den folgenden Befehl zum Anzeigen der Azure-Konten in der PowerShell-Umgebung und stellen Sie sicher, dass Ihr Konto aufgelistet ist:

    `Get-AzureAccount`

4.  Dann führen Sie das folgende Cmdlet anzeigen alle Abonnements, die mit lokalen PowerShell-Sitzung verbunden und überprüfen Sie, ob Ihr Abonnement aufgeführt:

    `Get-AzureSubscription | Format-Table SubscriptionName, IsDefault, IsCurrent, CurrentStorageAccountName`

5.  Legen Sie einen Standardwert Azure-Abonnement führen Sie auswählen-AzureSubscription Cmdlets aus:

        $SubscriptionName = 'Your subscription Name'
        Select-AzureSubscription -SubscriptionName $SubscriptionName –Default

6.  Überprüfen Sie, ob das Standardabonnement durch das Cmdlet "Get-AzureSubscription" ausgeführt:

    `Get-AzureSubscription -Default`

7.  Um Azure Storage verfügbaren PowerShell-Cmdlets anzuzeigen, führen Sie Folgendes aus:

    `Get-Command -Module Azure -Noun *Storage*`

### <a name="how-to-create-a-new-azure-storage-account"></a>Ein neues Azure Storage-Konto erstellen
Um Azure-Speicher zu verwenden, benötigen Sie ein Speicherkonto. Sie können ein neues Azure Storage-Konto nach der Konfiguration des Computers zum Abonnement erstellen.

1.  Führen Sie das Cmdlet Get-AzureLocation zu allen verfügbaren Datencenter stellen:

    `Get-AzureLocation | Format-Table -Property Name, AvailableServices, StorageAccountTypes`

2.  Führen Sie neu AzureStorageAccount-Cmdlet zum Erstellen eines neuen Storage-Kontos. Das folgende Beispiel erstellt ein neues Speicherkonto im Datencenter "West US".

        $location = "West US"
        $StorageAccountName = "yourstorageaccount"
        New-AzureStorageAccount –StorageAccountName $StorageAccountName -Location $location

> [AZURE.IMPORTANT] Der Name Ihres Speicherkontos muss in Azure und Kleinbuchstaben sein. Namenskonventionen und Einschränkungen finden Sie unter [Über Azure-Speicherkonten](storage-create-storage-account.md) und [Naming und verweisen, Blobs und Metadaten](http://msdn.microsoft.com/library/azure/dd135715.aspx).

### <a name="how-to-set-a-default-azure-storage-account"></a>Wie ein Standardkonto Azure-Speicher
Sie können mehrere Speicherkonten in Ihrem Abonnement verfügen. Wählen sie, und als Standardkonto Speicher für alle Speicher-Befehle in der PowerShell-Sitzung festlegen. Dadurch werden die Azure PowerShell Speicher Befehle ausführen, ohne den Speicherkontext explizit.

1.  Um ein Standard-Speicherkonto für Ihr Abonnement festzulegen, führen Sie das Cmdlet "Set-AzureSubscription".

        $SubscriptionName = "Your subscription name"
        $StorageAccountName = "yourstorageaccount"  
        Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName

2.  Führen Sie das Cmdlet Get-AzureSubscription überprüfen, ob das Speicherkonto Standardkonto Abonnement zugeordnet ist. Dieser Befehl gibt die Eigenschaften für das aktuelle Abonnement einschließlich seiner aktuellen Speicherkonto.

        Get-AzureSubscription –Current

### <a name="how-to-list-all-azure-storage-accounts-in-a-subscription"></a>Wie alle Azure-Speicher in einem Abonnement Firmenliste
Jede Azure-Abonnement kann bis zu 100 Speicherkonten verfügen. Aktuelle Informationen über Grenzen finden Sie unter [Azure-Abonnement und Service Grenzen, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).

Führen Sie das folgende Cmdlet zu Name und Status der Speicherkonten in das aktuelle Abonnement:

    Get-AzureStorageAccount | Format-Table -Property StorageAccountName, Location, AccountType, StorageAccountStatus

### <a name="how-to-create-an-azure-storage-context"></a>Einen Kontext Azure-Speicher erstellen
Kontext der Azure-Speicher ist ein Objekt in PowerShell Speicher Anmeldeinformationen kapseln. Verwenden einen Speicherkontext während nachfolgende Cmdlet ausführen, Ihre Anforderung zu authentifizieren, ohne das Speicherkonto und die Zugriffstaste explizit angeben kann. Erstellen einen Speicherkontext in vielerlei Hinsicht wie Name und speicherkontoschlüssel, gemeinsamen Zugriff Signaturtoken (SAS), Verbindungszeichenfolge oder anonym. Weitere Informationen finden Sie unter [New-AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx).  

Gehen Sie folgt einen Storage-Kontext erstellt:

- Führen Sie das Cmdlet [Get-AzureStorageKey](http://msdn.microsoft.com/library/azure/dn495235.aspx) Primärspeicher Tastenkombination für Ihr Konto Azure-Speicher zu. Als Nächstes rufen Sie [Neu AzureStorageContext](http://msdn.microsoft.com/library/azure/dn806380.aspx) Cmdlet einen Speicherkontext erstellen:

        $StorageAccountName = "yourstorageaccount"
        $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
        $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary


- Erstellen Sie ein Signaturtoken gemeinsamen Zugriff für einen Container Azure-Speicher und erstellen Sie einen Speicherkontext:

        $sasToken = New-AzureStorageContainerSASToken -Container abc -Permission rl
        $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -SasToken $sasToken

    Weitere Informationen finden Sie unter [Neue AzureStorageContainerSASToken](http://msdn.microsoft.com/library/azure/dn806416.aspx) und [Mithilfe von Shared Access Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).

- In einigen Fällen möchten Sie die Dienstendpunkte beim Erstellen eines neuen Speicherkontext angeben. Dies kann erforderlich sein, haben Sie einen benutzerdefinierten Domänennamen für das Speicherkonto BLOB-Dienst registriert oder Sie möchten eine SAS für den Zugriff auf Ressourcen. Die Dienstendpunkte in eine Verbindungszeichenfolge und erstellen Sie einen neue Speicherkontext wie folgt:

        $ConnectionString = "DefaultEndpointsProtocol=http;BlobEndpoint=<blobEndpoint>;QueueEndpoint=<QueueEndpoint>;TableEndpoint=<TableEndpoint>;AccountName=<AccountName>;AccountKey=<AccountKey>"
        $Ctx = New-AzureStorageContext -ConnectionString $ConnectionString

Weitere Informationen zum Konfigurieren einer Verbindungszeichenfolge Speicher finden Sie unter [Konfigurieren von Verbindungszeichenfolgen](storage-configure-connection-string.md).

Damit Ihren Computer einrichten und Verwalten von Abonnements und Speicherkonten mit Azure PowerShell gelernt werden Sie im nächsten Abschnitt erfahren Sie, wie Azure-Blobs verwalten und BLOB-Snapshots.

### <a name="how-to-retrieve-and-regenerate-azure-storage-keys"></a>Das Abrufen und Azure Speicherschlüssel Regenerieren

Ein Azure Storage-Konto enthält zwei kontoschlüssel. Das folgende Cmdlet können Sie Ihre Schlüssel.

    Get-AzureStorageKey -StorageAccountName "yourstorageaccount"

Verwenden Sie das folgende Cmdlet ein bestimmtes Schlüssels abrufen. Gültige Werte sind primäre und sekundäre.  

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Primary

    (Get-AzureStorageKey -StorageAccountName $StorageAccountName).Secondary

Möchten Sie Ihre Schlüssel neu erstellen, verwenden Sie das folgende Cmdlet. Gültige Werte für - KeyType sind "Primary" und "Sekundär"

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Primary”

    New-AzureStorageKey -StorageAccountName $StorageAccountName -KeyType “Secondary”

## <a name="how-to-manage-azure-blobs"></a>Verwalten von Azure-blobs
Azure BLOB-Speicher ist ein Dienst zum Speichern großer Mengen an unstrukturierten Daten wie Text oder binär, die überall in der Welt über HTTP oder HTTPS zugegriffen werden kann. In diesem Abschnitt wird davon ausgegangen, dass Sie bereits mit den Azure Blob Storage Service-Konzepten vertraut sind. Ausführliche Informationen finden Sie in [Erste Schritte mit BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md) und [BLOB-Konzepte](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="how-to-create-a-container"></a>Wie Sie einen Container erstellen
Jedes Blob in Azure-Speicher muss sich in einem Container. Sie können privaten Container mit dem New-AzureStorageContainer-Cmdlet erstellen:

    $StorageContainerName = "yourcontainername"
    New-AzureStorageContainer -Name $StorageContainerName -Permission Off

> [AZURE.NOTE] Es gibt drei anonymen Lesezugriff: **Off** **Blob**und **Container**. Anonymen Zugriff auf Blobs festgelegt Parameters Berechtigung **aus**. Standardmäßig der neue Container ist privat und kann nur vom Kontoinhaber zugegriffen werden. Um anonyme öffentliche Lesezugriff auf BLOB-Ressourcen, aber nicht auf Container Metadaten oder zur Liste der Blobs im Container Parameters Berechtigung auf **Blob**zu ermöglichen. Um öffentliche Lesezugriff auf BLOB-Ressourcen zu ermöglichen, legen Container Metadaten und die Liste der Blobs im Container Parameters Berechtigung **Container**. Weitere Informationen finden Sie unter [Verwalten anonymen Lesezugriff auf Container und Blobs](storage-manage-access-to-resources.md).

### <a name="how-to-upload-a-blob-into-a-container"></a>Einen Blob in einem Container uploaden
Block-Blobs und Seitenblobs unterstützt Azure BLOB-Speicher. Weitere Informationen finden Sie unter [Understanding Block Blobs, Anhängen BLobs und Seitenblobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Blobs in einem Container hochladen, können Sie das Cmdlet " [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) ". Standardmäßig lädt dieser Befehl die lokalen Dateien an ein. Verwenden Sie geben für den Blob - BlobType-Parameter.

Im folgenden Beispiel wird das Cmdlet " [Get-ChildItem](http://technet.microsoft.com/library/hh849800.aspx) " alle Dateien im angegebenen Ordner zu und übergibt sie an das nächste Cmdlet mit dem Pipelineoperator. Das Cmdlet " [Set-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806379.aspx) " uploads lokalen Dateien in Ihrem Container:

    Get-ChildItem –Path C:\Images\* | Set-AzureStorageBlobContent -Container "yourcontainername"

### <a name="how-to-download-blobs-from-a-container"></a>Herunterladen von Blobs aus einem container
Im folgenden Beispiel wird veranschaulicht, wie Container Blobs herunterladen. Im Beispiel wird zunächst eine Verbindung mit Azure-Speicher mit den Speicherkontext Konto, Speicher-Kontonamen und die primäre Taste. Das Beispiel ruft dann ein Blob Referenz mit dem Cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) . Anschließend wird das Cmdlet " [Get-AzureStorageBlobContent](http://msdn.microsoft.com/library/azure/dn806418.aspx) " Blobs in lokalen Ordner herunterladen.

    #Define the variables.
    $ContainerName = "yourcontainername"
    $DestinationFolder = "C:\DownloadImages"

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #List all blobs in a container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Download blobs from a container.
    New-Item -Path $DestinationFolder -ItemType Directory -Force
    $blobs | Get-AzureStorageBlobContent -Destination $DestinationFolder -Context $Ctx

### <a name="how-to-copy-blobs-from-one-storage-container-to-another"></a>Blobs aus einem Speicher in einen anderen kopieren
Sie können Blobs asynchron Speicherkonten und Bereiche kopieren. Im folgenden Beispiel wird veranschaulicht, wie Blobs aus einem Behälter in zwei verschiedenen Konten kopieren. Das Beispiel legt Variablen für Quell- und Speicherkonten und Speicherkontext für jedes Konto erstellt. Anschließend kopiert Beispiel Blobs aus dem Quellcontainer in den Zielcontainer mithilfe des [Start-AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) -Cmdlet. Es wird angenommen, dass die Quell- und Speicherkonten und Container vorhanden.

    #Define the source storage account and context.
    $SourceStorageAccountName = "yoursourcestorageaccount"
    $SourceStorageAccountKey = "Storage key for yoursourcestorageaccount"
    $SrcContainerName = "yoursrccontainername"
    $SourceContext = New-AzureStorageContext -StorageAccountName $SourceStorageAccountName -StorageAccountKey $SourceStorageAccountKey

    #Define the destination storage account and context.
    $DestStorageAccountName = "yourdeststorageaccount"
    $DestStorageAccountKey = "Storage key for yourdeststorageaccount"
    $DestContainerName = "destcontainername"
    $DestContext = New-AzureStorageContext -StorageAccountName $DestStorageAccountName -StorageAccountKey $DestStorageAccountKey

    #Get a reference to blobs in the source container.
    $blobs = Get-AzureStorageBlob -Container $SrcContainerName -Context $SourceContext

    #Copy blobs from one container to another.
    $blobs| Start-AzureStorageBlobCopy -DestContainer $DestContainerName -DestContext $DestContext

Beachten Sie, dass in diesem Beispiel wird eine asynchrone Kopie führt. Das Cmdlet [Get-AzureStorageBlobCopyState](http://msdn.microsoft.com/library/azure/dn806406.aspx) ausführen, um den Status jeder Kopie zu überwachen.

### <a name="how-to-copy-blobs-from-a-secondary-location"></a>Kopieren von Blobs von einem zweiten Standort
Sie können Blobs zwischen dem sekundären RA-GRS-aktiviertes Konto.

    #define secondary storage context using a connection string constructed from secondary endpoints.
    $SrcContext = New-AzureStorageContext -ConnectionString "DefaultEndpointsProtocol=https;AccountName=***;AccountKey=***;BlobEndpoint=http://***-secondary.blob.core.windows.net;FileEndpoint=http://***-secondary.file.core.windows.net;QueueEndpoint=http://***-secondary.queue.core.windows.net; TableEndpoint=http://***-secondary.table.core.windows.net;"
    Start-AzureStorageBlobCopy –Container *** -Blob *** -Context $SrcContext –DestContainer *** -DestBlob *** -DestContext $DestContext

### <a name="how-to-delete-a-blob"></a>Einen Blob löschen
Zum Löschen eines BLOBs zunächst eine Blob Referenz und rufen Sie das Cmdlet "Remove-AzureStorageBlob" auf. Das folgende Beispiel löscht alle Blobs in einem bestimmten Container. Im Beispiel legt Variablen für ein Speicherkonto und Storage-Kontext erstellt. Anschließend wird ein Blob Referenz mit dem Cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) abgerufen und [Remove-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806399.aspx) -Cmdlet entfernen Blobs aus einem Container in Azure-Speicher wird.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "containername"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to all the blobs in the container.
    $blobs = Get-AzureStorageBlob -Container $ContainerName -Context $Ctx

    #Delete blobs in a specified container.
    $blobs| Remove-AzureStorageBlob

## <a name="how-to-manage-azure-blob-snapshots"></a>Verwalten von Azure BLOB-snapshots
Azure können Sie einen Snapshot eines Blob erstellen. Ein Snapshot ist eine schreibgeschützte Version eines Blob zu einem Zeitpunkt ausgeführt wird. Nachdem ein Snapshot erstellt, kann es gelesen, kopiert, oder gelöscht, aber nicht geändert. Snapshots bieten eine Möglichkeit, um ein Blob zu einem Zeitpunkt angezeigt wird. Weitere Informationen finden Sie unter [Erstellen einer Momentaufnahme eines Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

### <a name="how-to-create-a-blob-snapshot"></a>Einen BLOB-Snapshot erstellen
Erstellen Sie einen Snapshot eines Blob zuerst eine Blob Referenz und rufen Sie dann die `ICloudBlob.CreateSnapshot` -Methode auf. Im folgende Beispiel legt Variablen für ein Speicherkonto und Storage-Kontext erstellt. Anschließend wird im Beispiel ein Blob Referenz mit dem Cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) abgerufen und führt die [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) -Methode, um einen Snapshot zu erstellen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $ContainerName = "yourcontainername"
    $BlobName = "yourblobname"
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $ContainerName -Blob $BlobName

    #Create a snapshot of the blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

### <a name="how-to-list-a-blobs-snapshots"></a>Wie ein Blob-snapshots
Sie können für ein Blob beliebig viele Schnappschüsse erstellen. Das Blob zum Nachverfolgen Ihrer aktuellen Snapshots zugeordnete Snapshots können angezeigt werden. Im folgende Beispiel verwendet vordefinierten Blob und ruft das Cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) Snapshots, Blob auflisten.  

    #Define the blob name.
    $BlobName = "yourblobname"

    #List the snapshots of a blob.
    Get-AzureStorageBlob –Context $Ctx -Prefix $BlobName -Container $ContainerName  | Where-Object  { $_.ICloudBlob.IsSnapshot -and $_.Name -eq $BlobName }

### <a name="how-to-copy-a-snapshot-of-a-blob"></a>Eine Momentaufnahme eines Blob kopieren
Sie können einen Snapshot eines Blob den Snapshot wiederherstellen. Ausführliche Informationen und Einschränkungen finden Sie unter [Erstellen einer Momentaufnahme eines Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx). Im folgende Beispiel legt Variablen für ein Speicherkonto und Storage-Kontext erstellt. Anschließend definiert das Beispiel die Container und BLOB-Variablen. Im Beispiel wird ein Blob Referenz mit dem Cmdlet [Get-AzureStorageBlob](http://msdn.microsoft.com/library/azure/dn806392.aspx) und führt die [ICloudBlob.CreateSnapshot](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.icloudblob.aspx) -Methode, um einen Snapshot zu erstellen. Dann führt das Beispiel das [Start AzureStorageBlobCopy](http://msdn.microsoft.com/library/azure/dn806394.aspx) Cmdlet den Snapshot mit dem ICloudBlob-Objekt für das Quell-Blob Blob kopiert. Werden Sie die Variablen auf Grundlage der Konfiguration vor Ausführen des Beispiels zu aktualisieren. Beachten Sie, dass angenommen, dass die Quell- und Zielcontainer und BLOB-Quelle vorhanden.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Define the variables.
    $SrcContainerName = "yoursourcecontainername"
    $DestContainerName = "yourdestcontainername"
    $SrcBlobName = "yourblobname"
    $DestBlobName = "CopyBlobName"

    #Get a reference to a blob.
    $blob = Get-AzureStorageBlob -Context $Ctx -Container $SrcContainerName -Blob $SrcBlobName

    #Create a snapshot of a blob.
    $snap = $blob.ICloudBlob.CreateSnapshot()

    #Copy the snapshot to another container.
    Start-AzureStorageBlobCopy –Context $Ctx -ICloudBlob $snap -DestBlob $DestBlobName -DestContainer $DestContainerName

Sie verwalten Azure-Blobs und BLOB-Snapshots mit Azure PowerShell bearbeitet haben, fahren Sie im nächsten Abschnitt erfahren, wie Tabellen, Warteschlangen und Dateien verwalten.

## <a name="how-to-manage-azure-tables-and-table-entities"></a>Azure-Tabellen und Tabellenentitäten verwalten
Azure Table Storage Service ist ein NoSQL-Datenspeicher zum Speichern und Abfragen große Datenmengen strukturierte, nicht relationale verwenden können. Die wichtigsten Komponenten des Dienstes sind Tabellen, Entitäten und Eigenschaften. Eine Tabelle ist eine Auflistung von Entitäten. Eine Entität ist eine Gruppe von Eigenschaften. Jede Entität kann bis zu 252 Eigenschaften haben alle Name-Wert-Paare sind. In diesem Abschnitt wird davon ausgegangen, dass Sie bereits mit den Azure Table Storage Service-Konzepten vertraut sind. Ausführliche Informationen finden Sie in der [Tabelle Service-Datenmodell verstehen](http://msdn.microsoft.com/library/azure/dd179338.aspx) und [mit Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md).

In den folgenden Unterabschnitten erfahren Sie, wie Azure Storage-Diensts mithilfe von Azure PowerShell verwaltet. Die fallen Szenarien **Erstellen**, **Löschen**und **Abrufen von** **Tabellen**sowie **Hinzufügen von** **Abfragen**und **Tabellenentitäten löschen**.

### <a name="how-to-create-a-table"></a>Erstellen eine Tabelle
Jede Tabelle muss ein Konto Azure-Speicher befinden. Das folgende Beispiel veranschaulicht das Erstellen in Azure-Speicher. Im Beispiel wird zunächst eine Verbindung mit Azure-Speicher mit den Speicherkontext Konto, Speicher-Kontonamen und die Zugriffstaste. Anschließend wird das [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) -Cmdlet zum Erstellen einer Tabelle in Azure-Speicher verwendet.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = "Storage key for yourstorageaccount ends with =="
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey

    #Create a new table.
    $tabName = "yourtablename"
    New-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-retrieve-a-table"></a>Wie Abrufen eine Tabelle
Sie können Abfragen und eine oder alle Tabellen in ein Speicherkonto. Im folgenden Beispiel wird veranschaulicht, wie eine Tabelle mit dem Cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) abrufen.

    #Retrieve a table.
    $tabName = "yourtablename"
    Get-AzureStorageTable –Name $tabName –Context $Ctx

Wenn Sie das Cmdlet "Get-AzureStorageTable" ohne Parameter aufrufen, wird alle Tabellen für ein Speicherkonto.

### <a name="how-to-delete-a-table"></a>Löschen eine Tabelle
Mit dem [Remove-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806393.aspx) -Cmdlet können Sie eine Tabelle aus ein Speicherkonto löschen.  

    #Delete a table.
    $tabName = "yourtablename"
    Remove-AzureStorageTable –Name $tabName –Context $Ctx

### <a name="how-to-manage-table-entities"></a>Tabellenentitäten verwalten
Derzeit bietet Azure PowerShell-Cmdlets zur Tabellenentitäten direkt verwalten nicht. Zum Ausführen von Vorgängen in Tabellenentitäten können Sie die Klassen in der [Azure-Speicher-Clientbibliothek für .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)erhalten.

#### <a name="how-to-add-table-entities"></a>Tabellenentitäten hinzufügen
Um eine Entität zu einer Tabelle hinzuzufügen, erstellen Sie zuerst ein Objekt, das die Eigenschaften der Entität definiert. Eine Entität kann bis zu 255 Eigenschaften einschließlich 3 Systemeigenschaften aufweisen: **PartitionKey**, **RowKey**und **Timestamp**. Sie sind verantwortlich für das Einfügen und aktualisieren die Werte **PartitionKey** und **RowKey**. Der Server verwaltet den Wert der **Timestamp**, der geändert werden kann. **PartitionKey** und **RowKey** identifizieren zusammen eindeutig jede Entität in einer Tabelle.

-   **PartitionKey**: bestimmt die Partition, die die Entität gespeichert ist.
-   **RowKey**: die Einheit innerhalb der Partition eindeutig identifiziert.

Sie können bis zu 252 benutzerdefinierte Eigenschaften für eine Entität definieren. Weitere Informationen finden Sie unter [Understanding Tabelle Service-Datenmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Im folgenden Beispiel wird veranschaulicht, wie Entitäten zu einer Tabelle hinzufügen. Veranschaulicht, wie die Mitarbeitertabelle abrufen und verschiedene Personen fügen. Zunächst wird eine Verbindung mit Azure-Speicher mit den Speicherkontext Konto, Speicher-Kontonamen und die Zugriffstaste. Danach wird die Tabelle mithilfe des Cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) abgerufen. Wenn die Tabelle nicht vorhanden ist, wird [New-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806417.aspx) -Cmdlet erstellen in Azure-Speicher. Als Nächstes definiert das Beispiel eine benutzerdefinierte Funktion hinzufügen-Entität, Entitäten in der Tabelle hinzufügen, indem jede Entität Partition und Zeilenschlüssel. Entität hinzufügen-Funktion ruft das Cmdlet " [New-Object](http://technet.microsoft.com/library/hh849885.aspx) " der [Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.dynamictableentity.aspx) -Klasse auf ein Entitätsobjekt erstellen. Später wird im Beispiel die [Microsoft.WindowsAzure.Storage.Table.TableOperation.Insert](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.insert.aspx) -Methode für dieses Entitätsobjekt zu einer Tabelle hinzufügen.

    #Function Add-Entity: Adds an employee entity to a table.
    function Add-Entity() {
        [CmdletBinding()]
        param(
           $table,
           [String]$partitionKey,
           [String]$rowKey,
           [String]$name,
           [Int]$id
        )

      $entity = New-Object -TypeName Microsoft.WindowsAzure.Storage.Table.DynamicTableEntity -ArgumentList $partitionKey, $rowKey
      $entity.Properties.Add("Name", $name)
      $entity.Properties.Add("ID", $id)

      $result = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Insert($entity))
    }

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Retrieve the table if it already exists.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx -ErrorAction Ignore

    #Create a new table if it does not exist.
    if ($table -eq $null)
    {
       $table = New-AzureStorageTable –Name $TableName -Context $Ctx
    }

    #Add multiple entities to a table.
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row1 -Name Chris -Id 1
    Add-Entity -Table $table -PartitionKey Partition1 -RowKey Row2 -Name Jessie -Id 2
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row1 -Name Christine -Id 3
    Add-Entity -Table $table -PartitionKey Partition2 -RowKey Row2 -Name Steven -Id 4

#### <a name="how-to-query-table-entities"></a>Tabellenentitäten Abfragen
Verwenden Sie zum Abfragen einer Tabelle die [Microsoft.WindowsAzure.Storage.Table.TableQuery](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tablequery.aspx) -Klasse. Angenommen, dass Sie das Skript in der angegebenen bereits ausgeführt haben Entitäten Abschnitt hinzufügen. Im Beispiel wird zunächst eine Verbindung mit Azure-Speicher mit den Speicherkontext Storage-Kontonamen und die Zugriffstaste. Anschließend versucht die zuvor erstellte Tabelle "Employees" mit dem Cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) abrufen. Das Cmdlet " [New-Object](http://technet.microsoft.com/library/hh849885.aspx) " in der Microsoft.WindowsAzure.Storage.Table.TableQuery-Klasse aufrufen erstellt ein neues Abfrageobjekt. Das Beispiel sucht die Entitäten, die eine Spalte "ID", deren Wert 1 in einen. Ausführliche Informationen finden Sie unter [Abfragen von Tabellen und Entitäten](http://msdn.microsoft.com/library/azure/dd894031.aspx). Beim Ausführen dieser Abfrage gibt alle Entitäten, die den Filterkriterien entsprechen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $TableName = "Employees"

    #Get a reference to a table.
    $table = Get-AzureStorageTable –Name $TableName -Context $Ctx

    #Create a table query.
    $query = New-Object Microsoft.WindowsAzure.Storage.Table.TableQuery

    #Define columns to select.
    $list = New-Object System.Collections.Generic.List[string]
    $list.Add("RowKey")
    $list.Add("ID")
    $list.Add("Name")

    #Set query details.
    $query.FilterString = "ID gt 0"
    $query.SelectColumns = $list
    $query.TakeCount = 20

    #Execute the query.
    $entities = $table.CloudTable.ExecuteQuery($query)

    #Display entity properties with the table format.
    $entities  | Format-Table PartitionKey, RowKey, @{ Label = "Name"; Expression={$_.Properties["Name"].StringValue}}, @{ Label = "ID"; Expression={$_.Properties["ID"].Int32Value}} -AutoSize

#### <a name="how-to-delete-table-entities"></a>Tabellenentitäten löschen
Sie können eine Entität mit der Partition und Zeile löschen. Angenommen, dass Sie das Skript in der angegebenen bereits ausgeführt haben Entitäten Abschnitt hinzufügen. Im Beispiel wird zunächst eine Verbindung mit Azure-Speicher mit den Speicherkontext Storage-Kontonamen und die Zugriffstaste. Anschließend versucht die zuvor erstellte Tabelle "Employees" mit dem Cmdlet [Get-AzureStorageTable](http://msdn.microsoft.com/library/azure/dn806411.aspx) abrufen. Wenn die Tabelle vorhanden ist, wird im Beispiel die [Microsoft.WindowsAzure.Storage.Table.TableOperation.Retrieve](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.retrieve.aspx) -Methode, um eine Entität anhand der Partition und Zeile Werte abzurufen. Übergeben Sie die Einheit der [Microsoft.WindowsAzure.Storage.Table.TableOperation.Delete](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.table.tableoperation.delete.aspx) -Methode löschen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the table.
    $TableName = "Employees"
    $table = Get-AzureStorageTable -Name $TableName -Context $Ctx -ErrorAction Ignore

    #If the table exists, start deleting its entities.
    if ($table -ne $null) {
       #Together the PartitionKey and RowKey uniquely identify every  
       #entity within a table.
       $tableResult = $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Retrieve("Partition2", "Row1"))
       $entity = $tableResult.Result
    if ($entity -ne $null)
    {
       #Delete the entity.
       $table.CloudTable.Execute([Microsoft.WindowsAzure.Storage.Table.TableOperation]::Delete($entity))
    }
    }

## <a name="how-to-manage-azure-queues-and-queue-messages"></a>Verwalten von Azure Warteschlangen und Nachrichten in Warteschlange
Azure Queue Storage ist ein Dienst zum Speichern von großen Anzahl von Nachrichten, die von überall auf der Welt über authentifizierte Aufrufe mit HTTP oder HTTPS zugegriffen werden können. In diesem Abschnitt wird davon ausgegangen, dass Sie bereits mit den Azure-Speicher Warteschlangendienst Konzepten vertraut sind. Ausführliche Informationen finden Sie unter [Erste Schritte mit Azure Queue Storage mit .NET](storage-dotnet-how-to-use-queues.md).

In diesem Abschnitt zeigen Sie Azure Storage-Warteschlangendienst mithilfe von Azure PowerShell verwalten. Die Szenarios umfassen **Einfügen** und Warteschlangennachrichten **Löschen** sowie **Erstellen**, **Löschen**und **Abrufen von Warteschlangen**.

### <a name="how-to-create-a-queue"></a>Erstellen eine Warteschlange
Im folgende Beispiel wird zuerst eine Verbindung mit Azure-Speicher mit den Speicherkontext Konto, Speicher-Kontonamen und die Zugriffstaste. Anschließend ruft [Neu AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806382.aspx) -Cmdlet eine Warteschlange mit dem Namen "Queuename" erstellen.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary
    $QueueName = "queuename"
    $Queue = New-AzureStorageQueue –Name $QueueName -Context $Ctx

Informationen zu Namenskonventionen für Azure-Warteschlange finden Sie unter [Benennen von Warteschlangen und Metadaten](http://msdn.microsoft.com/library/azure/dd179349.aspx).

### <a name="how-to-retrieve-a-queue"></a>Eine Warteschlange abrufen
Sie können Abfragen und Abrufen einer bestimmten Warteschlange oder eine Liste aller Warteschlangen ein Speicherkonto. Im folgenden Beispiel wird veranschaulicht, wie eine angegebene Warteschlange mit dem Cmdlet [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) abrufen.

    #Retrieve a queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue –Name $QueueName –Context $Ctx

Wenn Sie das Cmdlet " [Get-AzureStorageQueue](http://msdn.microsoft.com/library/azure/dn806377.aspx) " ohne Parameter aufrufen, wird eine Liste aller Warteschlangen.

### <a name="how-to-delete-a-queue"></a>Löschen eine Warteschlange
Rufen Sie zum Löschen einer Warteschlange und alle darin enthaltenen Nachrichten entfernen-AzureStorageQueue-Cmdlet. Im folgenden Beispiel wird veranschaulicht, wie eine angegebene Warteschlange mithilfe des Cmdlets entfernen AzureStorageQueue löschen.

    #Delete a queue.
    $QueueName = "yourqueuename"
    Remove-AzureStorageQueue –Name $QueueName –Context $Ctx

#### <a name="how-to-insert-a-message-into-a-queue"></a>Eine Nachricht in einer Warteschlange einfügen
Zum Einfügen einer Nachricht in eine vorhandene Warteschlange erstellen Sie zunächst eine neue Instanz der [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) -Klasse. Anschließend rufen Sie die [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -Methode. Eine CloudQueueMessage kann aus einer Zeichenfolge (im UTF-8-Format) oder ein Byte-Array erstellt werden.

Im folgenden Beispiel wird veranschaulicht, wie Nachrichten einer Warteschlange hinzufügen. Im Beispiel wird zunächst eine Verbindung mit Azure-Speicher mit den Speicherkontext Konto, Speicher-Kontonamen und die Zugriffstaste. Anschließend ruft die angegebene Warteschlange mit dem Cmdlet [Get-AzureStorageQueue](https://msdn.microsoft.com/library/azure/dn806377.aspx) ab. Wenn die Warteschlange vorhanden ist, wird das Cmdlet " [New-Object](http://technet.microsoft.com/library/hh849885.aspx) " eine Instanz der [Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage](http://msdn.microsoft.com/library/azure/jj732474.aspx) -Klasse erstellen. Später wird im Beispiel die [AddMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.addmessage.aspx) -Methode für dieses Nachrichtenobjekt einer Warteschlange hinzu. Hier steht die Warteschlange abruft und fügt die Nachricht 'MessageInfo':

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    #If the queue exists, add a new message.
    if ($Queue -ne $null) {
       # Create a new message using a constructor of the CloudQueueMessage class.
       $QueueMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList MessageInfo

       #Add a new message to the queue.
       $Queue.CloudQueue.AddMessage($QueueMessage)
    }


#### <a name="how-to-de-queue-at-the-next-message"></a>Wie bei der nächsten Meldung Warteschlange
Code Warteschlange eine Nachricht aus einer Warteschlange in zwei Schritten. Beim Aufrufen der Methode [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.GetMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.getmessage.aspx) erhalten Sie die nächste Nachricht in einer Warteschlange. Eine Nachricht von **GetMessage** unsichtbar Lesen von Nachrichten aus dieser Warteschlange Code. Abschließend die Nachricht aus der Warteschlange entfernt, müssen Sie auch die [Microsoft.WindowsAzure.Storage.Queue.CloudQueue.DeleteMessage](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.deletemessage.aspx) -Methode aufrufen. Diesen Schritten entfernen einer Nachricht wird sichergestellt, dass Code nicht verarbeiten einer Nachricht durch Hardware-oder Softwarefehler, eine andere Instanz des Codes dieselbe Nachricht und versuchen Sie es erneut. Der Code ruft **DeleteMessage** rechts nach dem Verarbeiten der Meldung.

    #Define the storage account and context.
    $StorageAccountName = "yourstorageaccount"
    $StorageAccountKey = Get-AzureStorageKey -StorageAccountName $StorageAccountName
    $Ctx = New-AzureStorageContext –StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey.Primary

    #Retrieve the queue.
    $QueueName = "queuename"
    $Queue = Get-AzureStorageQueue -Name $QueueName -Context $ctx

    $InvisibleTimeout = [System.TimeSpan]::FromSeconds(10)

    #Get the message object from the queue.
    $QueueMessage = $Queue.CloudQueue.GetMessage($InvisibleTimeout)
    #Delete the message.
    $Queue.CloudQueue.DeleteMessage($QueueMessage)

## <a name="how-to-manage-azure-file-shares-and-files"></a>Verwalten von Azure Freigaben und Dateien
Azure Dateispeicher bietet freigegebenen Speicher für Applikationen mit SMB-Standardprotokoll. Microsoft Azure virtuelle Computer und Clouddienste Daten über Anwendungskomponenten über bereitgestellte Freigaben freigeben und lokale Programme können Daten in einer Freigabe über Datei API und Azure PowerShell zugreifen.

Informationen für Azure Dateispeicher finden Sie unter [Erste Schritte mit Windows Azure Dateispeicher](storage-dotnet-how-to-use-files.md) und [Datei Service REST-API](http://msdn.microsoft.com/library/azure/dn167006.aspx).

## <a name="how-to-set-and-query-storage-analytics"></a>Wie Abfragen Speicheranalyse
[Azure Storage Analytics](storage-analytics.md) können Sie Metriken für Ihren Azure-Speicherkonten und Protokolldaten zu Anfragen an das Speicherkonto sammeln. Speichermetriken können Sie ein Speicherkonto und Speicher Protokollierung zur diagnose und Problembehandlung für das Speicherkonto überprüfen.
Standardmäßig ist speichermetriken für Storage Services nicht aktiviert. Sie können mithilfe der Azure-Portal oder Windows PowerShell oder programmgesteuert über die Speicher-Clientbibliothek überwachen. Speicherprotokollierung serverseitige geschieht und Datensatzdetails für erfolgreiche und fehlgeschlagene Anfragen in das Speicherkonto ermöglicht. Diese Protokolle können Sie Details zu lesen, schreiben und löschen Tabellen, Warteschlangen und Blobs sowie die Gründe für fehlgeschlagene Anfragen.

Aktivieren und Speichermetriken Daten mithilfe von PowerShell finden Sie unter [Speichermetriken mit PowerShell aktivieren](http://msdn.microsoft.com/library/azure/dn782843.aspx#HowtoenableStorageMetricsusingPowerShell).

Aktivieren und Protokollierung Speicher Daten mit PowerShell abrufen finden Sie unter [Speicher mit PowerShell Protokollierung aktivieren](http://msdn.microsoft.com/library/azure/dn782840.aspx#HowtoenableStorageLoggingusingPowerShell) und [Ihre Protokolldaten Speicher Protokollierung finden](http://msdn.microsoft.com/library/azure/dn782840.aspx#FindingyourStorageLogginglogdata).
Detaillierte Informationen zum Einsatz von Storage Metriken und Speicher Protokollierung Storage-Problemen finden Sie unter [Überwachung und Diagnose, Problembehandlung bei Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="how-to-manage-shared-access-signature-sas-and-stored-access-policy"></a>Shared Access Signatur (SAS) und gespeicherte Richtlinien verwalten
SAS sind Bestandteil das Sicherheitsmodell für jede Anwendung Azure-Speicher. Sie können eingeschränkte Berechtigungen für das Speicherkonto für Clients, die keinen kontoschlüssel. Standardmäßig kann nur der Besitzer des Speicherkontos Blobs und Tabellen innerhalb dieses Konto zugreifen. Wenn der Dienst oder die Anwendung zu dieser Ressourcen an andere Clients ohne den Zugriffsschlüssel muss, haben Sie drei Optionen:

- Legen Sie einen Container Berechtigungen ermöglicht anonymen Lesezugriff auf den Container und die Blobs. Dies ist für Tabellen oder Warteschlangen nicht zulässig.
- Verwenden Sie SAS erteilt Zugriffsrechte auf Container, Blobs, Queues und Tabellen für einen bestimmten Zeitraum beschränkt.
- Verwenden einer gespeicherten Richtlinie eine zusätzliche Kontrolle über SAS für einen Container oder die Blobs, eine Warteschlange oder eine Tabelle zu erhalten. Die gespeicherte Richtlinie können Sie die Startzeit Ablaufzeit oder Berechtigungen für eine Signatur oder um danach widerrufen wurde ausgestellt.

Eine SAS kann in zwei Formen:

- **Ad-hoc-SAS**: beim Erstellen einer ad-hoc-SAS, die Startzeit, die Ablaufzeit und Berechtigungen für die SAS werden auf dem SAS-URI angegeben. Diese Art von SAS kann Container, Blob, Tabelle erstellt oder Warteschlange ist nicht Ihnen.
- **SAS mit gespeicherten Zugriffsrichtlinie**: gespeicherte Zugriffsrichtlinie für Ressourcencontainer BLOB-Container, Tabellen oder Queue - und Sie können Integritätsregeln für eine oder mehrere SAS verwalten. Wenn Sie eine gespeicherte Zugriffsrichtlinie SAS zuordnen, erbt die SAS Constraints - Startzeit, Ablaufzeit und -für die gespeicherte Richtlinie definierten Berechtigungen. Dieser Typ von SAS ist Ihnen.

Weitere Informationen finden Sie unter [Verwenden gemeinsame Zugriff Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md) und [anonymen Lesezugriff Blobs und Container verwalten](storage-manage-access-to-resources.md).

In den folgenden Abschnitten erfahren Sie, wie ein gemeinsamer Zugriff Signatur token und gespeicherte Azure Tabellen erstellen. Azure PowerShell bereit Container, Blobs und Warteschlangen sowie ähnliche Cmdlets. Führen Sie die Skripts in diesem Abschnitt download [Azure PowerShell Version 0.8.14](http://go.microsoft.com/?linkid=9811175&clcid=0x409) oder höher.

### <a name="how-to-create-a-policy-based-shared-access-signature-token"></a>Eine Policy-basierte SAS-Token erstellen
Verwenden Sie das Cmdlet New AzureStorageTableStoredAccessPolicy einen neuen gespeicherten erstellen. Rufen Sie [New-AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) -Cmdlet zum Erstellen eines neuen gemeinsamen Zugriff Policy-basierte Signatur Tokens für eine Tabelle Azure-Speicher.

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx
    New-AzureStorageTableSASToken -Name $tableName -Policy $policy -Context $Ctx

### <a name="how-to-create-an-ad-hoc-non-revokable-shared-access-signature-token"></a>Ein ad-hoc-(nicht Ihnen) SAS-Token erstellen
Verwenden Sie das Cmdlet [Neu AzureStorageTableSASToken](http://msdn.microsoft.com/library/azure/dn806400.aspx) ein neues ad-hoc-(nicht Ihnen) SAS-Token für eine Azure Storage-Tabelle zu erstellen:

    New-AzureStorageTableSASToken -Name $tableName -Permission "rqud" -StartTime "2015-01-01" -ExpiryTime "2015-02-01" -Context $Ctx

### <a name="how-to-create-a-stored-access-policy"></a>Zum Erstellen einer gespeicherten Zugriffsrichtlinie
Verwenden Sie das Cmdlet New AzureStorageTableStoredAccessPolicy erstellt eine neue gespeicherte Richtlinie für eine Tabelle Azure-Speicher:

    $policy = "policy1"
    New-AzureStorageTableStoredAccessPolicy -Name $tableName -Policy $policy -Permission "rd" -StartTime "2015-01-01" -ExpiryTime "2016-01-01" -Context $Ctx

### <a name="how-to-update-a-stored-access-policy"></a>Eine gespeicherte Richtlinie aktualisieren
Verwenden Sie das Cmdlet "Set-AzureStorageTableStoredAccessPolicy" eine vorhandene gespeicherte Zugriffsrichtlinie für eine Azure Storage-Tabelle aktualisieren:

    Set-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Permission "rd" -NoExpiryTime -NoStartTime -Context $Ctx

### <a name="how-to-delete-a-stored-access-policy"></a>Eine gespeicherte Richtlinie löschen
Verwenden Sie das Cmdlet entfernen AzureStorageTableStoredAccessPolicy gespeicherten Zugriffsrichtlinie auf der Azure-Speicher Tabelle löschen:

    Remove-AzureStorageTableStoredAccessPolicy -Policy $policy -Table $tableName -Context $Ctx


## <a name="how-to-use-azure-storage-for-us-government-and-azure-china"></a>Verwendung von Azure Storage für US-Regierung und Azure China
Eine Azure-Umgebung ist eine unabhängige Bereitstellung von Microsoft Azure [Azure Regierung für die US-Regierung](https://azure.microsoft.com/features/gov/) [AzureCloud für globale Azure](https://portal.azure.com)und [AzureChinaCloud für Azure von 21Vianet in China betrieben wird](http://www.windowsazure.cn/). Sie können neue Azure-Umgebung für die US-Regierung und Azure China bereitstellen.

Um Azure Storage mit AzureChinaCloud verwenden, müssen Sie einen Speicherkontext erstellen, der mit AzureChinaCloud verknüpft ist. Führen Sie diese Schritte, um Ihnen den Einstieg erleichtern:

1.  Führen Sie das Cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) um die verfügbaren Azure-Umgebung finden Sie unter:

    `Get-AzureEnvironment`

2.  Windows PowerShell ein Azure China Konto hinzufügen:

    `Add-AzureAccount –Environment AzureChinaCloud`

3.  Erstellen Sie einen Speicherkontext für ein AzureChinaCloud-Konto:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureChinaCloud

Für die Verwendung der Azure-Speicher mit [Azure Regierung](https://azure.microsoft.com/features/gov/)eine neue Umgebung definieren und erstellen Sie einen neuen Speicherkontext mit dieser Umgebung:

1.  Führen Sie das Cmdlet [Get-AzureEnvironment](https://msdn.microsoft.com/library/azure/dn790368.aspx) um die verfügbaren Azure-Umgebung finden Sie unter:

    `Get-AzureEnvironment`

2.  Windows PowerShell ein Azure Regierung Konto hinzufügen:

    `Add-AzureAccount –Environment AzureUSGovernment`

3.  Erstellen Sie einen Speicherkontext für ein AzureUSGovernment-Konto:

        $Ctx = New-AzureStorageContext -StorageAccountName $AccountName -StorageAccountKey $AccountKey> -Environment AzureUSGovernment

Weitere Informationen finden Sie unter:

- [Microsoft Azure Regierung Developer Guide](../azure-government-developer-guide.md).
- [Übersicht über die Unterschiede beim Erstellen einer Anwendung für China Service](https://msdn.microsoft.com/library/azure/dn578439.aspx)

## <a name="next-steps"></a>Nächste Schritte
In diesem Handbuch haben Sie gelernt, Azure Azure PowerShell managen. Hier sind einige Artikeln und Ressourcen mehr über sie erfahren.

- [Azure-Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/)
- [Azure-Speicher-PowerShell-Cmdlets](http://msdn.microsoft.com/library/azure/dn806401.aspx)
- [Windows PowerShell-Referenz](https://msdn.microsoft.com/library/ms714469.aspx)

[Image1]: ./media/storage-powershell-guide-full/Subscription_currentportal.png
[Image2]: ./media/storage-powershell-guide-full/Subscription_Previewportal.png
[Image3]: ./media/storage-powershell-guide-full/Blobdownload.png


[Getting started with Azure Storage and PowerShell in 5 minutes]: #getstart
[Prerequisites for using Azure PowerShell with Azure Storage]: #pre
[How to manage storage accounts in Azure]: #manageaccount
[How to set a default Azure subscription]: #setdefsub
[How to create a new Azure storage account]: #createaccount
[How to set a default Azure storage account]: #defaultaccount
[How to list all Azure storage accounts in a subscription]: #listaccounts
[How to create an Azure storage context]: #createctx
[How to manage Azure blobs and blob snapshots]: #manageblobs
[How to create a container]: #container
[How to upload a blob into a container]: #uploadblob
[How to download blobs from a container]: #downblob
[How to copy blobs from one storage container to another]: #copyblob
[How to delete a blob]: #deleteblob
[How to manage Azure blob snapshots]: #manageshots
[How to create a blob snapshot]: #createshot
[How to list snapshots of a blob]: #listshot
[How to copy a snapshot of a blob]: #copyshot
[How to manage Azure tables and table entities]: #managetables
[How to create a table]: #createtable
[How to retrieve a table]: #gettable
[How to delete a table]: #remtable
[How to manage table entities]: #mngentity
[How to add table entities]: #addentity
[How to query table entities]: #queryentity
[How to delete table entities]: #deleteentity
[How to manage Azure queues and queue messages]: #managequeues
[How to create a queue]: #createqueue
[How to retrieve a queue]: #getqueue
[How to delete a queue]: #remqueue
[How to manage queue messages]: #mngqueuemsg
[How to insert a message into a queue]: #addqueuemsg
[How to de-queue at the next message]: #dequeuemsg
[How to manage Azure file shares and files]: #files
[How to set and query storage analytics]: #stganalytics
[How to manage Shared Access Signature (SAS) and Stored Access Policy]: #sas
[How to use Azure Storage for U.S. government and Azure China]: #gov
[Next Steps]: #next
