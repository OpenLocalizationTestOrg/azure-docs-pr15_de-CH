<properties
    pageTitle="Azure AD Xamarin erste Schritte | Microsoft Azure"
    description="Wie eine Xamarin Anwendung erstellen, die Integration mit Azure anmelden und ruft Azure AD geschützten APIs mit OAuth."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Azure AD Xamarin App integrieren

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin können Sie mobile apps in C# schreiben, iOS, Android und Windows (mobile Geräte und PCs) ausführen können. Wenn Sie eine Anwendung mit Xamarin erstellen, vereinfacht Azure AD und für Ihre Benutzer ihre Active Directory-Konten zu authentifizieren. Außerdem können die Anwendung sicher alle Web API geschützt von Azure AD Azure-API oder die Office 365-APIs verwenden.

Xamarin-apps, die auf geschützte Ressourcen zugreifen müssen, bietet Azure AD Active Directory Authentifizierungsbibliothek oder ADAL. Zweck des ADAL im Leben ist Ihre app Zugriffstoken zu erleichtern. Um zu veranschaulichen, wie einfach es ist, bauen hier "Verzeichnissuche" app, die wir:

-   Läuft auf iOS, Android, Windows-Desktop, Windows Phone und Windows Store.
- Verwendet eine einzelne portable Klassenbibliothek (PCL) Benutzer authentifizieren und Token für Azure AD Graph-API
-   Sucht in einem Verzeichnis für Benutzer mit einem bestimmten UPN.

Um die gesamte Arbeit der Anwendung zu erstellen, müssen Sie:

2. Xamarin Umgebung einrichten
2. Registrieren Sie Ihre Anwendung in Azure AD.
3. Installieren und Konfigurieren von ADAL.
5. ADAL von Azure AD Token zu verwenden.

Um zu beginnen, [ein Skelett Projekt](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) [herunterladen](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip)oder vollständiges Beispiel. Jeder ist eine Visual Studio 2013-Lösung. Sie benötigen auch einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung. Haben Sie bereits [erfahren Sie, wie man](active-directory-howto-tenant.md)Mieter.

## <a name="0-set-up-your-xamarin-development-environment"></a>*0 Xamarin Umgebung einrichten*
Da dieses Lernprogramm für iOS, Android und Windows umfasst, benötigen Visual Studio Sie und Xamarin zusammen. Zum Erstellen der erforderlichen Umgebung gehen Sie vollständige [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) auf MSDN. Diese enthalten Material können Sie weitere Informationen zu Xamarin Wartezeit für Installationsprogramme abgeschlossen. 

Sie haben einmal erforderliche Einrichtung abgeschlossen, öffnen Sie die Projektmappe in Visual Studio zu beginnen. Finden Sie sechs Projekte: fünf plattformspezifische Projekte und eine portable Klassenbibliothek über alle Plattformen hinweg gemeinsam genutzt werden`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. Anwendung Searcher Verzeichnis registrieren*
Um Ihre app Token zu aktivieren, müssen Sie zunächst in Azure AD-Mandanten registrieren und gewährt Zugriff auf Azure AD Graph-API:

-   Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie einen Mandanten in der Anwendung registriert.
-   Klicken Sie auf die Registerkarte **Applications** , und klicken Sie auf **Hinzufügen** in der unteren Schublade.
-   Befolgen Sie, und erstellen Sie eine neue **Systemeigene Clientanwendung**.
    -   Der **Name** der Anwendung wird die Anwendung an Endbenutzer beschrieben.
    -   **Uri umleiten** ist eine Kombination von Schema und String, mit dem Azure AD token Antworten zurückgeben. Geben Sie einen Wert, z. B. `http://DirectorySearcher`.
-   Nach Abschluss Anmeldung zuweisen AAD app eine eindeutige Client-ID. Sie benötigen diesen Wert in den nächsten Abschnitten so kopieren Sie sie von der Registerkarte **Konfigurieren** .
- Auch der Registerkarte **Konfigurieren** , suchen Sie den Abschnitt "Berechtigungen to Other Applications". Fügen Sie für die Anwendung "Azure Active Directory" die Berechtigung **Verzeichnis Zugriff auf Ihre Organisation** **Delegiert**Berechtigungen hinzu. Dadurch kann die Anwendung die Graph-API für Benutzer Abfragen.

## <a name="2-install--configure-adal"></a>*2. installieren und Konfigurieren von ADAL*
Jetzt haben Sie eine Anwendung in Azure AD können Sie ADAL installieren und identitätsbezogene Code schreiben. Damit ADAL mit Azure kommunizieren müssen Sie einige Informationen über Ihre app-Registrierung.
-   Zunächst ADAL aller Projekte in der Projektmappe mit der Paket-Manager-Konsole.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- Beachten Sie, dass jedes Projekt - zwei Bibliotheksverweise hinzugefügt werden PCL Teil ADAL und einen plattformspezifischen Teil.

-   Öffnen Sie im Projekt DirectorySearcherLib `DirectorySearcher.cs`. Ändern Sie die Klasse um den Werten in der Azure-Verwaltungsportal eingegebenen Werte. Code verweist diese Werte, wenn ADAL verwendet.
    -   Die `tenant` ist die Domäne der Azure AD-Mandanten, z.B. contoso.onmicrosoft.com
    -   Die `clientId` ClientId der Anwendung aus dem Portal kopiert wird.
    - Die `returnUri` ist RedirectUri im Portal, z. B. eingegebene `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. ADAL Token AAD zu verwenden*
*Fast* alle Anwendungslogik Authentifizierung liegt in `DirectorySearcher.SearchByAlias(...)`. Plattform-spezifische Projekte erforderlich ist, kontextbezogenen Parameter übergeben der `DirectorySearcher` PCL.

- Öffnen Sie zuerst `DirectorySearcher.cs` und einen neuen Parameter für die `SearchByAlias(...)` Methode. `IPlatformParameters`ist der kontextbezogene Parameter mit der Plattform-Objekten, die ADAL Authentifizierung durchführen muss.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Als Nächstes initialisieren Sie die `AuthenticationContext` -ADAL der Klasse. Dies ist, wo Sie ADAL mit Azure Kommunikation Koordinaten. Rufen Sie `AcquireTokenAsync(...)`, die akzeptiert die `IPlatformParameters` -Objekt und ruft den Authentifizierungsablauf muss einen Token an die Anwendung zurückgegeben.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`zunächst versucht ein Token für die angeforderte Ressource (in diesem Fall der Diagramm-API) zurück, ohne dass der Benutzer ihre Anmeldeinformationen eingeben (über Zwischenspeichern oder alte Token aktualisieren). Nur wenn nötig, zeigt Benutzer Azure AD Zeichen Seite es vor dem Erwerb des angeforderten Tokens.


- Sie können das Zugriffstoken an Graph-API-Anforderung in der Authorization-Header anfügen:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

Das ist alles für die `DirectorySearcher` PCL und Ihre Anwendung die Identität bezogenen Code.  Ist, rufen Sie die `SearchByAlias(...)` -Methode in jede Plattform Ansichten und dem erforderlichen Code einwandfrei verarbeitet den Benutzeroberfläche Lebenszyklus.

####<a name="android"></a>Android:
- In `MainActivity.cs`, fügen Sie einen Aufruf an `SearchByAlias(...)` Behandlungsroutine für die Schaltfläche:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- Müssen auch überschreiben die `OnActivityResult` Lebenszyklus-Methode keine Authentifizierung weiterleiten leitet an die entsprechende Methode.  ADAL bietet eine Hilfsmethode für diese Android:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Windows-Desktop:
- In `MainWindow.xaml.cs`, lediglich anrufen `SearchByAlias(...)` übergeben ein `WindowInteropHelper` auf des Desktops `PlatformParameters` Objekt:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- In `DirSearchClient_iOSViewController.cs`, iOS `PlatformParameters` Objekt akzeptiert einfach auf View Controller:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Universal Windows:
- Universelle Windows Ins `MainPage.xaml.cs` und die `Search` -Methode auf, die eine in einem freigegebenen Projekt Benutzeroberfläche nach Bedarf aktualisiert.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Herzlichen Glückwunsch! Sie haben nun eine funktionierende Xamarin-Anwendung, mit der Benutzer zu authentifizieren und sicheres Aufrufen von APIs mit OAuth 2.0 auf fünf verschiedenen Plattformen. Wenn nicht bereits geschehen ist jetzt die Zeit Ihren Mandanten mit Benutzern aufgefüllt. Der DirectorySearcher-Anwendung ausführen und dieser Benutzer anmelden. Suchen Sie nach anderen Benutzern basierend auf ihrem UPN.

ADAL erleichtert die gemeinsame bietet in Ihre Anwendung integrieren. Er übernimmt alle Prozesse laufen für Cache-Verwaltung, OAuth Unterstützung Anmeldung UI aktualisieren abgelaufenen Token und mehr Benutzer präsentiert. Wirklich wissen müssen lediglich einen einzigen API-Aufruf `authContext.AcquireToken*(…)`.

Zu Referenzzwecken vollständiges Beispiel (ohne die Konfigurationswerte) bereitgestellt [hier](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Sie können zusätzliche Identität Szenarien nun. Möglicherweise möchten versuchen:

[Sichere .NET Web API Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
