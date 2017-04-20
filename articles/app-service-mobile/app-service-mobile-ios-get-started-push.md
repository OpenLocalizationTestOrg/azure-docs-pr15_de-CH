<properties
    pageTitle="IOS-App mit Azure Mobile Apps Pushbenachrichtigungen hinzufügen"
    description="Informationen Sie zum Azure Mobile Apps senden Pushbenachrichtigungen zu iOS-app."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>IOS-App Pushbenachrichtigungen hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Übersicht
In diesem Lernprogramm fügen Sie Pushbenachrichtigungen Projekt [iOS quick start] , damit eine Push-Benachrichtigung an das Gerät gesendet wird, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, benötigen Sie das Erweiterungspaket Push Notification. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

[IOS-Simulator Pushbenachrichtigungen nicht unterstützt](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Sie benötigen eine physische iOS-Gerät und eine [Mitgliedschaft Apple Developer](https://developer.apple.com/programs/ios/).

##<a name="configure-hub"></a>Konfigurieren der Benachrichtigungshub

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Anwendung für Push-Benachrichtigung registrieren

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurieren von Azure Push-Benachrichtigung senden

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>Aktualisieren von Back-End-Push-Benachrichtigung senden

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>App Pushbenachrichtigungen hinzufügen

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Test Pushbenachrichtigungen

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>Mehr

* Vorlagen bieten Ihnen Flexibilität plattformübergreifende Push und lokalisierte drückt. [Wie mit iOS-Clientbibliothek für Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) veranschaulicht die Vorlagen zu registrieren.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS-Schnellstart]: app-service-mobile-ios-get-started.md
