<properties
    pageTitle="Xamarin.Android app Pushbenachrichtigungen hinzufügen | Azure App Service"
    description="Informationen Sie zum Verwenden von Azure App Service und Azure Notification Hubs Xamarin.Android app Pushbenachrichtigungen an"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Xamarin.Android app Pushbenachrichtigungen hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Übersicht


In diesem Lernprogramm hinzugefügt Pushbenachrichtigungen Projekt [Xamarin.Android schnell starten](app-service-mobile-windows-store-dotnet-get-started.md) , damit eine Push-Benachrichtigung an das Gerät gesendet wird, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, benötigen Sie das Erweiterungspaket Push Notification. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .


##<a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm ist Folgendes erforderlich:

+ Aktive Konto. Sie können für ein Konto bei [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302)anmelden.
+ [Google Cloud Messaging Clientkomponente](http://components.xamarin.com/view/GCMClient/).

##<a name="configure-hub"></a>Konfigurieren eines Benachrichtigung Hubs

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>Aktivieren Sie FB Cloud Messaging

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>Konfigurieren von Azure Push-Anforderung senden

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>Aktualisieren von Project Server zum Senden von Pushbenachrichtigungen

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Konfigurieren Sie das Clientprojekt für Pushbenachrichtigungen

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Ihre app Push Notifications Code hinzufügen

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Pushbenachrichtigungen in Ihrer Anwendung testen

Sie können die Anwendung von einem virtuellen Gerät im Emulator testen. Es sind zusätzliche Konfigurationsschritte erforderlich, wenn auf einem Emulator ausgeführt.

1. Stellen Sie sicher, dass Sie zum Bereitstellen oder auf ein virtuelles Gerät mit Google APIs angestrebt Debuggen, wie folgt Android Virtual Device (AVD)-Manager.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Android Gerät von **Apps**auf einem Konto hinzufügen > **Settings** > **Konto hinzufügen**, dann folgen.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Führen Sie die Aufgabenliste app vor und fügen Sie ein neues Todo-Element ein. Dieses Mal wird ein Symbol im Infobereich angezeigt. Öffnen Sie die Schublade Benachrichtigung um den vollständigen Text der Benachrichtigung.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
