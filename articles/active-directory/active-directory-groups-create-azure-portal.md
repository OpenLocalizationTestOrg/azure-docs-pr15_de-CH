<properties
    pageTitle="Erstellen einer neuen Gruppe in Azure Active Directory Vorschau | Microsoft Azure"
    description="Zum Erstellen einer Gruppe in Azure Active Directory und fügen Sie der Gruppe Benutzer (Mitglieder)"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Erstellen einer neuen Gruppe in Azure Active Directory-Vorschau

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-groups-create-azure-portal.md)
- [Azure-Verwaltungsportal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Erläutert das Erstellen und Auffüllen einer neuen Gruppe in der Vorschau Azure Active Directory (Azure AD). [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Verwenden Sie eine Gruppe, um Verwaltungsaufgaben wie Lizenzen oder Berechtigungen auf Benutzer oder Geräte gleichzeitig zuweisen.

## <a name="how-do-i-create-a-group"></a>Erstellen eine Gruppe

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2. Wählen Sie **Weitere Dienste**, geben Sie im Textfeld **Benutzer und Gruppen** und wählen Sie dann die **EINGABETASTE**.

  ![Verwaltung öffnen](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Wählen Sie **alle Gruppen**, auf die **Benutzer und Gruppen** .

  ![Öffnen das Blade Gruppen](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Wählen Sie auf dem Blatt **Benutzer und Gruppen - alle** **Hinzufügen** .

  ![Befehl hinzufügen](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. **Das Blatt** fügen Sie einen Namen und eine Beschreibung für die Gruppe hinzu.

6. Wählen Sie zur Gruppe hinzufügen wählen Sie **zugewiesen** im Feld **Mitgliedschaft** , und wählen Sie **Mitglieder**. Weitere Informationen zum Verwalten der Mitgliedschaft einer Gruppe dynamisch finden Sie unter [Using Attribute erweiterte Regeln für Gruppenmitgliedschaft erstellen](active-directory-groups-dynamic-membership-azure-portal.md).

  ![Mitglieder hinzufügen](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Blade **Mitglieder** wählen Sie eine oder mehr Benutzern oder Geräten der Gruppe und wählen die Schaltfläche **Wählen Sie** unten das Blade der Gruppe hinzu aus. **Das Feld** filtert die Anzeige anhand übereinstimmender Eintrag jeder Teil eines Benutzer oder Gerät. In diesem Feld sind keine Platzhalterzeichen akzeptiert.

6. Wenn Sie Mitglieder der Gruppe hinzugefügt haben, wählen Sie auf **dem Blatt** **Erstellen** .    

  ![Erstellen der Gruppe bestätigen](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>Weitere Informationen

Diese Artikel bieten zusätzliche Informationen zu Azure Active Directory.

* [Vorhandene Gruppen](active-directory-groups-view-azure-portal.md)
* [Verwalten einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitglieder einer Gruppe](active-directory-groups-members-azure-portal.md)
* [Verwalten von Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Dynamische Regeln für Benutzer in einer Gruppe verwalten](active-directory-groups-dynamic-membership-azure-portal.md)
