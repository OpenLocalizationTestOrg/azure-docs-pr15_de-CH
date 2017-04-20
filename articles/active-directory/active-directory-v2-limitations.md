<properties
    pageTitle="v2. 0-Endpunkt Grenzen und Einschränkungen | Microsoft Azure"
    description="Eine Liste der Grenzen und mit Azure AD v2. 0-Endpunkt."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="should-i-use-the-v20-endpoint"></a>Sollte den v2. 0-Endpunkt werden verwendet?

Bei der Anwendungsentwicklung, die in Azure Active Directory integrieren, müssen Sie entscheiden, ob Ihre v2. 0-Endpunkt und Authentifizierungsprotokolle Bedürfnisse.  Ursprüngliche Azure AD-Endpunkt wird weiterhin unterstützt und in mancher Hinsicht ist mehr Funktionsumfang als v2. 0.  Aber die v2. 0 Endpunkt [werden erhebliche Vorteile](active-directory-v2-compare.md) für Entwickler, die das Programmiermodell verwenden laden kann.

Zu diesem Zeitpunkt lautet Unsere Empfehlung bei Verwendung des Endpunkts v2. 0

- Wenn Sie persönliche Microsoft-Konten in der Anwendung unterstützen möchten, verwenden Sie den v2. 0-Endpunkt.  Aber in diesem Artikel aufgeführten insbesondere speziell zu arbeiten und Konten Schule Grenzen kennen.
- Wenn die Anwendung nur die Arbeit unterstützen und schulkonten erfordert, sollten Sie [die ursprünglichen Azure AD-Endpunkte](active-directory-developers-guide.md)verwenden.

Im Laufe der Zeit wächst v2. 0-Endpunkt zu aufgeführten, einschränken, damit nur den v2. 0-Endpunkt verwendet werden müssen.  In der Zwischenzeit soll dieser Artikel helfen Ihnen festzustellen, ob der Endpunkt v2. 0 ist.  Werden wir in diesem Artikel auf dem aktuellen Status des Endpunkts v2. 0, so überprüfen erneut bewerten Ihre Bedürfnisse mit den Funktionen der v2. 0 aktualisieren.

Haben Sie eine vorhandene Anwendung in Azure AD, die nicht den v2. 0-Endpunkt verwendet, ist völlig neu beginnen müssen.  In Zukunft werden wir können Sie Ihre vorhandenen Azure AD-Applikationen für die Verwendung mit der Version 2.0 ermöglichen bietet.

## <a name="restrictions-on-apps"></a>Beschränkungen apps
Die folgenden Typen von apps werden von v2. 0-Endpunkt derzeit nicht unterstützt.  Eine Beschreibung der unterstützten Typen von apps finden Sie in [diesem Artikel](active-directory-v2-flows.md).

##### <a name="standalone-web-apis"></a>Eigenständige Web-APIs
Mit der Version 2.0 können Sie [eine Web-API](active-directory-v2-flows.md#web-apis)erstellen, die mit OAuth 2.0 gesichert ist.  Jedoch werden, Web-API nur Token von einer Anwendung empfangen, die gemeinsam die gleiche Anwendung ID  Erstellen einer Webs-API, die von einem Client mit einer anderen Anwendung Id aufgerufen wird, wird nicht unterstützt.  Dieser Client ist nicht möglich oder Berechtigungen für das Web API zu erhalten.

Erstellen einer Web-API, die Token von einem Client mit der gleichen Anwendung Id akzeptiert finden Sie unter 2.0 Endpunkt Web API Samples in [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started).

##### <a name="web-api-on-behalf-of-flow"></a>Web-API im Namen von Fluss
Viele Architekturen enthalten eine Web-API, die anderen downstream Web API sowohl gesicherte v2. 0-Endpunkt aufrufen.  Dieses Szenario wird häufig in systemeigenen Clients, die eine Backend Web API ruft eines Microsoft online Service oder eine andere individuellen Web-API, die Azure AD unterstützt.

Dieses Szenario kann mit OAuth 2.0 Jwt Träger Anmeldeinformationen gewähren, bekannt als Flow On-Behalf-Of unterstützt werden.  Für v2. 0-Endpunkt wird auf Namen der Fluss jedoch derzeit nicht unterstützt.  Um diesem Datenstrom erhältlich Azure AD funktioniert service, [im Auftrag von Codebeispiel auf GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet)prüfen.

## <a name="restrictions-on-app-registrations"></a>App-Registrierungen beschränkt
Zu diesem Zeitpunkt müssen alle apps, die mit der Version 2.0 die Integration eine neuen app-Registrierung auf [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)erstellen.  Vorhandene Azure AD Microsoft Account apps nicht kompatibel mit der Version 2.0 oder apps in einem Portal neben neuen App Registrierungsportal registriert wird.  Wir planen eine Möglichkeit, vorhandene Applikationen für die Verwendung als v2. 0 Anwendung aktivieren. Zu diesem Zeitpunkt jedoch gibt es keine Migrationspfad für eine Anwendung an v2. 0-Endpunkt.

Ebenso funktioniert in der neuen App Registrierung registriert apps nicht gegen den ursprünglichen Azure AD-Authentifizierung Endpunkt.  Jedoch können Sie apps im Portal Registrierung App erstellt Microsoft Konto Authentifizierung Endpunkt erfolgreich Integration `https://login.live.com`.

App-Registrierungen auf [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) erstellt haben darüber hinaus Folgendes:

- **Homepage** -Eigenschaft auch die **Anmeldung URL** wird nicht unterstützt.  Ohne eine Homepage werden diesen nicht im Fenster Office MyApps angezeigt.
- Pro Anwendung Id sind nur zwei app Kennwörter erlaubt.
- App-Registrierung kann nur angezeigt und ein Entwicklerkonto verwaltet werden.  Nicht zwischen mehreren Entwicklern gemeinsam genutzt werden.
- Es gibt mehrere Einschränkungen auf das Format des Redirect_uri zulässig.  Finden Sie weitere Informationen im folgenden Abschnitt.

## <a name="restrictions-on-redirect-uris"></a>Einschränkungen umleiten URIs
Apps, die in der neuen Anwendung Registrierung registriert sind auf bestimmte Redirect_uri Werte eingeschränkt.  Redirect_uri für Web-apps und Services zunächst mit dem `https`, und alle Redirect_uri-Werte müssen eine DNS-Domäne.  Es ist beispielsweise nicht möglich Web app registriert, die Redirect_uris:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Das Registrierungssystem vergleicht gesamten DNS-Namen der vorhandenen Redirect_uri mit DNS-Namen des Redirect_uri, den Sie hinzufügen. Die Anforderung an den DNS-Namen schlägt fehl, wenn eine der folgenden Ursachen vorliegen:  

- Entspricht der gesamte DNS-Name des neuen Redirect_uri nicht den DNS-Namen der vorhandenen redirect_uri
- ist der gesamte DNS-Name des neuen Redirect_uri keine Unterdomäne der vorhandenen redirect_uri

Beispielsweise verfügt die Anwendung derzeit Redirect_uri:

`https://login.contoso.com`

Dann wird hinzugefügt:

`https://login.contoso.com/new`

entspricht genau der DNS-Name oder:

`https://new.login.contoso.com`

Was ist eine DNS-Unterdomäne login.contoso.com.  Wenn Sie eine App Login east.contoso.com hat und Login-west.contoso.com, Redirect_uris, dann muss die folgenden Redirect_uris in Reihenfolge hinzufügen:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Die beiden können hinzugefügt werden, sind untergeordnete Domänen der ersten Redirect_uri contoso.com. Diese Einschränkung wird in einer zukünftigen Version entfernt.

Informationen zum Registrieren einer Anwendung in der neuen Anwendung Registrierung finden Sie in [diesem Artikel](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services--apis"></a>Beschränkungen und APIs
V2. 0-Endpunkt liegt derzeit unterstützt anmelden für jede Anwendung registriert in neue Registrierung Bewerbungsformular, sofern diese in der Liste der [unterstützten Authentifizierung](active-directory-v2-flows.md).  Aber können diese apps nur OAuth 2.0 Zugriffstoken für begrenzte Ressourcen abrufen.  V2. 0-Endpunkt wird nur für Access_tokens ausgeben:

- Die Anwendung, die das Token angefordert hat.  Eine app kann ein Zugriffstoken, erwerben, besteht die logische Anwendung mehrere Komponenten oder Ebenen.  Um dieses Szenario in Aktion zu sehen, checken Sie unsere Lernprogramme für [Erste Schritte](active-directory-appmodel-v2-overview.md#getting-started) .
- Outlook E-Mail, Kalender und Kontakte REST-APIs, die am https://outlook.office.com befinden.  Um Informationen über eine Anwendung schreiben, die diese APIs zugreift, finden Sie diese [Office Schnellstart](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) -Lernprogramme.
- Microsoft Graph-APIs.  Microsoft Graph und alle Daten, die verfügbar sind, finden Sie auf [https://graph.microsoft.io](https://graph.microsoft.io).

Zurzeit sind keine Dienste unterstützt.  In Zukunft werden weitere Microsoft Online Services sowie Support für benutzerdefinierte integrierte Web-APIs und Services hinzugefügt.

## <a name="restrictions-on-libraries--sdks"></a>Beschränkungen Bibliotheken und SDKs
Bibliotheksunterstützung für v2. 0-Endpunkt ist zu diesem Zeitpunkt ziemlich beschränkt.  V2. 0-Endpunkt in einer produktionsanwendung verwendet werden soll, haben Sie folgenden Optionen:

- Wenn Sie eine Anwendung erstellen, können Sie sicher den unsere erhältlich serverseitige Middleware anmelden und Validierung token.  Dazu gehören owin-ID öffnen verbinden Middleware für ASP.NET und unser NodeJS Passport-Plugin.  Codebeispiele unsere Middleware sind im [Einstieg](active-directory-appmodel-v2-overview.md#getting-started) Bereich sowie.
- Andere Plattformen und systemeigene & mobile Anwendung auch integrieren mit der Version 2.0 Sie direkt senden und Empfangen von Nachrichten im Anwendungscode.  Die v2. 0 OpenID verbinden und OAuth Protokolle [explizit dokumentiert](active-directory-v2-protocols.md) die Integration ausführen.
- Open-Source-ID verbinden öffnen und OAuth Bibliotheken können Sie schließlich den Endpunkt v2. 0 integriert.  Das Protokoll v2. 0 sollte kompatibel mit vielen open-Source-Protokollbibliotheken ohne Änderungen.  Die Verfügbarkeit solcher Bibliotheken variiert je nach Sprache und Plattform und [Open ID verbinden](http://openid.net/connect/) und [OAuth 2.0](http://oauth.net/2/) -Websites verwalten eine Liste der gängigen Implementierungen. Weitere Informationen und die Liste der open-Source-Clientbibliotheken und Beispiele, die mit der Version 2.0 getestet finden Sie [Azure Active Directory (AD) v2. 0 und Authentifizierung Bibliotheken](active-directory-v2-libraries.md) .

Wir haben auch nur eine erste Vorschau [Microsoft Authentifizierung Library (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) für .NET veröffentlicht.  Gerne ausprobieren dieser Bibliothek in .NET Client und Server jedoch als Vorschau Bibliothek, nicht GA Qualität begleitet wird, unterstützt.

## <a name="restrictions-on-protocols"></a>Beschränkungen Protokolle
V2. 0-Endpunkt unterstützt nur öffnen ID Verbinden & OAuth 2.0.  Jedoch nicht alle Features und Funktionen der einzelnen Protokolle wurde in den Endpunkt v2. 0 integriert einschließlich:

- OpenID verbinden `end_session_endpoint`, eine Anwendung zum Beenden der Sitzung des Benutzers mit der v2. 0 ermöglicht.
- ID-v2. 0-Endpunkt ausgestellten Token enthalten nur paarweisen Bezeichner für den Benutzer.  Dies bedeutet, dass zwei verschiedene Clientanwendungen verschiedene IDs für denselben Benutzer erhalten.  Beachten Sie, dass Microsoft Graph Abfragen `/me` Endpunkt werden zu einer korrelierbaren-ID für den Benutzer, die von Clientanwendungen verwendet werden können.
- ID-v2. 0-Endpunkt ausgestellten Token enthalten eine `email` Anspruch für den Benutzer zu diesem Zeitpunkt auch wenn Erlaubnis des Benutzers anzeigen e-Mails erhalten.
- Der Endpunkt OpenID Benutzerinformationen herstellen. Benutzerinformationen Endpunkt ist zurzeit nicht auf V2. 0-Endpunkt implementiert.  Alle Benutzerprofildaten erhalten Sie möglicherweise an diesem Endpunkt jedoch von Microsoft Graph `/me` Endpunkt.
- Funktion & Gruppenansprüche.  Zu diesem Zeitpunkt unterstützt v2. 0-Endpunkt ausstellenden Rolle oder Gruppe Ansprüche im ID-Token nicht.

Lesen Sie zum besseren Verständnis Bereich Protokoll Funktionen in V2. 0-Endpunkt durch unsere [OpenID Verbinden & OAuth 2.0 Protokoll Verweis](active-directory-v2-protocols.md).

## <a name="restrictions-for-work--school-accounts"></a>Einschränkung für Konten Schule & arbeiten
Gibt einige Features Microsoft organisatorischen und geschäftlichen Benutzern, die noch nicht vom Endpunkt v2. 0 unterstützt werden.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Gerät bedingten Zugriff, einheitlichen und mobiler apps und Microsoft Graph
V2. 0-Endpunkt unterstützt noch keine Geräteauthentifizierung für mobile und systemeigenen wie systemeigenen apps unter iOS oder Android.  Dies könnte systemeigene Anwendung aufrufen Microsoft Graph für bestimmte Organisationen blockieren.  Geräteauthentifizierung ist erforderlich, wenn der Administrator gerätebasierte bedingte Zugriffsrichtlinie für eine Anwendung festgelegt.  Für den Endpunkt v2. 0 ist das wahrscheinlichste Szenario für bedingten Zugriff gerätebasierte Administrator eine Richtlinie für eine Ressource in Microsoft Graph wie Outlook-API festlegen.  Administrator legt diese Richtlinie und Ihre systemeigene Anwendung fordert einen Token, das die Microsoft Graph wird die Anforderung fehl, da Geräteauthentifizierung noch nicht unterstützt.  Web Applications, die Microsoft Graph-Token angefordert werden jedoch unterstützt, wenn Gerät basierende Richtlinien konfiguriert werden.  Im Web wird app Szenario Geräteauthentifizierung über Web-Browser des Benutzers ausgeführt.

Als Entwickler haben Sie wahrscheinlich keine Kontrolle über Microsoft Graph Ressourcen oder sogar gar geschieht es Richtlinien festgelegt werden.  Beim Erstellen einer Anwendung für Arbeits- und Benutzer verwenden Sie [den ursprünglichen Azure AD-Endpunkt](active-directory-developers-guide.md) erst der v2. 0-Endpunkt Geräteauthentifizierung unterstützt.  Weitere Informationen zu bedingten Zugriff gerätebasierte finden Sie [in diesem Artikel](active-directory-conditional-access.md#device-based-conditional-access).

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Integrierte Windows-Authentifizierung für föderierte Mieter
Wenn Sie ADAL (und damit der ursprünglichen Azure AD-Endpunkt) in Windows-Anwendung verwendet haben, können Sie genutzt haben die SAML-Assertion gewähren so.  Diese Bewilligung kann Benutzer von verbundenen Azure AD Mieter automatisch mit ihren lokalen Active Directory authentifizieren, ohne Anmeldeinformationen eingeben zu müssen.  Die SAML-Assertion Grant ist auf V2. 0-Endpunkt derzeit nicht unterstützt.
