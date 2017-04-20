<properties
    pageTitle="HTTP-Aktion in Logik-apps hinzufügen | Microsoft Azure"
    description="Übersicht über die HTTP-Aktion mit Eigenschaften"
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
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http-action"></a>Erste Schritte mit HTTP-Aktion

Mit HTTP-Aktion können Sie Workflows für Ihre Organisation erweitern und jeder Endpunkt über HTTP kommunizieren.

Sie können:

- Erstellen Sie Logik app Workflows, die bei eine Website, die Sie verwalten (Trigger) aktivieren.
- Jeder Endpunkt über HTTP erweitern Ihre Workflows in anderen Diensten kommunizieren.

Zunächst mithilfe der HTTP-Aktion in einer Anwendung Logik finden Sie unter [Erstellen einer app Logik](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>Verwenden des HTTP-Triggers

Ein Trigger ist ein Ereignis, das zum Starten des Workflows, die in einer Anwendung Logik definiert verwendet werden kann. [Weitere Informationen zu Triggern](connectors-overview.md).

Hier ist eine Sequenz wird der HTTP-Trigger im Designer App Logik einrichten.

1. HTTP-Trigger in Ihrer Anwendung Logik hinzufügen.
2. Geben Sie die Parameter für den HTTP-Endpunkt, den Sie abrufen möchten.
3. Ändern Sie das Wiederholungsintervall auf wie oft Fragen sollten.
4. Jetzt löst die Logik Anwendung mit Inhalten, die bei jeder Kontrolle zurückgegeben wird.

![HTTP-trigger](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>Funktionsweise der HTTP-trigger

Der HTTP-Trigger Ruft einen HTTP-Endpunkt in regelmäßigen Abständen. Standardmäßig HTTP-Antwort code weniger als 300 führt eine Logik Anwendung ausführen. Sie können eine Bedingung in der Codeansicht hinzufügen, die ausgewertet wird, nach dem HTTP-Aufruf um festzustellen, ob die Anwendung Logik ausgelöst werden soll. Hier ist ein Beispiel für einen HTTP-Trigger wird ausgelöst, wenn der zurückgegebene Statuscode größer oder gleich ist `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Einzelheiten über die HTTP-Trigger-Parameter sind auf [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger)verfügbar.

## <a name="use-the-http-action"></a>Die HTTP-Aktion

Eine Aktion ist eine Operation, die vom Workflow ausgeführt wird, die in einer Anwendung Logik definiert. [Weitere Informationen zu Aktionen](connectors-overview.md).

1. Klicken Sie auf **Neuen Schritt** .
2. Wählen Sie **eine Aktion hinzufügen**.
3. Geben Sie im Suchfeld Aktion **http** HTTP-Aktion auflisten.

    ![Wählen Sie die HTTP-Aktion](./media/connectors-native-http/using-action-1.png)

4. Alle Parameter, die für die HTTP-Aufruf hinzufügen.

    ![Die HTTP-Aktivität abgeschlossen](./media/connectors-native-http/using-action-2.png)

5. Klicken Sie links oben auf der Symbolleiste auf Speichern. Die Logik app sowohl speichern und veröffentlichen (aktivieren).

## <a name="http-trigger"></a>HTTP-trigger

Hier werden die Details für den Auslöser dieser Connector unterstützt. Der HTTP-Connector hat einen Trigger.

|Trigger|Beschreibung|
|---|---|
|HTTP|Führt einen HTTP-Aufruf und gibt den Antwortinhalt.|

## <a name="http-action"></a>HTTP-Aktion

Hier werden die Details für die Aktion, die diesen Connector unterstützt. Der HTTP-Connector hat eine mögliche Aktion.

|Aktion|Beschreibung|
|---|---|
|HTTP|Führt einen HTTP-Aufruf und gibt den Antwortinhalt.|

## <a name="http-details"></a>HTTP-details

Die folgenden Tabellen beschreiben die erforderlichen und optionalen Eingabefelder für die Aktion und die entsprechende Ausgabedetails mit der Aktion verbunden.


#### <a name="http-request"></a>HTTP-Anforderung
Im folgenden werden Felder für die Aktion, wodurch eine ausgehende HTTP-Anforderung.
Ein * ein Pflichtfeld ist.

|Angezeigter name|Eigenschaftenname|Beschreibung|
|---|---|---|
|Methode *|-Methode|Das HTTP-Verb verwendet|
|URI *|URI|Der URI der HTTP-Anforderung|
|Kopfzeilen|Kopfzeilen|Ein JSON-Objekt von HTTP-Headern enthalten|
|Körper|Körper|Der Hauptteil der HTTP-Anforderung|
|Authentifizierung|Authentifizierung|Details im Abschnitt [Authentifizierung](#authentication)|
<br>

#### <a name="output-details"></a>Ausgabedetails

Es folgen Ausgabedetails für die HTTP-Antwort.

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Kopfzeilen|Objekt|Antwort-Header|
|Körper|Objekt|Response-Objekt|
|Statuscode|int|HTTP-Statuscode|

## <a name="authentication"></a>Authentifizierung

Logik-Apps Feature von Azure App Service können Sie Authentifizierung für HTTP-Endpunkte verwenden. Diese Authentifizierung können Sie mit den **HTTP**, **[HTTP + stolz](./connectors-native-http-swagger.md)**und **[Http-Webhook](./connectors-native-webhook.md)** . Die folgenden Arten von Authentifizierung werden können konfiguriert:

* [Standardauthentifizierung](#basic-authentication)
* [Clientzertifikatsauthentifizierung](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth Authentifizierung](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Standardauthentifizierung

Das folgende Authentifizierungsobjekt ist für die Standardauthentifizierung erforderlich.
Ein * ein Pflichtfeld ist.

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Typ *|Typ|Authentifizierungstyp (muss `Basic` für die Standardauthentifizierung)|
|Benutzername *|Benutzername|Zu authentifizierende Benutzername|
|Kennwort *|Kennwort|Ein Kennwort zur Authentifizierung|

>[AZURE.TIP] Möchten Sie ein Kennwort verwenden, die aus der Definition der Verwendung abgerufen werden kann ein `securestring` Parameter und `@parameters()` [Workflow Definition Funktion](http://aka.ms/logicappdocs).

So erstellen Sie ein Objekt wie folgt im Authentifizierungsfeld:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Clientzertifikatsauthentifizierung

Das folgende Authentifizierungsobjekt ist für Clientzertifikatsauthentifizierung erforderlich. Ein * ein Pflichtfeld ist.

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Typ *|Typ|Die Art der Authentifizierung (muss `ClientCertificate` für Client-Zertifikate)|
|PFX *|PFX|Die Base64-codierten Inhalt der Datei persönliche Informationsaustausches (PFX)|
|Kennwort *|Kennwort|Das Kennwort für die PFX-Datei zugreifen|

>[AZURE.TIP] Verwenden Sie ein `securestring` Parameter und `@parameters()` [Workflow Definition Funktion](http://aka.ms/logicappdocs) Parameter, die in der Definition nach dem Speichern der Anwendung Logik gelesen werden.

Zum Beispiel:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD OAuth Authentifizierung

Das folgende Authentifizierungsobjekt ist für Azure AD OAuth Authentifizierung erforderlich. Ein * ein Pflichtfeld ist.

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Typ *|Typ|Die Art der Authentifizierung (muss `ActiveDirectoryOAuth` für Azure AD OAuth)|
|Mieter *|Mieter|Der Mieter Bezeichner für Azure AD-Mandanten|
|Zielgruppe *|Zielgruppe|Legen Sie auf`https://management.core.windows.net/`|
|Client -ID *|clientId|Die Client-ID für die Azure AD-Anwendung|
|Geheime *|Geheimnis|Der Schlüssel des Clients, die das Token angefordert werden|

>[AZURE.TIP] Sie können eine `securestring` Parameter und `@parameters()` [Workflow Definition Funktion](http://aka.ms/logicappdocs) Parameter, die in der Definition nach dem Speichern gelesen werden.

Zum Beispiel:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Nächste Schritte

Probieren Sie die Plattform und [erstellen eine Logik](../app-service-logic/app-service-logic-create-a-logic-app.md). Andere verfügbare Anschlüsse Logik Apps erkunden unserer [APIs Liste](apis-list.md)ansehen.
