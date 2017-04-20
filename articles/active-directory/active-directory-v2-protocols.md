<properties
    pageTitle="Protokolle von Azure AD v2. 0 | Microsoft Azure"
    description="Ein Handbuch zu Protokollen von Azure AD v2. 0-Endpunkt."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20--openid-connect"></a>v2. 0 Protokolle - OAuth 2.0 & OpenID verbinden

V2. 0-Endpunkt können Azure AD Identity-as-a-Service mit branchenüblichen Protokollen, OpenID verbinden und OAuth 2.0.  Während der Dienst standardkonform, können Unterschiede zwischen den beiden Implementierungen dieser Protokolle werden.  Die Informationen kann nützlich sein, wenn Code schreiben senden möchten und Behandeln von HTTP-Anfragen oder verwenden eine 3rd Party Quellbibliothek anstelle eines open Source-Bibliotheken öffnen.
<!-- TODO: Need link to libraries above -->

> [AZURE.NOTE]
    Nicht alle Azure Active Directory Szenarien und Funktionen werden vom Endpunkt v2. 0 unterstützt.  Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Die Grundlagen
Fast alle OAuth & OpenID Connect Gelder gibt vier Exchange:

![OAuth 2.0-Funktionen](../media/active-directory-v2-flows/protocols_roles.png)

- **Autorisierungsserver** ist der Endpunkt v2. 0.  Es ist sicherzustellen, dass der Benutzer Identität, erteilen und Widerrufen des Zugriffs auf Ressourcen und Token ausstellen.  Wird auch als Identitätsanbieter - sicher behandelt mit Informationen des Benutzers den Zugriff und Vertrauensstellungen zwischen einen.
- **Ressourcenbesitzer** ist normalerweise der Endbenutzer.  Es ist befugt, dritte Zugriff auf die Daten oder Ressourcen, das die Daten besitzt.
- Der **OAuth-Client** ist Ihre app, anhand dessen ID Anwendung  Ist in der Regel die Partei, der der Endbenutzer interagiert und Token vom autorisierungsserver angefordert.  Der Client muss Zugriff auf die Ressource Besitzer der Ressource erteilt.
- Der **Ressourcenserver** befindet sich die Ressource oder die Daten.  Die Autorisierungsserver sicher authentifiziert und autorisiert den OAuth Client vertraut und Träger Access_tokens sichergestellt wird, dass Zugriff auf eine Ressource gewährt werden kann.


## <a name="app-registration"></a>App-Registrierung
Jede Anwendung, die v2. 0-Endpunkt verwendet müssen bei [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) registriert werden, bevor sie interagieren mit OAuth oder OpenID verbinden.  App-Registrierung Abhol- und Ihre app einige Werte zuweisen:

- **Id der Anwendung** , die Ihre Anwendung eindeutig
- Ein **URI umleiten** oder **Paket-ID** , die verwendet werden können, Antworten auf Ihre app
- Einige andere Szenario Werte.

Ausführliche Informationen zum [Registrieren einer Anwendung](active-directory-v2-app-registration.md).

## <a name="endpoints"></a>Endpunkte
Nach der Registrierung kommuniziert Senden von Anfragen an v2. 0-Endpunkt die app mit Azure:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Wo die `{tenant}` kann einen von vier verschiedene Werte annehmen:

| Wert | Beschreibung |
| ----------------------- | ------------------------------- |
| `common` | Ermöglicht Benutzern mit persönlichen Microsoft-Konten und Arbeit oder Schule Konten von Azure Active Directory die Anwendung anmelden. |
| `organizations` | Nur Benutzer mit Konten arbeiten Schule können von Azure Active Directory die Anwendung anmelden. |
| `consumers` | Ermöglicht nur Benutzern mit persönlichen Microsoft-Konten (MSA) der Anwendung anmelden. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`oder`contoso.onmicrosoft.com` | Können nur Benutzer mit Konten arbeiten Schule aus einer bestimmten Azure Active Directory Mandanten der Anwendung anmelden.  Die benutzerfreundlichen Domänennamen Azure AD-Mandanten oder des Mieters Guid-Bezeichner kann verwendet werden.  |

Weitere Informationen zur Interaktion dieser Endpunkte wählen Sie ein bestimmtes app aus

## <a name="tokens"></a>Token
2.0 Implementierung von OAuth 2.0 und OpenID nutzen trägertoken, einschließlich trägertoken als JWTs dargestellt. Ein trägertoken ist ein einfaches Sicherheitstoken, den "Inhaber" den Zugriff auf eine geschützte Ressource gewährt. In diesem Sinne ist der "" eine Partei, die das Token darstellen kann. Wenn eine Partei erforderlichen Schritte zum Sichern des Tokens in der Übertragung und Speicherung nicht getroffen sind zunächst mit Azure trägertoken zu authentifizieren muss können sie abgefangen und durch einen unerwünschten Dritten verwendet. Zwar einige Sicherheitstoken einen integrierten Mechanismus zum verhindern, dass unbefugte Verwendung trägertoken verfügen über diesen Mechanismus und einen sicheren Kanal wie Transport Layer Security (HTTPS) transportiert werden müssen. Ein trägertoken im Klartext übertragen, kann ein Man-in der mittleren Angriff verwendet werden bösartige Angriffe Token und für nicht autorisierten Zugriff auf eine geschützte Ressource verwenden. Sicherheitsgrundsätze gelten beim Speichern trägertoken zur späteren Verwendung zwischenspeichern. Immer sicherstellen Sie, dass Ihre app überträgt und trägertoken sicher speichert. Weitere Sicherheitsaspekte zu produzierende Token finden Sie in [RFC 6750 Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Weitere Details verschiedener Typen von Token verwendet den Endpunkt v2. 0 steht in [v2. 0 token Endpunktverweis](active-directory-v2-tokens.md).

## <a name="protocols"></a>Protokolle

Wenn Sie einige Beispiel-Anfragen zu sehen sind, fangen mit einem der folgenden Lernprogramme.  Jeweils entspricht ein bestimmtes Szenario.  Benötigen Sie Hilfe bei der Bestimmung der richtige für Sie ist, checken Sie [die Anwendungstypen erstellen mit der v2. 0](active-directory-v2-flows.md).

- [Mobile und systemeigene Anwendung OAuth 2.0 erstellen](active-directory-v2-protocols-oauth-code.md)
- [Erstellen von Web Apps mit Openid verbinden](active-directory-v2-protocols-oidc.md)
- [Entwickeln von Apps mit den impliziten OAuth 2.0 Seite](active-directory-v2-protocols-implicit.md)
- [Build-Daemons oder Seite Serverprozesse OAuth 2.0 Client Anmeldeinformationen übertragen](active-directory-v2-protocols-oauth-client-creds.md)
- Abrufen von Token in eine Web-API mit der OAuth 2.0 im Auftrag von Flow (demnächst verfügbar)

<!-- - Get tokens using a username & password with the OAuth 2.0 Resource Owner Password Credentials Flow (coming soon) --> 
