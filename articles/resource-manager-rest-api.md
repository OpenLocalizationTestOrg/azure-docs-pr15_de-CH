<properties
   pageTitle="Ressourcenmanager REST APIs | Microsoft Azure"
   description="Übersicht der Ressourcenmanager REST APIs Authentifizierung und Verwendung"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Ressourcenmanager REST-APIs

> [AZURE.SELECTOR]
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Portal](./azure-portal/resource-group-portal.md) 
- [REST-API](resource-manager-rest-api.md)

Hinter jedem Aufruf an Azure-Ressourcen-Manager hinter jeder bereitgestellten Vorlage hinter jedem konfigurierten Speicherkonto gibt ein oder mehrere Aufrufe der Azure-Ressourcenmanager RESTful API. In diesem Thema widmet sich diese APIs und wie Sie diese ohne jede SDK gar aufrufen können. Dies ist sehr nützlich, wenn Sie Vollzugriff auf alle Anfragen zu Azure SDK für Ihre bevorzugte Sprache ist nicht verfügbar oder unterstützt keine Vorgänge ausführen möchten.

Dieser Artikel wird geht nicht über jede API in Azure verfügbar gemacht wird, aber vielmehr einige so wie weiter zu verbinden. Wenn Sie die Grundlagen verstehen, dann weiter und [Azure Ressourcenmanager REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn790568.aspx) finden Sie ausführliche Informationen zur Verwendung von Rest-APIs gelesen.

## <a name="authentication"></a>Authentifizierung
Authentifizierung für ARM wird von Azure Active Directory (AD) behandelt. Verbindung an eine beliebige API Azure AD ein Authentifizierungstoken zu authentifizieren muss, kann jede Anfrage weitergeben. Als wir einen reinen Aufruf direkt die REST-APIs beschreiben, nehmen wir auch, wo ein pop-up-Bildschirm Benutzername und Kennwort aufgefordert und vielleicht andere Authentifizierungsmechanismen verwendet zwei-Faktor-Authentifizierungsszenarien sogar kann, mit einem normalen Benutzername Kennwort authentifizieren möchten. Daher erstellen wir genannte Azure AD-Anwendung und einen Dienstprinzipal zu verwendende Anmeldung mit. Jedoch, Azure AD unterstützen mehrere Authentifizierungsverfahren und alle abzurufenden für nachfolgende Anforderungen API, Authentifizierungstoken verwendet werden kann.
Schritt für Schritt führen Sie [Erstellen Azure AD-Anwendung und Service](./resource-group-create-service-principal-portal.md) .

### <a name="generating-an-access-token"></a>Ein Zugriffstoken generiert 
Authentifizierung über Azure AD erfolgt zu Azure AD am login.microsoftonline.com. Um Sie authentifizieren, muss die folgende Informationen:

* Azure AD-Mandanten-ID (Name, Azure Anzeige anmelden Verwendung, häufig als Ihr Unternehmen aber nicht erforderlich)
* ID der Anwendung (bei Erstellung der Azure AD-Anwendung übernommen)
* Kennwort (die Auswahl beim Erstellen der Azure AD-Anwendung)

In der HTTP-Anfrage unbedingt die Werte "Azure AD Mandanten-ID", "Application ID" und "Kennwort" ersetzen.

**Generische HTTP-Anforderung:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... wird (wenn die Authentifizierung erfolgreich) führt eine ähnliche Antwort:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Zugriffstoken oben auf wurden wurde gekürzt, um die Lesbarkeit zu erhöhen)

**Erstellen von Zugriffstoken mit Bash:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Erstellen von Zugriffstoken mit PowerShell:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Die Antwort enthält ein Zugriffstoken Informationen wie lange Tokens gültig ist und Informationen darüber, welche Ressourcen Sie Tokens für.
Zugriffstoken HTTP-Anruf erhalten muss in alle Anforderung ARM-API als Header namens "Autorisierung" mit dem Wert "Träger YOUR_ACCESS_TOKEN" übergeben werden. Beachten Sie das Leerzeichen zwischen "Inhaber" und der Zugriffstoken.

Siehe aus dem obigen Ergebnis HTTP, gilt das Token für einen bestimmten Zeitraum während der Sie zwischenspeichern und Wiederverwendung, ebenso. Auch wenn Azure AD für jede API-Aufruf authentifizieren kann, wäre es sehr ineffizient.

## <a name="calling-arm-rest-apis"></a>ARM REST-APIs aufrufen

[Azure Ressourcenmanager REST-APIs werden im folgenden beschrieben](https://msdn.microsoft.com/library/azure/dn790568.aspx) , und es ist außerhalb des Gültigkeitsbereichs für dieses Lernprogramm die Verwendung jeder dokumentieren. Verwenden dieser Dokumentation nur wenige APIs erläutern die grundlegende Verwendung der APIs und wir verweisen Sie in der offiziellen Dokumentation.

### <a name="list-all-subscriptions"></a>Liste aller Abonnements

Eine der einfachsten Operationen können ist verfügbaren Abonnements aufgelistet, auf die Sie zugreifen können. In der Anfrage sehen, wie das Zugriffstoken als Header übergeben wird.

(Ersetzen Sie YOUR_ACCESS_TOKEN durch die tatsächliche Zugriffstoken.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... und dadurch erhalten Sie eine Liste der Abonnements dieses Service Principal zugreifen darf

(Abonnement-IDs unten wurden zur besseren Lesbarkeit gekürzt)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Liste aller Ressourcengruppen ein bestimmtes Abonnement

In einer Ressourcengruppe sind alle Ressourcen mit ARM-APIs geschachtelt. Werden wir Abfrage ARM für vorhandene Ressourcengruppen mit unserem Abonnement die folgenden HTTP-Anforderung abzurufen. Beachten Sie, wie die Abonnement-ID als Teil der URL dieses Mal übergeben wird.

(Ersetzen Sie YOUR_ACCESS_TOKEN und SUBSCRIPTION_ID mit tatsächlichen Zugriffstoken und Abonnement-ID)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Sie erhalten Antwort hängt davon ab, ob Sie alle Ressourcengruppen definiert haben und wie viele.

(Abonnement-IDs unten wurden zur besseren Lesbarkeit gekürzt)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Bisher wir haben nur wurden Abfragen ARM APIs Informationen, Zeit, wir einige Ressourcen stattdessen und beginnen die einfachste von allen, eine Ressourcengruppe. Die folgende HTTP-Anforderung erstellt eine neue Ressourcengruppe Region/Ort Ihrer Wahl und ein oder mehrere Tags hinzugefügt (im Beispiel unten nur Fügt ein Tag).

(Ersetzen Sie YOUR_ACCESS_TOKEN, SUBSCRIPTION_ID, RESOURCE_GROUP_NAME mit Ihren aktuellen Zugriffstoken Abonnement-ID und Name der zu erstellenden Ressource)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Wenn erfolgreich, erhalten Sie eine ähnliche Antwort

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Sie haben erfolgreich eine Ressourcengruppe in Azure erstellt. Herzlichen Glückwunsch!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>Bereitstellen von Ressourcen einer Ressourcengruppe mit einer ARM-Vorlage

Mit ARM können Sie Ihre Ressourcen mit ARM-Vorlagen bereitstellen. Ein ARM-Vorlage definiert verschiedene Ressourcen und deren abhängigen Dateien. Für diesen Abschnitt nehmen wir einfach Sie mit ARM-Vorlagen vertraut sind und nur zeigen wir Ihnen zu den API-Aufruf zum Starten der Bereitstellung eines. Eine detaillierte Dokumentation ARM-Vorlagen finden Sie hier.

Bereitstellung einer ARM-Vorlage nicht wesentlich unterscheiden sich wie andere APIs aufrufen. Ein wichtiger Aspekt ist die Bereitstellung einer Vorlage kann ziemlich lange dauern, je nach innerhalb der Vorlage und der API-Aufruf einfach zurückgegeben und von Ihnen als Entwickler Abfragen Status der Bereitstellung um nach Abschluss die Bereitstellung.

In diesem Beispiel verwenden wir eine öffentlich verfügbar gemachte ARM-Vorlage auf [GitHub](https://github.com/Azure/azure-quickstart-templates). Die Vorlage, die wir verwenden wird Linux VM West USA bereitgestellt. Auch wenn diese Vorlage die Vorlage in einem öffentlichen Repository wie GitHub, können Sie auch die vollständige Vorlage als Teil der Anforderung übergeben auswählen. Beachten Sie, dass wir Parameterwerte als Teil der Anforderung, die in der verwendeten Vorlage verwendet wird.

(Ersetzen Sie SUBSCRIPTION_ID, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD und DNS_NAME_FOR_PUBLIC_IP Werte für Ihre Anforderung)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Lange JSON-Antwort für diese Anforderung wurde ausgelassen, um die Lesbarkeit dieser Dokumentation. Die Antwort enthält Informationen zur Bereitstellung mit Vorlagen, die Sie gerade erstellt haben.

