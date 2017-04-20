<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Wie Sie apps direkt mithilfe von Azure Active Directory B2C unterstützten Protokolle erstellen."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD B2C: Authentifizierungsprotokolle

Azure Active Directory (Azure AD) B2C bietet Identität als Service für Ihre apps unterstützt zwei Protokolle nach Industriestandard: OpenID verbinden und OAuth 2.0. Der Dienst ist standardkonform, aber alle zwei Implementierung dieser Protokolle können Unterschiede.  Die Informationen in diesem Handbuch werden nützlich, wenn Sie Ihren Code durch Senden und HTTP-Anfragen nicht mithilfe einer open-Source-Bibliothek schreiben. Es wird empfohlen, diese Seite lesen, bevor die Details jedes spezifische Protokoll eintauchen. Aber bereits kennen Azure AD B2C hingegen kann man direkt [Referenzhandbücher Protokoll](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Die Grundlagen
Jede Anwendung, die Azure AD B2C verwendet muss in Ihrem B2C-Verzeichnis im [Azure-Portal](https://portal.azure.com)registriert werden. App-Registrierung erfasst und weist einige Werte für Ihre Anwendung:

- **ID der Anwendung** , die Ihre Anwendung eindeutig.
- Ein **URI umleiten** oder **Paket-ID** , direkte Antworten auf Ihre app verwendet werden können.
- Einige andere Szenario Werte. Weitere Informationen Sie [zum Registrieren der Anwendung](active-directory-b2c-app-registration.md).

Nach dem Registrieren Ihrer Anwendung kommuniziert es mit Azure AD Senden von Anfragen an den Endpunkt v2. 0:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Fast alle OAuth und OpenID verbinden Gelder sind vier Exchange beteiligt:

![OAuth 2.0-Funktionen](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- **Autorisierungsserver** ist der Azure AD v2. 0-Endpunkt. Es behandelt sicher was Benutzerinformationen und Zugriff. Es behandelt auch die Vertrauensstellungen zwischen den Parteien in einem Fluss. Es ist die Identität des Benutzers überprüft, erteilen und Widerrufen des Zugriffs auf Ressourcen und Token ausstellen. Es ist auch bekannt als Identitätsanbieter.
- **Ressourcenbesitzer** ist normalerweise der Endbenutzer. Es ist, die die Daten besitzt und hat die macht, dritte Zugriff auf die Daten oder Ressourcen.
- **OAuth-Client** ist Ihre app. Es wird durch seine Anwendung ID identifiziert. Es ist normalerweise die Partei, der Benutzer mit interagieren. Sie fordert auch Token vom autorisierungsserver. Besitzer der Ressource muss die Clientberechtigung Zugriff auf die Ressource gewähren.
- Der **Ressourcenserver** befindet sich die Ressource oder die Daten. Er vertraut den autorisierungsserver sicher authentifiziert und autorisiert des OAuth-Clients. Auch wird Träger Zugriffstoken verwendet, um sicherzustellen, dass Zugriff auf eine Ressource gewährt werden kann.

## <a name="policies"></a>Richtlinien
Azure AD B2C-Richtlinien sind wohl wichtigsten Features des Diensts. Azure AD B2C erweitert die Standardprotokolle OAuth 2.0 und OpenID verbinden durch Maßnahmen. Azure AD B2C mehr als einfache Authentifizierung und Autorisierung durchführen können. Richtlinien beschreiben vollständig Consumer Identität Erfahrungen, einschließlich Anmeldung, anmelden und Profil bearbeiten. Richtlinien können eine administrative Benutzeroberfläche definiert. Sie können mithilfe von speziellen Abfrageparameter in HTTP-Authentifizierung fordert ausgeführt werden. Richtlinien sind nicht von OAuth 2.0 und OpenID, so sollten Sie die Zeit zu verstehen. Weitere Informationen finden Sie im [Referenzhandbuch für Azure AD B2C-Richtlinie](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Token
Azure AD B2C-Implementierung von OAuth 2.0 und OpenID nutzt trägertoken einschließlich trägertoken als JSON Web Token (JWTs) dargestellt werden. Ein trägertoken ist ein einfaches Sicherheitstoken, den "Inhaber" den Zugriff auf eine geschützte Ressource gewährt. Der Inhaber ist eine Partei, die das Token darstellen kann. Azure AD muss zuerst eine Partei authentifizieren, bevor ein trägertoken erhalten. Wenn die erforderlichen Schritte zum Sichern des Tokens in der Übertragung und Speicherung nicht stammen, kann jedoch werden abgefangen und durch einen unerwünschten Dritten verwendet.

Einige Sicherheitstoken haben eine integrierte Methode, die verhindert, dass unbefugte Verwendung jedoch trägertoken verfügen über diesen Mechanismus. Sie müssen in einem sicheren Kanal wie Transport Layer Security (HTTPS) übertragen. Bei der Übertragung eines Träger Tokens außerhalb eines sicheren Kanals können böswillige Dritte einen Man-in-the-Middle-Angriff das Token und Zugriff auf eine geschützte Ressource zu verwenden. Sicherheitsgrundsätze gelten beim trägertoken gespeichert oder für die spätere Verwendung zwischengespeichert. Immer sicherstellen Sie, dass Ihre app überträgt und trägertoken sicher speichert.

Weitere Träger token Sicherheitsaspekte finden Sie unter [RFC 6750 Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Weitere Informationen über die verschiedenen Arten von Azure AD B2C verwendeten Token stehen in [Azure AD Tokenverweis](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protokolle

Wenn Sie einige Beispiel-Anfragen überprüfen möchten, beginnen Sie mit den folgenden Tutorials. Jeweils entspricht ein bestimmtes Szenario. Benötigen Sie Hilfe für Sie läuft, überprüfen Sie, [welche apps erstellen können mithilfe von Azure AD B2C](active-directory-b2c-apps.md).

- [Mobile und systemeigene Anwendungsentwicklung mit OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
- [Erstellen von webapps mit OpenID verbinden](active-directory-b2c-reference-oidc.md)
