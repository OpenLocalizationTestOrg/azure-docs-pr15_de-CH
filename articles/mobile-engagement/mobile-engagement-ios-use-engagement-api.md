<properties
    pageTitle="Verwendung der Engagement-API auf iOS"
    description="Neueste iOS SDK - Verwendung Engagement-API auf iOS"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>Verwendung der Engagement-API auf iOS

Dieses Dokument ist eine Ergänzung zum Dokument wie IOS zu integrieren: Tiefe Informationen zum Engagement-API verwenden, um Ihre Bewerbungsstatistik Bericht bietet.

Beachten Sie, dass Sie ggf. nur Engagement der Anwendung Sessions, Aktivitäten, Abstürze und technische Informationen am einfachsten wird die benutzerdefinierte zu `UIViewController` Objekten aus dem entsprechenden `EngagementViewController` Klasse.

Möchten Sie mehr, beispielsweise anwendungsspezifische Ereignisse, Fehler und Aufträge melden oder Aktivitäten Ihrer Anwendung anders gemeldet, als die Implementierung in der `EngagementViewController` Klassen, müssen Sie die Engagement-API verwenden.

Engagement-API bietet die `EngagementAgent` Klasse. Eine Instanz dieser Klasse kann abgerufen werden durch Aufrufen der `[EngagementAgent shared]` statische Methode (Beachten Sie, dass die `EngagementAgent` zurückgegebene Objekt ist ein Singleton).

Bevor API-Aufrufe die `EngagementAgent` Objekt muss durch Aufrufen der Methode initialisiert werden`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Engagement-Konzepte

Die folgenden Teile verfeinern allgemeine [Mobile Engagement Konzepte](mobile-engagement-concepts.md) für iOS-Plattform.

### <a name="session-and-activity"></a>`Session`und`Activity`

Eine *Aktivität* ist mit einer Seite der Anwendung heißt *Aktivität* startet, wenn der Bildschirm angezeigt und wird beendet, sobald der Bildschirm geschlossen: Dies ist der Fall, wenn das Engagement SDK integriert ist die `EngagementViewController` Klassen.

Aber *Aktivitäten* kann auch manuell mithilfe der API Engagement gesteuert werden. Dadurch teilen Sie einen Bildschirm in mehrere Teile Sub Weitere Informationen zur Verwendung von diesem Bildschirm (zum Beispiel bekannt, wie oft und wie lange Dialoge in diesem Bildschirm verwendet werden).

##<a name="reporting-activities"></a>Berichterstattung

### <a name="user-starts-a-new-activity"></a>Benutzer startet eine neue Aktivität

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Müssen Sie aufrufen `startActivity()` jedem Benutzer Aktivität ändert. Der erste Aufruf dieser Funktion startet eine neue Sitzung des Benutzers.

### <a name="user-ends-his-current-activity"></a>Der Benutzer beendet seine aktuelle Aktivität

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] Sie sollte **nie** rufen Sie diese Funktion alleine, außer wenn eine Verwendung der Anwendung in mehreren Sitzungen geteilt: ein Aufruf dieser Funktion würde am Ende der aktuellen Sitzung sofort so ein nachfolgender Aufruf `startActivity()` wird eine neue Sitzung starten. Diese Funktion wird vom SDK automatisch aufgerufen, wenn die Anwendung geschlossen wird.

##<a name="reporting-events"></a>Ereignisse

### <a name="session-events"></a>Sitzungsereignisse

Sitzungsereignisse werden normalerweise verwendet, um die Aktionen eines Benutzers während seiner Sitzung melden.

**Beispiel ohne zusätzliche Daten:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Beispiel mit zusätzlichen Daten:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Standalone-Ereignisse

Gegen Session-Ereignisse können eigenständige Ereignisse außerhalb einer Sitzung verwendet werden.

**Beispiel:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Melden von Fehlern

### <a name="session-errors"></a>Sitzungsfehler

Sitzungsfehler werden normalerweise zum Melden der Fehler Beeinträchtigung des Benutzers seine Sitzung verwendet.

**Beispiel:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Standalone-Fehler

Entgegen Sitzungsfehler können eigenständige Fehler außerhalb einer Sitzung verwendet werden.

**Beispiel:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Aufträge Reporting

**Beispiel:**

Angenommen Sie, Sie möchten die Dauer der Anmeldung melden:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Fehler während eines Auftrags melden

Fehler können mit ausgeführte Auftrag anstelle der aktuellen Sitzung miteinander verbunden.

**Beispiel:**

Angenommen Sie, Sie möchten Fehler bei der Anmeldung an:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Ereignisse während eines Auftrags

Ereignisse können mit ausgeführte Auftrag anstelle der aktuellen Sitzung miteinander verbunden.

**Beispiel:**

Angenommen Sie, wir haben ein soziales Netzwerk und wir verwenden einen Bericht die Gesamtzeit, während, die Benutzer mit dem Server verbunden ist. Der Benutzer empfangen Nachrichten von Freunden, dies ist ein Projekt-Ereignis.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>Zusätzliche Parameter

Ereignisse, Fehler, Aktivitäten und Projekte können beliebige Daten zugeordnet werden.

Diese Daten strukturiert sein, iOS NSDictionary-Klasse verwendet.

Extras enthalten, können `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` oder `NSDictionary` Instanzen.

> [AZURE.NOTE] Der zusätzliche Parameter wird in JSON serialisiert. Wenn Sie andere Objekte als die oben beschriebene übergeben möchten, müssen Sie die folgende Methode in der Klasse implementieren:
>
             -(NSString*)JSONRepresentation;
>
> Die Methode sollte eine JSON-Darstellung des Objekts zurück.

### <a name="example"></a>Beispiel

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Grenzen

#### <a name="keys"></a>Schlüssel

Jeder Schlüssel in der `NSDictionary` muss im folgenden regulären Ausdruck:

`^[a-zA-Z][a-zA-Z_0-9]*`

Also Schlüssel mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche starten müssen (\_).

#### <a name="size"></a>Größe

Extras sind auf **1024** Zeichen pro Aufruf (einmal codiert in JSON Engagement Agent).

Im vorherigen Beispiel wird an den Server gesendete JSON 58 Zeichen:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Anwendung Informationen

Sie können manuell melden, tracking-Informationen (oder eine andere anwendungsspezifische Informationen) mit der `sendAppInfo:` Funktion.

Hinweis Diese Informationen inkrementell gesendet werden können: der aktuelle Wert eines bestimmten Schlüssels für ein bestimmtes Gerät gespeichert.

Ereignis Extras wie die `NSDictionary` -Klasse zum abstrahieren Informationen, beachten Sie, dass Arrays oder Sub Wörterbücher als flache Zeichenfolgen (mit JSON-Serialisierung) behandelt werden.

**Beispiel:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Grenzen

#### <a name="keys"></a>Schlüssel

Jeder Schlüssel in der `NSDictionary` muss im folgenden regulären Ausdruck:

`^[a-zA-Z][a-zA-Z_0-9]*`

Also Schlüssel mit mindestens einem Buchstaben, gefolgt von Buchstaben, Ziffern oder Unterstriche starten müssen (\_).

#### <a name="size"></a>Größe

Anwendungsinformationen sind auf **1024** Zeichen pro Aufruf (einmal codiert in JSON Engagement Agent).

Im vorherigen Beispiel wird an den Server gesendete JSON 44 Zeichen:

    {"birthdate":"1983-12-07","gender":"female"}
