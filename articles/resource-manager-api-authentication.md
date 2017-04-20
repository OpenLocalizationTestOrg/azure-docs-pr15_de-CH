<properties 
   pageTitle="Active Directory-Authentifizierung und Ressourcenmanager | Microsoft Azure"
   description="Ein Entwicklerhandbuch Azure-Ressourcen-Manager-API und Active Directory-Authentifizierung für andere Azure-Abonnements eine app integrieren."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="dugill;tomfitz" />

# <a name="how-to-use-azure-active-directory-and-resource-manager-to-manage-a-customers-resources"></a>Wie mit Azure Active Directory und Ressourcenmanager Ressourcen des Kunden

## <a name="introduction"></a>Einführung

Wenn Sie ein Softwareentwickler sind, muss eine Anwendung erstellen, die Kunden Azure Ressourcen verwaltet, veranschaulicht dieses Thema Azure Ressourcenmanager APIs Authentifizierung und Zugriff auf Ressourcen in anderen Abonnements. 

Ihre app kann die Ressourcen-Manager-APIs auf verschiedene Weise zugreifen:

1. **Benutzer + app Zugriff**: für apps, die für einen angemeldeten Benutzer auf Ressourcen zugreifen. Dieser Ansatz funktioniert für apps, wie Web-apps und Befehlszeilentools, die nur "Interaktive" Azure-Ressourcen Verwaltung.
1. **Nur App**: für apps, Daemon Services und geplante Aufträge ausgeführt. Die app-Identität wird direkten Zugriff auf die Ressourcen gewährt. Dieser Ansatz funktioniert für apps, die langfristige "offline" in Azure zugreifen.

Dieses Thema enthält eine schrittweise Anleitung zum Erstellen einer Anwendung, die beide diese Autorisierung Methoden. Es veranschaulicht führen Sie jeden Schritt mit REST-API oder C#. Die vollständige ASP.NET MVC-Anwendung steht unter [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense)zur Verfügung.

Der Code für dieses Thema Web App ausgeführt wird, die am [http://vipswapper.azurewebsites.net/cloudsense](http://vipswapper.azurewebsites.net/cloudsense)ausprobieren können. 

## <a name="what-the-web-app-does"></a>Was bedeutet Web app

Die Web-app:

1. Zeichen in Azure Benutzer.
2. Fordert Benutzer Web app Zugriff auf Ressourcen-Manager gewähren.
3. Ruft Benutzer + app Zugriffstoken für den Zugriff auf Ressourcen-Manager.
4. Token (Schritt 3) Ressourcen-Manager und weisen die app Dienstprinzipalnamen bei Abonnements, die die app langfristigen Zugriff auf das Abonnement verwendet.
5. Ruft die app nur token.
6. Token (Schritt 5) zum Verwalten von Ressourcen im Abonnement über Ressourcen-Manager verwendet.

Hier ist die End-to-End-Anwendung.

![Ressourcenmanager Authentifizierungsablauf](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Als Benutzer geben Sie die Abonnement-Id für das Abonnement, das Sie verwenden möchten:

![Abonnement-Id bereitstellen](./media/resource-manager-api-authentication/sample-ux-1.png)

Wählen Sie das Konto für die Anmeldung verwenden.

![Konto wählen](./media/resource-manager-api-authentication/sample-ux-2.png)

Geben Sie Ihre Anmeldeinformationen.

![Anmeldeinformationen](./media/resource-manager-api-authentication/sample-ux-3.png)

Gewähren Sie des Anwendung Zugriffs auf Ihre Azure-Abonnements:
 
![Zugriff gewähren](./media/resource-manager-api-authentication/sample-ux-4.png)
 
Verwalten Sie Ihrer Abonnements verbunden:

![Abonnement verbinden](./media/resource-manager-api-authentication/sample-ux-7.png)


## <a name="register-application"></a>Registrieren der Anwendung

Vor der Codierung registrieren Sie Ihrer Anwendung mit Azure Active Directory (AD). App-Registrierung erstellt eine zentrale Identität für Ihre Anwendung in Azure AD. Sie enthält grundlegende Informationen über die Anwendung wie OAuth-Client-ID Antwort URLs und Anmeldeinformationen, die Ihre Anwendung authentifizieren und Azure-Ressourcen-Manager-APIs zugreifen. App-Registrierung zeichnet auch verschiedene Stellvertretungen, die Zugriff auf Microsoft-APIs für den Benutzer die Anwendung benötigt. 

Da Ihre Anwendung andere Abonnement zugreift, müssen Sie es als Multi-Tenant-Anwendung konfigurieren. Damit die Validierung gültig Bereitstellen einer Domäne Active Directory zugeordnet. Melden Sie sich mit Active Directory verknüpften Domänen finden [Verwaltungsportal](https://manage.windowsazure.com)an. Wählen Sie Active Directory und dann **Domänen**.

Im folgenden Beispiel wird veranschaulicht, wie sich die Anwendung mithilfe von Azure PowerShell. Sie müssen die neueste Version (August 2016) von Azure PowerShell für diesen Befehl funktioniert. 

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
    
Anmelden als die Anwendung benötigen Sie Anwendung-Id und Kennwort. Verwenden Sie dazu die Id, die aus dem vorherigen Befehl zurückgegeben wird

    $app.ApplicationId

Im folgenden Beispiel wird veranschaulicht, wie sich die Anwendung mithilfe von Azure-CLI. 

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

Die Ergebnisse enthalten die AppId müssen bei der Anwendung.

### <a name="optional-configuration---certificate-credential"></a>Optionale Konfiguration - Zertifikats Anmeldeinformationen

Azure AD Zertifikatanmeldeinformationen auch für Applikationen unterstützt: ein selbstsigniertes Zertifikat erstellen, halten Sie den privaten Schlüssel und Ihre Registrierung Azure AD-Anwendung den öffentlichen Schlüssel hinzufügen. Für die Authentifizierung Ihrer Anwendung sendet eine kleine Nutzlast Azure AD mit Ihrem privaten Schlüssel signiert und Azure AD überprüft die Signatur mit dem öffentlichen Schlüssel, den Sie registriert.

Finden Sie Informationen zum Erstellen einer AD-Anwendung mit einem Zertifikat [Verwenden Azure PowerShell Dienstprinzipal Zugriff auf Ressourcen erstellen](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) oder [Verwenden Azure CLI Dienstprinzipal Zugriff auf Ressourcen zu erstellen](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Abonnement-Id erhalten Sie Mandanten-id

Anfordern ein Tokens, mit der Ressourcen-Manager aufrufen, muss die Anwendung wissen, die Mandanten-ID Azure AD-Mandanten, der Azure-Abonnement hostet. Wahrscheinlich Benutzer wissen ihre Abonnement-Ids, aber sie wissen nicht, ihre Mieter Ids für Active Directory. Bitten Sie den Benutzer für die Abonnement-Id, zu Mandanten-Id des Benutzers. Geben Sie die Abonnement-Id beim Senden einer Anforderung zum Abonnement:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Der Benutzer ist nicht angemeldet noch, da die Mandanten-Id aus der Antwort abrufen, schlägt die Anforderung fehl. Diese Ausnahme rufen Sie die Mandanten-Id aus der Wert des Antwortheaders für **WWW-Authenticate ab**. Diese Implementierung der Methode [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) angezeigt.

## <a name="get-user--app-access-token"></a>Benutzer + app Zugriffstoken abrufen

Die Anwendung leitet den Benutzer zu Azure AD mit einer OAuth 2.0 autorisieren Anforderung - Anmeldeinformationen des Benutzers zu authentifizieren und einen Autorisierungscode. Die Anwendung verwendet den Autorisierungscode ein token für den Ressourcen-Manager zugreifen. Die [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) -Methode erstellt die Authentifizierungsanfrage.

Dieses Thema zeigt die REST API-Anfragen zur Authentifizierung des Benutzers. Hilfsbibliotheken können Sie Authentifizierung im Code führen. Weitere Informationen zu Bibliotheken finden Sie unter [Azure Active Directory Authentifizierungsbibliotheken](./active-directory/active-directory-authentication-libraries.md). Hinweise zur Integration von Identitätsmanagement in einer Anwendung finden Sie unter [Active Directory Azure-Entwicklerhandbuch](./active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Auth-Anforderung (OAuth 2.0)

Geben Sie eine Open-ID Connect-OAuth2.0 autorisieren Anforderung Azure AD autorisieren Endpunkt:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Die Abfragezeichenfolgen-Parameter, die für diese Anforderung werden in der [Anforderung einen Autorisierungscode](./active-directory/active-directory-protocols-oauth-code.md#request-an-authorization-code) Thema beschrieben.

Das folgende Beispiel zeigt, wie OAuth2.0 Genehmigung:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure Active Directory authentifiziert den Benutzer und gegebenenfalls fordert den Benutzer auf die Berechtigung für die Anwendung. Den Autorisierungscode zurück zum Antwort-URL der Anwendung. Abhängig von der angeforderten Response_mode Azure AD entweder sendet Daten in Abfragezeichenfolge oder Post-Daten.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Auth-Anforderung (Open ID verbinden)

Wenn Sie nicht nur für den Benutzer Azure Ressourcenmanager zugreifen möchten, aber auch Benutzer der Anwendung die Azure AD-Konto anmelden, Ausstellen einer geöffneten ID verbinden autorisieren anfordern. Mit Open ID Connect empfängt die Anwendung auch ein ID-Token von Azure AD, mit denen Ihre Anwendung den Benutzer anmelden.

Die Abfragezeichenfolgen-Parameter, die für diese Anforderung werden in [der Anforderung senden](./active-directory/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) Thema beschrieben.

Eine Beispiel-ID öffnen verbinden Anforderung ist:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure Active Directory authentifiziert den Benutzer und gegebenenfalls fordert den Benutzer auf die Berechtigung für die Anwendung. Den Autorisierungscode zurück zum Antwort-URL der Anwendung. Abhängig von der angeforderten Response_mode Azure AD entweder sendet Daten in Abfragezeichenfolge oder Post-Daten.

Eine Antwort öffnen-ID-Verbindung ist:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Token-Anforderung (OAuth2.0 Code gewähren Flow)

Damit Ihre Anwendung den Autorisierungscode von Azure AD erhalten hat, ist es Zeit, den token für Azure-Ressourcen-Manager zugreifen.  Ein OAuth2.0 Code gewähren Token anfordern Azure AD Token Endpunkt zu buchen: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Die Abfragezeichenfolgen-Parameter, die für diese Anforderung werden im Thema [verwenden den Autorisierungscode](./active-directory/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) beschrieben.

Das folgende Beispiel zeigt eine Anforderung für Code gewähren Token mit Kennwort:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Erstellen Sie beim Arbeiten mit Zertifikatanmeldeinformationen JSON Web Token (JWT) und Zeichen (RSA-SHA256) mit dem privaten Schlüssel des Zertifikats Anmeldeinformationen der Anwendung. [JWT token Ansprüche](./active-directory/active-directory-protocols-oauth-code.md#jwt-token-claims)erscheinen Anspruchstypen für das Token. Siehe [Active Directory Auth Klassenbibliothek (.NET) Code](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) Client Assertion JWT-Token signiert.

Clientauthentifizierung [Öffnen ID verbinden Spec](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) Details entnehmen. 

Das folgende Beispiel zeigt eine Anforderung für Code gewähren Tokens mit Zertifikat Anmeldeinformationen:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
    
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012
    
    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Eine Antwort wird für Code gewähren Token: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Token Grant Antwortcode behandeln

Eine erfolgreiche Antwort token (User + app) enthält das Zugriffstoken für Azure-Ressourcen-Manager. Die Anwendung verwendet dieses Zugriffstoken auf Ressourcen-Manager für den Benutzer. Die Lebensdauer von Zugriffstoken ausgestellt von Azure AD beträgt eine Stunde. Ist die Webanwendung muss erneuern (User + app) Zugriffstoken. Verwenden des Zugriffstokens erneuern muss, Aktualisierungstoken, das die Anwendung auf token empfängt. Ein OAuth2.0 Token anfordern Azure AD Token Endpunkt zu buchen: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Die Parameter der Anforderung aktualisieren werden aktualisiert [das Zugriffstoken](./active-directory/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens)beschrieben.

Im folgenden Beispiel wird veranschaulicht, wie mit der Aktualisierung token:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Obwohl Aktualisierungstoken zu neuen Zugriffstoken für Azure-Ressourcen-Manager verwendet werden können, sind sie nicht geeignet für offline-Zugriff durch die Anwendung. Aktualisieren Token Lebensdauer ist begrenzt und Aktualisierungstoken an den Benutzer gebunden. Wenn der Benutzer das Unternehmen verlässt, verliert die Anwendung Aktualisierungstoken Zugriff. Dieser Ansatz ist nicht geeignet für Applikationen Teams zum Verwalten ihrer Azure-Ressourcen verwendet werden.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Überprüfen Sie, ob der Benutzer Zugriff auf Abonnement zuweisen können

Die Anwendung hat einen Token auf Azure-Ressourcen-Manager für den Benutzer. Der nächste Schritt ist das Abonnement Ihrer app herstellen. Nach dem anschließen, kann Ihre app diese Abonnements verwalten, selbst wenn der Benutzer nicht vorhanden ist (langfristige Offlinezugriff). 

Rufen Sie für jedes Abonnement Verbindung [Ressourcenmanager Berechtigungen](https://msdn.microsoft.com/library/azure/dn906889.aspx) zu bestimmen, ob der Benutzer Zugriffsrechte für das Abonnement Management API.

Die [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) -Methode der ASP.NET MVC-Beispiel-app implementiert diesen Aufruf.

Im folgenden Beispiel wird veranschaulicht, wie ein Benutzer Berechtigungen für ein Abonnement. 83cfe939-2402-4581-b761-4f59b0a041e4 ist die Id des Abonnements.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Ein Beispiel für die Antwort zu Benutzerberechtigungen Abonnements ist:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

Die Berechtigungen API gibt mehrere Berechtigungen. Jede Berechtigung besteht aus zugelassene Aktionen () und unzulässige Aktionen (Notactions). Wenn eine Aktion in der Liste zulässiger Aktionen Erlaubnis und die Berechtigung in der Notactions nicht vorhanden ist, wird der Benutzer zum Durchführen der Aktion. **Microsoft.Authorization/RoleAssignments/Write** ist die Aktion, die Access Management gewährt. Die Anwendung muss Berechtigungen Ergebnis suchen Regex Übereinstimmung für diese Aktionszeichenfolge Aktionen und Notactions der einzelnen Berechtigungen analysieren.

## <a name="get-app-only-access-token"></a>Nur app token abrufen

Jetzt wissen Sie Wenn Benutzer Azure-Abonnement Zugriff zuweisen kann. Die nächsten Schritte sind:

1. Die Anwendungsidentität für das Abonnement die entsprechende RBAC-Rolle zuweisen.
2. Überprüfen Sie Access-Zuordnung durch Abfragen der Anwendung die Berechtigung für das Abonnement oder Ressourcen-Manager nur-app-Token verwenden.
1. Aufzeichnen der Verbindungs in der Datenstruktur Applications "verbundenen Abonnements" - Id des Abonnements beibehalten.

Sehen wir uns zunächst genauer. Um die Identität der Anwendung entsprechende RBAC-Rolle zuzuweisen, müssen Sie Folgendes ermitteln:

- Die Objekt-Id der Anwendung Identität des Benutzers Azure Active Directory
- Die Kennung der RBAC-Rolle, die die Anwendung für das Abonnement erforderlich ist

Wenn Ihre Anwendung von Azure AD Benutzerauthentifizierung wird ein Prinzipalobjekt Dienst für Ihre Anwendung, Azure AD erstellt. Azure ermöglicht RBAC-Rollen Service Hauptbenutzer zugewiesen werden direkt auf entsprechende Anträge auf Azure-Ressourcen gewähren. Diese Aktion ist genau was wir wollen. Abfrage-API Azure AD Graph Dienstprinzipalnamen der Anwendung der angemeldeten Benutzer-ID zu ermitteln ist Azure AD.

Sie nur ein Zugriffstoken für Azure-Ressourcen-Manager - Sie benötigen ein Zugriffstoken Azure AD Graph-API aufrufen. Jede Anwendung in Azure AD ist berechtigt, eigene Service principal-Objekt abgefragt werden daher nur app Zugriffstoken ausreichend ist.

<a id="app-azure-ad-graph">
### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>App nur für Azure AD Graph-API token abrufen

Um Ihre Anwendung zu authentifizieren und einen Token Azure AD Graph-API erhalten, token Azure AD-Endpunkt (**https://login.microsoftonline.com/ {Directory_domain_name} OAuth2/Token**) erteilen Sie Client Anmeldeinformationen Grant OAuth2.0 Fluss token Anforderung.

Die [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) -Methode Beispiel ASP.net MVC-Anwendung ruft eine Anwendung nur Zugriff token für Graph-API mit Active Directory-Authentifizierungsbibliothek für .NET.

Die Abfragezeichenfolgen-Parameter, die für diese Anforderung werden in der [Anforderung ein Zugriffstoken](./active-directory/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) Thema beschrieben.

Ein Beispiel für die Clientanmeldeinformationen Antrag Token: 

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Eine Beispielantwort Clientanmeldeinformationen gewähren Token: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Erhalten Sie ObjectId Dienstprinzipalnamen Anwendung in Azure AD Benutzer

Nun verwenden Sie das app nur Zugriffstoken [Prinzipale Azure AD Graph Service](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API bestimmen die Objekt-Id der Anwendung Dienstprinzipal im Verzeichnis Abfragen.

Die [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) Probe ASP.net MVC-Anwendung implementiert diesen Aufruf.

Das folgende Beispiel zeigt wie eine Anwendung Dienstprinzipalnamen. a0448380-c346-4f9f-b897-c18733de9394 ist die Client-Id der Anwendung.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Das folgende Beispiel zeigt eine Antwort auf die Anforderung einer Anwendung principal 

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC-Rolle ID abgerufen

Um Ihre Dienstprinzipal entsprechende RBAC-Rolle zuzuweisen, müssen Sie den Bezeichner der Azure RBAC-Rolle bestimmen.

Die richtige RBAC-Rolle für die Anwendung:

- Wenn die Anwendung nur das Abonnement überwacht ohne Veränderung ist nur Leseberechtigungen für das Abonnement. **Rolle** zuweisen.
- Gelingt die Anwendung Azure Abonnement erstellen/ändern/löschen von Entitäten muss eines der Teilnehmerberechtigungen.
  - Um einen bestimmten Typ von Ressource verwalten weisen Sie spezifische Contributor Rollen (Virtual Machine Contributor virtuellen Netzwerk Mitwirkenden, Storage Konto Teilnehmer usw.)
  - Alle Ressourcentypen verwalten, weisen Sie **der Beitragendenrolle** .

Die Zuweisung der Sicherheitsrolle für die Anwendung ist für Benutzer sichtbar, auf geringsten erforderlichen Berechtigungen.

Rufen Sie die [Ressourcenmanager Rollendefinition API](https://msdn.microsoft.com/library/azure/dn906879.aspx) alle Azure RBAC-Rollen und suchen dann das Ergebnis zum Suchen der gewünschten Rollendefinition nach Namen durchlaufen.

Die [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) -Methode der ASP.net MVC-Beispiel-app implementiert diesen Aufruf.

Die folgende Anforderung veranschaulicht, wie Azure RBAC-Rollenbezeichner abzurufen. 09cbd307-aa71-4aca-b346-5f253e6e3ebb ist die Id des Abonnements.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Die Antwort lautet folgendermaßen: 

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Sie müssen nicht fortlaufend API aufrufen. Nachdem Sie die bekannte GUID der Rollendefinition bestimmt haben, können Sie die Definition Rollen-Id als erstellen:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Hier sind bekannte Guids häufig verwendete integrierte Rollen:

| Rolle | GUID |
| ----- | ------ |
| Reader | acdd72a7-3385-48ef-bd42-f606fba81ae7
| Teilnehmer | b24988ac-6180-42a0-ab88-20f7382dd24c
| Virtual Machine-Teilnehmer | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| Virtuelles Netzwerk Teilnehmer | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Storage-Konto Teilnehmer | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Website-Teilnehmer | de139f84-1756-47ae-9be6-808fbbe84772
| Web Plan Contributor | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| SQL Server-Teilnehmer | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| SQL DB Contributor | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec

### <a name="assign-rbac-role-to-application"></a>Anwendung RBAC-Rolle zuweisen

Sie können Ihre Service principal mit dem [Ressourcen-Manager erstellen Zuweisung der Sicherheitsrolle](https://msdn.microsoft.com/library/azure/dn906887.aspx) API entsprechende RBAC-Rolle zuweisen.

Die [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) -Methode der ASP.net MVC-Beispiel-app implementiert diesen Aufruf.

Ein Beispiel für eine Anforderung Anwendung RBAC-Rolle zuweisen: 

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

In dem Antrag sind die folgenden Werte verwendet:

| GUID | Beschreibung |
| ------ | --------- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb | Die Kennung des Dauerauftrags
| c3097b31-7309-4c59-b4e3-770f8406bad2 | die Objekt-Id des Prinzipals Service der Anwendung
| acdd72a7-3385-48ef-bd42-f606fba81ae7 | die Id der Rolle
| 4f87261d-2816-465d-8311-70a27558df4c | eine neue Guid für die neue Rolle Zuordnung erstellt

Die Antwort lautet folgendermaßen: 

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Greifen Sie nur app token für Azure-Manager

Überprüfen der app hat den gewünschten Zugriff auf das Abonnement, führen Sie eine Testaufgabe für das Abonnement mit einem nur-app-token

Um eine Anwendung nur token zuzugreifen, Anweisungen Sie Abschnitt [zugreifen app nur für Azure AD Graph-API](#app-azure-ad-graph), mit einem anderen Wert für die Resource-Parameter: 

    https://management.core.windows.net/

[ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) Methode der Anwendung ASP.NET MVC-Beispiel wird eine Anwendung nur Zugriff token für Azure-Ressourcen-Manager mit Active Directory Authentifizierungsbibliothek für .net.

#### <a name="get-applications-permissions-on-subscription"></a>Berechtigungen der Anwendung Abonnements erhalten

Um sicherzustellen, dass die Anwendung den gewünschten Zugriff auf Azure-Abonnement verfügt, können Sie auch [Ressourcenmanager Berechtigungen](https://msdn.microsoft.com/library/azure/dn906889.aspx) API aufrufen. Dieser Ansatz ist ähnlich wie bestimmt werden kann, ob der Benutzer Rechte für das Abonnement Verwaltung. Diesmal allerdings die Berechtigungen API mit der nur-app-Token, das Sie im vorherigen Schritt erhalten.

Die [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) -Methode der ASP.NET MVC-Beispiel-app implementiert diesen Aufruf.

## <a name="manage-connected-subscriptions"></a>Verbundene Abonnements verwalten

Wenn Dienstprinzipal für das Abonnement der Anwendung entsprechende RBAC-Rolle zugewiesen wird, kann die Anwendung Überwachung/mit nur-app-Zugriffstoken für Azure-Ressourcen-Manager verwalten beibehalten.

Ihrem Besitzer der Anwendung Rolle Aufgabe Verwaltungsportal oder Befehlszeilenprogramme entfernt, ist die Anwendung nicht mehr auf dieses Abonnement. In diesem Fall sollte Sie benachrichtigt den Benutzer, den die Verbindung mit dem Abonnement von außerhalb der Anwendung getrennt wurde und Ihnen die Möglichkeit, die Verbindung "Reparieren". "Reparieren" würde einfach die Funktion Zuordnung erstellen, die offline gelöscht wurde.

Wie Benutzer Verbindung Abonnements für die Anwendung aktiviert ist, muss den Benutzer Abonnements zu trennen zulassen Trennen Sie Access Management der Sicht bedeutet, dass die Zuweisung der Sicherheitsrolle, die die Anwendung Dienstprinzipalnamen für das Abonnement entfernen. Optional kann beliebig in der Anwendung für das Abonnement auch entfernt. Nur Benutzer mit Zugriffsberechtigung auf das Abonnement Management können das Abonnement trennen.

Die [RevokeRoleFromServicePrincipalOnSubscription-Methode](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) der ASP.net MVC-Beispiel-app implementiert diesen Aufruf.

Das ist alles - Benutzer können nun problemlos verbinden und Verwalten ihrer Azure-Abonnements mit der Anwendung.

