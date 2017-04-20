<properties
    pageTitle="Azure Active Directory B2C: Grenzen und Einschränkungen | Microsoft Azure"
    description="Eine Liste der Einschränkungen mit Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-limitations-and-restrictions"></a>Azure Active Directory B2C: Grenzen und Einschränkungen

Es gibt verschiedene Features und Funktionen von Azure Active Directory (Azure AD) B2C noch nicht unterstützt. Viele bekannte Probleme und Grenzen werden vorwärts gerichtet werden, aber beachten Sie diese kundenorientierter Applikationen mit Azure AD B2C erstellen.

## <a name="issues-during-the-creation-of-azure-ad-b2c-tenants"></a>Probleme bei der Erstellung von Azure AD B2C Mieter

Wenn Probleme bei der [Erstellung einer Azure AD B2C Mieter](active-directory-b2c-get-started.md)auftreten, finden Sie unter [Erstellen einer Azure AD-Mandanten oder einen Mandanten Azure AD B2C - Probleme und deren Lösung](active-directory-b2c-support-create-directory.md) Anleitung.

Hinweis es bekannte Probleme bei einen vorhandenen B2C-Mandanten löschen und erneut mit dem gleichen Domänennamen erstellen. Sie haben einen B2C-Mandanten mit einem anderen Domänennamen erstellen.

## <a name="note-about-b2c-tenant-quotas"></a>Hinweis zum B2C Mieter Kontingente

Standardmäßig ist die Anzahl der Benutzer in einem B2C-Mandanten auf 50.000 Benutzer beschränkt. Möchten Sie die von Ihrem Mandanten B2C erhöhen, sollten Sie Support.

## <a name="branding-issues-on-verification-email"></a>Branding-Probleme auf e-Mail

Standardmäßige Überprüfung e-Mail enthält Microsoft branding. Es wird später entfernt. Jetzt können Sie es mithilfe des [Unternehmens branding Funktion](../active-directory/active-directory-add-company-branding.md)entfernen.

## <a name="restrictions-on-applications"></a>Beschränkungen Applikationen

Folgende Programme sind in Azure AD B2C derzeit nicht unterstützt. Finden Sie eine Beschreibung der unterstützten Anwendungstypen [Azure AD B2C: Anwendungstypen](active-directory-b2c-apps.md).

### <a name="single-page-applications-javascript"></a>Single Page Applications (JavaScript)

Viele moderne Clientanwendungen eine einzelne Seite Anwendung (SPA) Front-End, die hauptsächlich in JavaScript geschrieben und oft verwendet ein SPA Framework wie AngularJS, Ember.js, Durandal. Dieser Fluss ist noch nicht in Azure AD B2C verfügbar.

### <a name="daemons--server-side-applications"></a>Daemons / serverseitige Anwendung

Applikationen, lang andauernde Prozesse bzw. ohne Benutzer bearbeitet werden, benötigen auch gesicherte Ressourcen wie Web-APIs zugreifen. Diese Programme können authentifizieren und Token abrufen, indem der Anwendungsidentität (anstelle des Consumers delegierte Identität) [OAuth 2.0 Client Anmeldeinformationen Fluss](active-directory-b2c-reference-protocols.md#oauth2-client-credentials-grant-flow). Dieser Fluss ist noch nicht verfügbar in Azure AD B2C, so dass jetzt Applications Token erst ein interaktiver-Fluss aufgetreten ist.

### <a name="standalone-web-apis"></a>Eigenständige Web-APIs

In Azure AD B2C können Sie zum [Erstellen einer Web-API, die mithilfe von OAuth 2.0 Token gesichert ist](active-directory-b2c-apps.md#web-apis). Jedoch werden, Web-API nur Token von einem Client empfangen, die gemeinsam die gleiche Anwendung-ID. Erstellen eine Web-API, die von unterschiedlichen Clients zugegriffen wird nicht unterstützt.

### <a name="web-api-chains-on-behalf-of"></a>Web-API Ketten (im Auftrag von)

Viele Architekturen enthalten eine Web-API, die anderen downstream Web API sowohl von Azure AD B2C gesichert aufrufen. Dieses Szenario wird häufig in systemeigenen Clients, die eine Web-API-Back-End ein Microsoft online Service wie Azure AD Graph-API ruft.

Diesem Web API verketteten kann mit OAuth 2.0 Jwt Träger Anmeldeinformationen gewähren, bekannt als Fluss auf Auftrag von unterstützt werden. Jedoch ist auf Namen der Fluss nicht in Azure AD B2C implementiert.

## <a name="restriction-on-libraries-and-sdks"></a>Beschränkung auf Bibliotheken und SDKs

Die Gruppe von Microsoft unterstützten Bibliotheken, die Azure AD B2C arbeiten ist zurzeit eingeschränkt. Wir bieten Unterstützung für .NET Framework-basierte webapps und Dienste sowie NodeJS webapps.  Wir haben auch eine Vorschau .NET Clientbibliothek mit Azure AD B2C in Windows und anderen .NET apps MSAL genannt.

Wir noch keine Bibliothek anderen Sprachen oder Plattformen iOS und Android unterstützt haben.  Wenn Sie auf einer anderen Plattform als genannten erstellen möchten, empfehlen wir ein Open-Source-SDK auf unsere [OAuth 2.0 und OpenID verbinden Protokoll](active-directory-b2c-reference-protocols.md) .  Azure AD B2C implementiert OAuth & OpenID verbinden, macht es einen generischen OAuth oder OpenID verbinden Integration nutzen.

Unsere iOS & Android Schnellstart-Lernprogramme verwenden für Kompatibilität mit Azure AD B2C getestet haben Open-Source-Bibliotheken.  Alle Schnellstart-Lernprogramme stehen im Bereich [Erste Schritte](active-directory-b2c-overview.md#getting-started) .

## <a name="restriction-on-protocols"></a>Beschränkung auf Protokolle

Azure AD B2C unterstützt OpenID verbinden und OAuth 2.0. Jedoch nicht alle Features und Funktionen der einzelnen Protokolle implementiert wurde. Zum besseren Verständnis der unterstützt Funktionen in Azure AD B2C [OpenID und OAuth 2.0 Protokoll Referenz](active-directory-b2c-reference-protocols.md)lesen. Unterstützung von SAML und WS-Fed Protokoll ist nicht verfügbar.

## <a name="restriction-on-tokens"></a>Beschränkung auf tokens

Viele der von Azure AD B2C ausgestellten Token werden als JSON Web Token oder JWTs implementiert. Nicht alle Informationen in JWTs (als "Ansprüche" bezeichnet) ist jedoch ganz wie es oder fehlt. Beispiele sind "Sub" und die "Preferred_username".  Werte, Format oder Bedeutung der Ansprüche Änderung im Zeitverlauf Token für die Richtlinien unberührt - ihre Werte in Produktion apps abhängig.  Werte zu ändern, geben wir Ihnen die Möglichkeit, diese Änderungen aller Richtlinien konfigurieren.  Lesen Sie zum besseren Verständnis von Azure AD B2C-Dienst derzeit ausgegebenen Tokens [Tokenverweis](active-directory-b2c-reference-tokens.md).

## <a name="restriction-on-nested-groups"></a>Einschränkung für verschachtelte Gruppen

Verschachtelte Gruppenmitgliedschaften werden in Azure AD B2C Mieter nicht unterstützt. Wir möchten diese Funktion hinzuzufügen.

## <a name="restriction-on-differential-query-feature-on-azure-ad-graph-api"></a>Einschränkung für differenzielle Abfragefunktion Azure AD Graph-API

[Differenzielle Abfragefunktion Azure AD Graph-API](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-differential-query) wird in Azure AD B2C Mieter nicht unterstützt. Dies ist langfristig geplant.

## <a name="issues-with-user-management-on-the-azure-classic-portal"></a>Probleme mit der Verwaltung der klassischen Azure-Portal

B2C-Features werden der Azure-Portal zugegriffen. Klassische Azure-Portal können Sie jedoch andere Mieter Funktionen, einschließlich der Verwaltung. Derzeit gibt es ein paar Probleme mit Verwaltung (Registerkarte " **Benutzer** ") im klassischen Azure-Portal:

- Für lokales Benutzerkonto (d. h. ein Consumer, eine e-Mail-Adresse, Kennwort oder ein Benutzername und Kennwort anmeldet) entsprechen nicht **Feld** anmelden Bezeichners (e-Mail-Adresse oder Benutzername), die bei der Anmeldung verwendet wurde. Dies liegt das angezeigte Feld auf der Azure-Verwaltungsportal tatsächlich die Benutzer Principal Name (UPN), das B2C-Szenarien nicht verwendet wird. Zum Anzeigen des Bezeichners-in des lokalen Kontos finden Sie das Benutzerobjekt im [Graph-Explorer](https://graphexplorer.cloudapp.net/). Finden Sie das gleiche Problem mit sozialen Benutzerkonto (d. h. ein Verbraucher, Facebook, Google usw. anmeldet), aber in diesem Fall ist kein Bezeichner anmelden zu sprechen.

    ![Lokales Konto - UPN](./media/active-directory-b2c-limitations/limitations-user-mgmt.png)

- Bei einem lokalen Benutzerkonto wird nicht können keines der Felder auf der Registerkarte **Profil** speichern.

## <a name="issues-with-admin-initiated-password-reset-on-the-azure-classic-portal"></a>Probleme mit Admin initiierte Kennwort zurücksetzen auf klassischen Azure-portal

Zurücksetzen des Kennworts für ein Konto-basierte Verbraucher im klassischen Azure-Portal ( **Kennwort zurücksetzen** Befehl auf der Registerkarte **Benutzer** ) Consumer dürfen ihr Kennwort auf der nächsten Anmeldung ändern, wenn Zeichen verwenden oder Richtlinie anmelden, und aus der Anwendung. Um dieses Problem zu umgehen verwenden Sie die [Azure AD Graph-API](active-directory-b2c-devquickstarts-graph-dotnet.md) Passwort des Consumers (ohne Kennwortablauf) anmelden Richtlinie anstelle von Zeichen verwenden oder Richtlinie anmelden.

## <a name="issues-with-creating-a-custom-attribute"></a>Probleme beim Erstellen eines benutzerdefinierten Attributs

Ein [benutzerdefiniertes Attribut Azure-Portal hinzugefügt](active-directory-b2c-reference-custom-attr.md) wird sofort in Ihrem Mandanten B2C nicht erstellt. Sie müssen das benutzerdefinierte Attribut in mindestens einer der Richtlinien dafür in Ihrem B2C-Mandanten erstellt und über Graph-API verwenden.

## <a name="issues-with-verifying-a-domain-on-the-azure-classic-portal"></a>Probleme bei der Überprüfung einer Domäne klassischen Azure-portal

Derzeit kann keine Domäne in der [Azure-Verwaltungsportal](https://manage.windowsazure.com/)überprüfen.

## <a name="issues-with-sign-in-with-mfa-policy-on-safari-browsers"></a>Probleme beim Anmelden mit MFA Safari-Browser

Anfragen nicht zeitweise auf Safari-Browser HTTP 400 (Ungültige Anforderung) Fehler anmelden Richtlinien (mit MFA aktiviert). Diese liegt Safari Cookie niedrigen Grenzwert. Es gibt ein paar Workarounds für dieses Problem:

- Verwenden Sie "anmelden oder Einloggen Richtlinie" anstelle der "Anmelden".
- Reduzieren Sie die Anzahl von **Anwendungsansprüchen** in Ihrer Richtlinie angefordert wird.
