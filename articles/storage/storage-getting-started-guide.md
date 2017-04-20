<properties
    pageTitle="Erste Schritte mit Azure Storage in fünf Minuten | Microsoft Azure"
    description="Abheben Sie schnell auf Microsoft Azure-Blobs, Tabelle und Warteschlangen mit Azure Storage Quick Start, Visual Studio und Azure Speicheremulator. Führen Sie Ihre erste Azure-Speicher Anwendung in fünf Minuten."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="get-started-with-azure-storage-in-five-minutes"></a>Erste Schritte mit Azure Storage in fünf Minuten

## <a name="overview"></a>Übersicht

Es ist einfach, mit Azure-Speicher. In diesem Lernprogramm wird veranschaulicht, wie eine Azure Storage-Anwendung einrichten und schnell. Schnellstart mit die Vorlagen in Azure SDK für .NET verwenden. Diese Quick Start Ready to Run Code enthalten, der einige grundlegende Programmierungsszenarios mit Azure Storage veranschaulicht.

Erfahren Sie mehr über Azure-Speicher vor der Code anzeigen Sie [Weiter](#next-steps)

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, benötigen Sie die folgenden Komponenten:

1. Kompilieren und erstellen Sie die Anwendung benötigen Sie eine Version von [Visual Studio](https://www.visualstudio.com/) auf Ihrem Computer installiert.

2. Installieren Sie die neueste Version [Azure SDK für .NET](https://azure.microsoft.com/downloads/). Das SDK enthält die Beispielprojekte Azure Schnellstart Azure Speicheremulator und [Azure Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

3. Stellen Sie sicher, dass [.NET Framework 4.5](http://www.microsoft.com/download/details.aspx?id=30653) auf Ihrem Computer installiert haben, wie sie die Beispielprojekte Azure Schnellstart erforderlich ist, die wir in diesem Lernprogramm verwenden.

    Wenn Sie nicht sicher sind, welche Version von.NET Framework auf Ihrem Computer installiert ist, finden Sie unter [wie: bestimmt die.NET Framework-Versionen installiert](https://msdn.microsoft.com/vstudio/hh925568.aspx). Oder drücken Sie die Schaltfläche **Start** oder die Windows-Taste Geben Sie **Control Panel**. Klicken Sie auf **Programme** > **Programme und Features**, und ob.NET Framework 4.5 unter der installierten Programme aufgeführt ist.

4. Sie benötigen ein Azure-Abonnement sowie ein Azure-Speicher.

    - Ein Azure-Abonnement finden Sie [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) [Kaufoptionen](https://azure.microsoft.com/pricing/purchase-options/)und [Bietet Member](https://azure.microsoft.com/pricing/member-offers/) (für Mitglieder von MSDN, Microsoft Partner Network und BizSpark und andere Microsoft-Programme).
    - Erstellen ein Speicherkonto in Azure finden Sie [ein Speicherkonto erstellen](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Führen Sie Ihre erste Azure-Speicher-Anwendung für Azure-Speicher in der cloud

Haben Sie ein Konto, können Sie eine einfache Azure Storage-Anwendung mit einer Azure Quick Start Beispielprojekte in Visual Studio erstellen. In diesem Lernprogramm konzentriert sich auf die Beispielprojekte für Azure Storage: **Azure Storage: Blobs**, **Azure Storage: Dateien**, **Azure Storage: Warteschlangen**, und **Azure Storage: Tabellen**:

1. Starten Sie Visual Studio.
2. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
3. Klicken Sie im Dialogfeld **Neues Projekt** auf **installierte** > **Vorlagen** > **Visual C#** > **Cloud** > **Schnellstarts** > **Data Services**.
    ein. Wählen Sie eine der folgenden Vorlagen: **Azure Storage: Blobs**, **Azure Storage: Dateien**, **Azure Storage: Warteschlangen**, oder **Azure Storage: Tabellen**.
    b. Stellen Sie sicher, dass **.NET Framework 4.5** als Zielframework ausgewählt ist.
    - 3.c. Geben Sie einen Namen für das Projekt, und erstellen Sie neue Visual Studio-Projektmappe dargestellt:

    ![Azure Quick Start][Image1]

Sie möchten den Quellcode vor Ausführen der Anwendung überprüfen. Um den Code zu überprüfen, wählen Sie **Projektmappen-Explorer** in Visual Studio im Menü **Ansicht** . Doppelt klicken Sie die Datei Program.cs.

Führen Sie die Beispiel-Anwendung:

1.  Wählen Sie in Visual Studio **Projektmappen-Explorer** im Menü **Ansicht** . Öffnen Sie die Datei App.config, und kommentieren Sie die Verbindungszeichenfolge für den Emulator Azure-Speicher:

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Kommentieren Sie die Verbindungszeichenfolge für Azure Storage Service und der Storage-Konto namens und Schlüssel in der Datei App.config:`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    Rufen Sie den Zugriffsschlüssel Storage-Konto finden Sie unter [Speicher Zugriff verwalten](storage-create-storage-account.md#manage-your-storage-access-keys).

3.  Nach Bereitstellen der speicherkontoname und Tastenkombination in der Datei App.config im Menü **Datei** auf **Alle** speichern, um alle Projektdateien.
4.  Klicken Sie im Menü **Erstellen** auf **Projektmappe erstellen**.
5.  Drücken Sie im Menü **Debuggen** **F11** Lösung schrittweise ausführen, oder drücken **F5** , um die Projektmappe auszuführen.


## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Führen Sie Ihre erste Azure-Speicher Anwendung lokal für Speicheremulator Azure

Der [Speicheremulator Azure](storage-use-emulator.md) bietet eine Umgebung, die Dienste Azure Blob, Warteschlange und Tabelle zu Entwicklungszwecken emuliert. Den Speicheremulator können testen die speicheranwendung, ohne ein Azure-Abonnement oder Speicherkonto und ohne Kosten.

Um ihn zu testen, erstellen Sie eine einfache Azure Storage-Anwendung mit einer Azure Quick Start Beispielprojekte in Visual Studio. In diesem Lernprogramm konzentriert sich auf die Beispielprojekte **Azure BLOB-Speicher**, **Azure-Tabellenspeicher**und **Azure Queue Storage** :

1. Starten Sie Visual Studio.
2. Klicken Sie im Menü **Datei** auf **Neues Projekt**.
3. Klicken Sie im Dialogfeld **Neues Projekt** auf **installierte** > **Vorlagen** > **Visual C#** > **Cloud** > **Schnellstarts** > **Data Services**.
    ein. Wählen Sie eine der folgenden Vorlagen: **Azure Storage: Blobs**, **Azure Storage: Dateien**, **Azure Storage: Warteschlangen**, oder **Azure Storage: Tabellen**.
    b. Stellen Sie sicher, dass **.NET Framework 4.5** als Zielframework ausgewählt ist.
    c. Geben Sie einen Namen für das Projekt, und erstellen Sie neue Visual Studio-Projektmappe dargestellt:

    ![Azure Quick Start][Image1]

4.  Wählen Sie in Visual Studio **Projektmappen-Explorer** im Menü **Ansicht** . Öffnen Sie App und kommentieren Sie die Verbindungszeichenfolge für Ihr Konto Azure-Speicher, wenn Sie bereits hinzugefügt haben. Entfernen Sie dann die Verbindungszeichenfolge für den Emulator Azure-Speicher:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Sie möchten den Quellcode vor Ausführen der Anwendung überprüfen. Um den Code zu überprüfen, wählen Sie **Projektmappen-Explorer** in Visual Studio im Menü **Ansicht** . Doppelt klicken Sie die Datei Program.cs.

Führen Sie die beispielanwendung in Azure Speicheremulator:

1.  Drücken Sie die Schaltfläche **Start** oder Windows, *Microsoft Azure Speicheremulator*suchen und starten Sie die Anwendung. Wenn der Emulator gestartet wird, sehen Sie ein Symbol und eine Benachrichtigung im Bereich Windows Task.
2.  Klicken Sie in Visual Studio im Menü **Erstellen** auf **Projektmappe erstellen** .
3.  Im Menü **Debuggen** drücken Sie **F11** , um die Projektmappe schrittweise ausführen, oder drücken Sie **F5** , um die Lösung von Anfang bis Ende ausgeführt.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie weitere Informationen zu Azure-Speicher:

* [Einführung in Microsoft Azure-Speicher](storage-introduction.md)
* [Erste Schritte mit Azure-Speicher-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md)
* [Erste Schritte mit Azure Table Storage mit .NET](storage-dotnet-how-to-use-tables.md)
* [Erste Schritte mit Azure Queue Storage mit .NET](storage-dotnet-how-to-use-queues.md)
* [Erste Schritte mit Windows Azure Dateispeicher](storage-dotnet-how-to-use-files.md)
* [Daten mit AzCopy-Befehlszeilenprogramm](storage-use-azcopy.md)
* [Azure-Speicher-Dokumentation](https://azure.microsoft.com/documentation/services/storage/)
* [Microsoft Azure Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Azure-Speicherdienste REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
