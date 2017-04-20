<properties
    pageTitle="Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in Azure Active Directory Vorschau | Microsoft Azure"
    description="Erläutert, wie Benutzer hinzufügen oder Ändern von Benutzerinformationen in Azure Active Directory, einschließlich externe und Gastanwender."
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

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in Azure Active Directory-Vorschau

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-users-create-external-azure-portal.md)
- [Azure-Verwaltungsportal](active-directory-create-users-external.md)

Hinzufügen von Benutzern aus anderen Verzeichnissen in Azure Active Directory (Azure AD) Vorschau oder Partnerunternehmen erläutert. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md) Informationen zum Hinzufügen neuer Benutzer in Ihrer Organisation und Hinzufügen von Benutzer Konten Microsoft anzeigen Sie [Azure Active Directory neue Benutzer](active-directory-users-create-azure-portal.md) Hinzugefügte Benutzer nicht standardmäßig über Administratorberechtigungen verfügen, aber Sie können Rollen können jederzeit.

## <a name="add-a-user"></a>Hinzufügen eines Benutzers

1.  Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.

2.  Wählen Sie **Weitere Dienste**, geben Sie im Textfeld **Benutzer und Gruppen** und wählen Sie dann die **EINGABETASTE**.

    ![Verwaltung öffnen](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  Wählen Sie auf die **Benutzer und Gruppen** **Benutzer aus**, und wählen Sie **Hinzufügen**.

    ![Befehl hinzufügen](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. Blatt **Benutzer** bieten Sie einen Anzeigenamen in der **Namen** und der Anmeldung Benutzername **Benutzernamen**ein.

5. Kopieren Sie oder sonst Notieren Sie das generierte Kennwort, sodass Sie es für den Benutzer bereitstellen können, nachdem dieser Vorgang abgeschlossen ist.

6. Optional Wählen Sie **Profil** der Benutzer zuerst und name und Berufsbezeichnung einen Abteilungsnamen.
    
    ![Öffnen das Benutzerprofil](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - Wählen Sie **Gruppen** eine oder mehrere Gruppen Benutzer hinzufügen.

        ![Hinzufügen eines Benutzers zu Gruppen](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - Wählen Sie **organisatorischen Rollen** eine Rolle aus der Liste **Rollen** Benutzer zuweisen. Weitere Informationen zu Benutzer- und Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md).

        ![Zuweisen einer Rolle zu einen Benutzer](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Wählen Sie **Erstellen**.

8. Verteilen Sie das generierte Kennwort für den neuen Benutzer sicher, dass der Benutzer sich anmelden kann.

> [AZURE.IMPORTANT] Wenn Ihre Organisation mehr als eine Domäne verwendet, sollten Sie beim Hinzufügen eines Benutzerkontos zu folgenden Themen wissen:
>
> - Mit der gleichen Benutzerprinzipalname (UPN) in Domänen hinzufügen, um **erste** hinzuzufügen, z. B. geoffgrisso@contoso.onmicrosoft.com, **gefolgt von** geoffgrisso@contoso.com.
> - Fügen Sie **nicht** geoffgrisso@contoso.com vor dem Hinzufügen von geoffgrisso@contoso.onmicrosoft.com. Diese Reihenfolge ist wichtig und kann umständlich rückgängig machen.

Wenn Sie Informationen für einen Benutzer ändern, deren Identität mit lokalen Active Directory-Dienst synchronisiert wird, kann nicht die Benutzerinformationen im klassischen Azure-Portal ändern. Um die Benutzerinformationen zu ändern, verwenden Sie Ihre lokale Active Directory-Verwaltungstools.


## <a name="whats-next"></a>Was kommt als nächstes

- [Hinzufügen eines Benutzers](active-directory-users-create-azure-portal.md)
- [Zurücksetzen des Kennworts eines Benutzers im neuen Azure-portal](active-directory-users-reset-password-azure-portal.md)
- [Zuweisen eines Benutzers zu einer Rolle in Azure AD](active-directory-users-assign-role-azure-portal.md)
- [Ändern der Informationen eines Benutzers Arbeit](active-directory-users-work-info-azure-portal.md)
- [Verwalten von Benutzerprofilen](active-directory-users-profile-azure-portal.md)
- [Löschen eines Benutzers in Azure AD](active-directory-users-delete-user-azure-portal.md)
