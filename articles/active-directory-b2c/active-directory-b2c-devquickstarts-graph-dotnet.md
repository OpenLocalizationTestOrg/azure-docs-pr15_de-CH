<properties
    pageTitle="Azure Active Directory B2C: Verwenden Sie den Graph API | Microsoft Azure"
    description="Wie die Graph-API für B2C-Mandanten mit eine Anwendungsidentität automatisieren."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure AD B2C: Verwenden Sie den Graph API

Azure Active Directory (Azure AD) B2C Mieter sind sehr groß. Dies bedeutet, dass viele Mieter Management Aufgaben programmgesteuert durchgeführt werden müssen. Eine primäre Beispiel ist Verwaltung. Sie müssen eine vorhandene Benutzerspeicher B2C Mieter migrieren. Sie sollten Registrierung auf der Seite und erstellen Sie Benutzerkonten in Azure AD im Hintergrund. Diese Arten von Aufgaben müssen zum Erstellen, lesen, aktualisieren und Löschen von Benutzerkonten. Diese Aufgaben kann mithilfe von Azure AD Graph-API.

B2C Mieter stehen zwei primäre Modi mit Graph-API.

- Interaktive, einmalige Aufgaben sollten Sie fungieren als Administratorkonto B2C-Mandanten beim Ausführen von Aufgaben. Dieser Modus erfordert Administrator Anmeldeinformationen anmelden, bevor dieser Admin Graph-API Aufrufe durchführen kann.
- Automatisierte, kontinuierliche Aufgaben sollten Sie ein Dienstkonto verwenden, die erforderlichen Berechtigungen zum Durchführen von Verwaltungsaufgaben bieten. In Azure AD können Sie hierzu eine Anwendung registrieren und Azure AD authentifizieren. Dies erfolgt mithilfe einer **ID der Anwendung** , mit [OAuth 2.0 Clientanmeldeinformationen gewähren](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). In diesem Fall fungiert die Anwendung selbst nicht als Mitglied der Graph-API aufrufen.

In diesem Artikel besprechen wir den automatisierte Anwendungsfall durchführen. Um zu demonstrieren, erstellen wir eine .NET 4.5 `B2CGraphClient` führt, die Benutzer erstellen, lesen, aktualisieren und löschen böte. Der Client muss eine Windows-Befehlszeilenschnittstelle (CLI), die verschiedene Methoden aufrufen können. Allerdings ist der Code geschrieben, nicht interaktive, automatische Verhalten.

## <a name="get-an-azure-ad-b2c-tenant"></a>Abrufen einer Azure AD B2C Mieters

Bevor Sie Applikationen oder Benutzer erstellen oder überhaupt mit Azure interagieren können, benötigen Sie einen Azure AD B2C-Mandanten und ein globales Administratorkonto im Mandanten. Wenn Sie einen Mandanten bereits [Erste Schritte mit Azure AD B2C](active-directory-b2c-get-started.md)haben.

## <a name="register-a-service-application-in-your-tenant"></a>Registrieren Sie eine Dienst-Anwendung in Ihrem Mandanten

Nachdem B2C Mieter haben, müssen Sie Ihre Service-Anwendung mithilfe von Azure AD PowerShell-Cmdlets erstellen.
Zuerst downloaden Sie und installieren Sie der- [Microsoft Online Services-In-Assistenten](http://go.microsoft.com/fwlink/?LinkID=286152). Downloaden Sie und installieren Sie die [64-Bit-Azure Active Directory-Moduls für Windows PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
Um die Graph-API mit Ihrem B2C-Mandanten verwenden, müssen Sie eine spezielle Anwendung mit PowerShell registrieren. Führen Sie die Anweisung in diesem Artikel zu tun. Sie können nicht die bereits vorhandenen B2C-Anwendung verwenden, die in Azure-Portal registriert.

Nach der Installation des PowerShell-Moduls öffnen Sie PowerShell und Ihrem B2C-Mandanten an. Nachdem Sie `Get-Credential`, Sie werden aufgefordert, einen Benutzernamen und ein Kennwort, geben Sie den Benutzernamen und das Kennwort des Administratorkontos B2C Mieter.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Bevor Sie die Anwendung erstellen, müssen Sie einen neuen **geheimen**generieren.  Die Anwendung verwendet den geheimen Azure AD authentifizieren und Zugriffstoken. Sie können einen gültigen Schlüssel in PowerShell:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

Der letzte Befehl sollte geheimen neue drucken. Kopieren sie sicher. Sie werden ihn später benötigen. Jetzt können Sie Ihre Anwendung mit den neuen Clientschlüssel als Referenz für die Anwendung erstellen:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Erstellen Sie die Anwendung erfolgreich, sollten Eigenschaften der Anwendung wie die oben drucken. Sie benötigen beide `ObjectId` und `AppPrincipalId`, diese Werte zu kopieren.

Nachdem Sie eine Anwendung in Ihrem B2C-Mandanten erstellen, müssen Sie Berechtigungen zuweisen Benutzer CRUD-Operationen müssen. Anwendung drei Rollen zuweisen: Verzeichnis Leser (Benutzer lesen), Directory Autoren (erstellen und Aktualisieren von Benutzern), und ein Benutzer Kontoadministrator (Benutzer löschen). Diese Rollen sind bekannte Bezeichner ersetzen können die `-RoleMemberObjectId` mit `ObjectId` ab, und führen Sie die folgenden Befehle. Die Liste aller Verzeichnis Rollen führen `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

Jetzt haben eine Anwendung über die Berechtigung zum Erstellen, lesen, aktualisieren und Löschen Benutzer aus Ihrem B2C-Mandanten.

## <a name="download-configure-and-build-the-sample-code"></a>Herunterladen, konfigurieren und Erstellen des Beispielcodes

Zunächst den Beispielcode herunter, und läuft. Dann nehmen wir näher betrachten.  Sie können [den Beispielcode als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Sie können auch in ein Verzeichnis Ihrer Wahl Klonen:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Öffnen der `B2CGraphClient\B2CGraphClient.sln` Visual Studio-Projektmappe in Visual Studio. In der `B2CGraphClient` Projekt, öffnen Sie die Datei `App.config`. Ersetzen Sie drei app-Einstellung durch eigene Werte:

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Als Nächstes mit der rechten Maustaste auf den `B2CGraphClient` Lösung und Wiederherstellung im Beispiel. Wenn Sie erfolgreich sind, müsste nun ein `B2C.exe` ausführbare Datei im `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Erstellen Sie Benutzervorgänge mithilfe der Graph-API

Um die B2CGraphClient zu verwenden, Öffnen einer `cmd` Windows Befehl, und ändern Sie Ihr Verzeichnis der `Debug` Verzeichnis. Führen Sie die `B2C Help` Befehl.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Eine kurze Beschreibung jedes Befehls erscheint. Wenn Sie einen dieser Befehle aufrufen `B2CGraphClient` fordert der Azure AD Graph-API.

### <a name="get-an-access-token"></a>Ein Zugriffstoken abrufen

Jede Anforderung an die Graph-API erfordert ein Zugriffstoken für die Authentifizierung. `B2CGraphClient`Mithilfe der Open Source-Active Directory Authentifizierung Library (ADAL) Zugriffstoken abrufen. ADAL erleichtert die token Übernahme durch eine einfache API und einige wichtige Details wie caching Zugriffstoken kümmern. Sie müssen ADAL Token jedoch zu verwenden. Token erhalten von HTTP-Anfragen erstellen.

> [AZURE.NOTE]
    Dieses Codebeispiel verwendet ADAL v2 um mit Graph-API.  Sie müssen ADAL v2 oder v3 verwenden, um Zugriffstoken abzurufen, die mit der Azure AD Graph-API verwendet werden kann.

Wenn `B2CGraphClient` ausgeführt wird, erstellt eine Instanz der `B2CGraphClient` Klasse. Der Konstruktor für diese Klasse richtet ein Gerüst ADAL Authentifizierung:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Wir verwenden die `B2C Get-User` Befehl als Beispiel. Wenn `B2C Get-User` wird aufgerufen, ohne weiteren Eingaben, die CLI-Aufrufe der `B2CGraphClient.GetAllUsers(...)` Methode. Diese Methode ruft `B2CGraphClient.SendGraphGetRequest(...)`, sendet eine HTTP GET-Anforderung an die Graph-API. Vor dem `B2CGraphClient.SendGraphGetRequest(...)` die GET-Anforderung sendet, wird es zunächst Zugriff token mit ADAL:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Sie erhalten Zugriff token für die Graph-API durch Aufrufen der ADAL `AuthenticationContext.AcquireToken(...)` Methode. ADAL gibt eine `access_token` , das die Identität der Anwendung darstellt.

### <a name="read-users"></a>Benutzer lesen

Wenn Sie möchten eine Liste von Benutzern oder einen bestimmten Benutzer aus der Graph-API können Sie ein HTTP senden `GET` anfordern, die `/users` Endpunkt. Eine Anforderung für alle Benutzer in einem Mandanten sieht folgendermaßen aus:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Um diese Anforderung anzuzeigen, führen Sie Folgendes aus:

 ```
 > B2C Get-User
 ```

Es gibt zwei wichtige Aspekte zu beachten:

- Über ADAL erworben Zugriffstoken hinzugefügt wird die `Authorization` Header mit den `Bearer` Schema.
- B2C Mieter verwenden Sie den Query-Parameter `api-version=1.6`.

Beide dazu erfolgt der `B2CGraphClient.SendGraphGetRequest(...)` Methode:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Erstellen Sie Benutzerkonten für consumer

Beim Erstellen von Benutzerkonten in Ihrem B2C-Mandanten können eine HTTP senden `POST` anfordern, die `/users` Endpunkt:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Die meisten dieser Eigenschaften in dieser Anforderung müssen Consumer erstellen. Um mehr zu erfahren, klicken Sie [hier](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Beachten Sie, dass die `//` Kommentare wurden zur Veranschaulichung. Schließen sie nicht in eine tatsächliche Anforderung.

Um die Anforderung zu sehen, führen Sie einen der folgenden Befehle:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

Die `Create-User` Befehl wird eine JSON-Datei als Eingabeparameter. Eine JSON-Darstellung eines Benutzerobjekts enthält. Gibt es zwei Beispieldateien JSON im Beispielcode: `usertemplate-email.json` und `usertemplate-username.json`. Sie können diese Dateien Ihren Bedürfnissen entsprechend ändern. Neben den erforderlichen Feldern sind unterschiedliche optionale Felder, mit denen Sie, in diesen Dateien enthalten. Optionale Felder [Azure AD Graph-API Entitätsverweis](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity)finden.

Sehen Sie, wie die POST-Anforderung erstellt wird, in `B2CGraphClient.SendGraphPostRequest(...)`.

- Fügt ein Zugriffstoken, das `Authorization` -Header der Anforderung.
- Wird `api-version=1.6`.
- Es enthält das JSON-Benutzerobjekt im Hauptteil der Anforderung.

> [AZURE.NOTE]
Bei Konten, die aus einem vorhandenen Benutzerspeicher migrieren möchten niedrigere Kennwortstärke als [sicheres Kennwortstärke von Azure AD B2C erzwungen](https://msdn.microsoft.com/library/azure/jj943764.aspx)hat können Sie deaktivieren, sicheres Kennwort Anforderung mithilfe der `DisableStrongPassword` Wert der `passwordPolicies` Eigenschaft. Beispielsweise können Sie die oben angegebene erstellen benutzeranforderung: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Consumer-Benutzerkonten aktualisieren

Beim Aktualisieren von Benutzerobjekten ähnelt der Prozess Benutzerobjekte zu verwenden. Aber dieser Prozess verwendet das HTTP- `PATCH` Methode:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Versuchen Sie, einen Benutzer aktualisieren JSON Dateien mit neuen Daten aktualisieren. Anschließend können Sie `B2CGraphClient` einen der folgenden Befehle ausführen:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Überprüfen der `B2CGraphClient.SendGraphPatchRequest(...)` -Methode zum Senden dieser Anforderung.

### <a name="search-users"></a>Suchbenutzer

Sie können Benutzer in Ihrem B2C-Mandanten in einigen Methoden suchen. Einer des Benutzers mit Objekt-ID oder beiden mithilfe des Benutzers anmelden Bezeichner (d. h. die `signInNames` Eigenschaft).

Führen Sie einen der folgenden Befehle für einen bestimmten Benutzer zu suchen:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Hier sind einige Beispiele:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Benutzer löschen

Das Löschen eines Benutzers ist einfach. Das HTTP- `DELETE` -Methode und Konstrukt der URL mit der Objekt ID:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Zu Beispiel, geben diesen Befehl Delete-Anforderung, die an der Konsole ausgegeben:

```
> B2C Delete-User <object-id-of-user>
```

Überprüfen der `B2CGraphClient.SendGraphDeleteRequest(...)` -Methode zum Senden dieser Anforderung.

Sie können viele weitere Aktionen mit der Azure AD Graph-API neben Benutzer ausführen. [Azure AD Graph-API-Referenz](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) enthält Informationen über jede Aktion mit Probe Requests.

## <a name="use-custom-attributes"></a>Benutzerdefinierte Attribute

Die meisten Consumeranwendungen müssen einige benutzerdefinierte Informationen speichern. Eine Möglichkeit hierzu ist ein benutzerdefiniertes Attribut in Ihrem B2C-Mandanten definieren. Sie können dann behandeln dieses Attribut genauso behandeln jede Eigenschaft eines Benutzerobjekts. Sie können das Attribut aktualisieren, löschen Sie das Attribut Abfragen, indem Sie das Attribut, Attributs als Anspruch-in Token und senden.

Ein benutzerdefiniertes Attribut in Ihrem Mandanten B2C erhalten Sie unter [benutzerdefinierte B2C auf](active-directory-b2c-reference-custom-attr.md).

Die B2C-Mandanten mit definierten benutzerdefinierten Attributen anzeigen `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Die Ausgabe dieser Funktionen zeigt die Details jedes benutzerdefinierte Attribut wie:

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Sie können den vollständigen Namen wie `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, als Eigenschaft der Benutzerobjekte.  Die JSON-Datei mit der neuen Eigenschaft und einen Wert für die Eigenschaft aktualisieren und führen:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Mit `B2CGraphClient`, haben eine Dienst-Anwendung, die die B2C Mieter Benutzer programmgesteuert verwalten kann. `B2CGraphClient`verwendet eigene Anwendungsidentität Azure AD Graph-API authentifizieren. Außerdem werden Token mithilfe ein geheimen erhält. Beachten Sie diese Funktionalität in die Anwendung integrieren, einige Kernpunkte für B2C-apps:

- Sie müssen der Anwendung die erforderlichen Berechtigungen in den Mandanten gewährt.
- Jetzt müssen Sie ADAL v2 Zugriffstoken zu verwenden. (Sie können auch Nachrichten direkt senden, ohne eine Bibliothek.)
- Beim Aufrufen der Graph-API verwenden `api-version=1.6`.
- Beim Erstellen und Aktualisieren von Privatanwender müssen einige Eigenschaften, wie oben beschrieben.

Haben Sie Fragen oder Anfragen für Aktionen möchten Sie führen mithilfe der Graph-API auf Ihrem B2C-Mandanten, einen Kommentar zu diesem Artikel oder ein Problem im GitHub Code Sample Repository-Datei.
