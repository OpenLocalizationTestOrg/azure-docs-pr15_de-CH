<properties
   pageTitle="Logik-apps als aufrufbare Endpunkte"
   description="Das Erstellen und konfigurieren Triggers Endpunkten und in eine Anwendung Logik in Azure App Service verwenden"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>


# <a name="logic-apps-as-callable-endpoints"></a>Logik-apps als aufrufbare Endpunkte

Logik Apps kann synchronen HTTP-Endpunkt als Trigger direkt verfügbar machen.  Das Muster aufrufbare Endpunkte können Sie Logik Apps als geschachtelte Workflow über die Aktion "Workflow" in einer Anwendung Logik aufrufen.

Es gibt 3 Arten von Triggern, die Anfragen können:

* Anforderung
* ApiConnectionWebhook
* HttpWebhook

Für den Rest des Artikels werden **Anforderung** als Beispiel alle Prinzipien gelten jedoch gleich 2 Arten von Triggern.

## <a name="adding-a-trigger-to-your-definition"></a>Die Definition hinzufügen ein Triggers
Zunächst wird einen Trigger zur Definition Anwendung Logik hinzufügen, die eingehende Anfragen können.  Sie können im Designer für "HTTP Request" der Trigger Karte suchen. Anfragetext JSON-Schema definieren und Designer generiert Token zum Analysieren und Daten über die manuelle Auslösung durch den Workflow übergeben.  Ich empfehle mit einem Tool wie [jsonschema.net](http://jsonschema.net) generieren eine JSON-Schemas aus Beispiel Körper Nutzlast.

![Trigger Karte][2]

Nach dem Speichern Ihrer Logik-App-Definition wird eine Callback-URL ähnlich generiert:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

Diese URL enthält SAS die Abfrageparameter für die Authentifizierung verwendet.

Sie erhalten auch diesen Endpunkt im Azure-Portal:

![][1]

Oder telefonisch:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>Sicherheit für die Trigger-URL

Logik App Rückruf URLs werden mit einem SAS generiert.  Die Signatur wird als Abfrageparameter durchlaufen und muss vor die Anwendung Logik wird überprüft.  Es wird durch eine eindeutige Kombination eines geheimen Schlüssels Logik-app, der Name des Triggers und durchgeführten Operation generiert.  Sofern ein Benutzer Zugriff auf den geheimen Logik app Schlüssel würden sie keine gültige Signatur generieren können.

## <a name="calling-the-logic-app-triggers-endpoint"></a>Aufrufen der Logik app Trigger Endpunkt

Nach dem Erstellen des Endpunkts für den Trigger lösen Sie über eine `POST` auf die vollständige URL. Sie können zusätzliche Header und Inhalt im Textkörper enthalten.

Der Inhaltstyp ist `application/json` dann Sie können Verweiseigenschaften in der Anforderung. Andernfalls behandelt es als binäre Einheit, die an andere APIs übergeben werden, aber kann nicht ohne Konvertierung des Inhalts innerhalb des Workflows verwiesen.  Angenommen, Sie übergeben `application/xml` Content können `@xpath()` möchten eine Extraktion Xpath oder `@json()` Konvertierung von XML, JSON.  Weitere Informationen zum Arbeiten mit Datentypen [werden hier](app-service-logic-content-type.md)

Darüber hinaus können Sie in der Definition der JSON-Schema angeben. Dadurch wird den Designer Tokens zu generieren, die dann in Schritte übergeben.  Beispielsweise folgende machen einen `title` und `name` Token im Designer verfügbar:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>Verweisen auf den Inhalt der eingehenden Anforderung

Die `@triggerOutputs()` Funktion wird der Inhalt der eingehenden Anforderung ausgegeben. Beispielsweise würde es aussehen:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

Können die `@triggerBody()` Verknüpfung auf der `body` -Eigenschaft explizit. 

## <a name="responding-to-the-request"></a>Auf die Anforderung reagiert

Für einige Abfragen, die eine Anwendung Logik beginnen, sollten Sie einige Inhalte an den Aufrufer reagieren. Es wird ein neuer Aktionstyp aufgerufen **Antwort** der Statuscode, Nachrichtentext und Header für die Antwort verwendet werden kann. Beachten Sie, dass keine **Antwort** Form vorhanden ist, der Logik app Endpunkt wird *sofort* reagiert mit **202 akzeptiert**.

![HTTP-Antwort-Aktion][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Antworten sind die folgenden:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| statusCode | Der HTTP-Statuscode der eingehenden Anforderung reagieren. Gültiger Statuscode möglich, bei denen 2xx 4xx oder 5xx. 3xx Statuscodes sind nicht zulässig. | 
| Körper | Ein Textkörperobjekt, das eine Zeichenfolge sein kann, ein JSON-Objekt oder sogar Binärinhalt zuvor verwiesen. | 
| Kopfzeilen | Sie können eine beliebige Anzahl der Header in der Antwort enthalten sein | 

Alle Schritte in der Logik-app, die für die Antwort müssen innerhalb von *60 Sekunden* für die ursprüngliche Anforderung zu Antworten **, wenn der Workflow als geschachtelte Logik Anwendung aufgerufen wird**. Wenn keine Antwortaktion erreicht wird innerhalb von 60 Sekunden und der eingehenden Anforderung ab, und **408 Client Timeout** -HTTP-Antwort.  Für geschachtelte Logik Apps übergeordnete Anwendung Logik, bis Antwortwartezeit weiterhin dauert unabhängig von der Zeit.

## <a name="advanced-endpoint-configuration"></a>Erweiterte Endpunktkonfiguration

Logik-apps über eine integrierte Unterstützung für den Direktzugriff Endpunkt und immer die `POST` Methode zum Ausführen der Anwendung Logik starten. Die **HTTP-Listener** API app unterstützt zuvor auch URL-Segmenten und die HTTP-Methode ändern. Sie können auch zusätzliche Sicherheit einrichten oder eine benutzerdefinierte Domäne API app Host (Web app, die die API-Anwendung gehostet) hinzu. 

Diese Funktion ist über **API-Management**:
* [Ändern der Anforderung](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [Ändern Sie die URL-Segmenten der Anforderung](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* Die API Management Domänen auf der Registerkarte **Konfigurieren** im klassischen Azure-Portal einrichten
* Richten Sie Richtlinie für die Standardauthentifizierung (**Link benötigt**) überprüfen

## <a name="summary-of-migration-from-2014-12-01-preview"></a>Zusammenfassung der Migration von 2014-12-01-Vorschau

|  2014-12-01-Vorschau | 2016-06-01 |
|---------------------|--------------------|
| Klicken Sie auf **HTTP-Listener** API-app | Klicken Sie auf **Manuelle Auslösung** (keine API-app erforderlich) |
| HTTP-Listener "*Antwort automatisch gesendet*" festlegen | **Eine Reaktion** aufzunehmen oder nicht in der Workflowdefinition |
| Basic oder OAuth Authentifizierung konfigurieren | über API-management |
| Konfigurieren von HTTP-Methode | über API-management |
| Relativen Pfad konfigurieren | über API-management |
| Den eingehenden Text über verweisen`@triggerOutputs().body.Content` | Verweis per`@triggerOutputs().body` |
| Reaktion auf HTTP-Listener **HTTP senden** | Klicken Sie auf **HTTP-Anforderung Antworten** (keine API-app erforderlich)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
