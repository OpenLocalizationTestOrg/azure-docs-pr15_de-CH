<properties
   pageTitle="Hinzufügen oder Entfernen einer Benutzerrolle | Microsoft Azure"
   description="Informationen Sie zum privilegierten Identitäten mit Azure Active Directory privilegierte Identitätsmanagement-Anwendung Rollen hinzufügen."
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
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD privilegierten Identity Management: Das Hinzufügen oder Entfernen einer Benutzerrolle

Mit Azure Active Directory (AD), können ein globaler Administrator (oder Unternehmensadministrator) die Benutzer **dauerhaft** in Azure AD Rollen zugewiesen werden. Dies erfolgt mit PowerShell-Cmdlets wie `Add-MsolRoleMember` und `Remove-MsolRoleMember`. Oder sie können den Azure-Verwaltungsportal Siehe [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md).

Azure AD privilegierten Identitätsmanagement-Anwendung Administratoren privilegierten Rolle permanente Rolle zuweisen, sowie. Darüber hinaus können bevorzugte Rollenadministratoren Benutzer **berechtigt** Administratorrollen. Administrator berechtigt kann die Rolle aktiviert, wenn benötigen und deren Berechtigungen dann abläuft, wenn sie fertig sind.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Rollen mit PIM im Azure-Portal verwalten

In Ihrer Organisation können Sie Benutzer verschiedenen Administratorfunktionen in Azure AD, Office 365 und andere Microsoft- Dienste zuweisen.  Weitere Informationen zu verfügbaren Rollen können Rollen [in Azure AD PIM](active-directory-privileged-identity-management-roles.md)gefunden.

Zum Hinzufügen oder Entfernen eines Benutzers in einer Rolle mit privilegierten Identity Management bringen Sie PIM-Dashboard. Dann klicken Sie auf **Benutzer in Administratorrollen** , oder wählen Sie eine bestimmte Rolle (z. B. globaler Administrator) in der Tabelle Rollen.

> [AZURE.NOTE] Wenn PIM im Azure-Portal noch aktiviert noch nicht, finden Sie in [Erste Schritte mit Azure AD PIM](active-directory-privileged-identity-management-getting-started.md) Details.

Sie gegebenenfalls zu einem anderen Benutzerzugriff auf den PIM selbst werden Rollen erfordert PIM die Benutzer wie [Zugang zu PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)beschrieben.

## <a name="add-a-user-to-a-role"></a>Hinzufügen eines Benutzers zu einer Rolle

1. Wählen Sie in [Azure-Portal](https://portal.azure.com/) **Azure AD privilegierten Identitätsmanagement** nebeneinander auf dem Dashboard.
2. Wählen Sie **privilegierte Rollen verwalten**.
3. Wählen Sie in der Tabelle **Rollen Zusammenfassung** die Rolle zu verwalten.
4. Wählen Sie Blatt Rolle **Hinzufügen**.
5. Klicken Sie auf **Benutzer auswählen** und suchen Sie nach der Benutzer auf die **Benutzer auswählen** .  
6. Wählen Sie den Benutzer aus der Liste der Suchergebnisse, und klicken Sie auf **Fertig**.
4. Klicken Sie auf **OK** , um die Auswahl zu speichern. Der gewählte Benutzer wird in der Liste für die Rolle angezeigt.

> [AZURE.NOTE]
>Neue Benutzer in einer Rolle können nur für die Rolle standardmäßig. Klicken Sie auf Benutzer in der Liste, um die Rolle dauerhaft. Die Informationen des Benutzers wird in einem neuen Blatt angezeigt. Wählen Sie im Menü Informationen Benutzer **Stellen ber** .  
>Wenn ein Benutzer für Azure mehrstufige Authentifizierung (MFA) nicht registriert oder ist ein Microsoft-Konto (in der Regel @outlook.com), müssen Sie sie dauerhaft in ihre Rollen. Können Administratoren sollen für MFA während der Aktivierung registrieren.

Nun, dass der Benutzer für eine Rolle, teilen Sie mit, dass sie nach der Anleitung unter [Aktivieren oder Deaktivieren einer Rolle](active-directory-privileged-identity-management-how-to-activate-role.md)aktiviert.

## <a name="remove-a-user-from-a-role"></a>Entfernen eines Benutzers aus einer Rolle

Entfernen von Benutzern können Rollen zugewiesen werden können sicherstellen, dass immer mindestens ein Benutzer eine permanente globale Administrator ist.

Führen Sie diese Schritte, um einen bestimmten Benutzer aus einer Rolle entfernen:

1. Navigieren Sie zu der Rolle in der Rollenliste auswählen eine Rolle in Azure AD PIM-Dashboard oder durch Klicken auf die Schaltfläche **Benutzer in Administratorrollen** .
2. Klicken Sie auf den Benutzer in der Benutzerliste.
3. Klicken Sie auf **Entfernen**. Eine Meldung fordert Sie bestätigen.
4. Klicken Sie auf **Ja,** um die Rolle des Benutzers zu entfernen.

Wenn Sie nicht sicher sind, welche Benutzer ihre Rolle Aufgaben benötigen, können Sie [eine Access-Überprüfung für die Rolle starten](active-directory-privileged-identity-management-how-to-start-security-review.md).


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
