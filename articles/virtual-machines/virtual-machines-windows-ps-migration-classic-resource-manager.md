<properties
    pageTitle="Migrieren zu Resource Manager mit PowerShell | Microsoft Azure"
    description="Dieser Artikel führt durch Migration Plattform unterstützt IaaS-Ressourcen von Classic an Azure-Ressourcen-Manager mithilfe von Azure PowerShell-Befehlen"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Migrieren von IaaS-Ressourcen von Classic an Azure-Ressourcen-Manager mithilfe von Azure PowerShell

Diese Schritte zeigen, wie mit Azure PowerShell-Befehlen Infrastruktur als ein Service (IaaS) Ressourcen klassischen Bereitstellungsmodell Bereitstellungsmodell Azure Resource Manager migriert. 

Wenn Sie möchten, können Sie auch Ressourcen mithilfe der [Azure-Befehlszeilenschnittstelle (CLI Azure)](virtual-machines-linux-cli-migration-classic-resource-manager.md)migrieren.

- Hintergrundinformationen zu unterstützten Migrationsszenarien anzeigen Sie [Plattform unterstützt Migration IaaS-Ressourcen von classic an Azure-Ressourcen-Manager](virtual-machines-windows-migration-classic-resource-manager.md) 
- Ausführliche Leitfäden und eine Migration Exemplarische Vorgehensweise finden Sie in [technischen detaillierte technische Informationen zur Migration von classic an Azure-Ressourcen-Manager-Plattform unterstützt](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## <a name="step-1-plan-for-migration"></a>Schritt 1: Plan für die migration

Hier sind einige best Practices, die wir empfehlen migrieren IaaS-Ressourcen von klassischen Resource Manager bewerten:

- Lesen Sie [unterstützte und nicht unterstützte Features und Konfigurationen](virtual-machines-windows-migration-classic-resource-manager.md). Haben Sie virtuelle Computer, die nicht unterstützte Konfigurationen oder Features verwenden, empfiehlt sich die Konfiguration-Funktion Unterstützung angekündigt warten. Wenn es Ihnen passt entfernen mit Alternativ verschieben, Konfiguration, Migration aktivieren.
-   Wenn Sie Skripts, die Ihre Infrastruktur und Applikationen heute bereitstellen automatisiert, versuchen Sie, eine ähnliche Testinstallation mithilfe von Skripts für die Migration erstellen. Alternativ können Sie Beispiel-Umgebung einrichten, mit der Azure-Portal.

>[AZURE.IMPORTANT]Virtuelles Netzwerk-Gateways (-Standorten, Azure ExpressRoute Application Gateway, Punkt-zu-Standort) sind derzeit nicht für die Migration von Classic an Ressourcen-Manager unterstützt. Entfernen Sie zum Migrieren von klassischen virtuellen Netzwerks mit Gateway Gateway vor der Ausführung eines Commit-Vorgangs zum Netzwerk verschieben (Sie können vorbereiten Schritt ausführen, ohne das Gateway löschen). Schließen Sie nach Abschluss die Migration das Gateway in Azure-Ressourcen-Manager.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Schritt 2: Installieren der neuesten Version von Azure PowerShell

Es gibt zwei Optionen Azure PowerShell installiert: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) oder [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps). WebPI wird monatlich aktualisiert. PowerShell-Galerie wird fortlaufend aktualisiert. Dieser Artikel basiert auf Azure PowerShell Version 2.1.0.

Installationshinweise Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md).

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>Schritt 3: Legen Sie Ihr Abonnement und für migration

Starten Sie PowerShell Prompt. Für die Migration müssen beide Classic-Umgebung und Ressourcen-Manager.

Melden Sie sich bei Ihrem Konto für den Ressourcen-Manager-Modell.

```powershell
    Login-AzureRmAccount
```

Rufen Sie verfügbare Abonnements mit dem folgenden Befehl ab:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Legen Sie Ihre Azure-Abonnement für die aktuelle Sitzung. In diesem Beispiel wird der Standardname Abonnement zu **Azure Abonnement**. Ersetzen Sie der Abonnementname wird durch Ihren eigenen. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Sie müssen einmal versuchen, Migration Registrierung ist eine einmalige Schritt. Ohne Registrierung wird die folgende Fehlermeldung angezeigt: 

>   *BadRequest: Abonnement ist für die Migration nicht registriert.* 

Registrieren Sie mit dem Ressourcenanbieter Migration mithilfe des folgenden Befehls:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Warten Sie fünf Minuten die Registrierung abgeschlossen. Mit dem folgenden Befehl können Sie den Status der Genehmigung überprüfen:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Stellen Sie sicher, dass RegistrationState `Registered` bevor Sie fortfahren. 

Jetzt anmelden bei Ihrem Konto für das klassische Modell.

```powershell
    Add-AzureAccount
```

Rufen Sie verfügbare Abonnements mit dem folgenden Befehl ab:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Legen Sie Ihre Azure-Abonnement für die aktuelle Sitzung. In diesem Beispiel wird das Standardabonnement auf **Azure Abonnement**. Ersetzen Sie der Abonnementname wird durch Ihren eigenen. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Schritt 4: Sicherstellen Sie, dass Sie genügend Azure Ressourcenmanager Virtual Machine Kerne in Azure-Region der aktuellen Bereitstellung oder VNET

Den folgenden PowerShell-Befehl können Sie die aktuelle Anzahl der Kerne überprüfen Sie in Azure-Ressourcen-Manager. Über Core Kontingenten finden Sie unter [Grenzwerte und der Azure-Ressourcen-Manager](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

Dieses Beispiel überprüft die Verfügbarkeit in der Region **West USA** . Ersetzen Sie der Bereichsname wird durch Ihren eigenen. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Schritt 5: Führen Sie Befehle IaaS-Ressourcen migrieren

>[AZURE.NOTE] Die hier beschriebenen Vorgänge sind Idempotent. Haben Sie ein Problem nicht unterstützt oder Konfigurationsfehler empfohlen wiederholen vorbereiten, Abbruch oder commit Vorgang. Die Plattform versucht es erneut.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Migration virtueller Computer in einem Clouddienst (nicht in einem virtuellen Netzwerk)

Ruft die Liste der Cloud-Dienste mit dem folgenden Befehl, und wählen Sie Cloud-Dienst, den Sie migrieren möchten. Wenn die VMs in der Cloud-Dienst in einem virtuellen Netzwerk oder sie Web-oder Arbeitnehmer, der Befehl eine Fehlermeldung gibt.

```powershell
    Get-AzureService | ft Servicename
```

Ruft den bereitstellungsnamen für den Clouddienst. In diesem Beispiel ist der Name **My Service**. Eigene Dienstname ersetzen Sie den Dienstnamen Beispiel. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Bereiten Sie die virtuellen Computer in die Cloud-Dienst für die Migration vor. Es stehen zwei Optionen zur Auswahl.

* **Option 1. Migration von VMs zu einem virtuellen Netzwerk Plattform erstellt**

    Zuerst überprüfen Sie migrieren Cloud-Dienst mit den folgenden Befehlen:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Der Befehl zeigt alle Warnungen und Fehler, die Migration zu blockieren. Wenn die Validierung erfolgreich ist, können Sie mit dem Schritt **Vorbereiten** auf verschieben:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **Option 2. Migrieren Sie in ein vorhandenes virtuelles Netzwerk in der Ressourcen-Manager-Bereitstellungsmodell**

    In diesem Beispiel wird der Gruppe Ressourcenname **MyResourceGroup**Namen des virtuellen Netzwerks, **MyVirtualNetwork** und Teilnetznamen zu **MySubNet**. Ersetzen Sie die Namen im Beispiel mit den Namen Ihrer eigenen Ressourcen.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Erstens migrieren Cloud-Dienst mit dem folgenden Befehl überprüfen:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Der Befehl zeigt alle Warnungen und Fehler, die Migration zu blockieren. Wenn die Validierung erfolgreich ist, können Sie mit den folgenden vorbereiten Schritt fortfahren:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Nach erfolgreicher die Vorbereitung mit der obigen Optionen Fragen Sie Migrationsstatus VMs ab. Sind sie in der `Prepared` Zustand.

In diesem Beispiel wird der Name auf **"MyVM"**. Eigene VM-Name ersetzen Sie den Namen.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Überprüfen Sie die Konfiguration der vorbereiteten Ressourcen mithilfe von PowerShell oder Azure-Portal. Wenn Sie nicht für die Migration noch und den alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Gut vorbereitete Konfiguration Sie vorwärts und commit Ressourcen mithilfe des folgenden Befehls:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Migration virtueller Computer in einem virtuellen Netzwerk

Um virtuelle Computer in einem virtuellen Netzwerk migrieren, migrieren Sie das Netzwerk. Die virtuellen Computer migrieren automatisch mit dem Netzwerk. Wählen Sie das virtuelle Netzwerk, das Sie migrieren möchten. 

In diesem Beispiel wird den virtuellen Netzwerknamen zu **MyVnet**. Ersetzen Sie virtuelles Netzwerk Beispielnamen durch Ihren eigenen. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Wenn das virtuelle Netzwerk Web Worker-Rollen oder VMs mit nicht unterstützten Konfigurationen enthält, erhalten Sie eine Validierungsfehlermeldung.

Zunächst überprüfen Sie das virtuelle Netzwerk migrieren können mithilfe des folgenden Befehls:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Der Befehl zeigt alle Warnungen und Fehler, die Migration zu blockieren. Wenn die Validierung erfolgreich ist, können Sie mit den folgenden vorbereiten Schritt fortfahren:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Überprüfen Sie die Konfiguration für die vorbereiteten virtuellen Computer mithilfe von Azure PowerShell oder Azure-Portal. Wenn Sie nicht für die Migration noch und den alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Gut vorbereitete Konfiguration Sie vorwärts und commit Ressourcen mithilfe des folgenden Befehls:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>Ein Speicherkonto migrieren

Anschließend Migration virtueller Maschinen empfohlen Speicherkonten migrieren.

Vorbereitung jedes Storage-Konto Migration mithilfe des folgenden Befehls. In diesem Beispiel ist der speicherkontoname **MyStorageAccount**. Ersetzen Sie den Namen durch den Namen Ihres eigenen Speicherkontos. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Überprüfen Sie die Konfiguration für das vorbereitete Speicherkonto Azure PowerShell oder Azure-Portal. Wenn Sie nicht für die Migration noch und den alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Gut vorbereitete Konfiguration Sie vorwärts und commit Ressourcen mithilfe des folgenden Befehls:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur Migration finden Sie unter [Migration IaaS-Ressourcen von classic an Azure-Ressourcen-Manager-Plattform unterstützt](virtual-machines-windows-migration-classic-resource-manager.md).
- Um weitere Ressourcen zum Ressourcen-Manager mit Azure PowerShell migrieren, verwenden Sie ähnliche Schritte [AzureNetworkSecurityGroup verschieben](https://msdn.microsoft.com/library/mt786729.aspx) [Verschieben-AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)und [AzureRouteTable verschieben](https://msdn.microsoft.com/library/mt786718.aspx).
- Open-Source-Skripts, mit Azure Ressourcen klassische Resource Manager migrieren, finden Sie unter [Gemeinschaftsinstrumente in Azure Ressourcenmanager Migration für die migration](virtual-machines-windows-migration-scripts.md)
