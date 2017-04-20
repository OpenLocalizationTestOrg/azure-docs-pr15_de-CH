<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Wie Sie eine Anwendung erstellen, die mithilfe von Azure Active Directory B2C Web API aufruft."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure AD B2C: Aufrufen einer Webs-API von .NET Web app

Mithilfe von Azure Active Directory (Azure AD) B2C können Sie leistungsstarke Self-service-Management bietet webapps und Web-APIs in wenigen Schritten hinzufügen. In diesem Artikel "Aufgabenliste".NET Model-View-Controller (MVC) Web app erstellen, die Web-API aufruft, mit trägertoken

Dieser Artikel behandelt nicht die Implementierung anmelden, Anmeldung und Verwaltung von Profilen mit Azure AD B2C. Nachdem der Benutzer bereits authentifiziert konzentriert aufrufenden Web APIs. Wenn nicht bereits geschehen, sollten Sie lernen Sie die Grundlagen von Azure AD B2C mit [.NET Web app Einführung](active-directory-b2c-devquickstarts-web-dotnet.md) beginnen.

## <a name="get-an-azure-ad-b2c-directory"></a>Ein Azure AD B2C-Verzeichnis abrufen

Vor der Verwendung Azure AD B2C müssen Sie ein Verzeichnis oder Mieter.  Ein Verzeichnis ist ein Container für alle Benutzer, apps und Gruppen.  Haben Sie eine bereits, [B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md) , bevor Sie fortfahren, in diesem Handbuch.

## <a name="create-an-application"></a>Erstellen einer Anwendung

Als Nächstes müssen Sie eine Anwendung in Ihrem B2C-Verzeichnis erstellen. Dies informiert Azure AD, um die sichere Kommunikation mit Ihrer Anwendung. In diesem Fall wird das WebApp und Web-API durch eine einzelne **Anwendung ID**dargestellt werden, da bestehen aus einer logischen Anwendung. [Gehen folgendermaßen Sie vor, um eine Anwendung zu erstellen.](active-directory-b2c-app-registration.md) Achten Sie darauf,:

- **Web app/Web-API** in der Anwendung enthalten.
- Geben Sie `https://localhost:44316/` als **Antwort-URL**. Es ist die Standard-URL für dieses Codebeispiel.
- Kopieren Sie die **ID der Anwendung** , die Ihre Anwendung zugewiesen ist. Sie auch benötigen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Erstellen von Richtlinien

In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md)definiert. Diese Webanwendung enthält drei Identität Erfahrungen: anmelden, anmelden und Profil bearbeiten. Sie müssen eine Richtlinie jedes Typs erstellen gemäß [Richtlinie Referenzartikel](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wenn Sie die drei Richtlinien erstellen, müssen Sie:

- Wählen Sie in der Registrierungsrichtlinie **Anzeigename** und andere Anmeldung Attribute.
- Ansprüche Anwendung **angezeigten Namen** und die **Objekt-ID** in jeder Richtlinie auswählen Sie können auch andere Ansprüche.
- Kopieren Sie den **Namen** jeder Richtlinie nach ihrer Erstellung. Das Präfix muss `b2c_1_`. Sie benötigen diese Richtliniennamen später.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erstellt haben, können Sie Ihre app erstellen.

Beachten Sie, dass dieser Artikel nicht wie mit Richtlinien, die Sie gerade erstellt haben. Informationen über die Funktionsweise von Richtlinien in Azure AD B2C mit [.NET Web app Einführung](active-directory-b2c-devquickstarts-web-dotnet.md)beginnen.

## <a name="download-the-code"></a>Den Code herunterladen

Der Code für dieses Lernprogramm [auf GitHub verwaltet](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet). Zum Beispiel wie Sie, können Sie [das Skelette Projekt als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). Sie können auch das Skelett Klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

Die abgeschlossene Anwendung wird auch [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) oder die `complete` dasselbe Repository-Zweig.

Nach dem Herunterladen des Codes öffnen Sie Visual Studio sln-Datei für den Einstieg.

## <a name="configure-the-task-web-app"></a>Konfigurieren der Task Web app

Zu `TaskWebApp` Kommunikation mit Azure AD B2C müssen einige allgemeine Parameter bereitstellen. In der `TaskWebApp` Projekt, der `web.config` -Datei im Stammverzeichnis des Projekts, und Ersetzen Sie die Werte in der `<appSettings>` Abschnitt. Lassen Sie die `AadInstance`, `RedirectUri`, und `TaskServiceUrl` Werte sind.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Zugriffstoken und rufen Sie die Aufgabe API

In diesem Abschnitt wird behandelt, wie Token während mit Azure AD B2C um ein Web mit Azure AD B2C auch gesichert API zugreifen.

Dieser Artikel behandelt nicht wie die API zu sichern. Um zu erfahren, wie eine Web API sicher Anfragen authentifiziert mit Azure AD B2C, checken Sie [Web API Einstieg Artikel](active-directory-b2c-devquickstarts-api-dotnet.md)aus.

### <a name="save-the-sign-in-token"></a>Zeichen in Token speichern

Zunächst authentifizieren Sie des Benutzers (mithilfe eines Richtlinien) und Azure AD B2C erhalten Sie einen Token.  Wenn Sie nicht sicher, wie Richtlinien ausgeführt werden, zurück und versuchen Sie, die [.NET Web app Einführung](active-directory-b2c-devquickstarts-web-dotnet.md) lernen Sie die Grundlagen von Azure AD B2C.

Öffnen Sie die Datei `App_Start\Startup.Auth.cs`.  Eine wichtige Änderung müssen, um die `OpenIdConnectAuthenticationOptions` -müssen festlegen `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

return new OpenIdConnectAuthenticationOptions
{
    // For each policy, give OWIN the policy-specific metadata address, and
    // set the authentication type to the id of the policy
    MetadataAddress = String.Format(aadInstance, tenant, policy),
    AuthenticationType = policy,

    // These are standard OpenID Connect parameters, with values pulled from web.config
    ClientId = clientId,
    RedirectUri = redirectUri,
    PostLogoutRedirectUri = redirectUri,
    Notifications = new OpenIdConnectAuthenticationNotifications
    {
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Abrufen eines Tokens in Controllern

Die `TasksController` für die Kommunikation mit dem Web-API Senden von HTTP-Anfragen an die API lesen, erstellen und Löschen von Vorgängen verantwortlich ist.  Becuase die API von Azure AD B2C gesichert ist, müssen Sie zuerst das Token abzurufen, die im obigen Schritt gespeichert.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

Die `BootstrapContext` enthält das Vorzeichen Token durch einen B2C Richtlinien übernommen.

### <a name="read-tasks-from-the-web-api"></a>Aufgaben aus dem Web API lesen

Wenn Sie einen Token haben, verknüpfen Sie ihn an den HTTP- `GET` Anfordern der `Authorization` Header sicher aufrufen `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Erstellen und Löschen von Vorgängen im Web API

Folgen dem gleichen Muster beim Senden von `POST` und `DELETE` Anfragen an den Web-API, mit der `BootstrapContext` anmelden Token abrufen. Die Erstellaktion für Sie implementiert. Sie können versuchen, beenden die Löschaktion in `TasksController.cs`.

## <a name="run-the-sample-app"></a>Führen Sie die Beispiel-app

Schließlich erstellen Sie, und führen Sie die Anwendung. Melden Sie sich melden Sie an und erstellen Sie Aufgaben für den angemeldeten Benutzer. Abmelden und als anderer Benutzer anmelden. Erstellen Sie Aufgaben für diesen Benutzer. Beachten Sie, wie Aufgaben gespeicherte pro Benutzer der API sind, da die API das Token die Identität des Benutzers extrahiert erhält.

Zu Referenzzwecken vollständiges Beispiel [als ZIP-Datei bereitgestellt wird](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). Sie können auch von GitHub Klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
