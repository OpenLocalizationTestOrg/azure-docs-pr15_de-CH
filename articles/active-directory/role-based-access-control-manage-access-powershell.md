<properties
    pageTitle="Rollenbasierte Zugriffskontrolle (RBAC) Azure PowerShell verwalten | Microsoft Azure"
    description="Wie Sie RBAC Azure PowerShell, einschließlich Rollen auflisten, Zuweisen von Rollen und löschen Arbeitsaufträge für Benutzerrollen verwalten."
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
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-azure-powershell"></a>Rollenbasierte Zugriffskontrolle Azure PowerShell verwalten

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)


Rollenbasierte Zugriffskontrolle (RBAC) im Azure-Portal und Azure Resource Management-API können Sie Zugriff auf Ihr Abonnement detaillierte Ebene verwalten. Mit diesem Feature können Sie Zugriff auf Active Directory-Benutzer, Gruppen oder Service Prinzipale erteilen, indem sie einige Rollen in einem bestimmten Bereich zuweisen.

Bevor Sie PowerShell Rollenbasierten Verwaltung verwenden können, benötigen Sie Folgendes:

- Azure PowerShell Version 0.8.8 oder höher. Installieren Sie die neueste Version der Azure-Abonnement zugeordnet und Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md).

- Azure-Ressourcen-Manager-Cmdlets. [Azure-Ressourcen-Manager-Cmdlets](https://msdn.microsoft.com/library/mt125356.aspx) in PowerShell installieren.

## <a name="list-roles"></a>Liste Rollen

### <a name="list-all-available-roles"></a>Liste aller verfügbare Rollen
Liste RBAC-Rollen, die für die Zuordnung und die Vorgänge überprüfen, sie gewähren, `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShell-Get AzureRmRoleDefinition - screenshot](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Liste Aktionen einer Rolle
Liste die Aktionen für eine bestimmte Rolle verwenden `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShell-Get AzureRmRoleDefinition für eine bestimmte Rolle - screenshot](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Sehen, wer Zugriff
Rollenbasierten Zugriff Aufgaben aufzulisten, verwenden Sie `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Liste Rolle Aufgaben in einem bestimmten Bereich
Sie können alle Access-Aufgaben für eine angegebene Abonnement, Ressource oder Ressourcengruppe anzeigen. Um alle aktiven Aufgaben für eine Ressourcengruppe anzuzeigen, z. B. `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment für eine Ressourcengruppe - screenshot](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Liste Rollen für einen Benutzer
Verwenden Sie zum Auflisten aller Rollen, die einem angegebenen Benutzer zugewiesen und der, die Gruppen zugewiesen sind, denen der Benutzer angehört, `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShell-Get AzureRmRoleAssignment für einen Benutzer - screenshot](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Klassische Dienstadministrator Liste und Coadmin Rolle Aufgaben
Liste Access-Aufgaben für klassische Abonnement und coadministratoren verwenden:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Zugriff gewähren
### <a name="search-for-object-ids"></a>Suche nach Objekt-IDs
Um eine Rolle zuweisen, müssen Sie das (Benutzer, Gruppe oder Anwendung) und den Bereich.

Wenn Sie die Abonnement-ID nicht kennen, finden Sie diese **Abonnements** Blatt Azure-Portal. Die Abonnement-ID Abfragen finden Sie unter [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) auf MSDN.

Verwenden Sie zum Abrufen der Objekt-ID für ein Azure AD-Gruppe

    Get-AzureRmADGroup -SearchString <group name in quotes>

Verwenden Sie, um die Objekt-ID einer Azure AD Dienstprinzipalnamen oder Anwendung abzurufen:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Zuweisen einer Rolle zu einer Anwendung auf Abonnementebene
Gewähren von Zugriff auf eine Anwendung auf Abonnementebene verwenden:

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC PowerShell neue AzureRmRoleAssignment - screenshot](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Gruppenbereich Ressource Benutzer eine Rolle zuweisen
Verwenden Sie den Gruppenbereich Ressource Benutzer Zugriff gewähren

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShell neue AzureRmRoleAssignment - screenshot](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Zuweisen einer Rolle zu einer Gruppe auf den Ressourcenbereich
Um Zugriff zu einer Gruppe auf den Ressourcenbereich verwenden:

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShell neue AzureRmRoleAssignment - screenshot](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Entfernen
Um Zugriff für Benutzer und Gruppen zu entfernen, verwenden Sie Folgendes:

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShell-entfernen AzureRmRoleAssignment - screenshot](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Erstellen Sie eine benutzerdefinierte Rolle
Verwenden Sie eine benutzerdefinierte Rolle erstellen, die `New-AzureRmRoleDefinition` Befehl.

Beim Erstellen einer benutzerdefinierten Funktion mithilfe von PowerShell müssen Sie zunächst eine [integrierte Rollen](role-based-access-built-in-roles.md). Bearbeiten Sie die Attribute der *Aktionen*, *NotActions*oder *Bereiche* , die Sie möchten, und dann als neue Rolle speichern.

Im folgende Beispiel beginnt mit der *Virtual Machine* Beitragendenrolle und verwendet, erstellen Sie eine benutzerdefinierte Rolle namens *Virtual Machine-Operator*. Die neue Rolle gewährt Zugriff auf alle Lesevorgänge von *Microsoft.Compute*, *Microsoft.Storage*und *Microsoft.Network* Ressourcenprovider Zugang zu starten, starten und virtuellen Maschinen zu überwachen. Die benutzerdefinierte Rolle kann in zwei Abonnements verwendet werden.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Get AzureRmRoleDefinition - screenshot](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>Eine benutzerdefinierte Rolle ändern
Um eine benutzerdefinierte Rolle ändern, verwenden Sie zunächst die `Get-AzureRmRoleDefinition` Befehl zum Abrufen der Funktionsdefinition. Zweitens ändern Sie die gewünschten Rollendefinition. Verwenden Sie die `Set-AzureRmRoleDefinition` Befehl geänderten Rollendefinition.

Im folgenden Beispiel wird die `Microsoft.Insights/diagnosticSettings/*` Vorgang *Virtual Machine Operator* benutzerdefinierte Rolle.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShell-Set AzureRmRoleDefinition - screenshot](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Im folgende Beispiel hinzugefügt zugeordnet werden Bereiche der *Virtuellen Operator* benutzerdefinierte Rolle Azure-Abonnement.

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![RBAC PowerShell-Set AzureRmRoleDefinition - screenshot](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>Eine benutzerdefinierte Rolle löschen

Um eine benutzerdefinierte Rolle zu löschen, verwenden Sie die `Remove-AzureRmRoleDefinition` Befehl.

Im folgende Beispiel entfernt die *VM Operator* benutzerdefinierte Rolle.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShell-entfernen AzureRmRoleDefinition - screenshot](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Liste benutzerdefinierte Rollen
Verwenden, um die Rollen aufgelistet, die für die Zuordnung in einem Bereich, der `Get-AzureRmRoleDefinition` Befehl.

Das folgende Beispiel listet alle Rollen, die für die Zuordnung in das ausgewählte Abonnement.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShell-Get AzureRmRoleDefinition - screenshot](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

Im folgenden Beispiel steht *Virtual Machine Operator* benutzerdefinierte Rolle im *Production4* Abonnement da dieses Abonnement in der **AssignableScopes** der Rolle ist.

![RBAC PowerShell-Get AzureRmRoleDefinition - screenshot](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Siehe auch
- [Mithilfe von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
