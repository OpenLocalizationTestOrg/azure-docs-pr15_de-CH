<properties
    pageTitle="Hinzufügen neuer Benutzer zu Active Directory Azure | Microsoft Azure"
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
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Fügen Sie neuer Benutzer oder Benutzer mit Microsoft Azure Active Directory hinzu

Hinzufügen von Benutzern, um das Verzeichnis zu füllen. Dieser Artikel beschreibt hinzufügen neue Benutzer in Ihrer Organisation sowie zum Hinzufügen von Benutzern, die Microsoft-Konten. Weitere Informationen zum Hinzufügen von Benutzern aus anderen Verzeichnissen in Azure Active Directory oder Hinzufügen von Benutzern von Partnerunternehmen finden Sie unter [Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in Azure Active Directory](active-directory-create-users-external.md). Hinzugefügte Benutzer nicht standardmäßig über Administratorberechtigungen verfügen, aber Sie können Rollen können jederzeit.

## <a name="add-a-user"></a>Hinzufügen eines Benutzers

1. Melden Sie sich im [klassischen Azure-Portal](https://manage.windowsazure.com) mit einem Konto kein globaler Administrator für das Verzeichnis.
2. Wählen Sie **Active Directory**, und wählen Sie den Namen des Verzeichnisses Organisation.
3. Wählen Sie die Registerkarte **Benutzer** , und wählen Sie in der Befehlsleiste **Benutzer hinzufügen**.
4. Wählen Sie auf der Seite **Erzählen über diese Benutzer** unter **Benutzer**entweder:

    - **Neue Benutzer in Ihrer Organisation** – Fügt ein neues Benutzerkonto im Verzeichnis.
    - **Benutzer mit einem microsoftkonto** – Fügt ein Konto Microsoft Consumer zum Verzeichnis (z. B. ein Outlook-Konto)

5. Je nach **Typ des Benutzers**Geben Sie einen Benutzernamen ein (neuer Benutzer) oder eine e-Mail-Adresse (für einen Benutzer mit einem microsoftkonto).
6. Geben Sie auf **der Profilseite des Benutzers** vor- und Nachnamen ein, ein Anzeigename und eine Benutzerrolle aus **Rollen** . Weitere Informationen zu Benutzer- und Rollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md). Angeben, ob **Mehrfaktor-Authentifizierung aktivieren** für den Benutzer.
7. Wählen Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

> [AZURE.IMPORTANT] Wenn Ihre Organisation mehr als eine Domäne verwendet, sollten Sie beim Hinzufügen eines Benutzerkontos zu folgenden Themen wissen:
>
> - Mit der gleichen Benutzerprinzipalname (UPN) in Domänen hinzufügen, um **erste** hinzuzufügen, z. B. geoffgrisso@contoso.onmicrosoft.com, **gefolgt von** geoffgrisso@contoso.com.
> - Fügen Sie **nicht** geoffgrisso@contoso.com vor dem Hinzufügen von geoffgrisso@contoso.onmicrosoft.com. Diese Reihenfolge ist wichtig und kann umständlich rückgängig machen.

## <a name="change-user-information"></a>Ändern von Benutzerinformationen

Sie können alle Attribute Benutzer außer die Objekt-ID.

1. Öffnen Sie das Verzeichnis.
2. Wählen Sie die Registerkarte **Benutzer** , und wählen Sie den Anzeigenamen des Benutzers, den Sie ändern möchten.
3. Führen Sie ändern und dann auf **Speichern**.

Wenn Benutzer, den Sie ändern möchten, mit lokalen Active Directory-Dienst synchronisiert wird, kann nicht mit diesem Verfahren Benutzerinformationen ändern. Verwenden Sie zum Ändern des Benutzers der lokalen Active Directory-Verwaltungstools.

## <a name="guest-user-management-and-limitations"></a>Gast Verwaltung und Grenzen

Gastkonten sind Benutzer aus anderen Verzeichnissen, die in das Verzeichnis auf SharePoint-Dokumente, Programme oder andere Azure Ressourcen eingeladen wurden. Ein Gastkonto im Verzeichnis hat seine zugrunde liegenden Benutzertyp-Attribut auf "Gast". Normale Benutzer (insbesondere Mitglieder des Verzeichnisses) haben Benutzertyp Attribut "Member"

Genießen Sie eine begrenzte Anzahl von Berechtigungen im Verzeichnis. Diese Rechte eingeschränkt für Gäste zum Ermitteln von Informationen über andere Benutzer im Verzeichnis. Jedoch können Gastbenutzer noch interagieren mit den Benutzern und Gruppen die Ressourcen, die sie gerade bearbeiten. Gäste können:

- Siehe andere Benutzer und Gruppen, denen sie zugewiesen sind, Azure-Abonnement zugeordnet
- Finden Sie die Mitglieder der Gruppen, zu denen sie gehören
- Anderen Benutzern im Verzeichnis suchen Sie, wenn sie die vollständige e-Mail-Adresse des Benutzers kennen
- Sehen Sie nur eine begrenzte Anzahl von Attributen der Benutzer sehen sie-auf Name, Adresse, Benutzerprinzipalname (UPN) und Vorschaubild angezeigt
- Eine Liste der verifizierten Domänen im Verzeichnis
- Zustimmung zur Anwendung gewähren sie die gleiche Zugriffsberechtigungen für Mitglieder im Verzeichnis

## <a name="set-guest-user-access-policies"></a>Gast-Benutzerrichtlinien festlegen

Die Registerkarte **Konfigurieren** eines Verzeichnisses enthält Optionen zum Steuern des Zugriffs für Gastbenutzer. Diese Optionen können nur in Azure-Verwaltungsportal von einem globalen Verzeichnisadministrator geändert werden. Derzeit ist keine PowerShell oder API-Methode.

Zum Öffnen der Registerkarte **Konfigurieren** in der Azure-Verwaltungsportal wählen Sie **Active Directory aus**, und wählen Sie den Namen des Verzeichnisses.

![Registerkarte in Azure Active Directory konfigurieren][1]

Sie können die Optionen zum Steuern des Zugriffs für Gastbenutzer bearbeiten.

![für Gastbenutzer Zugriffssteuerungsoptionen][2]


## <a name="whats-next"></a>Was kommt als nächstes

- [Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in Azure Active Directory](active-directory-create-users-external.md)
- [Verwalten von Azure AD](active-directory-administer.md)
- [Verwalten von Kennwörtern in Azure AD](active-directory-manage-passwords.md)
- [Verwalten von Gruppen in Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
