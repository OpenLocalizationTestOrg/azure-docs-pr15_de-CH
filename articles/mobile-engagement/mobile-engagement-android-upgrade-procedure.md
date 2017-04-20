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


#<a name="upgrade-procedures"></a>Upgrade-Verfahren

Wenn bereits eine ältere Version unseres SDK in die Anwendung integriert haben, müssen Sie Punkte im SDK aktualisieren.

Möglicherweise müssen mehrere Verfahren, wenn Sie mehrere Versionen des SDK verpasst. Zum Beispiel wenn Sie migrieren von 1.4.0 zu 1.6.0 Sie zuerst die Prozedur "von 1.4.0 auf 1.5.0 müssen" dann "von 1.5.0, 1.6.0" Verfahren.

Welche Version Sie aktualisieren, müssen Sie ersetzen die `mobile-engagement-VERSION.jar` durch den neuen.

##<a name="from-420-to-421"></a>Von 4.2.0, 4.2.1

Dieser Schritt kann tatsächlich auf einer beliebigen Version des SDK, wird eine Verbesserung der Sicherheit erreichen Aktivitäten integrieren.

Sie sollten nun `exported="false"` für alle erreichen.

Reach-Aktivitäten sollten jetzt Aussehen auf Ihrem `AndroidManifest.xml`:

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

##<a name="from-400-to-410"></a>Von 4.0.0 auf 4.1.0

Das SDK jetzt Handle neue Berechtigungsmodell von Android.

Wenn Sie Position Features oder Überblick Benachrichtigungen lesen Sie [diesen Abschnitt](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Neben neuen Berechtigungsmodell unterstützen wir jetzt konfigurieren Speicherort Funktionen zur Laufzeit.
Wir sind auch kompatibel mit Manifesten Parameter Position aber ist jetzt veraltet. Runtime Konfiguration Entfernen der folgenden Abschnitte aus der ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

und [diese aktualisierten Prozedur](mobile-engagement-android-integrate-engagement.md#location-reporting) Laufzeitkonfiguration verwenden.

##<a name="from-300-to-400"></a>Von 3.0.0 auf 4.0.0

### <a name="native-push"></a>Native push

Native Push (GCM/ADM) wird jetzt auch für app-Benachrichtigung verwendet systemeigene Push Anmeldeinformationen für jede Art von pushkampagne konfigurieren müssen.

[Wenn nicht bereits Bitte gehen.](mobile-engagement-android-integrate-engagement-reach.md#native-push)

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Reach-Integration wurde geändert ``AndroidManifest.xml``.

Ersetzen Sie dies:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Durch

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

Es ist möglicherweise ein Ladebildschirm jetzt beim Klicken auf eine Ankündigung (mit Text/Webinhalte) oder eine Umfrage.
Sie müssen für diese Kampagnen 4.0.0 arbeiten hinzufügen:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Ressourcen

Einbetten der neuen `res/layout/engagement_loading.xml` Datei in das Projekt.

##<a name="from-240-to-300"></a>Aus 2.4.0, 3.0.0

Die folgenden beschreibt der Capptain Service Capptain SAS in einer Anwendung bereitgestellt von Azure Mobile Engagement SDK Integration migrieren. Wenn Sie von einer früheren Version migrieren, finden Sie in der Capptain-Website zunächst in 2.4.0 migrieren und wenden Sie das folgende Verfahren.

>[AZURE.IMPORTANT] Capptain und Mobile Engagement sind nicht dieselben Dienste und der unten beschriebenen Vorgehensweise zeigt nur die Clientanwendung zu migrieren. Migration im SDK in die Anwendung migrieren nicht Daten von Servern Capptain an Mobile Engagement Server.

### <a name="jar-file"></a>JAR-Datei

Ersetzen Sie `capptain.jar` von `mobile-engagement-VERSION.jar` in die `libs` Ordner.

### <a name="resource-files"></a>Ressourcendateien

Jede Ressourcendatei die bereitgestellte (vorangestellt `capptain_`) durch neue ersetzt werden (mit dem Präfix `engagement_`).

Wenn Sie diese Dateien angepasst, müssen Sie erneut anwenden die Anpassung auf die neuen Dateien **Alle Bezeichner aus den Ressourcendateien wurden ebenfalls umbenannt**.

### <a name="application-id"></a>ID der Anwendung

Engagement verwendet jetzt eine Verbindungszeichenfolge SDK Bezeichner wie der Anwendungsbezeichner konfigurieren.

Sie müssen `EngagementAgent.init` Methode in Ihrer Aktivität Launcher wie folgt:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Azure-Portal wird die Verbindungszeichenfolge für die Anwendung angezeigt.

Entfernen Sie jeden Aufruf von `CapptainAgent.configure` als `EngagementAgent.init` Methode ersetzt.

Die `appId` nicht konfigurierbar mit `AndroidManifest.xml`.

Entfernen Sie diesen Abschnitt aus der `AndroidManifest.xml` sie haben:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java API

Jeder Aufruf einer Java-Klasse unseres SDK wurde umbenannt werden; beispielsweise `CapptainAgent.getInstance(this)` muss umbenannt werden `EngagementAgent.getInstance(this)`, `extends CapptainActivity` muss umbenannt werden `extends EngagementActivity` usw....

Standardmäßige Agent-Einstellungsdateien integriert wurden, der Standarddateiname ist jetzt `engagement.agent` und der Schlüssel `engagement:agent`.

Beim Erstellen von Web Ankündigungen Javascript ist nun `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Änderungen passiert es der Dienst nicht mehr freigegeben und viele Empfänger sind nicht exportierbar mehr.

Service-Deklaration ist jetzt einfacher. beabsichtigte filtern und alle Metadaten darin entfernen und hinzufügen `exportable=false`.

Und alles wird umbenannt Engagement verwenden.

Es sieht nun folgendermaßen aus:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Soll Testprotokolle, die Metadaten jetzt wurde die Anwendung Tag als wurde umbenannt:

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Alle anderen Metadaten nur umbenannt worden, hier ist die vollständige (Kurs Umbenennen der, die Sie verwenden):

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play und SmartAd tracking wurde aus SDK werfen ohne entfernen:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

Die Reach-Aktivitäten sind jetzt wie folgt deklariert:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Haben Sie benutzerdefinierte Aktivitäten erreichen, müssen nur die beabsichtigte Maßnahmen entsprechend entweder `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` oder `com.microsoft.azure.engagement.reach.intent.action.POLL`.

Broadcast Empfänger wurden umbenannt und wir nun `exported=false`. Hier wird die vollständige Liste der Empfänger mit der neuen Spezifikation (der Kurs Umbenennen der, die Sie verwenden):

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Nachverfolgen der Empfänger wurde entfernt haben Sie diesen Abschnitt entfernen:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Hinweis Die Deklaration der Implementierung des Empfängers broadcast **EngagementMessageReceiver** geändert hat die `AndroidManifest.xml`. Ist die API zum Senden und beliebige XMPP Nachrichten von beliebigen XMPP-Elemente entfernen und die API zum Senden und Empfangen von Nachrichten zwischen Geräten entfernt wurden. Daher müssen Sie die folgenden Rückrufe aus der **EngagementMessageReceiver** Implementierung zu löschen:

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

und

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

Löschen Sie alle an **EngagementAgent** für:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

und

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Proguard Konfiguration kann betroffen mitgelieferten Regeln suchen wie:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
