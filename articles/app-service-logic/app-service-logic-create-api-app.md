<properties 
    pageTitle="Eine API für Logik-Apps erstellen" 
    description="Erstellen einer benutzerdefinierten API mit Logik Apps" 
    authors="jeffhollan" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na" 
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jehollan"/>
    
# <a name="creating-a-custom-api-to-use-with-logic-apps"></a>Erstellen einer benutzerdefinierten API mit Logik Apps

Wenn Sie Logik Apps-Plattform erweitern möchten, sind viele Arten können Sie APIs oder Systeme, die eines unserer vielen Out-of-the-Box-Connectors.  Die Möglichkeit einer API-App erstellen können Sie innerhalb eines Workflows Logik App aus aufrufen.

## <a name="helpful-tools"></a>Nützliche Tools

APIs mit Logik-Apps optimiert empfehlen wir ein [swagger](http://swagger.io) Dokument mit unterstützten Vorgänge und Parameter für Ihre API generiert.  Es gibt viele Bibliotheken (z. B. [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)), die automatisch für Sie stolz.  [TRex](https://github.com/nihaue/TRex) können zu kommentieren stolz mit Logik-Apps arbeiten (anzeigen Namen, Typen usw.).  Einige Beispiele für integrierte Logik Apps API-Apps müssen Sie unsere [GitHub Repository](http://github.com/logicappsio) oder [Blog](http://aka.ms/logicappsblog)überprüfen.

## <a name="actions"></a>Aktionen

Die Standardaktion für eine Anwendung Logik ist ein Domänencontroller, der eine HTTP-Anforderung akzeptiert und eine Antwort (normalerweise 200) zurückgeben.  Es gibt verschiedene Muster folgen können, um Aktionen Ihren Bedürfnissen entsprechend erweitern.

Standardmäßig Logik App Engine wird Timeout eine Anforderung nach einer Minute.  Allerdings haben Sie Ihre API Aktionen mehr ausführen und der Motor Abschluss einer asynchronen oder Webhook Muster unten abwarten.

Schreiben Sie für Standardaktionen einfach HTTP-Anforderungsmethode in Ihrer API über stolz ausgesetzt.  Beispiele für API-apps sehen, die mit Logik-Apps in unserem [GitHub Repository](https://github.com/logicappsio).  Hier gibt allgemeine Muster mit einem benutzerdefinierten Anschluss ausgeführt.

### <a name="long-running-actions---async-pattern"></a>Lang ausgeführte Aktionen - asynchrone Muster

Ein lange Schritt oder Aufgabe wird als Erstes müssen Sie sicherstellen, dass das Modul weiß noch nicht überschritten. Müssen Sie auch das Modul kommunizieren wie wissen, wann Sie die Aufgabe abgeschlossen haben und Sie abschließend relevante Daten an das Modul zurück, damit der Workflow fortgesetzt werden kann. Können Sie über eine API anhand des Fluss unten abgeschlossen. Diese Schritte sind in der Nummer der Ansicht benutzerdefinierte API:

1. Wenn eine Anforderung empfangen, senden eine Antwort sofort (bevor Arbeit). Diese Antwort wird ein `202 ACCEPTED` , lässt das Modul wissen Sie Daten akzeptiert die Nutzlast und jetzt verarbeiten. 202 Antwort sollte folgende Header enthalten: 
 * `location`Header (erforderlich): Dies ist ein absoluter Pfad zum URL Logik Apps können Sie den Status des Auftrags überprüfen.
 * `retry-after`(optional, Standardwert 20 Aktionen). Dies ist die Anzahl der Sekunden, die das Modul warten soll, bevor die URL des Orts Header überprüfen Status abrufen.

2. Wenn ein Auftragsstatus aktiviert ist, führen Sie überprüft: 
 * Wenn der Auftrag abgeschlossen ist: Zurückgeben einer `200 OK` Antwort mit der Antwort.
 * Wenn der Auftrag noch verarbeitet: Zurückgeben einer anderen `202 ACCEPTED` Antwort Headern als erste Reaktion

Dieses Muster ermöglicht Ausführen langer Vorgänge Thread Ihre benutzerdefinierte API jedoch erhalten eine Verbindung mit der Logik Apps, Timeout nicht bzw. bevor Arbeit abgeschlossen ist. Wenn dies in Ihrer Anwendung Logik hinzufügen, ist wichtig zu beachten, Sie brauchen nichts in die Definition für die Logik-App abrufen und überprüfen. Als das Modul 202 akzeptiert Antwort mit einem gültigen Speicherort Header erkennt, wird das asynchrone Muster berücksichtigt und weiterhin Location-Header abrufen, bis nicht 202 zurückgegeben wird.

Finden Sie ein Beispiel für dieses Muster in GitHub [hier](https://github.com/jeffhollan/LogicAppsAsyncResponseSample)

### <a name="webhook-actions"></a>Webhook Aktionen

Während der Workflow haben Sie der Anwendung Logik und wartet auf "Rückruf" fort.  Dieser Rückruf wird in Form von HTTP POST.  Um dieses Muster zu implementieren, müssen Sie zwei Endpunkte auf dem Domänencontroller bereitstellen: Ihr Abonnement.

Auf "abonnieren" Logik-App erstellen und Registrieren einer Callback-URL der API speichern kann und Rückruf mit als HTTP POST.  Inhalt/Header werden in der Anwendung Logik übergeben und in den Rest des Workflows verwendet werden.  Logik App Engine rufen abonnieren Punkt Ausführung als dieser Schritt trifft.

Wenn die Ausführung abgebrochen wurde, wird die Logik App Engine Endpunkt 'unsubscribe' aufrufen.  Ihre API kann Callback URL dann Abmelden nach Bedarf.

Derzeit unterstützt Logik App Designer nicht entdecken eines Endpunkts Webhook über stolz, um diese Art der Aktion "Webhook" Aktion hinzufügen und geben Sie URL, Kopfzeilen und Nachrichtentext der Anforderung.  Sie können die `@listCallbackUrl()` Workflow-Funktion in eines dieser Felder, um den Rückruf-URL übergeben.

Sehen Sie ein Beispiel eines Musters Webhook in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/blob/master/LogicAppTriggers/Controllers/WebhookTriggerController.cs)

## <a name="triggers"></a>Trigger

Neben Maßnahmen haben Sie Ihre benutzerdefinierte API Act als Trigger Logik-App.  Es gibt zwei Muster können, denen Sie unter Auslösen einer Anwendung Logik folgen:

### <a name="polling-triggers"></a>Abruf-Trigger

Abruf Trigger fungieren wie lange ausgeführt asynchrone Aktionen oben.  Logik App Engine ruft Endpunkt Trigger nach Ablauf einer bestimmten Zeit (je nach 15 Sekunden, Premium, Standard, 1 Minute und 1 Stunde kostenlos SKU).

Gibt es keine Daten, gibt der Trigger ein `202 ACCEPTED` Antwort mit einem `location` und `retry-after` Header.  Allerdings für Trigger empfohlen wird der `location` -Header enthält einen Abfrageparameter `triggerState`.  Dies ist eine Kennung für Ihre API feststellen, wann das letzte Mal die Anwendung Logik ausgelöst.  Daten vorhanden ist, gibt der Trigger ein `200 OK` mit Content-Nutzlast.  Dies löst die Logik-App.

Beispielsweise wurde ich abrufen, um festzustellen, ob eine Datei vorhanden ist können die folgenden Abruf Trigger erstellen:

* Wenn eine Anforderung keine TriggerState erhielt die API zurück ein `202 ACCEPTED` mit einem `location` Header, die TriggerState der aktuellen Uhrzeit und `retry-after` 15.
* Wenn eine Anforderung mit einem TriggerState empfangen wurde:
 * Überprüfen Sie, ob alle Dateien nach TriggerState DateTime hinzugefügt wurden. 
  * Zurückzugeben ist 1 Datei, ein `200 OK` mit Content Nutzlast erhöhen TriggerState DateTime Datei ich zurückgegeben und die `retry-after` 15.
  * Sind mehrere Dateien, kann ich 1 zurück, mit einer `200 OK`, Erhöhen der mein TriggerState in der `location` Kopf- und Set `retry-after` 0.  Auf diese Weise wissen weitere Daten vorhanden sind und es fordert sofort auf Modul können die `location` angegebene.
  * Wenn keine Dateien vorhanden sind, zurückgegeben eine `202 ACCEPTED` Antwort und lässt die `location` TriggerState identisch.  Legen Sie `retry-after` 15.

Sehen Sie ein Beispiel eines Triggers Abruf in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)

### <a name="webhook-triggers"></a>Webhook Trigger

Webhook Trigger fungieren wie Webhook Aktionen oben.  Logik App Engine rufen den Endpunkt'subscribe' Webhook Trigger hinzugefügt und gespeichert wird.  Die API kann Webhook URL registrieren und über HTTP-POST aufrufen, sobald Daten verfügbar sind.  Content Nutzlast und Header werden in die Logik App ausführen übergeben.

Wenn ein Trigger Webhook gelöscht wird (App Logik vollständig oder nur Webhook Trigger), Motor anrufen 'unsubscribe' URL, Ihre API abmelden kann den Rückruf-URL und beenden Sie alle Prozesse nach Bedarf.

Derzeit unterstützt Logik App Designer nicht entdecken Webhook Trigger durch stolz, um diese Art der Aktion "Webhook" Trigger hinzufügen und URL, Kopfzeilen und Nachrichtentext der Anforderung angeben.  Sie können die `@listCallbackUrl()` Workflow-Funktion in eines dieser Felder, um den Rückruf-URL übergeben.

Sehen Sie ein Beispiel eines Triggers Webhook in GitHub [hier](https://github.com/jeffhollan/LogicAppTriggersExample/tree/master/LogicAppTriggers)