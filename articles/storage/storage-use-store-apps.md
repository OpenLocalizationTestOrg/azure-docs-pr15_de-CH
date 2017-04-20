<properties
    pageTitle="Verwenden von Azure-Speicher in Windows Store-apps | Microsoft Azure"
    description="Erfahren Sie, wie Sie eine Windows Store-app erstellen, Azure Blob, Warteschlange, Tabelle oder Datei speichern."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Verwendung von Azure-Speicher in Windows Store-apps

## <a name="overview"></a>Übersicht

Dieses Handbuch veranschaulicht die Schritte beim Entwickeln von Windows Store-app, die Azure-Speicher verwendet.

## <a name="download-required-tools"></a>Erforderliche Tools herunterladen

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) erleichtert das Erstellen, Debuggen, lokalisieren, Paket und Bereitstellen von Windows Store-apps. Visual Studio 2012 oder höher ist erforderlich.
- [Azure Storage-Clientbibliothek](https://www.nuget.org/packages/WindowsAzure.Storage) bietet eine Windows-Runtime-Klassenbibliothek für die Arbeit mit Azure-Speicher.
- [WCF-Datendienste Tools für Windows Store-Apps](http://www.microsoft.com/download/details.aspx?id=30714) erweitert Dienstverweis hinzufügen Erfahrung mit clientseitigen OData-Unterstützung für Windows Store-apps in Visual Studio.

## <a name="develop-apps"></a>Entwickeln von apps

### <a name="getting-ready"></a>Vorbereitung

Erstellen Sie ein neues Windows Store-app-Projekt in Visual Studio 2012 oder höher:

![Store-apps-Speicher-Vs-Projekt][store-apps-storage-vs-project]

Fügen Sie einen Verweis auf die Clientbibliothek Azure Storage durch Rechtsklick **Verweise**, auf **Verweis hinzufügen**klicken und dann auf Speicher-Client-Bibliothek für Windows-Runtime, die Sie heruntergeladen haben:

![Store-apps-Speicher-wählen-Bibliothek][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Mithilfe der Bibliothek mit dem Blob und Warteschlangen

An diesem Punkt kann Ihre app Azure Blob und Warteschlange Dienste aufrufen. Fügen Sie die folgende Anweisung **verwenden** Azure Speichertypen direkt verwiesen werden können:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Fügen Sie eine Schaltfläche auf der Seite. Fügen Sie folgenden Code zum **Click** -Ereignis und ändern Sie die Ereignishandlermethode mit dem [Async-Schlüsselwort](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Dabei wird davon ausgegangen, dass Sie zwei Variablen, *Kontoname* und *AccountKey*. Sie stehen für den Namen Ihres Speicherkontos und kontoschlüssel, der mit diesem Konto verknüpft ist.

Erstellen Sie und führen Sie die Anwendung. Schaltfläche überprüft, ob ein Container namens *container1* in Ihrem Konto vorhanden ist und dann erstellen, wenn nicht.

### <a name="using-the-library-with-the-table-service"></a>Mithilfe der Bibliothek mit der Tabelle

Zur Kommunikation mit der Azure-Diensts hängen WCF-Datendienste für die Windows Store-app-Bibliothek. Fügen Sie einen Verweis auf die erforderlichen Bibliotheken WCF mithilfe der Paket-Manager-Konsole:

![Store-apps-Storage-Paket-manager][store-apps-storage-package-manager]

Verwenden Sie den folgenden Befehl zum Punkt Paket-Manager zu dem Speicherort auf Ihrem Computer:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Mit diesem Befehl werden automatisch alle erforderlichen Verweise zum Projekt hinzugefügt. Wenn Sie nicht die Konsole Paket-Manager verwenden möchten, können Liste Paketquellen WCF-Datendienste NuGet-Ordner auf dem lokalen Computer hinzufügen und fügen den Verweis über die Benutzeroberfläche Siehe [Verwalten von NuGet Pakete mithilfe des Dialogfelds](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Wenn Sie WCF-Datendienste NuGet-Paket verwiesen haben, ändern Sie den Code im **Click** -Ereignis der Schaltfläche:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Dieser Code überprüft, ob eine Tabelle namens *Tabelle1* in Ihrem Konto vorhanden und wird erstellt, wenn nicht.

Sie können auch hinzufügen auf Microsoft.WindowsAzure.Storage.Table.dll, das im gleichen Paket herunterladen verfügbar ist. Diese Bibliothek enthält zusätzliche Funktionen, wie reflektionsbasierte Serialisierung und allgemeiner Abfragen. Beachten Sie, dass diese Bibliothek JavaScript nicht unterstützt.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
