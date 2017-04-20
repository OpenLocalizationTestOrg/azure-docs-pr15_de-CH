<properties
    pageTitle="Verwendung des Projekts API für Android"
    description="Aktuelle Android SDK - Verwendung Engagement API für Android"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="piyushjo;ricksal" />

#<a name="how-to-use-the-engagement-api-on-android"></a>Verwendung des Projekts API für Android

Dieses Dokument ist eine Ergänzung zum Dokument [Erweiterte Reporting-Optionen für Android Mobile Engagement-SDK](mobile-engagement-android-advanced-reporting.md). Es bietet Tiefe Informationen zum Engagement-API verwenden, um Anwendungsstatistiken melden.

Beachten Sie, dass Sie ggf. nur Engagement der Anwendung Sessions, Aktivitäten, Abstürze und technische Informationen am einfachsten wird alle Ihre `Activity` untergeordnete Klassen erben von der entsprechenden `EngagementActivity` Klasse.

Möchten Sie mehr, beispielsweise anwendungsspezifische Ereignisse, Fehler und Aufträge melden oder Aktivitäten Ihrer Anwendung anders gemeldet, als die Implementierung in der `EngagementActivity` Klassen, müssen Sie die Engagement-API verwenden.

Engagement-API bietet die `EngagementAgent` Klasse. Eine Instanz dieser Klasse kann abgerufen werden durch Aufrufen der `EngagementAgent.getInstance(Context)` statische Methode (Beachten Sie, dass die `EngagementAgent` zurückgegebene Objekt ist ein Singleton).

##<a name="engagement-concepts"></a>Engagement-Konzepte

Die folgenden Teile optimieren die allgemeine [Mobile Engagement Konzepte](mobile-engagement-concepts.md)für Android.

### <a name="session-and-activity"></a>`Session`und`Activity`

Bleibt der Benutzer mehr als ein paar Sekunden zwischen zwei *Aktivitäten*im Leerlauf, wird seine Abfolge von *Aktivitäten* in zwei unterschiedlichen *Sessions*aufgeteilt. Diese Sekunden heißen "Session Timeout".

Eine *Aktivität* ist mit einer Seite der Anwendung heißt *Aktivität* startet, wenn der Bildschirm angezeigt und wird beendet, sobald der Bildschirm geschlossen: Dies ist der Fall, wenn das Engagement SDK integriert ist die `EngagementActivity` Klassen.

Aber *Aktivitäten* kann auch manuell mithilfe der API Engagement gesteuert werden. Dadurch teilen Sie einen Bildschirm in mehrere Teile Sub Weitere Informationen zur Verwendung von diesem Bildschirm (zum Beispiel bekannt, wie oft und wie lange Dialoge in diesem Bildschirm verwendet werden).

##<a name="reporting-activities"></a>Berichterstattung

> [AZURE.IMPORTANT] Sie müssen Bericht wie in diesem Abschnitt verwenden Sie das `EngagementActivity` Klasse und seine Varianten wie zu integrieren wie Android Dokument.

### <a name="user-starts-a-new-activity"></a>Benutzer startet eine neue Aktivität

            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

Müssen Sie aufrufen `startActivity()` jedem Benutzer Aktivität ändert. Der erste Aufruf dieser Funktion startet eine neue Sitzung des Benutzers.

Am besten rufen Sie diese Funktion für jede Aktivität ist `onResume` Rückruf.

### <a name="user-ends-his-current-activity"></a>Der Benutzer beendet seine aktuelle Aktivität

            EngagementAgent.getInstance(this).endActivity();

Müssen Sie aufrufen `endActivity()` mindestens einmal bei der Benutzer hat seine letzte Aktivität. Dieser Engagement SDK informiert, dass des Benutzers derzeit im Leerlauf läuft, Sitzung einmal das Sitzungstimeout geschlossen werden müssen (Aufruf `startActivity()` vor Ablauf des Timeouts Sitzung wird die Sitzung einfach fortgesetzt).

Am besten rufen Sie diese Funktion für jede Aktivität ist `onPause` Rückruf.

##<a name="reporting-events"></a>Ereignisse

### <a name="session-events"></a>Sitzungsereignisse

Sitzungsereignisse werden normalerweise verwendet, um die Aktionen eines Benutzers während seiner Sitzung melden.

**Beispiel ohne zusätzliche Daten:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Beispiel mit zusätzlichen Daten:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Standalone-Ereignisse

Gegen Session-Ereignisse können eigenständige Ereignisse außerhalb einer Sitzung auftreten.

**Beispiel:**

Angenommen Sie, Sie möchten Ereignisse beim broadcast Ereignisempfänger ausgelöst:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

##<a name="reporting-errors"></a>Melden von Fehlern

### <a name="session-errors"></a>Sitzungsfehler

Sitzungsfehler werden normalerweise zum Melden der Fehler Beeinträchtigung des Benutzers seine Sitzung verwendet.

**Beispiel:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Standalone-Fehler

Entgegen Sitzungsfehler können eigenständige Fehler außerhalb einer Sitzung auftreten.

**Beispiel:**

Im folgenden Beispiel wird veranschaulicht, wie Fehler an der Speicher wird knapp Telefon während Anwendungsprozess ausgeführt wird.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

##<a name="reporting-jobs"></a>Aufträge Reporting

### <a name="example"></a>Beispiel

Angenommen Sie, Sie möchten die Dauer der Anmeldung melden:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Fehler während eines Auftrags melden

Fehler können mit ausgeführte Auftrag anstelle der aktuellen Sitzung miteinander verbunden.

**Beispiel:**

Angenommen Sie, Sie möchten Bericht Fehler bei Anmeldung Prozess:

[...] public void anmelden (Kontext,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Ereignisberichterstattung während eines Auftrags

Ereignisse können mit ausgeführte Auftrag anstelle der aktuellen Sitzung miteinander verbunden.

**Beispiel:**

Angenommen Sie, wir haben ein soziales Netzwerk und wir verwenden einen Bericht die Gesamtzeit, während, die Benutzer mit dem Server verbunden ist. Der Benutzer kann bleiben im Hintergrund auch wenn er einer anderen Anwendung verwendet oder im Ruhezustand Telefon so vorhanden ist.

Der Benutzer empfangen Nachrichten von Freunden, dies ist ein Projekt-Ereignis.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

##<a name="extra-parameters"></a>Zusätzliche Parameter

Ereignisse, Fehler, Aktivitäten und Projekte können beliebige Daten zugeordnet werden.

Diese Daten strukturiert werden, Android Bundle Klasse verwendet (tatsächlich funktioniert wie zusätzliche Parameter Android Absichten). Beachten Sie, dass ein Bündel Arrays oder ein anderes Paket-Instanzen enthalten kann.

> [AZURE.IMPORTANT] Wenn Sie parcelable oder serialisierbare Parameter hinzufügen, sicherstellen ihrer `toString()` -Methode implementiert, um eine lesbare Zeichenfolge zurück. Serialisierbare Klassen, die nicht vorübergehende Felder enthalten, die nicht serialisierbar machen Android abstürzen, wenn Sie aufrufen`bundle.putSerializable("key",value);`

> [AZURE.WARNING] Dünn besetzte Arrays überzählige Parameter werden nicht unterstützt, d. h. wird nicht wie ein Array serialisiert werden. Sie sollten Sie in standard-Arrays vor der Verwendung zusätzlicher Parameter konvertieren.

### <a name="example"></a>Beispiel

            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Grenzen

#### <a name="keys"></a>Schlüssel

Jeder Schlüssel in der `Bundle` muss im folgenden regulären Ausdruck:

`^[a-zA-Z][a-zA-Z_0-9]*`

Also Schlüssel mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche starten müssen (\_).

#### <a name="size"></a>Größe

Extras sind auf **1024** Zeichen pro Aufruf (einmal codiert in JSON Engagement Service).

Im vorherigen Beispiel wird an den Server gesendete JSON 58 Zeichen:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Anwendung Informationen

Sie können manuell melden, tracking-Informationen (oder eine andere anwendungsspezifische Informationen) mit der `sendAppInfo()` Funktion.

Hinweis Diese Informationen inkrementell gesendet werden können: der aktuelle Wert eines bestimmten Schlüssels für ein bestimmtes Gerät gespeichert.

Bundle-Klasse dient wie Ereignis Extras abstrakte Informationen, beachten Sie, dass Arrays oder untergeordnete Pakete als flache Zeichenfolgen (mit JSON-Serialisierung) behandelt werden.

### <a name="example"></a>Beispiel

Hier ist ein Codebeispiel Benutzer Geschlecht und Geburtsdatum:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Grenzen

#### <a name="keys"></a>Schlüssel

Jeder Schlüssel in der `Bundle` muss im folgenden regulären Ausdruck:

`^[a-zA-Z][a-zA-Z_0-9]*`

Also Schlüssel mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche starten müssen (\_).

#### <a name="size"></a>Größe

Anwendungsinformationen sind auf **1024** Zeichen pro Aufruf (einmal codiert in JSON Engagement Service).

Im vorherigen Beispiel wird an den Server gesendete JSON 44 Zeichen:

            {"expiration":"2016-12-07","status":"premium"}
