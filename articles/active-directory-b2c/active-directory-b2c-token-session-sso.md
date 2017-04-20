<properties
    pageTitle="Azure Active Directory B2C: Token Sitzung und Konfiguration für einzelne Zeichen | Microsoft Azure"
    description="Token, Sitzung und einzelne anmelden Konfiguration in Azure Active Directory B2C"
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

# <a name="azure-active-directory-b2c-token-session-and-single-sign-on-configuration"></a>Azure Active Directory B2C: Token Sitzung und Konfiguration für einzelne Zeichen

Diese Funktion bietet Ihnen detaillierte Kontrolle auf [pro-Policy-Basis](active-directory-b2c-reference-policies.md)von:
 
1. Lebensdauer der Sicherheitstoken von Azure Active Directory (Azure AD) B2C ausgegeben.
2. Lebensdauer der Web-Anwendung Sessions von Azure AD B2C verwaltet.
3. Einmaliges Anmelden (SSO) Verhalten über mehrere apps und Richtlinien in Ihrem B2C-Mandanten.

Sie können diese Funktion in Ihrem Mandanten B2C folgendermaßen verwenden:

1. Gehen Sie [zu B2C-Features Blade](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) navigieren Azure-Portal.
2. Klicken Sie auf **Anmelden**. *Hinweis: können Sie diese Funktion auf beliebige Richtlinie nicht nur auf* *-In Richtlinien***.
3. Öffnen Sie eine Richtlinie durch Mausklick. Klicken Sie beispielsweise auf **B2C_1_SiIn**.
4. Klicken Sie am oberen Rand der Blade **Bearbeiten** .
5. Klicken Sie auf **Token, Sitzung und Konfiguration für einzelne Zeichen**.
6. Ändern Sie gewünschte. Enthält Informationen Sie zu verfügbaren Eigenschaften in den folgenden Abschnitten.
7. Klicken Sie auf **OK**.
8. **Klicken Sie auf das Blatt.**

![Screenshot der Token, Sitzung und Konfiguration für einzelne Zeichen](./media/active-directory-b2c-token-session-sso/token-session-sso.png)

## <a name="token-lifetimes-configuration"></a>Token Lebensdauer Konfiguration

Azure AD B2C unterstützt das [OAuth 2.0 Authorization Protocol](active-directory-b2c-reference-protocols.md) für sicheren Zugriff auf geschützte Ressourcen ermöglichen. Um diese Unterstützung zu implementieren, gibt Azure AD B2C verschiedene [Sicherheitstoken](active-directory-b2c-reference-tokens.md). Dies sind die Eigenschaften, die zum Verwalten der Lebensdauer von Sicherheitstoken von Azure AD B2C ausgegeben:

- **Access & ID-token Gültigkeitsdauer (Minuten)**: die Gültigkeitsdauer des Tokens Träger OAuth 2.0 verwendet auf eine geschützte Ressource zugreifen. Azure AD B2C gibt ID-Token zu diesem Zeitpunkt. Dieser Wert gilt für Zugangs-Token, wenn wir sie unterstützen.
   - Standard = 60 Minuten.
   - Minimum (inklusive) = 5 Minuten.
   - Maximum (inklusive) = 1440 Minuten.
- **Aktualisierung Tokengültigkeitsdauer (Tage)**: der maximale Zeitraum vor dem Aktualisierungstoken kann einen neuen Zugriff oder Token-ID verwendet (und gegebenenfalls neue aktualisieren, wenn die Anwendung gewährt wurde die `offline_access` Bereich).
   - Standard = 14 Tage.
   - Minimum (inklusive) = 1 Tag.
   - Maximum (inklusive) = 90 Tage.
- **Aktualisierungstoken gleitende Fenster Gültigkeitsdauer (Tage)**: nach dieser Zeit, den Benutzer verstreicht muss erneut authentifizieren, unabhängig von der Gültigkeitsdauer der letzten Aktualisieren Token von der Anwendung abgerufen. Es kann nur verfügbar, wenn der Schalter auf **Bounded**festgelegt ist. Es muss größer oder gleich der **Aktualisieren Tokengültigkeitsdauer (Tage)** Wert. Wenn der Schalter auf **unbegrenzt**kann keinen bestimmten Wert angeben.
   - Standard = 90 Tage.
   - Minimum (inklusive) = 1 Tag.
   - Maximum (inklusive) = 365 Tage.

Dies sind einige der Anwendungsfälle können mit diesen Eigenschaften:

- Lassen Sie Benutzer, die in einer mobilen Anwendung angemeldet bleiben zu, solange die Anwendung ständig aktiv ist Sie können dazu die **Aktualisieren gleitende Fenster Tokengültigkeitsdauer (Tage)** wechseln Sie in die Richtlinie **unbegrenzt** .
- Erfüllen der Branche Sicherheits- und Compliance-Vorschriften entsprechenden token Gültigkeitsdauer festlegen.

## <a name="session-configuration"></a>Sitzungskonfiguration

Azure AD B2C unterstützt das [Authentifizierungsprotokoll OpenID Verbindung](active-directory-b2c-reference-oidc.md) zum Aktivieren der sicheren Anmeldung in ASP.NET-Webanwendungen. Dies sind die Eigenschaften, die Web-Anwendung Sessions verwalten:

- **WebApp Gültigkeitsdauer (Minuten)**: die Lebensdauer von Azure AD B2C Sitzungscookie im Browser nach erfolgreicher Authentifizierung des Benutzers gespeichert.
   - Standard = 1440 Minuten.
   - Minimum (inklusive) = 15 Minuten.
   - Maximum (inklusive) = 1440 Minuten.
- **Web app Sitzungstimeout**: Wenn dieser Schalter auf **Absolute**festgelegt ist, wird der Benutzer gezwungen, nach von **WebApp Gültigkeitsdauer (Minuten)** verstrichen ist angegebenen Zeitraums erneut authentifizieren. Wenn dieser Schalter auf **Fahrzeuge** (Standardeinstellung) festgelegt ist, bleibt der Benutzer angemeldet als Benutzer in der Webanwendung ständig aktiv ist.

Dies sind einige der Anwendungsfälle können mit diesen Eigenschaften:

- Anforderungen der Branche Sicherheit und Compliance durch Festlegen der entsprechenden anwendungssitzung Lebensdauer.
- Erneute Authentifizierung nach einem festgelegten Zeitraum während der Interaktion des Benutzers mit hoher Sicherheit Teil der Webanwendung zu erzwingen. 

## <a name="single-sign-on-sso-configuration"></a>Einmaliges Anmelden (SSO) Konfiguration

Haben Sie mehrere Programme und Richtlinien in Ihrem Mandanten B2C, können Sie Sie mit der **Konfiguration für einzelne Zeichen** -Eigenschaft Benutzerinteraktionen verwalten. Sie können die Eigenschaft auf eine der folgenden Optionen festlegen:

- **Mieter**: Dies ist die Standardeinstellung. Mit dieser Einstellung kann mehrere Programme und Richtlinien in Ihrem Mandanten B2C derselben benutzersitzung freigeben. Z. B. wenn ein Benutzer in einer Anwendung anmeldet, kann Contoso einkaufen, er oder sie nahtlos in anderen einem Contoso-Apotheke nach Zugriff signieren.
- **Anwendung**: ermöglicht einem Benutzer Sitzung ausschließlich für eine Anwendung unabhängig von anderen Programmen. Beispielsweise soll den Benutzer bei Contoso Pharma (mit den gleichen Anmeldeinformationen) anmelden, wenn er in Contoso angemeldet ist, eine andere Anwendung auf demselben B2C Mieter. 
- **Richtlinie**: ermöglicht einem Benutzer Sitzung ausschließlich für eine Richtlinie unabhängig von der Anwendung verwenden. Z. B. wenn der Benutzer bereits angemeldet und einen Multi Faktor-Authentifizierung (MFA) Schritt abgeschlossen, er Zugriff auf höhere Sicherheit Teil mehrerer Applikationen erhalten als die Sitzung gebunden, die Richtlinie nicht ablaufen.
- **Deaktiviert**: Dies zwingt den Benutzer, auf die Benutzer bei jeder Ausführung der Richtlinie ausführen. Beispielsweise können mehrere Benutzer für die Anwendung (in einem freigegebenen desktop Szenario), während ein Benutzer bleibt angemeldet während der ganzen Zeit.
