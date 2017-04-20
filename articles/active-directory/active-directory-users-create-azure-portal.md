<properties
    pageTitle="Hinzufügen neuer Benutzer zu Active Directory Azure Vorschau | Microsoft Azure"
    description="Erläutert, wie neue Benutzer hinzufügen oder Ändern von Benutzerinformationen in Azure Active Directory."
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


# <a name="add-new-users-to-azure-active-directory-preview"></a>Fügen Sie neuer Benutzer zu Active Directory Azure-Vorschau hinzu

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-users-create-azure-portal.md)
- [Azure-Verwaltungsportal](active-directory-create-users.md)

Hinzufügen neuer Benutzer in Ihrer Organisation in der Vorschau Azure Active Direstory (Azure AD) erläutert. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md)

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2.  Wählen Sie **Weitere Dienste**, geben Sie im Textfeld **Benutzer und Gruppen** und wählen Sie dann die **EINGABETASTE**.

    ![Verwaltung öffnen](./media/active-directory-users-create-azure-portal/create-users-user-management.png)

3.  Wählen Sie auf die **Benutzer und Gruppen** **alle Benutzer**, und wählen Sie **Hinzufügen**.

    ![Befehl hinzufügen](./media/active-directory-users-create-azure-portal/create-users-add-command.png)

4.  Geben Sie die Details des Benutzers wie **Name** und **Benutzername**. Namen Domänenteil des Benutzernamens muss entweder der ersten Domäne "foo.onmicrosoft.com" Standarddomänenname oder verifiziert, nicht verbundene Domänennamen wie "contoso.com".

5. Kopieren Sie oder sonst Notieren Sie das generierte Kennwort, sodass Sie es für den Benutzer bereitstellen können, nachdem dieser Vorgang abgeschlossen ist.

6. Optional können Sie öffnen und geben Sie die Informationen im Blade- **Profil** , Blade- **Gruppen** oder Blade **verzeichnisrolle** für den Benutzer. Weitere Informationen zu Benutzer- und Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md).

7.  Wählen Sie auf dem Blatt **Benutzer** **Erstellen**.

8. Verteilen Sie das generierte Kennwort für den neuen Benutzer sicher, dass der Benutzer sich anmelden kann.

## <a name="whats-next"></a>Was kommt als nächstes

- [Einen externen Benutzer hinzufügen](active-directory-users-create-external-azure-portal.md)
- [Zurücksetzen des Kennworts eines Benutzers im neuen Azure-portal](active-directory-users-reset-password-azure-portal.md)
- [Ändern der Informationen eines Benutzers Arbeit](active-directory-users-work-info-azure-portal.md)
- [Verwalten von Benutzerprofilen](active-directory-users-profile-azure-portal.md)
- [Löschen eines Benutzers in Azure AD](active-directory-users-delete-user-azure-portal.md)
- [Zuweisen eines Benutzers zu einer Rolle in Azure AD](active-directory-users-assign-role-azure-portal.md)
