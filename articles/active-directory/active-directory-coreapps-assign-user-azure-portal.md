<properties
    pageTitle="Eine Enterprise-Anwendung in Azure Active Directory Vorschau einen Benutzer oder eine Gruppe zuweisen | Microsoft Azure"
    description="Eine Enterprise-Anwendung einen Benutzer oder eine Gruppe in Azure Active Directory zugewiesen auswählen"
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
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Eine Enterprise-Anwendung in Azure Active Directory Vorschau einen Benutzer oder eine Gruppe zuweisen

Es ist einfach, Ihre Anwendung in Azure Active Directory (Azure AD) Vorschau einen Benutzer oder eine Gruppe zuweisen. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Sie müssen die entsprechenden Berechtigungen zum Verwalten von Enterprise-Anwendung. In der aktuellen Vorschau müssen Sie globaler Administrator für das Verzeichnis sein.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Wie wird eine Enterprise-Anwendung zuweisen Zugriff?

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2. Wählen Sie **Weitere Dienste**, geben Sie Azure Active Directory in das Textfeld ein und Sie dann die **EINGABETASTE**.

3. Der *Verzeichnisname *Azure Active Directory - ** ** Blade (also den Azure AD Blade für das Verzeichnis, das Sie verwalten), wählen Sie **Enterprise Applications **.

    ![Enterprise-apps öffnen](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. Blade **Anwendung** wählen Sie **Alle Programme**. Sie sehen eine Liste der apps verwalten können.

5. Blade **NTD - alle** Aktivieren einer Anwendung.

6. ***Appname*** Blade (Blade mit dem Namen der ausgewählten Anwendung im Titel) Wählen Sie **Benutzer und Gruppen**.

    ![Alle Befehl Anträge](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. Auf der ***Anwendungsname*** **-Benutzer & Zuordnung** Blade, wählen Sie den Befehl **Hinzufügen** .

8. Wählen Sie auf die **Zuweisung hinzufügen** **Benutzer und Gruppen**.

    ![Die Anwendung einen Benutzer oder eine Gruppe zuweisen](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. Wählen Sie auf die **Benutzer und Gruppen** einen oder mehrere Benutzer oder Gruppen aus und wählen Sie die Schaltfläche **Wählen Sie** unten das Blade.

10. Wählen Sie auf der **Aufgabe hinzufügen** **Rolle**. Wählen Sie auf dem Blatt **Rolle auswählen** eine Rolle für die ausgewählten Benutzer oder Gruppen gelten, und wählen Sie dann die Schaltfläche " **OK** " am unteren Rand der Blade.

11. -Blade **Zuweisung hinzufügen** Schaltfläche **weisen** am unteren Rand das Blade. Zugewiesene Benutzer oder Gruppen müssen Enterprise App der ausgewählten Rolle definierten Berechtigungen.

## <a name="next-steps"></a>Nächste Schritte

- [Alle meine Gruppen](active-directory-groups-view-azure-portal.md)
- [Entfernen Sie Benutzer oder Gruppen Zuweisung aus einer Enterprise-Anwendung](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Deaktivieren von User anmelden für eine Enterprise-Anwendung](active-directory-coreapps-disable-app-azure-portal.md)
- [Ändern des Namens oder Logos eine Enterprise-Anwendung](active-directory-coreapps-change-app-logo-user-azure-portal.md)
