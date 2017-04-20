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

#<a name="how-to-integrate-engagement-on-android"></a>Integration der Engagement für Android

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

Dieses Verfahren beschreibt die einfachsten Projekts Analyse und Überwachung der Android Anwendung aktivieren.

> [AZURE.IMPORTANT] Die minimale Android SDK API-Ebene müssen 10 oder höher (Android 2.3.3 oder höher).

Die folgenden Schritte sind ausreichend Bericht alle Statistiken über Benutzer, Sitzung, Aktivitäten, Abstürze und technische berechnet erforderlichen Protokolle aktiviert. Bericht über Protokolle benötigt weitere Statistiken wie Ereignisse, Fehler und Aufträge berechnen erfolgt manuell mit Engagement-API (siehe [wie der erweiterte Mobile Engagement tagging API Android](mobile-engagement-android-use-engagement-api.md) seit Anwendung abhängig sind.

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>Engagement-SDK und Service in Android Projekt einbetten

[Hier](https://aka.ms/vq9mfn) bekommen Android SDK downloaden `mobile-engagement-VERSION.jar` und in der `libs` Projektordner Android (Libs-Ordner erstellen, falls noch nicht vorhanden ist).

> [AZURE.IMPORTANT]
> Wenn Sie das Anwendungspaket mit ProGuard erstellen, müssen Sie einige Klassen. Der folgende Codeausschnitt Konfiguration können:
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Geben Sie die Verbindungszeichenfolge Engagement durch Aufrufen der folgenden Methode in der Launcher-Aktivität:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Azure-Portal wird die Verbindungszeichenfolge für die Anwendung angezeigt.

-   Nicht vorhanden, fügen Sie folgenden Android Berechtigungen (vor der `<application>` Tag):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   Fügen Sie den folgenden Abschnitt (zwischen dem `<application>` und `</application>` Tags):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Ändern `<Your application name>` durch den Namen der Anwendung.

> [AZURE.TIP] Die `android:label` Attribut können Sie den Namen des Dienstes Engagement auswählen wie Endbenutzer im Fenster "Dienste ausführen" von ihrem Handy angezeigt wird. Es wird empfohlen, dieses Attribut festgelegt `"<Your application name>Service"` (z.B. `"AcmeFunGameService"`).

Angeben der `android:process` Attribut wird sichergestellt, dass der Engagement-Dienst, in einen eigenen Prozess ausgeführt wird (Engagement im gleichen Prozess wie die Anwendung Ihre Haupt-UI-Thread möglicherweise langsamer).

> [AZURE.NOTE] Setzen Sie in Code `Application.onCreate()` und andere Anwendung Rückrufe für alle Ihre Anwendung Prozesse, einschließlich der Engagement. Es möglicherweise unerwünschte Nebenwirkungen (wie nicht benötigte Speicherbereiche und Threads im Prozess des Engagements, doppelte broadcast Empfänger oder Dienste).

Überschreiben Sie `Application.onCreate()`, es wird empfohlen, fügen Sie den folgenden Codeausschnitt Anfang der `Application.onCreate()` Funktion:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Tun das gleiche für `Application.onTerminate()`, `Application.onLowMemory()` und `Application.onConfigurationChanged(...)`.

Sie können auch `EngagementApplication` statt erweitern `Application`: der Rückruf `Application.onCreate()` führt die Überprüfung Prozess und `Application.onApplicationProcessCreate()` nur der aktuelle Prozess nicht der Hostingdienst Engagement ist, dieselben Regeln für die Rückrufe anwenden.

##<a name="basic-reporting"></a>Grundlegende Berichterstattung

### <a name="recommended-method-overload-your-activity-classes"></a>Empfohlen: Überladen der `Activity` Klassen

Um Bericht über alle erforderlichen Engagement Benutzer Sessions, Aktivitäten, Abstürze und technische Statistiken berechnet Protokolle aktivieren, Sie müssen nur alle Ihre `*Activity` untergeordnete Klassen erben von der entsprechenden `Engagement*Activity` Klassen (z.B. Wenn Ihre ältere Aktivität erweitert `ListActivity`, stellen erweitert `EngagementListActivity`).

**Ohne:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Mit:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Bei Verwendung `EngagementListActivity` oder `EngagementExpandableListActivity`, stellen Sie sicher, dass bei jedem Aufruf `requestWindowFeature(...);` erfolgt vor dem Aufruf von `super.onCreate(...);`, da andernfalls ein Absturz auftritt.

Sie finden diese Klassen in die `src` Ordner und können in Ihr Projekt kopieren. Die Klassen sind auch **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternative Methode: Rufen Sie `startActivity()` und `endActivity()` manuell

Wenn nicht oder nicht überladen möchten Ihre `Activity` Klassen können stattdessen starten und beenden die Aktivitäten durch Aufrufen von `EngagementAgent`Methoden direkt.

> [AZURE.IMPORTANT] Android SDK nie ruft der `endActivity()` Methode, auch wenn die Anwendung geschlossen wird (bei Android Applikationen nie geschlossen). Daher ist es *sehr* empfehlenswert, rufen Sie die `startActivity()` -Methode in der `onResume` Rückruf *Alle* Ihre Aktivitäten und `endActivity()` Methode in der `onPause()` Rückruf *aller* Aktivitäten. Dies ist die einzige Möglichkeit um sicherzustellen, dass Sessions nicht ausgelaufen ist. Wenn eine Sitzung wegen wird der Engagement-Dienst nie Engagement Backend trennen (da der Dienst verbunden bleibt, solange eine Sitzung aussteht).

Hier ist ein Beispiel:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

Dieses Beispiel ähnelt der `EngagementActivity` Klasse und seine Varianten, deren Quellcode aus der `src` Ordner.

##<a name="test"></a>Test

Nun überprüfen Sie die Integration von der mobilen Anwendung in einem Emulator oder Gerät und überprüfen, dass eine Sitzung auf der Registerkarte Monitor registriert.

In den nächsten Abschnitten sind optional.

##<a name="location-reporting"></a>Standortangabe

Speicherorte gemeldet werden, Sie sollten einige Zeilen der Konfiguration (zwischen dem `<application>` und `</application>` Tags).

### <a name="lazy-area-location-reporting"></a>Träge Bereich Standortangabe

Träge Bereich Position reporting ermöglicht, Land, Region und Ort Geräte zugeordnet. Diese Standortangabe verwendet nur Netzwerkadressen (basierend auf Zelle ID oder WIFI). Bereich wird höchstens einmal pro Sitzung gemeldet. GPS wird niemals verwendet und daher dieser Position Bericht verfügt über wenige (nicht, Nein) Auswirkung auf den Akku.

Gemeldete Bereichen werden verwendet, um geografische Statistiken zu Benutzern, Sitzungen, Ereignissen und Fehlern berechnen. Sie können auch als Kriterium in Reach-Kampagnen verwendet werden.

Um träge Bereich Position Berichte aktivieren, können Sie dies tun, mit der Konfiguration, die weiter oben in diesem Verfahren:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen die folgende Berechtigungen hinzufügen, wenn:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Oder Sie können halten ``ACCESS_FINE_LOCATION`` Wenn Sie bereits in der Anwendung verwenden.

### <a name="real-time-location-reporting"></a>Echtzeit-reporting Speicherort

Echtzeit Position reporting ermöglicht den Breiten- und Längengrad zugeordnete Geräte melden. Standardmäßig Standortangabe nur verwendet die Netzwerkadressen (basierend auf Zelle ID oder WIFI) und die Berichterstattung ist nur aktiv, wenn die Anwendung (z. B. während einer Sitzung) im Vordergrund ausgeführt wird.

Echtzeit sind *keine* Statistik berechnet. Ihr einziger Zweck ist die Verwendung von Echtzeit-geofencing \<erreichen Publikum Geofencing\> Kriterium in Reichweite Kampagnen.

Um Echtzeit Position Berichte aktivieren, können Sie dies tun, mit der Konfiguration, die weiter oben in diesem Verfahren:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen die folgende Berechtigungen hinzufügen, wenn:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Oder Sie können halten ``ACCESS_FINE_LOCATION`` Wenn Sie bereits in der Anwendung verwenden.

#### <a name="gps-based-reporting"></a>GPS-basierte Berichte

Standardmäßig verwendet das Echtzeit Standortangabe nur Netzwerk Speicherorte. Aktivieren von GPS-basierten Standorten (die viel genauer sind), verwenden Sie das Konfigurationsobjekt:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen die folgende Berechtigungen hinzufügen, wenn:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Hintergrund reporting

Standardmäßig ist Echtzeit Standortangabe nur aktiv beim Ausführen der Anwendung im Vordergrund (z. B. während einer Sitzung). Verwenden Sie zum Aktivieren der auch im Hintergrund, das Configuration-Objekt:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Wenn die Anwendung im Hintergrund ausgeführt wird nur Network Orten werden gemeldet, auch wenn das GPS aktiviert.

Standortbericht Hintergrund angehalten, wenn der Benutzer das Gerät neu gestartet, können Sie dies machen es automatisch beim Systemstart starten:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Sie müssen die folgende Berechtigungen hinzufügen, wenn:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M Berechtigungen

Android M, einige Berechtigungen werden zur Laufzeit verwaltet und Benutzer benötigt.

Die Laufzeitberechtigungen werden standardmäßig für neue Installationen von app ausgeschaltet werden Wenn Android API-Ebene 23 Ziel. Andernfalls wird es standardmäßig aktiviert.

Der Benutzer kann diese Berechtigungen Menü Einstellungen Gerät aktivieren. Berechtigungen im Menü Deaktivieren der Anwendung Hintergrundprozesse beendet, ein Systemverhalten und hat keine Auswirkung auf Push im Hintergrund empfangen.

Im Bereich Mobile Engagement werden die Berechtigungen, die Genehmigung zur Laufzeit:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(nur bei Android API-Ebene 23 für diese Ausrichtung)

Der externe Speicher wird nur für Reichweite Überblick Funktion verwendet. Wenn Sie finden Benutzer aufzufordern, diese Berechtigung störend sein, entfernen Sie es nur für Mobile jedoch Kosten deaktivieren Überblick-Funktion verwendet.

Für die Features Speicherort sollte verfügen Benutzer über ein standard-Dialogfeld anfordern. Stimmt der Benutzer müssen Sie sagen ``EngagementAgent`` zu dieser Änderung Rechnung in Echtzeit (andernfalls die Änderung erfolgt der Benutzer die Anwendung startet beim nächsten).

Hier ist ein Codebeispiel verwendet eine Aktivität der Anwendung Berechtigungen anfordern und das Ergebnis positiv auf ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Erweiterte Berichte

Optional, wenn Sie anwendungsspezifische Ereignisse, Fehler und Aufträge melden möchten, müssen Sie über die Methoden der Engagement-API verwenden die `EngagementAgent` Klasse. Ein Objekt dieser Klasse kann der Tabellenname durch Aufrufen der `EngagementAgent.getInstance()` statische Methode.

Engagement-API können alle Projekts erweiterten Funktionen und in der Engagement-API auf Android verwenden (sowie in der technischen Dokumentation der `EngagementAgent` Klasse).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Erweiterte Konfiguration (AndroidManifest.xml)

### <a name="wake-locks"></a>Sperren aktivieren

Möchten Sie sicher sein, dass Statistiken gesendet werden, in Echtzeit Verwendung Wifi oder der Bildschirm ausgeschaltet ist, fügen Sie die folgende optionale Berechtigungen:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Crash-Bericht

Wenn Sie Fehlerberichte deaktivieren möchten, fügen Sie diese (zwischen dem `<application>` und `</application>` Tags):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst-Schwellenwert

Standardmäßig protokolliert der Engagement Berichte in Echtzeit. Meldet die Anwendung Protokolle häufig, empfiehlt sich die Protokolle Puffer und sie gleichzeitig auf einem regulären Zeitbasis (Dies wird "Burstmodus" bezeichnet). Hierzu fügen Sie (zwischen dem `<application>` und `</application>` Tags):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Der Burstmodus leicht die Akkulaufzeit erhöhen jedoch wirkt sich auf das Engagement: alle Sitzungen und Aufträge Dauer Burst-Schwellenwert (also Sessions und kürzer als Burst-Schwellenwert werden Aufträge) gerundet. Es wird empfohlen, einen Burst-Schwellenwert nicht mehr als 30000 (30) verwenden.

### <a name="session-timeout"></a>Sitzungstimeout

Eine Sitzung ist beendet 10 s nach dem Ende der letzten Aktivität (was in der Regel durch Drücken der Taste Home oder zurück, Telefon im Leerlauf festlegen oder in einer anderen Anwendung wechseln). Dies ist eine Sitzung teilen jeder Zeit Benutzer beenden und zurück an die Anwendung sehr schnell zu vermeiden (die passieren, wenn er ein Bild auswählen prüfen können Benachrichtigung usw.). Sie sollten diesen Parameter zu ändern. Hierzu fügen Sie (zwischen dem `<application>` und `</application>` Tags):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Protokoll melden deaktivieren

### <a name="using-a-method-call"></a>Ein Methodenaufruf

Engagement zu Protokolle senden möchten, können Sie aufrufen:

            EngagementAgent.getInstance(context).setEnabled(false);

Dieser Aufruf ist persistent: freigegebene Einstellungsdatei verwendet.

Wenn Engagement aktiv ist, wenn Sie diese Funktion aufrufen, dauert es eine Minute für den Dienst zu beenden. Aber es den Dienst überhaupt nächsten nicht starten starten die Anwendung.

Können Protokolldateien wieder dieselbe Funktion mit reporting `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integration in Ihre eigenen`PreferenceActivity`

Anstatt diese Funktion können Sie auch diese Einstellung integrieren, direkt in die vorhandene `PreferenceActivity`.

Konfigurieren Engagement Ihrer Voreinstellungsdatei (mit den gewünschten Modus) in der `AndroidManifest.xml` Datei mit `application meta-data`:

-   Die `engagement:agent:settings:name` Schlüssel wird verwendet, um den Namen der freigegebenen Voreinstellungsdatei definieren.
-   Die `engagement:agent:settings:mode` Schlüssel definieren den Modus der freigegebenen Einstellungsdatei verwendet wird, verwenden Sie den gleichen Modus in der `PreferenceActivity`. Der Modus muss als Zahl übergeben werden: Wenn Sie eine Kombination aus Konstanten im Code verwenden, überprüfen Sie den Wert.

Engagement immer verwenden die `engagement:key` boolesche Schlüssel in der Einstellungsdatei für die Verwaltung dieser Einstellung.

Im folgenden Beispiel `AndroidManifest.xml` zeigt die Standardwerte:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Hinzufügen einer `CheckBoxPreference` im Layout Einstellung wie folgt:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
