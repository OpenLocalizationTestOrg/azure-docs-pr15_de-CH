<properties
    pageTitle="Einrichten von Azure Active Directory für Self-Service-anwendungsverwaltung | Microsoft Azure"
    description="Benutzerverantwortliche Verwaltung ermöglicht das Erstellen und Verwalten von Sicherheitsgruppen oder Office 365-Gruppen in Azure Active Directory und bietet Benutzern die Möglichkeit Anforderung Sicherheitsgruppe oder Gruppenmitgliedschaften Office 365"
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/10/2016"
    ms.author="curtand"/>

# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Einrichten von Azure Active Directory für benutzerverantwortliche Verwaltung

Benutzerverantwortliche Verwaltung ermöglicht das Erstellen und Verwalten von Sicherheitsgruppen oder Office 365-Gruppen in Azure Active Directory (Azure AD). Benutzer können auch anfordern, Gruppe oder Office 365 Gruppenmitgliedschaften und der Besitzer der Gruppe kann dann genehmigen oder ablehnen Mitgliedschaft. Auf diese Weise kann Kontrolle der Gruppenmitgliedschaft Personen delegiert werden, Business-Kontext für diese Mitgliedschaft verstehen. Gruppe Self-service-Management-Funktionen stehen nur für Sicherheitsgruppen und Office 365 Gruppen jedoch nicht für e-Mail-aktivierte Gruppen oder Verteilerlisten.

Benutzerverantwortliche Verwaltung umfasst derzeit zwei wichtige Szenarien: delegierte Verwaltung und benutzerverantwortliche Verwaltung.

- **Delegiert Verwaltung** 
   Beispiel ist ein Administrator, der Zugriff auf eine SaaS-Anwendung verwaltet das Unternehmen. Verwalten von Berechtigungen wird umständlich, dieser Administrator bittet der Eigentümer eine neue Gruppe erstellen. Der Administrator weist Access für die Anwendung in die neue Gruppe und der Gruppe alle Personen, die bereits Zugriff auf die Anwendung hinzugefügt. Der Eigentümer kann dann weitere Benutzer hinzufügen und diese Benutzer automatisch der Anwendung bereitgestellt. Der Eigentümer muss nicht warten, bis der Administrator Zugriff für Benutzer verwalten. Administrator Manager in eine andere Gruppe die gleiche Berechtigungen erteilt, kann diese Person auch für ihre eigenen Benutzer verwalten. Weder der Eigentümer der Manager kann anzeigen und die Benutzer verwalten. Der Administrator sieht noch alle Benutzer mit Zugriff auf die Anwendung und die Zugriffsrechte Block bei Bedarf.

- **Benutzerverantwortliche Verwaltung** 
   ist ein Beispiel für dieses Szenario zwei Benutzer, SharePoint-Websites, die unabhängig voneinander eingerichtet haben. Sie möchten die Teams auf ihre Websites zugreifen. Zu diesem Zweck erstellen sie eine Gruppe in Azure AD und in SharePoint Online wählt jede Gruppe Zugriff auf Websites. Wenn jemand Zugriff sie aus der Anforderung und genehmigte sie erhalten Zugriff auf beide SharePoint-Websites automatisch. Später entscheidet davon, dass alle Personen, die Zugriff auf die Website auch auf eine bestimmte SaaS-Anwendung sollte. Der Administrator der SaaS-Anwendung kann Zugriffsrechte für die Anwendung der SharePoint Online-Website hinzufügen. Danach Anfragen genehmigt ermöglicht den Zugriff auf SharePoint Online Standorte und SaaS Anwendung.

## <a name="making-a-group-available-for-end-user-self-service"></a>Erstellen einer Gruppe für Endbenutzer Self-service

1. Öffnen Sie in [Azure-Verwaltungsportal](https://manage.windowsazure.com)Azure AD-Verzeichnis.

2. Legen Sie auf der Registerkarte **Konfigurieren** **delegierte Verwaltung** aktiviert.

3. **Benutzer können Sicherheitsgruppen** oder **Benutzer können Office Gruppen erstellen** auf aktiviert festgelegt.

Wenn **Benutzer Sicherheitsgruppen erstellen** aktiviert ist, dürfen alle Benutzer im Verzeichnis neue Sicherheitsgruppen erstellen und diesen Gruppen Mitglieder hinzufügen. Diese neuen Gruppen werden auch im Zugriff für alle Benutzer angezeigt. Wenn die Einstellung für die Gruppe zulässig ist, können andere Benutzer zu diesen Gruppen erstellen. **Benutzer können Sicherheitsgruppen** deaktiviert ist, Benutzer können keine Gruppen erstellen und vorhandene Gruppen, deren Eigentümer Sie sind, nicht geändert. Sie können weiterhin die Mitgliedschaften dieser Gruppen verwalten und Anfragen von anderen Benutzern zu ihren Gruppen genehmigen.

**Self-Service für Sicherheitsgruppen verwenden können Benutzer** können Sie um eine differenziertere Zugangskontrolle für benutzerverantwortliche Verwaltung für die Benutzer zu erreichen. Wenn **Benutzer Gruppen erstellen** aktiviert ist, dürfen alle Benutzer im Verzeichnis neue Gruppen erstellen und diesen Gruppen Mitglieder hinzufügen. Einige Einstellung **Benutzer Self-Service für Sicherheitsgruppen verwenden können** , sind Sie beschränken Gruppe eine begrenzte Gruppe von Benutzern. Wenn dieser Schalter auf einige müssen Sie der Gruppe SSGMSecurityGroupsUsers Benutzer hinzufügen, bevor sie neue Gruppen erstellen und Mitglieder hinzufügen. **Self-Service für Sicherheitsgruppen verwenden können Benutzer** auf alle festlegen, können Sie alle Benutzer in Ihrem Verzeichnis zum Erstellen neuer Gruppen.

**Die Self-Service für Sicherheitsgruppen verwenden können** Feld können Sie einen benutzerdefinierten Namen für eine Gruppe angeben, deren Member Self-Service verwenden können.

## <a name="additional-information"></a>Weitere Informationen

Diese Artikel bieten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)

* [Azure Active Directory-Cmdlets für die Gruppe konfigurieren](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)

* [Was ist Azure Active Directory?](active-directory-whatis.md)

* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
