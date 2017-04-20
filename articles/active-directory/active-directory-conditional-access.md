<Properties
    pageTitle="Azure Active Directory Zugangskontrolle | Microsoft Azure"  
    description="Verwenden Sie bedingter in Azure Active Directory für bestimmte Situationen überprüft bei der Authentifizierung für den Zugriff auf Programme."  
    services="active-directory"
    keywords="Zugangskontrolle apps, Zugangskontrolle mit Azure, sicheren Zugriff auf Unternehmensressourcen, bedingter Richtlinien"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/21/2016"
    ms.author="markvi"/>


# <a name="conditional-access-in-azure-active-directory"></a>Zugangskontrolle in Azure Active Directory   

Control-Funktionen in Azure Active Directory (Azure AD) bedingten Zugriff bieten sichere Ressourcen in der Cloud und lokal auf einfache Weise. Bedingte Richtlinien wie mehrstufige Authentifizierung schützen gegen Diebstahl und Phishing. Andere bedingte Richtlinien helfen Ihrer Organisation Daten. Beispielsweise zusätzlich Anmeldeinformationen, einer Richtlinie, die nur Geräte möglicherweise, die mobile Device Management-System registriert sind, wie Windows Intune vertrauliche Dienstleistungen Ihrer Organisation zugreifen können.


## <a name="prerequisites"></a>Erforderliche Komponenten

Azure AD Zugangskontrolle ist ein Feature von [Azure Active Directory Premium](http://www.microsoft.com/identity). Jeder Benutzer, der eine Anwendung zugreift, die bedingte Richtlinien angewendet müssen eine Azure AD Premium-Lizenz. Sie können Lizenzen Vorschriften [nicht lizenzierte Benutzer](https://aka.ms/utc5ix)erfahren.


## <a name="how-is-conditional-access-control-enforced"></a>Wie wird bedingte Zugriffskontrolle durchgesetzt?  

Bedingte Zugriffskontrolle an Azure AD überprüft für die spezifische Hygienevorschriften für Benutzer Zugriff auf eine Anwendung festlegen. Nachdem Access erfüllt sind, wird der Benutzer authentifiziert und kann auf die Anwendung zugreifen.  

![Übersicht über die Zugangskontrolle](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a>Startbedingungen

Vorschriften, die in einer bedingten Richtlinie enthalten sind:

- **Gruppenmitgliedschaft**. Steuern des Benutzerzugriffs basierend auf der Mitgliedschaft in einer Gruppe.

- **Speicherort**. Der Speicherort des Benutzers Trigger mehrstufige Authentifizierung und Block-Steuerelemente verwenden, wenn ein Benutzer nicht in einem vertrauenswürdigen Netzwerk ist.

- **Plattform**. Verwenden Sie die Geräteplattform wie iOS, Android, Windows Mobile oder Windows als Bedingung für die Richtlinien.

- **Gerät aktiviert**. Des Geräts wird aktiviert oder deaktiviert, während der Auswertung von Richtlinien überprüft. Wenn ein verlorenes oder Gestohlenes Gerät im Verzeichnis deaktivieren, können mehr Richtlinien erfüllen.

- **Anmelden und Benutzer Risiko**. Sie können [Azure AD-Schutz](active-directory-identityprotection.md) für bedingte Risiko-Richtlinien. Bedingte Risiko Richtlinien helfen Ihre Organisation voraus basierend auf Risiken und ungewöhnliche Aktivitäten zu schützen.


## <a name="controls"></a>Steuerelemente

Hierbei handelt es sich um Steuerelemente, mit denen Sie eine bedingte Richtlinie zu erzwingen:

- **Mehrstufige Authentifizierung**. Sie können starke Authentifizierung durch mehrstufige Authentifizierung erforderlich. Mehrstufige Authentifizierung können mit Azure mehrstufige Authentifizierung oder lokale mehrstufige Authentifizierungsanbieter, kombiniert mit Active Directory-Verbunddienste (AD FS). Mehrstufige Authentifizierung schützt Ressourcen vor dem Zugriff durch ein nicht autorisierter Benutzer, der Zugriff auf die Anmeldeinformationen eines gültigen Benutzers erlangt haben kann.

- **Blockieren**. Sie können Benutzer sperren wie Benutzer gelten. Beispielsweise können Sie Zugriff blockieren, wenn ein Benutzer nicht in einem vertrauenswürdigen Netzwerk.

- **Kompatible Geräte**. Sie können bedingte Richtlinien auf festlegen. Sie können eine Richtlinie so einrichten, dass nur Computer, die Domäne oder mobile Geräte, die eine mobile Device Management-Anwendung registriert sind Ressourcen Ihrer Organisation zugreifen können. Beispielsweise können Sie mit Windows Intune der Geräte überprüfen und dann Azure AD für die Erzwingung melden, wenn der Benutzer versucht, auf eine Anwendung zugreifen. Detaillierte Anleitung zur Verwendung von Windows Intune zu apps und Daten finden Sie unter [schützen apps und Daten mit Windows Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune). Windows Intune können Sie Datenschutz für verlorene oder gestohlene Geräte erzwingen. Weitere Informationen finden Sie unter [Schutz Ihrer Daten mit vollständigen oder selektiv mit Windows Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).

## <a name="applications"></a>Applikationen

Sie können auf eine bedingten Richtlinie erzwingen. Festlegen Sie Zugriffsebenen für Programme und Dienste in der Cloud oder lokal. Die Richtlinie ist direkt auf der Website oder einem Dienst angewendet. Die Richtlinie wird erzwungen, Zugriff auf den Browser und Programme, die auf den Dienst zugreifen.


## <a name="device-based-conditional-access"></a>Gerät bedingten Zugriff

Sie können Zugriff auf Applikationen von Geräten, Azure AD registriert werden und die spezifische Auflagen erfüllen. Gerät bedingten Zugriff schützt Ressourcen einer Organisation von Benutzern, die Zugriff auf die Ressourcen von:

- Unbekannte oder nicht verwalteten Geräte.
- Geräte, die Sicherheitsrichtlinien der Organisation erfüllen.

Sie können Richtlinien basierend auf diese Anforderungen festlegen:

- **Domäne Geräte**. Legen Sie eine Richtlinie zum Beschränken des Zugriffs auf Geräte lokalen Active Directory-Domäne angehören und auch registrierten Azure AD. Diese Richtlinie gilt für Windows-Desktops, Laptops und Tablets Enterprise.
Weitere Informationen zum automatische Registrierung Geräte mit Azure AD-Domäne finden Sie unter [Automatische Registrierung von Windows Geräte Azure Active Directory - Domäne einrichten](active-directory-conditional-access-automatic-device-registration-setup.md).

- **Kompatible Geräte**. Festlegen einer Richtlinie Zugriff auf Geräte einschränken, die **kompatible** im Systemverzeichnis Management gekennzeichnet sind. Diese Richtlinie gewährleistet, dass nur Geräte, die Sicherheitsrichtlinien wie Erzwingen der Verschlüsselung auf einem Gerät erfüllen zugreifen dürfen. Diese Richtlinie können Sie von den folgenden Geräten Zugriff:

    - **Windows - Domäne Geräte**. Verwaltet von System Center Configuration Manager (im aktuellen Zweig) in einer hybridkonfiguration bereitgestellt.
    - **Windows 10 Mobile Arbeit oder PDAs**. Verwaltet von Intune oder von einem Verwaltungssystem unterstützten mobilen Geräts.
    - **ios- und Android-Geräte**. Windows Intune verwaltet.


Benutzer Programme zugreifen, die Geräte-basierte geschützt sind, muss Richtlinien die Anwendung von einem Gerät zugreifen, die diese Richtlinie erfüllt. Zugriff verweigert, wenn auf ein Gerät, das nicht erfüllt.

Informationen zum Konfigurieren einer Geräte-basierte Richtlinien in Azure AD finden Sie unter [Festlegen von Device Grundlage bedingter Richtlinien für Applikationen Azure Active Directory verbunden](active-directory-conditional-access-policy-connected-applications.md).

## <a name="resources"></a>Ressourcen

Siehe folgende Kategorien und Artikel Weitere Informationen zu bedingten Zugriff für Ihre Organisation festlegen.


### <a name="multi-factor-authentication-and-location-policies"></a>Mehrstufige Authentifizierung und Speicherort Richtlinien

- [Erste Schritte mit Zugangskontrolle Azure AD verbundene Apps basierend auf, Speicherort und mehrstufige Authentifizierungsrichtlinien](active-directory-conditional-access-azuread-connected-apps.md)
- [Anträge, die unterstützt werden](active-directory-conditional-access-supported-apps.md)


### <a name="device-based-conditional-access"></a>Gerät bedingten Zugriff

- [Legen Sie gerätebasierte bedingte Richtlinie für Zugriffskontrolle auf Applikationen Azure Active Directory verbunden](active-directory-conditional-access-policy-connected-applications.md)
- [Automatische Registrierung von Windows Azure Active Directory-Domäne Geräte einrichten](active-directory-conditional-access-automatic-device-registration-setup.md)
- [Problembehandlung für Active Directory Azure Fragen](active-directory-conditional-access-device-remediation.md)
- [Schutz der Daten auf verlorenen oder gestohlenen Geräten mit Windows Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)


### <a name="protect-resources-based-on-sign-in-risk"></a>Schützen von Ressourcen basierend auf Anmelden

-   [Azure AD-Schutz](active-directory-identityprotection.md)

### <a name="next-steps"></a>Nächste Schritte

- [Zugangskontrolle FAQs](active-directory-conditional-faqs.md)
- [Technische Referenz](active-directory-conditional-access-technical-reference.md)
