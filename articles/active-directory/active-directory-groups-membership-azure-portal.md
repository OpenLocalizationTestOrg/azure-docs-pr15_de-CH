<properties
    pageTitle="Ihre Gruppe Mitglied von Azure Active Directory Vorschau ist Gruppen verwalten | Microsoft Azure"
    description="Gruppen können andere Gruppen in Azure Active Directory enthalten. Hier ist die Mitgliedschaften verwalten."
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


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>Verwalten von Gruppen ist die Gruppe Mitglied in Azure Active Directory-Vorschau

Gruppen können andere Gruppen in Active Directory Azure Vorschau enthalten. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Hier ist die Mitgliedschaften verwalten.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Wie finde ich die Gruppen ist meine Gruppe Mitglied?

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2.  Wählen Sie **Weitere Dienste**, geben Sie im Textfeld **Benutzer und Gruppen** und wählen Sie dann die **EINGABETASTE**.

  ![Verwaltung öffnen](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Wählen Sie **alle Gruppen**, auf die **Benutzer und Gruppen** .

  ![Öffnen das Blade Gruppen](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Wählen Sie eine Gruppe auf die **Benutzer und Gruppen - alle** Blade.

5. Gruppieren Sie * *Group - *Gruppenname* ** select Blade **Mitgliedschaft **.

  ![Öffnen das Blatt Mitgliedschaften](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Wählen Sie zum Hinzufügen der Gruppe als Mitglied einer anderen Gruppe auf dem Blatt **Group - Gruppenmitgliedschaften** **Hinzufügen** .

7. Wählen Sie eine Gruppe aus dem Blade **Auswählen** , und wählen Sie die Schaltfläche **Wählen Sie** unten das Blade. Sie können nur eine Gruppe zu einem Zeitpunkt Ihrer Gruppe hinzufügen. **Das Feld** filtert die Anzeige anhand übereinstimmender Eintrag jeder Teil eines Benutzer oder Gerät. In diesem Feld sind keine Platzhalterzeichen akzeptiert.

  ![Gruppenmitgliedschaft hinzufügen](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Wählen Sie eine Gruppe um die Gruppe als Mitglied einer anderen Gruppe auf dem Blatt **Group - Gruppenmitgliedschaften** entfernen.

9. Blade ***Groupname*** wählen Sie **Entfernen** , und bestätigen Sie aufgefordert werden.

  ![Mitgliedschaft Befehl Entfernen](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Abschluss der Änderung der Gruppenmitgliedschaft für Ihre Gruppe wählen Sie **Speichern**.


## <a name="additional-information"></a>Weitere Informationen

Diese Artikel bieten zusätzliche Informationen zu Azure Active Directory.

* [Vorhandene Gruppen](active-directory-groups-view-azure-portal.md)
* [Erstellen einer neuen Gruppe und Hinzufügen von Mitgliedern](active-directory-groups-create-azure-portal.md)
* [Verwalten einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitglieder einer Gruppe](active-directory-groups-members-azure-portal.md)
* [Dynamische Regeln für Benutzer in einer Gruppe verwalten](active-directory-groups-dynamic-membership-azure-portal.md)
