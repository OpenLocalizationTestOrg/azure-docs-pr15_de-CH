<properties
    pageTitle="Freigeben von Firmen Azure AD |  Microsoft Azure"
    description="Beschreibt, wie Active Directory Azure Unternehmen für lokalen apps und Consumer Cloud-Dienste sicher freigeben können."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Freigeben von Firmen für Azure AD

## <a name="overview"></a>Übersicht
Organisationen müssen manchmal für mehrere Personen einen einzelnen Benutzernamen und Kennwort verwenden. Dies geschieht in der Regel in zwei Fällen:

- Wenn Programme zugreifen, die einen eindeutigen Benutzernamen und ein Kennwort für jeden Benutzer erforderlich, ob lokalen apps oder Consumer cloud-Services (z. B. corporate sozialen Netzwerken).
- Bei Mehrbenutzer-Umgebung erstellen. Sie müssen ein lokales Konto, das erweiterten Berechtigungen verfügt und Installation, Verwaltung und Wiederherstellung von Aktivitäten (z. B. die lokale "globaler Administrator" für Office 365) Stammkonto oder in Salesforce Kern.

Normalerweise würde diese Konten durch die Anmeldeinformationen (Benutzername/Kennwort) an die richtigen Personen verteilen oder speichern sie an einem freigegebenen Speicherort, in dem mehrere vertrauenswürdige Agenten darauf zugreifen können, freigegeben werden.

Herkömmliche Freigabemodell hat mehrere Nachteile:

- Zugriff auf neue Anträge müssen Sie Anmeldeinformationen für alle Benutzer verteilen, die Zugriff benötigt.
- Jede freigegebene Anwendung erfordern seinen eigenen Satz von freigegebenen Anmeldeinformationen Benutzer mehrere Anmeldeinformationen speichern. Wenn Benutzer zu viele Anmeldeinformationen merken, erhöht sich das Risiko, dass sie risikoreiche Praktiken zurückgreifen. (z. B. Passwörter schreiben).
- Sie ist nicht erkennbar, die auf eine Anwendung zugreifen.
- Sie ist nicht erkennbar, wer *Zugriff auf* eine Anwendung hat.
- Wenn Sie Zugriff auf eine Anwendung entfernen müssen, müssen Sie die Anmeldeinformationen aktualisieren und Weitergabe für alle Benutzer, die Zugriff auf diese Anwendung.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory Firma freigeben

Azure AD bietet einen neuen Ansatz für freigegebene Konten, der diese Nachteile eliminiert.

Azure AD Administrator konfiguriert, welche Programme ein Benutzer zugreifen kann, indem der und Auswahl des besten für einzelne Zeichen für diese Anwendung geeignet. Eine solche *kennwortbasierte Single Sign-On*kann Azure AD fungieren als eine Art "Bank" bei der Anmeldung für die Anwendung.

Benutzer melden Sie sich mit ihrer Organisation Konto. Dies ist das Konto, das sie regelmäßig auf ihren Desktop oder e-Mail verwenden. Entdecken und Zugriff nur Programme, denen sie zugeordnet sind. Freigegebene Konten kann dieser Liste eine beliebige Anzahl gemeinsam verwendete Anmeldeinformationen enthalten. Die Endbenutzer müssen nicht merken oder notieren Sie sich die verschiedenen Konten, die sie verwenden können.

Freigegebene Konten nicht nur erhöhen Aufsicht und Verwendbarkeit, sie erhöhen auch die Sicherheit. Benutzer mit Berechtigungen zur Verwendung der Anmeldeinformationen sehe das gemeinsame Kennwort aber stattdessen erhalten Berechtigungen das Kennwort als Teil einer instrumentierten Authentifizierung verwenden. Weiter mit einigen Programmen Kennwort SSO-Sie können Azure Anzeige regelmäßig Rollover (Update) mit großen, komplexen Kennwörter kontosicherheit erhöht. Der Administrator kann mühelos gewähren oder Aufheben des Zugriffs auf eine Anwendung und wissen auch Zugriff auf das Konto und die in der Vergangenheit zugegriffen.

Azure AD unterstützt freigegebene Konten für jede Enterprise Mobility Suite (EMS) Premium oder Basic lizenzierte Benutzer über alle einzelnen Kennwort anmelden Applications. Sie können Konten für alle Tausende von integrierten Applikationen in der Galerie und können Ihre eigene Anwendung Kennwort authentifizieren mit [benutzerdefinierten SSO-apps](active-directory-sso-integrate-saas-apps.md).

Azure AD bietet Features für die gemeinsame Nutzung von Konten gehören:

- [Einmaliges Anmelden für Kennwort](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Kennwort anmelden Agenten
- [Zuordnung](active-directory-accessmanagement-self-service-group-management.md)
- Benutzerdefinierte Kennwort apps
- [App Dashboard/Verwendungsberichten](active-directory-passwords-get-insights.md)
- Endbenutzer Zugriff Portale
- [App-proxy](active-directory-application-proxy-get-started.md)
- [Active Directory-Markt](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Freigeben eines Kontos
Azure AD verwenden ein Konto freigeben zu müssen:

- Hinzufügen einer Anwendung [Galerie](https://azure.microsoft.com/marketplace/active-directory/) oder [benutzerdefinierte Anwendung](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
- Konfigurieren der Anwendung für einmaliges Anmelden (SSO) Kennwort
- [Arbeitsgruppen-Zuordnung](active-directory-accessmanagement-group-saasapps.md) verwenden und die Option freigegebene Anmeldeinformationen eingeben
- Optional: in einigen Programmen wie LinkedIn, Facebook und Twitter können Sie die Option für [Automatische Azure AD-Kennwort](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx) aktivieren

Sie können auch sicherer gemeinsam genutzter Konten mit mehrstufige Authentifizierung (MFA) (Weitere Informationen zu [Sicherung von Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) und verwalten Zugriff auf die Anwendung [Azure AD Self-Service-](active-directory-accessmanagement-self-service-group-management.md) Verwaltung delegieren.

## <a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Apps mit bedingten Zugriff schützen](active-directory-conditional-access.md)
- [Gruppe Self-service-Management/SSAA](active-directory-accessmanagement-self-service-group-management.md)
