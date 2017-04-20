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

#<a name="how-to-integrate-engagement-reach-on-android"></a>Engagement zu Android Integration

> [AZURE.IMPORTANT] Integration beschrieben zu integrieren wie Android Dokument vor dieser Anleitung folgen.

##<a name="standard-integration"></a>Standard-integration

Die Reach-SDK erfordert **Android Support Library (v4)**.

Am schnellsten Projekt in **Eclipse** Bibliothek hinzufügen `Right click on your project -> Android Tools -> Add Support Library...`.

Wenn Sie Eclipse verwenden, können Sie die Anleitung lesen [hier].

Kopieren Sie Reichweite Ressourcendateien aus dem SDK in Ihr Projekt:

-   Kopieren Sie die Dateien aus der `res/layout` Ordner im Lieferumfang des SDK in die `res/layout` Ordner der Anwendung.
-   Kopieren Sie die Dateien aus der `res/drawable` Ordner im Lieferumfang des SDK in die `res/drawable` Ordner der Anwendung.

Bearbeiten der `AndroidManifest.xml` Datei:

-   Fügen Sie den folgenden Abschnitt (zwischen dem `<application>` und `</application>` Tags):

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
            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
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

-   Sie benötigen diese Berechtigung systembenachrichtigungen wiedergeben, die beim Start nicht angeklickt wurden (andernfalls sie bleiben auf der Festplatte, aber nicht mehr angezeigt werden, müssen Sie dies).

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

-   Geben Sie ein Symbol für Benachrichtigungen (beide in app und welche) kopieren und Bearbeiten von Abschnitt (zwischen dem `<application>` und `</application>` Tags):

            <meta-data android:name="engagement:reach:notification:icon" android:value="<name_of_icon_WITHOUT_file_extension_and_WITHOUT_'@drawable/'>" />

> [AZURE.IMPORTANT] Dieser Abschnitt ist **obligatorisch** , wenn verwendet systembenachrichtigungen beim Erreichen Kampagnen erstellen. Android verhindert, dass systembenachrichtigungen ohne Symbole angezeigt werden. Also wenn Sie diesen Abschnitt überspringen, wird Ihre Endbenutzer erhalten nicht.

-   Beim Erstellen von Kampagnen mit System benachrichtigt mit Überblick müssen die folgenden Berechtigungen (nach der `</application>` Tag) fehlen:

            <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
            <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

  -   Android M und die Anwendung Android API-Ebene 23 oder höher abzielt ``WRITE_EXTERNAL_STORAGE`` Berechtigung benutzergenehmigung erforderlich. Lesen Sie [diesen Abschnitt](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

-   Für System-Benachrichtigung können Sie auch in Reichweite Kampagne angeben, wenn das Klingeln oder Vibrieren sollte. Dafür müssen Sie sicherstellen, dass Sie folgende Berechtigung deklariert (nach der `</application>` Tag):

            <uses-permission android:name="android.permission.VIBRATE" />

    Ohne diese Berechtigung verhindert Android System Benachrichtigung wird angezeigt, wenn der Ring oder die Option Vibrieren erreichen Kampagnen-Manager überprüft.

-   Wenn Sie die Anwendung unter Verwendung **ProGuard** und Fehler im Zusammenhang mit Android Support Library oder JAR-Engagement, fügen Sie folgende Zeilen zu Ihrem `proguard.cfg` Datei:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }

## <a name="native-push"></a>Native Push

Reach-Modul konfiguriert wird, müssen Sie konfigurieren systemeigenen Push zu Kampagnen auf dem Gerät angezeigt.

Wir unterstützen Android zwei Dienste:

  - Google Geräte: Verwenden der [wie integrieren GCM mit Engagement](mobile-engagement-android-gcm-integrate.md) Anleitung [Google Cloud Messaging] .
  - Amazon-Geräte: Verwenden der [wie integrieren ADM mit Engagement](mobile-engagement-android-adm-integrate.md) Anleitung [Amazon Gerät Messaging] .

Möchten Sie Amazon und Google Play möglich alles innerhalb 1 AndroidManifest.xml/APK Entwicklung Zielgeräten. Aber bei Amazon können sie Ihre Anwendung ablehnen, wenn sie GCM Code.

Sie sollten in diesem Fall mehrere APKs verwenden.

**Die Anwendung kann jetzt empfangen und Reichweite Kampagnen anzeigen.**

##<a name="how-to-handle-data-push"></a>Umgang mit datenpush

### <a name="integration"></a>Integration

Soll die Anwendung erreichen Daten Push erhalten haben, erstellen Sie eine Unterklasse von `com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver` und in den `AndroidManifest.xml` Datei (zwischen der `<application>` oder `</application>` Tags):

            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>

Dann können Sie überschreiben die `onDataPushStringReceived` und `onDataPushBase64Received` Rückrufe. Hier ist ein Beispiel:

            public class MyDataPushReceiver extends EngagementReachDataPushReceiver
            {
              @Override
              protected Boolean onDataPushStringReceived(Context context, String category, String body)
              {
                Log.d("tmp", "String data push message received: " + body);
                return true;
              }

              @Override
              protected Boolean onDataPushBase64Received(Context context, String category, byte[] decodedBody, String encodedBody)
              {
                Log.d("tmp", "Base64 data push message received: " + encodedBody);
                // Do something useful with decodedBody like updating an image view
                return true;
              }
            }

### <a name="category"></a>Kategorie

Der Kategorieparameter ist beim Erstellen einer Kampagne Daten optional und legt Sie Filterdaten kann. Dies ist nützlich, wenn mehrere broadcast Empfänger Behandlung verschiedener Daten drückt, oder wenn Sie verschiedene push von `Base64` Daten und Typ identifizieren, bevor sie analysieren möchten.

### <a name="callbacks-return-parameter"></a>Rückgabeparameter Rückrufe

Es folgen einige Richtlinien zur ordnungsgemäßen Behandlung den Rückgabeparameter der `onDataPushStringReceived` und `onDataPushBase64Received`:

-   Broadcast Empfänger sollte zurückgeben `null` im Rückruf, wenn sie nicht wissen, wie einen datenpush behandeln. Die Kategorie verwenden um festzustellen, ob Receivers broadcast datenpush verarbeiten soll.
-   Einen broadcast Empfänger zurückgeben sollte `true` Rückruf datenpush akzeptiert.
-   Einen broadcast Empfänger zurückgeben sollte `false` im Rückruf, wenn es den datenpush erkennt, aber aus irgendeinem Grund verwirft. Beispielsweise, `false` Wenn die empfangenen Daten sind ungültig.
-   Wenn eine-Empfänger gibt Broadcast `true` anderen einer while `false` für die gleichen datenpush ist das Verhalten nicht definiert, sollten Sie nie tun.

Der Rückgabetyp wird nur für die Statistik erreichen:

-   `Replied`wird erhöht, wenn ein broadcast Empfänger entweder `true` oder `false`.
-   `Actioned`wird erhöht, wenn der broadcast Empfänger zurückgegeben `true`.

##<a name="how-to-customize-campaigns"></a>Anpassen von Kampagnen

Ändern Sie zum Anpassen der Kampagnen erreichen SDK bereitgestellten Layouts.

Sie sollten alle Bezeichner in Layouts und halten Ansichtstypen, die speziell für Text und Bildansichten einen Bezeichner verwenden. Einige Ansichten werden nur zum ein- oder ausblenden Bereichen, ihres Typs geändert werden kann. Überprüfen Sie den Quellcode auf den bereitgestellten Layouts ändern soll.

### <a name="notifications"></a>Benachrichtigung

Es gibt zwei Arten von Benachrichtigungen: System und in app Benachrichtigungen die verschiedenen Layout-Dateien verwenden.

#### <a name="system-notifications"></a>System-Benachrichtigung

System-Benachrichtigung anpassen müssen Sie **Kategorien**verwenden. Sie können [Kategorien](#categories)wechseln.

#### <a name="in-app-notifications"></a>In app-Benachrichtigungen

Eine Benachrichtigung in app wird standardmäßig eine Ansicht der aktuellen Aktivität Benutzeroberfläche Android Methode dynamisch hinzugefügt wird `addContentView()`. Dies ist eine Benachrichtigung Überlagerung bezeichnet. Benachrichtigung Overlays sind für eine schnelle Integration, da sie nicht erfordern, dass Sie ein Layout in der Anwendung ändern.

Ändern Sie das Aussehen der Benachrichtigung Overlays können Sie einfach die Datei ändern `engagement_notification_area.xml` an Ihre Bedürfnisse.

> [AZURE.NOTE] Die Datei `engagement_notification_overlay.xml` , die zur Erstellung eine Überlagerung Benachrichtigung ist die Datei `engagement_notification_area.xml`. Sie können es (wie der Infobereich in der Überlagerung Positionierung) Ihren Bedürfnissen entsprechend anpassen.

##### <a name="include-notification-layout-as-part-of-an-activity-layout"></a>Enthalten Sie Benachrichtigung Layout als Teil einer Aktivität Layouts

Overlays können sind für eine schnelle Integration jedoch unpraktisch sein oder Nebeneffekte in besonderen Fällen. Overlay-System kann auf ein, damit Nebenwirkungen für besondere Aktivitäten verhindern angepasst werden.

Sie können unsere Benachrichtigung Layout in Ihr vorhandenes Layout durch die Anweisung Android **enthalten** sind. Im folgenden ist ein Beispiel einer geänderten `ListActivity` Layout mit nur einer `ListView`.

**Vor der Integration des Projekts:**

            <?xml version="1.0" encoding="utf-8"?>
            <ListView
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@android:id/list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent" />

**Engagement-Integration:**

            <?xml version="1.0" encoding="utf-8"?>
            <LinearLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <ListView
                android:id="@android:id/list"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:layout_weight="1" />

              <include layout="@layout/engagement_notification_area" />

            </LinearLayout>

In diesem Beispiel haben wir einen übergeordneten Container hinzugefügt, da das ursprüngliche Layout eine Listenansicht als Element der obersten Ebene verwendet. Wir zusätzlich `android:layout_weight="1"` zum Hinzufügen eine Ansicht unten eine Liste mit konfiguriert `android:layout_height="fill_parent"`.

Engagement erreichen SDK erkennt automatisch, dass Benachrichtigung Layout in dieser Aktivität enthalten und keine Überlagerung für diese Aktivität hinzufügen.

> [AZURE.TIP] Verwenden Sie ein ListActivity in der Anwendung verhindert sichtbar Reichweite überlagern Sie auf mehr Elemente in der Listenansicht geklickt. Dies ist ein bekanntes Problem. Um dieses Problem zu beheben empfehlen Benachrichtigung Layout im eigenen Layout der Liste Aktivität wie im vorherigen Beispiel einbetten.

##### <a name="disabling-application-notification-per-activity"></a>Deaktivieren der Anwendung Benachrichtigung pro Aktivität

Wenn Sie nicht die Überlagerung an Ihre Aktivität hinzugefügt werden, und wenn Ihr eigenes Layout Benachrichtigung Layout angeben, können die Überlagerung für diese Aktivität in Deaktivieren der `AndroidManifest.xml` durch Hinzufügen einer `meta-data` Abschnitt wie folgt:

            <activity android:name="SplashScreenActivity">
              <meta-data android:name="engagement:notification:overlay" android:value="false"/>
            </activity>

#### <a name="categories"></a>Kategorien

Beim Ändern der bereitgestellten Layouts Ändern der Darstellung von der Benachrichtigung. Kategorien können Sie verschiedene gezielte sucht (möglicherweise Verhalten) für Benachrichtigungen definieren. Eine Kategorie kann beim Erstellen einer Kampagne Reichweite angegeben. Denken Sie daran Kategorien können auch Ankündigungen und Umfragen, anpassen, die in diesem Dokument beschrieben.

Um eine Kategorie Handler für Benachrichtigungen zu registrieren, müssen Sie einen Aufruf hinzufügen, wenn die Anwendung initialisiert wird.

> [AZURE.IMPORTANT] Lesen Sie die Warnung zum Android: Prozess-Attribut \<Android-Sdk-Vertriebsprozess\> wie Android Thema fortfahren zu integrieren.

Angenommen Sie vorherige Warnung bestätigt und eine Unterklasse von `EngagementApplication`:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), "myCategory");
              }
            }

Die `MyNotifier` Objekt ist die Implementierung der Kategorie Benachrichtigungshandler. Ist entweder eine Implementierung der `EngagementNotifier` -Schnittstelle oder eine Unterklasse die Implementierung: `EngagementDefaultNotifier`.

Beachten Sie, dass der gleiche Notifier können mehrere Kategorien folgendermaßen registrieren:

            reachAgent.registerNotifier(new MyNotifier(this), "myCategory", "myAnotherCategory");

Um die Kategorie Implementierung ersetzen, können Sie die Implementierung wie im folgenden Beispiel registrieren:

            public class MyApplication extends EngagementApplication
            {
              @Override
              protected void onApplicationProcessCreate()
              {
                // [...] other init
                EngagementReachAgent reachAgent = EngagementReachAgent.getInstance(this);
                reachAgent.registerNotifier(new MyNotifier(this), Intent.CATEGORY_DEFAULT); // "android.intent.category.DEFAULT"
              }
            }

Die aktuelle Kategorie in einem Handler verwendet als Parameter in den meisten Methoden können in überschreiben übergeben `EngagementDefaultNotifier`.

Erfolgt entweder als ein `String` Parameter oder indirekt in ein `EngagementReachContent` Objekt hat einen `getCategory()` Methode.

Ändern die meisten Erstellung Benachrichtigungsvorgang neu definieren Methoden auf `EngagementDefaultNotifier`, erweiterte Anpassung jederzeit in der technischen Dokumentation und den Quellcode ansehen.

##### <a name="in-app-notifications"></a>In app-Benachrichtigungen

Wenn Sie alternative Layouts für eine bestimmte Kategorie verwenden möchten, können Sie diese wie im folgenden Beispiel implementieren:

            public class MyNotifier extends EngagementDefaultNotifier
            {
              public MyNotifier(Context context)
              {
                super(context);
              }

              @Override
              protected int getOverlayLayoutId(String category)
              {
                return R.layout.my_notification_overlay;
              }


              @Override
              public Integer getOverlayViewId(String category)
              {
                return R.id.my_notification_overlay;
              }

              @Override
              public Integer getInAppAreaId(String category)
              {
                return R.id.my_notification_area;
              }
            }

**Beispiel für `my_notification_overlay.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <RelativeLayout
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/my_notification_overlay"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <include layout="@layout/my_notification_area" />

            </RelativeLayout>

Wie Sie sehen, unterscheidet der Überlagerung Ansichtsbezeichner Standard. Es ist wichtig, dass jedes Layout einen eindeutigen Bezeichner für Overlays verwenden.

**Beispiel für `my_notification_area.xml` :**

            <?xml version="1.0" encoding="utf-8"?>
            <merge
              xmlns:android="http://schemas.android.com/apk/res/android"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent">

              <RelativeLayout
                android:id="@+id/my_notification_area"
                android:layout_width="fill_parent"
                android:layout_height="64dp"
                android:layout_alignParentTop="true"
                android:background="#B000">

                <LinearLayout
                  android:orientation="horizontal"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent"
                  android:gravity="center_vertical">

                  <ImageView
                    android:id="@+id/engagement_notification_icon"
                    android:layout_width="48dp"
                    android:layout_height="48dp" />

                  <LinearLayout
                    android:id="@+id/engagement_notification_text"
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_weight="1"
                    android:gravity="center_vertical">

                    <TextView
                      android:id="@+id/engagement_notification_title"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:singleLine="true"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Medium" />

                    <TextView
                      android:id="@+id/engagement_notification_message"
                      android:layout_width="fill_parent"
                      android:layout_height="wrap_content"
                      android:maxLines="2"
                      android:ellipsize="end"
                      android:textAppearance="@android:style/TextAppearance.Small" />

                  </LinearLayout>

                  <ImageView
                    android:id="@+id/engagement_notification_image"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:adjustViewBounds="true" />

                  <ImageButton
                    android:id="@+id/engagement_notification_close_area"
                    android:visibility="invisible"
                    android:layout_width="wrap_content"
                    android:layout_height="fill_parent"
                    android:src="@android:drawable/btn_dialog"
                    android:background="#0F00" />

                </LinearLayout>

                <ImageButton
                  android:id="@+id/engagement_notification_close"
                  android:layout_width="wrap_content"
                  android:layout_height="fill_parent"
                  android:layout_alignParentRight="true"
                  android:src="@android:drawable/btn_dialog"
                  android:background="#0F00" />

              </RelativeLayout>

            </merge>

Wie Sie sehen, unterscheidet der Benachrichtigung Bereich Ansichtsbezeichner Standard. Es ist wichtig, dass jedes Layout einen eindeutigen Bezeichner für Benachrichtigung Bereiche.

Dieses einfache Beispiel Kategorie macht Anwendung (oder in app) Benachrichtigung am oberen Bildschirmrand angezeigt. Wir im Infobereich selbst verwendeten standard-IDs nicht ändern.

Wenn Sie ändern möchten, müssen Sie definieren die `EngagementDefaultNotifier.prepareInAppArea` Methode. Empfahl der technischen Dokumentation und den Quellcode des `EngagementNotifier` und `EngagementDefaultNotifier` Wenn der erweiterten Anpassung möchten.

##### <a name="system-notifications"></a>System-Benachrichtigung

Durch die Erweiterung `EngagementDefaultNotifier`, überschreiben Sie `onNotificationPrepared` Benachrichtigung ändern, der durch die Implementierung erstellt wurde.

Zum Beispiel:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content)
              throws RuntimeException
            {
              if ("ongoing".equals(content.getCategory()))
                notification.flags |= Notification.FLAG_ONGOING_EVENT;
              return true;
            }

In diesem Beispiel wird eine systembenachrichtigung für einen Inhalt als laufende Ereignis Kategorie "laufende" wird angezeigt.

Möchten Sie erstellen die `Notification` Objekt völlig kann man `false` Methode und Aufruf `notify` selbst auf die `NotificationManager`. In diesem Fall ist es wichtig, dass Sie einen `contentIntent`, `deleteIntent` und der Benachrichtigung Identifier, mit `EngagementReachReceiver`.

Hier ist ein richtiges Beispiel einer Implementierung:

            @Override
            protected boolean onNotificationPrepared(Notification notification, EngagementReachInteractiveContent content) throws RuntimeException
            {
              /* Required fields */
              NotificationCompat.Builder builder = new NotificationCompat.Builder(mContext)
                .setSmallIcon(notification.icon)              // icon is mandatory
                .setContentIntent(notification.contentIntent) // keep content intent
                .setDeleteIntent(notification.deleteIntent);  // keep delete intent

              /* Your customization */
              // builder.set...

              /* Dismiss option can be managed only after build */
              Notification myNotification = builder.build();
              if (!content.isNotificationCloseable())
                myNotification.flags |= Notification.FLAG_NO_CLEAR;

              /* Notify here instead of super class */
              NotificationManager manager = (NotificationManager) mContext.getSystemService(Context.NOTIFICATION_SERVICE);
              manager.notify(getNotificationId(content), myNotification); // notice the call to get the right identifier

              /* Return false, we notify ourselves */
              return false;
            }

##### <a name="notification-only-announcements"></a>Nur Benachrichtigung anzeigen

Die Verwaltung auf eine Benachrichtigung nur Ankündigung durch Überschreiben angepasst werden `EngagementDefaultNotifier.onNotifAnnouncementIntentPrepared` so ändern Sie die vorbereitete `Intent`. Mit dieser Methode können Sie leicht die Flags optimieren.

Beispielsweise fügen Sie der `SINGLE_TOP` Flag:

            @Override
            protected Intent onNotifAnnouncementIntentPrepared(EngagementNotifAnnouncement notifAnnouncement,
              Intent intent)
            {
              intent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP);
              return intent;
            }

Für ältere Engagement Benutzer Hinweis systembenachrichtigungen ohne URL jetzt Anwendungsstart sei es im Hintergrund, damit diese Methode eine Ankündigung ohne Aktion URL aufgerufen werden kann. Sie sollten, die beim Anpassen der Absicht.

Sie können auch implementieren `EngagementNotifier.executeNotifAnnouncementAction` völlig.

##### <a name="notification-life-cycle"></a>Benachrichtigung-Lebenszyklus

Verwendung die Standardkategorie nennt einige Lebenszyklusmethoden auf der `EngagementReachInteractiveContent` -Objekt Statistiken und den Status der Kampagne aktualisieren:

-   Wenn die Benachrichtigung wird in der Anwendung angezeigt oder in der Statusleiste die `displayNotification` -Methode (die Statistik meldet) von `EngagementReachAgent` Wenn `handleNotification` gibt `true`.
-   Wenn die Benachrichtigung geschlossen wird, die `exitNotification` -Methode aufgerufen wird, Statistik gemeldet und nächste Kampagnen können nun bearbeitet werden.
-   Wenn die Benachrichtigung geklickt haben, `actionNotification` wird aufgerufen, Statistik gemeldet und die zugeordnete Absicht gestartet.

Wenn die Implementierung von `EngagementNotifier` das Standardverhalten umgeht diese Lebenszyklusmethoden selbst aufrufen müssen. Die folgenden Beispiele veranschaulichen einige Fälle, in denen das Standardverhalten umgangen wird:

-   Erweitern von nicht `EngagementDefaultNotifier`, z. B. Kategorie Behandlung völlig implementiert.
-   Für System-Benachrichtigung überschrieben hat die `onNotificationPrepared` und Sie `contentIntent` oder `deleteIntent` in der `Notification` Objekt.
-   Für in app-Benachrichtigungen überschrieben hat `prepareInAppArea`, müssen Sie mindestens zugeordnet `actionNotification` eines der Benutzeroberfläche gesteuert.

> [AZURE.NOTE] Wenn `handleNotification` löst eine Ausnahme der Inhalt gelöscht und `dropContent` aufgerufen. Diese Statistiken gemeldet wird und nächste Kampagnen können nun bearbeitet.

### <a name="announcements-and-polls"></a>Ankündigungen und Umfragen

#### <a name="layouts"></a>Layouts

Sie können die `engagement_text_announcement.xml`, `engagement_web_announcement.xml` und `engagement_poll.xml` Dateien Text anzeigen, Web Ankündigungen und Umfragen anpassen.

Diese Dateien freigeben, zwei allgemeine Layouts für den Titel und den Bereich. Das Layout für den Titel ist `engagement_content_title.xml` und die gleichnamige zeichenbare Datei für den Hintergrund verwendet. Das Layout für die Aktion "und" Beenden "ist `engagement_button_bar.xml` und die gleichnamige zeichenbare Datei für den Hintergrund verwendet.

In einer Umfrage Frage Layout und ihre Wahlmöglichkeiten sind dynamisch vergrößert mehrmals mit der `engagement_question.xml` Layoutdatei Fragen und `engagement_choice.xml` -Datei für die Auswahl.

#### <a name="categories"></a>Kategorien

##### <a name="alternate-layouts"></a>Alternative layouts

Wie Benachrichtigungen kann die Kampagne Kategorie alternative Layouts für Produkt- und Umfragen müssen verwendet werden.

Z. B. zum Erstellen einer Kategorie für eine Ankündigung Text kann `EngagementTextAnnouncementActivity` , und verweisen sie die `AndroidManifest.xml` Datei:

            <activity android:name="com.your_company.MyCustomTextAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>

Beachten Sie, dass die Kategorie in den Zielfilter mit Ankündigung Standardaktivität unterscheiden.

Reach-SDK wird beabsichtigte um die richtige Aktivität für eine bestimmte Kategorie zu beheben und greift zurück auf die Standardkategorie Wenn die Auflösung nicht.

Dann implementieren Sie `MyCustomTextAnnouncementActivity`, möchten das Layout ändern (unter Beibehaltung derselben Ansichtsbezeichner), nur haben Sie zum Definieren der Klasse wie im folgenden Beispiel:

            public class MyCustomTextAnnouncementActivity extends EngagementTextAnnouncementActivity
            {
              @Override
              protected String getLayoutName()
              {
                return "my_text_announcement";  // tell super class to use R.layout.my_text_announcement
              }
            }

Die Standardkategorie Ankündigungen Text ersetzen Ersetzen `android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"` von der Implementierung.

Auf ähnliche Weise können Web Ankündigungen und Umfragen angepasst werden.

Für Web Bekanntgaben erweitern Sie `EngagementWebAnnouncementActivity` und Ihre Aktivität in die `AndroidManifest.xml` wie im folgenden Beispiel:

            <activity android:name="com.your_company.MyCustomWebAnnouncementActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="my_category" />
                <data android:mimeType="text/html" />    <!-- only difference with text announcements in the intent is the data mime type -->
              </intent-filter>
            </activity>

Für Umfragen kann `EngagementPollActivity` und die in der `AndroidManifest.xml` wie im folgenden Beispiel:

            <activity android:name="com.your_company.MyCustomPollActivity">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="my_category" />
              </intent-filter>
            </activity>

##### <a name="implementation-from-scratch"></a>Implementierung von Grund auf

Sie können Kategorien für Ihre Ankündigung (und Umfrage) implementieren, ohne eines der `Engagement*Activity` erreichen SDK bereitgestellten Klassen. Dies ist beispielsweise nützlich, eine Layout definieren, die nicht dieselben Ansichten der Standardlayouts verwendet werden soll.

Wie Voranmeldung Anpassung empfiehlt es sich den Quellcode der standard-Implementierung.

Hier sind einige Dinge zu beachten: Reichweite startet mit einem Absicht (intent Filter entspricht) sowie einen zusätzlichen Parameter Content-ID ist.

Rufen Sie das Inhaltsobjekt enthalten die Felder beim Erstellen der Kampagne auf der Website angegebene hierzu Sie:

            public class MyCustomTextAnnouncement extends EngagementActivity
            {
              private EngagementAnnouncement mContent;

              @Override
              protected void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);

                /* Get content */
                mContent = EngagementReachAgent.getInstance(this).getContent(getIntent());
                if (mContent == null)
                {
                  /* If problem with content, exit */
                  finish();
                  return;
                }

                setContentView(R.layout.my_text_announcement);

                /* Configure views by querying fields on mContent */
                // ...
              }
            }

Statistiken, sollten Sie melden, angezeigt der `onResume` Ereignis:

            @Override
            protected void onResume()
            {
             /* Mark the content displayed */
             mContent.displayContent(this);
             super.onResume();
            }

Dann vergessen Sie nicht, rufen Sie `actionContent(this)` oder `exitContent(this)` auf das Inhaltsobjekt vor dem Hintergrund die Aktivität eingehen.

Rufen Sie entweder nicht `actionContent` oder `exitContent`, Statistiken wird nicht (d. h. keine Analyse für die Kampagne) gesendet und vor allem die nächsten Kampagnen nicht benachrichtigt erst nach einem der Anwendungsprozess Neustart.

Orientierung oder sonstige Konfiguration können Code schwierig zu bestimmen, ob die Aktivität Hintergrund oder nicht, wird die standardmäßige Implementierung sorgt Inhalt als beendet, wenn der Benutzer die Aktivität lässt gemeldet wird (entweder durch Drücken von `HOME` oder `BACK`) aber ändert die Ausrichtung.

Hier ist der interessante Teil der Implementierung:

            @Override
            protected void onUserLeaveHint()
            {
              finish();
            }

            @Override
            protected void onPause()
            {
              if (isFinishing() && mContent != null)
              {
                /*
                 * Exit content on exit, this is has no effect if another process method has already been
                 * called so we don't have to check anything here.
                 */
                mContent.exitContent(this);
              }
              super.onPause();
            }

Wie Sie sehen, wenn Sie aufgerufen, `actionContent(this)` und die Aktivität beendet `exitContent(this)` kann sicher aufgerufen werden, ohne dass.

[Hier]:http://developer.android.com/tools/extras/support-library.html#Downloading
[Google Cloud Messaging]:http://developer.android.com/guide/google/gcm/index.html
[Amazon Gerät Messaging]:https://developer.amazon.com/sdk/adm.html
