<properties
   pageTitle="Rollen in PIM | Microsoft Azure"
   description="Erfahren Sie, welche Rollen für privilegierte Identitäten Endung Azure privilegierten Identity Management verwendet werden."
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
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Rollen in Azure AD Berechtigungen Identitätsmanagement

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Sie können Benutzer in Ihrer Organisation unterschiedliche Administratorrollen in Azure AD. Zuordnungsstatus Rolle steuern, welche Aufgaben wie hinzufügen oder Entfernen von Benutzern oder Service-Einstellungen, die Benutzer auf Azure AD Office 365 Microsoft Online Services und andere verbundene Anwendung ausführen.  

Ein globaler Administrator können die Benutzer **dauerhaft** in Azure AD mithilfe von PowerShell-Cmdlets wie Rollen zugewiesen werden `Add-MsolRoleMember` und `Remove-MsolRoleMember`, oder durch [Zuweisen von Administratorrollen in Azure Active Directory](active-directory-assign-admin-roles.md)im Verwaltungsportal.

Azure AD Berechtigungen Identitätsmanagement (PIM) verwaltet Richtlinien für privilegierten Zugriff für Benutzer in Azure AD. PIM weist Benutzern Rollen in Azure AD, und weisen Sie einer Person dauerhaft in der Rolle für die Rolle sein. Wenn ein Benutzer dauerhaft einer Rolle zugewiesen oder aktiviert eine Zuordnung Rolle berechtigt sie Active Directory Azure, Office 365 und anderen Applikationen mit ihren Rollen zugewiesenen Berechtigungen verwalten.

Es gibt keinen Unterschied jemand mit einem gegenüber Assignment berechtigte Rolle Zugriff. Der einzige Unterschied ist, dass manche dieser Zugriff jederzeit nicht. Bestehen für die Rolle und können einschalten und ausschalten Wenn sie müssen.

## <a name="roles-managed-in-pim"></a>Rollen im PIM verwaltet

Privilegierte Identity Management können Sie allgemeine Administratorrollen Benutzer zuweisen:


- **Globaler administrator** (auch bekannt als Unternehmensadministrator) hat Zugriff auf alle administrativen Funktionen. Sie können mehrere globale Administratoren in Ihrer Organisation. Wer erwerben Sie Office 365 automatisch angemeldet wird ein globaler Administrator
- **Privilegierte Rollenadministrator** verwaltet Azure AD PIM und Rolle Aufgaben für andere Benutzer aktualisiert.  
- **Abrechnung Administrator** macht Käufe, verwaltet Abonnements Supportanfragen verwaltet und überwacht Service.
- **Administrator Kennwort** Zurücksetzen von Kennwörtern, Serviceanfragen verwaltet und überwacht Service. Kennwort-Admins sind auf Zurücksetzen von Kennwörtern für Benutzer.
- **Dienstadministratoren** Serviceanfragen verwaltet und überwacht service Health.

  > [AZURE.NOTE] Bei Verwendung von Office 365 vor der Zuweisung der Administratorrolle Service an einen Benutzer zunächst weisen Sie dann den Benutzer Administratorberechtigungen an einen Dienst wie Exchange Online.

- **Benutzerverwaltungsadministrator** Zurücksetzen von Kennwörtern, Dienst überwacht und verwaltet Benutzerkonten, Gruppen und Serviceanfragen. Management Benutzeradministrator kann nicht globalen Administrator löschen, andere Administratorrollen erstellen oder Zurücksetzen von Kennwörtern für Abrechnung, global und Dienstadministratoren.
- **Exchange-Administrator** verfügt über Administratorzugriff auf Exchange Online über die Exchange-Verwaltungskonsole (BK) und möglich, nahezu jede Aufgabe im Exchange Online.
- **SharePoint-Administrator** verfügt über Administratorzugriff auf SharePoint Online über das SharePoint Online Administrationscenter und kann nahezu jede Aufgabe in SharePoint Online.
- **Skype für Business Administrator** verfügt über Administratorzugriff auf Skype für Unternehmen über Skype Business Admin Center und fast alle Aufgaben in Skype für Business Online ausführen.

Dieser Artikel Weitere Informationen zum [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md) und [Zuweisen von Administratorrollen in Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Über PIM können Sie [diesen Rollen Benutzer zuweisen](active-directory-privileged-identity-management-how-to-add-role-to-user.md) , so dass Benutzer [die Rolle bei Bedarf aktivieren](active-directory-privileged-identity-management-how-to-activate-role.md).

Wenn Sie einen anderen Benutzer Zugang zu PIM selbst verwalten möchten, werden Rollen erfordert PIM die Benutzer wie [PIM zugreifen](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)beschrieben.


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Rollen im PIM nicht verwaltet

Rollen in Exchange Online und SharePoint Online außer den oben genannten nicht in Azure AD dargestellt und sind daher nicht im PIM angezeigt. Weitere Informationen zum Ändern der Zuweisung von differenzierte Rolle in Office 365-Diensten finden Sie unter [Berechtigungen in Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure-Abonnements und Ressourcengruppen werden auch nicht in Azure AD dargestellt. Um Azure-Abonnements zu verwalten, finden Sie unter [Hinzufügen oder Ändern von Azure Administratorrollen](../billing-add-change-azure-subscription-administrator.md) und Informationen zu Azure RBAC finden Sie unter [Azure Role-Based-Zugriffskontrolle](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Benutzerrollen und anmelden
Bei einigen Microsoft Services und Applikationen Zuweisen einer Rolle zu einen Benutzer ermöglichen, dass Benutzer ein Administrator möglicherweise nicht.

Zugriff auf Azure-Verwaltungsportal erfordert, dass der Benutzer ein Dienstadministrator oder Co-Administrator Azure-Abonnement, selbst wenn der Benutzer nicht der Azure-Abonnements verwalten muss.  Zum Verwalten von Konfigurationen für Azure AD im klassischen Portal muss Benutzer den globalen Administrator in Azure AD und Co Abonnementadministrator Azure-Abonnement sein.  Zum Hinzufügen von Benutzern zu Azure-Abonnements finden Sie unter [Hinzufügen oder Ändern von Azure Administratorrollen](../billing-add-change-azure-subscription-administrator.md).

Zugriff auf Microsoft Online Services benötigen Benutzer auch zugewiesen werden eine Lizenz vor dem Öffnen der Service Portal oder Verwaltungsaufgaben ausführen.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Zuweisen einer Lizenz zu einem Benutzer in Azure AD

1. [Klassischen Azure-Portal] anmelden (http://manage.windowsazure.com) globales Administratorkonto oder ein CO Administratorkonto.
2. Wählen Sie **Alle Elemente** im Hauptmenü.
3. Wählen Sie das Verzeichnis zu arbeiten und die Lizenzen zugeordnet.
4. **Lizenzen**auswählen Die Liste der verfügbaren Lizenzen wird angezeigt.
5. Wählen Sie den Lizenzplan enthält die Lizenzen, die Sie verteilen möchten.
6. Wählen Sie **Benutzer zuweisen**.
7. Wählen Sie den Benutzer, dem eine Lizenz zuweisen möchten.
8. Klicken Sie auf die Schaltfläche **zuweisen** .  Der Benutzer kann jetzt in Azure anmelden.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
