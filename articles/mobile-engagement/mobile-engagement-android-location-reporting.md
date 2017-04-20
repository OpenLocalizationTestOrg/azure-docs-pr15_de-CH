<properties
    pageTitle="Speicherort für Android SDK Azure Mobile Engagement"
    description="Beschreibt die Position für Azure Mobile Engagement Android SDK konfigurieren"
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Speicherort für Android SDK Azure Mobile Engagement

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-integrate-engagement.md)

Dieses Thema beschreibt die Lage für Android-Anwendung.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Standortangabe

Speicherorte gemeldet werden, Sie sollten einige Zeilen der Konfiguration (zwischen dem `<application>` und `</application>` Tags).

### <a name="lazy-area-location-reporting"></a>Träge Bereich Standortangabe

Träge Bereich Position reporting ermöglicht reporting Land, Region und Ort Geräte. Diese Standortangabe verwendet nur Netzwerkadressen (basierend auf Zelle ID oder WIFI). Bereich wird höchstens einmal pro Sitzung gemeldet. GPS wird nicht verwendet und daher diese Position Bericht hat geringe Auswirkung auf der Batterie.

Gemeldete Bereichen werden verwendet, um geografische Statistiken zu Benutzern, Sitzungen, Ereignissen und Fehlern berechnen. Sie können auch als Kriterium in Reach-Kampagnen verwendet werden.

Sie aktivieren die träge Bereichsposition nach der Konfiguration weiter oben in diesem Verfahren:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen eine Berechtigung Speicherort angeben. Dieser Code verwendet ``COARSE`` Berechtigung:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Wenn Ihre Anwendung erfordert, können Sie ``ACCESS_FINE_LOCATION`` statt.

### <a name="real-time-location-reporting"></a>Der aktuelle Ort reporting

Echtzeitort reporting ermöglicht den Breiten- und Längengrad Geräte reporting. Standardmäßig verwendet diese Standortangabe nur Netzwerkadressen basierend auf Zelle ID oder WIFI. Die Berichterstattung ist nur aktiv, wenn die Anwendung (z. B. während einer Sitzung) im Vordergrund ausgeführt wird.

In Echtzeit sind *keine* Statistik berechnet. Ihr einziger Zweck ist die Verwendung von Echtzeit-geofencing \<erreichen Publikum Geofencing\> Kriterium in Reichweite Kampagnen.

Fügen Sie eine Codezeile aktivieren Echtzeitort reporting hinzu, Verbindungszeichenfolge Engagement in der Launcher-Aktivität festlegen. Das Ergebnis sieht folgendermaßen aus:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS-basierte Berichte

Standardmäßig verwendet das Echtzeit Standortangabe nur Netzwerk-Standorten. Aktivieren des GPS-Standorten, die wesentlich genauere verwenden Sie das Configuration-Objekt:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen die folgende Berechtigungen hinzufügen, wenn:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Hintergrund reporting

Standardmäßig ist in Echtzeit Standortangabe nur aktiv beim Ausführen der Anwendung im Vordergrund (z. B. während einer Sitzung). Verwenden Sie zum Aktivieren der auch im Hintergrund, dieses Configuration-Objekt:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Wenn die Anwendung im Hintergrund läuft nur Netzwerk-Standorten gemeldet, auch wenn das GPS aktiviert.

Wenn der Benutzer das Gerät neu gestartet, wird Hintergrund Standortbericht beendet. Automatisch beim Systemstart starten soll, fügen Sie diesen Code hinzu.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Sie müssen die folgende Berechtigungen hinzufügen, wenn:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M Berechtigungen

Android M, einige Berechtigungen werden zur Laufzeit verwaltet und Benutzer Zustimmung.

Wenn Android API-Ebene 23 abzielen, sind die Laufzeitberechtigungen für Neuinstallationen app standardmäßig deaktiviert. Andernfalls sind sie standardmäßig aktiviert.

Sie können diese Berechtigungen Menü Einstellungen Gerät aktivieren. Berechtigungen im Systemmenü ausschalten bricht die Hintergrundprozesse Anwendung ein Systemverhalten und hat keine Auswirkung auf Push im Hintergrund erhalten.

Im Bereich Mobile Engagement Position Berichte werden die Berechtigungen, die Genehmigung zur Laufzeit:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Ein standard-Dialogfeld Benutzer fordern Sie Berechtigungen an. Stimmt der Benutzer sagen ``EngagementAgent`` zu dieser Änderung Rechnung in Echtzeit. Andernfalls wird die Änderung wieder verarbeitet der Benutzer die Anwendung startet.

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
