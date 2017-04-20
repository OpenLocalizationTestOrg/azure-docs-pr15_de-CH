<properties
    pageTitle="Azure Mobile Engagement Web SDK APIs | Microsoft Azure"
    description="Die neuesten Updates und Verfahren zum SDK von Azure Mobile Engagement"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="web"
    ms.devlang="js"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="piyushjo" />

# <a name="use-the-azure-mobile-engagement-api-in-a-web-application"></a>Verwenden Sie Azure Mobile Engagement-API in einer Webanwendung

Dieses Dokument ist ein Dokument, das Sie darüber informiert [Mobile Engagement in eine Anwendung](mobile-engagement-web-integrate-engagement.md)integrieren. Es enthält detaillierte Informationen zum Azure Mobile Engagement-API verwenden, um Anwendungsstatistiken melden.

Mobile Engagement-API bietet die `engagement.agent` Objekt. Azure Mobile Engagement Web SDK alias ist standardmäßig `engagement`. Sie können diesen Alias von SDK-Konfiguration neu definieren.

## <a name="mobile-engagement-concepts"></a>Mobile Engagement-Konzepte

Die folgenden Teile verfeinern allgemeine [Konzepte Mobile Engagement](mobile-engagement-concepts.md) für die Plattform.

### <a name="session-and-activity"></a>`Session`und`Activity`

Wenn der Benutzer mehr als ein paar Sekunden zwischen zwei Aktivitäten im Leerlauf bleibt, wird Reihenfolge der Aktivitäten des Benutzers in zwei unterschiedlichen geteilt. Diese Sekunden heißen die Sitzung ist abgelaufen.

Wenn Webtests nicht das Ende des Benutzers selbst deklariert (durch Aufrufen der `engagement.agent.endActivity` Funktion), Mobile Engagement Server Sitzung innerhalb von drei Minuten nach dem Schließen der Anwendungsseite automatisch abläuft. Dies ist das Sitzungstimeout Server bezeichnet.

### `Crash`

Automatisierte Berichte nicht abgefangene JavaScript-Ausnahmen werden nicht standardmäßig erstellt. Jedoch können Sie die melden Abstürze manuell mithilfe der `sendCrash` (siehe Abschnitt Berichterstattung stürzt ab).

## <a name="reporting-activities"></a>Berichterstattung

Berichte über Benutzeraktivitäten enthält, wenn ein Benutzer eine neue Aktivität startet und wenn der Benutzer die aktuelle Aktivität beendet.

### <a name="user-starts-a-new-activity"></a>Benutzer startet eine neue Aktivität

    engagement.agent.startActivity("MyUserActivity");

Müssen Sie aufrufen `startActivity()` jedes Mal Benutzeraktivität geändert wird. Der erste Aufruf dieser Funktion startet eine neue Sitzung des Benutzers.

### <a name="user-ends-the-current-activity"></a>Der Benutzer beendet die aktuelle Aktivität

    engagement.agent.endActivity();

Müssen Sie aufrufen `endActivity()` mindestens einmal bei der Benutzer beendet die letzte Aktivität. Mobile Engagement von SDK wird dadurch mitgeteilt, dass der Benutzer derzeit inaktiv ist und Sitzung nach Ablauf des Zeitlimits für die Sitzung geschlossen werden soll. Rufen Sie `startActivity()` vor Ablauf des Timeouts Sitzung wird die Sitzung einfach fortgesetzt.

Da keine zuverlässige Wenn das Navigator-Fenster geschlossen wird ist, ist es häufig schwierig oder unmöglich zu Ende Benutzeraktivitäten in eine. Deshalb Server Mobile Engagement Sitzung innerhalb von drei Minuten nach dem Schließen der Anwendungsseite automatisch abläuft.

## <a name="reporting-events"></a>Ereignisse

Berichte über Ereignisse behandelt Sitzung und eigenständige Ereignisse.

### <a name="session-events"></a>Sitzungsereignisse

Sitzungsereignisse werden normalerweise verwendet, um die Aktionen eines Benutzers während der Sitzung des Benutzers melden.

**Beispiel ohne zusätzliche Daten:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login');
      // [...]
    }

**Beispiel mit zusätzlichen Daten:**

    loginButton.onclick = function() {
      engagement.agent.sendSessionEvent('login', {user: 'alice'});
      // [...]
    }

### <a name="standalone-events"></a>Standalone-Ereignisse

Im Gegensatz zu Sitzungsereignisse können eigenständige Ereignisse außerhalb des Kontexts einer Sitzung auftreten.

Verwenden ``engagement.agent.sendEvent`` anstelle von ``engagement.agent.sendSessionEvent``.

## <a name="reporting-errors"></a>Melden von Fehlern

Fehler gemeldet werden, Sitzung und Standalone-Fehlern.

### <a name="session-errors"></a>Sitzungsfehler

Sitzungsfehler werden normalerweise zum Melden der Fehler, die der Benutzer während der Sitzung des Benutzers auswirken.

**Beispiel ohne zusätzliche Daten:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short');
      }
      // [...]
    }

**Beispiel mit zusätzlichen Daten:**

    var validateForm = function() {
      // [...]
      if (password.length < 6) {
        engagement.agent.sendSessionError('password_too_short', {length: 4});
      }
      // [...]
    }

### <a name="standalone-errors"></a>Standalone-Fehler

Im Gegensatz zu Sitzungsfehler können eigenständige Fehler außerhalb des Kontexts einer Sitzung auftreten.

Verwenden `engagement.agent.sendError` anstelle von `engagement.agent.sendSessionError`.

## <a name="reporting-jobs"></a>Aufträge Reporting

Reporting auf Aufträge reporting Fehler und Ereignisse während eines Auftrags und reporting abstürzen.

**Beispiel:**

Wenn Sie eine AJAX-Anforderung überwachen möchten, verwenden Sie Folgendes:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
      // [...]
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-errors-during-a-job"></a>Melden von Fehlern während eines Auftrags

Fehler können mit ausgeführte Auftrag nicht auf der aktuellen Sitzung verbunden.

**Beispiel:**

Möchten Sie einen Fehler melden, wenn eine AJAX-Anforderung fehlschlägt:

    // [...]
    xhr.onreadystatechange = function() {
      if (xhr.readyState == 4) {
        // [...]
        if (xhr.status == 0 || xhr.status >= 400) {
          engagement.agent.sendJobError('publish_xhr', 'publish', {status: xhr.status, statusText: xhr.statusText});
        }
        engagement.agent.endJob('publish');
      }
    }
    engagement.agent.startJob('publish');
    xhr.send();
    // [...]

### <a name="reporting-events-during-a-job"></a>Ereignisberichterstattung während eines Auftrags

Ereignisse verknüpft werden ausgeführte Auftrag nicht auf der aktuellen Sitzung Dank der `engagement.agent.sendJobEvent` Funktion.

Funktioniert genau wie `engagement.agent.sendJobError`.

### <a name="reporting-crashes"></a>Abstürze Reporting

Verwenden der `sendCrash` Funktion Bericht manuell stürzt ab.

Die `crashid` Argument ist ein String, der den Absturz bezeichnet.
Die `crash` Argument ist normalerweise Stapelrahmen des Absturzes als Zeichenfolge.

    engagement.agent.sendCrash(crashid, crash);

## <a name="extra-parameters"></a>Zusätzliche Parameter

Sie können beliebige Daten Ereignis, Fehler, Aktivität oder Auftrag anfügen.

Die Daten können ein JSON-Objekt (aber kein Array oder primitiver Typ) sein.

**Beispiel:**

    var extras = {"video_id": 123, "ref_click": "http://foobar.com/blog"};
    engagement.agent.sendEvent("video_clicked", extras);

### <a name="limits"></a>Grenzen

Grenzwerte für zusätzliche Parameter sind in den Bereichen der regulären Ausdrücke für Schlüssel, Werttypen und Größe.

#### <a name="keys"></a>Schlüssel

Jeder Schlüssel im Objekt muss im folgenden regulären Ausdruck:

    ^[a-zA-Z][a-zA-Z_0-9]*

Somit Schlüssel mit mindestens einem Buchstaben beginnen müssen, gefolgt von Buchstaben, Ziffern oder Unterstriche (\_).

#### <a name="values"></a>Werte

Werte sind Zeichenfolge, Zahl und boolesche Typen auf.

#### <a name="size"></a>Größe

Extras sind maximal 1.024 Zeichen pro Aufruf (nach Mobile Engagement Web SDK wird in JSON codiert).

## <a name="reporting-application-information"></a>Anwendung Informationen

Sie können manuell melden tracking-Informationen (oder andere anwendungsspezifische Informationen) mithilfe der `sendAppInfo()` Funktion.

Beachten Sie, dass diese Informationen inkrementell gesendet werden kann. Der aktuelle Wert für einen bestimmten Schlüssel werden für ein bestimmtes Gerät gespeichert.

Ein JSON-Objekt können Sie wie Ereignis Extras Informationen abstrahieren. Beachten Sie, dass Arrays oder Objekte als flache Zeichenfolgen (mit JSON-Serialisierung) behandelt werden.

**Beispiel:**

Hier ist ein Codebeispiel, das Geschlecht des Benutzers und Geburtsdatum:

    var appInfos = {"birthdate":"1983-12-07","gender":"female"};
    engagement.agent.sendAppInfo(appInfos);

### <a name="limits"></a>Grenzen

Die Informationen betreffen, sind die Bereiche der regulären Ausdrücke für Schlüssel und Größe.

#### <a name="keys"></a>Schlüssel

Jeder Schlüssel im Objekt muss im folgenden regulären Ausdruck:

    ^[a-zA-Z][a-zA-Z_0-9]*

Somit Schlüssel mit mindestens einem Buchstaben beginnen müssen, gefolgt von Buchstaben, Ziffern oder Unterstriche (\_).

#### <a name="size"></a>Größe

Anwendungsinformationen beträgt 1.024 Zeichen pro Aufruf (nach Mobile Engagement Web SDK wird in JSON codiert).

Im vorangehenden Beispiel ist an den Server gesendete JSON 44 Zeichen:

    {"birthdate":"1983-12-07","gender":"female"}
