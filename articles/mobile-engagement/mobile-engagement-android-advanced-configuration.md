<properties
    pageTitle="Erweiterte Konfiguration für Azure Mobile Engagement Android SDK"
    description="Beschreibt die erweiterten Konfigurationsoptionen einschließlich Android Manifest Azure Mobile Engagement Android SDK"
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Erweiterte Konfiguration für Azure Mobile Engagement Android SDK

> [AZURE.SELECTOR]
- [Universelle Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-configuration.md)

Dieses Verfahren beschreibt, wie verschiedene Konfigurationsoptionen für Azure Mobile Engagement apps konfigurieren.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Erforderliche Berechtigungen
Einige Optionen erfordern bestimmte Berechtigungen die Referenz und Zeile in die jeweilige Funktion hier. Diese Berechtigungen zu AndroidManifest.xml Ihres Projekts hinzufügen, unmittelbar vor oder nach dem `<application>` Tag.

Berechtigungscode muss wie folgt aussehen, füllen Sie die entsprechenden Berechtigungen aus der folgenden Tabelle.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Berechtigung | Bei |
| ---------- | --------- |
| INTERNET | Erforderlich. Für grundlegende Berichte |
| ACCESS_NETWORK_STATE | Erforderlich. Für grundlegende Berichte |
| RECEIVE_BOOT_COMPLETED | Erforderlich. Benachrichtigung Center nach dem Neustart des Geräts anzeigen |
| WAKE_LOCK | Empfohlen. Ermöglicht das Sammeln von Daten, wenn WiFi oder bei ausgeschaltetem Bildschirm |
| VIBRIEREN | Optional. Vibration bei Benachrichtigung empfangen werden können |
| DOWNLOAD_WITHOUT_NOTIFICATION | Optional. Aktiviert die Benachrichtigung Android Überblick |
| WRITE_EXTERNAL_STORAGE | Optional. Aktiviert die Benachrichtigung Android Überblick |
| ACCESS_COARSE_LOCATION | Optional. Ermöglicht in Echtzeit Standortangabe |
| ACCESS_FINE_LOCATION | Optional. Ermöglicht reporting GPS-Speicherort |

Beginnend mit Android M [einige Berechtigungen werden zur Laufzeit verwaltet](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Verwenden Sie bereits ``ACCESS_FINE_LOCATION``, müssen Sie auch ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Android Manifest Konfigurationsoptionen

### <a name="crash-report"></a>Crash-Bericht

Um Berichte zu deaktivieren, fügen Sie diesen Code zwischen die `<application>` und `</application>` Tags:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst-Schwellenwert

Standardmäßig protokolliert der Engagement Berichte in Echtzeit. Wenn Ihre Anwendungsprotokolle häufig variieren, ist es besser, Protokolle Puffer und sie gleichzeitig auf einem regulären Zeitbasis ("burst-Modus" genannt). Hierzu fügen Sie diesen Code zwischen die `<application>` und `</application>` Tags:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Burst-Modus leicht erhöht die Lebensdauer der Batterie jedoch wirkt sich auf das Engagement: alle Sitzungen und Aufträge Dauer Burst-Schwellenwert (also Sessions und kürzer als Burst-Schwellenwert werden Aufträge) gerundet. Burst-Schwellenwert sollte nicht mehr als 30000 (30).

### <a name="session-timeout"></a>Sitzungstimeout

 Zum Ende einer Aktivität der Taste **Home** oder **wieder** im Leerlauf Telefon oder in einer anderen Anwendung wechseln. Standardmäßig wird eine Sitzung zehn Sekunden nach dem Ende der letzten Aktivität beendet. Eine Sitzung teilen vermeiden jedem Benutzer wird beendet und die Anwendung schnell wieder das passieren der Benutzer ein Bild auswählt überprüft wird, kann eine Benachrichtigung usw.. Sie sollten diesen Parameter zu ändern. Hierzu fügen Sie diesen Code zwischen die `<application>` und `</application>` Tags:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Protokoll melden deaktivieren

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
-   Die `engagement:agent:settings:mode` Schlüssel zur Definition des Modus der freigegebenen Einstellungsdatei. Verwenden Sie den gleichen Modus in der `PreferenceActivity`. Der Modus muss als Zahl übergeben werden: Wenn Sie eine Kombination aus Konstanten im Code verwenden, überprüfen Sie den Wert.

Engagement immer verwendet die `engagement:key` boolesche Schlüssel in der Einstellungsdatei für die Verwaltung dieser Einstellung.

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
