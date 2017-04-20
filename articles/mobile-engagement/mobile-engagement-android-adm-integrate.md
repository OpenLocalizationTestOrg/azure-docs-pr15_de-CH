<properties
    pageTitle="Azure Mobile Engagement Android SDK-Integration"
    description="Neueste Updates und Verfahren für Android SDK für Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-integrate-adm-with-engagement"></a>ADM mit Integration

> [AZURE.IMPORTANT] Integration beschrieben zu integrieren wie Android Dokument vor dieser Anleitung folgen.
>
> Dieses Dokument ist nur sinnvoll, wenn bereits integriert der Reach-Modul und Amazon-Geräten übertragen. Um Reach-Kampagnen in Ihre Anwendung integrieren, lesen Sie bitte zuerst zu integrieren Engagement für Android.

##<a name="introduction"></a>Einführung

Integration von ADM kann die Anwendung abgelegt werden, wenn Amazon Android-Geräte.

ADM-Nutzlasten abgelegt SDK immer enthalten die `azme` im Datenobjekt. So verwenden Sie ADM anderweitig in der Anwendung, Filtern Sie Push basierend auf diesem Schlüssel.

> [AZURE.IMPORTANT] Nur Amazon Kindle Geräte mit Android 4.0.3 oder unterstützt Amazon Gerät Messaging; Allerdings können Sie diesen Code sicher auf andere Geräte integrieren.

##<a name="sign-up-to-adm"></a>ADM anmelden

Wenn nicht bereits erfolgt, müssen Sie Ihrem Konto Amazon ADM aktivieren.

Die Prozedur finden Sie unter: [<https://developer.amazon.com/sdk/adm/credentials.html>].

Nach Abschluss des Verfahrens erhalten Sie:

-   OAuth Anmeldeinformationen (eine Client-ID und eines geheimen) Engagement Geräte übertragen können.
-   Ein API-Schlüssel, der in die Anwendung integriert werden.

##<a name="sdk-integration"></a>SDK-integration

### <a name="managing-device-registrations"></a>Verwalten von geräteregistrierungen

Jedes Gerät muss einen Registrierungsbefehl an ADM-Server senden, andernfalls sie nicht erreicht werden kann.

Wenn Sie bereits die [ADM-Clientbibliothek]verwenden und bereits [integrierte ADM] gelangen Sie direkt zu Android-Sdk-Adm.

Wenn ADM noch nicht integriert haben, hat Engagement einfacher in der Anwendung zu aktivieren:

Bearbeiten der `AndroidManifest.xml` Datei:

-   Fügen Sie den Amazon-Namespace die Datei sollte wie folgt beginnen:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   In der `<application/>` fügen diesem Abschnitt:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Nach Amazon-Tag hinzufügen, müssen Sie einen Buildfehler unter Android 2.1 ist Ziel-Projekt erstellen. Ein Buildziel **Android 2.1 +** verwendet (keine Sorge, Sie haben immer noch eine `minSdkVersion` 4 festgelegt).
-   Integrieren Sie ADM-API-Schlüssel als Vermögenswert durch folgende [Vorgehensweise].

Folgen Sie den nächsten Abschnitten.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Identifikationsnummer Engagement Push-Dienst kommunizieren und Benachrichtigung

Um die Identifikationsnummer des Geräts Engagement Push Service kommunizieren und die Benachrichtigung hinzufügen Folgendes Ihre `AndroidManifest.xml` Datei innerhalb der `<application/>` tag (selbst wenn Sie ADM ohne verwenden):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Sicherzustellen, dass die folgenden Berechtigungen der `AndroidManifest.xml` (vor der `</application>` Tag).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>GRANT Engagement OAuth-Anmeldeinformationen

Übermitteln Sie OAuth Anmeldedaten (Client-ID und geheimen) im Engagement-Portal.

[< Https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM-Clientbibliothek]:https://developer.amazon.com/sdk/adm/setup.html
[integrierte ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[Dieses Verfahren]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
