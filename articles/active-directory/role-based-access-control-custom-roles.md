<properties
    pageTitle="Benutzerdefinierte Rollen in Azure RBAC | Microsoft Azure"
    description="Informationen Sie zum Definieren von benutzerdefinierter Rollen mit Azure Role-Based Access Control für genauere Identitätsmanagement in Azure-Abonnement."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="kgremban"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/25/2016"
    ms.author="kgremban"/>


# <a name="custom-roles-in-azure-rbac"></a>Benutzerdefinierte Rollen in Azure RBAC


Erstellen Sie eine benutzerdefinierte Rolle in Azure Role-Based Access Control (RBAC), wenn keine integrierten Rollen Ihre spezifischen Bedürfnisse. Benutzerdefinierte Rollen können mithilfe von [Azure PowerShell](role-based-access-control-manage-access-powershell.md), [Azure-Befehlszeilenschnittstelle](role-based-access-control-manage-access-azure-cli.md) (CLI) und die [REST-API](role-based-access-control-manage-access-rest.md)erstellt werden. Integrierte Funktionen wie können Benutzer, Gruppen und Applikationen Abonnement, Ressourcengruppe und Ressourcenbereiche benutzerdefinierte Rollen zugewiesen werden. Benutzerdefinierte Rollen in Azure AD-Mandanten gespeichert und können über alle Abonnements, die diesem Mandanten als Azure AD-Verzeichnis für das Abonnement verwendet freigegeben werden.

Folgendes ist ein Beispiel für eine benutzerdefinierte Rolle für die Überwachung und Neustart virtueller Maschinen:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Aktionen
**Actions** -Eigenschaft eine benutzerdefinierte Rolle gibt die Azure Operationen, die Rolle Zugriff gewährt. Es ist eine Sammlung von Vorgang Zeichenfolgen, die sicherungsfähige Vorgänge Azure Ressourcen identifizieren. Operation Zeichenfolgen mit Platzhaltern (\*) Zugriff auf alle Vorgänge, die die Vorgangszeichenfolge entsprechen. Zum Beispiel:

-   `*/read`gewährt Zugriff auf alle Ressourcentypen Azure Ressourcenprovider Lesevorgänge.
-   `Microsoft.Network/*/read`gewährt Zugriff auf alle Ressourcentypen in Microsoft.Network Ressourcenprovider Azure Lesevorgänge.
-   `Microsoft.Compute/virtualMachines/*`gewährt Zugriff auf alle Vorgänge von virtuellen Maschinen und untergeordneten Typen.
-   `Microsoft.Web/sites/restart/Action`erteilt Zugriff auf um Websites zu starten.

Mit `Get-AzureRmProviderOperation` (in PowerShell) oder `azure provider operations show` (in Azure CLI) Liste Operationen von Azure Ressourcenanbieter. Sie können auch Befehle, ob eine Operationszeichenfolge gültig ist und Platzhalter Vorgang Zeichenfolgen zu erweitern.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![PowerShell HTML - Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | OperationName FT-Operation](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure CLI Screenshot - Azure Provider Vorgänge anzeigen "Microsoft.Compute/virtualMachines/\*/action" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Verwenden Sie die **NotActions** -Eigenschaft, wenn Satz von Operationen, die Sie zulassen möchten leichter durch Ausschließen von eingeschränkten Operationen definiert ist. Durch eine benutzerdefinierte Rolle gewährten Zugriffs wird berechnet durch Subtrahieren **NotActions** Vorgänge aus den **Aktionen** .

> [AZURE.NOTE] Wenn ein Benutzer eine Rolle, die eine Operation in **NotActions**ausgeschlossen und erhält eine zweite Rolle, die Zugriff auf denselben Vorgang zugewiesen, der Benutzer kann zum Ausführen der Operation. **NotActions** ist eine Deny-Regel – es ist einfach eine bequeme Möglichkeit zum Erstellen zulässigen Operationen bei bestimmte Vorgängen ausgeschlossen werden müssen.

## <a name="assignablescopes"></a>AssignableScopes
Die **AssignableScopes** -Eigenschaft die benutzerdefinierte Rolle gibt die Bereiche an (Abonnements, Ressourcengruppen oder Ressourcen) in denen benutzerdefinierte Rolle zugewiesen ist. Sie können die benutzerdefinierte Rolle für die Zuordnung in nur Abonnements oder Ressourcengruppen, die sie benötigen, und nicht alle Benutzerfunktionalität für den Rest des Abonnements oder Ressourcengruppen.

Beispiele für gültige Bereiche zugeordnet werden:

-   "/ Abonnements c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ Abonnements e91d47c4-76f3-4271-a796-21b4ecfe3624" - macht die Rolle für die Zuordnung in zwei Abonnements.
-   "/ Abonnements c276fc76-9cd4-44c9-99a7-4fd71546436e" - macht die Rolle für die Zuordnung in einem Abonnement.
-  "/ Abonnements-c276fc76-9cd4-44c9-99a7-4fd71546436e/ResourceGroups/Netzwerk" ist die Rolle für die Zuordnung nur die Netzwerkressourcengruppe.

> [AZURE.NOTE] Verwenden Sie mindestens ein Abonnement, Ressourcengruppe oder Ressource-ID

## <a name="custom-roles-access-control"></a>Benutzerdefinierte Rollen Zugriffskontrolle
Die **AssignableScopes** -Eigenschaft die benutzerdefinierte Rolle steuert auch, wer anzeigen, ändern und löschen die Rolle.

- Wer kann eine benutzerdefinierte Rolle erstellen?
    Besitzer (und Benutzeradministratoren) Abonnements, Ressourcengruppen und Ressourcen können benutzerdefinierte Rollen erstellen für die Verwendung in diesen Bereichen.
    Das Erstellen der Rolle Anwender können `Microsoft.Authorization/roleDefinition/write` Vorgang für die **AssignableScopes** der Rolle.

- Wer eine benutzerdefinierte Rolle?
    Besitzer (und Benutzeradministratoren) Abonnements, Ressourcengruppen und Ressourcen können benutzerdefinierte Rollen in diesen Bereichen. Benutzer müssen zum Ausführen der `Microsoft.Authorization/roleDefinition/write` Vorgang für alle **AssignableScopes** eine benutzerdefinierte Rolle.

- Die benutzerdefinierte Rollen anzeigen können?
    Alle integrierte Funktionen in Azure RBAC erlauben Rollen zugewiesen werden. Benutzer ausführen können die `Microsoft.Authorization/roleDefinition/read` auf einen Bereich kann RBAC-Rollen, die für die Zuordnung in diesem Bereich anzuzeigen.

## <a name="see-also"></a>Siehe auch
- [Rolle Based Access Control](role-based-access-control-configure.md): Erste Schritte mit RBAC in Azure-Portal.
- Informationen Sie zum Verwalten des Zugriffs mit:
    - [PowerShell](role-based-access-control-manage-access-powershell.md)
    - [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
    - [REST-API](role-based-access-control-manage-access-rest.md)
- [Integrierte Rollen](role-based-access-built-in-roles.md): Details zu den Rollen, die standardmäßig im RBAC.
