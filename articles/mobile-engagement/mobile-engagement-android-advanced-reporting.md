<properties
    pageTitle="Erweiterte Optionen für Azure Mobile Engagement Android SDK"
    description="Beschreibt, wie erweiterte reporting Analytics Azure Mobile Engagement Android SDK erfassen"
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
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-engagement-on-android"></a>Erweiterte Berichte mit Engagement für Android

> [AZURE.SELECTOR]
- [Universelle Windows](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-advanced-reporting.md)

Weitere reporting-Szenarien in Ihrem Android-Anwendung beschrieben. Sie können die app im [Schnellstart](mobile-engagement-android-get-started.md) -Lernprogramm erstellt diese Optionen zuweisen.

## <a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Lernprogramm abgeschlossenen war absichtlich direkt, sondern sind erweiterte Optionen auswählen können.

## <a name="modifying-your-activity-classes"></a>Ändern der `Activity` Klassen

[Lernprogramm-Erste Schritte](mobile-engagement-android-get-started.md)Sie musste wurde zu Ihrem `*Activity` Unterklassen erben vom entsprechenden `Engagement*Activity` Klassen. Beispielsweise erweitert Ihre ältere Aktivitäten `ListActivity`, macht es die Erweiterung `EngagementListActivity`.

> [AZURE.IMPORTANT] Bei Verwendung `EngagementListActivity` oder `EngagementExpandableListActivity`, stellen Sie sicher, dass bei jedem Aufruf `requestWindowFeature(...);` erfolgt vor dem Aufruf von `super.onCreate(...);`, andernfalls Absturz.

Sie finden diese Klassen in die `src` Ordner und können in Ihr Projekt kopieren. Die Klassen sind auch **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternative Methode: Rufen Sie `startActivity()` und `endActivity()` manuell

Wenn nicht oder nicht überladen möchten Ihre `Activity` Klassen können stattdessen starten und beenden die Aktivitäten durch Aufrufen von der `EngagementAgent`Methoden direkt.

> [AZURE.IMPORTANT] Android SDK nie ruft der `endActivity()` Methode, auch wenn die Anwendung geschlossen wird (bei Android Applikationen nie geschlossen). Daher ist es *sehr* empfehlenswert, rufen Sie die `startActivity()` -Methode in der `onResume` Rückruf *Alle* Ihre Aktivitäten und `endActivity()` Methode in der `onPause()` Rückruf *aller* Aktivitäten. Dies ist die einzige Möglichkeit um sicherzustellen, dass Sessions nicht weitergegeben werden. Wenn eine Sitzung wegen trennt Engagement Dienst nie Engagement Backend (da der Dienst verbunden bleibt, solange eine Sitzung aussteht).

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

Dieses Beispiel ähnelt dem `EngagementActivity` Klasse und seine Varianten, deren Quellcode aus der `src` Ordner.

## <a name="using-applicationoncreate"></a>Application.onCreate() verwenden

Setzen Sie in Code `Application.onCreate()` und in anderen Anwendung Rückrufe für alle Ihre Anwendung Prozesse, einschließlich des Engagement-Dienstes ausgeführt wird. Es müssen nicht mehr benötigte Speicherbereiche wie Threads im Prozess des Engagements oder doppelte broadcast Empfänger oder unerwünschte Nebeneffekte.

Überschreiben Sie `Application.onCreate()`, wir empfehlen folgende Codeausschnitt Anfang der `Application.onCreate()` Funktion:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Tun das gleiche für `Application.onTerminate()`, `Application.onLowMemory()`, und `Application.onConfigurationChanged(...)`.

Sie können auch `EngagementApplication` statt erweitern `Application`: der Rückruf `Application.onCreate()` führt die Überprüfung Prozess und `Application.onApplicationProcessCreate()` nur der aktuelle Prozess nicht der Hostingdienst Engagement ist, dieselben Regeln für die Rückrufe anwenden.

## <a name="tags-in-the-androidmanifestxml-file"></a>Tags in der Datei AndroidManifest.xml

In der Service-Tag in der Datei AndroidManifest.xml die `android:label` Attribut können Sie den Namen des Dienstes Engagement auswählen Endbenutzern im Fenster "Dienste ausführen" von ihrem Handy angezeigt wird. Wenn dieses Attribut auf empfohlen `"<Your application name>Service"` (z. B. `"AcmeFunGameService"`).

Angeben der `android:process` Attribut wird sichergestellt, dass das Engagement in eigenen Prozess wird (Engagement im gleichen Prozess wie die Anwendung Ihre Haupt-UI-Thread möglicherweise langsamer).

## <a name="building-with-proguard"></a>Mit ProGuard

Wenn Sie das Anwendungspaket mit ProGuard erstellen, müssen Sie einige Klassen. Der folgende Codeausschnitt Konfiguration können:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
    }
