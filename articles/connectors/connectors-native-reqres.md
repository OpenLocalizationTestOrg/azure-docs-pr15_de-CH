<properties
    pageTitle="Mithilfe von Anforderung und Antwort Aktionen | Microsoft Azure"
    description="Übersicht über die Anforderung und Antwort Trigger und Aktion in einer Azure Logik"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-request-and-response-components"></a>Erste Schritte mit der Anforderung und Antwort

Mit den Komponenten in einer Anwendung Logik Anforderung und Antwort können Sie in Echtzeit auf Ereignisse reagieren.

Beispielsweise können Sie:

- Reagieren Sie auf eine HTTP-Anforderung mit einer lokalen Datenbank über eine Logik-app.
- Eine Anwendung Logik aus einem externen Webhook-Ereignis ausgelöst.
- Rufen Sie eine Logik-app mit einer Anforderung und Antwort Aktion innerhalb einer anderen Anwendung Logik.

Zunächst mit Anforderung und Antwort Aktionen in einer Anwendung Logik finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-request-trigger"></a>Verwenden des Triggers HTTP-Anforderung

Ein Trigger ist ein Ereignis, das zum Starten des Workflows, die in einer Anwendung Logik definiert verwendet werden kann. [Weitere Informationen zu Triggern](connectors-overview.md).

Hier ist eine Beispiel-Sequenz zum Einrichten einer HTTP-Anforderung im Designer App Logik.

1. **Anforderung - Wenn eine HTTP-Anforderung empfangen** Trigger in Ihrer Anwendung Logik hinzufügen. Optional können ein JSON-Schema Sie (mithilfe eines Tools wie [JSONSchema.net](http://jsonschema.net)) für den Hauptteil der Anforderung. Dies ermöglicht dem Designer Token für Eigenschaften in der HTTP-Anforderung generiert.
2. Hinzufügen einer anderen Aktion die Anwendung Logik zu speichern.
3. Nach dem Speichern der Anwendung Logik, erhalten Sie die HTTP-Anfrage-URL von der Anfrage aus.
4. (Ein Tool wie [Postman](https://www.getpostman.com/)können) HTTP POST an den URL löst die Logik-app.

>[AZURE.NOTE] Wenn Sie eine Aktion Reaktion definieren einer `202 ACCEPTED` Antwort sofort an den Aufrufer zurückgegeben. Antwortaktion können Sie eine Antwort.

![Antwort-trigger](./media/connectors-native-reqres/using-trigger.png)

## <a name="use-the-http-response-action"></a>Verwenden Sie die HTTP-Antwort-Aktion

Der HTTP-Antwort-Vorgang ist nur gültig, wenn Sie in einen Workflow verwendet, die von einer HTTP-Anforderung ausgelöst. Wenn Sie eine Aktion Reaktion definieren einer `202 ACCEPTED` Antwort sofort an den Aufrufer zurückgegeben.  Sie können eine Reaktion bei jedem Schritt im Workflow hinzufügen. Die Logik-app nur hält die eingehende Anforderung geöffnet eine Minute lang auf eine Antwort.  Nach einer Minute, wenn keine Antwort vom Workflow gesendet wurde und eine Reaktion in der Definition besteht einer `504 GATEWAY TIMEOUT` an den Aufrufer zurückgegeben.

Hier ist eine HTTP-Antwort-Aktion hinzufügen:

1. Klicken Sie auf **Neuen Schritt** .
2. Wählen Sie **eine Aktion hinzufügen**.
3. Geben Sie im Suchfeld Aktion **Antwort** zum Auflisten der Reaktion.

    ![Wählen Sie die Reaktion](./media/connectors-native-reqres/using-action-1.png)

4. Fügen Sie alle Parameter, die für die HTTP-Antwort angezeigt.

    ![Führen Sie die Reaktion](./media/connectors-native-reqres/using-action-2.png)

5. Klicken Sie auf die linke obere Ecke der Symbolleiste speichern und Ihre Logik app sowohl speichern und veröffentlichen (aktivieren).

## <a name="request-trigger"></a>Trigger anfordern

Hier werden die Details für den Auslöser dieser Connector unterstützt. Gibt ein einzelne Anforderung Trigger.

|Trigger|Beschreibung|
|---|---|
|Anforderung|Tritt ein, wenn eine HTTP-Anforderung empfangen wird|

## <a name="response-action"></a>Reaktion

Hier werden die Details für die Aktion, die diesen Connector unterstützt. Gibt eine Antwort, die Begleitung von Anforderung Trigger nur verwendet werden kann.

|Aktion|Beschreibung|
|---|---|
|Antwort|Gibt eine Antwort auf die korrelierte HTTP-Anforderung|

### <a name="trigger-and-action-details"></a>Trigger und die Aktion

Die folgenden Tabellen beschreiben die Eingabefelder für die Trigger und und die entsprechenden Details ausgeben.

#### <a name="request-trigger"></a>Trigger anfordern
Folgendes ist ein Eingabefeld für den Trigger aus einer eingehenden HTTP-Anforderung.

|Angezeigter name|Eigenschaftenname|Beschreibung|
|---|---|---|
|JSON-Schema|Schema|JSON-Schema der Hauptteil der HTTP-Anforderung|
<br>

**Ausgabedetails**

Es folgen Einzelheiten Ausgabe für die Anforderung.

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Kopfzeilen|Objekt|Anfrage-Header|
|Körper|Objekt|Request-Objekt|

#### <a name="response-action"></a>Reaktion

Im folgenden werden Felder für die HTTP-Antwort-Aktion. Ein * ein Pflichtfeld ist.

|Angezeigter name|Eigenschaftenname|Beschreibung|
|---|---|---|
|Status Code *|statusCode|Der HTTP-Statuscode|
|Kopfzeilen|Kopfzeilen|Ein JSON-Objekt alle Antwortheader enthalten|
|Körper|Körper|Der Antworttext|

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie die Plattform und [erstellen eine Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Andere verfügbare Anschlüsse Logik Apps erkunden unserer [APIs Liste](apis-list.md)ansehen.
