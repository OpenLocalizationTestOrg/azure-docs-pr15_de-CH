<properties
   pageTitle="Bereitstellungsvorgänge mit Azure CLI anzeigen | Microsoft Azure"
   description="Beschreibt, wie mithilfe der Azure-CLI Ressourcenmanager Bereitstellung Probleme ermittelt."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Bereitstellungsvorgänge mit Azure CLI anzeigen

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Wenn Sie einen Fehler Ressourcen in Azure bereitstellen erhalten, möchten Sie mehr über die Bereitstellungsvorgänge angezeigt, die ausgeführt wurden. Azure CLI bietet Befehle, mit denen Sie den Fehler und mögliche Updates ermitteln.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Sie können Fehler vermeiden, indem Ihre Vorlage und die Infrastruktur vor der Bereitstellung überprüfen. Sie können auch zusätzliche Anforderung und Antwortinformationen während der Bereitstellung, die später zur Fehlerbehebung protokollieren. Überprüfung und Protokollierung Anforderung und Antwort Informationen finden Sie unter [Bereitstellen einer Ressourcengruppe Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy-cli.md).

## <a name="use-audit-logs-to-troubleshoot"></a>Verwenden von Überwachungsprotokollen beheben

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Fehler bei einer Bereitstellung finden gehen Sie folgendermaßen vor:

1. Um die Überwachungsprotokolle anzuzeigen, führen Sie den Befehl **Azure Gruppe Protokoll anzeigen** . Enthalten Sie die **-letzte Bereitstellung** Option das Protokoll der letzten Bereitstellung abgerufen.

        azure group log show ExampleGroup --last-deployment

2. Der Befehl **Azure Gruppe Protokoll anzeigen** gibt viele Informationen. Für die Problembehandlung soll in der Regel darauf Operationen, die fehlgeschlagen. Das folgende Skript verwendet die **Json -** Option und [Jq](https://stedolan.github.io/jq/) JSON-Dienstprogramm Protokoll Bereitstellungsfehlern suchen.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    Sie sehen, dass **Eigenschaften** in Json über den fehlgeschlagenen Vorgang Informationen enthält.

    Verwenden Sie die **– ausführliche** und **Vv -** Informationen aus den Protokollen.  Verwenden der **– ausführliche** Option zum Anzeigen Vorgänge durch Weiter `stdout`. Für eine vollständige Anforderung Verlauf die Option **- Vv** . Die Nachrichten bieten oft wichtige Hinweise zur Ursache von Fehlern.

3. Verwenden Sie den folgenden Befehl auf fehlerhafte Einträge angezeigt:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Verwenden Sie Bereitstellungsvorgänge Problembehandlung

1. Abrufen des allgemeinen Status einer Bereitstellung mit dem Befehl **show Azure bereitstellen** . Im folgenden Beispiel ist die Bereitstellung fehlgeschlagen.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. Um die Meldung für fehlgeschlagene Vorgänge für eine Bereitstellung verwenden:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Nächste Schritte

- Hilfe zu bestimmten Bereitstellungsfehler beheben finden Sie unter [beheben Sie häufige Fehler bei der Bereitstellung von Ressourcen in Azure mit Azure-Ressourcen-Manager](resource-manager-common-deployment-errors.md).
- Weitere Informationen zur Verwendung der Überwachungsprotokolle andere Aktionen überwachen, finden Sie unter [Vorgänge mit Ressourcen-Manager überwachen](resource-group-audit.md).
- Überprüfen Sie die Bereitstellung vor der Ausführung finden Sie unter [Bereitstellen einer Ressourcengruppe Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).
