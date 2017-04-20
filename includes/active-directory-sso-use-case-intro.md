Unternehmen verwenden mehr [Software als Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) Programme für Produktivität da Cloud-Technologie und Tools werden mehr verfügbar. Mit steigender Anzahl der SaaS-apps wird es schwierig für Administratoren Konten und Zugriffsrechte und die Benutzer ihre Passwörter merken. Diese Programme einzeln erstellt zusätzliche Arbeit und ist weniger sicher.


- Mitarbeiter, die viele Kennwörter auf eher weniger sichere Methoden daran, Kennwörter aufschreiben oder viele Konten die gleichen Kennwörter verwenden.

- Beim Eintreffen eines neuen Mitarbeiters oder eine müssen ihrer Konten einzeln bereitgestellt oder Ihre Bereitstellung.

- Darüber hinaus können Mitarbeiter mit SaaS-apps für ihre Arbeit ohne Starten IT, d. h. erstellen sie ihre eigenen Konten auf Systemen, die IT-Administratoren noch nicht genehmigt und werden nicht überwacht.  

Einmaliges Anmelden (SSO) ist eine Lösung für diese Herausforderung. Es ist am einfachsten verwalten mehrere apps und Benutzer konsistent anmelden. Azure Active Directory (Azure AD) bietet eine robuste SSO-Lösung und verfügbare integrierte häufig mit Lernprogramme für Administratoren schnell eine neue Anwendung und Bereitstellung von Benutzern gestartet wurde.


## <a name="how-does-azure-active-directory-integrate-apps"></a>Wie ist Azure Active Directory apps integriert?  

Azure AD können Sie Ihre apps und bereitgestellte Konten. Dies kann durch zwei Vorgehensweisen erfolgen.

- Ist die Anwendung integrierte App Katalog gelangen Sie über dieses Portal apps und Einstellungen um SSO zu ermöglichen. Für jede Anwendung Gallery können Sie loslegen führen Sie durch einfache Anleitung in der Galerie und in Azure-Portal-auf aktivieren.

- Ist die Anwendung nicht in der Galerie können Sie die meisten apps noch in Azure AD als benutzerdefinierte Anwendung einrichten. Dies erfordert etwas mehr Fachwissen zu konfigurieren. Sie können jede Anwendung, die als verbundenen Anwendung SAML 2.0 unterstützt oder eine Anwendung, die eine HTML-basierte Anmeldeseite als Kennwort SSO-Anwendung hinzufügen.

In dem Fall, in dem Benutzer ihre eigenen Konten für SaaS-apps, die nicht von verwaltet erstellt, IT [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) -Tool bietet eine Lösung. Dieses Programm überwacht Webverkehr zur Identifizierung der Organisation und die Anzahl der Personen, die jeweils die apps verwendet werden. IT können diese Informationen apps erfahren Benutzer bevorzugen und entscheiden, welche Azure AD für SSO integrieren.  

Bei der Integration einer Anwendung in Azure AD zuzuordnen die Benutzer festgelegten Anwendungsidentitäten Ihrer Azure AD Identitäten.  
