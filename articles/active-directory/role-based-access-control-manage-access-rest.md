<properties
    pageTitle="Rollenbasierte Zugriffskontrolle mit der REST-API verwalten"
    description="Rollenbasierte Zugriffskontrolle mit der REST-API verwalten"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>Rollenbasierte Zugriffskontrolle mit der REST-API verwalten

> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)

Role-Based Access Control (RBAC) in Azure-Portal und Azure-Ressourcen-Manager-API können Sie Zugriff auf Ihr Abonnement und Ressourcen abgestimmte Ebene verwalten. Mit diesem Feature können Sie Zugriff auf Active Directory-Benutzer, Gruppen oder Service Prinzipale erteilen, indem sie einige Rollen in einem bestimmten Bereich zuweisen.

## <a name="list-all-role-assignments"></a>Listen Sie alle rollenzuweisungen

Listet alle rollenzuweisungen auf den angegebenen Bereich und Subscopes.

Liste Rolle Aufgaben benötigen Sie Zugriff auf `Microsoft.Authorization/roleAssignments/read` auf den Bereich. Die integrierten Rollen erhalten Zugriff auf diesen Vorgang. Weitere Informationen über Rollen zugewiesen und Verwalten von Azure Ressourcen Siehe [Azure Role-Based zugreifen](role-based-access-control-configure.md).

### <a name="request"></a>Anforderung

Verwenden der **GET** -Methode mit der folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

Festlegen des URIS die folgenden Ersatzartikel Wunsch anpassen

1. Ersetzen Sie *{Bereich}* mit der Rolle Aufgaben aufgelistet werden sollen. Die folgenden Beispiele veranschaulichen die Bereich für verschiedene Ebenen festzulegen:

  - Abonnement: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. 2015-07-01 *{api-Version}* ersetzen.

3. Ersetzen Sie *{Filter}* durch die Bedingung, die Sie zum Filtern der Liste Rolle Zuordnung anwenden möchten:

  - Liste Rolle Aufgaben für nur den angegebenen Bereich, einschließlich nicht rollenzuweisungen am Subscopes:`atScope()`    
  - Liste Rolle Aufgaben für einen bestimmten Benutzer, Gruppe oder Anwendung:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - Liste Rolle Aufgaben für einen bestimmten Benutzer, einschließlich Gruppen geerbt |`assignedTo('{objectId of user}')`

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Informationen Sie über eine Zuweisung der Sicherheitsrolle

Ruft Informationen über eine einzelne rollenzuweisung Zuweisung Rollenbezeichner angegeben.

Informationen zur Zuweisung der Sicherheitsrolle haben Sie Zugriff auf `Microsoft.Authorization/roleAssignments/read` Vorgang. Die integrierten Rollen erhalten Zugriff auf diesen Vorgang. Weitere Informationen über Rollen zugewiesen und Verwalten von Azure Ressourcen Siehe [Azure Role-Based zugreifen](role-based-access-control-configure.md).

### <a name="request"></a>Anforderung

Verwenden der **GET** -Methode mit der folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Festlegen des URIS die folgenden Ersatzartikel Wunsch anpassen

1. Ersetzen Sie *{Bereich}* mit der Rolle Aufgaben aufgelistet werden sollen. Die folgenden Beispiele veranschaulichen die Bereich für verschiedene Ebenen festzulegen:

  - Abonnement: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Der GUID-Bezeichner der rollenzuordnung *{Rolle-Aufgaben-Id}* ersetzen.

3. 2015-07-01 *{api-Version}* ersetzen.

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Erstellen Sie eine Rolle Aufgabe

Erstellen Sie eine Rolle Aufgabe im angegebenen Bereich für den angegebenen Prinzipal die angegebene Rolle erteilen.

Erstellen Sie eine rollenzuordnung benötigen Sie Zugriff auf `Microsoft.Authorization/roleAssignments/write` Vorgang. Der integrierten Rollen erhalten nur *Besitzer* und *Benutzer Zugriff Administrator* diesen Vorgang ausführen. Weitere Informationen über Rollen zugewiesen und Verwalten von Azure Ressourcen Siehe [Azure Role-Based zugreifen](role-based-access-control-configure.md).

### <a name="request"></a>Anforderung

Verwenden der **PUT** -Methode mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Festlegen des URIS die folgenden Ersatzartikel Wunsch anpassen

1. Ersetzen Sie *{Bereich}* mit dem Bereich an dem Sie die Rolle Arbeitsaufträge erstellen möchten. Beim Erstellen einer Aufgabe Rolle in einem übergeordneten Bereich erben alle untergeordneten Bereiche dieselbe Rolle Zuordnung. Die folgenden Beispiele veranschaulichen die Bereich für verschiedene Ebenen festzulegen:

  - Abonnement: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups myresourcegroup1   
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Eine neue GUID der GUID-Bezeichner der neuen Rolle Zuordnung wird ersetzen Sie *{Rolle-Aufgaben-Id}* .

3. 2015-07-01 *{api-Version}* ersetzen.

Der Anforderungstext vorsehen Sie Werte im folgenden Format:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Elementnamen     | Erforderlich | Typ   | Beschreibung |
|------------------|----------|--------|-------------|
| roleDefinitionId | Ja      | Zeichenfolge | Der Bezeichner der Rolle. Das Format des Bezeichners ist:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | Ja      | Zeichenfolge | die ObjectId des Azure AD Principals (Benutzer, Gruppe oder Dienstprinzipalnamen), die die Rolle zugeordnet ist. |

### <a name="response"></a>Antwort

Statuscode: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Rollenzuweisung einer löschen

Löschen einer Rolle Zuordnung im angegebenen Bereich.

Zum Löschen einer rollenzuordnung benötigen Sie Zugriff auf die `Microsoft.Authorization/roleAssignments/delete` Vorgang. Der integrierten Rollen erhalten nur *Besitzer* und *Benutzer Zugriff Administrator* diesen Vorgang ausführen. Weitere Informationen über Rollen zugewiesen und Verwalten von Azure Ressourcen Siehe [Azure Role-Based zugreifen](role-based-access-control-configure.md).

### <a name="request"></a>Anforderung

Verwenden Sie die **DELETE** -Methode mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

Festlegen des URIS die folgenden Ersatzartikel Wunsch anpassen

1. Ersetzen Sie *{Bereich}* mit dem Bereich an dem Sie die Rolle Arbeitsaufträge erstellen möchten. Die folgenden Beispiele veranschaulichen die Bereich für verschiedene Ebenen festzulegen:

  - Abonnement: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Die Funktion Zuordnung Id GUID *{Rolle-Aufgaben-Id}* ersetzen.

3. 2015-07-01 *{api-Version}* ersetzen.

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Alle Rollen auflisten

Listet alle Funktionen, die für die Zuordnung im angegebenen Bereich.

Liste Rollen benötigen Sie Zugriff auf `Microsoft.Authorization/roleDefinitions/read` auf den Bereich. Die integrierten Rollen erhalten Zugriff auf diesen Vorgang. Weitere Informationen über Rollen zugewiesen und Verwalten von Azure Ressourcen Siehe [Azure Role-Based zugreifen](role-based-access-control-configure.md).

### <a name="request"></a>Anforderung

Verwenden der **GET** -Methode mit der folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

Festlegen des URIS die folgenden Ersatzartikel Wunsch anpassen

1. Ersetzen Sie *{Bereich}* mit dem Bereich Rollen aufgelistet werden sollen. Die folgenden Beispiele veranschaulichen die Bereich für verschiedene Ebenen festzulegen:

  - Abonnement: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups myresourcegroup1  
  - Resource /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. 2015-07-01 *{api-Version}* ersetzen.

3. Ersetzen Sie *{Filter}* durch die Bedingung, die Sie zum Filtern der Liste der Rollen übernehmen möchten:

  - Liste Rollen zur Zuordnung im angegebenen Bereich und alle seine untergeordneten Bereiche:`atScopeAndBelow()`
  - Suche für eine Rolle mit genauen Anzeigenamen: `roleName%20eq%20'{role-display-name}'`. Verwenden der URL-codiert den genauen Anzeigenamen der Rolle. Zum Beispiel`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Informationen Sie zu einer Rolle

Ruft Informationen über eine bestimmte Rolle Definition Rollenbezeichner angegeben. Informationen zu einer bestimmten Rolle mit den Anzeigenamen finden Sie in der [Liste alle Rollen](role-based-access-control-manage-access-rest.md#list-all-roles).

Informationen zu einer Rolle haben Sie Zugriff auf `Microsoft.Authorization/roleDefinitions/read` Vorgang. Die integrierten Rollen erhalten Zugriff auf diesen Vorgang. Weitere Informationen über Rollen zugewiesen und Verwalten von Azure Ressourcen Siehe [Azure Role-Based zugreifen](role-based-access-control-configure.md).

### <a name="request"></a>Anforderung

Verwenden der **GET** -Methode mit der folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Festlegen des URIS die folgenden Ersatzartikel Wunsch anpassen

1. Ersetzen Sie *{Bereich}* mit der Rolle Aufgaben aufgelistet werden sollen. Die folgenden Beispiele veranschaulichen die Bereich für verschiedene Ebenen festzulegen:

  - Abonnement: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie *{Rolle Definition Id}* mit der GUID-Bezeichner für die Funktionsdefinition.

3. 2015-07-01 *{api-Version}* ersetzen.

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Erstellen Sie eine benutzerdefinierte Rolle
Erstellen Sie eine benutzerdefinierte Rolle.

Erstellen Sie eine benutzerdefinierte Rolle haben Sie Zugriff auf `Microsoft.Authorization/roleDefinitions/write` Vorgang auf dem `AssignableScopes`. Der integrierten Rollen erhalten nur *Besitzer* und *Benutzer Zugriff Administrator* diesen Vorgang ausführen. Weitere Informationen über Rollen zugewiesen und Verwalten von Azure Ressourcen Siehe [Azure Role-Based zugreifen](role-based-access-control-configure.md).

### <a name="request"></a>Anforderung

Verwenden der **PUT** -Methode mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Festlegen des URIS die folgenden Ersatzartikel Wunsch anpassen

1. Die erste *AssignableScope* der benutzerdefinierten Rolle *{Bereich}* ersetzen. Den folgenden Beispielen für verschiedene Ebenen Bereich angeben.

  - Abonnement: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Eine neue GUID der GUID-Bezeichner für die neue benutzerdefinierte Rolle wird ersetzen Sie *{Rolle Definition Id}* .

3. 2015-07-01 *{api-Version}* ersetzen.

Der Anforderungstext vorsehen Sie Werte im folgenden Format:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elementnamen | Erforderlich | Typ | Beschreibung |
|--------------|----------|------|-------------|
| Name         | Ja | Zeichenfolge   | GUID-Bezeichner der benutzerdefinierten Rolle.    |
| properties.roleName               | Ja | Zeichenfolge   | Name der benutzerdefinierten Funktion angezeigt. Maximal 128 Zeichen.                        |
| Properties.Description            | Nein  | Zeichenfolge   | Beschreibung der benutzerdefinierten Rolle. Maximale Größe von 1024 Zeichen.                                               |
| Properties.Type                   | Ja | Zeichenfolge   | "CustomRole" festgelegt                                         |
| Properties.Permissions.Actions    | Ja | String] | Ein Zeichenfolgenarray Aktion, durch die benutzerdefinierte Rolle gewährten Operationen angibt.             |
| properties.permissions.notActions | Nein  | String] | Ein Zeichenfolgenarray Aktion, Operationen ausgeschlossen werden durch die benutzerdefinierte Rolle gewährten Operationen angibt. |
| properties.assignableScopes       | Ja | String] | Ein Array von Bereichen, in denen benutzerdefinierte Rolle verwendet werden kann.   |

### <a name="response"></a>Antwort

Statuscode: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Eine benutzerdefinierte Rolle aktualisieren

Ändern Sie eine benutzerdefinierte Rolle.

Um eine benutzerdefinierte Rolle ändern, müssen Sie Zugriff auf `Microsoft.Authorization/roleDefinitions/write` Vorgang auf dem `AssignableScopes`. Der integrierten Rollen erhalten nur *Besitzer* und *Benutzer Zugriff Administrator* diesen Vorgang ausführen. Weitere Informationen über Rollen zugewiesen und Verwalten von Azure Ressourcen Siehe [Azure Role-Based zugreifen](role-based-access-control-configure.md).

### <a name="request"></a>Anforderung

Verwenden der **PUT** -Methode mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Festlegen des URIS die folgenden Ersatzartikel Wunsch anpassen

1. Die erste *AssignableScope* der benutzerdefinierten Rolle *{Bereich}* ersetzen. Die folgenden Beispiele veranschaulichen die Bereich für verschiedene Ebenen festzulegen:

  - Abonnement: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Der GUID-Bezeichner der benutzerdefinierten Rolle *{Rolle Definition Id}* ersetzen.

3. 2015-07-01 *{api-Version}* ersetzen.

Der Anforderungstext vorsehen Sie Werte im folgenden Format:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elementnamen | Erforderlich | Typ | Beschreibung |
|--------------|----------|------|-------------|
| Name         | Ja      | Zeichenfolge | GUID-Bezeichner der benutzerdefinierten Rolle. |
| properties.roleName | Ja | Zeichenfolge | Anzeigename der aktualisierte benutzerdefinierte Rolle. |
| Properties.Description | Nein | Zeichenfolge | Beschreibung der aktualisierte benutzerdefinierte Rolle. |
| Properties.Type | Ja | Zeichenfolge | "CustomRole" festgelegt |
| Properties.Permissions.Actions | Ja | String] | Ein Zeichenfolgenarray Aktion angeben die Vorgänge, die aktualisierte benutzerdefinierte Rolle Zugriff gewährt. |
| properties.permissions.notActions | Nein | String] | Ein Zeichenfolgenarray Aktion angeben die durchzuführenden Operationen ausschließen aktualisierte benutzerdefinierte Rolle gewährt. |
| properties.assignableScopes | Ja | String] | Ein Array von Bereichen, in denen aktualisierte benutzerdefinierte Rolle verwendet werden kann. |

### <a name="response"></a>Antwort

Statuscode: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Eine benutzerdefinierte Rolle löschen

Löschen Sie eine benutzerdefinierte Rolle

Um eine benutzerdefinierte Rolle zu löschen, müssen Sie Zugriff auf `Microsoft.Authorization/roleDefinitions/delete` Vorgang auf dem `AssignableScopes`. Der integrierten Rollen erhalten nur *Besitzer* und *Benutzer Zugriff Administrator* diesen Vorgang ausführen. Weitere Informationen über Rollen zugewiesen und Verwalten von Azure Ressourcen Siehe [Azure Role-Based zugreifen](role-based-access-control-configure.md).

### <a name="request"></a>Anforderung

Verwenden Sie die **DELETE** -Methode mit dem folgenden URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

Festlegen des URIS die folgenden Ersatzartikel Wunsch anpassen

1. Ersetzen Sie *{Bereich}* mit dem Bereich an dem Sie die Funktionsdefinition löschen möchten. Die folgenden Beispiele veranschaulichen die Bereich für verschiedene Ebenen festzulegen:

  - Abonnement: /subscriptions/ {Abonnement-Id}  
  - Ressourcengruppe: /subscriptions/ {Abonnement-Id} / ResourceGroups myresourcegroup1  
  - Ressource: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Ersetzen Sie *{Rolle Definition Id}* mit der Id GUID Rolle Definition der benutzerdefinierten Funktion.

3. 2015-07-01 *{api-Version}* ersetzen.

### <a name="response"></a>Antwort

Statuscode: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
