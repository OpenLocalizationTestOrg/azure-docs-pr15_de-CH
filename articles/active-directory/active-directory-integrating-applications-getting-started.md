<properties
   pageTitle="Azure Active Directory mit erste integrieren Schritte |  Microsoft Azure"
   description="Dieser Artikel ist eine erste Schritte zur Integration von Azure Active Directory (AD) und lokale Applikationen Cloudanwendungen."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory mit erste integrieren Schritte
## <a name="overview"></a>Übersicht
Dieses Thema soll Ihnen einen Wegweiser für die Integration von Applikationen mit Azure Active Directory (AD). Jeder der folgenden Abschnitte enthalten eine kurze Zusammenfassung eines Themas detailliertere Sie erkennen können, welche Teile der erste Schritte für Sie relevant sind.  Hyperlinks für eingehendere Informationen zu den einzelnen Themen.

## <a name="before-you-begin-take-inventory"></a>Bevor Sie beginnen, Bestandsaufnahme
Bevor Sie springen, Azure AD Anwendung integrieren, ist es wichtig zu wissen, wo Sie sind und wo Sie wollen.  Die folgenden Fragen sollen Sie denken Azure AD-Integration hinzu.

### <a name="application-inventory"></a>Anwendungsbestand
- Wo sind die Komponenten? Die Eigentümer
- Welche Art der Authentifizierung benötigen Ihre Anwendung?
- Wer benötigt Zugriff auf die Programme?
- Möchten Sie eine neue Anwendung bereitstellen?
  - Sie erstellen es nun innerbetrieblich und Bereitstellen einer Azure Compute-Instanz?
  - Werden Sie einen verwenden, die in der Galerie Azure?

### <a name="user-and-group-inventory"></a>Benutzer- und Lager
- Wo wohnen die Benutzerkonten?
 - Lokale Active Directory
 - Azure AD
 - In einer anderen Anwendung, die Sie besitzen
 - In-Anwendung
 - Alle oben genannten
- Welche Berechtigungen und Rollen zugewiesen haben Benutzer derzeit? Müssen Sie den Zugriff überprüfen, oder sind Sie sicher, dass die Benutzerarbeitsaufträge Zugriff und Rolle entsprechende jetzt?
- Sind Gruppen in der lokalen Active Directory bereits?
 - Aufbau von Gruppen
 - Die Mitglieder der Gruppe sind
 - Welche Rolle Berechtigungen Aufgaben haben die Gruppen derzeit?
- Müssen Sie vor der Integration von Benutzer/Gruppe Datenbanken bereinigen?  (Dies ist eine sehr wichtige Frage. Garbage im Müll.)

### <a name="access-management-inventory"></a>Access Management Lager
- Wie verwalten Sie aktuell Benutzerzugriff auf Programme? Braucht ändern?  Haben Sie dazu Access, wie z. B. mit [RBAC](role-based-access-control-configure.md) verwalten berücksichtigt?
- Wer benötigt Zugriff auf was?

Nicht haben Sie die Antworten auf diese Fragen voraus, aber das ist normal.  Dieses Handbuch hilft Ihnen einige Fragen zu beantworten und einige Entscheidungen.

## <a name="prerequisites"></a>Erforderliche Komponenten
- Ein Azure-Abonnement und eine Azure Active Directory-Verzeichnisdienst.  Wenn Sie bereits ein Azure-Abonnement haben, können Sie Azure kostenlos für 30 Tage ausprobieren. [Probieren Sie es aus!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Anwendungsintegration in Azure AD
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Suchen von mangelnder Cloudanwendungen mit Cloud App Discovery
Wie bereits erwähnt, möglicherweise Programme, die von Ihrem Unternehmen bisher verwaltet wurde noch nicht.  Als Teil der Inventory-Prozess kann Cloudanwendungen finden. Finden Sie [Cloudanwendungen mit Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Authentifizierung
Jede Anwendung möglicherweise unterschiedliche Authentifizierung. Azure AD Signaturzertifikate mit dienen, die SAML 2.0, WS-Federation OpenID verbinden Protokolle sowie Kennwort einmaliges Anmelden verwenden. Weitere Informationen zur Anwendung finden Sie unter Authentifizierungstypen mit Azure AD [Verwalten von Zertifikaten für verbundene einmaliges Anmelden in Azure Active Directory](active-directory-sso-certs.md) und [Kennwort einmaliges auf](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Aktivieren von SSO Azure AD App-Proxy
Mit Microsoft Azure AD-Anwendungsproxy bieten Sie Zugriff auf Programme befindet sich innerhalb des privaten Netzwerks sicher, überall und auf jedem Gerät. Nach der Installation eines Anwendung Proxy Connectors in Ihrer Umgebung kann es problemlos in Azure AD konfiguriert werden.

### <a name="integrating-applications-with-azure-ad"></a>Azure AD integrieren applications
Die folgenden Artikeln unterschiedlich Applikationen Azure AD integriert und bieten einige.

- [Bestimmen die Active Directory verwenden](active-directory-administer.md)
- [In der Galerie Azure-Anwendung verwenden](active-directory-appssoaccess-whatis.md)
- [Integration von SaaS Anwendungsliste Lernprogramme](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Verwalten des Zugriffs auf Programme
Die folgenden Artikel beschreiben Weise Zugriff auf Programme nach Azure AD mit Azure AD Connectors mit Azure AD integriert verwalten.

- [Verwalten des Zugriffs auf apps mit Azure AD](active-directory-managing-access-to-apps.md)
- [Automatisieren mit Azure Active Directory Connectors](active-directory-saas-app-provisioning.md)
- [Zuweisen von Benutzern zu einer Anwendung](active-directory-applications-guiding-developers-assigning-users.md)
- [Zuweisen von Gruppen zu einer Anwendung](active-directory-applications-guiding-developers-assigning-groups.md)
- [Gemeinsam verwendete Konten](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integrieren von benutzerdefinierten Programmen
Wenn Sie eine neue Anwendung schreiben, Entwickler bei der Nutzung Azure AD möchten anzeigen Sie [Leitfaden Entwickler](active-directory-applications-guiding-developers-for-lob-applications.md)

Ihre benutzerdefinierte Anwendung der Azure Galerie hinzufügen, finden Sie unter ["Bringen Ihre Anwendung" Azure AD Self SAML-Konfiguration](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Siehe auch

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
