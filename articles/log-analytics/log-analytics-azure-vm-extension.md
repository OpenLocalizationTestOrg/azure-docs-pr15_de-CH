<properties
    pageTitle="Protokollanalyse Azure virtuelle Computer herstellen | Microsoft Azure"
    description="Die empfohlene Methode, Protokolle und Metriken wird für Windows und Linux virtuellen Computern in Azure Log Analytics Azure VM-Erweiterung installieren. Azure-Portal oder PowerShell können die Protokollanalyse virtuellen Erweiterung auf Azure VMs."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="richrund"/>

# <a name="connect-azure-virtual-machines-to-log-analytics"></a>Protokollanalyse Azure virtuelle Computer herstellen

Für Windows- und Linux-Computern ist die empfohlene Methode für das Sammeln von Protokollen und Metriken den Protokollanalyse-Agent installieren.

Den Protokollanalyse-Agent auf Azure virtuellen Computern installieren am einfachsten über die Erweiterung Log Analytics VM.  Mit der Erweiterung vereinfacht die Installation und konfiguriert automatisch den Agent zum Senden von Daten an den Protokollanalyse-Arbeitsbereich, den Sie angeben. Der Agent wird auch automatisch aktualisiert sicherstellen, dass Sie die aktuellen Features und Fixes.

Windows virtuelle Maschinen können *Microsoft Monitoring Agent* VM-Erweiterung.
Virtuelle Linux-Computer können *OMS Agent für Linux* VM-Erweiterung.

Weitere Informationen zu [Azure Virtual Machine Extensions](../virtual-machines/virtual-machines-windows-extensions-features.md) und [Linux Agent] (... / virtual-machines/virtual-machines-linux-agent-user-guide.md).

Wenn Sie Agenten-basierte Auflistung für Daten verwenden, müssen Sie [Datenquellen Protokollanalyse](log-analytics-data-sources.md) Geben Sie Protokolle und Metriken sammeln möchten konfigurieren.

>[AZURE.IMPORTANT] Wenn Protokollanalyse zu Index-Protokolldaten mithilfe von [Azure Diagnostics](log-analytics-azure-storage.md)konfigurieren und konfigurieren Sie den Agent dieselben Protokolle sammeln, werden die Protokolle zweimal erfasst. Sie Zahlen für beide Datenquellen. Haben Sie den Agent installiert, dann Sie Daten sammeln sollten, mit allein Agent - Protokollanalyse sammeln Daten von Azure Diagnostics nicht konfigurieren.

Es gibt drei einfache Ansätzen Protokollanalyse VM-Erweiterung zu aktivieren:

+ Mithilfe des Azure-Portals
+ Mithilfe von Azure PowerShell
+ Mithilfe einer Vorlage Azure-Ressourcen-Manager

## <a name="enable-the-vm-extension-in-the-azure-portal"></a>VM-Erweiterung im Azure-Portal aktivieren

Installieren den Protokollanalyse-Agent und die Azure virtuellen Maschine, die Ausführung mit dem [Azure-Portal](https://portal.azure.com).

### <a name="to-install-the-log-analytics-agent-and-connect-the-virtual-machine-to-a-log-analytics-workspace"></a>Den Protokollanalyse-Agent installieren und den virtuellen Computer Protokollanalyse-Arbeitsbereich

1.  [Azure-Portal](http://portal.azure.com)anmelden.
2.  Wählen Sie auf der linken Seite des Portals **Durchsuchen** wechseln Sie **Protokoll Analytics (OMS)** und auswählen.
3.  Wählen Sie in der Protokollanalyse Arbeitsbereiche eine Azure VM verwenden möchten.  
    ![OMS-Arbeitsbereiche](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4.  Wählen Sie unter **Analytics Verwaltung** **virtueller Maschinen**.  
    ![Virtuelle Computer](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5.  Wählen Sie in der Liste der **virtuellen Computer**den virtuellen Computer, auf dem der Agent installiert werden soll. **OMS-Verbindungsstatus** für die VM gibt an, dass es **nicht verbunden**ist.  
    ![VM nicht verbunden](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6.  Wählen Sie die Details für den virtuellen Computer **Verbinden**. Der Agent wird automatisch installiert und konfiguriert für den Protokollanalyse-Arbeitsbereich. Dieser Vorgang dauert einige Minuten, bei der OMS-Verbindungsstatus *verbinden...*  
    ![VM verbinden](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7.  Nach der Installation und den Agent verbinden wird **OMS** -Verbindungsstatus **dieses Arbeitsbereichs**anzeigen aktualisiert werden.  
    ![Verbunden](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)


## <a name="enable-the-vm-extension-using-powershell"></a>Mithilfe von PowerShell VM-Erweiterung aktivieren

Es gibt verschiedene Befehle für Azure klassische virtuelle Computer und Ressourcen-Manager virtuelle Computer. Es folgen Beispiele für Classic und virtuelle Maschinen Ressourcen-Manager.

Verwenden Sie für klassische virtuelle Maschinen das folgende PowerShell-Beispiel:

```
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment the following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

Verwenden Sie für Ressourcen-Manager virtuelle Computer das folgende PowerShell-Beispiel:

```
Login-AzureRMAccount
Select-AzureSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable to find OMS Workspace $workspaceName. Do you need to run Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment the following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```
Beim Konfigurieren der virtuellen Computer mithilfe von PowerShell müssen Sie die **Arbeitsplatz-ID** und **Primärschlüssel**. Id und Schlüssel **auf der OMS-Portal oder mithilfe von PowerShell wie im vorherigen Beispiel gezeigt** gefunden werden.

![Arbeitsplatz-ID und Primärschlüssel](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

## <a name="deploy-the-vm-extension-using-a-template"></a>Bereitstellen der VM-Erweiterung mithilfe einer Vorlage

Mithilfe von Azure-Ressourcen-Manager können Sie eine einfache Vorlage (im JSON-Format) erstellen, die die Bereitstellung und Konfiguration der Anwendung definiert. Diese Vorlage wird als Vorlage Ressourcenmanager bezeichnet und ermöglicht das deklarative Definieren der Bereitstellung. Sie können mithilfe einer Vorlage wiederholt die Anwendung im gesamten Lebenszyklus der Anwendung bereitstellen und vertrauen, dass Ihre Ressourcen in einem konsistenten Zustand bereitgestellt werden.

Mit der Protokollanalyse-Agent als Teil der Ressourcen-Manager-Vorlage können Sie sicherstellen, dass jede virtuelle Maschine vordefinierten Arbeitsbereich Protokollanalyse mitzuteilen.

Weitere Informationen zu Ressourcen-Manager-Vorlagen finden Sie unter [Azure Ressourcenmanager erstellen Vorlagen](../resource-group-authoring-templates.md).

Es folgt ein Beispiel einer Ressourcen-Manager-Vorlage dient der Bereitstellung einer virtuellen Maschine, die Windows installiert Microsoft Monitoring Agent-Erweiterung ausgeführt wird. Diese Vorlage ist eine Vorlage typisch virtuellen Computer mit folgenden Besonderheiten:

+ WorkspaceId und ein Parameter
+ Erweiterung Ressourcenabschnitt Microsoft.EnterpriseCloud.Monitoring
+ Ausgaben von WorkspaceId und WorkspaceSharedKey suchen


```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for the Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for the Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for the Public IP. Must be lowercase. It should match with the following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMD workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "The Windows version for the VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

Sie können eine Vorlage bereitstellen, mit dem folgenden PowerShell-Befehl:

```
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-windows-virtual-machines"></a>Problembehandlung bei virtuellen Windows-Maschinen

Wenn *Microsoft Monitoring Agent* VM-Agent-Erweiterung nicht installieren reporting oder führen Sie die folgenden Schritte aus, um das Problem zu beheben.

1. Überprüfen Sie, ob der Azure-VM-Agent installiert ist und arbeiten ordnungsgemäß mithilfe der Schritte in [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
  + Sie können auch die VM Protokolldatei überprüfen.`C:\WindowsAzure\logs\WaAppAgent.log`
  + Wenn das Protokoll nicht vorhanden ist, wird der VM-Agent nicht installiert.
    - [Installieren der Azure-VM-Agent auf klassische VMs](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md)
2. Bestätigen Sie, dass Microsoft Monitoring Agent Erweiterung Heartbeat-Vorgang läuft folgendermaßen:
  + Melden Sie sich am virtuellen Computer
  + Taskplaner öffnen und suchen Sie die `update_azureoperationalinsight_agent_heartbeat` Aufgabe
  + Bestätigen Sie der Task ist aktiviert und wird jede Minute ausgeführt
  + Die Heartbeat-Protokolldatei Einchecken`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Prüfung Microsoft Monitoring Agent VM-Erweiterung in`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. Sicherstellen Sie, dass der virtuelle Computer PowerShell-Skripts ausführen können
4. Sicherstellen Sie, dass Berechtigungen auf C:\Windows\temp verändert
5. Anzeigen des Status von Microsoft Monitoring Agent durch Eingeben des folgenden in einer erhöhten PowerShell-Fenster des virtuellen Computers`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Überprüfen Sie die Installationsprotokolldateien Microsoft Monitoring Agent in`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Weitere Informationen finden Sie in [troubleshooting Windows Extensions](../virtual-machines/virtual-machines-windows-extensions-troubleshoot.md).

## <a name="troubleshooting-linux-virtual-machines"></a>Problembehandlung bei virtuelle Linux-Computer

Wenn die *OMS-Agenten für Linux* VM-Agent-Erweiterung nicht installieren oder reporting ist führen Sie die folgenden Schritte zur Problembehandlung.

1. Wenn der Erweiterungsstatus *unbekannt* Kontrollkästchen Azure VM-Agent installiert ist und arbeiten ordnungsgemäß durch die VM Protokolldatei überprüfen`/var/log/waagent.log`
  + Wenn das Protokoll nicht vorhanden ist, wird der VM-Agent nicht installiert.
  - [Installieren Sie den Agent Azure VM unter Linux VMs](../virtual-machines/virtual-machines-linux-agent-user-guide.md)
2. Für andere fehlerhaften Status überprüfen OMS-Agent für Linux VM-Erweiterung Dateien anmelden `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` und`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Den Erweiterungsstatus fehlerfrei jedoch Daten werden nicht hochgeladen überprüfen Sie OMS-Agent für Linux Protokolldateien`/var/opt/microsoft/omsagent/log/omsagent.log`

Weitere Informationen finden Sie in [troubleshooting Linux Extensions](../virtual-machines/virtual-machines-linux-extensions-troubleshoot.md).


## <a name="next-steps"></a>Nächste Schritte

+ Konfigurieren Sie [Datenquellen im Log-Analyse](log-analytics-data-sources.md) der Protokolle und Metriken sammeln.
+ Zum Sammeln von Daten von virtuellen Maschinen [Lösungen Lösungskatalog Protokollanalyse hinzufügen](log-analytics-add-solutions.md).
+ [Sammeln von Daten mithilfe von Azure-Diagnose](log-analytics-azure-storage.md) für andere Ressourcen, die in Azure ausgeführt werden.

Bei Computern, die nicht in Azure ermöglicht die Installation des Protokollanalyse-Agents mithilfe der Methoden, die in den folgenden Artikeln beschrieben werden:

+ [Protokollanalyse Windows-Computern herstellen](log-analytics-windows-agents.md)
+ [Protokollanalyse Linux-Computer herstellen](log-analytics-linux-agents.md)
