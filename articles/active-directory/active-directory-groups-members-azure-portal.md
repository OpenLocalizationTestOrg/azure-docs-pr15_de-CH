<properties
    pageTitle="Verwaltung der Mitglieder einer Gruppe in Active Directory Azure Vorschau | Microsoft Azure"
    description="Wie Benutzer und Geräte, die Mitglieder einer Gruppe in Active Directory Azure"
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
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="manage-the-members-for-a-group-in-azure-active-directory-preview"></a>Verwaltung der Mitglieder einer Gruppe in Active Directory Azure-Vorschau

Erläutert, wie die Mitglieder einer Gruppe in Azure Active Directory (Azure AD) Vorschau. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md)

## <a name="how-do-i-find-the-members-and-manage-them"></a>Wie Mitglieder die und verwalten?

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2.  Wählen Sie **Weitere Dienste**, geben Sie im Textfeld **Benutzer und Gruppen** und wählen Sie dann die **EINGABETASTE**.

  ![Verwaltung öffnen](./media/active-directory-groups-members-azure-portal/search-user-management.png)

3.  Wählen Sie **alle Gruppen**, auf die **Benutzer und Gruppen** .

  ![Öffnen das Blade Gruppen](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)

4. Wählen Sie eine Gruppe auf die **Benutzer und Gruppen - alle** Blade.

5. In der * *Gruppe - *Groupname* ** select Blade **Mitglieder **.

  ![Öffnen das Blade Mitglieder](./media/active-directory-groups-members-azure-portal/view-group-members.png)

6. Um Mitglieder der Gruppe auf dem Blatt **Group - Mitglieder** hinzufügen wählen Sie **Mitglieder hinzufügen**.

  ![Member-Befehl hinzufügen](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)

7. Blade **Mitglieder** wählen Sie eine oder mehr Benutzern oder Geräten der Gruppe und wählen die Schaltfläche **Wählen Sie** unten das Blade der Gruppe hinzu aus. **Das Feld** filtert die Anzeige anhand übereinstimmender Eintrag jeder Teil eines Benutzer oder Gerät. In diesem Feld sind keine Platzhalterzeichen akzeptiert.

8. Wählen Sie ein Element, um Mitglieder aus der Gruppe auf dem Blatt **Group - Mitglieder** entfernen.

9. Blade ***Membername*** wählen Sie **Entfernen** , und bestätigen Sie aufgefordert werden.

  ![Member-Befehl Entfernen](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)

9. Wenn Sie Mitglieder der Gruppe ändern haben, wählen Sie **Speichern**.


## <a name="additional-information"></a>Weitere Informationen

Diese Artikel bieten zusätzliche Informationen zu Azure Active Directory.

* [Vorhandene Gruppen](active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)
* [Verwalten einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten von Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Dynamische Regeln für Benutzer in einer Gruppe verwalten](active-directory-groups-dynamic-membership-azure-portal.md)
