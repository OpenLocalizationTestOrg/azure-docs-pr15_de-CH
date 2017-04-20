<properties
    pageTitle="Sichern von Systemzugriff in Azure AD | Microsoft Azure"
    description="Ein Thema, das Verfahren für die Absicherung von Systemzugriff in Azure Azure Active Directory und Microsoft Online Services erläutert."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>Systemzugriff Sichern in Azure AD

Privilegierten Zugang ist ein wichtiger erster Schritt zum Schutz von Unternehmensressourcen in modernen Unternehmen. Privilegierte Konten sind verwalten IT-Systeme. Cyber Angreifer-diese Konten Zugriff auf Daten und Systeme des Unternehmens. Um Zugang zu sichern, sollten Sie Konten und Systeme vor Angriffen durch böswillige Benutzer isolieren.

Weitere Benutzer starten Systemzugriff Cloud-Dienste zu. Dazu gehören globale Administratoren Office365, Azure-Abonnement-Administratoren und Benutzer mit Administratorzugriff auf VMs oder SaaS-apps.

Microsoft empfiehlt, führen Sie diese Anleitung auf [Privilegierten Zugriff sichern](https://technet.microsoft.com/library/mt631194.aspx).

Für Kunden mit Azure Active Directory zum Verwalten des Zugriffs auf Azure, Office 365 oder andere Microsoft Services und Applikationen Grundsätze dieser Benutzerkonten verwaltet und von Active Directory oder in Azure Active Directory authentifiziert. Die folgenden Abschnitte bieten Informationen zu Azure AD-Funktionen unterstützen privilegierten Zugang.

## <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung

Erhöhen Sie die Sicherheit der Administratorauthentifizierung benötigen Sie mehrstufige Authentifizierung (MFA) bevor Sie Berechtigungen gewähren. MFA ist eine Methode, die Sie überprüfen die Verwendung von mehr als nur einen Benutzernamen und ein Kennwort erforderlich. Es bietet eine zweite Benutzer anmelden und Transaktionen.

Azure mehrstufige Authentifizierung hilft Schutz Zugriff auf Daten und Applikationen Benutzer Nachfrage nach einem einfachen Vorgang. Er bietet starke Authentifizierung über einen Bereich einfach Überprüfungsoptionen, einschließlich Anrufe, SMS, mobile app-Benachrichtigungen oder Verifizierungscode und 3rd Party EID Token.

Einen Überblick über die Funktionsweise von Azure mehrstufige Authentifizierung finden Sie im folgende Video.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

Weitere Informationen finden Sie unter [MFA MFA für Azure zu Office 365](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Zeitlich Berechtigungen

In einigen Organisationen möglicherweise haben viele Benutzer umfangreiche Funktionen. Ein Benutzer könnte hinzugefügt wurden der Rolle für eine bestimmte Aktivität wie für einen Dienst anmelden, aber nicht die Berechtigungen häufig danach.

Belichtungszeit Berechtigungen senken und Ihre Sichtbarkeit in ihrer Verwendung beschränken Sie Benutzer zu nur ihre Rechte Just-in-Time (JIT), wenn sie eine Aufgabe ausführen müssen. Azure Active Directory und Microsoft Online Services können Sie [Azure AD privilegierten Identitätsmanagement (PIM)](http://aka.ms/AzurePIM).


![PIM-dashboard][2]


## <a name="attack-detection"></a>Von Angriffen

[Azure Active Directory Schutz](../active-directory-identityprotection.md) bietet eine konsolidierte Ansicht Risiken und potenzielle Schwachstellen Ihres Unternehmens Identitäten. Basierend auf Risiken, berechnet Schutz ein Benutzer Risikostufe für die einzelnen Benutzer aktivieren Sie risikobasierte Richtlinien automatisch Schutz Ihrer Organisation konfigurieren. Diese Richtlinien zusammen mit anderen Kontrollen Zugangskontrolle Azure Active Directory mit EMS können automatisch Benutzer blockieren oder Vorschläge, die das Zurücksetzen von Kennwörtern und mehrstufige Authentifizierung erzwingen.

![Azure AD-Schutz][3]

## <a name="conditional-access"></a>Zugangskontrolle

Mit bedingten Zugriffskontrolle überprüft Azure Active Directory die besonderen Umständen auswählen, wenn einen Benutzer authentifizieren, bevor Zugriff auf eine Anwendung. Die Suchkriterien erfüllt, wird der Benutzer authentifiziert und Zugriff auf die Anwendung.


![MFA bedingten Zugriffsregeln festlegen][4]


## <a name="related-articles"></a>Verwandte Artikel

- [Azure Mehrfaktor-Authentifizierung](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md) aktivieren
- Aktivieren von [Azure AD Berechtigungen Identitätsmanagement](../active-directory-privileged-identity-management-configure.md)
- [Azure AD-Schutz](../active-directory-identityprotection.md) aktivieren
- [Zugangskontrolle-Steuerelemente](../active-directory-conditional-access.md) aktivieren


Weitere Informationen zum Erstellen einer Sicherheits-Roadmap finden Sie im Abschnitt "Pflichten des Kunden und Roadmap" des Dokuments [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) . Weitere Informationen zum Engagement von Microsoft bei diesen Themen wenden Sie sich an Ihren Microsoft-Vertreter oder [Cyber-Solutions-Seite](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
