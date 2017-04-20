<properties
    pageTitle="Berechtigungen für Benutzer zu bestimmten Lab-Richtlinien | Microsoft Azure"
    description="Erfahren Sie, wie Benutzer Berechtigungen bestimmten Lab Richtlinien in DevTest Labs basierend auf den Bedürfnissen des Benutzers"
    services="devtest-lab,virtual-machines,visual-studio-online"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Berechtigungen Sie für Benutzer zu bestimmten Lab-Richtlinien

## <a name="overview"></a>Übersicht

Dieser Artikel veranschaulicht das PowerShell verwenden, um Benutzern Berechtigungen zu einer bestimmten Lab. Auf diese Weise können Berechtigungen auf Bedürfnisse des Benutzers erfolgen. Sie möchten z. B. einem bestimmten Benutzer die VM-Richtlinien jedoch nicht die Kosten Richtlinien ändern einräumen.

## <a name="policies-as-resources"></a>Richtlinien als Ressourcen

Wie im Artikel [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md) , kann RBAC abgestimmte Verwaltung von Ressourcen für Azure. RBAC können Sie Aufgaben in Ihrem Team DevOps trennen und das Ausmaß des Zugriffs für Benutzer, die sie für ihre Aufgaben gewähren.

In DevTest Labs ist eine Richtlinie einen Ressourcentyp, der RBAC-Aktion **Microsoft.DevTestLab/labs/policySets/policies/**ermöglicht. Jede Übungseinheit Richtlinie ist eine Ressource den Ressourcentyp Richtlinie und als Bereich um eine RBAC-Rolle zugewiesen werden.

Z. B. erteilen Benutzern Lese-/Schreibberechtigung Richtlinie **Zugelassene VM Größen** erstellen würde eine benutzerdefinierte Rolle mit **Microsoft.DevTestLab/labs/policySets/policies/** * Maßnahmen, und weisen Sie die Benutzer dieser benutzerdefinierten Rolle im Bereich des * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Weitere Informationen über benutzerdefinierte Funktionen in RBAC finden Sie unter [benutzerdefinierte Rollen Zugriffskontrolle](../active-directory/role-based-access-control-custom-roles.md).

##<a name="creating-a-lab-custom-role-using-powershell"></a>Erstellen einer benutzerdefinierten Lab-Rolle mit PowerShell
Um loszulegen, benötigen den folgenden Artikel lesen die erklären, wie installieren und Konfigurieren von Azure PowerShell-Cmdlets: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Wenn Sie Azure PowerShell-Cmdlets eingerichtet haben, können Sie folgenden Aufgaben ausführen:

- Alle Operationen/Aktionen für ein Ressourcenanbieter auflisten
- Liste Aktionen in einer bestimmten Rolle:
- Erstellen Sie eine benutzerdefinierte Rolle

PowerShell-Skript zeigt Beispiele für folgende Aufgaben:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Zuweisen von Berechtigungen für einen Benutzer für eine bestimmte Richtlinie mithilfe von benutzerdefinierten Rollen
Wenn Sie benutzerdefinierten Rollen definiert haben, können Sie sie Benutzern zuweisen. Um eine benutzerdefinierte Rolle einem Benutzer zuweisen, benötigen Sie zunächst die **ObjectId** Benutzer darstellt. Dazu verwenden Sie das Cmdlet " **Get-AzureRmADUser** ".

Im folgenden Beispiel ist die **ObjectId** des Benutzers *SomeUser* 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Haben die **ObjectId** für Benutzer und eine benutzerdefinierte Rolle können Sie den Benutzer mit dem **New-AzureRmRoleAssignment** -Cmdlet Rolle zuweisen:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

Im vorherigen Beispiel wird die **AllowedVmSizesInLab** -Richtlinie verwendet. Verwenden Sie die folgenden Richtlinien:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte

Einmal haben Sie Benutzer zu bestimmten Lab Berechtigungen hier sind einige Schritte zu berücksichtigen:

- [Sicherer Zugang zu einem Labor](devtest-lab-add-devtest-user.md).

- [Labrichtlinien festlegen](devtest-lab-set-lab-policy.md).

- [Erstellen einer Lab-Vorlage](devtest-lab-create-template.md).

- [Benutzerdefinierte Elemente für Ihre virtuellen Computer erstellen](devtest-lab-artifact-author.md).

- [Hinzufügen einer VM mit einem Labor](devtest-lab-add-vm-with-artifacts.md).
