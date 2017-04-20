<properties
   pageTitle="Wie PIM Zugriff zu | Microsoft Azure"
   description="Informationen Sie zum Hinzufügen von Rollen zu Benutzern mit der Erweiterung Azure Active Directory privilegierte Identitätsmanagement PIM zu verwalten."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>Wie Sie Zugang zu Azure AD privilegierten Identitätsmanagement

Der globale Administrator automatisch Azure AD privilegierten Identitätsmanagement (PIM) für eine Organisation ermöglicht erhalten Rollen zugewiesen und auf PIM. Niemand wird Schreibzugriff standardmäßig, einschließlich anderer globalen Administratoren. Andere globale Administratoren, Administratoren und Sicherheit Leser haben schreibgeschützten Zugriff auf Azure AD PIM. Zum Zugreifen auf den PIM kann der erste Benutzer andere **privilegierte Rolle** Administratorrolle zuweisen. Diese Zuordnung muss in PIM selbst ausgeführt werden und nicht über PowerShell oder anderen Portalen geändert werden.

> [AZURE.NOTE] Verwalten von Azure AD PIM erfordert Azure MFA. Da Microsoft Konten für Azure MFA registrieren können, kann kein Benutzer mit einem Microsoft-Konto anmeldet Azure AD PIM zugreifen.

Sicher gibt immer mindestens zwei Benutzer in einer Administratorrolle privilegierten Rolle einen Benutzer gesperrt oder ihr Konto gelöscht.

## <a name="give-another-user-access-to-manage-pim"></a>Einem anderen Benutzer Zugriff auf PIM verwalten

1. [Azure-Portal](https://portal.azure.com/) anmelden und die app **Azure AD privilegierten Identitätsmanagement** Dashboard wählen.
2. Wählen Sie **privilegierte Rollen verwalten** > **privilegierte Rollenadministrator** > **Hinzufügen**.

    ![Privilegierte Rollenadministratoren - Screenshot hinzufügen][1]

4. Verwaltete Benutzer hinzufügen-Blade ist Schritt 1 bereits abgeschlossen. Wählen Sie Schritt 2, **Wählen Sie Benutzer** und Suche nach dem Benutzer, den Sie hinzufügen möchten.

    ![Benutzer - screenshot][2]

6. Wählen Sie den Benutzer aus den Suchergebnissen aus, und klicken Sie auf **Fertig**.
7. Klicken Sie auf **OK** , um die Auswahl zu speichern. Der gewählte Benutzer wird in der Liste der privilegierten Rollenadministratoren angezeigt.

    - Wenn Sie ein Benutzer eine neue Rolle zuweisen, werden sie automatisch als geeignete eingerichtet zum Aktivieren der Rolle. Klicken Sie auf die Benutzer in der Liste, zu dauerhaft in die Rolle. Wählen Sie im Menü Informationen Benutzer **Stellen ber** .

8. Senden Sie dem Benutzer einen Link zu [Erste Schritte mit Azure AD privilegierten Identitätsmanagement](active-directory-privileged-identity-management-getting-started.md).


## <a name="remove-another-users-access-rights-for-managing-pim"></a>Entfernen Sie ein anderer Benutzer Zugriffsrechte für die Verwaltung von PIM

Bevor Sie jemanden Administratorrolle privilegierten Rolle entfernen, stellen Sie immer sicher noch werden zwei Benutzer zugewiesen.

1. Klicken Sie auf Rolle **privilegierten Rollenadministrator**im Dashboard PIM.  Die Liste der Benutzer in dieser Rolle wird angezeigt.
2. Klicken Sie auf den Benutzer in der Benutzerliste.
3. Klicken Sie auf **Entfernen**.  Eine Meldung wird angezeigt.
4. Klicken Sie auf **Ja,** um den Benutzer aus der Rolle entfernen.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
