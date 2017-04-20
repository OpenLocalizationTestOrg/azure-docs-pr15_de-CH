<properties
   pageTitle="Azure Active Directory Developer Glossar | Microsoft Azure"
   description="Eine Liste der Begriffe für häufig verwendete Active Directory Azure Developer Konzepte und Features."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory Developer-Glossar
Dieser Artikel enthält Definitionen der Azure Active Directory (AD) Developer Hauptkonzepte, die bei der Entwicklung von Azure AD lernen.

## <a name="access-token"></a>Zugriffstoken
Ein [Sicherheitstoken](#security-token) von einem [autorisierungsserver](#authorization-server)ausgestellt und von einer [Clientanwendung](#client-application) verwendet, um Zugriff auf eine [geschützte Ressourcenserver](#resource-server). In der Regel in Form von [JSON Web Token (JWT)][JWT], das Token verkörpert Zulassung für den Client vom [Ressourcenbesitzer](#resource-owner)für die angeforderte Zugriffsstufe. Das Token enthält alle entsprechenden [Ansprüche](#claim) Thema Aktivierung von der Clientanwendung als eine Form der Anmeldeinformationen verwenden, wenn eine bestimmte Ressource zugreifen. Dadurch auch die Notwendigkeit der Ressourcenbesitzer Anmeldeinformationen für den Client verfügbar machen.

Zugriffstoken werden manchmal als "Benutzer + App" oder "App-Only", je nach Anmeldeinformationen vertreten bezeichnet. Wenn eine Anwendung beispielsweise die:

- ["Autorisierungscode" Bewilligung](#authorization-grant)authentifiziert Benutzer als Besitzer der Ressource Delegieren der Genehmigung an den Client auf die Ressource zuzugreifen. Der Client authentifiziert danach beim Zugriffstoken abrufen. Das Token kann an als ein Token "Benutzer + App" bezeichnet werden wie es den Benutzer, der die Clientanwendung und die Anwendung autorisiert.
- ["Clientanmeldeinformationen" Bewilligung](#authorization-grant)ermöglicht dem Client die alleinige Authentifizierung Funktion ohne die Ressourcenbesitzer Authentifizierung/Autorisierung Token auch als ein Token "Nur App" bezeichnet werden kann.

Siehe [Azure AD Tokenverweis] [ AAD-Tokens-Claims] Weitere Informationen.

## <a name="application-manifest"></a>Anwendungsmanifest  
Eine Funktion von [Azure-Verwaltungsportal][AZURE-classic-portal], die JSON-Repräsentation der Anwendung Identität Konfiguration als aktualisiert die zugehörige [Anwendung] erzeugt[ AAD-Graph-App-Entity] und [ServicePrincipal] [ AAD-Graph-Sp-Entity] Elemente. Finden Sie unter [Understanding Anwendungsmanifest Azure Active Directory] [ AAD-App-Manifest] Weitere Informationen.

## <a name="application-object"></a>Application-Objekt  
Wenn Sie registrieren/aktualisieren eine Anwendung in [Azure-Verwaltungsportal][AZURE-classic-portal], das Portal erstellt/Updates ein Application-Objekt und ein entsprechendes [Service principal-Objekt](#service-principal-object) für diese Mandanten. Anwendungsobjekt *definiert* die Anwendung die Identität Konfiguration global (alle Mandanten, Zugriff hat), bietet einer Vorlage, aus der der entsprechende Service principal-Objekte für die Verwendung zur Laufzeit (in bestimmten Mandanten) lokal *abgeleitet* sind.

[Anwendung] und Service Principal Objekte[ AAD-App-SP-Objects] Weitere Informationen.

## <a name="application-registration"></a>anwendungsregistrierung  
Damit eine Anwendung integrieren und Azure AD Identitäts- und Funktionen delegieren können, muss es mit einer Azure AD [Mieter](#tenant)registriert werden. Beim Registrieren der Anwendung in Azure AD bieten Sie eine Identität Konfiguration für Ihre Anwendung zu Azure AD integriert und Features verwenden:

- Robuste Management von Single Sign-On mit Azure AD Identity Management [OpenID verbinden] [ OpenIDConnect] protokollimplementierung
- Vermittelten Zugriff auf [geschützte Ressourcen](#resource-server) durch [Clientanwendungen](#client-application)über Azure AD OAuth 2.0 [Autorisierung](#authorization-server) Implementierung
- Für das Verwalten des Clientzugriffs auf geschützte Ressourcen, die Ressource Besitzer Autorisierung [Zustimmung Framework](#consent) .

Finden Sie unter [Integrating Applications Azure Active Directory] [ AAD-Integrating-Apps] Weitere Informationen.

## <a name="authentication"></a>Authentifizierung
Der Vorgang der Herausforderung eines legitimen Anmeldeinformationen liefern die Grundlage für die Erstellung von einem Sicherheitsprinzipal für Identitäts- und verwendet werden. Während einer [Bewilligung, OAuth2](#authorization-grant) z. B. füllt Authentifizierung Partei die Rolle [Ressourcenbesitzer](#resource-owner) oder [Clientanwendung](#client-application)verwendet Grant.

## <a name="authorization"></a>Autorisierung
Der Vorgang der Gewährung einer authentifizierten Sicherheitsprinzipal tun. Es gibt zwei primäre Anwendungsfälle in Azure AD-Programmiermodell:

- Bei einer fortlaufenden [OAuth2 Bewilligung](#authorization-grant) : [Ressourcenbesitzer](#resource-owner) Autorisierung an die [Clientanwendung](#client-application)erteilt dem Client ermöglichen, Besitzer der Ressource zugreifen.
- Beim Zugriff auf Ressourcen vom Client: Implementierung der [Ressourcenserver](#resource-server)mit [Anspruch](#claim) Werte stellen im [Zugriffstoken](#access-token) zu Access Control basierend auf sie.

## <a name="authorization-code"></a>Autorisierungscode
Kurzer lebten "token" zur [Clientanwendung](#client-application) durch [autorisierungsendpunkt](#authorization-endpoint)als Teil des Flusses "Autorisierungscode" eines der vier OAuth2 [Autorisierung gewährt](#authorization-grant). Der Code wird in der Clientanwendung auf Authentifizierung ein [Ressourcenbesitzer](#resource-owner), dass der Besitzer der Ressource die angeforderten Ressourcen Zugriff auf delegiert hat zurückgegeben. Als Teil des Flusses der Code später ein [Zugriffstoken](#access-token)eingelöst.

## <a name="authorization-endpoint"></a>autorisierungsendpunkt
Einer der Endpunkte vom [autorisierungsserver](#authorization-server), gewährleistet eine [Bewilligung](#authorization-grant) bei Bewilligung OAuth2 Interaktion mit der [Ressourcenbesitzer](#resource-owner) verwendet gewähren Fluss. Je nach Berechtigung erteilen verwendet kann die tatsächliche Abgangsgeld einschließlich [Autorisierungscode](#authorization-code) oder [Sicherheitstoken](#security-token)variieren.

Finden Sie unter OAuth2 Spezifikationen [Autorisierung gewährt Typen] [ OAuth2-AuthZ-Grant-Types] und [autorisierungsendpunkt] [ OAuth2-AuthZ-Endpoint] Abschnitte und [OpenIDConnect Spezifikation] [ OpenIDConnect-AuthZ-Endpoint] Weitere Informationen.

## <a name="authorization-grant"></a>Berechtigung erteilen
Anmeldeinformationen [der Ressourcenbesitzer](#resource-owner) [Autorisierung](#authorization) auf die geschützten Ressourcen gewährt eine [Clientanwendung](#client-application)darstellt. Eine Clientanwendung können [vier gewähren der Autorisierung OAuth2 festgelegten] [ OAuth2-AuthZ-Grant-Types] zu erteilen, je nach Client/Anforderungen: "Authorization Code Grant", "Client-Anmeldeinformationen gewährt" "implizite Gewährung" und "Ressource Besitzer Anmeldeinformationen gewährt". Die Anmeldeinformationen an den Client zurückgegeben wird ein [Zugriffstoken](#access-token)oder einen [Autorisierungscode](#authorization-code) (gegen später ein Zugriffstoken), je nach Berechtigung erteilen verwendet.

## <a name="authorization-server"></a>autorisierungsserver
[OAuth2 Autorisierungsframeworks]festgelegten[OAuth2-Role-Def], verantwortlich für das Ausstellen von Access Server Token nach erfolgreich [Ressourcenbesitzer](#resource-owner) Authentifizierung und die Autorisierung an den [Client](#client-application) . Eine [Clientanwendung](#client-application) interagiert mit autorisierungsserver zur Laufzeit über die [Autorisierung](#authorization-endpoint) und [token](#token-endpoint) Endpunkte gemäß der OAuth2 definiert [Autorisierung gewährt](#authorization-grant).

Bei Azure AD Anwendungsintegration Azure AD Autorisierung Serverrolle für Azure AD-Applikationen und Microsoft Service-APIs, z. B. [Microsoft Graph-APIs]implementiert[Microsoft-Graph].

## <a name="claim"></a>Forderung
Ein [Sicherheitstoken](#security-token) enthält Ansprüche, die Assertionen eine Entität (z. B. einer [Clientanwendung](#client-application) oder [Ressourcenbesitzer bieten](#resource-owner)) zu einer anderen Entität (z. B. der [Ressourcenserver](#resource-server)). Ansprüche sind Name-Wert-Paare, die Fakten Thema token (z. B. der Sicherheitsprinzipal, der [autorisierungsserver](#authorization-server)authentifiziert) weitergeben. Die Ansprüche in einem gegebenen Token vorhanden sind mehrere Variablen, einschließlich des Tokens Anmeldeinformationen zur Authentifizierung Betreff, Konfiguration usw. abhängig.

Siehe [Tokenverweis Azure AD] [ AAD-Tokens-Claims] Weitere Informationen.

## <a name="client-application"></a>Client-Anwendung  
[OAuth2 Autorisierungsframeworks]festgelegten[OAuth2-Role-Def], eine Anwendung, die Anfragen [Ressourcenbesitzer](#resource-owner)geschützt. Der Begriff "Client" impliziert keine bestimmte Implementierung Hardwareeigenschaften (z. B., ob die Anwendung auf einem Server, Desktop oder anderen Geräten ausgeführt wird).  

Eine Clientanwendung fordert [Autorisierung](#authorization) Ressourcenbesitzer an einen Fluss [OAuth2 Bewilligung](#authorization-grant) und APIs-Daten auf den Namen der Ressource zugreifen kann. [Definiert zwei Arten von Clients]Autorisierungsframeworks OAuth2[OAuth2-Client-Types]"Vertraulich" und "public", basierend auf dem Client seine Anmeldeinformationen vertraulich. Applikationen können [WebClient (vertraulich)](#web-client) implementieren, die auf einem Webserver ein [native Client (public)](#native-client) installiert auf einem Gerät oder [Benutzer-Agent-basierten Client (public)](#user-agent-based-client) im Browser des Geräts ausgeführt wird.

## <a name="consent"></a>Zustimmung
Der Prozess ein [Ressourcenbesitzer](#resource-owner) Erteilung der Genehmigung an die [Clientanwendung](#client-application)bestimmte [Berechtigungen](#permissions) für geschützte Ressourcen für Besitzer der Ressource. Je nach der vom Client angeforderten Berechtigungen werden ein Administrator oder Benutzer aufgefordert, den Zugriff auf ihre Organisation-Person-Daten bzw. Zustimmung. Beachten Sie in einem Szenario mit [mehreren Mandanten](#multi-tenant-application) der Anwendung [Dienstprinzipal](#service-principal-object) auch Mandanten mündigen Benutzer aufgezeichnet wird.

## <a name="id-token"></a>Token-ID
Eine [OpenID verbinden] [ OpenIDConnect-ID-Token] [Sicherheitstoken](#security-token) von [autorisierungsserver](#authorization-server) [autorisierungsendpunkt](#authorization-endpoint)enthält [Ansprüche](#claim) für die Authentifizierung der Benutzer [Ressourcenbesitzer](#resource-owner)bereitgestellt. Wie ein Zugriffstoken ID-Token werden auch als digitale [JSON Web Token (JWT)][JWT]. Im Gegensatz zu einem Zugriffstoken ein ID-Token Ansprüche nicht für Zwecke Ressourcenzugriff verwendet und insbesondere Zugriffskontrolle.

Siehe [Tokenverweis Azure AD] [ AAD-Tokens-Claims] Weitere Informationen.

## <a name="multi-tenant-application"></a>Multi-Tenant-Anwendung
Eine Klasse der [Clientanwendung](#client-application) , anmelden und [Zustimmung](#consent) von Benutzern in jeder Azure AD [Mieter](#tenant)einschließlich Mieter als der Client Sitz bereitgestellt. Dagegen könnten eine Anwendung als einzelne Mandanten nur Anmeldungen von Benutzerkonten in der gleichen Mieter wie bereitgestellt, in denen die Anwendung registriert. [Native Client](#native-client) sind Multi-Tenant standardmäßig [Web-](#web-client) Clientanwendungen zwischen Einzel- und Multi-Tenant ausgewählt haben.

Informationen [zu jeder Azure AD-Benutzer mit der Multi-Tenant-Anwendung] [ AAD-Multi-Tenant-Overview] Weitere Informationen.

## <a name="native-client"></a>Native client
Eine Art von [Client-Anwendung](#client-application) , die direkt auf einem Gerät installiert ist. Da Code auf einem Gerät ausgeführt wird, gilt die "public" Client aufgrund seiner Anmeldeinformationen privat/vertraulich gespeichert. Siehe [OAuth2-Clienttypen und Profile] [ OAuth2-Client-Types] Weitere Informationen.

## <a name="permissions"></a>Berechtigungen
Eine [Clientanwendung](#client-application) erhält Zugriff auf einen [Ressourcenserver](#resource-server) durch Berechtigungsanfragen deklarieren. Zwei Typen sind verfügbar:

- "" Stellvertreter, welche [Bereich](#scopes) Zugriff unter Genehmigung der angemeldeten [Ressourcenbesitzer](#resource-owner)delegiert, die Ressource zur Laufzeit als ["scp" Ansprüche](#claim) im [Zugriffstoken](#access-token)des Clients vorgestellt.
- "Anwendung" Berechtigungen [rollenbasierten](#roles) Zugriff Identität der Anwendung Anmeldeinformationen anfordern/werden als ["Rollen" Ansprüche](#claim) im Zugriffstoken des Clients auf die Ressource zur Laufzeit dargestellt.

Sie auch Flächen bei der [Zustimmung](#consent) vom Administrator oder Ressourcenbesitzer die Gelegenheit, dem Clientzugriff auf Ressourcen in ihrer Mandanten erteilen/verweigern.

Berechtigung auf "Applications" konfiguriert / Registerkarte im [klassischen Azure-Portal]"Configure"[AZURE-classic-portal]"Berechtigungen an andere Programme" durch Auswählen der gewünschten "delegiert Berechtigungen" und "Berechtigungen" (diese muss Mitglied der globalen Administratorrolle). Da [Öffentliche Client](#client-application) Anmeldeinformationen erhalten kann, kann er nur delegierte Berechtigungen anfordern, während [vertrauliche Client](#client-application) Anforderung beide delegiert und Berechtigungen verfügt. Der Client [Application-Objekt](#application-object) speichert die deklarierten Berechtigungen in seiner [RequiredResourceAccess-Eigenschaft][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>Ressourcenbesitzer
[OAuth2 Autorisierungsframeworks]festgelegten[OAuth2-Role-Def], eine Einheit Zugriff auf eine geschützte Ressource. Wenn der Besitzer der Ressource um eine Person handelt, ist es eine genannt. Beispielsweise wenn eine [Clientanwendung](#client-application) [Microsoft Graph API]Postfach eines Benutzers zugreifen möchte[Microsoft-Graph], erfordert Berechtigungen vom Besitzer Ressource des Postfachs.

## <a name="resource-server"></a>Ressourcenserver
[OAuth2 Autorisierungsframeworks]festgelegten[OAuth2-Role-Def], Server, Hosts zur Annahme und reagieren auf Ressourcen schützen geschützt Anfragen von [Clientanwendungen](#client-application) , die ein [Token zuzugreifen](#access-token)darstellen. Auch eine geschützte Ressourcenserver oder ressourcenanwendung.

Einem Ressourcenserver stellt APIs und über Zugriff auf die geschützten Ressourcen [Bereiche](#scopes) und [Rollen](#roles)mit der Autorisierung OAuth 2.0 erzwingt. Beispiele: Azure AD Graph-API bietet Zugriff auf Azure AD Mieter Daten und die Office 365-APIs, die Zugriff auf Daten wie e-Mail und Kalender. Beide sind auch über die [Microsoft Graph-API-][Microsoft-Graph].  

Wie eine Clientanwendung Ressource Anwendungskonfiguration Identität durch [Registrierung](#application-registration) in einer Azure AD-Mandanten Bereitstellen der Anwendung und Service principal-Objekt entsteht. Einige Microsoft bereitgestellten APIs wie Azure AD Graph-API registriert Service Hauptbenutzer bereitgestellt alle während der Bereitstellung bereits.

## <a name="roles"></a>Rollen
Wie [Bereiche](#scopes)können Rollen für einen [Ressourcenserver](#resource-server) Zugriff auf geschützte Ressourcen gesteuert. Es gibt zwei Typen: "Benutzerrolle" implementiert rollenbasierte Zugriffskontrolle für Benutzer/Gruppen, die Zugriff auf die Ressource "" Anwendungsrolle implementiert für [Clientanwendungen](#client-application) , die Zugriff benötigen.

Rollen sind Zeichenfolgen Ressource definiert (beispielsweise "Kosten Genehmiger", "Schreibgeschützt", "Directory.ReadWrite.All") in [Azure-Verwaltungsportal] verwaltete[ AZURE-classic-portal] über die Ressource [Anwendungsmanifest](#application-manifest)in der Ressource [AppRoles-Eigenschaft][AAD-Graph-Sp-Entity]. Klassische Azure-Portal wird auch "Benutzer" Rollen Benutzer zuweisen und konfigurieren Sie Client- [Berechtigungen](#permissions) Zugriff auf eine Rolle "Application" verwendet.

Ausführliche Informationen zu Anwendungsrollen von Azure AD Graph-API verfügbar gemacht werden, finden Sie unter [Graph API Berechtigungsbereichen][AAD-Graph-Perm-Scopes]. Ein Implementierungsbeispiel mit einer schrittweisen finden Sie unter [Rollenbasierte Zugriffskontrolle in Azure AD mit Cloudanwendungen][Duyshant-Role-Blog].

## <a name="scopes"></a>Bereiche
Wie [Rollen](#roles)können Bereiche einem [Ressourcenserver](#resource-server) Zugriff auf geschützte Ressourcen gesteuert. Bereiche zur Implementierung [Bereichsbasierte] [ OAuth2-Access-Token-Scopes] Zugriffskontrolle für eine [Clientanwendung](#client-application) , die delegierten Zugriff auf die Ressource vom Besitzer gewährt wurde.

Bereiche sind Ressource definierten Zeichenfolgen (z. B. "Mail.Read", "Directory.ReadWrite.All") im [klassischen Azure-Portal] verwaltet[ AZURE-classic-portal] über die Ressource [Anwendungsmanifest](#application-manifest)in der Ressource [oauth2Permissions Eigenschaft][AAD-Graph-Sp-Entity]. Azure-Verwaltungsportal dient auch Client-Anwendung Zugriff auf einen Bereich [delegiert Berechtigungen](#permissions) konfigurieren.

Eine beste Practice-Namenskonvention ist ein "resource.operation.constraint" Format. Ausführliche Informationen zu Bereichen von Azure AD Graph-API verfügbar gemacht werden, finden Sie unter [Graph API Berechtigungsbereichen][AAD-Graph-Perm-Scopes]. Bereiche von Office 365-Diensten verfügbar gemachten finden Sie [Office 365-API-Berechtigungen Referenz][O365-Perm-Ref].

## <a name="security-token"></a>Sicherheitstoken
Ein signiertes Dokument Ansprüche oder ein OAuth2 Token SAML 2.0-Assertion enthält. Für eine OAuth2 [die Bewilligung](#authorization-grant), ein [Zugriffstoken](#access-token) (OAuth2) und ein [ID-Token](OpenID Connect) Arten von Sicherheitstoken, die als [JSON Web Token (JWT)]implementiert[JWT].

## <a name="service-principal-object"></a>Service principal-Objekt
Wenn Sie registrieren/aktualisieren eine Anwendung in [Azure-Verwaltungsportal][AZURE-classic-portal], das Portal erstellt/Updates ein [Application-Objekt](#application-object) und ein entsprechendes Service principal-Objekt für diese Mandanten. Anwendungsobjekt *definiert* die Anwendungskonfiguration Identity global (in allen Mandanten, in dem die verknüpfte Anwendung wurde Zugriff gewährt), und die Vorlage, aus der der entsprechende Service principal-Objekte für die Verwendung zur Laufzeit (in bestimmten Mandanten) lokal *abgeleitet* sind.

[Anwendung] und Service Principal Objekte[ AAD-App-SP-Objects] Weitere Informationen.

## <a name="sign-in"></a>Anmelden
Der Prozess der [Clientanwendung](#client-application) Endbenutzer Authentifizierung initiieren und zugehörigen Zustand ein [Sicherheitstoken](#security-token) erwerben und Bereiche Applikationssitzung in diesem Zustand erfassen. Status können Artefakte wie Benutzerprofile und Informationen aus token abgeleitet.

Die Funktion einer Anwendung dient normalerweise zum Implementieren von Single Sign-On (SSO). Es kann auch "Anmeldung"-Funktion als Einstiegspunkt für Endbenutzer Zugriff auf eine Anwendung (bei der ersten Anmeldung) vorangestellt werden. Anmelde-Funktion zum Sammeln und weitere benutzerspezifische Zustand beibehalten und benötigen [Benutzer einverstanden](#consent).

## <a name="sign-out"></a>Abmeldung
Der Prozess nicht authentifizieren [Clientanwendung](#client-application) Sitzung bei der [Anmeldung](#sign-in) ein Endbenutzer den Benutzerstatus trennen zugeordnet

## <a name="tenant"></a>Mieter
Eine Instanz von Azure AD-Verzeichnis ist ein Azure AD-Mandanten genannt. Es bietet eine Vielzahl von Features, einschließlich:

- Registrierungsdienst für integrierte
- Authentifizierung von Benutzerkonten und registrierte Programme
- REST-Endpunkte unterstützt verschiedene Protokolle, einschließlich OAuth2 und SAML einschließlich [autorisierungsendpunkt](#authorization-endpoint), [token Endpunkt](#token-endpoint) und der "Allgemeine" [Multi-Tenant](#multi-tenant-application)-Anwendung erforderlich.

Ein Mieter ist auch mit einer Azure AD oder Office 365-Abonnement während der Bereitstellung des Abonnements Identitäts- und Zugriffsmanagement Features für das Abonnement. Informationen [zu einem Mandanten Azure Active Directory] [ AAD-How-To-Tenant] ausführliche Informationen über die verschiedenen Arten erhalten Sie Zugriff auf einen Mandanten. [Wie Azure-Abonnements] sind Azure Active Directory zugeordnet[ AAD-How-Subscriptions-Assoc] Weitere Informationen zur Beziehung zwischen Abonnements und Azure AD-Mandanten.

## <a name="token-endpoint"></a>Token-Endpunkt
Einer der Endpunkte vom [autorisierungsserver](#authorization-server) unterstützt OAuth2 [Autorisierung gewährt](#authorization-grant). Je nach den Zuschuss einsetzbar ein [Zugriffstoken](#access-token) (und zugehörige 'aktualisieren' Token) an einen [Client](#client-application)oder [Token-ID](#ID-token) mit [OpenID verbinden] [ OpenIDConnect] Protokoll.

## <a name="user-agent-based-client"></a>Benutzer-Agent-basierten client
Eine Art von [Anwendung](#client-application) , Code von einem Webserver gedownloadet und ausgeführt wird wie eine einzelne Seite Anwendung (SPA) in einen Benutzeragenten (z. B. einem Webbrowser). Da Code auf einem Gerät ausgeführt wird, gilt die "public" Client aufgrund seiner Anmeldeinformationen privat/vertraulich gespeichert. Siehe [OAuth2-Clienttypen und Profile] [ OAuth2-Client-Types] Weitere Informationen.

## <a name="user-principal"></a>Benutzerprinzipalnamen
Ähnlich wie ein Dienst verwendet eine Instanz der Anwendung dargestellt, ist ein Benutzerprinzipalobjekt anderen Sicherheitsprinzipal Benutzer dar. Azure AD Graph [Benutzerentität] [ AAD-Graph-User-Entity] definiert das Schema für ein Benutzerobjekt auch benutzerbezogene Eigenschaften wie vor-und Nachname, Benutzerprinzipalnamen, Directory Rollenmitgliedschaft. Dadurch Identität Benutzerkonfiguration für Azure AD Benutzerprinzipalnamen zur Laufzeit herstellen. Benutzerprinzipals wird verwendet, um einen authentifizierten Benutzer für einmaliges Anmelden, Delegierung [Zustimmung](#consent) aufzeichnen, Access Control Beschlüsse usw. darstellen.

## <a name="web-client"></a>WebClient
Eine Art von [Client-Anwendung](#client-application) , die Code auf einem Webserver und Lage führt als "Vertraulich" Client funktioniert durch sichere Anmeldeinformationen auf dem Server gespeichert. Siehe [OAuth2-Clienttypen und Profile] [ OAuth2-Client-Types] Weitere Informationen.

## <a name="next-steps"></a>Nächste Schritte
[Azure AD-Entwicklerhandbuch] [ AAD-Dev-Guide] ist das Portal für alle Azure AD Entwicklung Themen, einschließlich einer Übersicht [Anwendungsintegration] [ AAD-How-To-Integrate] und die Grundlagen von [Azure AD-Authentifizierung und unterstützten Authentifizierungsszenarien][AAD-Auth-Scenarios].

Verwenden Sie Disqus Kommentare Abschnitt zu Feedback zu optimieren und unseren Inhalt.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
