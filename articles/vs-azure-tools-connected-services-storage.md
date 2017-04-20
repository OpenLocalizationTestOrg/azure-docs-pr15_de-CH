<properties 
   pageTitle="Azure-Speicher mit verbundenen Dienste in Visual Studio hinzufügen | Microsoft Azure"
   description="Ihre app durch Visual Studio verbunden Dienste hinzufügen im Dialogfeld fügen Sie Azure-Speicher hinzu"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Azure hinzufügen mithilfe von Visual Studio verbunden Services

## <a name="overview"></a>Übersicht

Visual Studio 2015 können alle C#-Clouddienst, .NET Backend-mobile Service, ASP.NET Website oder Dienst, ASP.NET 5 Service oder Azure Webauftrags Dienst Azure-Speicher herstellen **Verbundenen Dienste hinzufügen** Dialogfeld. Verbundene Dienstfunktionalität fügt die benötigten Verweise und Verbindungscode und Konfigurationsdateien entsprechend ändert. Das Dialogfeld wird auch Dokumentation, das angibt, was die nächsten Schritte zu BLOB-Speicher, Warteschlangen und Tabellen sind.

## <a name="supported-project-types"></a>Unterstützte Projekttypen

Das Dialogfeld Verbindung können Sie die folgenden Projekttypen zu Azure-Speicher herstellen.

- ASP.NET Web Projects

- ASP.NET 5 Projekte

- .NET Cloud Service Webrolle und Worker-Rolle Projekte

- Mobile Services-Projekten

- Azure Webauftrags Projekte


## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Das Dialogfeld Verbindung mit Azure-Speicher zu verbinden

1. Stellen Sie sicher, dass Sie ein Azure-Konto verfügen. Wenn Sie ein Azure-Konto haben, können Sie eine [kostenlose Testversion](http://go.microsoft.com/fwlink/?LinkId=518146)anmelden. Haben Sie ein Azure-Konto können Sie Storage-Konten erstellen, mobile Dienste erstellen und Azure Active Directory konfigurieren.

1. Öffnen Sie das Projekt in Visual Studio das Kontextmenü für den Knoten **Verweise** im Projektmappen-Explorer und wählen Sie **Verbundene Dienst hinzufügen**.

    ![Hinzufügen eines verbundenen Dienstes](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. Klicken Sie im Dialogfeld **Verbundenen Dienst hinzufügen** wählen Sie **Azure Storage**und wählen Sie dann die Schaltfläche **Konfigurieren** . Sie können aufgefordert Azure anmelden, wenn Sie dies nicht bereits getan haben.

    ![Dialogfeld Connected-Service - Speicher hinzufügen](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1. Klicken Sie im Dialogfeld **Azure Storage** ein Storage-Konto auswählen und **Hinzufügen**.

    Möchten Sie ein neues Speicherkonto erstellen, gehen Sie zum nächsten Schritt. Andernfalls fahren Sie mit Schritt 6 fort.

    ![Dialogfeld Azure-Speicher](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. So erstellen Sie ein neues Speicherkonto 

    1. Wählen Sie unten im Dialogfeld Azure Storage **Storage neues Konto erstellen** .

    1. Füllen Sie das Dialogfeld **Speicherkonto erstellen** und dann die Schaltfläche **Erstellen** .
    
        ![Dialogfeld Azure-Speicher](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        Wenn Sie in das Dialogfeld **Azure Storage** sind, der neue Speicher in der Liste angezeigt.

    1. Wählen Sie in der Liste den neuen Speicher, und wählen Sie **Hinzufügen**.

1. Der Speicherdienst verbunden wird unter dem Knoten Verweise des Projekts Webauftrag.

    ![Azure-Speicher im Webprojekt Aufträge](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. Überprüfen Sie die Seite Erste Schritte angezeigt und herausfinden Sie, wie das Projekt geändert wurde. Getting Started-Seite wird im Browser angezeigt, verbundenen Dienst hinzufügen. Überprüfen der vorgeschlagenen Schritte und Codebeispiele oder wechseln Sie zur Seite geschehen, welche Verweise zum Projekt hinzugefügt wurden, und wie die Konfigurationsdateien Dateien geändert wurden.

## <a name="how-your-project-is-modified"></a>Wie Projekt geändert

Das Dialogfeld abschließend Visual Studio Verweise hinzugefügt und bestimmte Dateien ändert. Einfügen hängt vom Projekttyp ab. 

 - ASP.NET Projekte finden Sie unter [wo – ASP.NET Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513126). 
 - ASP.NET 5-Projekten finden Sie unter [wo – ASP.NET 5 Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513124). 
 - Cloud Projekte Webrollen und Worker-Rollen finden Sie unter [wo-Cloud-Dienst-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=516965). 
 - Webauftrags Projekte finden Sie unter [wo - Webauftrags Projekte](./storage/vs-storage-webjobs-what-happened.md).

## <a name="next-steps"></a>Nächste Schritte

1. Erste Schritte mit Codebeispiele als Richtlinie und den gewünschten Speichertyp erstellen und starten Code Zugriff auf Ihr Speicherkonto schreiben!

1. Fragen und Hilfe
     - [MSDN-Forum: Azure-Speicher](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Azure-Speicher-Teamblog](http://blogs.msdn.com/b/windowsazurestorage/)

     - [Speicheranlage in azure.microsoft.com](https://azure.microsoft.com/services/storage/)

     - [Dokumentation zur azure.microsoft.com](https://azure.microsoft.com/documentation/services/storage/)

