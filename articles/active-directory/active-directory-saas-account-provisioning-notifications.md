<properties
    pageTitle="Konto Benachrichtigungen bereitstellen | Microsoft Azure"
    description="Erfahren Sie, wie Fragen zur benutzerbereitstellung benachrichtigt werden, die Ihre Aufmerksamkeit erfordern Konto Bereitstellung Benachrichtigung aktivieren."
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
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="account-provisioning-notifications"></a>Bereitstellen von Benachrichtigungen Konto

Mit Benutzer bereitstellen, können Benutzer im dritten SaaS Applikationen Verwaltung automatisieren. <br>
Obwohl dies automatisiert ist, ist die Interaktion mit diesen Prozess von Zeit zu Zeit erforderlich. <br>
Dies ist beispielsweise der Fall, bei das Kennwort des Kontos, das Sie mit Datenaustausch konfiguriert SaaS Anwendung abgelaufen ist. 

Konto Bereitstellung Benachrichtigung aktivieren, wird sicherstellen, dass Sie Fragen zur benutzerbereitstellung benachrichtigt, die Ihre Aufmerksamkeit erfordern.

Aktivieren oder deaktivieren Konto Benachrichtigungen als Teil des Benutzers bereitstellen Konfiguration für Dritte SaaS-Anwendung bereitstellen.

![Benutzer bereitstellen][1] 



Bereitstellen von Benachrichtigungen Konto aktivieren das zugehörige Kontrollkästchen auf der **Bestätigung** und geben Sie den e-Mail-Alias des Empfängers.

![Bereitstellen von Benachrichtigungen Konto][2]
 


Sie können eine Verteilerliste als Empfänger eingeben. Allerdings ist es wichtig zu beachten, dass die e-Mail-Benachrichtigung enthält Links zu Berichten, die nur von Administratoren Azure AD.

Haben Konto Bereitstellung Benachrichtigungen aktiviert erhalten Sie e-Mails über kritische Probleme, die benutzerbereitstellung zugeordnet sind. Um eine e-Mail zu vermeiden, erhalten Sie nur eine Benachrichtigung pro Tag für jede SaaS-Anwendung für e-Mail-Benachrichtigung aktiviert ist.


##<a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Benutzer Bereitstellung/aufheben der SaaS-Apps zu automatisieren](active-directory-saas-app-provisioning.md)
- [Anpassen der Attribute Mappings für Benutzer bereitstellen](active-directory-saas-customizing-attribute-mappings.md)
- [Schreiben von Ausdrücken für Attribute Mappings](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Bereichsfilter für Benutzer bereitstellen](active-directory-saas-scoping-filters.md)
- [So aktivieren Sie die automatische Bereitstellung von Benutzern und Gruppen aus dem Active Directory Azure Applications verwenden SCIM](active-directory-scim-provisioning.md)
- [Liste der Lernprogramme zur Integration SaaS-Apps](active-directory-saas-tutorial-list.md)



<!--Image references-->
[1]: ./media/active-directory-saas-account-provisioning-notifications/ic766307.png
[2]: ./media/active-directory-saas-account-provisioning-notifications/ic766308.png