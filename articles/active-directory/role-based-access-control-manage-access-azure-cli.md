<properties
    pageTitle="Rollenbasierte Zugriffskontrolle (RBAC) mit Azure CLI verwalten | Microsoft Azure"
    description="Erfahren Sie mehr über das Role-Based Access Control (RBAC) Azure Befehlszeilenschnittstelle verwalten von Rollen und Rolle Aktionen und Bereiche Abonnement und Anwendung Rollen zuordnen."
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

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Verwalten Sie Role-Based Access Control Azure Befehlszeilenschnittstelle

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)

Rollenbasierte Zugriffskontrolle (RBAC) im Azure-Portal und Azure-Ressourcen-Manager-API können Sie Zugriff auf Ihr Abonnement und Ressourcen abgestimmte Ebene verwalten. Mit diesem Feature können Sie Zugriff auf Active Directory-Benutzer, Gruppen oder Service Prinzipale erteilen, indem sie einige Rollen in einem bestimmten Bereich zuweisen.

Bevor Sie zu RBAC Azure Befehlszeilenschnittstelle (CLI) verwenden können, benötigen Sie Folgendes:

- Azure CLI Version 0.8.8 oder höher. Installieren Sie die neueste Version der Azure-Abonnement zugeordnet und finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md).
- Azure Ressourcenmanager in Azure CLI. Weitere Informationen finden Sie in [Verwendung der Azure-CLI mit der Ressourcen-Manager](../xplat-cli-azure-resource-manager.md) .

## <a name="list-roles"></a>Liste Rollen

### <a name="list-all-available-roles"></a>Liste aller verfügbare Rollen
Zum Auflisten aller verfügbaren Rollen verwenden:

        azure role list

Das folgende Beispiel zeigt die Liste *aller verfügbaren*Rollen.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![RBAC Azure Befehlszeile - Liste der Azure-Rolle - screenshot](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Liste Aktionen einer Rolle
Liste die Aktionen einer Rolle verwenden:

    azure role show "<role name>"

Das folgende Beispiel zeigt die Aktionen der *Teilnehmer* und *Virtuellen Teilnehmer* .

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![RBAC Azure Befehlszeile - Azure-Rolle anzeigen - screenshot](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>Liste Zugriff
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Liste Rolle Aufgaben auf einer Ressourcengruppe
Rolle Aufgaben auflisten, die in einer Ressourcengruppe vorhanden sind, verwenden Sie Folgendes:

    azure role assignment list --resource-group <resource group name>

Im folgenden Codebeispiel wird die Verwendung der Rolle Aufgaben *Pharma-Sales-Projecforcast* -Gruppe.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Befehlszeile RBAC Azure - Arbeitsauftragsliste von Gruppe Screenshot Azure-Rolle](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Liste Rolle Aufgaben für einen Benutzer
Verwenden Sie zum Auflisten Rolle Aufgaben für einen bestimmten Benutzer und Aufgaben, die ein Benutzer Gruppen zugewiesen sind

    azure role assignment list --signInName <user email>

Sie sehen auch Rollen zugewiesen werden, die aus Gruppen geerbt werden durch den Befehl ändern:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Das folgende Beispiel zeigt die Rolle Arbeitsaufträge, die gewährt werden, die *sameert@aaddemo.com* Benutzer. Dies schließt direkt mit dem Benutzer zugewiesenen Rollen und Rollen von Gruppen geerbt werden.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![RBAC Azure Befehlszeile - Arbeitsauftragsliste Benutzer Azure-Rolle - screenshot](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>Zugriff gewähren
Verwenden Sie zum Gewähren des Zugriffs nach bezeichnete die Rolle zuweisen möchten:

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Auf Abonnementebene Gruppe eine Rolle zuweisen
Verwenden Sie zum Zuweisen einer Rolle zu einer Gruppe auf Abonnementebene

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Im folgenden Beispiel wird *Christine Kochs Team* auf *Abonnementebene* *Rolle* zugewiesen.


![Azure Befehlszeile RBAC - Gruppe Screenshot erstellen Zuordnung der Azure-Rolle](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Zuweisen einer Rolle zu einer Anwendung auf Abonnementebene
Verwenden Sie zum Zuweisen einer Rolle zu einer Anwendung auf Abonnementebene

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Im folgenden Beispiel wird gewährt *der Beitragendenrolle mit einer *Azure AD* -Anwendung auf das ausgewählte Abonnement* .

 ![RBAC Befehlszeile Azure - Anwendung Azure-Rolle Aufgabe erstellen](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Gruppenbereich Ressource Benutzer eine Rolle zuweisen
Verwenden Sie den Gruppenbereich Ressource Benutzer eine Rolle zuweisen

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Dabei gewährt *Virtual Machine* Teilnehmerrolle auf *samert@aaddemo.com* Benutzer den Gruppenbereich *Pharma Sales ProjectForcast* Ressource.

![RBAC Azure Befehlszeile - Zuordnung der Azure-Rolle erstellen Benutzer-screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Zuweisen einer Rolle zu einer Gruppe auf den Ressourcenbereich
Zuweisen eine Rolle zu einer Gruppe auf den Ressourcenbereich verwenden:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Im folgenden Beispiel wird einer *Azure AD* -Gruppe in einem *Subnetz*der Beitragendenrolle *Virtual Machine* gewährt.

![Azure Befehlszeile RBAC - Gruppe Screenshot erstellen Zuordnung der Azure-Rolle](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>Entfernen
Verwenden Sie zum Entfernen einer rollenzuweisung

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Im folgenden Beispiel wird *Virtual Machine Beitragender* Rolle Zuordnung aus der *sammert@aaddemo.com* Benutzer für die Ressourcengruppe *Pharma-Sales-ProjectForcast* .
Im Beispiel entfernt die Zuweisung der Sicherheitsrolle aus einer Gruppe auf das Abonnement.

![RBAC Azure Befehlszeile - Azure-Rolle Zuordnung löschen - screenshot](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Erstellen Sie eine benutzerdefinierte Rolle
Erstellen Sie eine benutzerdefinierte Funktion verwenden:

    azure role create --inputfile <file path>

Das folgende Beispiel erstellt eine benutzerdefinierte Rolle als *Virtual Machine Operator*bezeichnet. Die benutzerdefinierte Rolle gewährt Zugriff auf alle Lesevorgänge von *Microsoft.Compute*, *Microsoft.Storage*und *Microsoft.Network* Ressourcenprovider Zugang zu starten, starten und virtuellen Maschinen zu überwachen. Die benutzerdefinierte Rolle kann in zwei Abonnements verwendet werden. Dieses Beispiel verwendet eine JSON-Datei als Eingabe.

![JSON - benutzerdefinierte Rollendefinition - screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure Befehlszeile - Azure-Rolle erstellen - screenshot](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Eine benutzerdefinierte Rolle ändern

Um eine benutzerdefinierte Funktion zu ändern, verwenden Sie zuerst die `azure role show` Befehl Rollendefinition abrufen. Zweitens ändern Sie gewünschte Definitionsdatei Rolle. Verwenden Sie `azure role set` zum Speichern der geänderten Rollendefinition.

    azure role set --inputfile <file path>

Im folgenden Beispiel wird den Vorgang *Microsoft.Insights/diagnosticSettings/* **Aktionen**und Azure-Abonnement auf **AssignableScopes** virtuellen Operator benutzerdefinierte Rolle.

![JSON - benutzerdefinierte Rollendefinition - Screenshot ändern](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![RBAC Azure Befehlszeile - Azure-Rolle Set - screenshot](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Eine benutzerdefinierte Rolle löschen

Um eine benutzerdefinierte Rolle zu löschen, verwenden Sie zuerst die `azure role show` Befehl **ID** der Rolle bestimmt. Verwenden Sie dann die `azure role delete` Befehl, um die Rolle zu löschen, indem Sie die **ID**.

Im folgende Beispiel entfernt die *VM Operator* benutzerdefinierte Rolle.

![RBAC Azure Befehlszeile - Azure-Rolle löschen - screenshot](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Liste benutzerdefinierte Rollen

Verwenden, um die Rollen aufgelistet, die für die Zuordnung in einem Bereich, der `azure role list` Befehl.

Das folgende Beispiel listet alle Rolle für die Zuordnung in das ausgewählte Abonnement.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![RBAC Azure Befehlszeile - Liste der Azure-Rolle - screenshot](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

Im folgenden Beispiel steht *Virtual Machine Operator* benutzerdefinierte Rolle im *Production4* Abonnement da dieses Abonnement in der **AssignableScopes** der Rolle ist.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![RBAC Azure Befehlszeile - Azure Rollenliste für benutzerdefinierte Rollen - screenshot](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>RBAC-Themen
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
