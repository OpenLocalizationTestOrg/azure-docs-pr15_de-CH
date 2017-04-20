<properties 
    pageTitle="Einrichten der Computer für Media Services-Entwicklung mit .NET" 
    description="Erfahren Sie mehr über die erforderlichen Komponenten für Media Services mit Media Services SDK für .NET. Auch Informationen Sie zum Visual Studio-Anwendung erstellen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>Media Services-Entwicklung mit .NET

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

In diesem Thema wird erläutert, wie zu Media Services Anwendungsentwicklung mit .NET.

**Azure Media Services.NET SDK** -Bibliothek können Sie beim Programmieren mit .NET Media Services. Um auch .NET zu erleichtern, wird die **Azure Media Services .NET SDK** -Erweiterungsbibliothek bereitgestellt. Diese Bibliothek enthält eine Reihe von Erweiterungsmethoden und Hilfsfunktionen, die .NET Code vereinfachen. Beide Bibliotheken sind über **NuGet** und **GitHub**verfügbar.


##<a name="prerequisites"></a>Erforderliche Komponenten

-   Ein Media Services-Konto in eine neue oder vorhandene Azure-Abonnement. Finden Sie [ein Media Services-Konto erstellen](media-services-portal-create-account.md).
-   Betriebssysteme: Windows 10, Windows 7, Windows 2008 R2 oder Windows 8.
-   .NET Framework 4.5.
-    Visual Studio 2015, Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 SP1 (Professional, Premium, Ultimate oder Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Erstellen und Konfigurieren eines Visual Studio-Projekts

In diesem Abschnitt wird das Erstellen eines Projekts in Visual Studio und für Media Services Development veranschaulicht.  In diesem Fall ist das Projekt eine C# Windows Konsolenanwendung hier gezeigten Setupschritte gelten jedoch für andere Projekttypen können Sie Media Services Applications (z. B. Windows Forms-Anwendung oder einer ASP.NET Web-Anwendung) erstellen.

Dieser Abschnitt zeigt, wie mit **NuGet** Media Services .NET SDK und anderen abhängigen Bibliotheken hinzufügen.

Alternativ können Sie die neuesten Media Services .NET SDK Bits von GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) und [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)) erhalten, erstellen Sie die Projektmappe und fügen Sie Verweise auf das Clientprojekt. Beachten Sie, dass alle erforderlichen Abhängigkeiten heruntergeladen und automatisch extrahiert.

1. Erstellen Sie eine neue C# in Visual Studio 2010 SP1 oder höher VS. Geben Sie **Name**, **Speicherort**und **Projektmappennamen**, und klicken Sie auf OK.

2. Erstellen der Lösung.

2. Verwenden Sie **NuGet** zum Installieren und **Azure Media Services .NET SDK**Erweiterungen. Installieren dieses Paket, **Media Services.NET SDK** installiert und andere erforderliche Abhängigkeit hinzugefügt.

    Stellen Sie sicher, dass Sie die neueste Version von NuGet installiert haben. Weitere Informationen und installieren benötigen finden Sie auf der [NuGet](http://nuget.codeplex.com/).

2. Namen Sie im Projektmappen-Explorer den des Projekts, und wählen Sie verwalten NuGet-Pakete.

    Das Dialogfeld "NuGet-Pakete verwalten" angezeigt wird.

3. In den Onlinekatalog Azure MediaServices Extensions suchen Sie, wählen Sie Azure Media Services .NET SDK Erweiterungen und klicken Sie dann auf die Schaltfläche installieren.

    Das Projekt geändert und Verweise auf Media Services .NET SDK Extensions, Media Services .NET SDK und anderen abhängigen Assemblys hinzugefügt.

4. Zur Förderung der Entwicklung Umweltschutz in Betracht NuGet-Paket wiederherzustellen. Weitere Informationen finden Sie unter [NuGet-Paket wiederherstellen "](http://docs.nuget.org/consume/package-restore).

3. Fügen Sie einen Verweis auf **System.Configuration** -Assembly. Diese Assembly enthält System.Configuration. **ConfigurationManager** -Klasse, die Zugriff auf die Konfigurationsdateien (z. B. App.config).

    Fügen Sie Verweise mit dem Dialogfeld Verweise verwaltet Maustaste Projekt im Projektmappen-Explorer. Wählen Sie hinzufügen und Verweise.

    Das Dialogfenster Referenzen verwalten.

4. Finden Sie unter .NET Framework-Assemblys die System.Configuration-Assembly, und klicken Sie auf OK.
5. Öffnen Sie die Datei App.config (die Datei dem Projekt hinzugefügt, wenn sie nicht standardmäßig hinzugefügt wurde) und einen *AppSettings* -Abschnitt der Datei hinzufügen.     
Legen Sie die Werte für Ihren Azure Media Services Account und Schlüssel, wie im folgenden Beispiel gezeigt.

    Um den Namen und Schlüssel Werte finden, Azure-Portal wechseln und Ihr Konto wählen. Das Fenster Einstellungen auf der rechten Seite. Wählen Sie im Einstellungsfenster Schlüssel. Klicken auf das Symbol neben jedem Textfeld kopiert den Wert in die Zwischenablage.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. Überschreiben der vorhandenen **mithilfe** Anweisung am Anfang der Datei Program.cs mit dem folgenden Code.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

An diesem Punkt können Sie entwickeln eine Anwendung Media Services.    


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
