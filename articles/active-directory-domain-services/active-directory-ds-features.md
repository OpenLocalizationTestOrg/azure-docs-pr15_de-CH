<properties
    pageTitle="Azure Active Directory Domain Services: Funktionen | Microsoft Azure"
    description="Funktionen von Azure Active Directory-Domänendienste"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure Active Directory-Domänendienste

## <a name="features"></a>Funktionen
Die folgenden Features sind in Azure AD-Domänendienste verwalteten Domänen.

- **Einfache Bereitstellung:** Sie können Azure AD-Domänendienste für Ihren Azure AD-Mandanten mit nur wenigen Mausklicks. Azure AD-Mandanten ist ein Cloud-Mieter oder mit Ihrem lokalen Verzeichnis synchronisiert kann schnell Ihre verwalteten Domäne bereitgestellt werden.

- **Unterstützung für Domäne beitreten:** Sie können Computer in Azure virtual Network ist der verwalteten Domäne für Domäne beitreten. Windows-Client und Server-Betriebssysteme arbeiten nahtlos mit Domänen vom Azure Active Directory-Domänendienste-Domäne beitreten-Erlebnis. Sie können auch automatisierte Tools gegen solche Domänen Domänenbeitritt.

- **Einer Domäneninstanz pro Azure AD-Verzeichnis:** Sie können eine einzelne Active Directory-Domäne für jede Azure AD-Verzeichnis erstellen.

- **Domänen mit benutzerdefinierten Namen erstellen:** Domänen mit benutzerdefinierten Namen (z. B. ' contoso100.com') Azure Active Directory-Domänendienste verwenden können. Sie können überprüft oder nicht überprüften Domänennamen verwenden. Optional können Sie eine Domäne mit dem integrierten Domänensuffix erstellen (d. h. ' *. onmicrosoft.com ") von Azure AD-Verzeichnis bereitgestellt.

- **In Azure AD integriert:** Sie müssen nicht konfigurieren oder Verwalten von Azure Active Directory-Domänendienste-Replikation. Benutzerkonten, Gruppenmitgliedschaften und Benutzeranmeldeinformationen (Kennwörter) aus dem Azure AD-Verzeichnis sind automatisch in Azure AD-Domänendienste verfügbar. Neue Benutzer, Gruppen oder Ändern von Attributen von Azure AD-Mandanten oder Ihrem lokalen Verzeichnis werden in Azure AD-Domänendienste automatisch synchronisiert.

- **NTLM und Kerberos-Authentifizierung:** Mit Unterstützung für NTLM und Kerberos-Authentifizierung können Sie Programme bereitstellen, die integrierte Windows-Authentifizierung benötigen.

- **Unternehmen Anmeldeinformationen/Kennwörter verwenden:** Kennwörter für Benutzer in Azure AD-Mandanten arbeiten mit Azure Active Directory-Domänendienste. Benutzer können ihre corporate Anmeldeinformationen Domänenbeitritt Maschinen interaktiv oder über Remotedesktop anmelden und verwalteten Domäne authentifizieren.

- **Lesen LDAP-Bindung und LDAP-Unterstützung:** Die können Clientanwendungen auf LDAP-Bindung zum Authentifizieren von Benutzern in Domänen von Azure Active Directory-Domänendienste bedient. Darüber hinaus funktioniert, Lesevorgänge LDAP-Abfrage Benutzer/Computer Attribute aus dem Verzeichnis verwenden, auch gegen Azure Active Directory-Domänendienste.

- **Sicheres LDAP (LDAPS):** Sie können Zugriff auf das Verzeichnis über sicheres LDAP (LDAPS) aktivieren. LDAP-Zugriff ist innerhalb des virtuellen Netzwerks standardmäßig verfügbar. Allerdings können Sie optional auch LDAP-Zugriff über das Internet aktivieren.

- **Gruppenrichtlinien:** Können Sie für Benutzer und Computer ein integriertes Gruppenrichtlinienobjekt jeder Container Einhaltung erzwingen Sicherheitsrichtlinien für Benutzerkonten und Domänencomputern erforderlich.

- **Verwaltung von DNS:** Mitglieder der Gruppe 'AAD DC Administrators' können DNS für Ihre verwalteten Domäne mit bekannten DNS-Administrationstools wie DNS-Administration-MMC-Snap-in verwalten.

- **Erstellen benutzerdefinierte Organisationseinheiten (OUs):** Mitglieder der Gruppe 'AAD DC Administrators' können benutzerdefinierte Organisationseinheiten in der verwalteten Domäne erstellen. Diese Benutzer werden vollständige Administratorrechte benutzerdefinierten Organisationseinheiten erteilt, damit sie Software können Dienstkonten, Computer, Gruppen innerhalb dieser benutzerdefinierten Organisationseinheiten.

- **In mehreren Azure Regionen verfügbar:** Siehe [Azure Services nach Region](https://azure.microsoft.com/regions/#services/) kennen Azure Regionen, die Azure AD-Domänendienste verfügbar ist.

- **Hohe Verfügbarkeit:** Azure bietet AD-Domäne hohen Verfügbarkeit für Ihre Domäne. Diese Funktion bietet höhere Service-Verfügbarkeit und Stabilität auf Fehler. Integrierte Systemüberwachungsereignissen bietet automatische Behebung von Fehlern durch neue Instanzen fehlgeschlagenen ersetzen und weiteren Dienst für Domain einrichten.

- **Vertraute Verwaltungstools:** Vertraute Windows Server Active Directory-Verwaltungstools oder die Active Directory-Verwaltungscenter Active Directory PowerShell können verwalteten Domains verwalten.
