<properties 
    pageTitle="Erstellen von APIs in Azure API Management" 
    description="Informationen Sie zum Erstellen und Konfigurieren von APIs in Azure API Management." 
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

# <a name="how-to-create-apis-in-azure-api-management"></a>Erstellen von APIs in Azure API Management

Eine API in API Management stellt einen Satz von Operationen, die von Clientanwendungen aufgerufen werden können. Neue APIs im Publisher-Portal erstellt und anschließend die gewünschten Operationen hinzugefügt werden. Nachdem Operationen hinzugefügt werden, wird die API zu einem Produkt hinzugefügt und kann veröffentlicht werden. Veröffentlichte API können Sie abonniert und von Entwicklern verwendet werden.

Dieses Handbuch zeigt den ersten Schritt im Prozess: das Erstellen und konfigurieren Sie eine neue API in API Management. Weitere Informationen zum Hinzufügen von Operationen und Veröffentlichen eines Produkts finden Sie unter [Vorgänge eine API hinzufügen][] und [zum Erstellen und Veröffentlichen eines Produkts][].

## <a name="create-new-api"> </a>Eine neue API erstellen

APIs erstellt und im Publisher-Portal konfiguriert. Klicken Sie Publisher-Portal **Verwalten** in Azure-Verwaltungsportal für Ihre API-Verwaltungsdienst.

![Publisher-portal][api-management-management-console]

>Wenn Sie eine Dienstinstanz API Management noch nicht erstellt haben, finden Sie im Lernprogramm [Erste Schritte mit Azure API Management][] [Erstellen API Management Service][] .

Klicken Sie auf **APIs** der **API-** Menü auf der linken Seite und dann auf **API hinzufügen**.

![API erstellen][api-management-create-api]

Verwenden Sie das Fenster **neue API hinzufügen** neue API konfigurieren.

![Neues API hinzufügen][api-management-add-new-api]

Die folgenden Felder werden zum Konfigurieren der neuen API verwendet.

-   **Name der Web-API** bietet einen eindeutigen beschreibenden Namen für die API. Er ist Entwickler und Herausgeber Portale angezeigt.
-   **Webdienst-URL** verweist auf den HTTP-Dienst die API implementieren. API-Management leitet Anfragen an diese Adresse.
-   Die Basis-URL der API-Verwaltungsdienst **Web API URL-Suffix** hinzugefügt. Die Basis-URL ist für alle APIs durch eine API Management Service-Instanz gehostet. API Management unterscheidet die APIs von dem Suffix und daher das Suffix muss für jede API für einen bestimmten Herausgeber.
-   **Web-API-URL-Schema** bestimmt, welche Protokolle verwendet werden können, auf die API zugreifen. HTTPs ist standardmäßig.
-   Um diese neue API optional ein Produkt hinzuzufügen, klicken Sie auf die Dropdown- **Produkte (optional)** , und wählen Sie ein Produkt. Dieser Schritt kann mehrmals wiederholt werden, die API für mehrere Produkte hinzufügen.

Sobald die gewünschten Werte konfiguriert sind, klicken Sie auf **Speichern**. Nach der Erstellung der neuen API wird die Zusammenfassungsseite für die API im Publisher-Portal angezeigt.

![API-Zusammenfassung][api-management-api-summary]

## <a name="configure-api-settings"> </a>Settings API konfigurieren

Verwenden **die Registerkarte** überprüfen und Bearbeiten der Konfiguration einer API. **Web-API namens** **Webdienst-URL**und **Web API URL-Suffix** zunächst die API erstellt und geändert werden kann hier festgelegt. **Beschreibung** enthält eine Beschreibung und **Web API-URL-Schema** bestimmt, welche Protokolle verwendet werden können, auf die API zugreifen.

![API-Einstellung][api-management-api-settings]

Wählen Sie die Registerkarte **Sicherheit** , um die Authentifizierung für den Back-End-Dienst die API implementieren Gateway konfigurieren. Die **mit** Dropdown-kann zum Konfigurieren der Authentifizierung für **HTTP basic** oder **Clientzertifikate** verwendet werden. Um HTTP-Standardauthentifizierung verwenden, geben Sie einfach die gewünschten Anmeldeinformationen. Informationen über Clientzertifikatsauthentifizierung finden Sie unter [Sichern Backenddiensten Clientzertifikatsauthentifizierung in Azure API Management verwenden][].

Die Registerkarte **Sicherheit** kann auch verwendet werden, die **Benutzer** mit OAuth 2.0 konfiguriert. Weitere Informationen finden Sie unter [Autorisieren Developer Konten OAuth 2.0 in Azure API Management][].

![Einfache Authentifizierung][api-management-api-settings-credentials]

Klicken Sie auf **Speichern** , um Änderungen speichern auf den API.

## <a name="next-steps"> </a>Nächste Schritte

API erstellt und die Einstellungen die nächsten Schritte sind die API Vorgänge hinzufügen, fügen Sie die API zu einem Produkt hinzu und veröffentlichen Sie, sodass Entwickler verfügbar ist. Weitere Informationen finden Sie in den folgenden Artikeln.

-   [Wie eine API Operationen hinzugefügt][]
-   [Erstellen und Veröffentlichen eines Produkts][]





[api-management-create-api]: ./media/api-management-howto-create-apis/api-management-create-api.png
[api-management-management-console]: ./media/api-management-howto-create-apis/api-management-management-console.png
[api-management-add-new-api]: ./media/api-management-howto-create-apis/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-create-apis/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-create-apis/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-create-apis/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-create-apis/api-management-echo-operations.png

[What is an API?]: #what-is-api
[Create a new API]: #create-new-api
[Configure API settings]: #configure-api-settings
[Configure API operations]: #configure-api-operations
[Next steps]: #next-steps

[Wie eine API Operationen hinzugefügt]: api-management-howto-add-operations.md
[Erstellen und Veröffentlichen eines Produkts]: api-management-howto-add-products.md

[Erste Schritte mit Azure API Management]: api-management-get-started.md
[Erstellen Sie eine Instanz der API Management service]: api-management-get-started.md#create-service-instance
[Back-End-Services Client mit Zertifikatauthentifizierung in Azure API Management schützen]: api-management-howto-mutual-certificates.md
[Wie autorisieren Developer Konten OAuth 2.0 in Azure API Management]: api-management-howto-oauth2.md