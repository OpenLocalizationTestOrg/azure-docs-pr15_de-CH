<properties
    pageTitle="Erste Schritte mit Azure App Service Mobile Apps für apps Xamarin.iOS | Microsoft Azure"
    description="Dieses Lernprogramm mit Mobile Apps für Xamarin.iOS Entwicklung beginnen."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>


#<a name="create-a-xamarinios-app"></a>Erstellen einer Xamarin.iOS

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht die Cloud-Back-End-Dienst eine Xamarin.iOS app ein Azure mobile app Back-End hinzufügen.  Erstellen Sie neue mobile app Backend- und eine einfache _Aufgabenliste_ Xamarin.iOS Anwendung, die in Azure app-Daten gespeichert.

Dieses Lernprogramm ist eine Voraussetzung für alle anderen Xamarin.iOS Lehrgänge zu mithilfe des Mobile Apps in Azure App Service.

##<a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie die folgenden Komponenten:

* Ein aktives Azure-Konto. Haben Sie ein Konto, testen Sie eine Azure und bis zu 10 freie mobile apps, die Sie auch nach der Testphase nutzen können. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio mit Xamarin. Weitere Informationen finden Sie in [Installation und Setup für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

* Mac Xcode Version 7.0 oder höher und Xamarin Studio Community installiert. Siehe [Installation und Setup für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) und [Setup, Installation und Überprüfung für Mac-Benutzer](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

>[AZURE.NOTE]Wenn Sie mit Azure App Service beginnen, bevor Sie für ein Azure-Konto, gehen Sie [versuchen App](https://tryappservice.azure.com/?appServiceName=mobile)Service. Sie können sofort eine kurzlebige Starter-app in App Service erstellen – keine Kreditkarte und keine Zusagen.

## <a name="create-an-azure-mobile-app-backend"></a>Ein Azure Mobile App-Back-End erstellen

Gehen Sie Backend Mobile-Anwendung erstellen.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Konfigurieren Sie das Serverprojekt

Sie haben jetzt eine Azure Mobile App-Back-End bereitgestellt, die von mobile Clientanwendungen verwendet werden kann. Downloaden Sie nun ein Serverprojekt für eine einfache "Aufgabenliste" Back-End- und Azure veröffentlichen.

Die folgenden Schritte zum Serverprojekt Node.js oder .NET Backend Verwendung konfigurieren.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Herunterladen und Ausführen der Anwendung Xamarin.iOS

1. [Azure-Portal] in einem Browserfenster geöffnet.

2. Klicken Sie auf Blatt Einstellungen für Ihre Mobile auf **Einstieg** > **Xamarin.iOS**. Klicken Sie unter Schritt 3 **Erstellen einer neuen Anwendung** , wenn nicht bereits ausgewählt ist.  Klicken Sie anschließend auf die Schaltfläche **Download** .

    Eine Clientanwendung, die die mobile Back-End-Verbindung heruntergeladen. Speichern Sie die komprimierte Datei auf Ihrem lokalen Computer, und notieren Sie wo speichern.

3. Extrahieren Sie das Projekt, das Sie heruntergeladen haben, und öffnen Sie es Xamarin Studio (oder Visual Studio).

    ![][9]

    ![][8]

4. Drücken Sie F5, um das Projekt erstellen und Starten der Anwendung im Emulator iPhone.

5. In der Anwendung geben Sie einen aussagekräftigen Text _Xamarin erfahren Sie_, wie die **+** Schaltfläche.

    ![][10]

    Daten aus der Anforderung werden in der TodoItem-Tabelle eingefügt. In der Tabelle gespeicherten Elemente von mobile-app Backend zurückgegeben werden, und die Daten in der Liste angezeigt.

>[AZURE.NOTE]Überprüfen Sie den Code, der die mobile Anwendung Backend Abfragen und Einfügen von Daten in der Datei QSTodoService.cs C# zugreift.

##<a name="next-steps"></a>Nächste Schritte

* [Ihre Anwendung Offline synchronisieren hinzufügen](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Authentifizierung für Ihre Anwendung hinzufügen](app-service-mobile-xamarin-ios-get-started-users.md)
* [Xamarin.Android app Pushbenachrichtigungen hinzufügen](app-service-mobile-xamarin-ios-get-started-push.md)
* [Verwendung den verwalteten Client für Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure-portal]: https://portal.azure.com/
