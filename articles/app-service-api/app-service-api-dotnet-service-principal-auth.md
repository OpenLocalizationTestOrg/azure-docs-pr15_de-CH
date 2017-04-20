<properties
    pageTitle="Authentifizierung für API-Apps in Azure App Service principal | Microsoft Azure"
    description="Informationen Sie zu einer API-app in Azure App Service für Dienst-Szenarien."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016" 
    ms.author="rachelap"/>

# <a name="service-principal-authentication-for-api-apps-in-azure-app-service"></a>Authentifizierung für API-Apps in Azure App Service principal

## <a name="overview"></a>Übersicht

Erläutert, wie App-Authentifizierung für den *internen* Zugriff auf API-apps verwenden. Ein interner Szenario ist mit einer API-app vor, der von Anwendungscode sein soll. Die empfohlene Methode zum Implementieren dieses Szenarios in App Service ist Azure AD aufgerufene API-Anwendung zu verwenden. Sie rufen die geschützte API-app mit einer Person, die Sie von Azure AD Abrufen von Anmeldeinformationen Anwendungsidentität (Service principal). Alternativen zum Azure AD verwenden finden Sie im Abschnitt **- Dienst Authentifizierung** [Azure App Service-Authentifizierung (Übersicht)](../app-service/app-service-authentication-overview.md#service-to-service-authentication).

In diesem Artikel erfahren Sie:

* Wie Sie Azure Active Directory (Azure AD) API-app von nicht authentifizierten Zugriff schützen.
* Wie Sie eine geschützte API-app eine API-app WebApp oder mobile Anwendung mit Azure AD Dienstanmeldeinformationen Principal (app-Identität). Informationen aus einer Anwendung Logik verwenden finden Sie [unter App Service Logik Apps Ihre benutzerdefinierte API gehostet](../app-service-logic/app-service-logic-custom-hosted-api.md).
* Wie Sie sicherstellen, dass geschützte API-app angemeldete Benutzer über einen Browser aufgerufen werden kann.
* Wie Sie sicherstellen, dass geschützte API-app nur von bestimmten Azure aufgerufen werden Active Directory-Dienst Prinzipal.

Der Artikel enthält zwei Abschnitte:

* Im Abschnitt [zum Konfigurieren der Authentifizierung in Azure App Service principal](#authconfig) erläutert im Allgemeinen konfigurieren Authentifizierung für alle API-app und geschützte API-app verwenden. Dieser Abschnitt gilt für alle Frameworks von App Service, einschließlich .NET, Node.js und Java unterstützt.

* Beginnend mit dem Abschnitt [fortfahren .NET Einsteiger - Tutorials](#tutorialstart) , schrittweise das Lernprogramm ein "Interner Zugriff" Szenario für eine App Service unter Beispiel konfigurieren. 

## <a id="authconfig"></a>Azure App Service principal Authentifizierung konfigurieren

Dieser Abschnitt enthält allgemeine Informationen, die für jede API-Anwendung gelten. Schritte werden Liste .NET Beispiel Anwendung weiterhin [.NET API-Apps Tutorials](#tutorialstart)finden.

1. Navigieren Sie in [Azure-Portal](https://portal.azure.com/)zu Blatt **Einstellungen** der API-app, das zu schützen, finden im Abschnitt **Features** und klicken Sie auf **Authentifizierung / Autorisierung**.

    ![Authentifizierung/Autorisierung in Azure-portal](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In der **Authentifizierung / Autorisierung** Blade, klicken Sie **auf**.

4. Wählen Sie in der Dropdownliste **Aktion Anforderung nicht authentifiziert** **Azure Active Directory einloggen** .

5. Wählen Sie unter **Authentifizierungsanbieter** **Azure Active Directory**.

    ![Authentifizierung Blade in Azure-portal](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Konfigurieren Sie das Blade **Azure Active Directory Einstellungen** zum Erstellen einer neuen Azure AD-Anwendung oder eine vorhandene Azure AD-Anwendung haben Sie bereits eine, die Sie verwenden möchten.

    Interne Szenarien beinhalten in der Regel eine API-app Aufrufen einer API-app. Sie können separate Azure AD-Applikationen für jede API-app oder einer Azure AD-Anwendung.

    Detaillierte Informationen zum Blatt finden Sie unter [Ihre App mit Azure Active Directory-Konto konfigurieren](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

7. Klicken Sie mit Authentifizierung Anbieter Konfiguration fertig sind auf **OK**.

7. In der **Authentifizierung / Autorisierung** Blade, klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern](./media/app-service-api-dotnet-service-principal-auth/authsave.png)

Wenn dies geschieht, können App Service nur Anfragen von Aufrufern in der Azure AD-Mandanten. Geschützte API-app ist keine Authentifizierung oder Autorisierung Code erforderlich. Trägertoken an API-app mit häufig verwendeten Ansprüche im HTTP-Header übergeben wird und diese Informationen im Code zu überprüfen, ob Anfragen von bestimmten Anrufer wie einen Dienstprinzipal werden gelesen.

Diese Funktionalität Authentifizierung funktioniert für alle Sprachen, die App Service unterstützt, einschließlich .NET, Node.js und Java. 

#### <a name="how-to-consume-the-protected-api-app"></a>Wie Sie geschützte API-app

Der Aufrufer muss einen Azure AD trägertoken mit API-Aufrufe bereitstellen. Um ein trägertoken Service principal Anmeldeinformationen abzurufen, verwendet der Aufrufer Active Directory-Authentifizierung Library (ADAL für [.NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory), [Node.js](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)und [Java](https://github.com/AzureAD/azure-activedirectory-library-for-java)). Um ein Token zu erhalten, bietet der Code, ADAL ADAL die folgende Informationen:

* Der Name der Azure AD-Mandanten.
* Die Client-ID und geheimen (app Schlüssel) von Azure AD app Aufrufer zugeordnet.
* Die Client-ID des zugeordneten geschützten API-app Azure AD-Anwendung. (Nur eine Azure AD-Anwendung verwendet wird, ist dieselbe Client-ID wie der Aufrufer.)

Diese Werte stehen in Azure Anzeigenseiten [Azure-Verwaltungsportal](https://manage.windowsazure.com/).

Nachdem das Token erworben hat, von der Aufrufer mit HTTP-Anfragen der Authorization-Header enthält.  App Service überprüft das Token und Anfragen geschützte API-app erreichen können.

#### <a name="how-to-protect-the-api-app-from-access-by-users-in-the-same-tenant"></a>Die API-app vor dem Zugriff durch Benutzer in demselben Mandanten schützen

Trägertoken für Benutzer in demselben Mandanten gelten für geschützte API-app.  Möchten Sie sicherzustellen, dass nur ein Dienstprinzipal geschützte API-app aufrufen kann, fügen Sie Code in geschützte API-app die folgenden Ansprüche aus dem Token überprüft:

* `appid`sollte die Client-ID der Azure AD-Anwendung, die den Aufrufer zugeordnet ist. 
* `oid`(`objectidentifier`) sollte die wichtigsten Dienst-ID des Aufrufers. 

Zudem Anwendungsdiensts der `objectidentifier` im Header X-MS-CLIENT-PRINZIPAL-ID anfordern.

### <a name="how-to-protect-the-api-app-from-browser-access"></a>Die API-app Browser Zugriff schützen

Wenn Sie Code in die geschützte API-app überprüfen nicht und Verwendung einer separaten Anwendung für geschützte API-app Azure AD unbedingt Azure AD-Anwendung Antwort-URL nicht die API-app-Basis-URL ist. Weist die Antwort-URL direkt auf die geschützte API-app konnte ein Benutzer in der gleichen Azure AD-Mandanten nach API-app, anmelden und erfolgreich aufrufen die API.

## <a id="tutorialstart"></a>Der .NET API-Apps Tutorial Serie

Wenn Sie Node.js oder Java Tutorial Reihe für API-apps sind, fahren Sie mit Abschnitt [Weiter](#next-steps) . 

Der Rest dieses Artikels weiterhin .NET API-Apps-Tutorials und setzt voraus, dass [Benutzer Authentifizierung Lernprogramm](app-service-api-dotnet-user-principal-auth.md) abgeschlossen haben und das Beispiel ausgeführte Anwendung in Azure mit Benutzerauthentifizierung aktiviert.

## <a name="set-up-authentication-in-azure"></a>Authentifizierung in Azure

In diesem Abschnitt App-Dienst konfigurieren, damit die nur HTTP-Anfragen zu der Ebene API-Anwendung können handelt, die gültige Azure AD trägertoken. 

Im folgenden Abschnitt Konfigurieren Sie die Zwischenebene API-app Anwendungsanmeldeinformationen Azure AD senden und erhalten einen trägertoken trägertoken an der Ebenen-API-Anwendung senden. Dieser Prozess wird im Diagramm dargestellt.

![Service-Authentifizierung Diagramm](./media/app-service-api-dotnet-service-principal-auth/appdiagram.png)

Wenn Sie Probleme beim Lernprogramm Richtung ausführen, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) am Ende des Lernprogramms. 

1. [Azure-Portal](https://portal.azure.com/)Blatt **Einstellungen** der API-Anwendung, die Sie erstellt für ToDoListDataAPI (Datenebene) API-app navigieren und **Klicken**.

2. Blatt **Einstellungen** finden Sie im Abschnitt **Features** und klicken Sie dann auf **Authentifizierung / Autorisierung**.

    ![Authentifizierung/Autorisierung in Azure-portal](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In der **Authentifizierung / Autorisierung** Blade, klicken Sie **auf**.

4. Wählen Sie in der Dropdownliste **Aktion Anforderung nicht authentifiziert** **Azure Active Directory einloggen**.

    Dies ist die Einstellung, die App Service sichergestellt wird, die nur Anfragen Reichweite API-app authentifizierte. Für Anfragen, die gültige trägertoken App Service übergibt die Token auf API-App und HTTP-Header mit häufig verwendeten Informationen einfacher Code Verfügung füllt.

5. Klicken Sie unter **Authentifizierungsanbieter**auf **Azure Active Directory**.

    ![Authentifizierung Blade in Azure-portal](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Klicken Sie auf **Express**Blatt **Azure Active Directory Settings** .

    Mit der **Express** kann Option Azure automatisch AAD-Anwendung in Ihrem Azure AD- [Mandanten](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)erstellen. 

    Sie müssen einen Mandanten erstellen, da jede Azure-Konto automatisch hat.

7. Klicken Sie unter **Verwaltungsmodus** **Erstellen neue AD App** Wenn sie nicht bereits ausgewählt ist.

    Das Portal Stecker **App erstellen** Feld mit einem Standardwert. Standardmäßig heißt Azure AD-Anwendung die API-app identisch. Alternativ können Sie einen anderen Namen eingeben.
    
    ![Azure AD-Optionen](./media/app-service-api-dotnet-service-principal-auth/aadsettings.png)

    **Hinweis**: Alternativ können Sie ein einzelnes Azure AD für die aufrufende Anwendung API und geschützte API-app. Wenn Sie diese Alternative wählen, benötigen Sie eine Azure AD-Anwendung bereits zuvor in diesem Lernprogramm der Benutzer Authentifizierung erstellt nicht hier die Option **Neue AD App erstellen** . In diesem Lernprogramm verwenden Sie separate Azure AD-Applikationen für die aufrufende Anwendung API und geschützte API-app.

8. Notieren Sie den Wert im Feld **App erstellen** . Sie sehen diese AAD Anwendung in Azure-Verwaltungsportal später.

7. Klicken Sie auf **OK**.

10. In der **Authentifizierung / Autorisierung** Blade, klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern](./media/app-service-api-dotnet-service-principal-auth/saveauth.png)

    App Service erstellt eine Anwendung Active Directory Azure **Anmelden** URL und **Antwort-URL** automatisch auf die URL der API-Anwendung festgelegt. Dieser Wert ermöglicht Benutzern in Ihrem Mandanten AAD und die API-App.

### <a name="verify-that-the-api-app-is-protected"></a>Überprüfen Sie, ob API-app geschützte

1. Wechseln Sie im Browser auf die URL der API-app: Blatt **API-app** im Azure-Portal klicken Sie unter **URL**. 

    Sie werden eine Anmeldeseite umgeleitet, da nicht authentifizierte Anfragen dürfen die API-app erreichen. 

    Wenn Ihr Browser zur Benutzeroberfläche Swagger geht, Ihr Browser möglicherweise bereits angemeldet-in diesem Fall öffnen ein Fensters InPrivate oder Inkognito und Swagger UI URL.

18. Melden Sie sich mit den Anmeldeinformationen eines Benutzers in Ihrem Mandanten AAD.

    Wenn Sie angemeldet sind, wird die "erfolgreich" Seite im Browser angezeigt.

## <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Konfigurieren Sie das Projekt ToDoListAPI und Azure AD-Token senden

In diesem Abschnitt werden die folgenden Aufgaben ausführen:

* Fügen Sie Code in der mittleren Ebene API-app, die Azure AD Anwendungsanmeldeinformationen einen Token und Senden von HTTP-Anfragen auf der Ebene API-Anwendung verwendet.
* Azure AD erhalten Sie benötigten Anmeldeinformationen.
* Geben Sie die Anmeldeinformationen in Azure App Service Runtime-umgebungseinstellungen in der mittleren Ebene API-app. 

### <a name="configure-the-todolistapi-project-to-acquire-and-send-the-azure-ad-token"></a>Konfigurieren Sie das Projekt ToDoListAPI und Azure AD-Token senden

Änderungen Sie die folgenden im ToDoListAPI-Projekt in Visual Studio.

1. Kommentieren Sie den Code in der Datei *ServicePrincipal.cs* .

    Dies ist der Code ADAL für .NET zu Azure AD trägertoken verwendet.  Es verwendet mehrere Werte, die später in der Azure-Runtime-Umgebung festgelegt werden. Hier ist der Code: 

        public static class ServicePrincipal
        {
            static string authority = ConfigurationManager.AppSettings["ida:Authority"];
            static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
            static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
            static string resource = ConfigurationManager.AppSettings["ida:Resource"];
        
            public static AuthenticationResult GetS2SAccessTokenForProdMSA()
            {
                return GetS2SAccessToken(authority, resource, clientId, clientSecret);
            }
        
            static AuthenticationResult GetS2SAccessToken(string authority, string resource, string clientId, string clientSecret)
            {
                var clientCredential = new ClientCredential(clientId, clientSecret);
                AuthenticationContext context = new AuthenticationContext(authority, false);
                AuthenticationResult authenticationResult = context.AcquireToken(
                    resource,
                    clientCredential);
                return authenticationResult;
            }
        }

    **Hinweis:** Dieser Code erfordert die ADAL für .NET NuGet-Paket (Microsoft.IdentityModel.Clients.ActiveDirectory), die im Projekt bereits installiert ist. Wenn Sie das Projekt neu erstellen, müssten Sie dieses Paket installieren. Dieses Paket wird nicht automatisch von der API-app neues Projekt Vorlage installiert.

2. Kommentieren Sie *Controller-ToDoListController*den Code in der `NewDataAPIClient` -Methode, die das Token an HTTP-Fügt im Autorisierungsheader anfordert.

        client.HttpClient.DefaultRequestHeaders.Authorization =
            new AuthenticationHeaderValue("Bearer", ServicePrincipal.GetS2SAccessTokenForProdMSA().AccessToken);

3. Bereitstellen des Projekts ToDoListAPI. (Maustaste auf das Projekt, und klicken Sie auf **Veröffentlichen > Veröffentlichen**.)

    Visual Studio stellt das Projekt und öffnet einen Browser Web app-Basis-URL. Dies zeigt eine Fehlerseite 403 normal Versuch zu einem Web API-Basis-URL in einem Browser.

4. Schließen Sie den Browser.

### <a name="get-azure-ad-configuration-values"></a>Azure AD-Konfigurationswerte abrufen

11. Wechseln Sie in [Azure-Verwaltungsportal](https://manage.windowsazure.com/)zu **Azure Active Directory**.

12. Klicken Sie auf der Registerkarte **Verzeichnis** AAD-Mandanten.

14. Klicken Sie auf **Applikationen > Applications Mein Unternehmen**, und klicken Sie dann auf das Häkchen.

15. Klicken Sie in der Liste der Programme auf Azure für Sie erstellt, wenn Authentifizierung für ToDoListDataAPI (Datenebene) API-Anwendung aktiviert.

16. Klicken Sie auf die Registerkarte **Konfigurieren** .

5. Kopieren Sie den **Client-ID** -Wert und speichern Sie es irgendwo können Sie es später. 

8. Im klassischen Azure-Portal zurück zu der Liste der **Programme Mein Unternehmen gehören**und auf AAD-Anwendung, die für die mittlere Ebene ToDoListAPI API-app (die in der vorherigen praktischen Einführung erstellte, nicht in diesem Lernprogramm erstellte) erstellt.

16. Klicken Sie auf die Registerkarte **Konfigurieren** .

5. Kopieren Sie den **Client-ID** -Wert und speichern Sie es irgendwo können Sie es später.

6. Wählen Sie unter **Schlüssel** **1 Jahr** aus der Dropdownliste **Dauer** .

6. Klicken Sie auf **Speichern**.

    ![App-Schlüssel generieren](./media/app-service-api-dotnet-service-principal-auth/genkey.png)

7. Kopieren Sie den Wert, und speichern sie irgendwo können Sie es später.

    ![Neue app-Schlüssel kopieren](./media/app-service-api-dotnet-service-principal-auth/genkeycopy.png)

### <a name="configure-azure-ad-settings-in-the-middle-tier-api-apps-runtime-environment"></a>Einstellungen Sie Azure AD-in der mittleren Ebene API app Runtime-Umgebung

1. Zum [Azure-Portal](https://portal.azure.com/), und navigieren Sie zu der **API-App** Blade für API-Anwendung, die das Projekt TodoListAPI (mittlere Ebene) hostet.

2. Klicken Sie auf **Settings > Einstellungen**.

3. Fügen Sie in der **App** -Einstellungen die folgenden Schlüssel und Werte:

  	| **Schlüssel** | IDA: Behörde |
  	|---|---|
  	| **Wert** | https://Login.microsoftonline.com/ {Ihre Azure AD Mieter Name} |
  	| **Beispiel** | https://Login.microsoftonline.com/contoso.onmicrosoft.com |

  	| **Schlüssel** | IDA: ClientId |
  	|---|---|
  	| **Wert** | Client-ID der aufrufenden Anwendung (mittlere Schicht - ToDoListAPI) |
  	| **Beispiel** | 960adec2-b74a-484a-960adec2-b74a-484a |

  	| **Schlüssel** | IDA: ClientSecret |
  	|---|---|
  	| **Wert** | App-Schlüssel der aufrufenden Anwendung (mittlere Schicht - ToDoListAPI) |
  	| **Beispiel** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

  	| **Schlüssel** | IDA: Ressource |
  	|---|---|
  	| **Wert** | Client-ID für die aufgerufene Anwendung (Datenebene - ToDoListDataAPI) |
  	| **Beispiel** | e65e8fc9-5f6b-48e8-e65e8fc9-5f6b-48e8 |

    **Hinweis**: für `ida:Resource`, verwenden Sie unbedingt die aufgerufene Anwendung **Client-ID** und nicht des **App-ID-URI**.

    `ida:ClientId`und `ida:Resource` sind unterschiedliche Werte für dieses Lernprogramm da Verwendung trennen Azure AD Applicaations für den mittleren und Datenebene. Bei Verwendung einer Azure AD-Anwendung für die aufrufende Anwendung API und geschützte API-app verwenden Sie denselben Wert in beiden `ida:ClientId` und `ida:Resource`.

    Der Code verwendet die ConfigurationManager, diese Werte abzurufen, damit sie in der Datei Web.config des Projekts oder in Azure Runtime-Umgebung gespeichert werden können. Während der Ausführung einer Anwendung ASP.NET in Azure App Service überschreiben umgebungseinstellungen automatisch in Web.config aus. Umgebung Einstellungen im Allgemeinen eine [sicherere Möglichkeit zum Speichern von vertraulichen Informationen im Vergleich zu einer Web.config-Datei](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

6. Klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern](./media/app-service-api-dotnet-service-principal-auth/appsettings.png)

### <a name="test-the-application"></a>Testen der Anwendung

1. Gehen Sie in einem Browser auf die HTTPS-URL AngularJS front-End-Web-App.

2. Klicken Sie auf die Registerkarte **Aufgabenliste** und mit Anmeldeinformationen für einen Benutzer in Azure AD-Mandanten. 

4. Fügen Sie Aufgabe hinzu, um sicherzustellen, dass die Anwendung funktioniert.

    ![Seite Aufgabenliste](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

    Wenn die Anwendung nicht wie erwartet funktioniert, überprüfen Sie alle eingegebenen Azure-Portal. Wenn alle Einstellungen korrekt zu sein scheinen, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) später in diesem Lernprogramm.

## <a name="protect-the-api-app-from-browser-access"></a>Die API-app Browserzugriff zu schützen

In diesem Lernprogramm erstellt eine Separate Azure AD-Anwendung für ToDoListDataAPI (Datenebene) API-app. Wie Sie wissen App Service eine Anwendung AAD erstellt, konfiguriert API-app-URL in einem Browser und melden Sie den Benutzer können so AAD-Anwendung. Das bedeutet, dass ein Benutzer in der Azure AD-Mandanten nicht nur Service principal auf die API zugreifen kann. 

Browserzugriff ohne Schreiben von Code in die geschützte API-app verhindert werden soll, können Sie die **Antwort-URL** der Anwendung AAD ändern, sodass von API-app-Basis-URL unterscheidet. 

### <a name="disable-browser-access"></a>Browserzugriff deaktivieren

1. Ändern Sie den Wert im Feld **Antwort-URL** das Verwaltungsportal **Konfigurieren** Registerkarte AAD-Anwendung, die für die TodoListService erstellt wurde, sodass eine gültige URL jedoch nicht die API-app-URL ist.
 
2. Klicken Sie auf **Speichern**.

### <a name="verify-browser-access-no-longer-works"></a>Überprüfen Sie, ob Browserzugriff funktioniert nicht mehr

Zuvor überprüft Sie, dass Sie die API-app-URL in einem Browser wechseln können durch einen einzelnen Benutzer-Anmeldeinformationen anmelden. In diesem Abschnitt überprüfen Sie, dass dies nicht möglich ist. 

1. Gehen Sie in einem neuen Browserfenster wieder auf die URL der API-app.

2. Melden Sie möchten an, wenn Sie dazu aufgefordert werden.

3. Anmeldung erfolgreich führt aber zu einer Fehlerseite.

    AAD app haben konfiguriert werden, sodass Benutzer in AAD-Mandanten nicht anmelden und Zugriff auf die API von einem Browser aus. Sie können weiterhin API-app zugreifen, mit einem Service principal Token überprüfen können Web app-URL und weitere Aufgaben hinzufügen.

## <a name="restrict-access-to-a-particular-service-principal"></a>Beschränken des Zugriffs auf einen bestimmten Prinzipal  

Jetzt alle Anrufer, die einen Token für einen Benutzer abrufen kann oder Dienstprinzipalnamen in Ihrem Mandanten Azure AD rufen TodoListDataAPI (Datenebene) API-app. Sie möchten sicherstellen, dass die Ebenen-API-Anwendung nur Anrufe von TodoListAPI (mittlere Ebene) API-app und nur von bestimmten Dienstprinzipalnamen akzeptiert. 

Sie können diese Einschränkung durch Hinzufügen von Code zum Überprüfen der `appid` und `objectidentifier` Ansprüche bei eingehenden Anrufen.

In diesem Lernprogramm legen Sie den Code, der app und Service principal-ID auf Controller-Aktionen überprüft  Alternativen zum Verwenden einer benutzerdefinierten sind `Authorize` -Attribut oder Startlaufwerks Sequenzen möchten diese Überprüfung (z. B. owin-Middleware). Ein Beispiel finden Sie [diese Anwendung](https://github.com/mohitsriv/EasyAuthMultiTierSample/blob/master/MyDashDataAPI/Startup.cs). 

Ändern der folgenden TodoListDataAPI-Projekt.

2. Öffnen Sie die Datei *Controllers/TodoListController.cs* .

3. Kommentieren Sie die Zeilen festgelegt `trustedCallerClientId` und `trustedCallerServicePrincipalId`.

        private static string trustedCallerClientId = ConfigurationManager.AppSettings["todo:TrustedCallerClientId"];
        private static string trustedCallerServicePrincipalId = ConfigurationManager.AppSettings["todo:TrustedCallerServicePrincipalId"];

4. Kommentieren Sie den Code in der Methode CheckCallerId. Diese Methode wird am Anfang jeder Aktionsmethode im Controller. 

        private static void CheckCallerId()
        {
            string currentCallerClientId = ClaimsPrincipal.Current.FindFirst("appid").Value;
            string currentCallerServicePrincipalId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            if (currentCallerClientId != trustedCallerClientId || currentCallerServicePrincipalId != trustedCallerServicePrincipalId)
            {
                throw new HttpResponseException(new HttpResponseMessage { StatusCode = HttpStatusCode.Unauthorized, ReasonPhrase = "The appID or service principal ID is not the expected value." });
            }
        }

5. Das Projekt ToDoListDataAPI Azure App Service erneut.

6. In Ihrem Browser AngularJS front-End Web app HTTPS-URL wechseln und auf die Registerkarte **Aufgaben** .

    Die Anwendung funktioniert nicht, da Aufrufe an den Back-End fehlschlagen. Der neue Code prüft tatsächliche Appid und Objectidentifier aber nicht noch die Werte gegen überprüfen. Der Browser Developer Toolskonsole meldet, dass der Server einen HTTP-Fehler 401 zurückgibt.

    ![Fehler in Developer Tools Konsole](./media/app-service-api-dotnet-service-principal-auth/webapperror.png)

    In den folgenden Schritten konfigurieren Sie den erwarteten Werten.

8. Mithilfe von Azure AD PowerShell, ruft den Wert der Dienstprinzipalnamen für Azure AD-Anwendung, die für das TodoListWebApp-Projekt erstellt.

    ein. Informationen Azure PowerShell installieren und Ihr Abonnement finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).

    b. Um eine Liste der Service-Prinzipale zu erhalten, führen die `Login-AzureRmAccount` Befehl und die `Get-AzureRmADServicePrincipal` Befehl.

    c. Dienstprinzipalnamen Anwendung TodoListAPI suchen Sie Objectid und speichern Sie sie an einem Speicherort, dem Sie später kopieren können.

7. Navigieren Sie in Azure-Portal an die API-app-Blade für API-Anwendung, der das ToDoListDataAPI-Projekt bereitgestellt.

9. Klicken Sie auf **Settings > Einstellungen**.

3. Fügen Sie in der **App** -Einstellungen die folgenden Schlüssel und Werte:

  	| **Schlüssel** | TODO:TrustedCallerServicePrincipalId |
  	|---|---|
  	| **Wert** | Service principal Id der aufrufenden Anwendung |
  	| **Beispiel** | 4f4a94a4-6f0d-4072-4f4a94a4-6f0d-4072 |

  	| **Schlüssel** | TODO:TrustedCallerClientId |
  	|---|---|
  	| **Wert** | Client-ID des aufrufenden Anwendung - Anwendung TodoListAPI Azure AD kopiert |
  	| **Beispiel** | 960adec2-b74a-484a-960adec2-b74a-484a |

6. Klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern](./media/app-service-api-dotnet-service-principal-auth/trustedcaller.png)

6. In Ihrem Browser zurück zum Web app-URL und auf die Registerkarte **Aufgaben** .

    Dieses Mal funktioniert die Anwendung wie erwartet, da vertrauenswürdiger Aufrufer app und Service principal-ID werden die erwarteten Werte.

    ![Seite Aufgabenliste](./media/app-service-api-dotnet-service-principal-auth/mvchome.png)

## <a name="building-the-projects-from-scratch"></a>Erstellen von Projekten neu

Zwei Web API Projekte wurden unter Verwendung der Projektvorlage **Azure API-App** und ersetzt den Standard-Domänencontroller-Werte mit einer Aufgabenliste erstellt. Für den Erwerb von Azure AD Service principal Token im ToDoListAPI-Projekt, wurde [Active Directory Authentifizierung Library (ADAL) für .NET](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet-Paket installiert.
 
Weitere Informationen zum Erstellen einer einseitigen Anwendung AngularJS mit Web API-Back-End wie ToDoListAngular [Hände auf Lab: Erstellen Sie eine einzelne Seite Anwendung (SPA) Angular.js mit ASP.NET Web API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Informationen zum Azure AD Authentifizierungscode hinzufügen finden Sie unter [Securing AngularJS einzelne Seite Apps mit Azure](../active-directory/active-directory-devquickstarts-angular.md).

## <a name="troubleshooting"></a>Problembehandlung

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Stellen Sie sicher, dass ToDoListAPI (mittlere Stufe) und ToDoListDataAPI (Datenebene) verwechseln. Zum Beispiel in diesem Lernprogramm hinzufügen Authentifizierung Data Tier API App **aber app-Schlüssel muss aus der Azure AD-Anwendung, die Sie für die mittlere Ebene API-Anwendung erstellt**.

## <a name="next-steps"></a>Nächste Schritte

Dies ist das letzte Lernprogramm nacheinander API-Apps. 

Weitere Informationen zu Active Directory Azure finden Sie unter den folgenden Ressourcen.

* [Azure AD Entwicklerhandbuch](http://aka.ms/aaddev)
* [Azure AD-Szenarien](http://aka.ms/aadscenarios)
* [Azure AD-Beispiele](http://aka.ms/aadsamples)

    [WebApp-WebAPI-OAuth2-AppIdentity-DotNet](http://github.com/AzureADSamples/WebApp-WebAPI-OAuth2-AppIdentity-DotNet) -Beispiel ist ähnlich wie in diesem Lernprogramm, aber ohne App-Authentifizierung angezeigt wird.

Informationen zum Visual Studio-Projekte auf API-apps mithilfe von Visual Studio oder durch [Automatisierung der Bereitstellung](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) von einem [Quellcodeverwaltungssystem](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)bereitstellen finden Sie unter [Bereitstellen einer Azure App Service-app](../app-service-web/web-sites-deploy.md).
