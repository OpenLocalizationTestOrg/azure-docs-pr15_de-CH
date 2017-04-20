<properties
   pageTitle="Bereitstellungsvorgänge mit PowerShell anzeigen | Microsoft Azure"
   description="Beschreibt das Azure PowerShell Ressourcenmanager Bereitstellung Probleme erkennen."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Bereitstellungsvorgänge Azure PowerShell anzeigen

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Sie können die Vorgänge für eine Bereitstellung über Azure PowerShell anzeigen. Möglicherweise interessiert Sie Fehler während der Bereitstellung erhalten haben, damit dieser Artikel konzentriert sich auf Vorgänge, die nicht die Vorgänge anzeigen. PowerShell bietet Cmdlets, mit denen Sie auf einfache Weise die Fehler und mögliche Updates ermitteln.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Sie können Fehler vermeiden, indem Ihre Vorlage und die Infrastruktur vor der Bereitstellung überprüfen. Sie können auch zusätzliche Anforderung und Antwortinformationen während der Bereitstellung, die später zur Fehlerbehebung protokollieren. Überprüfung und Protokollierung Anforderung und Antwort Informationen finden Sie unter [Bereitstellen einer Ressourcengruppe Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).

## <a name="use-deployment-operations-to-troubleshoot"></a>Verwenden Sie Bereitstellungsvorgänge Problembehandlung

1. Zu den allgemeinen Status einer Bereitstellung mit dem Befehl **Get-AzureRmResourceGroupDeployment** . Sie können die Ergebnisse für diese Bereitstellung filtern, die fehlgeschlagen sind.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Die fehlgeschlagenen Installationen im folgenden Format zurückgegeben:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Jede Bereitstellung besteht in der Regel mehrere Vorgänge mit jeder Vorgang einen Schritt im Bereitstellungsprozess. Um Bereitstellung Ursache zu ermitteln, müssen Sie normalerweise über die Bereitstellungsvorgänge angezeigt. Sie sehen den Status der Vorgänge mit **Get-AzureRmResourceGroupDeploymentOperation**.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Die mehrere Vorgänge jeweils im folgenden Format zurückgegeben:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Um weitere Informationen über fehlgeschlagene Vorgänge erhalten, rufen Sie die Eigenschaften mit Status **Fehler ab** .

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Die alle fehlgeschlagenen Vorgänge jeweils im folgenden Format zurückgegeben:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Beachten Sie die Tracking-ID für den Vorgang. Die benötigen im nächsten Schritt eine bestimmte Operation konzentrieren.

4. Um Statusnachricht eines bestimmten fehlgeschlagenen Vorgangs abzurufen, verwenden Sie den folgenden Befehl:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Die zurückgibt:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Verwenden von Überwachungsprotokollen beheben

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Fehler bei einer Bereitstellung finden gehen Sie folgendermaßen vor:

1. Rufen Sie Protokolleinträge führen Sie den Befehl **Get-AzureRmLog aus** . **ResourceGroup** und **Status** -Parameter können nur Ereignisse zurück, die für eine einzelne Ressource fehlgeschlagen. Wenn Sie Start-und End nicht angeben, werden die Einträge der letzten Stunde zurückgegeben.
Beispielsweise rufen ausführen fehlgeschlagene Vorgänge der letzten Stunde:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    Sie können eine bestimmte Zeitspanne angeben. Im nächsten Beispiel erfahren fehlgeschlagene Aktionen für den letzten Tag. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Oder Sie können eine genaue Start- und Endzeit für fehlgeschlagene Aktionen festlegen:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Wenn dieser Befehl zu viele Einträge und Eigenschaften zurückgibt, können Sie die überwachen konzentrieren durch die **Properties** -Eigenschaft abrufen. Den **DetailedOutput** -Parameter, um die Fehlermeldungen angezeigt werden wir auch.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Die Eigenschaften der Protokolleinträge im folgenden Format zurückgegeben:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Anhand dieser Ergebnisse genauerer auf das zweite Element. Sie können die Ergebnisse verfeinern, anhand der Statusanzeige für diesen Eintrag.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Die zurückgibt:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Nächste Schritte

- Hilfe zu bestimmten Bereitstellungsfehler beheben finden Sie unter [beheben Sie häufige Fehler bei der Bereitstellung von Ressourcen in Azure mit Azure-Ressourcen-Manager](resource-manager-common-deployment-errors.md).
- Weitere Informationen zur Verwendung der Überwachungsprotokolle andere Aktionen überwachen, finden Sie unter [Vorgänge mit Ressourcen-Manager überwachen](resource-group-audit.md).
- Überprüfen Sie die Bereitstellung vor Ausführung finden Sie unter [Bereitstellen einer Ressourcengruppe Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).

