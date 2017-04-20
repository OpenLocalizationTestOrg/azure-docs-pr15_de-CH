<properties 
   pageTitle="Konfiguration und Verwendung der Speicheremulator mit Visual Studio | Microsoft Azure"
   description="Konfiguration und Verwendung der Speicheremulator mit Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Konfiguration und Verwendung des Speicheremulators mit Visual Studio

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Übersicht
Die Azure SDK Development Environment enthält Speicheremulator ein Dienstprogramm, das BLOB-Warteschlange und Tabelle Speicherdienste Azure zur Ihrem lokalen Entwicklungscomputer simuliert. Sind Sie einem Cloud-Dienst erstellen, Azure-Speicherdienste beschäftigt oder Schreiben jeder externen Anwendung, die die Speicherdienste aufruft, können Sie Code lokal mit Speicheremulator testen. Azure Tools for Microsoft Visual Studio integriert Verwaltung der Speicheremulator in Visual Studio. Azure Tools initialisiert die Datenbank bei der ersten Verwendung Emulator, startet den Emulator Speicherdienst beim Ausführen oder Debuggen des Codes von Visual Studio und bietet schreibgeschützten Zugriff auf Speicher Emulator Daten über den Azure-Speicher-Explorer.

Detailinformationen Speicheremulator Anforderungen sowie Hinweise zur benutzerdefinierten Konfiguration finden Sie unter [Verwenden der Speicheremulator Azure für Entwicklung und Testing](./storage/storage-use-emulator.md).

>[AZURE.NOTE] Es gibt einige Unterschiede Funktionalität zwischen Speicher-Emulator Simulation und Azure-Speicher. [Unterschiede zwischen der Speicheremulator und Azure](./storage/storage-use-emulator.md) in Azure SDK-Dokumentation für Informationen über die Unterschiede anzeigen

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Konfigurieren eine Verbindungszeichenfolge für Speicheremulator

Um Code in einer Rolle Speicheremulator zugreifen sollten Sie konfigurieren eine Verbindungszeichenfolge Speicheremulator Punkte und später auf einem Azure Storage-Konto geändert werden können. Eine Verbindungszeichenfolge ist eine Einstellung, die Ihrer Rolle zur Laufzeit ein Speicherkonto Verbindung lesen kann. Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [Konfigurieren der Azure-Anwendung](https://msdn.microsoft.com/library/azure/2da5d6ce-f74d-45a9-bf6b-b3a60c5ef74e#BK_SettingsPage).

>[AZURE.NOTE] Mithilfe der **DevelopmentStorageAccount** -Eigenschaft können Sie einen Verweis auf das Speicherkonto Emulator Code zurück. Dieser Ansatz funktioniert Wenn Speicheremulator Code zugreifen möchten, aber wenn Sie Ihre Anwendung in Azure veröffentlichen möchten, müssen eine Verbindungszeichenfolge Code um diese Verbindungszeichenfolge verwenden, bevor Sie ihn veröffentlichen zu Ihr Azure Storage-Konto erstellen. Wenn Sie zwischen Speicher-Emulator und ein Azure-Speicher häufig wechseln, wird eine Verbindungszeichenfolge dieser Prozess vereinfacht.

## <a name="initializing-and-running-the-storage-emulator"></a>Initialisieren und Speicheremulator ausführen

Sie können angeben, dass beim Ausführen oder den Dienst in Visual Studio debuggen, Visual Studio Speicheremulator automatisch startet. Öffnen Sie im Projektmappen-Explorer im Kontextmenü der **Azure** -Projekt zu, und wählen Sie **Eigenschaften**. Auf der Registerkarte **Entwicklung** in der Liste **Starten Azure Speicheremulator** wählen Sie **True** (Wenn sie den Wert nicht bereits festgelegt).

Beim ersten Ausführen oder den Dienst von Visual Studio Debuggen startet Speicheremulator eine Initialisierung. Dabei Häfen für Speicheremulator reserviert und erstellt die Datenbank Emulator. Abschließend muss dabei nicht erneut ausgeführt, wenn die Emulation Datenbank gelöscht wird.

>[AZURE.NOTE] Ab Juni 2012 Release Azure Tools sollten Speicheremulator standardmäßig SQL Express LocalDB. In früheren Versionen von Azure Tools Speicheremulator führt für eine Standardinstanz von SQL Express 2005 oder 2008, die Sie installieren müssen, bevor Sie können Azure SDK installieren. Sie können auch Speicheremulator für eine benannte Instanz von SQL Express oder eine benannte oder eine Standardinstanz von Microsoft SQL Server ausführen. Speicheremulator gegen eine andere Instanz als die Standardinstanz konfigurieren, finden Sie unter [Verwenden der Speicheremulator Azure für Entwicklung und Testing](./storage/storage-use-emulator.md).

Der Speicheremulator bietet eine Benutzeroberfläche zum Anzeigen des Status der lokalen Speicherdienste und starten, beenden und zurücksetzen. Nach dem Start des Speicherdienst Emulator können Benutzeroberfläche angezeigt starten oder beenden Sie den Dienst mit der rechten Maustaste auf das Symbol für Microsoft Azure-Emulator in der Windows-Taskleiste.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Emulator Daten im Server-Explorer anzeigen

Azure Storage Node in Server-Explorer können Sie Daten und Einstellungen für Speicherkonten einschließlich Speicheremulator BLOB- und Daten. Weitere Informationen finden Sie unter [Durchsuchen und Verwalten von Speicherressourcen mit Server-Explorer](https://msdn.microsoft.com/library/azure/ff683677.aspx) .
