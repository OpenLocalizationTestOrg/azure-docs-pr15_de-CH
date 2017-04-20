<properties
    pageTitle="Azure AD Connect Version Gesundheitsdaten"
    description="Dieses Dokument beschreibt die Versionen für Azure AD Connect Health und was in diesen Versionen aufgenommen."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health: Versionsverlauf Version

Das Team Azure Active Directory aktualisiert Azure AD Connect Health mit neuen Features und Funktionen. Dieser Artikel führt die Versionen und Features, die veröffentlicht wurden.

## <a name="october-2016"></a>Oktober 2016
**Agent-Update:**
- Azure AD Connect Health Agent für AD FS \(Version 2.6.408.0\)
    1. Verbesserung der IP-Adressen im Authentifizierungsanfragen erkennen
    2. Fehlerkorrekturen Alarme beziehen
- Azure AD Connect Health Agent für AD DS (Version 2.6.408.0)
    1. Fehlerkorrekturen Alarme beziehen.
- Azure AD Connect Health Agent zur Synchronisierung (Version 2.6.353.0) mit Azure AD Connect Version 1.1.281.0 veröffentlicht
    1. Bereitstellen Sie die erforderlichen Daten für die Synchronisation Fehlerberichten
    2. Fehlerkorrekturen Alarme beziehen

**Neue Vorschaufunktionen:**
- Synchronisierung Fehlerberichte für Azure AD verbinden

**Neue Funktionen:**
- Azure AD Connect Health für AD FS - Feld IP-Adresse ist im Bericht über Top-50 Benutzer mit Benutzername/Kennwort ungültig.

## <a name="july-2016"></a>Juli 2016

**Neue Vorschaufunktionen:**

- [Azure AD Connect Health für AD DS](active-directory-aadconnect-health-adds.md).


## <a name="january-2016"></a>Januar 2016


**Agent-Update:**

- Azure AD Connect Health Agent für AD FS (Version 2.6.91.1512)


**Neue Funktionen:**

- [Testtool für Azure AD Konnektivität herstellen Health Agents](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)


## <a name="november-2015"></a>November 2015


**Neue Funktionen:**

- Unterstützung für [Rollenbasierte Zugriffskontrolle](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)


**Neue Vorschaufunktionen:**

- [Azure AD Connect Health für die Synchronisierung](active-directory-aadconnect-health-sync.md).

**Behobene Probleme:**

- Fehlerkorrekturen für Fehler beim Agent-Registrierung.

## <a name="september-2015"></a>September 2015

**Neue Funktionen:**

- Falscher Benutzername Kennwort Bericht für AD FS
- Unterstützung für nicht authentifizierte HTTP-Proxy konfigurieren
- Konfigurieren von Agenten auf Server Core unterstützt
- Verbesserte Alerts für AD FS
- Verbesserung der Azure AD verbinden Systemintegritäts-Agent für AD FS für Konnektivität und hochladen.


**Behobene Probleme:**

- Fehlerkorrekturen Verwendung Erkenntnisse für AD FS Fehlertypen.


## <a name="june-2015"></a>Juni 2015

**Erste Version von Azure AD Connect Health AD FS und AD FS-Proxy.**

**Neue Funktionen:**

- Alarme für AD FS und AD FS-Proxy Server mit e-Mail-Benachrichtigung.
- Einfacher Zugriff auf AD FS-Topologie und Muster in AD FS-Leistungsindikatoren.
- Entwicklung erfolgreicher token Anfragen auf AD FS-Servern Applikationen Authentifizierungsmethoden usw. anfordern Netzwerk Standort gruppiert.
- Trends im Anforderungsfehler auf AD FS-Servern Applikationen Fehler Typen usw. gruppiert.
- Einfacher Agent-Bereitstellung mit Azure AD globaler Administrator-Anmeldeinformationen.  




## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über [Ihre lokalen Monitor Identität Infrastruktur und Synchronisierung in der Cloud](active-directory-aadconnect-health.md).
