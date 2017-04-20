<properties
    pageTitle="Mit Windows 10 Geräte in Ihrem Arbeitsbereich | Microsoft Azure"
    description="Bietet eine Übersicht über Funktionen für Benutzer und IT kontrastierenden Arten ein Gerät bereitgestellt und in einem Unternehmen mit Windows 10 verwendet werden kann."
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
    ms.date="10/17/2016"
    ms.author="femila"/>

# <a name="using-windows-10-devices-in-your-workplace"></a>Verwenden Windows 10 Geräte am Arbeitsplatz

Gilt für: Windows 10 PCs

Windows 10 bietet drei Modelle für Organisationen, die Ressourcen der Art Arbeit sicher und bequem zugreifen können.

- **Azure Active Directory beitreten** (Azure AD Join) für Arbeitskräfte, die Zugriff auf Ressourcen wie Office 365 hauptsächlich in der Cloud. Azure AD Join arbeitet Self-service-Erlebnis in Windows 10 neue Bereitstellung.
- **Domänenbeitritt**für Organisationen Investitionen in lokalen apps und Ressourcen. Beitreten zu einer Domäne bietet eine verbesserte 10 Windows Azure AD an.
- **Neue einfacheres BYOD**für Benutzer, die Windows und einfach Zugriff auf Ressourcen auf persönlichen Geräten ein Arbeitskonto oder Schule hinzufügen möchten.

Die folgende Tabelle zeigt einen Snapshot-Funktionen für Benutzer und IT-Administratoren kontrastierenden Arten, die ein Gerät bereitgestellt und in einem Unternehmen mit Windows 10 verwendet werden kann:

|                                                                                                                                                                 | Beitreten zu einer Domäne     | Azure AD Join | Persönliches Gerät |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------|-----------------|
| Windows-Gerät anmelden für Geschäfts-oder.                                                                                                                      | Ja             | Ja           | Nein              |
| Benutzer einmaliges Anmelden (SSO), Office 365 und Azure AD apps. SSO ist die Möglichkeit, nur einmal anmelden, um Zugriff auf Unternehmensressourcen. | Ja             | Ja           | Ja             |
| Benutzer SSO Kerberos-NTLM-apps.                                                                                                                                  | Ja             | Begrenzt       | Ja, über VPN         |
| Sichere Autorisierung und einfache Anmeldung für Arbeit oder Schule mit Microsoft Passport Windows Hello.                                                                   | Ja             | Ja           | Ja             |
| Zugriff auf Enterprise Windows Store mit einer Arbeit oder Schule (kein microsoftkonto).                                                                                    | Ja             | Ja           | Ja             |
| Enterprise-kompatible roaming User Settings auf Geräten mit Geschäfts-oder.                                                                                 | Ja             | Ja           | Ja             |
| Die Möglichkeit, organisatorische apps auf Geräten Zugriff auf, die mit Organisationseinheiten Gerät kompatibel sind.                                                      | Ja             | Ja           | Ja             |
| Benutzer von Geräten "Arbeit überall" provisioning                                                                                                | Nein              | Ja           | Ja             |
| Möglichkeit zum Verwalten von Geräten.                                                                                                                                       | Ja, über GP-SCCM | Ja           | Ja             |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Mit Arbeit Geräte Azure AD Join und Domäne beitreten in Windows 10

Windows 10 bietet zwei Methoden zum Arbeiten Geräte auf Ressourcen der Art Arbeit:

- Azure AD Join
- Beitreten zu einer Domäne

 Beide können gültige Optionen je nach Bedarf und Anforderungen einer Organisation sein. In einigen Fällen können Organisationen profitieren beide Methoden der Bereitstellung aktivieren.

## <a name="when-to-use-azure-active-directory-join"></a>Verwendung von Azure Active Directory beitreten

Azure AD Join ist eine neue Self-service bei Windows 10 bereitstellen.  Er richtet Arbeitnehmer, die Zugriff auf Arbeitsressourcen wie Office 365 hauptsächlich in der Cloud. Es ist eine einfache Möglichkeit, Computern, Tablets und Telefone für Unternehmen konfigurieren. Geräte werden über mobile Device Management verwaltet konsistent Steuerelemente auf Windows-Plattformen.

**Mit Azure AD Join aus folgenden Gründen**:

- Soll die Self-service-Bereitstellung Geräte für Arbeitskräfte zu.
- Sie bieten Benutzern arbeiten mobile Geräte wie Tablets und Telefone.
- Eine Gruppe von Benutzern wie Saisonarbeiter, Auftragnehmer und Studenten in Azure AD nicht in Active Directory verwalten möchten.
- Sie möchten fügen Funktionen für Arbeitskräfte in Zweigstellen mit lokalen begrenzte Infrastruktur bereitstellen.
- Sie haben keine lokale Active Directory.

Einige Organisationen verwenden Azure AD Join als die primäre Methode zum Bereitstellen von Arbeit Geräte, als sie oder ihre Ressourcen zur Cloud migriert. Hybrid-Organisationen mit Active Directory und Azure AD können auch eine oder andere Methode je nach Benutzer oder Abteilung bereitstellen.

Schulen und Universitäten, könnte z. B. in Active Directory Personal und die Schüler in Azure AD verwalten. Einige Unternehmen möchten Zweigstellen oder Vertrieb in Azure AD verwalten. Azure AD Join und Domäne Join-Methoden können innerhalb einer hybridorganisation verwendet werden. Azure AD Join kann eine großartige Ergänzung zum Beitreten zu einer Domäne für die Bereitstellung von Geräten in ein.

**Ist der häufigste Zugriff auf Unternehmensressourcen über die Cloud Ihrer Organisation möglicherweise nutzen zusätzliche Wenn**:

- Abhängigkeit von lokalen Identitätsinfrastruktur können entfernt werden.
- Abrufen von imaging-Lösungen ermöglicht Self-service-Konfiguration können Sie Geräte Bereitstellungsmodell vereinfachen.
- Verwaltung mobiler Geräte können alle Geräte über verschiedene Plattformen hinweg verwalten.

Weitere Informationen zu Azure AD Join finden Sie unter [Erweitern Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Domäne beitreten (bzw. Verwendung beibehalten)

Die letzten 15 Jahren haben viele Organisationen Domänenbeitritt Arbeit Geräte verwendet. Es ermöglicht Benutzern das anmelden, um ihre Geräte mit Active Directory Arbeit oder Schule Konten. Beitreten zu einer Domäne kann IT-Geräte zentral und vollständig verwalten. Organisationen normalerweise Sicherungsmethoden Bereitstellung Geräte hängen und häufig verwenden Sie System Center Configuration Manager (SCCM) oder Group Policy (GP) verwalten.

**Unternehmen verwenden Domäne beitreten (oder Verwendung beibehalten) aus folgenden Gründen**:

- Sie haben Win32-apps für diese Geräte, NTLM/Kerberos bereitgestellt.
- Sie benötigen GP oder SCCM-DCM Geräte verwalten.
- Möchten Sie weiterhin imaging-Lösungen Konfigurieren von Geräten für Ihre Mitarbeiter.

**Beitreten zu einer Domäne in Windows 10 bietet Ihnen außerdem folgende Vorteile bei Azure AD mit**:

- Starke Authentifizierung Gerät gebunden und einfache Anmeldung für Arbeit oder Schule mit Microsoft Passport Windows Hello.
- Zugriff auf das Unternehmen Windows Store für Geräte, die Arbeit oder Schule Konten (kein Microsoft-Konto erforderlich).
- Enterprise-kompatible roaming User Settings über Geräte, Arbeit oder Schule Konten (kein Microsoft-Konto erforderlich).
- Die Möglichkeit, organisatorische apps auf Geräten Zugriff auf, die mit Organisationseinheiten Gerät kompatibel sind.

Weitere Informationen zu Azure AD Join finden Sie unter [Erweitern Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Aktivieren von persönlichen Geräten Arbeit oder Schule beitreten

Zur Unterstützung von BYOD im Unternehmen können Windows 10 Benutzer ihre Computer, Tablet oder Telefon Arbeits- oder Schulcomputer Konto hinzu. Nachdem der Benutzer ein Konto Arbeits- oder Schulcomputer hinzufügt, wird das Gerät Azure AD registriert und optional im mobilen Gerät-Management-System, das die Organisation konfiguriert wurde registriert. Das Verzeichnis zeigen dieser Geräte als 'Registriert' vs. "verknüpft Azure AD". IT-Administraors können verschiedene Richtlinien basierend auf diesen Informationen etwas leichter auf persönlichen Geräten als Arbeit Geräte ggf. anwenden.

Benutzer können eine hinzufügen oder Konto für ihre persönlichen Geräte bequem Schule. Sie hierzu beim Zugriff auf eine Anwendung Arbeit zum ersten Mal können, oder sie es manuell Menü Einstellungen. Dieses Konto bietet SSO auf Unternehmensressourcen.

Weitere Informationen zu Azure AD Join finden Sie unter [Verbinden Domäne Geräte Azure AD für Windows 10 auftritt](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Domäne oder Azure AD Verknüpfung aktivieren

Alle oben beschriebenen Methoden (Domänenbeitritt und Azure AD Join hinzufügen Arbeit oder Schule Konto) Einstiegspunkte in die Benutzeroberfläche Windows 10 haben. Jedoch alle IT-Administratoren die Funktionalität der Infrastruktur aktivieren, bevor die ausgeführt wird.

## <a name="requirements-for-deploying-azure-ad-join"></a>Vorschriften für die Bereitstellung von Azure AD Join

Zum Bereitstellen von Azure AD Join für beliebige Benutzer benötigen Sie Folgendes:

- Ein Azure AD-Abonnement.
- Ein Azure AD Premium-Abonnement, wie mobile Device Management automatische Registrierung, benötigen Sie weitere Funktionen.
- Verwaltung mobiler Geräte – z. B. ein Windows Intune-Abonnement, mobile Device Management für Office 365 oder einem Partner Mobilgerät Management-Anbieter, die in Azure AD integriert. Siehe [FAQs](#frequently-asked-questions) am Ende dieses Artikels Weitere Informationen.

Sind Ihre Anlagen Hybrid, empfehlen wir nachdrücklich, Azure AD Connect erweitern das lokale Verzeichnis Azure AD bereitzustellen.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Domänenbeitritt in Azure AD verwenden

Beitreten zu einer Domäne weiterhin wie bisher. Jedoch werden der Azure AD Vorteile benötigen Sie Folgendes:

- Ein Azure AD-Abonnement.
- Eine Bereitstellung von Azure AD mit lokalen Verzeichnis Azure AD erweitern.
- Eine Richtlinie, die Domäne bedingte auf Azure AD ermöglicht.
- Eine Richtlinie, die Zugriff auf Geräte "Domäne" Zugriff für einige Geräte werden soll.
- System Center Configuration Manager Version 1509 technischen Vorschau aktivieren Sie Regeln für kompatible Geräte erfordern. Siehe die TechNet-Dokumentation und Blogbeitrag.

Informationen zum Beitreten zu einer Domäne in Windows 10 finden Sie unter [Verbinden Domäne Geräte Azure AD für Windows 10 auftritt](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>Vorschriften für BYOD und "Hinzufügen eines Arbeit oder Schule"

So aktivieren Sie "bring your own Device" (BYOD) mit Arbeits- oder Schulcomputer Konten benötigen Sie:

- Ein Azure AD-Abonnement.
- Ein Azure AD Premium-Abonnement, wie mobile Device Management automatische Registrierung, benötigen Sie weitere Funktionen.

## <a name="requirements-for-using-microsoft-passport"></a>Vorschriften für die Verwendung von Microsoft Passport

Um Microsoft Passport zu aktivieren, benötigen Sie Folgendes:

- Eine Infrastruktur öffentlicher Schlüssel (PKI) für die zertifikatbasierte Authentifizierung unterstützt, die Microsoft Passport verwendet.
- Eine Intune-Abonnement für zertifikatbasierte Authentifizierung unterstützt, die Microsoft Passport für Azure AD und Geschäfts-oder verwendet.
- System Center Configuration Manager Version 1509 Technical Preview (Siehe TechNet-Dokumentation und Blogbeitrag) für zertifikatbasierte Authentifizierung unterstützt, die Microsoft Passport für Domänenbeitritt verwendet.
- Eine Richtlinie zum Aktivieren von Microsoft Passport in der Organisation.

Als Alternative zur Verwendung einer PKI können Sie Microsoft Passport basierte folgendermaßen:

- Bereitstellen von Windows Server 2016 "Produktion Preview 1" DCs (ohne Domäne oder Gesamtstruktur-Funktionsebenen, mehrere Domänencontroller für jede Active Directory-Standort ausreichend Redundanz dienen).
- Gruppenrichtlinie für die Aktivierung von Microsoft Passport in der Organisation festgelegt.

Weitere Informationen über Microsoft Passport und Windows Hello in Windows 10 finden Sie unter < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Welche Partner mobile Device Management-Produkte in Azure AD integriert werden?

Die folgenden Lieferantenprodukte integrieren Azure AD für einheitliche Anmeldung und Zugangskontrolle in Windows 10:

- AirWatch von VMware
- Citrix-Xenmobile
- Lightspeed Mobile Manager
- SOTI lokale Verwaltung mobiler Geräte
- MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Was ist mit Arbeitsplatz in Windows 10 beitreten?
Arbeitsplatz Verknüpfung wurde in Windows 8.1 verwendet, um BYOD zu aktivieren. Windows 10 ist BYOD über "Hinzufügen eine Arbeit oder Schule Konto" aktiviert, wie weiter oben in diesem Dokument erläutert. Für Organisationen, die die Verwaltung mobiler Geräte in Azure AD integriert nicht Benutzer des Geräts in Management über **Einstellungen**registrieren > **Konten** > **Arbeit Zugriff**.


### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Können Benutzer ihre Microsoft-Konto an ihrer Domäne in Windows 10?
Nicht in Windows 10. In Windows 8.1 können Benutzern Domäne "Microsoft-Konto (z. B. Hotmail, Live, Outlook, Xbox, etc.) ihr Domänenkonto bestimmte wie SSO, Live-Diensten verwenden Windows Store und roaming User Settings auf Geräten ermöglichen verbinden". Windows 10 wurde das Microsoft-Konto "Funktionen connect" eingestellt. Benutzer kann ein oder mehrere Microsoft-Konten als Dienstleistungen wie Windows Store SSO ermöglichen zusätzliche Konten hinzufügen. Dies erfolgt im **Settings** > **Konten** > **Ihr Konto**.

Benutzer, die ein Upgrade von Windows 8.1 Domäne Geräte und hatte Microsoft-Konto, verbunden, automatisch dazu verwendeten Konten verbundenen Microsoft-Konto verfügen.


## <a name="additional-information"></a>Weitere Informationen
* [Windows 10 für Unternehmen: Verwendungsmöglichkeiten für Geräte für die Arbeit](active-directory-azureadjoin-windows10-devices-overview.md)
* [Erweitern von Cloud-Funktionen für Windows 10 Geräte Azure Active Directory beitreten](active-directory-azureadjoin-user-upgrade.md)
* [Informationen Sie zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Geräte Domäne Azure AD für Windows 10 Erlebnisse](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)
