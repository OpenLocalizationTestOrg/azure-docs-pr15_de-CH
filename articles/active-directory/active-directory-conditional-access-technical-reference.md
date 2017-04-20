
<properties
    pageTitle="Azure Active Directory Zugangskontrolle technische | Microsoft Azure"
    description="Mit bedingten Zugriffskontrolle überprüft Azure Active Directory die besonderen Umständen, wenn Benutzer authentifizieren, bevor Zugriff auf die Anwendung auswählen. Die Suchkriterien erfüllt, wird der Benutzer authentifiziert und Zugriff auf die Anwendung."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Technische Referenz zu Azure Active Directory Zugangskontrolle

## <a name="services-enabled-with-conditional-access"></a>Dienste mit Zugangskontrolle aktiviert
Bedingte Zugriffsregeln werden über verschiedene Azure AD-Anwendung unterstützt. Diese Liste enthält:

- Verbundanwendungen aus der Galerie Azure AD
- Kennwort-SSO-Anwendung aus der Galerie Azure AD
- Registriert den Anwendungsproxy Azure Applications
- Entwickelte Zeile und Multi-Tenant-Applikationen in Azure AD registriert
- Visual Studio Online
- Azure RemoteApp
-   Dynamics CRM
- Microsoft Office 365 Yammer
- Microsoft Office 365 Exchange Online
- Microsoft Office 365 SharePoint Online (einschließlich OneDrive für Unternehmen)


## <a name="enable-access-rules"></a>Aktivieren von Regeln

Jede Regel kann aktiviert oder deaktiviert eine pro Anwendung Basen. Wenn die Regeln **auf** sie aktiviert und für Benutzer der Anwendungdes erzwungen. **Aus** werden sie nicht verwendet werden und wirkt sich nicht auf den Benutzer Anmeldevorgang.

## <a name="applying-rules-to-specific-users"></a>Anwenden von Regeln auf bestimmte Benutzer
Regeln können an bestimmten Sicherheitsgruppe **Angewendet**auf Benutzer angewendet werden. **Anwenden,** können **Alle Benutzer** oder **Gruppen**festgelegt werden. Wenn die Regeln gelten für **Alle Benutzer** auf alle Benutzer mit Zugriff auf die Anwendung. Die Option **Gruppen** kann bestimmte Sicherheits- und Verteilergruppen zu Regeln nur für diese Gruppen erzwungen.

Beim Bereitstellen einer Regel gilt zuerst anwenden eine begrenzte Benutzer festgelegt, die Mitglieder der Gruppen steuern. Danach kann die Regel auf **Alle Benutzer**angewendet. Dadurch wird die Regel für alle Benutzer in der Organisation verwendet werden.

Ausgewählte Gruppen können auch mit der Option **außer** Richtlinie ausgenommen werden. Mitglieder dieser Gruppen werden ausgeschlossen, auch wenn sie in einer Gruppe enthalten.

## <a name="at-work-networks"></a>"At Work"-Netzwerke


Bedingte Regeln, mit denen ein Netzwerk "am Arbeitsplatz" abhängig von vertrauenswürdigen IP-Adressbereiche, die in Azure AD konfiguriert oder "inside Corpnet" Anspruch von AD FS. Dazu gehören:

- Mehrstufige Authentifizierung nicht am Arbeitsplatz
- Zugriff nicht am Arbeitsplatz

Optionen für die Angabe "am Arbeitsplatz" Netzwerke

1. Konfigurieren Sie vertrauenswürdige IP-Adressbereiche der [kombinierten Authentifizierung Konfigurationsseite](../multi-factor-authentication/multi-factor-authentication-whats-next.md). Bedingte Richtlinie verwendet konfigurierten Bereiche zu jeder Authentifizierungsanfrage token ausstellen Regeln ausgewertet. 
2. Konfigurieren des inneren Corpnet behaupten, diese Option kann mit verbundenen Verzeichnissen mit AD FS verwendet. [Erfahren Sie mehr über den inneren Krone Ansprüche](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).
3. Konfigurieren Sie öffentliche IP-Adressbereiche. Auf der Registerkarte konfigurieren können Sie für das Verzeichnis öffentliche IP-Adressen festlegen. Conditional Access diese als "at Work" IP-Adressen, um dadurch zusätzliche Bereiche konfiguriert werden, überschreiten die 50 IP-Adresse, die der Seite MFA erzwungen wird.



## <a name="rules-based-on-application-sensitivity"></a>Regeln auf Anwendung

Regeln werden pro Anwendung die hochwertige Dienste gesichert werden, ohne Beeinträchtigung der Zugriff auf andere Dienste konfiguriert. Bedingte Regeln können auf die Registerkarte **Konfigurieren** der Anwendung konfiguriert werden. 

Regeln angeboten:

- **Mehrstufige Authentifizierung**
 - Alle Benutzer, denen diese Richtlinie gilt werden über mehrstufige Authentifizierung einmal authentifizieren.
 
- **Mehrstufige Authentifizierung nicht am Arbeitsplatz**
 - Wenn diese Richtlinie angewendet wird, werden alle Benutzer mindestens mehrstufige Authentifizierung durchgeführt haben, wenn sie den Dienst von einem Remotestandort Arbeitsunterbrechung zugreifen. Wenn sie eine Arbeitsaufgabe zu verschieben, müssen sie die kombinierte Authentifizierung beim Zugriff auf den Dienst ausführen.
 
- **Zugriff nicht am Arbeitsplatz** 
 - Wenn Benutzer an einem Remotestandort aus verschieben, werden sie blockiert, wenn "Sperren nicht am Arbeitsplatz" Richtlinie zugewiesen ist.  Sie werden an einem Arbeitsplatz erneut zugreifen.


## <a name="related-topics"></a>Verwandte Themen

- [Zugang zu Office 365 und anderen apps verbunden in Azure Active Directory](active-directory-conditional-access.md)
- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
