<properties
    pageTitle="Entfernen Sie Benutzer oder Gruppen Zuweisung aus einer Enterprise-Anwendung in Azure Active Directory Vorschau | Microsoft Azure"
    description="Access-Zuordnung eines Benutzers oder einer Gruppe aus einer Enterprise-Anwendung in Azure Active Directory entfernen"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Entfernen eines Benutzers oder die Zuordnung aus einer Enterprise-Anwendung in Azure Active Directory-Vorschau

Es ist einfach zu einem Benutzer oder einer Gruppe Zugriff auf eine Enterprise-Anwendung in Azure Active Directory (Azure AD) Vorschau zugewiesen wird. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Sie müssen die entsprechenden Berechtigungen zum Verwalten von Enterprise-Anwendung. In der aktuellen Vorschau müssen Sie globaler Administrator für das Verzeichnis sein.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Wie entferne ich einen Benutzer oder eine Gruppe zuweisen?

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2. Wählen Sie **Weitere Dienste**, geben Sie **Azure Active Directory** in das Textfeld ein und Sie dann die **EINGABETASTE**.

3. Der *Verzeichnisname *Azure Active Directory - ** ** Blade (also den Azure AD Blade für das Verzeichnis, das Sie verwalten), wählen Sie **Enterprise Applications **.

    ![Enterprise-apps öffnen](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Blade **Anwendung** wählen Sie **Alle Programme**. Sie sehen eine Liste der apps verwalten können.

5. Blade **NTD - alle** Aktivieren einer Anwendung.

6. ***Appname*** Blade (Blade mit dem Namen der ausgewählten Anwendung im Titel) Wählen Sie **Benutzer und Gruppen**.

    ![Auswählen von Benutzern oder Gruppen](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Auf der ***Anwendungsname*** **-Benutzer & Zuordnung** Blade weitere Benutzer oder Gruppen auswählen, und wählen Sie dann den Befehl **Entfernen** . Bestätigen Sie aufgefordert werden.

    ![Befehl Entfernen](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Nächste Schritte

- [Alle meine Gruppen](active-directory-groups-view-azure-portal.md)
- [Eine Enterprise-Anwendung einen Benutzer oder eine Gruppe zuweisen](active-directory-coreapps-assign-user-azure-portal.md)
- [Deaktivieren von User anmelden für eine Enterprise-Anwendung](active-directory-coreapps-disable-app-azure-portal.md)
- [Ändern des Namens oder Logos eine Enterprise-Anwendung](active-directory-coreapps-change-app-logo-user-azure-portal.md)
