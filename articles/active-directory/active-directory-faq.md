<properties
    pageTitle="Häufig gestellte Fragen zur Azure Active Directory | Microsoft Azure"
    description="Azure Active Directory – häufig gestellte Fragen, die Antworten auf Fragen im Zusammenhang mit Zugriff auf Azure und Azure Active Directory und Passwort."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>

# <a name="azure-active-directory-faq"></a>Häufig gestellte Fragen zur Azure Active Directory

Azure Active Directory ist eine umfassende as a Service (IDaaS)-Lösung, die alle Aspekte der Identität, Verwaltung und Sicherheit umfasst.


Weitere Informationen finden Sie unter [Azure Active Directory?](active-directory-whatis.md).



## <a name="accessing-azure-and-azure-active-directory"></a>Zugriff auf Azure und Azure Active Directory


**Warum erhalte ich "keine Abonnements gefunden" beim Versuch, Azure AD im klassischen Azure-Portal (https://manage.windowsazure.com) zugreifen?**

**A:** Klassische Azure-Portal zugreifen muss jeder Benutzer ein Azure-Abonnement berechtigt. Haben Sie eine bezahlte Office 365 oder Azure AD [http://aka.ms/accessAAD](http://aka.ms/accessAAD) für eine einmalige Aktivierungsschritt navigieren, andernfalls müssen Sie eine vollständige [Azure Testversion](https://azure.microsoft.com/pricing/free-trial/) oder ein kostenpflichtiges Abonnement aktivieren. 

Weitere Informationen finden Sie unter:

- [Azure-Abonnements Azure Active Directory zugeordnet sind](active-directory-how-subscriptions-associated-directory.md)

- [Verwalten Sie das Verzeichnis für Ihr Office 365-Abonnement in Azure](active-directory-manage-o365-subscription.md)

---

**Was ist die Beziehung zwischen Azure, Office 365 und Azure?**

**A:** Azure Active Directory bietet allgemeine Identitäts- und Funktionen für alle Microsoft-Onlinedienste. Ob Office 365, Microsoft Azure, Windows Intune oder andere verwendetem bereits Azure AD anmelden und Management für alle diese Dienste zugreifen. 

Tatsächlich werden alle Benutzer, die Sie für die Microsoft Online Services aktiviert haben wie Benutzerkonten in einer oder mehreren Azure AD Instanzen definiert. Sie können diese Konten kostenlos Azure AD-Funktionen wie Cloud-Anwendung.
 
Darüber hinaus Azure AD Dienstleistungen bezahlt (z.B.: Azure AD Basic, Premium, EMS etc.) ergänzen Online Services wie Office 365 und Microsoft Azure mit umfassenden Enterprise Skalierung Management und Sicherheit.


---



## <a name="getting-started-with-hybrid-azure-ad"></a>Erste Schritte mit Hybrid Azure AD


**F: wie kann ich meinem lokalen Verzeichnis Azure AD verbinden?**

**A:** Sie können das lokale Verzeichnis Azure AD mit **Azure AD verbinden**. 

Weitere Informationen finden Sie in [Ihrer lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).


---

**F: wie SSO zwischen meinem lokalen Verzeichnis und Meine Cloudanwendungen richte ich?**

**A:** Sie müssen nur SSO zwischen lokalen Verzeichnis und Azure AD einrichten. Als access Ihre Cloudanwendungen und Azure Laufwerke der Dienst automatisch Ihre Benutzer ordnungsgemäß mit ihren lokalen Anmeldeinformationen authentifizieren.

Implementieren von SSO lokal leicht Föderation Lösungen wie ADFS oder Kennwort-Hash-Synchronisierung konfigurieren lässt. Sie können problemlos beide Optionen Azure AD Connect-Konfigurationsassistenten bereitstellen.
  

Weitere Informationen finden Sie in [Ihrer lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
  

---

**F: bietet Azure Active Directory ein Self-service-Portal für Benutzer in der Organisation?**

**A:** Ja, bietet Active Directory Azure [Azure AD-Abdeckung](http://myapps.microsoft.com) für Benutzer und Anwendung. Wenn Sie ein Office 365-Kunde sind, finden Sie viele Funktionen in Office 365-Portal. 

Weitere Informationen finden Sie unter [Einführung in die](active-directory-saas-access-panel-introduction.md). 



---

**F: hilft Azure AD Meine lokale Infrastruktur mir?**

**A:** Ja, Ja. Azure AD Premium Edition bietet **Health verbinden**. Azure AD Connect Health können Sie überwachen und Einblick in Ihre lokale Identitätsinfrastruktur und Synchronization Services.  

Weitere Informationen finden Sie unter [Überwachen der lokalen Identität Infrastruktur und Synchronisierung Dienste in der Cloud](active-directory-aadconnect-health.md).  

---

## <a name="password-management"></a>Kennwort-management

**F: kann Azure AD Kennwort Zurückschreiben verwenden ohne Kennwort synchronisieren? (Alias Azure AD SSPR mit Kennwort Zurückschreiben verwenden möchte, aber ich will in der Cloud? gespeicherten Kennwörter)**

**A:** Sie brauchen Ihre Passwörter AD Azure AD synchronisieren um Zurückschreiben aktivieren. Das lokale Verzeichnis zur Authentifizierung des Benutzers in einer verbundenen Umgebung abhängig Azure AD SSO. Dieses Szenario erfordert keine lokalen Kennwort in Azure AD nachverfolgt werden.

---

**F: dauert wie lange ein Kennwort lokal AD zurückgeschrieben werden?**

**A:** Kennwort Zurückschreiben arbeitet in Echtzeit. 

Weitere Informationen finden Sie unter [Erste Schritte mit Passwort-Management](active-directory-passwords-getting-started.md) 


---

**F: verwenden kann ich Kennwort Rückschreib-Kennwörter, die von einem Administrator verwaltet werden?**

**A:** Ja, wenn Sie Kennwort Zurückschreiben aktiviert haben, werden Kennwort Operationen werden in der lokalen Umgebung zurückgeschrieben.  

Weitere Antworten auf Kennwort Fragen finden Sie unter [Kennwort Managementfragen](active-directory-passwords-faq.md).

---

## <a name="application-access"></a>Anwendung


**F: Wo finde ich eine Liste der bereits in Azure AD integriert sind und ihre Funktionen?**

**A:** Azure AD verfügt über 2600 integrierte Anwendung von Microsoft, Application Service Provider oder Partner. Alle integrierte Applikationen unterstützen SSO. SSO können Sie Ihre Organisation Anmeldeinformationen auf Ihre apps. Einige der Programme unterstützen auch automatisierte erteilen und entziehen

Eine vollständige Liste der integrierten Applikationen finden Sie unter [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).


---

**F: ist die erforderliche Anwendung nicht auf dem Markt Azure AD?**

**A:** Mit Azure AD Premium können Sie hinzufügen und jede Anwendung konfigurieren. Je nach Funktionalität der Anwendung und Ihre Präferenzen können Sie SSO und automatisierte Bereitstellung konfigurieren.  

Weitere Informationen finden Sie unter:

- [Einmaliges Anmelden für Clientanwendungen konfigurieren, die nicht in der Galerie Azure Active Directory](active-directory-saas-custom-apps.md)
- [So aktivieren Sie die automatische Bereitstellung von Benutzern und Gruppen aus dem Active Directory Azure Applications verwenden SCIM](active-directory-scim-provisioning.md) 


---

**F: Wie melde Benutzer in Clientanwendungen mithilfe von Azure Active Directory?**
 
**A:** Azure Active Directory bietet verschiedene Methoden angezeigt und wie ihre Anwendung zugreifen:

- Azure AD-Abdeckung

- Office 365 Anwendungsstartprogramm

- Direkte Anmeldung verbundenen apps

- Deep Links zu verbundenen, Passwort oder vorhandenen apps

Weitere Informationen finden Sie unter [Bereitstellen von Azure AD integrierte Programme für Benutzer](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).


---

**Was sind die Unterschiede in Azure Active Directory ermöglicht Authentifizierung und einmaliges Anmelden Anwendung?**
 
**A:** Azure Active Directory unterstützt viele standardisierte Protokolle für die Authentifizierung und Autorisierung SAML 2.0 OpenID verbinden, OAuth 2.0 und WS-Federation. Azure AD unterstützt auch Kennwort Vaulting und automatisierte Funktionen für apps, die nur formularbasierte Authentifizierung unterstützen.  

Weitere Informationen finden Sie unter:

- [Authentifizierungsszenarien Azure AD](active-directory-authentication-scenarios.md)

- [Active Directory-Authentifizierungsprotokolle](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Wie einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**F: hinzufügen kann ich Anwendung lokal ausführen?**

**A:** Azure AD-Anwendungsproxy bietet einfachen und sicheren Zugriff auf lokale Web Applications, die Sie auswählen. Sie können diesen genauso zugreifen SaaS-apps in Azure Active Directory zugreifen. Gibt es keine Notwendigkeit, ein VPN oder Ändern Ihrer Netzwerk-Infrastruktur.  

Weitere Informationen finden Sie unter [sicheren remote-Zugriff für lokale Anwendung](active-directory-application-proxy-get-started.md).


--- 

**F: benötige ich MFA für Benutzer einer bestimmten Anwendung?**

**A:** Bedingte Azure AD-Zugriff können Sie für jede Anwendung eine eindeutige Richtlinie zuweisen. In der Richtlinie können Sie MFA benötigen, jederzeit oder wenn Benutzer nicht mit dem lokalen Netzwerk verbunden sind.  

Weitere Informationen finden Sie unter [Sicherung auf Office 365 und anderen apps in Azure Active Directory verbunden](active-directory-conditional-access.md).


---

**Was ist die automatisierte Benutzerbereitstellung für SaaS-Apps?**

**A:** Azure Active Directory können Sie die Erstellung, Wartung und Entfernung von Identitäten in vielen gängigen Cloud (SaaS) automatisieren. 

Weitere Informationen finden Sie unter [automatisieren Benutzer und SaaS-Anwendung in Azure Active Directory Deprovisioning](active-directory-saas-app-provisioning.md)

---



