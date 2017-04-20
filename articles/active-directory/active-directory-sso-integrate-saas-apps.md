<properties
    pageTitle="Einmaliges Anmelden für Azure Active Directory integrieren SaaS-apps |  Microsoft Azure"
    description="Authentifizierung für einzelne Zeichen und zentrale Verwaltung SaaS-Apps in Azure Active Directory-Bereitstellung zu aktivieren. Eine Übersicht zum Integrieren von Azure Active Directory SaaS-Apps."
    services="active-directory"
      keywords="SaaS-apps Azure AD integriert"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Einmaliges Anmelden für Azure Active Directory integrieren Sie SaaS-apps  

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-enterprise-apps-manage-sso.md)
- [Azure-Verwaltungsportal](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Zunächst einrichten einmaliges Anmelden für eine Anwendung, die Sie in Ihr Unternehmen bringen, werden Sie ein vorhandenes Verzeichnis in Azure Active Directory (Azure AD) verwenden. Ein Azure AD-Verzeichnis können, die durch Microsoft Azure, Office 365 und Windows Intune erhalten. Wenn Sie zwei oder mehr, finden Sie unter festzustellen welche [Azure AD-Verzeichnis verwalten](active-directory-administer.md) .

## <a name="authentication"></a>Authentifizierung

Für Applikationen unterstützen, die SAML 2.0, WS-Verbund oder OpenID verbinden Protokolle Azure Active Directory verwendet Signaturzertifikate Vertrauensstellungen einrichten. Weitere Informationen finden Sie unter [Verwalten von Zertifikaten für föderierte einmaliges Anmelden](active-directory-sso-certs.md).

Für Applikationen, die nur HTML-formularbasierte anmelden unterstützen, Azure Active Directory verwendet 'Kennwort Vaulting Vertrauensstellungen einrichten. Dadurch können die Benutzer in Ihrer Organisation eine SaaS-Anwendung von Azure AD mit Benutzerkontoinformationen SaaS-Anwendung werden automatisch angemeldet. Azure AD sammelt und speichert sichere Informationen für das Benutzerkonto und das zugehörige Kennwort. Weitere Informationen finden Sie unter [kennwortbasierte einmaliges Anmelden](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Autorisierung

Bereitgestellte Konto kann ein Benutzer berechtigt, eine Anwendung zu verwenden, nachdem sie durch einmaliges authentifiziert sind. Benutzerbereitstellung erfolgt manuell oder hinzufügen und Benutzerinformationen SaaS App basierend auf Änderungen in Azure Active Directory entfernen. Weitere Informationen zur Verwendung von vorhandenen Azure AD Connectors für automatisierte Bereitstellung finden Sie [automatisierte Bereitstellung und Aufheben der Bereitstellung für SaaS-Applikationen](active-directory-saas-app-provisioning.md).

Andernfalls können Sie manuell Benutzerinformationen zu einer Anwendung hinzufügen oder andere Bereitstellung Lösungen, die auf dem Markt verfügbar sind.

## <a name="access"></a>Zugriff

Azure AD bietet anpassbare verschiedene Programme für Endbenutzer in Ihrer Organisation bereitstellen. Sie sind bestimmte Bereitstellung oder Lösung nicht gesperrt. Können Sie [die Lösung, die Ihren Bedürfnissen am besten entspricht](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Zusätzliche Hinweise für Applikationen bereits verwendet

Einrichten von einmaliges für eine Anwendung, die Ihr Unternehmen wird einen anderen Prozess beim Erstellen neuer Konten für eine neue Anwendung. Es gibt ein paar vorbereitende Schritte einschließlich: Zuordnen von Benutzeridentitäten in der Anwendung zu Azure AD Identitäten und verstehen, wie Benutzer treten nach der Integration einer Anwendung anmelden.

> [AZURE.NOTE] Um SSO für eine vorhandene Anwendung einrichten, müssen Sie globale Administratorrechte in beiden Azure AD und SaaS-Anwendung.

### <a name="mapping-user-accounts"></a>Konten zuordnen

Identität des Benutzers ist normalerweise einen eindeutigen Bezeichner, der eine e-Mail-Adresse oder Benutzerprinzipalname (UPN) sein. Sie müssen Link (Map) des Benutzers Anwendungsidentität zu ihren jeweiligen Azure AD-Identität. Gibt es ein paar Methoden dazu je nach Anforderung der Anwendungsauthentifizierung.

Weitere Informationen zum Zuordnen von Anwendungsidentitäten mit Azure AD finden Sie unter [Anpassen von Ansprüchen im SAML-Token ausgestellt](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) und [Customizing Attribut Mappings für die Bereitstellung](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Grundlegendes zu Anmeldung Erfahrung

Wenn Sie SSO für eine Anwendung, die bereits verwendet wird integrieren, ist es wichtig zu erkennen, dass die Benutzeroberfläche betroffen sind. Für alle beginnt Benutzer mit ihrer Azure AD-Anmeldeinformationen anmelden. Es könnte auch sein, dass sie ein anderes Portal verwenden müssen, auf die Anwendung zugreifen.

SSO für einige Programme auf die Anwendung Schnittstelle, aber für andere Programme ausgeführt werden, der Benutzer muss ein zentrales Portal ([Meine Apps](http://myapps.microsoft.com) oder [Office365](http://portal.office.com/myapps)) durchlaufen. Erfahren Sie mehr über die verschiedenen Arten von SSO und der Nutzung im [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

Eine andere Ressource ist im [Leitfaden Entwickler](active-directory-applications-guiding-developers-for-lob-applications.md) Artikel *Suppressing Benutzer einverstanden* .

## <a name="next-steps"></a>Nächste Schritte


SaaS-Apps, die in der Galerie suchen, bietet Azure AD eine Reihe von [Lernprogrammen zur Integration SaaS-apps](active-directory-saas-tutorial-list.md).

Wenn app nicht Galerie ist, können Sie [der Galerie Azure AD App eine benutzerdefinierte Anwendung hinzugefügt](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

Mehr Details für alle diese Probleme in der Bibliothek Azure.com mit [wie Zugriff und single Sign-on Azure Active Directory ist.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Siehe auch

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
