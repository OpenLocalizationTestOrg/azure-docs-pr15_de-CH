<properties
    pageTitle="Azure Mobile Engagement Android SDK-Integration"
    description="Neueste Updates und Verfahren für Android SDK für Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-gcm-with-mobile-engagement"></a>GCM mit Mobile Integration

> [AZURE.IMPORTANT] Integration beschrieben zu integrieren wie Android Dokument vor dieser Anleitung folgen.
>
> Dieses Dokument ist nur sinnvoll, wenn bereits integriert der Reach-Modul und Google Geräte übertragen. Um Reach-Kampagnen in Ihre Anwendung integrieren, lesen Sie bitte zuerst zu integrieren Engagement für Android.

##<a name="introduction"></a>Einführung

Integration von GCM ermöglicht die Anwendung abgelegt werden.

GCM Nutzlasten abgelegt SDK immer enthalten die `azme` im Datenobjekt. So verwenden Sie GCM anderweitig in der Anwendung, Filtern Sie basierend auf diesem Schlüssel drückt.

> [AZURE.IMPORTANT] Nur Geräte mit Android 2.2 oder höher, mit Googleplay installiert und mit Google Hintergrund Verbindung aktiviert kann verschoben werden mit GCM; Allerdings können Sie diesen Code sicher auf nicht unterstützte Geräte integrieren (verwendet nur Absichten).

##<a name="create-a-google-cloud-messaging-project-with-api-key"></a>Erstellen eines Google Cloud Messaging mit API-Schlüssel

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

##<a name="sdk-integration"></a>SDK-integration

### <a name="managing-device-registrations"></a>Verwalten von geräteregistrierungen

Jedes Gerät muss einen Registrierungsbefehl an die Google-Server senden, andernfalls sie nicht erreicht werden kann.

Ein Gerät kann sich von GCM-Benachrichtigung Abmelden (das Gerät ist automatisch, wenn die Anwendung deinstalliert).

Wenn [Google spielen SDK] nicht verwenden oder nicht bereits die Absicht Registrierung selbst senden, machen Sie Engagement das Gerät automatisch für Sie registriert.

Fügen Sie dazu die folgenden der `AndroidManifest.xml` Datei innerhalb der `<application/>` Tag:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Identifikationsnummer Engagement Push-Dienst kommunizieren und Benachrichtigung

Um die Identifikationsnummer des Geräts Engagement Push Service kommunizieren und die Benachrichtigung hinzufügen Folgendes Ihre `AndroidManifest.xml` Datei innerhalb der `<application/>` tag (selbst wenn Sie geräteregistrierungen selbst):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Sicherzustellen, dass die folgenden Berechtigungen der `AndroidManifest.xml` (nach der `</application>` Tag).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##<a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Der API-Schlüssel GCM gewähren Sie Mobile Engagement Zugriff

Führen Sie [Dieses Handbuch](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) GCM API-Schlüsseln Mobile Engagement Zugriff gewähren.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start
