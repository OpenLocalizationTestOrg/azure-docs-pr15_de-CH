<properties
    pageTitle="Android Azure Mobile Apps Pushbenachrichtigungen hinzufügen"
    description="Informationen Sie zum Azure Mobile Apps mithilfe der Android Push-Benachrichtigung senden."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Ihre Android Pushbenachrichtigungen hinzufügen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Übersicht
In diesem Lernprogramm fügen Sie Pushbenachrichtigungen [Android Schnellstart] hinzu, damit eine Push-Benachrichtigung an das Gerät gesendet wird, jedes Mal, wenn ein Datensatz eingefügt wird.

Wenn Sie Project Server heruntergeladene Schnellstart nicht verwenden, benötigen Sie das Erweiterungspaket Push Notification. Weitere Informationen finden Sie unter [Arbeiten mit der Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes:

* Je nach Ihrem Projekt Back-End-IDE:

    * [Android Studio](https://developer.android.com/sdk/index.html) hat diese app Node.js-Backend.

    * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) oder später, wenn diese app hat .net Backend.

* Android 2.3 oder höher Google Repository Revision 27 oder höher und Googleplay 9.0.2 oder höher für FB Cloud Messaging.

* [Schnelleinstieg Android]abzuschließen.

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Erstellen Sie ein Projekt, das FB Cloud Messaging unterstützt

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfigurieren eines Benachrichtigung Hubs

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurieren von Azure Push-Benachrichtigung senden

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Push-Benachrichtigung für Project Server aktivieren

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Push-Benachrichtigung für Ihre Anwendung hinzufügen

In diesem Abschnitt Aktualisieren Sie Ihren Client Android Pushbenachrichtigungen behandeln.

### <a name="verify-android-sdk-version"></a>Android SDK-Version überprüfen

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Der nächste Schritt ist Google Play Services installieren. Google Cloud Messaging hat API-Ebenen Mindestanforderungen für Entwicklung und testing, die Eigenschaft **MinSdkVersion** im Manifest entsprechen muss.

Wenn Sie ein älteres Gerät testen, und Lesen Sie dann [Festlegen, Google spielen Services SDK] , wie tief Sie können diesen Wert festlegen, und entsprechend festgelegt.

### <a name="add-google-play-services-to-the-project"></a>Google Play Services zum Projekt hinzufügen

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Fügen Sie Code hinzu

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Testen der app veröffentlichten mobilen Dienst

Sie können die Anwendung durch ein Handy mit einem USB-Kabel direkt anfügen oder ein virtuelles Gerät im Emulator testen.

## <a name="more"></a>Mehr

<!-- URLs -->
[Schnelleinstieg Android]: app-service-mobile-android-get-started.md

[Einrichten von Google Play Services SDK]:https://developers.google.com/android/guides/setup
