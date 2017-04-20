<properties 
    pageTitle="Sperren von Ressourcen mit dem Ressourcen-Manager | Microsoft Azure" 
    description="Benutzer aktualisieren oder Löschen bestimmte Ressourcen durch Anwenden einer Einschränkung auf alle Benutzer und Rollen." 
    services="azure-resource-manager" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tomfitz"/>

# <a name="lock-resources-with-azure-resource-manager"></a>Lock-Ressourcen mit Azure-Ressourcen-Manager

Als Administrator müssen Sie ein Abonnement, Ressourcengruppe oder Ressource, um zu verhindern, dass andere Benutzer in Ihrer Organisation versehentlich löschen oder Ändern von kritischen Ressourcen sperren. Sie können der Sperrebene **CanNotDelete** oder **schreibgeschützt**festlegen. 

- **CanNotDelete** bedeutet, dass autorisierte Benutzer können weiterhin lesen und ändern eine Ressource jedoch nicht löschen. 
- **ReadOnly** bedeutet, dass autorisierte Benutzer können aus einer Ressource gelesen aber nicht löschen oder Aktionen auf. Die Ressource-Berechtigung ist auf **Rolle** . 

**ReadOnly** anwenden kann zu unerwarteten Ergebnissen führen, da einige Vorgänge wie Vorgänge tatsächlich zusätzliche Aktionen erfordern gelesen. Beispielsweise verhindert eine **schreibgeschützte** Sperre ein Speicherkonto Inverkehrbringen alle Schlüssel aufgelistet. Vorgang: Schlüssel über eine POST-Anforderung verarbeitet wird, da die zurückgegebenen Schlüssel für verfügbaren Liste Schreibvorgänge. Beispielsweise verhindert eine App-Dienstressource eine **schreibgeschützte** Sperre Inverkehrbringen Visual Studio Server Explorer Dateien für die Ressource angezeigt, da diese Schreibzugriff erforderlich sind.

Im Gegensatz zu rollenbasierte Zugriffskontrolle können Sie Management Sperren gilt für alle Benutzer und Rollen. Zum Festlegen von Berechtigungen für Benutzer und Rollen finden Sie unter [Azure Role-based Access Control](./active-directory/role-based-access-control-configure.md).

Wenn Sie eine Sperre in einem übergeordneten Bereich anwenden, erben alle untergeordneten Ressourcen dieselbe Sperre. Sogar Ressourcen, die Sie später hinzufügen erben die Sperre vom übergeordneten Element. Die restriktivste Sperre der Vererbung hat Vorrang.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Die erstellen oder Löschen von Sperren in Ihrer Organisation

Erstellen oder Löschen von Management sperren, haben Sie Zugriff auf **Microsoft.Authorization/\* ** oder **Microsoft.Authorization/locks/\* ** Aktionen. Integrierte Rollen werden diese Aktionen nur **Besitzer** und **Benutzer Zugriff Administrator** erteilt werden.

## <a name="creating-a-lock-through-the-portal"></a>Erstellen eine Sperre über das portal

[AZURE.INCLUDE [resource-manager-lock-resources](../includes/resource-manager-lock-resources.md)]

## <a name="creating-a-lock-in-a-template"></a>Erstellen eine Sperre in einer Vorlage

Das folgende Beispiel zeigt eine Vorlage, die eine Sperre auf ein Speicherkonto. Das Speicherkonto, die Sperre angewendet wird als Parameter bereitgestellt. Der Name der Sperre wird durch Verkettung der Ressourcenname mit **/Microsoft.Authorization/** und der Name der Sperre, in diesem Fall **MyLock**erstellt.

Bereitgestellten Typ ist spezifisch für den Ressourcentyp. Speicher festlegen Sie, "Microsoft.Storage/storageaccounts/providers/locks".

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "lockedResource": {
          "type": "string"
        }
      },
      "resources": [
        {
          "name": "[concat(parameters('lockedResource'), '/Microsoft.Authorization/myLock')]",
          "type": "Microsoft.Storage/storageAccounts/providers/locks",
          "apiVersion": "2015-01-01",
          "properties": {
            "level": "CannotDelete"
          }
        }
      ]
    }

## <a name="creating-a-lock-with-rest-api"></a>Erstellen eine Sperre mit REST-API

Die [REST API für Management Sperren](https://msdn.microsoft.com/library/azure/mt204563.aspx)bereitgestellte Ressourcen können gesperrt werden. Die REST-API können Sie zum Erstellen und Löschen sperren und Abrufen von Informationen über bestehende Sperren.

Um eine Sperre zu erstellen, führen Sie Folgendes aus:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

Der Bereich konnte ein Abonnement, eine Ressourcengruppe oder eine Ressource sein. Lock-Name ist beliebig Sperre aufgerufen. Verwenden Sie für api-Version **2015-01-01**.

Enthalten Sie in der Anforderung ein JSON-Objekt, das die Eigenschaften für die Sperre angibt.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

Beispiele finden Sie unter [REST API für Management Sperren](https://msdn.microsoft.com/library/azure/mt204563.aspx).

## <a name="creating-a-lock-with-azure-powershell"></a>Erstellen eine Sperre Azure PowerShell

Azure PowerShell bereitgestellte Ressourcen Sperren mit **Neuen AzureRmResourceLock** wie im folgenden Beispiel gezeigt.

    New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

Azure PowerShell enthält weitere Befehle zum Sperren **Set AzureRmResourceLock** eine Sperre aktualisieren und löschen eine Sperre **Entfernen AzureRmResourceLock** arbeiten.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Arbeiten mit Ressourcen finden Sie unter [Sperren Sie Ihre Azure-Ressourcen](http://blogs.msdn.com/b/cloud_solution_architect/archive/2015/06/18/lock-down-your-azure-resources.aspx)
- Logisch Organisieren Ihrer Ressourcen finden Sie unter [Using Tags zu Ressourcen](resource-group-using-tags.md)
- Ändern der Ressourcengruppe in eine Ressource befindet, siehe [Ressourcen neue Ressourcengruppe](resource-group-move-resources.md)
- Sie können eingeschränkt und Konventionen für Ihr Abonnement mit angepassten Richtlinien anwenden. Weitere Informationen finden Sie unter [Verwenden von Richtlinien zum Verwalten von Ressourcen und Zugriffskontrolle](resource-manager-policy.md).
