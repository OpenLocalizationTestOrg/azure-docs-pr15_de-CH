<properties
    pageTitle="Azure Active Directory Schutz entdeckten Schwachstellen | Microsoft Azure"
    description="Übersicht über Schwachstellen von Azure Active Directory Schutz erkannt."
    services="active-directory"
    keywords="Azure active Directory Schutz, Cloud app Discovery, Verwalten von Applikationen, Sicherheit, Risiken, Risiko, Schwachstelle, Sicherheitsrichtlinien"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Azure Active Directory Schutz entdeckten Schwachstellen 

Schwachstellen werden Sicherheitslücken in Ihrer Umgebung, die von einem Angreifer ausgenutzt werden können. Wir empfehlen Adresse diese Schwachstellen Sicherheitslage Ihrer Organisation verbessern und verhindern, dass Angreifer auszubeuten.
<br><br>
![Sicherheitslücken](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")
<br>

Die folgenden Abschnitte enthalten Überblick Identitätsschutz gemeldete Schwachstellen.

## <a name="multi-factor-authentication-registration-not-configured"></a>Mehrstufige Authentifizierung Registrierung nicht konfiguriert 

Diese Sicherheitslücke können Sie die Bereitstellung von Azure mehrstufige Authentifizierung in Ihrer Organisation steuern. 

Azure mehrteilige Authentifizierung bietet eine zweite Sicherheitsebene Benutzerauthentifizierung. Sie können Zugriff auf Daten und Programme zu schützen und Benutzer Nachfrage nach einem einfachen Vorgang. Bietet starke Authentifizierung über eine Reihe von einfachen Überprüfungsoptionen, Telefonanruf, SMS oder mobile app Benachrichtigung oder Überprüfung Code und 3. Partei EID-Token.

Wir empfehlen, Azure mehrstufige Authentifizierung Benutzer anmelden müssen. Mehrstufige Authentifizierung spielt eine Schlüsselrolle in risikobasierte bedingte Richtlinien über Schutz.

Weitere Informationen finden Sie unter [Neuigkeiten Azure mehrstufige Authentifizierung?](../multi-factor-authentication/multi-factor-authentication.md)


## <a name="unmanaged-cloud-apps"></a>Nicht verwaltete Cloud apps

Diese Sicherheitslücke können Sie nicht verwaltete Cloud-apps in Ihrer Organisation zu identifizieren.
 
In modernen Unternehmen sind die IT-Abteilung häufig nicht alle Benutzer in ihrer Organisation verwenden, um ihre Arbeit Cloudanwendungen. Es ist leicht zu sehen, warum Administratoren Bedenken nicht autorisierten Zugriff auf Unternehmensdaten, mögliche Datenverluste und andere Sicherheitsrisiken. 

Wir empfehlen, Ihrer Organisation Bereitstellung Cloud App Discovery nicht verwalteten Cloudanwendungen ermitteln und diesen mit Azure Active Directory verwalten.

Weitere Informationen finden Sie [nicht verwaltete Cloudanwendungen mit Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).



##<a name="security-alerts-from-privileged-identity-management"></a>Sicherheitshinweise von privilegierten Identitätsmanagement

Diese Sicherheitslücke können Sie erkennen und Alarme über privilegierten Identitäten in der Organisation.  

Zu privilegierte Vorgänge zu Organisationen müssen Benutzer temporär oder permanent Systemzugriff in Azure AD gewähren Azure oder Office 365 Ressourcen oder andere SaaS-apps. Jeder dieser privilegierte Benutzer vergrößert sich die Angriffsfläche Ihrer Organisation. Diese Sicherheitslücke können Sie Benutzer mit unnötigen Systemzugriff identifizieren und Maßnahmen zur Verringerung oder Beseitigung des Risikos klingen. 

Wir empfehlen Ihr Unternehmen verwendet Azure AD privilegierten Identitätsmanagement, Kontrolle, Verwaltung und Monitor privilegierte Identitäten und den Zugriff auf Ressourcen in Azure AD sowie andere Microsoft-Onlinedienste wie Office 365 oder Windows Intune.

Weitere Informationen finden Sie in [Azure AD privilegierten Identitätsmanagement](active-directory-privileged-identity-management-configure.md). 



## <a name="see-also"></a>Siehe auch

 - [Azure Active Directory-Schutz](active-directory-identityprotection.md)
