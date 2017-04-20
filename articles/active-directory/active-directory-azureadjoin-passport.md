<properties
    pageTitle="Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport | Microsoft Azure"
    description="Bietet eine Übersicht über Microsoft Passport und Weitere Informationen zum Bereitstellen von Microsoft Passport."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="authenticating-identities-without-passwords-through-microsoft-passport"></a>Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport

Die aktuellen Methoden der Authentifizierung Kennwörter allein sind nicht ausreichend Anwender zu schützen. Benutzer und Kennwörter vergessen wiederverwenden. Kennwörter sind breachable, Phishable, anfällig für Risse und erraten. Sie erhalten auch schwer zu merken und anfällig für Angriffe, wie "[den Hash übergeben](https://technet.microsoft.com/dn785092.aspx)".

## <a name="about-microsoft-passport"></a>Informationen zu Microsoft Passport
Microsoft Passport ist ein Private/öffentliche Schlüssel oder zertifikatbasierte Authentifizierung Ansatz für Unternehmen und Verbraucher, die Kennwörter hinausgeht. Diese Art der Authentifizierung abhängig Schlüsselpaar Anmeldeinformationen, die Ersetzen von Kennwörtern und Verstöße gegen Diebstahl und Phishing sind.

 Microsoft Passport können Benutzer authentifizieren, um ein Microsoft-Konto, ein Windows Server Active Directory-Konto, ein Konto Microsoft Azure Active Directory (Azure AD) oder ein nicht-Microsoft-Dienst, der schnell Identität Online (FIDO) Authentifizierung unterstützt. Nach einer anfänglichen zweistufigen Überprüfung während der Microsoft Passport-Registrierung Microsoft Passport ist auf dem Gerät des Benutzers eingerichtet und Benutzersätze Geste Windows Hello oder eine PIN. Der Benutzer gibt die Bewegung um ihre Identität zu überprüfen. Windows verwendet dann Microsoft Passport zum Authentifizieren des Benutzers und Zugriff auf geschützte Ressourcen und Services.

Der private Schlüssel stehen nur durch eine "Benutzeraktion" PIN, Biometrik oder ein entferntes Gerät wie einer Smartcard, die der Benutzer verwendet das Gerät anmelden. Diese Information ist ein asymmetrisches Schlüsselpaar und ein Zertifikat verknüpft. Der private Schlüssel ist bestätigt, wenn das Gerät einen Trusted Platform Module (TPM) Chip Hardware. Der private Schlüssel bleibt immer das Gerät.

Der öffentliche Schlüssel ist in Azure Active Directory und Windows Server Active Directory (für lokal) registriert. Identitätsprovider (IDP) durch Zuordnen des öffentlichen Schlüssels des Benutzers auf den privaten Schlüssel den Benutzer überprüfen und Informationen-in einer Time Password (OTP), PhoneFactor oder anderen Benachrichtigungsmechanismus.

## <a name="why-enterprises-should-adopt-microsoft-passport"></a>Warum sollten Unternehmen Microsoft Passport verwenden

Aktivieren Sie Microsoft Passport, können Unternehmen ihre Ressourcen noch durch sichern:

* Einrichten von Microsoft Passport mit einer Option Hardware bevorzugt. Dies bedeutet, dass TPM 1.2 oder TPM 2.0 verfügbar werden Schlüssel generiert. Wenn TPM nicht verfügbar ist, wird den Schlüssel generieren.

* Definieren von Komplexität und Länge der PIN und ob Hello Verwendung in Ihrer Organisation aktiviert ist.

* Konfigurieren von Microsoft Passport unterstützt Smartcard-ähnliche Szenarios mit zertifikatbasierte Vertrauensstellung.

## <a name="how-microsoft-passport-works"></a>Funktionsweise von Microsoft Passport
1. Schlüssel werden auf der Hardware, Software oder TPM erzeugt. Viele Geräte haben integrierten TPM-Chip, der die Hardware Geräte integriert Kryptografieschlüssel sichert. TPM 1.2 oder 2.0 TPM generiert Schlüssel oder Zertifikate, die generierten Schlüssel erstellt werden.

2. Das TPM bestätigt diese Schlüssel Hardware gebunden.

3. Eine einzelne Sperre Bewegung wird das Gerät entsperrt. Dieser Bewegung ermöglicht den Zugriff auf mehrere Ressourcen ist das Gerät Domäne oder Azure AD verknüpft.

## <a name="how-the-microsoft-passport-lifecycle-works"></a>Funktionsweise des Microsoft Passport-Lebenszyklus

![Microsoft Passport-Lebenszyklus](./media/active-directory-azureadjoin/active-directory-azureadjoin-microsoft-passport.png)

Im obige Diagramm veranschaulicht das öffentlich-Private Schlüsselpaar und Validierung vom Identitätsanbieter. Diese Schritte werden hier erklärt:

1. Der Benutzer weist ihre Identität durch mehrere integrierte Rechtschreibprüfung Methoden (Gesten, physische Smartcards, mehrstufige Authentifizierung) und sendet diese Informationen eine Identität Anbieter (IDP) wie Azure Active Directory oder lokale Active Directory.

2. Das Gerät dann erstellt den Schlüssel bestätigt den Schlüssel, der öffentliche Teil des Schlüssels, mit Station verbunden, anmeldet und den Schlüssel registrieren IDP übermittelt.

4. Als Identitätsanbieter den öffentlichen Teil des Schlüssels registriert, fordert die IDP Gerät sich mit privaten Teil des Schlüssels.

5. Die dann validiert und stellt dem Authentifizierungstoken, mit dem Benutzer und dem Gerät kann die geschützten Ressourcen. Vertriebenen können plattformübergreifende apps schreiben oder Browserunterstützung (über JavaScript-Webcrypto-APIs) erstellen und Verwenden von Microsoft Passport-Anmeldeinformationen für die Benutzer verwenden.

## <a name="the-deployment-requirements-for-microsoft-passport"></a>Bereitstellung an Microsoft Passport
### <a name="at-the-enterprise-level"></a>Auf Unternehmensebene

* Das Unternehmen hat ein Azure-Abonnement.

### <a name="at-the-user-level"></a>Auf Benutzerebene

* Der Benutzer Computer führt Windows 10 Professional oder Enterprise.

Detaillierte Anweisungen finden Sie unter [Aktivieren von Microsoft Passport für die Arbeit in der Organisation](active-directory-azureadjoin-passport-deployment.md).


## <a name="additional-information"></a>Weitere Informationen

* [Windows 10 für Unternehmen: Verwendungsmöglichkeiten für Geräte für die Arbeit](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten](active-directory-azureadjoin-user-upgrade.md)
* [Informationen Sie zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Geräte Domäne Azure AD für Windows 10 Erlebnisse](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)
