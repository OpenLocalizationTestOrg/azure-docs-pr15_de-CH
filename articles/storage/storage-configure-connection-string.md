<properties 
    pageTitle="Eine Verbindungszeichenfolge Azure-Speicher konfigurieren | Microsoft Azure"
    description="Konfigurieren Sie eine Verbindungszeichenfolge Azure Storage-Konto. Eine Verbindungszeichenfolge enthält die Informationen, die von der Anwendung zur Laufzeit Zugriff auf ein Speicherkonto Authentifizierung erforderlich."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Konfigurieren Sie Verbindungszeichenfolgen Azure-Speicher

## <a name="overview"></a>Übersicht

Eine Verbindungszeichenfolge enthält Authentifizierungsinformationen benötigt Ihre Anwendung Daten in Azure Storage-Konto zugreifen. Sie können eine Verbindungszeichenfolge zu konfigurieren:

- Verbinden Sie mit der Azure-Speicher-Emulator.
- Ein Speicherkonto in Azure zugreifen.
- Zugriff auf angegebene Ressourcen in Azure über SAS (SAS).

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Das Speichern der Verbindungszeichenfolge

Die Anwendung müssen die Verbindungszeichenfolge zur Laufzeit zur Authentifizierung an den Azure-Speicher zugreifen. Sie haben eine Reihe von Optionen für das Speichern der Verbindungszeichenfolge:

- Für eine ausgeführte Anwendung auf dem Desktop oder auf ein Gerät speichert die Verbindungszeichenfolge in einer `app.config `Datei oder `web.config` Datei. Der Abschnitt **AppSettings** die Verbindungszeichenfolge hinzufügen.
- Für eine Anwendung in Azure-Clouddienst ausgeführt können Sie die Verbindungszeichenfolge in [Azure Service Konfigurationsdatei Schema (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx)speichern. Abschnitt **ConfigurationSettings** Dienstkonfigurationsdatei Verbindungszeichenfolge hinzufügen.
- Sie können auch die Verbindungszeichenfolge direkt im Code verwenden. In den meisten Szenarios empfiehlt jedoch die Verbindungszeichenfolge in einer Konfigurationsdatei zu speichern.

Speichern der Verbindungszeichenfolge in der Konfigurationsdatei erleichtert die Verbindungszeichenfolge zum Wechseln zwischen Speicheremulator und Azure Speicherkonto in der Cloud. Sie müssen nur die Verbindungszeichenfolge auf der zielumgebung bearbeiten.

[Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) -Klasse können Sie Zugriff auf die Verbindungszeichenfolge zur Laufzeit unabhängig von dem die Anwendung ausgeführt wird.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Erstellen Sie eine Verbindungszeichenfolge für Speicheremulator

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Weitere Informationen über Speicheremulator finden Sie unter [verwenden Azure Speicheremulator für Entwicklung und Testing](storage-use-emulator.md) .

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Erstellen Sie eine Verbindungszeichenfolge für eine Azure Storage-Konto

Verwenden Sie zum Erstellen einer Verbindungszeichenfolge Azure Storage-Konto Format der Verbindungszeichenfolge unten. Angeben, ob Sie möchten das Speicherkonto über HTTPS (empfohlen) oder HTTP, ersetzen Sie `myAccountName` mit dem Namen Speicherkonto und Ersetzen `myAccountKey` mit Ihrem Konto zugreifen:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Beispielsweise wird die Verbindungszeichenfolge Verbindungszeichenfolge von Beispiel aussehen:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Azure-Speicher unterstützt HTTP und HTTPS in einer Verbindungszeichenfolge. mit HTTPS wird jedoch dringend empfohlen.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Erstellen Sie eine Verbindungszeichenfolge mit einem SAS

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Erstellen einer Verbindungszeichenfolge auf einen expliziten Speicher-Endpunkt

Sie können die Endpunkte in Ihrer Verbindungszeichenfolge anstelle der Standardendpunkte explizit angeben. Geben Sie eine Verbindungszeichenfolge erstellen, die einen expliziten Endpunkt gibt, vollständige Dienstendpunkt für jeden Dienst, einschließlich der Protokollspezifikation (HTTPS (empfohlen) oder HTTP) im folgenden Format:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Ist ein Szenario, in dem Sie einen expliziten Endpunkt angeben möchten, Sie eine benutzerdefinierte Domäne Ihren Endpunkt BLOB-Speicher zugeordnet. In diesem Fall können Sie Ihre benutzerdefinierten Endpunkt für BLOB-Speicher in der Verbindungszeichenfolge angeben und optional Standardendpunkte für den Dienst festlegen, wenn Ihre Anwendung.

Hier sind Beispiele für gültige Verbindungszeichenfolgen, die einen expliziten Endpunkt für den BLOB-Dienst angeben:

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

Endpunktwert, der in der Verbindungszeichenfolge aufgeführt zur Anforderung URIs BLOB-Dienst erstellen und bestimmt das Formular alle URIs, die dem Code zurückgegeben werden.

Beachten Sie, dass möchten Sie einen Endpunkt aus der Verbindungszeichenfolge weglassen, dann Sie diese Verbindungszeichenfolge verwenden, Zugriff auf Daten in diesem Dienst aus dem Code nicht.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Erstellen einer Verbindungszeichenfolge mit einem Suffix Endpunkt

Erstellen eine Verbindungszeichenfolge Speicherdienst in Regionen oder mit anderen Endpunkt Suffixe wie Azure China oder Azure Governance verwenden Sie das folgende Format der Verbindungszeichenfolge. Angeben, ob das Speicherkonto über HTTP oder HTTPS verbinden, ersetzen `myAccountName` durch den Namen Ihres Speicherkontos ersetzen `myAccountKey` mit Ihrem Konto Zugriffstaste ersetzen `mySuffix` mit dem URI:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


Die Verbindungszeichenfolge sollte z. B. die folgende Verbindungszeichenfolge ähnlich aussehen:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Analyse einer Verbindungszeichenfolge

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Nächste Schritte

- [Verwenden Sie den Azure-Speicheremulator für Entwicklung und Testing](storage-use-emulator.md)
- [Azure-Speicher-Explorers](storage-explorers.md)
- [Mithilfe SAS (SAS)](storage-dotnet-shared-access-signature-part-1.md)
