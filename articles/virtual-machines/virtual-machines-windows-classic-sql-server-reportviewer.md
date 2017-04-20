<properties 
    pageTitle="ReportViewer in einer Website verwenden | Microsoft Azure"
    description="Dieses Thema beschreibt, wie eine Microsoft Azure-Website mit Visual Studio ReportViewer-Steuerelement erstellen, das einen Bericht auf einer Microsoft Azure Virtual Machine gespeichert wird."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar" 
    tags="azure-service-management" />
<tags 
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Eine Website in Azure verwenden Sie ReportViewer

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Erstellen Sie eine Microsoft Azure-Website mit Visual Studio ReportViewer-Steuerelement, die einen auf einer Microsoft Azure Virtual Machine gespeicherten Bericht angezeigt. Das ReportViewer-Steuerelement wird in einer Webanwendung, mit der ASP.NET Web Application-Vorlage zu erstellen.

>[AZURE.IMPORTANT] ASP.NET MVC Webanwendungsprojekt Vorlagen unterstützt das ReportViewer-Steuerelement nicht.

ReportViewer in Ihre Microsoft Azure-Website einbinden, müssen Sie die folgenden Aufgaben ausführen.

- **Hinzufügen** Das Bereitstellungspaket Assemblys

- **Konfigurieren** Authentifizierung und Autorisierung

- **Veröffentlichen** der ASP.NET Web-Anwendung in Azure

## <a name="prerequisites"></a>Erforderliche Komponenten

Lesen Sie den Abschnitt "allgemeine Empfehlung und best Practices" in [SQL Server Business Intelligence in Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).

>[AZURE.NOTE] ReportViewer-Steuerelemente werden mit Visual Studio Standard Edition oder höher ausgeliefert. Wenn Sie Web Developer Express Edition verwenden, installieren Sie die [MICROSOFT REPORT VIEWER 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) ReportViewer Laufzeitfunktionen verwenden.
>
>In Microsoft Azure nicht ReportViewer im lokalen Verarbeitungsmodus konfiguriert werden.

[Reporting Services-Berichts-Viewer-Steuerelement und Microsoft Azure Virtual Machine-basierte Berichtsserver](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)Whitepaper lesen.

## <a name="adding-assemblies-to-the-deployment-package"></a>Das Bereitstellungspaket Assemblys hinzufügen

Beim Hosten Ihrer ASP.NET Anwendung lokale ReportViewer-Assemblys werden normalerweise direkt im globalen Assemblycache (GAC) des IIS-Servers während der Installation von Visual Studio installiert und können direkt von der Anwendung zugegriffen werden. Jedoch beim Hosten der Anwendung ASP.NET in der Cloud lässt Microsoft Azure nicht alle im GAC installiert werden, sodass Sie sicherstellen müssen, dass ReportViewer-Assemblys für die Anwendung verfügbar sind. Fügen Sie Verweise im Projekt, und konfigurieren sie lokal kopiert werden.

Im Remoteverarbeitungsmodus verwendet das ReportViewer-Steuerelement die folgenden Assemblys:

- **Microsoft.ReportViewer.WebForms.dll**: enthält den ReportViewer-Code ReportViewer in Ihrer Seite verwenden möchten. Ein Verweis auf diese Assembly wird dem Projekt hinzugefügt, beim Ablegen eines ReportViewer-Steuerelements auf einer ASP.NET Seite im Projekt.

- **Microsoft.ReportViewer.Common.dll**: enthält Klassen, die das ReportViewer-Steuerelement zur Laufzeit verwendet. Es wird nicht automatisch dem Projekt hinzugefügt.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Einen Verweis auf Microsoft.ReportViewer.Common hinzufügen

- Maustaste **Referenzknoten des Projekts** die Option **Verweis hinzufügen**wählen Sie auf der Registerkarte .NET die Assembly, und klicken Sie auf **OK**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Die Assemblys der Anwendung ASP.NET lokal zugänglich

1. Klicken Sie im Ordner **Verweise** auf der Microsoft.ReportViewer.Common-Assembly, sodass seine Eigenschaften im Eigenschaftenfenster angezeigt.

1. Setzen Sie im Bereich Eigenschaften **Lokale Kopie** auf True ein.

1. Wiederholen Sie die Schritte 1 und 2 für Microsoft.ReportViewer.WebForms.

### <a name="to-get-reportviewer-language-pack"></a>Zu ReportViewer-Sprachpaket

1. Installieren Sie das entsprechende Microsoft Report Viewer 2012 Runtime verteilbare Paket vom [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=317386).

1. Wählen Sie die Sprache aus der Dropdown-Liste und die Seite mit der entsprechenden Download Center-Seite umgeleitet wird.

1. Klicken Sie auf **herunterladen** , um die ReportViewerLP.exe herunterzuladen.

1. Nachdem Sie ReportViewerLP.exe heruntergeladen haben, klicken Sie auf **Ausführen** , um sofort installieren, oder klicken Sie auf **Speichern** , um es auf Ihrem Computer speichern. Wenn Sie auf **Speichern**klicken, müssen Sie den Namen des Ordners ein, in dem Sie die Datei speichern.

1. Suchen Sie den Ordner, in dem die Datei gespeichert. ReportViewerLP.exe Maustaste und **Klicken**auf **als Administrator ausführen**.

1. Nach ReportViewerLP.exe ausführen, sehen Sie die c:\windows\assembly hat die Ressource Dateien **Microsoft.ReportViewer.Webforms.Resources** und **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Konfigurieren für lokalisierte ReportViewer-Steuerelement

1. Downloaden Sie und installieren Sie Microsoft Report Viewer 2012 Runtime verteilbare Paket gemäß der oben angegebenen Anleitung.

1. Erstellen <language> Ordner und kopieren die zugeordnete Ressource Assemblydateien vorhanden. Die Assembly Ressourcendateien kopiert werden: **Microsoft.ReportViewer.Webforms.Resources.dll** und **Microsoft.ReportViewer.Common.Resources.dll**. Wählen Sie die Assemblydateien, und legen Sie im Bereich Eigenschaften auf "**immer kopieren**" **In Ausgabeverzeichnis kopieren** .

1. Culture und UICulture für das Webprojekt fest Weitere Informationen zum Festlegen der Kultur und Benutzeroberflächenkultur für eine ASP.NET Webseite finden Sie [wie: Festlegen der Kultur und Benutzeroberflächenkultur für ASP.NET Web Page Globalization](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Konfigurieren von Authentifizierung und Autorisierung

ReportViewer benötigt gültige Anmeldeinformationen zur Authentifizierung am Berichtsserver und die Anmeldeinformationen müssen vom Berichtsserver autorisiert werden, auf die Berichte zuzugreifen, werden soll. Informationen zur Authentifizierung finden Sie im Whitepaper [Berichtanzeige-Steuerelements für Reporting Services und Microsoft Azure Virtual Machine Berichtsserver basiert](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>ASP.NET Web-Anwendung in Azure veröffentlichen

Informationen zum Veröffentlichen einer ASP.NET Web-Anwendung in Azure finden Sie [wie: Migrieren und Veröffentlichen einer Web-Anwendung in Azure aus Visual Studio](../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) und [mit Web-Apps und ASP.NET](../app-service-web/web-sites-dotnet-get-started.md).

>[AZURE.IMPORTANT] Wenn der Befehl Azure Bereitstellungsprojekt hinzufügen oder Hinzufügen von Azure Cloud Service Project im Projektmappen-Explorer im Kontextmenü nicht angezeigt wird, müssen Sie das Zielframework für das Projekt auf.NET Framework 4 ändern.
>
>Die beiden Befehle bieten dieselbe Funktionalität. Mindestens einer der Befehl erscheint im Kontextmenü je nach Version des Microsoft Azure SDK installiert haben.

## <a name="resources"></a>Ressourcen

[Microsoft-Berichte](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server Business Intelligence in Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md)

[Mithilfe von PowerShell Azure VM mit einem einheitlichen Berichtsserver erstellen](virtual-machines-windows-classic-ps-sql-report.md)

[Reporting Services-Bericht-Viewer-Steuerelement und Microsoft Azure basiert Virtual Machine Berichtsserver](http://download.microsoft.com/download/2/2/0/220DE2F1-8AB3-474D-8F8B-C998F7C56B5D/Reporting%20Services%20report%20viewer%20control%20and%20Azure%20VM%20based%20report%20servers.docx)
