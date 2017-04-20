<properties
   pageTitle="Skalieren der ACS-Cluster mit der Azure-CLI | Microsoft Azure"
   description="Das Skalieren von Azure Container Service Cluster mithilfe der Azure-CLI."
   services="container-service"
   documentationCenter=""
   authors="Thraka"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Andockfenster Container Micro-Services Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="timlt"/>

# <a name="scale-an-azure-container-service"></a>Einen Azure Containerservice skalieren

Sie können die Anzahl der Knoten skalieren Azure Container Service (ACS) hat mit dem Azure-CLI-Tool. Verwendung der Azure-CLI skalieren, gibt das Tool die eine neuen Konfigurationsdatei, die auf Container angewendete Änderung darstellt.

## <a name="about-the-command"></a>Über den Befehl

Azure-Ressourcen-Manager-Modus für die Interaktion mit Azure Container muss der Azure-CLI. Sie können auf den Ressourcen-Manager-Modus durch Aufrufen von `azure config mode arm`. Die `acs` Befehl hat einen untergeordnete Befehl mit dem Namen `scale` die Skalierung Operationen für einen containerdienst ausführt. Sie können über die verschiedenen Parameter im Befehl Skalieren mit Hilfe `azure acs scale --help`, die Ausgaben etwa so:

```azurecli
azure acs scale --help

help:    The operation to scale a container service.
help:
help:    Usage: acs scale [options] <resource-group> <name> <new-agent-count>
help:
help:    Options:
help:      -h, --help                               output usage information
help:      -v, --verbose                            use verbose output
help:      -vv                                      more verbose with debug output
help:      --json                                   use json output
help:      -g, --resource-group <resource-group>    resource-group
help:      -n, --name <name>                        name
help:      -o, --new-agent-count <new-agent-count>  New agent count
help:      -s, --subscription <subscription>        The subscription identifier
help:
help:    Current Mode: arm (Azure Resource Management)
```

## <a name="use-the-command-to-scale"></a>Verwenden Sie den Befehl Skalieren

Um einen containerdienst skalieren, müssen Sie der **Ressourcengruppe** und **Azure Container Service (ACS) Namen**, und auch die neue Anzahl von Agents angeben. Mit einem kleineren oder höher, können Sie skalieren bzw. oben oder unten.

Sie möchten wissen, was die aktuelle Anzahl der Agents für einen containerservice bevor skalieren. Verwenden der `azure acs show <resource group> <ACS name>` Befehl Config ACS zurückgegeben. Beachten Sie das <mark>Count</mark> -Ergebnis.

#### <a name="see-current-count"></a>Aktuelle Anzahl

```azurecli
azure acs show containers-test containerservice-containers-test

info:    Executing command acs show
data:
data:     Id                 : /subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test
data:     Name               : containerservice-containers-test
data:     Type               : Microsoft.ContainerService/ContainerServices
data:     Location           : westus
data:     ProvisioningState  : Succeeded
data:     OrchestratorProfile
data:       OrchestratorType : DCOS
data:     MasterProfile
data:       Count            : 1
data:       DnsPrefix        : myprefixmgmt
data:       Fqdn             : myprefixmgmt.westus.cloudapp.azure.com
data:     AgentPoolProfiles
data:       #0
data:         Name           : agentpools
data:         <mark>Count          : 1</mark>
data:         VmSize         : Standard_D2
data:         DnsPrefix      : myprefixagents
data:         Fqdn           : myprefixagents.westus.cloudapp.azure.com
data:     LinuxProfile
data:       AdminUsername    : azureuser
data:       Ssh
data:         PublicKeys
data:           #0
data:             KeyData    : ssh-rsa <ENCODED VALUE>
data:     DiagnosticsProfile
data:       VmDiagnostics
data:         Enabled        : true
data:         StorageUri     : https://<storageid>.blob.core.windows.net/
```  

#### <a name="scale-to-new-count"></a>Neuen skalieren

Wahrscheinlich klar ist, skalieren Sie containerservice durch Aufrufen von `azure acs scale` und die **Ressourcengruppe** **ACS Name**und **Anzahl an Agenten**. Beim Skalieren von containerdienst gibt Azure CLI eine JSON-Zeichenfolge in die neue Konfiguration des containerservice, einschließlich Anzahl der neue Agent.

```azurecli
azure acs scale containers-test containerservice-containers-test 10

info:    Executing command acs scale
data:    {
data:        id: '/subscriptions/<guid>/resourceGroups/containers-test/providers/Microsoft.ContainerService/containerServices/containerservice-containers-test',
data:        name: 'containerservice-containers-test',
data:        type: 'Microsoft.ContainerService/ContainerServices',
data:        location: 'westus',
data:        provisioningState: 'Succeeded',
data:        orchestratorProfile: { orchestratorType: 'DCOS' },
data:        masterProfile: {
data:            count: 1,
data:            dnsPrefix: 'myprefixmgmt',
data:            fqdn: 'myprefixmgmt.westus.cloudapp.azure.com'
data:        },
data:        agentPoolProfiles: [
data:            {
data:                name: 'agentpools',
data:                <mark>count: 10</mark>,
data:                vmSize: 'Standard_D2',
data:                dnsPrefix: 'myprefixagents',
data:                fqdn: 'myprefixagents.westus.cloudapp.azure.com'
data:            }
data:        ],
data:        linuxProfile: {
data:            adminUsername: 'azureuser',
data:            ssh: {
data:                publicKeys: [
data:                    { keyData: 'ssh-rsa <ENCODED VALUE>' }
data:                ]
data:            }
data:        },
data:        diagnosticsProfile: {
data:            vmDiagnostics: { enabled: true, storageUri: 'https://<storageid>.blob.core.windows.net/' }
data:        }
data:    }
info:    acs scale command OK
``` 

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen eines Clusters](container-service-deployment.md)