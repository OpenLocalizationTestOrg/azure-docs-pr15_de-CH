<properties
    pageTitle="Erste Schritte mit Azure Apps für Xamarin.Android apps"
    description="Folgen Sie dieser Anleitung zum Einstieg in Azure Mobile Apps für Xamarin Android Entwicklung"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor="" />

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha" />

#<a name="create-a-xamarinandroid-app"></a>Erstellen einer Xamarin.Android

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Übersicht

Dieses Lernprogramm zeigt wie eine Xamarin.Android app Cloud-Back-End-Dienst hinzugefügt. Weitere Informationen finden Sie unter [Was sind Mobile Apps](app-service-mobile-value-prop.md).

Screenshot von abgeschlossenen Anwendung lautet wie folgt:

![][0]

Dieses Lernprogramm ist eine Voraussetzung für andere Mobile Apps Lernprogramme für Xamarin.Android apps.

##<a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie die folgenden Komponenten:

* Ein aktives Azure-Konto. Wenn Sie ein Konto haben, melden Sie sich für eine Testversion von Azure und bis zu 10 freie Mobile Apps. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio mit Xamarin. Weitere Informationen finden Sie in [Installation und Setup für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

>[AZURE.NOTE]Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [versuchen App](https://tryappservice.azure.com/?appServiceName=mobile)Service.  Sie können sofort kurzlebige Starter Mobile App in App Service erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="create-an-azure-mobile-app-backend"></a>Ein Azure Mobile App-Back-End erstellen

Gehen Sie Backend Mobile-Anwendung erstellen.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Sie haben jetzt eine Azure Mobile App-Back-End bereitgestellt, die von mobile Clientanwendungen verwendet werden kann. Downloaden Sie nun ein Serverprojekt für eine einfache "Aufgabenliste" Back-End- und Azure veröffentlichen.

## <a name="configure-the-server-project"></a>Konfigurieren Sie das Serverprojekt

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Herunterladen und Ausführen der Anwendung Xamarin.Android

1. Klicken Sie unter **herunterladen und Ausführen des Projekts Xamarin.Android**auf **Download** .

    Speichern Sie die komprimierte Datei auf Ihrem lokalen Computer, und notieren Sie wo speichern.

2. Drücken Sie **F5** , um das Projekt und die Anwendung zu starten.

3. Geben Sie in der app einen aussagekräftigen Text wie _das Lernprogramm abgeschlossen_ , und klicken Sie dann auf die Schaltfläche **Hinzufügen** .

    ![][10]

    Daten aus der Anforderung werden in der TodoItem-Tabelle eingefügt. In der Tabelle gespeicherten Elemente vom mobile-app-Back-End zurückgegeben werden und die Daten in der Liste angezeigt.

    > [AZURE.NOTE] Überprüfen Sie den Code, der die mobile Anwendung Backend Abfragen und Einfügen von Daten in der Datei ToDoActivity.cs C# zugreift.

##<a name="next-steps"></a>Nächste Schritte

* [Ihre Anwendung Offline synchronisieren hinzufügen](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Authentifizierung für Ihre Anwendung hinzufügen](app-service-mobile-xamarin-android-get-started-users.md)
* [Xamarin.Android app Pushbenachrichtigungen hinzufügen](app-service-mobile-xamarin-android-get-started-push.md)
* [Verwendung den verwalteten Client für Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)


<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
