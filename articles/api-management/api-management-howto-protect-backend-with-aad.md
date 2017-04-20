<properties
    pageTitle="So schützen Sie eine Web API-Backend mit Active Directory Azure-API"
    description="Informationen Sie zu Web API-Backend mit Active Directory Azure-API." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>So schützen Sie eine Web API-Backend mit Active Directory Azure-API

Das folgende Video veranschaulicht und Web API-Back-End-build mit OAuth 2.0-Protokoll mit Active Directory Azure-API.  Dieser Artikel enthält eine Übersicht und Weitere Informationen zu den Schritten im Video. Dieser 24 Minuten Video wird gezeigt, wie an:

-   Erstellen Sie einer Web-API-Back-End- und mit AAD - ab 1:30 Uhr
-   Importieren Sie die API in API - ab 7:10
-   Konfigurieren Sie zum Aufrufen von API - ab 9:09-Entwicklerportal
-   Konfigurieren Sie eine desktop-Anwendung aufrufen, die API - ab 18:08
-   Konfigurieren einer Richtlinie Validierung JWT Anfragen - 20:47 ab vorab genehmigen

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Ein Azure AD-Verzeichnis erstellen

Sichern Sie Ihre Web-API unterstützt Azure Active Directory muss mit einem AAD-Mandanten. In diesem Video wird ein Mieter namens **APIMDemo** verwendet. Erstellen Sie einen Mandanten AAD [Klassischen Azure-Portal](https://manage.windowsazure.com) anmelden und auf **neu**->**App Services**->**Active Directory**->**Verzeichnis**->**Benutzerdefinierte**. 

![Azure Active Directory][api-management-create-aad-menu]

In diesem Beispiel wird ein Verzeichnis namens **APIMDemo** mit der Standarddomäne namens **DemoAPIM.onmicrosoft.com**erstellt. Dieses Verzeichnis wird in dem Video verwendet.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Erstellen Sie einen Web-API Service von Azure Active Directory gesichert

In diesem Schritt wird eine Web API-Backend mit Visual Studio 2013 erstellt. Dieser Schritt des Videos beginnt um 1:30. Web-API erstellen Backend-Projekt in Visual Studio, klicken Sie auf **Datei**->**neu**->**Projekt**, und wählen Sie **ASP.NET Web-Anwendung** aus **der Liste der Vorlagen** . In diesem Video wird das Projekt **APIMAADDemo**benannt. Klicken Sie auf **OK** , um das Projekt zu erstellen. 

![Visual Studio][api-management-new-web-app]

Klicken Sie auf **Web-API** auf, **Wählen Sie eine Vorlage aus** Web API-Projekt erstellen. Zur Azure Directory Authentifizierung klicken Sie auf **Authentifizierung ändern**.

![Neues Projekt][api-management-new-project]

Geben Sie die **Domäne** des AAD-Mandanten auf **Organisationseinheiten Konten** In diesem Beispiel ist die Domäne **DemoAPIM.onmicrosoft.com**. Bereich des Verzeichnisses erhalten Sie auf der Registerkarte **Domänen** des Verzeichnisses.

![Domänen][api-management-aad-domains]

Konfigurieren Sie die Einstellungen im Dialogfeld **Authentifizierung ändern** , und klicken Sie auf **OK**.

![Authentifizierung ändern][api-management-change-authentication]

**Auf** Visual Studio versucht, Ihre Anwendung mit Ihrem Azure AD-Verzeichnis registrieren und Sie möglicherweise Visual Studio anmelden. Melden Sie sich mit einem Administratorkonto für das Verzeichnis.

![Melden Sie sich bei Visual Studio][api-management-sign-in-vidual-studio]

Konfigurieren Sie dieses Projekt als eine Azure Web API aktivieren Sie das Kontrollkästchen für **Host in der Cloud** , und klicken Sie auf **OK**.

![Neues Projekt][api-management-new-project-cloud]

Sie möglicherweise Azure anmelden, und dann können Web App.

![Konfigurieren][api-management-configure-web-app]

In diesem Beispiel wird ein neuer **App Service-Plan** mit dem Namen **APIMAADDemo** angegeben.

Klicken Sie auf **OK** , um das Web App konfigurieren und erstellen Sie das Projekt.

## <a name="add-the-code-to-the-web-api-project"></a>Fügen Sie den Code zum Projekt Web-API

Der nächste Schritt im Video hinzugefügt Project Web API Code. Dieser Schritt startet bei 4:35.

Die Web-API in diesem Beispiel implementiert einen rechnerdienst ein Modell mit einem Controller. Fügen Sie das Modell für den Dienst Maustaste **Modelle** im **Projektmappen-Explorer** , und wählen Sie **Hinzufügen**, **Klasse**. Die Klasse `CalcInput` und klicken Sie auf **Hinzufügen**.

Fügen Sie den folgenden `using` -Anweisung an den Anfang der `CalcInput.cs` Datei.

    using Newtonsoft.Json;

 Ersetzen Sie die generierte Klasse mit dem folgenden Code.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

Maustaste **Controller** im **Projektmappen-Explorer** und wählen Sie **Add**->**Controller**. Wählen Sie **Web API 2 Controller - leer** , und klicken Sie auf **Hinzufügen**. Geben Sie **CalcController** für den Namen des Controllers und klicken Sie auf **Hinzufügen**.

![Controller hinzufügen][api-management-add-controller]

Fügen Sie den folgenden `using` -Anweisung an den Anfang der `CalcController.cs` Datei.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

Ersetzen Sie die generierten Controller-Klasse durch folgenden Code. Dieser Code implementiert die `Add`, `Subtract`, `Multiply`, und `Divide` Operationen der einfachen Rechner-API.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Drücken Sie **F6** zum Erstellen und Überprüfen der Lösung.

## <a name="publish-the-project-to-azure"></a>Veröffentlichen Sie das Projekt in Azure

Projekt wird in diesem Schritt die Visual Studio in Azure veröffentlicht. Dieser Schritt des Videos beginnt bei 5:45.

Zum Veröffentlichen des Projekts in Azure Maustaste auf das **APIMAADDemo** -Projekt in Visual Studio und **Veröffentlichen**. Behalten Sie die Standardeinstellungen im Dialogfeld **Web veröffentlichen** , und klicken Sie auf **Veröffentlichen**.

![Web veröffentlichen][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Gewähren von Berechtigungen für Azure AD Back-End-Dienst-Anwendung

Eine neue Anwendung für Back-End-Dienst ist im Verzeichnis Azure AD als Teil der Konfiguration und Veröffentlichen des Projekts Web-API erstellt. In diesem Schritt des Videos ab 6:13 sind Back-End-Web-API-Berechtigungen gewährt.

![Anwendung][api-management-aad-backend-app]

Klicken Sie auf den Namen der Anwendung erforderlichen Berechtigungen konfigurieren. Navigieren Sie zu der Registerkarte **Konfigurieren** und scrollen Sie Abschnitt **Berechtigungen für andere Programme** . Klicken Sie auf die Dropdownliste neben **Windows Azure** **Active Directory** **Berechtigungen** für **Verzeichnisdaten lesen**das Kontrollkästchen Sie und klicken Sie auf **Speichern**.

![Hinzufügen von Berechtigungen][api-management-aad-add-permissions]

>[AZURE.NOTE] Wenn **Windows Azure** **Active Directory** unter Berechtigungen für andere Programme nicht aufgeführt ist, klicken Sie auf **Anwendung hinzufügen** und aus der Liste hinzufügen.

Notieren der **App-ID-URI** für die Verwendung in einem späteren Schritt für Entwickler-API Verwaltungsportal Azure AD-Anwendung konfiguriert ist.

![App-Id URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Die Web-API in API Management importieren

APIs werden vom Portal Publisher API konfiguriert der Azure-Verwaltungsportal zugegriffen wird. Klicken Sie auf **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst zu Publisher-Portal. Wenn eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [verwalten Ihre erste API][] [Erstellen API Management Service][] .

![Publisher-portal][api-management-management-console]

Operationen kann [APIs manuell hinzugefügt](api-management-howto-add-operations.md)oder importiert werden. In diesem Video werden Operationen ab 6:40 stolz-Format importiert.

Erstellen Sie eine Datei namens `calcapi.json` mit folgendem Inhalt und auf Ihrem Computer speichern. Sicherstellen, dass die `host` -Attribut verweist auf Web API-Backend. In diesem Beispiel `"host": "apimaaddemo.azurewebsites.net"` wird verwendet.

{"swagger": "2.0", "Info": {"Title": "Calculator", "Beschreibung": "Arithmetik über HTTP!", "Version": "1.0"}, "Host": "apimaaddemo.azurewebsites.net", "BasePath": "/ api", "Programme": ["http"] "Pfade": {"hinzufügen? eine = {a} & b = {b}": {"get": {"Beschreibung": "Antwortet mit einer Summe von zwei Zahlen" "OperationId": "Add Two Ganzzahlen", "Parameter": [{"Name": "a", "in": "query", "Beschreibung": "ersten Operanden. Standardwert ist <code>51</code>. ","erforderlich": true"Standard":"51","Enum":""[51]}, {"Name":"b","in":"query","Beschreibung":"zweiten Operanden. Standardwert ist <code>49</code>. ","erforderlich": true"Standard":"49","Enum":""[49]}],"Antworten": {}}}," / Sub?a = {a} & b = {b} ": {"get": {"Beschreibung":"Antwortet mit einer Differenz zwischen zwei Zahlen","OperationId":"Subtrahieren zwei ganze Zahlen","Parameter": [{"Name":"a","in":"query","Beschreibung":"ersten Operanden. Standardwert ist <code>100</code>. ","erforderlich": true"Standard":"100","Enum":""[100]}, {"Name":"b","in":"query","Beschreibung":"zweiten Operanden. Standardwert ist <code>50</code>. ","erforderlich": true"Standard":"50","Enum": ["50"]}],"Antworten": {}}}," / Div?a = {a} & b = {b} ": {"get": {"Beschreibung":"Antwortet mit den Quotienten von zwei Zahlen","OperationId":"Zwei Ganzzahlen Division","Parameter": [{"Name":"a","in":"query","Beschreibung":"ersten Operanden. Standardwert ist <code>100</code>. ","erforderlich": true"Standard":"100","Enum":""[100]}, {"Name":"b","in":"query","Beschreibung":"zweiten Operanden. Standardwert ist <code>20</code>. ","erforderlich": true"Standard":"20","Enum":""[20]}],"Antworten": {}}}," / Mul?a = {a} & b = {b} ": {"get": {"Beschreibung":"Antwortet mit einem Produkt von zwei Zahlen","OperationId":"Multipliziert zwei ganze Zahlen","Parameter": [{"Name":"a","in":"query","Beschreibung":"ersten Operanden. Standardwert ist <code>20</code>. ","erforderlich": true"Standard":"20","Enum":""[20]}, {"Name":"b","in":"query","Beschreibung":"zweiten Operanden. Standardwert ist <code>5</code>. ","erforderlich": true"Standard":"5","Enum":""[5]}],"Antworten": {}}}}}

Zum Importieren des Rechners API klicken Sie **APIs** der **API** Menü auf der linken Seite und dann auf **API importieren**.

![API importieren][api-management-import-api]

Führen Sie so konfigurieren Sie den API-Rechner aus.

1. Klicken Sie auf **aus Datei**, suchen Sie die `calculator.json` gespeicherte Datei, und klicken Sie auf die Schaltfläche **Swagger** .
2. Geben Sie **Calc** im Textfeld **Web API URL-Suffix** .
3. Klicken Sie im Feld **Produkte (optional)** , und wählen Sie **Starter**.
4. Klicken Sie auf **Speichern** , um die API importieren.

![Neues API hinzufügen][api-management-import-new-api]

Nach dem Importieren der API wird die Zusammenfassungsseite für die API im Publisher-Portal angezeigt.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Aufruf der API erfolglos Entwicklerportal

Zu diesem Zeitpunkt die API in API Management importiert wurden, jedoch kann nicht noch aufgerufen werden erfolgreich im Entwicklerportal Back-End-Dienst mit Azure AD-Authentifizierung geschützt ist. Dies zeigt im Video 7:40 mit den folgenden Schritten ab.

Klicken Sie oben rechts im Publisher-Portal **-Entwicklerportal** .

![Entwicklerportal][api-management-developer-portal-menu]

**APIs** klicken Sie und der **Rechner** API.

![Entwicklerportal][api-management-dev-portal-apis]

Klicken Sie auf **ausprobieren**.

![Versuch es][api-management-dev-portal-try-it]

Klicken Sie auf **Senden** , und beachten Sie den Antwortstatus **401 Unauthorized**.

![Senden][api-management-dev-portal-send-401]

Die Anforderung ist nicht autorisiert, da Back-End-API von Azure Active Directory geschützt ist. Portal muss vor dem Aufrufen der API erfolgreich Entwickler Entwickler OAuth 2.0 autorisieren konfiguriert werden. Dieser Prozess wird in den folgenden Abschnitten beschrieben.

## <a name="register-the-developer-portal-as-an-aad-application"></a>Entwicklerportal als AAD Anwendung registrieren

Zunächst konfigurieren Entwicklerportal Entwickler OAuth 2.0 autorisieren ist Entwicklerportal als AAD Anwendung registriert. Dies zeigt 8:27 des Videos ab.

Navigieren Sie aus dem ersten Schritt dieses Video in diesem Beispiel **APIMDemo** Azure AD-Mandanten und navigieren Sie zur Registerkarte **Applications** .

![Neue Anwendung][api-management-aad-new-application-devportal]

Klicken Sie auf **Hinzufügen** , um eine neue Active Directory Azure-Anwendung erstellen und auswählen **von meinem Unternehmen entwickelte Anwendung hinzufügen**.

![Neue Anwendung][api-management-new-aad-application-menu]

Wählen Sie **Web-Anwendung oder Web-API**, geben Sie einen Namen und klicken Sie auf den Pfeil. In diesem Beispiel wird **APIMDeveloperPortal** verwendet.

![Neue Anwendung][api-management-aad-new-application-devportal-1]

**Einmaliges** URL Geben Sie die URL der API Management-Dienst und fügen `/signin`. In diesem Beispiel wird **https://contoso5.portal.azure-api.net/signin **verwendet.

Geben Sie die URL der API-Verwaltungsdienst **App Id URL** und Zeichen Sie einige eindeutige. Können alle gewünschten Zeichen und in diesem Beispiel wird **https://contoso5.portal.azure-api.net/dp** verwendet. Wenn die gewünschte **Anwendung Eigenschaften** konfiguriert werden, klicken Sie auf das Häkchen, um die Anwendung zu erstellen.

![Neue Anwendung][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>Konfigurieren eines Servers API Management OAuth 2.0 Autorisierung

Im nächste Schritt wird einen OAuth 2.0 autorisierungsserver Management API konfiguriert. Dieser Schritt im Video ab 9:43 veranschaulicht.

Klicken Sie auf **Sicherheit** aus der API-Menü auf der linken Seite und klicken Sie auf **Add Autorisierung** **OAuth 2.0**auf.

![Autorisierungsserver hinzufügen][api-management-add-authorization-server]

Geben Sie in die Felder **Name** und **Beschreibung** einen Namen und eine Beschreibung. Diese Felder dienen zur Identifizierung des Servers OAuth 2.0 Autorisierung innerhalb der Dienstinstanz API Management. In diesem Beispiel wird die **Autorisierung Server Demo** verwendet. Später bei Angabe einen OAuth 2.0-Server für die Authentifizierung für eine API wählen Sie diesen Namen.

**Client-Registrierung Seite URL** eingeben Platzhalter wie `http://localhost`.  **URL der Client Registrierung** verweist auf der Seite, mit denen Benutzer erstellen und konfigurieren ihre eigenen Konten für OAuth 2.0-Provider, die Verwaltung von Konten unterstützen. In diesem Beispiel Benutzer nicht erstellen und konfigurieren ihre eigenen Konten Platzhalter verwendet.

![Autorisierungsserver hinzufügen][api-management-add-authorization-server-1]

Geben Sie nun **Autorisierung Endpunkt-URL** und **Endpunkt-URL-Token**.

![Autorisierungsserver][api-management-add-authorization-server-1a]

Diese Werte können auf **App Endpunkte** AAD Anwendung erstellten Entwicklerportal abgerufen werden. Auf die Endpunkte auf die Registerkarte **Konfigurieren** der AAD Anwendung navigieren und auf **Ansicht Endpunkte**.

![Anwendung][api-management-aad-devportal-application]

![Endpunkte anzeigen][api-management-aad-view-endpoints]

Kopieren Sie **OAuth 2.0 autorisierungsendpunkt** und **Autorisierung Endpunkt-URL** -Textfeld einfügen.

![Autorisierungsserver hinzufügen][api-management-add-authorization-server-2]

Kopieren Sie **OAuth 2.0 token Endpunkt** und **Token Endpunkt-URL** -Textfeld einfügen.

![Autorisierungsserver hinzufügen][api-management-add-authorization-server-2a]

Neben Einfügen in token Endpunkt fügen Sie zusätzlichen Textparameter benannte **Ressource** und Zwecken Wert **App Id URI** AAD Anwendung für Back-End-Dienst, der erstellt wurde Visual Studio-Projekt veröffentlicht wurde hinzu.

![App-Id URI][api-management-aad-sso-uri]

Geben Sie dann die Anmeldeinformationen des Clients. Dies sind die Anmeldeinformationen für die Ressource zugreifen, in diesem Fall des Back-End-Dienstes.

![Client-Anmeldeinformationen][api-management-client-credentials]

Die **Client-Id**, navigieren Sie zur Registerkarte **Konfigurieren** AAD Anwendung für Back-End-Dienst und die **Client-Id**kopieren.

Der **Geheimen** auf **Dauer auswählen** Dropdown-Abschnitt **Schlüssel** und ein Intervall angeben. In diesem Beispiel wird 1 Jahr verwendet.

![Client-ID][api-management-aad-client-id]

Klicken Sie auf **Speichern** , um die Konfiguration zu speichern und Anzeigen des Schlüssels. 

>[AZURE.IMPORTANT] Notieren Sie sich diesen Schlüssel. Sobald Sie Azure Active Directory Konfigurationsfenster schließen, kann der Schlüssel wieder angezeigt werden.

Kopieren Sie den Schlüssel in die Zwischenablage, wechseln Sie zu Publisher-Portal **Geheimen** Textbox fügen Sie den Schlüssel ein und klicken Sie auf **Speichern**.

![Autorisierungsserver hinzufügen][api-management-add-authorization-server-3]

Unmittelbar nach der Clientanmeldeinformationen ist eine Autorisierung Code gewähren. Kopieren Sie diesen Autorisierungscode und wechseln zur Azure AD Developer Portal Anwendung Seite sowie einfügen Bewilligung erteilen Sie im Feld **Antwort-URL** , und klicken Sie erneut auf **Speichern** .

![Antwort-URL][api-management-aad-reply-url]

Im nächste Schritt werden Berechtigungen für Entwicklerportal AAD-Anwendung konfigurieren. Klicken Sie auf **Berechtigungen** und das Kontrollkästchen Sie **Verzeichnisdaten lesen**. Klicken Sie auf **Speichern** , um diese Änderung zu speichern, und klicken Sie auf **Anwendung hinzufügen**.

![Hinzufügen von Berechtigungen][api-management-add-devportal-permissions]

Klicken Sie auf das Suchsymbol beginnend mit Feld **APIM** eingeben Wählen Sie **APIMAADDemo**und klicken Sie auf Speichern.

![Hinzufügen von Berechtigungen][api-management-aad-add-app-permissions]

**Delegierte Berechtigungen** für **APIMAADDemo** auf das Kontrollkästchen Sie **Access APIMAADDemo**und klicken Sie auf **Speichern**. Dadurch kann der Entwickler Portal Anwendung Zugriff auf die Back-End-Dienst.

![Hinzufügen von Berechtigungen][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Aktivieren Sie OAuth 2.0 Benutzerberechtigung für Rechner-API

OAuth 2.0-Server konfiguriert ist, können Sie es in den Einstellungen für Ihre API angeben. Dadurch zeigt das Video ab 14:30.

Klicken Sie im linken Menü auf **APIs** , und klicken Sie auf **Rechner** anzeigen und Konfigurieren der.

![Rechner-API][api-management-calc-api]

Navigieren Sie zur Registerkarte **Sicherheit** Kontrollkästchen Sie **OAuth 2.0** , wählen Sie den gewünschten autorisierungsserver Dropdown **autorisierungsserver** und klicken Sie auf **Speichern**.

![Rechner-API][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Rufen Sie die Calculator-API erfolgreich Entwicklerportal

Nun, OAuth 2.0 Autorisierung zur API konfiguriert ist, können Vorgänge erfolgreich aus dem Developer Center aufgerufen. Dieser Schritt zeigt im Video um 15:00 Uhr ab.

Navigieren Sie zu dem **Hinzufügen zwei Ganzzahlen** Betrieb der Rechner im Entwicklerportal und **versuchen Sie es**auf. Beachten Sie das neue Element im Abschnitt **Authorization** autorisierungsserver hinzugefügten entspricht.

![Rechner-API][api-management-calc-authorization-server]

Dropdownliste Autorisierung wählen Sie **Autorisierungscode aus** , und geben Sie die Anmeldeinformationen des Kontos verwenden. Wenn Sie bereits mit dem Konto angemeldet sind, können Sie nicht aufgefordert.

![Rechner-API][api-management-devportal-authorization-code]

Klicken Sie auf **Senden** und der **Antwortstatus** der **200 OK** und die Ergebnisse des Vorgangs in der Antwortinhalt.

![Rechner-API][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Konfigurieren Sie eine desktop-Anwendung aufrufen, die API

Im nächste Verfahren im Video beginnt um 16:30 und konfiguriert eine einfache desktop-Anwendung die API aufrufen. Desktop-Anwendung in Azure AD registrieren und Zugriff auf das Verzeichnis und die Back-End-ist der erste Schritt. Am 18:25 ist eine Demonstration der desktop-Anwendung eine Operation auf dem Rechner API aufrufen.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Konfigurieren einer Richtlinie Validierung JWT Anfragen vorab genehmigen

Im letzten Schritt des Videos beginnt bei 20:48 und erläutert die [Überprüfen JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) -Richtlinie verwenden, um Anfragen bereits überprüft die Zugriffstoken jede eingehende Anforderung autorisieren. Die Anforderung nicht durch die Richtlinie JWT überprüfen überprüft, wird die Anforderung durch API Management blockiert und ist nicht auf dem Back-End übergeben.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

Eine andere Konfiguration und Verwendung dieser Richtlinie finden Sie unter [Cloud decken Episode 177: mehr Management-API-Funktionen](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) und Vorspulen 13:50. Schneller Vorlauf bis 15:00 zu Richtlinien im Gruppenrichtlinien-Editor konfiguriert und dann eine Demonstration Aufrufen einer Operation Entwicklerportal mit und ohne erforderliche Autorisierungstoken 18:50.

## <a name="next-steps"></a>Nächste Schritte
-   Überprüfen Sie weitere [Videos](https://azure.microsoft.com/documentation/videos/index/?services=api-management) über API Management.
-   Andere an Ihren Back-End-Dienst finden Sie unter [Zertifikat Authentifizierung](api-management-howto-mutual-certificates.md) und [Verbindung VPN- oder ExpressRoute](api-management-howto-setup-vpn.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance
[Die erste API verwalten]: api-management-get-started.md
