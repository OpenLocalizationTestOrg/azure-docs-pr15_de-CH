<properties
    pageTitle="Migrieren Sie IaaS-Ressourcen von Classic an Azure-Ressourcen-Manager mit Azure CLI | Microsoft Azure"
    description="Dieser Artikel führt Plattform unterstützt Migration der Ressourcen von Classic an Azure-Ressourcen-Manager mithilfe von Azure-CLI"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Migrieren Sie IaaS-Ressourcen von Classic an Azure-Ressourcen-Manager mit Azure-CLI

Diese Schritte zeigen, wie mit Azure Befehlszeilenschnittstelle (CLI) Befehle Infrastruktur als ein Service (IaaS) Ressourcen klassischen Bereitstellungsmodell Bereitstellungsmodell Azure Resource Manager migriert. Der Artikel muss der [Azure-CLI](../xplat-cli-install.md).

>[AZURE.NOTE] Die hier beschriebenen Vorgänge sind Idempotent. Haben Sie ein Problem nicht unterstützt oder Konfigurationsfehler empfohlen wiederholen vorbereiten, Abbruch oder commit Vorgang. Die Plattform wird dann erneut versuchen.

## <a name="step-1-prepare-for-migration"></a>Schritt 1: Vorbereitung der migration

Hier sind einige best Practices, die wir empfehlen migrieren IaaS-Ressourcen von klassischen Resource Manager bewerten:

- Lesen Sie die [Liste der Features oder nicht unterstützte Konfigurationen](virtual-machines-windows-migration-classic-resource-manager.md). Wenn Sie virtuelle Computer, die nicht unterstützte Konfigurationen oder Features verwenden, empfiehlt sich die für die Feature-Konfiguration vorgestellt warten. Alternativ können Sie dieses Feature entfernen oder verschieben aus dieser Konfiguration Migration aktivieren, wenn sie Ihren Bedürfnissen entspricht.
-   Wenn Sie Skripts, die Ihre Infrastruktur und Applikationen heute bereitstellen automatisiert, versuchen Sie, eine ähnliche Testinstallation mithilfe von Skripts für die Migration erstellen. Alternativ können Sie Beispiel-Umgebung einrichten, mit der Azure-Portal.

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Schritt 2: Legen Sie Ihr Abonnement und registrieren Anbieter

Für Migrationsszenarien müssen beide Classic-Umgebung und Ressourcen-Manager. [Azure-CLI installieren](../xplat-cli-install.md) und [Aktivieren Sie Ihr Abonnement](../xplat-cli-connect.md).

Anmelden bei Ihrem Konto.
    
    azure login

Wählen Sie mithilfe des folgenden Befehls Azure-Abonnement.

    azure account set "<azure-subscription-name>"

>[AZURE.NOTE] Die Registrierung ist ein Schritt jedoch einmal versuchen Migration ausgeführt werden soll. Ohne Registrierung sehen Sie die folgende Fehlermeldung angezeigt 

>   *BadRequest: Abonnement ist für die Migration nicht registriert.* 

Registrieren Sie mit dem Ressourcenanbieter Migration mithilfe des folgenden Befehls. Beachten Sie, dass in einigen Fällen dieser Befehl Timeout. Die Registrierung wird jedoch erfolgreich sein.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Warten Sie fünf Minuten die Registrierung abgeschlossen. Mit dem folgenden Befehl können Sie den Status der Genehmigung überprüfen. Stellen Sie sicher, dass RegistrationState `Registered` bevor Sie fortfahren.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Wechseln Sie die CLI auf dem `asm` Modus.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Schritt 3: Sicherstellen Sie, dass Sie genügend Azure Ressourcenmanager Virtual Machine Kerne in Azure-Region der aktuellen Bereitstellung oder VNET

Für diesen Schritt müssen Sie wechseln zu `arm` Modus. Tun Sie dies mit dem folgenden Befehl.

```
azure config mode arm
```

Folgenden CLI-Befehl können Sie die aktuelle Anzahl der Kerne überprüfen Sie in Azure-Ressourcen-Manager. Über Core Kontingenten finden Sie unter [Grenzwerte und der Azure-Ressourcen-Manager](../articles/azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Anschließend überprüfen diesen Schritt, Sie können an `asm` Modus.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Schritt 4: Option 1 - Migration virtueller Computer in einem Clouddienst 

Ruft die Liste der Cloud-Dienste mit dem folgenden Befehl, und wählen Sie Cloud-Dienst, den Sie migrieren möchten. Beachten Sie, dass die VMs in der Cloud-Dienst in einem virtuellen Netzwerk oder wenn sie Web-Worker-Rollen haben, Sie eine Fehlermeldung erhalten.

    azure service list

Führen Sie den folgenden Befehl den Namen für den Clouddienst der ausführlichen Ausgabe. In den meisten Fällen entspricht der Namen Cloud-Dienstnamen.

    azure service show <serviceName> -vv

Bereiten Sie die virtuellen Computer in die Cloud-Dienst für die Migration vor. Es stehen zwei Optionen zur Auswahl.

Migration von VMs zu Plattform erstellte virtuelle Netzwerk, verwenden Sie den folgenden Befehl ein.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Wenn Sie ein vorhandenes virtuelles Netzwerk in der Ressourcen-Manager-Bereitstellungsmodell migrieren möchten, verwenden Sie den folgenden Befehl.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> subnetName <vnetName>

Nachdem die Vorbereitung erfolgreich war, finden Sie über die ausführliche Migrationsstatus VMs und sicherzustellen, dass sie die `Prepared` Zustand.

    azure vm show <vmName> -vv

Überprüfen Sie die Konfiguration der vorbereiteten Ressourcen mithilfe von CLI oder Azure-Portal. Wenn Sie nicht für die Migration noch und den alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

    azure service deployment abort-migration <serviceName> <deploymentName>

Gut vorbereitete Konfiguration Sie vorwärts und Ressourcen mithilfe des folgenden Befehls commit.

    azure service deployment commit-migration <serviceName> <deploymentName>


    
## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Schritt 4: Option 2 - Migration virtueller Computer in einem virtuellen Netzwerk

Wählen Sie das virtuelle Netzwerk, das Sie migrieren möchten. Hinweis Wenn das virtuelle Netzwerk Web-Worker-Rollen oder VMs mit nicht unterstützten Konfigurationen enthält, erhalten Sie eine Validierungsfehlermeldung.

Virtuelle Netzwerke im Abonnement mithilfe des folgenden Befehls zu erhalten.

    azure network vnet list
    
Die Ausgabe sieht in etwa wie folgt:

![Screenshot der Befehlszeile mit dem gesamten virtuellen Netzwerknamen hervorgehoben.](./media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

Im obigen Beispiel ist **VirtualNetworkName** der gesamte Name **"Gruppe classicubuntu16 classicubuntu16"**.

Vorbereiten des virtuellen Netzwerks Ihrer Wahl für die Migration mithilfe des folgenden Befehls.

    azure network vnet prepare-migration <virtualNetworkName>

Überprüfen Sie die Konfiguration für die vorbereiteten virtuellen Computer mithilfe von CLI oder Azure-Portal. Wenn Sie nicht für die Migration noch und den alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

    azure network vnet abort-migration <virtualNetworkName>

Gut vorbereitete Konfiguration Sie vorwärts und Ressourcen mithilfe des folgenden Befehls commit.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Schritt 5: Ein Speicherkonto migrieren

Anschließend virtuelle Computer migrieren, sollten das Speicherkonto migrieren.

Bereiten Sie das Speicherkonto mithilfe des folgenden Befehls für die Migration vor

    azure storage account prepare-migration <storageAccountName>

Überprüfen Sie die Konfiguration für das vorbereitete Speicherkonto CLI oder Azure-Portal. Wenn Sie nicht für die Migration noch und den alten Zustand zurückkehren möchten, verwenden Sie den folgenden Befehl.

    azure storage account abort-migration <storageAccountName>

Gut vorbereitete Konfiguration Sie vorwärts und Ressourcen mithilfe des folgenden Befehls commit.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Nächste Schritte

- [Plattform unterstützt Migration IaaS-Ressourcen von Classic an Ressourcen-Manager](virtual-machines-windows-migration-classic-resource-manager.md)
- [Technische detaillierte technische Informationen zur Plattform unterstützt Migration von Classic an Ressourcen-Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
