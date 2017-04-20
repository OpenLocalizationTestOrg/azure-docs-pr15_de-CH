
<properties
   pageTitle="Authentifizierungsszenarien Azure Anzeige | Microsoft Azure"
   description="Eine Übersicht über die fünf häufigsten Authentifizierungsszenarien für Azure Active Directory (AAD)"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="authentication-scenarios-for-azure-ad"></a>Authentifizierungsszenarien Azure AD

Azure Active Directory (Azure AD) vereinfacht Authentifizierung für Entwickler durch Identität als Dienst mit Unterstützung für Standardprotokolle wie OAuth 2.0 OpenID verbinden sowie open Source Bibliotheken für verschiedene Plattformen zu schnell Programmieren beginnen. Dieses Dokument hilft Ihnen, die verschiedenen Szenarien Azure AD unterstützt verstehen und erfahren Sie, wie. Es ist in folgende Abschnitte unterteilt:

- [Grundlagen der Authentifizierung in Azure AD](#basics-of-authentication-in-azure-ad)

- [Ansprüche in Sicherheitstokens Azure AD](#claims-in-azure-ad-security-tokens)

- [Grundlagen zur Registrierung einer Anwendung in Azure AD](#basics-of-registering-an-application-in-azure-ad)

- [Anwendungstypen und Szenarien](#application-types-and-scenarios)

  - [Webbrowser Anwendung](#web-browser-to-web-application)

  - [Einzelne Seite Anwendung (SPA)](#single-page-application-spa)

  - [Systemeigene Anwendung Web API](#native-application-to-web-api)

  - [Web Application to Web-API](#web-application-to-web-api)

  - [Daemon oder Serveranwendung Web API](#daemon-or-server-application-to-web-api)



## <a name="basics-of-authentication-in-azure-ad"></a>Grundlagen der Authentifizierung in Azure AD

Wenn Sie mit grundlegenden Konzepte der Authentifizierung in Azure AD nicht vertraut sind, lesen Sie diesen Abschnitt. Andernfalls sollten Sie [Anwendungstypen](#application-types-and-scenarios)und Szenarien zu überspringen.

Sehen Sie sich das einfachste Szenario Identität erforderlich ist: ein Benutzer in einem Webbrowser auf eine Anwendung authentifizieren muss. Dieses Szenario ist im Abschnitt [Webbrowser Anwendung](#web-browser-to-web-application) ausführlich beschrieben, aber es ist ein nützlicher Ausgangspunkt zu veranschaulichen die Funktionen von Azure AD und Funktionsweise des Szenarios. Betrachten Sie das folgende Diagramm für dieses Szenario:

![Übersicht über Web-Anwendung anmelden](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

Das Diagramm oben beachten sieht müssen die verschiedenen Komponenten kennen:

- Azure AD ist der Identitätsanbieter verantwortlich für die Identität der Benutzer und Programme, die im Verzeichnis der Organisation vorhanden und letztendlich ausstellenden Sicherheitstoken nach erfolgreicher Authentifizierung der Benutzer und Programme.


- Eine Anwendung, die Authentifizierung Azure AD auslagern muss in Azure AD registriert werden, registriert die Anwendung im Verzeichnis eindeutig.


- Open-Source-Azure AD-authentifizierungsbibliotheken können Entwickler Authentifizierung vereinfachen, indem Sie die Protokolldetails zur Behandlung. Weitere Informationen finden Sie unter [Azure Active Directory Authentifizierungsbibliotheken](active-directory-authentication-libraries.md) .


•, Sobald ein Benutzer authentifiziert wurde, muss die Anwendung überprüfen Sicherheitstoken des Benutzers, um sicherzustellen, dass die Authentifizierung für den beabsichtigten Parteien erfolgreich war. Die bereitgestellte authentifizierungsbibliotheken können Entwickler Validieren alle Token von Azure AD einschließlich JSON Web Token (JWT) oder SAML 2.0. Wenn Sie manuell validieren möchten, finden Sie im [JWT Tokenhandler](https://msdn.microsoft.com/library/dn205065.aspx) .


> [AZURE.IMPORTANT] Azure AD verwendet die Kryptografie mit öffentlichen Schlüsseln Token signieren und überprüfen, ob sie gültig sind. Finden Sie [Wichtige Informationen zum Signieren von Schlüssel Rollover in Azure AD](active-directory-signing-key-rollover.md) für Weitere Informationen über die notwendige Logik, müssen Sie in Ihrer Anwendung, um sicherzustellen, dass immer die neuesten Schlüssel aktualisiert wird.


• Von Anfragen und Antworten für den Authentifizierungsprozess anhand Authentifizierungsprotokoll, das verwendet wurde, wie OAuth 2.0 OpenID verbinden, WS-Verbund oder SAML 2.0. Diese Protokolle werden in [Azure Active Directory Authentifizierungsprotokolle](active-directory-authentication-protocols.md) Thema und in den folgenden Abschnitten erläutert.

> [AZURE.NOTE] Azure AD unterstützt OAuth 2.0 und OpenID verbinden Standards umfassenden machen Träger Token, einschließlich trägertoken als JWTs dargestellt. Ein trägertoken ist ein einfaches Sicherheitstoken, den "Inhaber" den Zugriff auf eine geschützte Ressource gewährt. In diesem Sinne ist der "" eine Partei, die das Token darstellen kann. Wenn eine Partei erforderlichen Schritte zum Sichern des Tokens in der Übertragung und Speicherung nicht getroffen sind zunächst mit Azure trägertoken zu authentifizieren muss können sie abgefangen und durch einen unerwünschten Dritten verwendet. Zwar einige Sicherheitstoken einen integrierten Mechanismus zum verhindern, dass unbefugte Verwendung trägertoken verfügen über diesen Mechanismus und einen sicheren Kanal wie Transport Layer Security (HTTPS) transportiert werden müssen. Ein trägertoken im Klartext übertragen, kann ein Man-in der mittleren Angriff verwendet werden bösartige Angriffe Token und für nicht autorisierten Zugriff auf eine geschützte Ressource verwenden. Sicherheitsgrundsätze gelten beim Speichern trägertoken zur späteren Verwendung zwischenspeichern. Immer sicherstellen Sie, dass Ihre Anwendung überträgt und trägertoken sicher speichert. Weitere Sicherheitsaspekte zu produzierende Token finden Sie in [RFC 6750 Abschnitt 5](http://tools.ietf.org/html/rfc6750).


Jetzt haben Sie einen Überblick über die Grundlagen, lesen Sie die Abschnitte unten Bereitstellung in Azure AD Funktionsweise und Szenarien Azure AD unterstützt.


## <a name="claims-in-azure-ad-security-tokens"></a>Ansprüche in Sicherheitstokens Azure AD

Von Azure AD ausgestellte Sicherheitstoken enthalten Ansprüche oder Assertionen von Informationen zum Thema authentifiziert wurde. Diese Ansprüche können von der Anwendung für verschiedene Vorgänge verwendet werden. Beispielsweise können sie überprüfen das Token des Antragstellers Verzeichnis Mieter identifizieren, Benutzerinformationen anzeigen, Autorisierung des Antragstellers bestimmen und usw. verwendet werden. Im angegebenen Sicherheitstoken Ansprüche sind die Art des Tokens Anmeldeinformationen zur Authentifizierung der Benutzer und der Konfiguration abhängig. Eine kurze Beschreibung der einzelnen Anspruch von Azure AD ausgegeben wird in der folgenden Tabelle bereitgestellt. Weitere Informationen finden Sie in [unterstützte und Anspruchstypen](active-directory-token-and-claims.md).


| Forderung | Beschreibung |
|-------|-------------|
| ID der Anwendung | Identifiziert die Anwendung, die das Token verwendet.
| Zielgruppe | Identifiziert die Empfänger Ressource Token bestimmt ist. |
| Anwendung Authentifizierung Kontextreferenz-Klasse | Gibt an, wie der Client authentifiziert (öffentliche Client und vertraulichen Client) wurde. |
| Instant Authentifizierung | Zeichnet Datum und Uhrzeit bei die Authentifizierung. |
| Authentifizierungsmethode | Gibt an, wie der Betreff des Tokens authentifiziert wurde (Kennwort, Zertifikat usw.). |
| Vorname | Der Vorname des Benutzers bietet wie Azure AD. |
| Gruppen | Enthält die Ids von Azure AD Gruppen, denen der Benutzer angehört. |
| Identitätsanbieter | Zeichnet den Identitätsanbieter, der den Betreff des Tokens authentifiziert. |
| Ausgestellt am | Die Zeit aufgezeichnet, an der das Token häufig zum token frische ausgestellt wurde. |
| Aussteller | Identifiziert den STS, der das Token sowie Azure AD-Mandanten ausgegeben. |
| Nachname | Enthält den Nachnamen des Benutzers in Azure AD festgelegt. |
| Name | Stellt einen Menschen lesbaren Wert, der Gegenstand der Token identifiziert. |
| Objekt-Id | Enthält einen unveränderlichen, eindeutigen Bezeichner des Betreffs in Azure AD. |
| Rollen | Enthält die Anzeigenamen von Azure AD Anwendungsrollen, die der Benutzer gewährt wurden. |
| Gültigkeitsbereich | Gibt die Berechtigungen an die Clientanwendung. |
| Betreff | Gibt den Prinzipal über dem Token Informationen bestätigt. |
| Mandanten-Id | Enthält einen unveränderlichen, eindeutigen Bezeichner des Mieters Verzeichnis, das das Token ausgestellt. |
| Gültigkeitsdauer des Tokens | Definiert das Zeitintervall, in dem ein Token gültig ist. |
| Benutzerprinzipalnamen | Der Benutzerprinzipalname des Betreffs enthält. |
| Version | Enthält die Versionsnummer des Tokens. |


## <a name="basics-of-registering-an-application-in-azure-ad"></a>Grundlagen zur Registrierung einer Anwendung in Azure AD

Jede Anwendung, die Authentifizierung Azure AD lagert muss in einem Verzeichnis registriert werden. Dieser Schritt umfasst die Anwendung einschließlich der URL, wo es liegt, die URL nach der Authentifizierung den URI der Anwendung und weitere Antworten senden, Azure AD erzählen. Diese Informationen sind einige Gründen erforderlich:

- Azure AD benötigt Koordinaten beim Anmelden oder Austausch von Token mit der Anwendung kommunizieren. Dazu gehören die folgenden:

  - URI der Anwendung-ID: Der Bezeichner für eine Anwendung. Dieser Wert wird in Azure Active Directory während der Authentifizierung an, welche Anwendung die Token für Aufrufer gesendet. Außerdem wird dieser Wert im Token enthalten, damit die Anwendung weiß, war das Ziel.


  - Antwort-URL umleiten URI: Web-API oder Anwendung die Antwort-URL befindet, Azure AD Authentifizierungsantwort einschließlich einen Token sendet, wenn die Authentifizierung erfolgreich war. Bei einer systemeigenen Anwendung ist der URI umleiten einen eindeutigen Bezeichner, Azure AD User-Agent in einer Anforderung OAuth 2.0 umleiten.


  - Client-ID: Die ID für eine Anwendung von Azure AD entsteht bei die Anwendung registriert wird. Wenn ein Autorisierungscode oder Token angefordert werden Client-ID und Schlüssel Azure AD während der Authentifizierung gesendet.


  - Schlüssel: Der Schlüssel, der mit einer Client-ID an bei Azure AD Web API aufrufen.


- Azure AD benötigt, um sicherzustellen, dass die Anwendung die erforderlichen Berechtigungen für die Verzeichnisdaten andere Programme in Ihrer Organisation usw.

Bereitstellung wird klarer, wenn Sie verstehen, dass es zwei Kategorien von Programmen, die entwickelt und Azure AD integriert werden können:

- Einzelne Mieter Anwendung: eine einzelne Anwendung zur Verwendung in einer Organisation. Diese sind normalerweise Line-of-Business (LoB) ein geschrieben. Eine einzelne Anwendung nur von Benutzern in einem Verzeichnis zugegriffen werden muss und daher nur muss in einem Verzeichnis bereitgestellt werden. Diese Programme werden normalerweise von einem Entwickler in der Organisation registriert.


- Multi-Tenant-Anwendung: eine Multi-Tenant-Anwendung soll in vielen Unternehmen nicht nur eine Organisation. Diese sind in der Regel Software-as-a-Service (SaaS) von einem unabhängigen Softwareanbieter (ISV) geschrieben. Multi-Tenant-Applikationen müssen in jedem Verzeichnis, in dem sie verwendet werden, Benutzer oder Administrator Zustimmung zur Registrierung erfordert bereitgestellt. Diese Zustimmung Prozess beginnt, wenn eine Anwendung im Verzeichnis registriert und Graph-API oder vielleicht Web API Zugriff erhält. Wenn ein Benutzer oder Administrator aus einer anderen Organisation registriert die Anwendung, wird ein Dialogfeld angezeigt, die die Berechtigungen wird die Anwendung. Der Benutzer oder Administrator kann dann die Anwendung zustimmen erhält die Anwendungszugriff auf die angegebenen Daten und schließlich die Anwendung in ihrem Verzeichnis registriert. Weitere Informationen finden Sie unter [Übersicht über die Zustimmung](active-directory-integrating-applications.md#overview-of-the-consent-framework).

Einige zusätzliche Aspekte entstehen, wenn eine Multi-Tenant anstatt eine einzelne Anwendung entwickeln. Angenommen, Sie Benutzer in mehreren Verzeichnissen die Anwendung verfügbar gemacht werden, sollten Sie einen Mechanismus zum Bestimmen der Mieter Sie. Eine einzelne Anwendung muss nur Suchen in einem eigenen Verzeichnis für einen Benutzer und eine Multi-Tenant-Anwendung ein bestimmtes Benutzers aus den Verzeichnissen in Azure AD identifizieren. Zu diesem Zweck stellt Azure AD einen gemeinsamen Authentifizierung Endpunkt, jede Anwendung Multi-Tenant-in Anfragen statt eine mandantenspezifische steuern können. Dieser Endpunkt ist https://login.microsoftonline.com/common für alle Verzeichnisse in Azure AD mandantenspezifische Endpunkt https://login.microsoftonline.com/contoso.onmicrosoft.com werden kann. Der gemeinsame Endpunkt ist besonders wichtig, wenn die Anwendung entwickeln, da die notwendige Logik mit mehreren Mandanten-Anmeldung, Abmeldung und token Validierung benötigen.

Wenn Sie eine einzelne Anwendung entwickeln jedoch für viele Organisationen verfügbar machen möchten, können Sie einfach die Anwendung und die Konfiguration in Azure AD mehrinstanzenfähigen kann ändern. Azure AD verwendet darüber hinaus den gleichen Schlüssel für alle Token in allen Verzeichnissen, ob Authentifizierung in einer einzelnen Mandanten oder Multi-Tenant-Anwendung bereitstellen.

Jedes Szenario in diesem Dokument aufgeführten enthält ein Unterbereich, die Bereitstellung Anforderungen beschreibt. Finden Sie ausführliche Informationen über die Bereitstellung einer Anwendung in Azure AD und die Unterschiede zwischen Einzel- und Multi-Tenant-Applikationen [Integrieren Applications Azure Active Directory](active-directory-integrating-applications.md) Informationen. Lesen Sie allgemeine Anwendungsszenarien Azure AD verstehen.

## <a name="application-types-and-scenarios"></a>Anwendungstypen und Szenarien

In diesem Dokument beschriebenen Szenarien kann mit verschiedenen Sprachen und Plattformen entwickelt werden. Sie werden alle durch vollständige Codebeispiele unterstützt in unserem [Code Samples Guide](active-directory-code-samples.md)oder direkt von den entsprechenden [Github Beispiel Repositories](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory)verfügbar sind. Wenn die Anwendung eine bestimmte oder Teil einer End-to-End-Szenario, kann in den meisten Fällen diese Funktionalität unabhängig hinzufügen. Beispielsweise haben Sie eine systemeigene Anwendung, die eine Web-API aufruft, können Sie problemlos eine Anwendung hinzufügen, die auch im Web API aufruft. Das folgende Diagramm veranschaulicht die Szenarien und Anwendungstypen, und andere Komponenten hinzugefügt werden können:

![Anwendungstypen und Szenarien](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Dies sind fünf primäre Anwendungsszenarien von Azure AD unterstützt:

- [Webbrowser Anwendung](#web-browser-to-web-application): muss ein Benutzer eine Anwendung anmelden, die von Azure AD gesichert ist.

- [Einzelne Seite Anwendung (SPA)](#single-page-application-spa): muss ein Benutzer eine Seite Anwendung anmelden, die von Azure AD gesichert ist.

- [Systemeigene Anwendung Web-API](#native-application-to-web-api): eine systemeigene Anwendung, die auf Telefon, Tablet oder PC muss zum Authentifizieren eines Benutzers zu Ressourcen von einer API, die von Azure AD gesichert.

- [Web Application to Web API](#web-application-to-web-api): eine Anwendung muss Ressourcen von einer Web-API von Azure AD gesichert.

- [Daemon oder Server Web API](#daemon-or-server-application-to-web-api): ein Daemon oder eine Serveranwendung ohne Web-Benutzeroberfläche benötigt Ressourcen von einer Web-API von Azure AD gesichert.

### <a name="web-browser-to-web-application"></a>Webbrowser Anwendung

Dieser Abschnitt beschreibt eine Anwendung, die ein Benutzer in einem Webbrowser auf eine Anwendung authentifiziert. In diesem Szenario weist die Anwendung der Browser des Benutzers in Azure AD anmelden. Azure AD gibt eine Antwort über den Browser des Benutzers, der Ansprüche der Benutzer in ein Sicherheitstoken enthält. Dieses Szenario unterstützt einmaliges WS-Federation SAML 2.0 und OpenID Verbindung verwendet.


#### <a name="diagram"></a>Diagramm
![Authentifizierungsablauf Browser Web-Anwendung](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)


#### <a name="description-of-protocol-flow"></a>Beschreibung des Protokolls Fluss


1. Wenn ein Benutzer die Anwendung und muss sich besucht, in Azure AD über eine Anforderung an den Endpunkt Authentifizierung umgeleitet.


2. Benutzer auf der Anmeldeseite anmelden.


3. Wenn die Authentifizierung erfolgreich ist, erstellt ein Authentifizierungstoken Azure AD und gibt eine Antwort auf Antwort-URL der Anwendung, die in Azure-Verwaltungsportal konfiguriert wurde. Für eine produktionsanwendung sollte diese Antwort-URL HTTPS. Das zurückgegebene Token enthält Ansprüche Benutzer und Azure AD, die von der Anwendung überprüft das Token erforderlich sind.


4. Die Anwendung überprüft das Token mithilfe einer öffentlichen Signaturschlüssel Ausstellerinformationen an das Verbundmetadaten-Dokument Azure AD. Nachdem die Anwendung das Token überprüft, startet Azure AD eine neue Sitzung mit dem Benutzer. Diese Sitzung kann der Benutzer auf die Anwendung zugreifen, bis sie abgelaufen ist.


#### <a name="code-samples"></a>Code-Beispiele


Weitere Codebeispiele Webbrowser auf Web Application Scenarios. Und schauen Sie regelmäßig – wir ständig neue Beispiele hinzugefügt. [Webbrowser Web Application](active-directory-code-samples.md#web-browser-to-web-application).


#### <a name="registering"></a>Registrieren


- Einzelnen Mandanten: Wenn Sie eine Anwendung für Ihre Organisation erstellen, müssen sie im Verzeichnis des Unternehmens registriert werden mithilfe von Azure-Verwaltungsportal.


- Multi-Tenant: Wenn Sie eine Anwendung erstellen, die von Benutzern außerhalb der Organisation verwendet werden können, es muss in dem Verzeichnis des Unternehmens registriert, aber auch muss registriert werden, im Verzeichnis der Organisation, die die Anwendung verwenden. Um Ihre Anwendung im Verzeichnis verfügbar machen, können Sie einen Anmeldevorgang einschließen, für Ihre Kunden, die Ihre Anwendung zustimmen können. Wenn sie für Ihre Anwendung anmelden, werden sie ein Dialogfeld, das die Berechtigungen wird die Anwendung und dann die Zustimmung vorgelegt. Je nach erforderlichen Berechtigungen ein Administrator in der Organisation müssen zustimmen. Wenn Benutzer oder Administrator zustimmt, wird die Anwendung im Verzeichnis registriert. Weitere Informationen finden Sie in der [Anwendung in Azure Active Directory integrieren](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Ablauf des Tokens

Die Sitzung des Benutzers läuft nach Ablauf die Lebensdauer von Azure AD ausgestellte Token. Die Anwendung kann dieses Zeitraums gegebenenfalls wie Benutzer basierend auf einem Zeitraum der Inaktivität abgemeldet verkürzen. Wenn die Sitzung abläuft, wird der Benutzer erneut aufgefordert.





### <a name="single-page-application-spa"></a>Einzelne Seite Anwendung (SPA)

Dieser Abschnitt beschreibt die Authentifizierung für eine einzelne Seite-Anwendung, die Azure AD verwendet und OAuth 2.0 implizite Autorisierung gewährt zum Sichern des Web API wieder beenden. Single Page Applications sind in der Regel als Darstellungsschicht JavaScript (front End) strukturiert, die im Browser und ein Web API-Back-End, die auf einem Server ausgeführt und Geschäftslogik der Anwendung implementiert wird. Erfahren Sie mehr über implizite Autorisierung finden Sie unter gewähren und Sie entscheiden, ob es für Ihr Szenario geeignete Hilfe [OAuth2 implizite Gewährung Fluss in Azure Active Directory verstehen](active-directory-dev-understanding-oauth2-implicit-grant.md).

In diesem Szenario beim Benutzer, signiert JavaScript vorne Endverwendungen [Active Directory-Authentifizierungsbibliothek für JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) und die implizite Autorisierung Azure AD einen ID-Token (ID-Token) erhältlich. Das Token wird zwischengespeichert und Client fügt ihn an die Anforderung als trägertoken bei der Web-API aufgerufen back-End, owin-Middleware gesichert ist. 
#### <a name="diagram"></a>Diagramm

![Einzelne Seite Anwendungsdiagramm](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Beschreibung des Protokolls Fluss

1. Der Benutzer navigiert zu der Anwendung.


2. Die Anwendung gibt die JavaScript-front-End (Darstellungsschicht) an den Browser zurück.


3. Der Benutzer initiiert anmelden, z. B. durch Klicken auf ein Zeichen links. Der Browser sendet eine Azure AD Autorisierung Endpunkt einen Token-ID anfordern. Diese Anforderung enthält die Client-ID und Antwort-URL die Abfrageparameter.


4. Azure AD überprüft die Antwort-URL für die registrierte Antwort-URL, die in Azure-Verwaltungsportal konfiguriert wurde.


5. Benutzer auf der Anmeldeseite anmelden.


6. Wenn die Authentifizierung erfolgreich ist, Azure AD erstellt einen ID-Token und als ein URL-Fragment (#), die URL der Anwendung Antwort gibt. Für eine produktionsanwendung sollte diese Antwort-URL HTTPS. Das zurückgegebene Token enthält Ansprüche Benutzer und Azure AD, die von der Anwendung überprüft das Token erforderlich sind.


7. Im Browser ausgeführten JavaScript-Clientcode extrahiert das Token zu verwenden, ruft die Anwendung Web API wieder beenden sichern.


8. Der Browser Ruft die Anwendung Web API wieder mit dem Zugriffstoken Authorization-Header enden.

#### <a name="code-samples"></a>Code-Beispiele


Siehe die Codebeispiele für einzelne Seite Anwendung (SPA) Szenarien. Achten Sie darauf, dass Sie häufig – Schauen wir ständig neue Beispiele hinzugefügt. Die [Seite Anwendung (SPA)](active-directory-code-samples.md#single-page-application-spa).


#### <a name="registering"></a>Registrieren


- Einzelnen Mandanten: Wenn Sie eine Anwendung für Ihre Organisation erstellen, müssen sie im Verzeichnis des Unternehmens registriert werden mithilfe von Azure-Verwaltungsportal.


- Multi-Tenant: Wenn Sie eine Anwendung erstellen, die von Benutzern außerhalb der Organisation verwendet werden können, es muss in dem Verzeichnis des Unternehmens registriert, aber auch muss registriert werden, im Verzeichnis der Organisation, die die Anwendung verwenden. Um Ihre Anwendung im Verzeichnis verfügbar machen, können Sie einen Anmeldevorgang einschließen, für Ihre Kunden, die Ihre Anwendung zustimmen können. Wenn sie für Ihre Anwendung anmelden, werden sie ein Dialogfeld, das die Berechtigungen wird die Anwendung und dann die Zustimmung vorgelegt. Je nach erforderlichen Berechtigungen ein Administrator in der Organisation müssen zustimmen. Wenn Benutzer oder Administrator zustimmt, wird die Anwendung im Verzeichnis registriert. Weitere Informationen finden Sie in der [Anwendung in Azure Active Directory integrieren](active-directory-integrating-applications.md).

Nach der Registrierung der Anwendungdes muss darauf konfiguriert, OAuth 2.0 implizite Gewährung Protokoll verwenden. Dieses Protokoll wird für die Anwendung deaktiviert. Um implizite Gewährung OAuth2-Protokoll für die Anwendung aktivieren, Azure-Verwaltungsportal Anwendungsmanifest herunter, den Wert "oauth2AllowImplicitFlow" auf True gesetzt und manifest zurück auf das Portal hochladen. Ausführliche Informationen finden Sie unter [Aktivierung OAuth 2.0 implizite Gewährung für einzelne Seite](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Ablauf des Tokens

Wenn Sie ADAL.js Authentifizierung mit Azure zu verwenden, haben Sie mehrere Features, die erleichtern einen abgelaufenen Token aktualisieren sowie Token für zusätzliche API Webressourcen, die von der Anwendung aufgerufen werden können. Bei erfolgreicher des Benutzers in Azure AD Authentifizierung wird eine Sitzung geschützt durch ein Cookie für den Benutzer zwischen Browser und Azure AD eingerichtet. Es ist wichtig zu beachten, dass die Sitzung zwischen dem Benutzer und Azure AD und nicht zwischen dem Benutzer und der Webanwendung auf dem Server vorhanden ist. Wenn ein Sicherheitstoken abläuft, wird mit ADAL.js dieser Sitzung automatisch ein anderes Token abgerufen. Dies geschieht mithilfe einen versteckten iFrame zum Senden und Empfangen der Anforderung über das implizite Gewährung OAuth-Protokoll. ADAL.js können auch denselben Mechanismus zu automatisch Zugriffstoken von Azure AD für andere Web-API-Ressourcen, die die Anwendung ruft als diesen Cross-Ursprung Ressourcenfreigabe (CORS) unterstützt im Verzeichnis des Benutzers registriert werden und erforderliche Zustimmung erhielt der Benutzer bei der Anmeldung.


### <a name="native-application-to-web-api"></a>Systemeigene Anwendung Web API


Dieser Abschnitt beschreibt eine systemeigene Anwendung, die für einen Benutzer eine Web-API aufruft. Dieses Szenario baut auf OAuth 2.0 Authorization Code Zuschusstyp öffentlicher Client in Abschnitt 4.1 [OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749)beschrieben. Systemeigene Anwendung erhält ein Zugriffstoken für den Benutzer mithilfe des Protokolls OAuth 2.0. Dieses Zugriffstoken wird dann in der Web-API-Anforderung gesendet, autorisiert den Benutzer und gibt die gewünschte Ressource.

#### <a name="diagram"></a>Diagramm

![Systemeigene Anwendung Web API-Diagramm](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Authentifizierungsablauf für systemeigene Anwendung-API

#### <a name="description-of-protocol-flow"></a>Beschreibung des Protokolls Fluss


Wenn Sie AD-Authentifizierungsbibliotheken verwenden, werden die meisten der unten beschriebenen Protokolldetails, wie Browser-Popup, Zwischenspeichern von token und Behandlung von Aktualisierungstoken behandelt.

1. Mithilfe eines Browsers Popup systemeigene Anwendung Autorisierung Endpunkt in Azure AD anfordert. Diese Anforderung enthält die Client-ID und die Umleitung URI der einheitlichen Anwendung Verwaltungsportal und Anwendung ID-URI für die Web-API. Wenn der Benutzer angemeldet hat nicht, werden sie aufgefordert, erneut


2. Azure Active Directory authentifiziert den Benutzer. Multi-Tenant-Anwendung und Zustimmung ist erforderlich, um die Anwendung, der Benutzer müssen zustimmen, wenn sie dies noch nicht getan haben. Nach Erteilung der Zustimmung und nach erfolgreicher Authentifizierung stellt Azure AD eine Autorisierung Code Antwort zurück an die Clientanwendung Umleitung URI.


3. Bei Azure AD eine Autorisierung Antwortcode auf URI-Umleitung, die Clientanwendung beendet Browser Interaktion und den Autorisierungscode aus der Antwort extrahiert. Mit dieser Berechtigung, fordert die Client-Anwendung token Azure AD-Endpunkt, die den Autorisierungscode enthält details über die Clientanwendung (Client-ID und URI-Umleitung) und die gewünschte Ressource (Anwendung ID-URI für die Web-API).


4. Autorisierungscode und Informationen über die Client-Anwendung sowie API werden von Azure AD überprüft. Nach erfolgreicher Überprüfung gibt Azure AD zwei Token: ein Zugriffstoken JWT und ein JWT Aktualisierungstoken. Darüber hinaus gibt Azure AD grundlegende Informationen wie ihre Anzeige Namen und Mandanten-ID


5. Über HTTPS verwendet die Clientanwendung zurückgegebene Zugriffstoken JWT Web API JWT Zeichenfolge mit "Träger" in der Authorization-Header der Anforderung hinzu. Web-API das JWT-Token überprüft und wenn Validierung erfolgreich ist, gibt die gewünschte Ressource.


6. Ablauf des Zugriffstokens wird die Client-Anwendung eine Fehlermeldung, die angibt, dass der Benutzer erneut authentifizieren muss. Wenn die Anwendung eine gültige Aktualisierungstoken, kann es zu einem Zugriffstoken, ohne dass der Benutzer erneut verwendet werden. Läuft das Aktualisierungstoken müssen die Anwendung interaktiv Benutzer erneut authentifizieren.


> [AZURE.NOTE] Aktualisierungstoken ausgestellt von Azure AD kann Zugriff auf mehrere Ressourcen verwendet werden. Beispielsweise haben Sie eine Anwendung mit der Berechtigung zwei Web-APIs aufrufen, Aktualisierungstoken lässt sich ein token für die anderen Web API zugreifen.


#### <a name="code-samples"></a>Code-Beispiele


Systemeigene Anwendung Web-API-Szenarios finden Sie unter Code-Beispiele. Und schauen Sie regelmäßig – wir ständig neue Beispiele hinzugefügt. [Systemeigene Anwendung Web API](active-directory-code-samples.md#native-application-to-web-api).


#### <a name="registering"></a>Registrieren


- Einzelne Mieter: Sowohl die systemeigene Anwendung und Web-API müssen im selben Verzeichnis in Azure AD registriert werden. Web-API kann konfiguriert werden, um einen Satz von Berechtigungen, verfügbar machen, die die systemeigene Anwendung Zugriff auf die Ressourcen zu beschränken. Die Clientanwendung anschließend wählt die gewünschten Berechtigungen "Berechtigungen to Other Applications" Dropdown-Menü in der Azure-Verwaltungsportal.


- Multi-Tenant: Zuerst registriert die systemeigene Anwendung nur Entwickler oder Publisher des Verzeichnisses. Zweitens ist die systemeigene Anwendung konfiguriert funktionieren erforderlichen Berechtigungen anzugeben. Diese Liste der erforderlichen Berechtigungen wird ein Dialogfeld angezeigt, wenn ein Benutzer oder Administrator im Zielverzeichnis der Anwendung zustimmt, in ihrer Organisation verfügbar ist. Einige Programme erfordern Berechtigungen nur auf alle Benutzer in der Organisation bekommen kann. Andere Programme erfordern Administratorrechte, die ein Benutzer in der Organisation zustimmen kann. Nur Verzeichnisadministrator kann Clientanwendungen zustimmen, die diese Berechtigungen erfordern. Wenn Benutzer oder Administrator zustimmt, wird nur die Web-API in ihrem Verzeichnis registriert. Weitere Informationen finden Sie in der [Anwendung in Azure Active Directory integrieren](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Ablauf des Tokens


Wenn systemeigene Anwendung der Autorisierungscode verwendet ein JWT token zuzugreifen, erhält er auch ein Token JWT aktualisieren. Ablauf des Zugriffstokens Aktualisierungstoken lässt sich der Benutzer erneut authentifiziert werden, ohne dass sie erneut. Dieses Aktualisierungstoken wird dann zum Authentifizieren des Benutzers ein Zugriffstoken und Aktualisierungstoken ergibt.





### <a name="web-application-to-web-api"></a>Web Application to Web-API


Dieser Abschnitt beschreibt eine Anwendung, die Ressourcen aus einem Web API abrufen muss. In diesem Szenario stehen zwei Identität, mit denen die Anwendung authentifizieren, und rufen Sie die Web-API: die Identität einer Anwendung oder einem delegierten Benutzeridentität.

*Anwendungsidentität:* Dieses Szenario verwendet OAuth 2.0 Client Anmeldeinformationen gewähren, wie die Anwendung authentifizieren und Zugriff auf die Web-API. Verwendung eine Anwendungsidentität Web API nur erkennen, dass die Anwendung aufruft, als das Web erhält API keine Informationen über den Benutzer. Erhält die Anwendung Informationen über den Benutzer über das Anwendungsprotokoll gesendet und nicht von Azure AD signiert. Die Web-API vertraut, dass die Anwendung den Benutzer authentifiziert. Aus diesem Grund heißt dieses Musters ein vertrauenswürdiges Subsystem.

*Delegiert Identität:* Dieses Szenario kann auf zwei Arten erreicht werden: OpenID verbinden und OAuth 2.0 Authorization Code gewähren mit vertraulichen Client. Die Anwendung erhält ein Zugriffstoken für den Benutzer die Web-API ist, die der Benutzer erfolgreich authentifiziert, die Anwendung und die Anwendung konnte eine delegierte Benutzeridentität zum Web-API aufrufen. Dieses Zugriffstoken wird in der Web-API-Anforderung gesendet, autorisiert den Benutzer und gibt die gewünschte Ressource.

#### <a name="diagram"></a>Diagramm

![Web Application Web API-Diagramm](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)



#### <a name="description-of-protocol-flow"></a>Beschreibung des Protokolls Fluss

Die Anwendungsidentität und delegierte Benutzer Identitätstypen werden im Fluss unten erläutert. Der wichtige Unterschied ist, dass der delegierte Benutzer zunächst einen Autorisierungscode Identität muss der Benutzer kann anmelden und Zugriff auf die Web-API.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Anwendungsidentität OAuth 2.0 Client Anmeldeinformationen gewähren

1. Ein Benutzer Azure AD in die Anwendung signiert (siehe [Webbrowser Anwendung](#web-browser-to-web-application) oben).


2. Die Anwendung muss ein Zugriffstoken erwerben Web API authentifizieren und die gewünschte Ressource abrufen können. Er fordert Azure AD token Endpunkt, Anmeldeinformationen, Client-ID und Web API-Anwendung URI-ID bereitstellen.


3. Azure AD Anwendung authentifiziert und ein Zugriffstoken JWT, mit der Web-API-aufrufen, zurückgegeben.


4. Über HTTPS verwendet die Anwendung zurückgegebene Zugriffstoken JWT Web API JWT Zeichenfolge mit "Träger" in der Authorization-Header der Anforderung hinzu. Web-API das JWT-Token überprüft und wenn Validierung erfolgreich ist, gibt die gewünschte Ressource.

##### <a name="delegated-user-identity-with-openid-connect"></a>Delegierte Benutzeridentität mit OpenID verbinden

1. Ein Benutzer einer Web-Anwendung mithilfe von Azure AD signiert (siehe Abschnitt [Webbrowser Web Application](#web-browser-to-web-application) ). Wenn der Benutzer die Anwendung nicht noch die Web Anwendung aufrufen, die Web-API in seinem Auftrag zugestimmt hat, muss der Benutzer Zustimmung. Die Anwendung zeigt die Berechtigungen, die erforderlich ist, und diese sind Administratorrechte normaler Benutzer im Verzeichnis nicht werden zuzustimmen. Dabei Zustimmung gilt nur für Multi-Tenant dazu nicht einzelne, wie die Anwendung bereits die erforderlichen Berechtigungen verfügen. Wenn der Benutzer angemeldet, erhalten die Anwendung eine Token-ID, Informationen über die Benutzer einen Autorisierungscode.


2. Mit den Autorisierungscode von Azure AD ausgestellt, fordert die Anwendung token Azure AD-Endpunkt, die den Autorisierungscode enthält details über die Clientanwendung (Client-ID und URI-Umleitung) und die gewünschte Ressource (Anwendung ID-URI für die Web-API).


3. Autorisierungscode und Informationen über die Anwendung und Web-API werden von Azure AD überprüft. Nach erfolgreicher Überprüfung gibt Azure AD zwei Token: ein Zugriffstoken JWT und ein JWT Aktualisierungstoken.


4. Über HTTPS verwendet die Anwendung zurückgegebene Zugriffstoken JWT Web API JWT Zeichenfolge mit "Träger" in der Authorization-Header der Anforderung hinzu. Web-API das JWT-Token überprüft und wenn Validierung erfolgreich ist, gibt die gewünschte Ressource.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Delegierte Benutzeridentität mit OAuth 2.0 Authorization Code

1. Ein Benutzer ist bereits eine Web-Anwendung angemeldet, deren Authentifizierungsmechanismus unabhängig von Azure AD ist.


2. Die Anwendung erfordert einen Autorisierungscode ein Zugriffstoken, damit eine Anforderung über den Browser Azure AD Autorisierung Endpunkt, die Client-ID bereitstellen ausgibt und URI für die Web-Anwendung nach erfolgreicher Authentifizierung umgeleitet. Der Benutzer meldet sich bei Azure AD.


3. Wenn der Benutzer die Anwendung nicht noch die Web Anwendung aufrufen, die Web-API in seinem Auftrag zugestimmt hat, muss der Benutzer Zustimmung. Die Anwendung zeigt die Berechtigungen, die erforderlich ist, und diese sind Administratorrechte normaler Benutzer im Verzeichnis nicht werden zuzustimmen. Dabei Zustimmung gilt nur für Multi-Tenant dazu nicht einzelne, wie die Anwendung bereits die erforderlichen Berechtigungen verfügen.


4. Nachdem der Benutzer zugestimmt hat, empfängt die Anwendung den Autorisierungscode muss ein Zugriffstoken zu.


5. Mit den Autorisierungscode von Azure AD ausgestellt, fordert die Anwendung token Azure AD-Endpunkt, die den Autorisierungscode enthält details über die Clientanwendung (Client-ID und URI-Umleitung) und die gewünschte Ressource (Anwendung ID-URI für die Web-API).


6. Autorisierungscode und Informationen über die Anwendung und Web-API werden von Azure AD überprüft. Nach erfolgreicher Überprüfung gibt Azure AD zwei Token: ein Zugriffstoken JWT und ein JWT Aktualisierungstoken.


7. Über HTTPS verwendet die Anwendung zurückgegebene Zugriffstoken JWT Web API JWT Zeichenfolge mit "Träger" in der Authorization-Header der Anforderung hinzu. Web-API das JWT-Token überprüft und wenn Validierung erfolgreich ist, gibt die gewünschte Ressource.

#### <a name="code-samples"></a>Code-Beispiele

Web Application to Web-API-Szenarios finden Sie in den Codebeispielen. Und schauen Sie regelmäßig – wir ständig neue Beispiele hinzugefügt. [Anwendung Web API](active-directory-code-samples.md#web-application-to-web-api).


#### <a name="registering"></a>Registrieren

- Einzelne Mieter: Für die Anwendungsidentität und delegierte Benutzer Identität Fällen muss die Anwendung und die Web-API in demselben Verzeichnis in Azure AD registriert werden. Web-API kann konfiguriert werden, um einen Satz von Berechtigungen, verfügbar machen, die Zugriff auf die Ressourcen der Anwendung beschränken. Wenn ein delegierter Benutzer Identität verwendet wird, muss die Anwendung in Azure-Verwaltungsportal im Dropdownmenü "Berechtigungen to Other Applications" die gewünschten Berechtigungen aus. Dieser Schritt ist nicht erforderlich, wenn der Typ der Identität verwendet wird.


- Multi-Tenant: Zuerst ist die Anwendung konfiguriert funktionieren erforderlichen Berechtigungen anzugeben. Diese Liste der erforderlichen Berechtigungen wird ein Dialogfeld angezeigt, wenn ein Benutzer oder Administrator im Zielverzeichnis der Anwendung zustimmt, in ihrer Organisation verfügbar ist. Einige Programme erfordern Berechtigungen nur auf alle Benutzer in der Organisation bekommen kann. Andere Programme erfordern Administratorrechte, die ein Benutzer in der Organisation zustimmen kann. Nur Verzeichnisadministrator kann Clientanwendungen zustimmen, die diese Berechtigungen erfordern. Stimmt der Benutzer oder Administrator sind Anwendung und Web-API sowohl in ihrem Verzeichnis registriert.

#### <a name="token-expiration"></a>Ablauf des Tokens

Bei der Anwendung der Autorisierungscode verwendet ein JWT token zuzugreifen, erhält er auch ein Token JWT aktualisieren. Ablauf des Zugriffstokens Aktualisierungstoken lässt sich der Benutzer erneut authentifiziert werden, ohne dass sie erneut. Dieses Aktualisierungstoken wird dann zum Authentifizieren des Benutzers ein Zugriffstoken und Aktualisierungstoken ergibt.


### <a name="daemon-or-server-application-to-web-api"></a>Daemon oder Serveranwendung Web API


Dieser Abschnitt beschreibt eine Daemon oder Server-Anwendung, die Ressourcen aus einem Web API abrufen muss. Zwei untergeordneten Szenarien, die in diesem Abschnitt gelten: ein Daemon, der muss eine Web API basiert auf OAuth 2.0 Client Anmeldeinformationen Zuschusstyp; und einem Server (z. B. eine Web-API), die Web-API basiert auf der Spezifikation OAuth 2.0 On-Behalf-Of aufrufen.

Für das Szenario wird eine Daemon Anwendung muss eine Web-API Aufrufen einiges zu beachten. Benutzerinteraktion ist zunächst nicht mit einer Daemon-Anwendung erfordert die Anwendung ihre eigene Identität. Ein Beispiel für eine Anwendung Daemon ist ein Stapelverarbeitungsauftrag oder ein Betriebssystemdienst im Hintergrund ausgeführt. Dieser Typ von Anwendung fordert ein Zugriffstoken unter Verwendung der Anwendungsidentität und dessen Client-ID-Anmeldeinformationen (Kennwort oder Zertifikat) und Anwendung zu Azure AD-ID-URI. Nach erfolgreicher Authentifizierung Zugriffstoken der Daemon ein von Azure AD dann im Web-API-Aufrufen verwendet wird.

Für das Szenario wird eine Server-Anwendung muss eine Web-API Aufrufen nützlich wird. Angenommen Sie, ein Benutzer auf eine systemeigene Anwendung authentifiziert wurde, muss diese systemeigene Anwendung Web-API aufrufen. Azure AD gibt ein Zugriffstoken JWT Web-API aufrufen. Web-API muss einem anderen downstream Web API aufrufen, können im Auftrag von Flow delegieren die Identität des Benutzers und L2-Web API authentifizieren.

#### <a name="diagram"></a>Diagramm

![Daemon oder Server-Anwendung Web API-Diagramm](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Beschreibung des Protokolls Fluss

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Anwendungsidentität OAuth 2.0 Client Anmeldeinformationen gewähren

1. Zunächst erforderlich die Server-Anwendung in Azure AD als sich ohne menschliches Eingreifen wie eine interaktive Anmeldung Dialogfeld Authentifizierung. Er fordert Azure AD token Endpunkt, Anmeldeinformationen, Client-ID und ID-URI-Anwendung bereitstellen.


2. Azure AD Anwendung authentifiziert und ein Zugriffstoken JWT, mit der Web-API-aufrufen, zurückgegeben.


3. Über HTTPS verwendet die Anwendung zurückgegebene Zugriffstoken JWT Web API JWT Zeichenfolge mit "Träger" in der Authorization-Header der Anforderung hinzu. Web-API das JWT-Token überprüft und wenn Validierung erfolgreich ist, gibt die gewünschte Ressource.


##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Delegierter Benutzeridentität OAuth 2.0 im Auftrag von Spezifikation

Fluss erläutert wird davon ausgegangen, dass ein Benutzer auf eine andere Anwendung (z. B. eine systemeigene Anwendung) authentifiziert wurde und ihre Benutzeridentität zu Zugriffstoken der ersten Ebene Web API verwendet wurde.

1. Systemeigene Anwendung sendet Zugriffstoken an der ersten Ebene Web API.


2. Die ersten Ebene Web API fordert von Azure AD token Endpunkt die Client-ID und Anmeldeinformationen sowie Zugriffstoken des Benutzers. Darüber hinaus wird die Anfrage gesendet, mit einer on_behalf_of-Parameter, der das Web API fordert neue Token eine nachgelagerte Web API für den ursprünglichen Benutzer aufrufen.


3. Azure AD überprüft, dass die ersten Ebene Web API verfügt über Berechtigungen zum Zugriff auf das Web L2-API und validiert die Anforderung zurückgeben ein Zugriffstoken JWT und ein JWT Token auf der ersten Ebene Web API.


4. Über HTTPS Ruft die ersten Ebene Web API L2-Web API ZugewiesenAn token in der Authorization-Header in der Anforderung. Die ersten Ebene Web API kann weiterhin L2-Web-API als Zugriffstoken und Aktualisierungstoken gültig sind.

#### <a name="code-samples"></a>Code-Beispiele

Finden Sie in den Codebeispielen Daemon oder Server-Anwendung Web-API-Szenarios. Und schauen Sie regelmäßig – wir ständig neue Beispiele hinzugefügt. [Server- oder Dämon Web API](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Registrieren

- Einzelne Mieter: Für die Anwendungsidentität und delegierte Benutzer Identität Fällen muss die Daemon oder Server Anwendung im selben Verzeichnis in Azure AD registriert werden. Die Web-API kann konfiguriert werden, um einen Satz von Berechtigungen, verfügbar machen, die den Daemon oder Zugriff auf die Ressourcen des Servers zu beschränken. Wenn ein delegierter Benutzer Identität verwendet wird, muss die Server-Anwendung in Azure-Verwaltungsportal im Dropdownmenü "Berechtigungen to Other Applications" die gewünschten Berechtigungen aus. Dieser Schritt ist nicht erforderlich, wenn der Typ der Identität verwendet wird.


- Mehrinstanzenfähigen: Zuerst wird die Daemon oder Server Anwendung konfiguriert funktionieren erforderlichen Berechtigungen anzugeben. Diese Liste der erforderlichen Berechtigungen wird ein Dialogfeld angezeigt, wenn ein Benutzer oder Administrator im Zielverzeichnis der Anwendung zustimmt, in ihrer Organisation verfügbar ist. Einige Programme erfordern Berechtigungen nur auf alle Benutzer in der Organisation bekommen kann. Andere Programme erfordern Administratorrechte, die ein Benutzer in der Organisation zustimmen kann. Nur Verzeichnisadministrator kann Clientanwendungen zustimmen, die diese Berechtigungen erfordern. Wenn Benutzer oder Administrator zustimmt, registriert sowohl Web-APIs in ihrem Verzeichnis.

#### <a name="token-expiration"></a>Ablauf des Tokens

Wenn die erste Anwendung der Autorisierungscode verwendet ein JWT token zuzugreifen, erhält er auch ein Token JWT aktualisieren. Ablauf des Zugriffstokens Aktualisierungstoken lässt sich der Benutzer erneut authentifiziert, ohne Anmeldeinformationen anzufordern. Dieses Aktualisierungstoken wird dann zum Authentifizieren des Benutzers ein Zugriffstoken und Aktualisierungstoken ergibt.

## <a name="see-also"></a>Siehe auch

[Azure Active Directory Developer's Guide](active-directory-developers-guide.md)

[Azure Active Directory-Beispiele](active-directory-code-samples.md)

[Wichtige Informationen zu Azure AD Key Rollover anmelden](active-directory-signing-key-rollover.md)

[OAuth 2.0 in Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)
