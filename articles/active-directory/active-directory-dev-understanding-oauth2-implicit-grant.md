<properties
   pageTitle="Grundlegendes zur impliziten OAuth2 Fluss in Azure Active Directory erteilen | Microsoft Azure"
   description="Informationen über Azure Active Directory-Implementierung der impliziten OAuth2-Flow gewähren und für Ihre Anwendung."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Grundlegendes zu OAuth2 implizite Gewährung Fluss in Azure Active Directory (AD)

Die implizite Gewährung OAuth2 ist oftmals mit der längsten Sicherheitsprobleme in der Spezifikation OAuth2 gewähren. Und doch ist der Ansatz von ADAL JS und die empfehlen wir beim Schreiben von SPA Applikationen implementiert. Was gibt? Es kommt der Nachteile: und wie sich herausstellt, wird die implizite Gewährung am besten können Sie verfolgen, Anwendung, die eine Web-API über JavaScript in einem Browser verwenden.

## <a name="what-is-the-oauth2-implicit-grant"></a>Was ist die implizite Gewährung OAuth2?

Der ultimative [OAuth2 Autorisierung erteilt](https://tools.ietf.org/html/rfc6749#section-1.3.1) ist Berechtigung erteilen, zwei separate Endpunkte verwendet. Autorisierungsendpunkt der Phase Interaktion verwendet, für die ein Autorisierungscode führt. Token Endpunkt wird vom Client verwendet für den Austausch von Code für ein Zugriffstoken und oft ein Aktualisierungstoken. ASP.NET-Webanwendungen müssen eigene Anwendung Anmeldeinformationen token Endpunkt, autorisierungsserver den Client authentifizieren kann.

[Implizite OAuth2 gewähren](https://tools.ietf.org/html/rfc6749#section-1.3.2) ist eine Variante ermöglicht einen Client zu einem Zugriffstoken (und ID-Token bei [OpenId verbinden](http://openid.net/specs/openid-connect-core-1_0.html)) direkt aus dem autorisierungsendpunkt Kontaktaufnahme mit token Endpunkt und Authentifizieren der Clientanwendung. Diese Variante wurde speziell für JavaScript-Anwendung basierte, die in einem Webbrowser ausgeführt: in der ursprünglichen Spezifikation OAuth2 Token in einem URI-Fragment zurückgegeben werden. Dadurch token Bits für den JavaScript-Code auf dem Client, aber garantiert, dass sie nicht in leitet auf dem Server enthalten sind. Direkt aus dem autorisierungsendpunkt leitet Token Browser zurückgeben. Es hat auch den Vorteil, dass alle an cross-Ursprung Aufrufe sind erforderlich, wenn token Endpunkt an die JavaScript-Anwendung erforderlich ist.

Ein wichtiges Merkmal eines OAuth2 implizite Gewährung ist die Tatsache, dass so nie zurückgeben Aktualisierungstoken an den Client übergeben. Wie wir im nächsten Abschnitt sehen werden, das nicht wirklich erforderlich und sogar ein Sicherheitsproblem.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Geeignete Szenarien für die implizite Gewährung von OAuth2

Als OAuth2-Spezifikation selbst deklariert wurde die implizite Gewährung entwickelt um Benutzer-Agent-Anwendung – d. h. JavaScript-Anwendung in einem Browser ausführen aktivieren. Das entscheidende Merkmal dabei ist, dass JavaScript-Code für den Zugriff auf Serverressourcen (normalerweise Web-API) und für die Anwendung UX entsprechend aktualisieren. Programme wie Google Mail oder Outlook Web Access vorstellen: Bei Auswahl eine Nachricht aus Ihrem Posteingang nur Visualisierung Meldungsbereich ändert sich, um die neue Auswahl angezeigt, während der Rest der Seite unverändert bleibt. Dies ist im Gegensatz zu herkömmlichen Umleitung-basierten Web apps, führt jeder Benutzerinteraktion ein Postback der Seite an Seite gerenderte Antwort des neuen Servers.

Programme, die extrem JavaScript-basierten Ansatz zu Single Page Applications oder SPA heißen: soll diese Anträge dienen nur ein HTML-Startseite und zugeordnete JavaScript, alle nachfolgenden Interaktionen von Web-API-Aufrufe gesteuert über JavaScript ausgeführt. Jedoch hybride Ansätze, die Anwendung ist meist Postback jedoch gelegentlich JS Ruft, sind keine Seltenheit – die Diskussion über implizite Fluss Verwendung für die sowie.

Umleitung-basierte sichere normalerweise ihre Anfragen über Cookies jedoch Ansatz nicht auch für JavaScript-Applikationen funktioniert. Cookies sind nur für die Domäne, der sie für generiert wurden während JavaScript aufrufen zu anderen Domänen geleitet werden können. Tatsächlich werden die häufig der Fall: Microsoft Graph API Office API Azure API – alle Wohnsitz außerhalb der Domäne aus, in dem der Antrag auf Anwendung vorstellen. Vermehrt für JavaScript-Anwendung ist, müssen keine Back-End-100 % 3. Partei Web APIs Unternehmen Funktionen implementieren.

Derzeit ist die bevorzugte Methode zum Schutz einer Web-API-Aufrufe für die OAuth2 Träger token Ansatz, in dem jeder Aufruf von Zugriffstoken OAuth2 begleitet. Web-API untersucht eingehende Zugriffstoken und wenn es die erforderlichen Bereiche gefunden, gewährt Zugriff auf den angeforderten Vorgang. Implizite Fluss bietet ein bequemes Verfahren für JavaScript-Applikationen zu Zugriffstoken für eine Web-API bietet zahlreiche Vorteile gegenüber Cookies:

- Token zuverlässig ohne Kreuz Ursprung aufrufen – obligatorische Eintragung der Umleitung erhalten URI, Token zurückgegeben werden, garantiert, dass Token nicht verschoben werden
- JavaScript-Applikationen erhalten so viele Zugriffstoken Bedarf für beliebig viele Web-APIs sie Ziel – ohne Einschränkung auf Domänen,
- HTML5-Funktionen wie Sitzung oder lokaler Speicher gewähren token Zwischenspeichern und Verwaltung der Lebensdauer, während Cookies für die Anwendung ist
- Zugriffstoken werden nicht anfällig für siteübergreifende Anforderungsfälschungsangriffe (CSRF)

Der implizite Gewährung Fluss erteilt Aktualisierungstoken meist aus Sicherheitsgründen. Ein Aktualisierungstoken ist nicht so eng wie Zugriffstoken gewährt macht bei ausgetreten ist daher viel mehr Schaden zuzufügen beschränkt. Implizite Fluss Token werden in der URL, damit die Überwachung ist höher als Authorization Code erteilt.

Beachten Sie jedoch, dass JavaScript-Anwendung einen anderen Mechanismus für die Erneuerung von Zugangs-Token ohne wiederholt Anmeldeinformationen für Benutzer verfügt. Anwendung können versteckten Iframe neue token Abfragen autorisierungsendpunkt von Azure AD ausführen: solange der Browser weiterhin eine aktive Sitzung (lesen: hat einen Sitzungscookie) Azure AD-Domäne die Authentifizierungsanfrage erfolgreich ohne Benutzerinteraktion auftreten kann. 

Dieses Modell ermöglicht JavaScript-Anwendung den Zugriffstoken unabhängig erneuern und sogar für eine neue API (vorausgesetzt, dass der Benutzer zuvor sie zugestimmt. Dies vermeidet die zusätzliche Belastung für Anschaffung, Wartung und Schutz eine Artefakt hohen Wert wie ein Aktualisierungstoken. Ermöglicht die automatische Erneuerung Artefakt Sitzungscookie Azure AD wird außerhalb der Anwendung. Ein weiterer Vorteil dieses Ansatzes ist von Azure AD mit jeder Anwendung in Azure AD im Browser Registerkarten angemeldet Benutzer abmelden kann. Löschen von Azure AD Sitzungscookie dadurch und JavaScript-Anwendung wird automatisch nicht mehr Token für den Benutzer, signierte erneuern.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Ist die implizite Gewährung für meine Anwendung?

Die implizite Gewährung stellt mehr Risiken als andere gewährt. Beachten müssen Bereiche sind gut dokumentiert (Siehe beispielsweise [Missbrauch von Zugriffstoken imitieren Ressource Besitzer in impliziten Flow] [ OAuth2-Spec-Implicit-Misuse] und [OAuth 2.0 Bedrohungsmodell und Sicherheitsaspekte][OAuth2-Threat-Model-And-Security-Implications]). Höhere Risikoprofil ist jedoch weitgehend, da Programme aktivieren, die aktiven Code bereitgestellt durch eine Remoteressource Browser ausgeführt werden soll. Wenn Sie planen eine Architektur SPA haben keine Back-End-Web-API über JavaScript aufrufen möchten und implizite Fluss für token Übernahme wird empfohlen.

Wenn die Anwendung eine native Client, nicht implizite Fluss ideal. Fehlender Azure AD Sitzungscookie im Kontext der native Client nimmt Anwendung bedeutet eine langlebige Sitzung aufrechterhalten. Das bedeutet, dass der Anwendung fordert wiederholt den Benutzer beim Zugriffstoken für neue Ressourcen.

Wenn Sie eine Anwendung Backend und eine API Backend-Code verwenden entwickeln, ist implizit auch nicht geeignet. Andere Zuschüsse geben macht. Beispielsweise bietet OAuth2 Client Anmeldeinformationen gewähren Token zu erhalten, die die Berechtigungen für die Anwendung selbst, im Gegensatz zu Benutzer Delegationen widerspiegeln. Das bedeutet, dass der Client die Möglichkeit, den programmgesteuerten Zugriff auf Ressourcen auch beibehalten, wenn ein Benutzer in einer Sitzung usw. nicht aktiv ist. Nicht nur, aber solche gewährt höhere Sicherheit gewährleistet. Beispielsweise Zugriffstoken transit nie über den Browser des Benutzers, sie nicht Risiko im Browserverlauf gespeichert und so weiter. Die Clientanwendung kann auch starken Authentifizierung ausführen einen Token anfordert.

## <a name="next-steps"></a>Nächste Schritte

- Eine vollständige Liste der Ressourcen einschließlich Informationen für Protokolle und OAuth2 Autorisierung Flows Unterstützung von Azure AD, finden Sie in [Azure AD Developer's Guide][AAD-Developers-Guide]
- Informationen [zum Integrieren von einer Anwendung in Azure AD]  [ ACOM-How-To-Integrate] auf der Integrationsprozess Anwendung.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

