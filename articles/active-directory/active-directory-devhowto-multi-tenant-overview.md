<properties
   pageTitle="Wie Sie eine Anwendung erstellen, die jeder Benutzer Active Directory Azure anmelden können | Microsoft Azure"
   description="Anleitung zum Erstellen einer Anwendung können schrittweise ein Benutzer alle Azure Active Directory Mieter auch Multi-Tenant-Anwendung signieren."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="skwan;bryanla"/>

# <a name="how-to-sign-in-any-azure-active-directory-ad-user-using-the-multi-tenant-application-pattern"></a>Jede Azure Active Directory (AD) Benutzer mit der Multi-Tenant-Anwendung signieren
Wenn Sie eine Software als dienstanwendung Unternehmen anbieten, können Sie Ihrer Anwendung anmelden von Azure AD-Mandanten konfigurieren.  In Azure AD nennt mehrinstanzenfähigen Ihre Anwendung erstellen.  Benutzer in Azure AD-Mandanten werden die Anwendung nach der Zustimmung für ihr Konto die Anwendung anmelden.  

Wenn Sie eine vorhandene Anwendung haben, die hat sein eigenes Konto oder andere Zeichen in andere Cloudanbieter unterstützt, ist Azure AD Anmelden aus jeder Mieter hinzufügen einfach Ihre app registrieren, Anmelden über OAuth2 OpenID verbinden oder SAML hinzufügen und setzen Ihre Anwendung eine Anmeldung mit Microsoft. Klicken Sie unten Weitere Informationen zum branding Ihrer Anwendung.

[! [Schaltfläche Anmelden] [AAD-anmelden]][AAD-App-Branding]


Es wird vorausgesetzt, dass Sie bereits eine einzelne Anwendung für Azure AD kennen.  Wenn Sie nicht, Kopf bis [Developer Guide Homepage] [ AAD-Dev-Guide] und probieren Sie unsere quick Start!

Es gibt vier einfache Schritten Ihre Anwendung in Azure AD Multi-Tenant app konvertiert:

1.  Aktualisieren der anwendungsregistrierung zu Multi-tenant
2.  Aktualisieren Sie den Code zum Senden von Anfragen an die/Common Endpunkt 
3.  Aktualisieren Sie den Code um mehrere Emittenten Werte verarbeiten
4.  Verstehen Sie Benutzer und Administrator Zustimmung und entsprechenden Code ändern

Jeder Schritt im Detail betrachten. Außerdem gelangen Sie direkt zu [dieser Liste mehrinstanzenfähigen Proben][AAD-Samples-MT].

## <a name="update-registration-to-be-multi-tenant"></a>Registrierung, Multi-Tenant aktualisieren
Standardmäßig sind Web app-API registriert in Azure AD einzelne.  Können Sie Ihre Registrierung mehrinstanzenfähigen finden Sie die Option "Anwendung ist Multi-Tenant" auf der Konfigurationsseite Ihrer Anwendung Registrierung im [klassischen Azure-Portal] [ AZURE-classic-portal] und auf "Ja".

Hinweis: Bevor eine Multi-Tenant beantragt werden kann, erfordert Azure AD App-ID-URI der Anwendung eindeutig sein. App-ID URI gehört wie, die eine Anwendung in Protokollnachrichten identifiziert wird.  Für eine einzelne Anwendung genügt für App-ID URI, Pächter eindeutig sein.  Für eine Multi-Tenant-Anwendung muss es eindeutig sein Azure AD Anwendung über alle finden.  Globale Eindeutigkeit wird erzwungen durch Anfordern der App-ID URI, einen Hostnamen verfügen, der eine verifizierte Domäne Azure AD-Mandanten entspricht.  Beispielsweise wurde der Name des Ihrem Mandanten contoso.onmicrosoft.com und eine gültige App ID URI wäre `https://contoso.onmicrosoft.com/myapp`.  Wäre der Mieter verifizierte Domäne `contoso.com`, dann einen gültigen URI der App-ID auch `https://contoso.com/myapp`.  Festlegen einer Anwendung als Multi-Tenant schlägt fehl, wenn die App-ID-URI dieses Muster nicht.

Systemeigene Clientregistrierungen werden Multi-Tenant standardmäßig.  Sie müssen eine Aktion zu einem systemeigenen Client Anwendung Registrierung Multi-Tenant.

## <a name="update-your-code-to-send-requests-to-common"></a>Aktualisieren Sie den Code um Anfragen an/Common
Eine einzelne Anwendung erhalten anmelden Anfragen des Mieters Endpunkt anmelden.   Beispielsweise wäre contoso.onmicrosoft.com Endpunkt:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

Anfragen des Mieters Endpunkt können Benutzer (oder Gäste) in diesem Mandanten Clientanwendungen in diesem Mandanten anmelden.  Mit einer mehrinstanzenfähigen weiß Anwendung vorne welche Mieter Benutzer ist Endpunkt des Mieters Anfragen können nicht.  Stattdessen werden Anfragen an einen Endpunkt gesendet, die über alle Azure AD Mieter Multiplexen:

    https://login.microsoftonline.com/common

Wenn Azure AD erhält eine Anforderung für die/Common Endpunkt es Benutzer anmeldet und folglich entdeckt die Mieter Benutzer stammt.  Die gemeinsamen Endpunkt arbeitet mit allen von Azure AD unterstützten Authentifizierungsprotokolle: OpenID verbinden, OAuth 2.0 SAML 2.0 und WS-Federation.

Das Zeichen auf der Anwendung enthält ein Token, das den Benutzer darstellt.  Issuer-Wert im Token veranlaßt ein welche Mieter Benutzer stammt.  Wenn eine Antwort zurückgegeben, aus der/Common Endpunkt Issuer-Wert im Token des Benutzers Mieter entspricht.  Beachten Sie die/Common ist Endpunkt nicht Mieter und keinen Aussteller ist es einen multiplexer.  Verwendung/Common muss die Logik in Ihre Anwendung Token überprüft aktualisiert werden, um dies zu berücksichtigen. 

Wie bereits erwähnt, vorzusehen Multi-Tenant-Applikationen auch konsistent anmelden Benutzer nach Brandingrichtlinien für Azure AD-Anwendung. Klicken Sie unten Weitere Informationen zum branding Ihrer Anwendung.

[! [Schaltfläche Anmelden] [AAD-anmelden]][AAD-App-Branding]

Werfen Sie einen Blick auf die Verwendung der/Common Endpunkt und Ihre Implementierung Code genauer.

## <a name="update-your-code-to-handle-multiple-issuer-values"></a>Aktualisieren Sie den Code um mehrere Emittenten Werte verarbeiten
ASP.NET-Webanwendungen Web APIs erhalten und Tokens von Azure AD überprüfen.  

> [AZURE.NOTE] Beim systemeigenen Clientanwendungen anfordern und Chips von Azure AD werden diese APIs senden, wo sie überprüft werden.  Systemeigene Anwendung müssen Token nicht überprüft und als undurchsichtig.

Sehen wir uns an, wie eine Anwendung Token überprüft vom Azure AD empfängt.  Eine einzelne Anwendung dauert normalerweise ein Endpunktwert wie:

    https://login.microsoftonline.com/contoso.onmicrosoft.com

und wie erstellen eine Metadaten-URL (in diesem Fall OpenID verbinden) verwenden:

    https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration

zwei entscheidende Informationen herunterladen, mit denen Token überprüfen: Pächter Unterzeichnung Schlüssel und Issuer-Wert.  Jede Azure AD-Mandanten hat einen eindeutige Issuer-Wert des Formulars:

    https://sts.windows.net/31537af4-6d77-4bb9-a681-d2394888ea26/

ist der GUID-Wert umbenennen sichere Version des Mandanten-ID des Mandanten.  Wenn die Metadaten obigen Link auf `contoso.onmicrosoft.com`, diese Issuer-Wert im Dokument angezeigt.

Eine einzelne Anwendung einen Token überprüft wurde, überprüft die Signatur des Tokens für den Signaturschlüssel aus dem Dokument und stellt sicher, dass der Issuer-Wert im Token entspricht, die in dem Dokument gefunden wurde.

Da die/Common Endpunkt nicht Mieter entsprechen und keinen Aussteller untersuchen Issuer-Wert in den Metadaten/common hat einen aus einer Vorlage gebildeten URL statt einen tatsächlichen Wert:

    https://sts.windows.net/{tenantid}/

Daher kann keine mehrinstanzenfähige Anwendung Token überprüfen, durch den Aussteller Wert in den Metadaten mit dem `issuer` Wert im Token.  Multi-Tenant-Anwendung benötigt Logik Aussteller Werte gültig sind und welche nicht entscheiden, basierend auf den Mieter ID Teil der Aussteller.  

Beispielsweise wenn eine Multi-Tenant-Anwendung nur anmelden bestimmten Mandanten, die für den Dienst angemeldet haben, es muss überprüfen den Issuer-Wert oder `tid` Anspruchswert im Token sicherzustellen, Pächter in ihre Liste der Abonnenten.  Wenn eine Multi-Tenant-Anwendung nur Personen behandelt, nicht Zugriff basierend auf Mieter Entscheidungen können sie Issuer-Wert vollständig ignorieren.

In der Multi-Tenant-Beispiele finden Sie im Abschnitt [Verwandte Inhalt](#related-content) am Ende dieses Artikels ist Aussteller Validierung deaktiviert, um jede Azure AD-Mandanten anmelden aktivieren.

Jetzt sehen wir uns die Benutzerfunktionalität für Benutzer, die Multi-Tenant-Anwendung anmelden.

## <a name="understanding-user-and-admin-consent"></a>Grundlegendes zu Benutzer- und Administrator Zustimmung
Ein Benutzer einer Anwendung in Azure AD anmelden muss die Anwendung in Ihrem Mandanten dargestellt.  Dies ermöglicht es der Organisation Dinge wie eindeutige Richtlinien gelten, wenn Benutzer ihren Mandanten der Anwendung anmelden.  Diese Registrierung ist für eine einzelne Anwendung einfach. ist das passiert, wenn die Anwendung im [klassischen Azure-Portal]registriert eine[AZURE-classic-portal].

Multi-Tenant-Anwendung befindet sich in die Registrierung für die Anwendung in Azure AD Mieter vom Entwickler verwendet.  Wenn ein Benutzer von einem anderen Mandanten die Anwendung zum ersten Mal anmeldet Azure AD bittet sie, die von der Anwendung angeforderten Berechtigungen zuzustimmen.  Wenn sie zustimmen, eine Darstellung der Anwendung einen *Dienstprinzipal* wird in Ihrem Mandanten erstellt und anmelden können weiterhin. Delegierung wird auch im Verzeichnis erstellt, in dem die Zustimmung zur Anwendung aufgezeichnet. [Anwendungsobjekte] und Service Principal Objekte[ AAD-App-SP-Objects] ausführliche Informationen über der Anwendung Objekte Application und ServicePrincipal und wie sie zueinander.

![Zustimmung zur Single-Tier-Anwendung][Consent-Single-Tier] 

Diese Zustimmung Erfahrung ist von der Anwendung angeforderten Berechtigungen betroffen.  Azure AD unterstützt zwei Arten von Berechtigungen nur für app und delegierte:

- Delegierte Berechtigung gewährt eine Anwendung als angemeldet Benutzer für eine Teilmenge der Dinge handeln den Benutzer tun.  Sie können z. B. einer Anwendung Delegierte Berechtigung des angemeldeten Benutzers Kalender Lesen erteilen.
- Eine app nur direkt an die Identität der Anwendung Berechtigung ist.  Beispielsweise nur-app-Berechtigung zum Lesen der Liste der Benutzer in einem Mandanten einer Anwendung gewähren und es werden Sie unabhängig davon, bei der Anwendung angemeldet ist.

Einige Berechtigungen können durch einen normalen Benutzer zugestimmt werden andere Tenant-Administrator Genehmigung erfordern. 

### <a name="admin-consent"></a>Admin-Zustimmung
Nur App Zustimmung immer eine-Mandantenadministrators sein.  Wenn Ihre Anwendung eine Berechtigung nur app fordert normaler Benutzer versucht, die Anwendung anzumelden, erhalten die Anwendung Fehlermeldung Zustimmung kann der Benutzer nicht.

Bestimmte delegierten Berechtigungen Zustimmung auch eine-Mandantenadministrators sein.  Beispielsweise können wieder Azure AD Schreiben der angemeldeten Benutzer ein-Mandantenadministrators Zustimmung erfordert.  Wie app-Berechtigungen Wenn gewöhnliche Benutzer einer Anwendung anmelden, die delegierte Berechtigung anfordert, die Administrator Zustimmung erfordert erhalten die Anwendung Fehler.  Ob eine Berechtigung Admin Zustimmung hängt vom Entwickler, der die Ressource veröffentlicht und finden Sie in der Dokumentation für die Ressource benötigt.  Links zu Themen, in denen die verfügbaren Berechtigungen für Azure AD Graph-API und Microsoft Graph-API sind im Abschnitt [Verwandte Inhalt](#related-content) dieses Artikels.

Wenn Ihre Anwendung Berechtigungen, die Zustimmung Admin verwendet, müssen Sie eine Bewegung in Ihrer Anwendung z. B. eine Schaltfläche oder einen Link, der Administrator die Aktion initiieren kann.  Für diese Aktion ist eine übliche Identifizierungsprozess OAuth2-OpenID verbinden, die auch die Anwendung sendet Anforderung der `prompt=admin_consent` -Parameters abgefragt.  Der Administrator zugestimmt hat und den Dienstprinzipalnamen des Debitors Mandanten erstellt, nachfolgende Anfragen anmelden müssen nicht die `prompt=admin_consent` Parameter.   Da der Administrator entschieden hat, werden die erforderlichen Berechtigungen, werden Zustimmung zu diesem Zeitpunkt keine anderen Benutzer Pächter aufgefordert.

Die `prompt=admin_consent` Parameter kann auch von Clientanwendungen, die Berechtigungen, die Admin Zustimmung nicht erforderlich anzufordern, jedoch wollen Erlebnis, Pächter Admin "angemeldet" für die Anwendung einmal verwendet und keine anderen Benutzer Aufforderung zur Zustimmung zu diesem Zeitpunkt auf.

Wenn eine Anwendung Administrator Zustimmung erfordert und der Administrator meldet sich bei der Anwendungdes die `prompt=admin_consent` Parameter nicht gesendet, der Administrator die Anwendung erfolgreich zustimmen werden aber sie werden nur für dieses Benutzerkonto bekommen.  Normale Benutzer werden möglicherweise trotzdem nicht anmelden und die Zustimmung zur Anwendung.  Dies ist nützlich, wenn Sie dem Tenant-Administrator die Möglichkeit, die Anwendung vor anderen Benutzern Zugriff gewähren möchten.

Tenant-Administrator kann reguläre Benutzer Zustimmung Anwendung deaktivieren.  Wenn diese Funktion deaktiviert ist, steht Admin Zustimmung für die Anwendung in den Mandanten eingerichtet werden.  Wenn Sie Ihre Anwendung mit normaler Benutzer Zustimmung deaktiviert testen möchten, finden Sie die Konfigurationsoption in Azure AD-Mandanten Konfigurationsabschnitt der [Azure-Verwaltungsportal][AZURE-classic-portal].

> [AZURE.NOTE] Einige Programme soll eine Erfahrung, reguläre Benutzer können zunächst Zustimmung und die Anwendung später Administrator und fordern Berechtigungen, die Zustimmung der Verwaltung betreffen.  Es gibt keine Möglichkeit dazu mit einer Anwendung in Azure AD heute.  Die bevorstehende Azure AD v2 Endpunkt können Clientanwendungen Anfrageberechtigungen zur Laufzeit statt bei der Anmeldung dieses Szenario ermöglicht.  Weitere Informationen finden Sie in [Azure AD App-Modell v2-Entwicklerhandbuch][AAD-V2-Dev-Guide].

### <a name="consent-and-multi-tier-applications"></a>Zustimmung und Multi-Tier-Anwendung
Ihre Anwendung möglicherweise mehrere Ebenen, die in Azure AD durch eigene Registrierung dargestellt.  Beispielsweise ruft eine systemeigene Anwendung, die eine Web-API aufruft oder eine Anwendung, die eine Web-API.  In beiden Fällen fordert der Client (systemeigene Anwendung oder WebApp) Berechtigungen für die Ressource (Web-API) aufrufen.  Der Client erfolgreich in einen Debitor Mieter zugestimmt ist alle Ressourcen, Berechtigungen anfordert, Kunden Mandanten.  Wenn diese Bedingung nicht erfüllt, wird Azure AD einen Fehler zurück, dass die Ressource zuerst hinzugefügt werden muss.

Dies kann ein Problem sein, die logische Anwendung besteht aus zwei oder mehr Anwendung erfassen, beispielsweise einen separaten Client und Ressource.  Wie Sie die Ressource in der Kunde Mieter erste?  Azure AD umfasst dabei Client und Ressourcen in einem einzigen Schritt zugestimmt, in dem der Benutzer insgesamt durch den Client und die Ressource auf Zustimmung angeforderten Berechtigungen aktivieren.  Um dieses Verhalten zu aktivieren, muss die Ressource anwendungsregistrierung enthalten App-ID des Clients als ein `knownClientApplications` im Anwendungsmanifest.  Zum Beispiel:

    knownClientApplications": ["94da0930-763f-45c7-8d26-04d5938baab2"]

Diese Eigenschaft kann über die Ressource [Anwendungsmanifest]aktualisiert[AAD-App-Manifest], und in einem einheitlichen Multi-Tier-Client aufrufen Web API-Beispiel im Abschnitt [Verwandte Inhalt](#related-content) am Ende dieses Artikels gezeigt. Im folgenden Diagramm Überblick Zustimmung für eine Anwendung mit mehreren Ebenen:

![Zustimmung zur bekannten Multi-Tier-Anwendung][Consent-Multi-Tier-Known-Client] 

Ähnliche Fall die verschiedenen Ebenen einer Anwendung in verschiedenen Mandanten registriert sind.  Beispielsweise bei Erstellen einer systemeigenen Clientanwendung, die die Office 365 Exchange Online-API aufruft.  Entwickeln Sie die systemeigene Anwendung und später für die systemeigene Anwendung des Kunden Mieter ausführen muss Exchange Online Dienstprinzipalnamen vorhanden sein.  In diesem Fall hat der Kunde für den Service principal in ihrer Mandanten erstellt Exchange Online erwerben.  Bei einer API von einem Unternehmen als Microsoft erstellt muss der Entwickler der API können Kunden Zustimmung ihrer Anwendung in Mieter Kunde z. B. eine Webseite, die mit den in diesem Artikel beschriebenen Mechanismen Zustimmung steuert.  Nach dem Erstellen der Dienstprinzipalnamen in der Mieter erhalten systemeigene Anwendung Token API.

Im folgenden Diagramm Überblick Zustimmung für eine Multi-Tier-Anwendung in verschiedenen Mandanten registriert:

![Zustimmung zur mehrteiliges Multi-Tier-Anwendung][Consent-Multi-Tier-Multi-Party] 

### <a name="revoking-consent"></a>Widerruf
Benutzer und Administratoren können die Anwendung jederzeit widersprechen:

- Benutzer widerrufen Zugriff auf einzelne Programme aus [Access Panel-Anwendung] [ AAD-Access-Panel] Liste.
- Administratoren Zugriff auf Programme aus Azure AD mit Azure AD Verwaltungsabschnitt des [klassischen Azure-Portal]Löschen widerrufen[AZURE-classic-portal].

Stimmt ein Administrator eine Anwendung für alle Benutzer in einem Mandanten, widerrufen nicht Benutzer einzeln Zugriff.  Nur der Administrator Zugriff widerrufen kann, und nur für die gesamte Anwendung.

### <a name="consent-and-protocol-support"></a>Zustimmung und Unterstützung
Zustimmung in Azure AD über OAuth, OpenID verbinden, unterstützt WS-Federation und SAML-Protokolle.  Die SAML und WS-Federation Protokolle unterstützen die `prompt=admin_consent` Parameter so Admin Zustimmung nur über OAuth und OpenID verbinden.

## <a name="multi-tenant-applications-and-caching-access-tokens"></a>Multi-Tenant-Applikationen und Zwischenspeichern Zugriffstoken
Multi-Tenant-Applikationen erhalten Zugriffstoken APIs aufrufen, die von Azure AD geschützt sind.  Ein häufiger Fehler wird Active Directory Authentifizierung Library (ADAL) mit einer mehrinstanzenfähigen Anwendung zunächst einen Token für einen Benutzer/Common, Anforderung beantwortet und fordern dann nachfolgenden Token für den gleichen Benutzer auch mit/Common.  Da die Antwort von Azure AD stammt nicht von Mieter gemeinsam ADAL speichert das Token als Pächter. Der nachfolgende Aufruf von/Common ein Zugriffstoken für den Benutzer zu den Cacheeintrag fehlt, und der Benutzer wird aufgefordert, sich erneut anzumelden.  Zur Vermeidung von Cache fehlt, sicherzustellen Sie, dass nachfolgende für einen bereits angemeldeten Benutzer des Mieters Endpunkt aufgerufen werden.

## <a name="related-content"></a>Verwandte Inhalte

- [Multi-Tenant-Anwendungsbeispiele][AAD-Samples-MT]
- [Richtlinien für Applikationen][AAD-App-Branding]
- [Azure AD-Entwicklerhandbuch][AAD-Dev-Guide]
- [Anwendung und Service Principal-Objekte][AAD-App-SP-Objects]
- [Integration von Applikationen in Azure Active Directory][AAD-Integrating-Apps]
- [Übersicht über die Zustimmung][AAD-Consent-Overview]
- [Microsoft Graph API Berechtigungsbereiche][MSFT-Graph-AAD]
- [Azure AD Graph API Berechtigungsbereiche][AAD-Graph-Perm-Scopes]

Verwenden Sie Disqus Kommentare Abschnitt zu Feedback zu optimieren und unseren Inhalt.

<!--Reference style links IN USE -->
[AAD-Access-Panel]:  https://myapps.microsoft.com
[AAD-App-Branding]: ./active-directory-branding-guidelines.md
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Consent-Overview]: ./active-directory-integrating-applications.md#overview-of-the-consent-framework
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Overview]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-graph-api/
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Samples-MT]: https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multitenant
[AAD-Why-To-Integrate]: ./active-directory-how-to-integrate.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[MSFT-Graph-AAD]: https://graph.microsoft.io/en-us/docs/authorization/permission_scopes

<!--Image references-->
[AAD-Sign-In]: ./media/active-directory-devhowto-multi-tenant-overview/sign-in-with-microsoft-light.png
[Consent-Single-Tier]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-single-tier.png
[Consent-Multi-Tier-Known-Client]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-known-clients.png
[Consent-Multi-Tier-Multi-Party]: ./media/active-directory-devhowto-multi-tenant-overview/consent-flow-multi-tier-multi-party.png

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AAD-V2-Dev-Guide]: ./active-directory-appmodel-v2-overview.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Code-Grant-Flow]: https://msdn.microsoft.com/library/azure/dn645542.aspx
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3 
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken














