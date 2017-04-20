<properties 
    pageTitle="Verwendung des Projekts API auf Windows Phone Silverlight" 
    description="Verwendung des Projekts API auf Windows Phone Silverlight"    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" /> 

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a>Verwendung des Projekts API auf Windows Phone Silverlight

Dieses Dokument ist eine Ergänzung zum Dokument [Mobile Engagement in Windows Phone Silverlight-app integrieren](mobile-engagement-windows-phone-integrate-engagement.md). Es bietet Tiefe Informationen zum Engagement-API verwenden, um Anwendungsstatistiken melden.

Wenn soll nur Engagement der Anwendung Sessions, Aktivitäten, Abstürze und technische Informationen, die einfachste Möglichkeit, alle Ihre `PhoneApplicationPage` untergeordnete Klassen erben von der `EngagementPage` Klasse.

Möchten Sie mehr, beispielsweise anwendungsspezifische Ereignisse, Fehler und Aufträge melden oder Aktivitäten Ihrer Anwendung anders gemeldet, als die Implementierung in der `EngagementPage` Klassen, müssen Sie die Engagement-API verwenden.

Engagement-API bietet die `EngagementAgent` Klasse. Zugriff auf diese Methoden über `EngagementAgent.Instance`.

Auch wenn das Agent-Modul nicht initialisiert wurde, jeder Aufruf der API wird zurückgestellt und erneut ausgeführt wird, wenn der Agent verfügbar ist.

##<a name="engagement-concepts"></a>Engagement-Konzepte

Die folgenden Teile verfeinern Mobile Engagement Konzepte für die Windows Phone-Plattform.

### <a name="session-and-activity"></a>`Session`und`Activity`

Eine *Aktivität* ist gewöhnlich eine Seite der Anwendung, d. h. *Aktivität* startet, wenn die Seite angezeigt wird und wird beendet, wenn die Seite geschlossen wird: Dies ist der Fall, wenn das Engagement SDK integriert ist die `EngagementPage` Klasse.

Aber *Aktivitäten* kann auch manuell mithilfe der API Engagement gesteuert werden. Dadurch teilen eine bestimmte Seite mehrere Sub Teile erfahren mehr über die Verwendung dieser Seite (z. B. bekannten wie oft und wie lange Dialogs innerhalb dieser Seite verwendet werden).

##<a name="reporting-activities"></a>Berichterstattung

### <a name="user-starts-a-new-activity"></a>Benutzer startet eine neue Aktivität

#### <a name="reference"></a>Referenz

            void StartActivity(string name, Dictionary<object, object> extras = null)

Müssen Sie aufrufen `StartActivity()` jedem Benutzer Aktivität ändert. Der erste Aufruf dieser Funktion startet eine neue Sitzung des Benutzers.

> [AZURE.IMPORTANT] Das SDK automatisch die Endaktivität-Methode aufrufen beim Schließen der Anwendung. Daher wird empfohlen die Methode StartActivity aufrufen, sobald die Aktivität des Benutzers ändern und nie Endaktivität Methode aufrufen, da diese Methode aufzurufen erzwingt die aktuelle Sitzung beendet werden.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Der Benutzer beendet seine aktuelle Aktivität

#### <a name="reference"></a>Referenz

            void EndActivity()

Müssen Sie aufrufen `EndActivity()` mindestens einmal bei der Benutzer hat seine letzte Aktivität. Dieser Engagement SDK informiert, dass des Benutzers derzeit im Leerlauf läuft, Sitzung einmal das Sitzungstimeout geschlossen werden müssen (Aufruf `StartActivity()` vor Ablauf des Timeouts Sitzung wird die Sitzung einfach weiter).

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Aufträge Reporting

### <a name="start-a-job"></a>Starten eines Auftrags

#### <a name="reference"></a>Referenz

            void StartJob(string name, Dictionary<object, object> extras = null)

Den Auftrag können Sie bestimmte Vorgänge über einen Zeitraum überwachen.

#### <a name="example"></a>Beispiel

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Beenden eines Auftrags

#### <a name="reference"></a>Referenz

            void EndJob(string name)

Als Abschluss ein Vorgangs durch einen Auftrag verfolgt rufen die EndJob-Methode für dieses Projekt Sie mit der Auftragsname.

#### <a name="example"></a>Beispiel

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Ereignisse

Es gibt drei Arten von Ereignissen:

-   Standalone-Ereignisse
-   Sitzungsereignisse
-   Job-Ereignisse

### <a name="standalone-events"></a>Standalone-Ereignisse

#### <a name="reference"></a>Referenz

            void SendEvent(string name, Dictionary<object, object> extras = null)

Eigenständige Ereignisse können außerhalb einer Sitzung auftreten.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Sitzungsereignisse

#### <a name="reference"></a>Referenz

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Sitzungsereignisse werden normalerweise verwendet, um die Aktionen eines Benutzers während seiner Sitzung melden.

#### <a name="example"></a>Beispiel

**Ohne Daten:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Daten:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Job-Ereignisse

#### <a name="reference"></a>Referenz

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Job-Ereignisse werden normalerweise verwendet, um die Aktionen eines Benutzers innerhalb eines Projektes melden.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Melden von Fehlern

Es gibt drei Arten von Fehlern:

-   Standalone-Fehler
-   Sitzungsfehler
-   Auftragsfehler

### <a name="standalone-errors"></a>Standalone-Fehler

#### <a name="reference"></a>Referenz

            void SendError(string name, Dictionary<object, object> extras = null)

Entgegen Sitzungsfehler können eigenständige Fehler außerhalb einer Sitzung auftreten.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Sitzungsfehler

#### <a name="reference"></a>Referenz

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Sitzungsfehler werden normalerweise zum Melden der Fehler Beeinträchtigung des Benutzers seine Sitzung verwendet.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Auftragsfehler

#### <a name="reference"></a>Referenz

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Fehler können mit ausgeführte Auftrag anstelle der aktuellen Sitzung miteinander verbunden.

#### <a name="example"></a>Beispiel

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Abstürze Reporting

Der Agent stellt zwei Methoden mit abstürzen.

### <a name="send-an-exception"></a>Ausnahme senden

#### <a name="reference"></a>Referenz

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Beispiel

Sie können eine Ausnahme jederzeit telefonisch senden:

            EngagementAgent.Instance.SendCrash(aCatchedException);

Einen optionalen Parameter können Engagement Sitzung gleichzeitig als Absturz senden beenden. Dazu rufen Sie:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Wenn Sie dies tun, werden die Sitzung und Aufträge geschlossen nach Senden des Absturzes.

### <a name="send-an-unhandled-exception"></a>Eine nicht behandelte Ausnahme senden

#### <a name="reference"></a>Referenz

            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

Angebot umfasst auch eine Methode, um nicht behandelte Ausnahmen senden. Dies ist besonders nützlich, wenn Silverlight UnhandledException-Ereignishandler verwendet.

Diese Methode wird **immer** beenden Engagement Sitzung und Aufträge nach aufgerufen wird.

#### <a name="example"></a>Beispiel

Sie können eigene UnhandledException-Handler implementiert wird (besonders wenn Berichterstellungsfunktion Engagement Absturz deaktiviert haben). In der `Application_UnhandledException` Methode der `App.xaml.cs` Datei:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code
            
              EngagementAgent.Instance.SendCrash(e);
            }

##<a name="onactivated"></a>OnActivated

### <a name="reference"></a>Referenz

            void OnActivated(ActivatedEventArgs e)

Beim Weiterleiten einer Anwendung navigiert nach Deactivated-Ereignis ausgelöst wird, versucht das Betriebssystem die Anwendung in einen inaktiven Zustand versetzen. Die Anwendung ist veraltet. In diesem Prozess eine Anwendung beendet wird, aber einige Daten über den Zustand der Anwendung und die einzelnen Seiten in der Anwendung erhalten.

Sie müssen `EngagementAgent.Instance.OnActivated(e)` in die `Application_Activated` -Methode aus der Datei App.xaml.cs Engagement Agent zurückgesetzt, wenn die Anwendung als veraltet markiert wurde.

### <a name="example"></a>Beispiel

            // Inside your App.xaml.cs file
            
            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

##<a name="device-id"></a>Geräte-Id

            String GetDeviceId()

Durch Aufrufen dieser Methode können Sie Engagement Geräte-Id abrufen.

##<a name="extras-parameters"></a>Extras-Parameter

Ein Ereignis, einen Fehler, eine Aktivität oder einen Auftrag können beliebige Daten zugeordnet werden. Diese Daten können unter Verwendung eines Wörterbuchs strukturiert werden. Schlüssel und Werte kann beliebigen Typs.

Extras Daten serialisiert werden, haben Sie eigene Extras einfügen möchten Sie einen Datenvertrag für diesen hinzufügen.

### <a name="example"></a>Beispiel

Wir erstellen eine neue Klasse "Person".

            using System.Runtime.Serialization;
            
            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Dann fügen wir eine `Person` Instanz einer Extra.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Wenn Sie andere Objekte verschieben, unbedingt die ToString()-Methode implementiert eine lesbare Zeichenfolge zurück.

### <a name="limits"></a>Grenzen

#### <a name="keys"></a>Schlüssel

Jeder Schlüssel im Objekt muss im folgenden regulären Ausdruck:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Also Schlüssel mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche starten müssen (\_).

#### <a name="size"></a>Größe

Extras sind maximal **1024** Zeichen pro Aufruf.

##<a name="reporting-application-information"></a>Anwendung Informationen

### <a name="reference"></a>Referenz

            void SendAppInfo(Dictionary<object, object> appInfos)

Sie können manuell Funktion tracking-Informationen (oder eine andere anwendungsspezifische Informationen) den SendAppInfo() melden.

Hinweis Diese Informationen inkrementell gesendet werden können: der aktuelle Wert eines bestimmten Schlüssels für ein bestimmtes Gerät gespeichert. Wie Ereignis Extras ein Wörterbuch verwenden\<Objekt, Objekt\> Informationen an.

### <a name="example"></a>Beispiel

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Grenzen

#### <a name="keys"></a>Schlüssel

Jeder Schlüssel im Objekt muss im folgenden regulären Ausdruck:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Also Schlüssel mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche starten müssen (\_).

#### <a name="size"></a>Größe

Anwendungsinformationen sind maximal **1024** Zeichen pro Aufruf.

Im vorherigen Beispiel wird an den Server gesendete JSON 44 Zeichen:

            {"subscription":"2013-12-07","premium":"true"}
 
##<a name="logging"></a>Protokollierung
###<a name="enable-logging"></a>Aktivieren der Protokollierung

Das SDK kann konfiguriert werden, Testprotokolle in der IDE-Konsole erstellen.
Diese Protokolle sind standardmäßig nicht aktiviert. Um dies anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen Wert von der `EngagementTestLogLevel` -Enumeration, beispielsweise:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
