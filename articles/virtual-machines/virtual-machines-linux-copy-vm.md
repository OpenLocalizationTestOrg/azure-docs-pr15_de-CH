<properties
    pageTitle="Erstellen Sie eine Kopie Ihrer Azure Linux VM | Microsoft Azure"
    description="So erstellen Sie eine Kopie des virtuellen Computers Azure Linux in der Ressourcen-Manager-Bereitstellungsmodell"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>

# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure"></a>Erstellen Sie eine Kopie einer virtuellen Linux-Maschine auf Azure


Dieser Artikel veranschaulicht die Kopie unter Linux mit dem Ressourcen-Manager-Bereitstellungsmodell Ihre Azure Virtual Machine (VM) erstellen. Zuerst kopieren über das Betriebssystem und Datenträger in einen neuen Container und Netzwerkressourcen eingerichtet und Erstellen neuen virtueller Computer.

Sie können auch [Hochladen und VM von benutzerdefinierten Image Erstellen](virtual-machines-linux-upload-vhd.md).


## <a name="before-you-begin"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Sie zunächst die Schritte folgende Vorkenntnisse mitbringen:

- Sie haben [Azure CLI] (... / befehlsschnittstelleninstanz-Cli-install.md) heruntergeladen und auf Ihrem Computer installiert. 

- Sie benötigen einige Informationen zu Ihrer vorhandenen Azure Linux VM:

| Quellinformationen VM | Es erhalten |
|------------|-----------------|
| Name des virtuellen Computers | `azure vm list` |
| Name der Ressourcengruppe | `azure vm list` |
| Speicherort | `azure vm list` |
| Speicher-Kontoname | `azure storage account list -g <resourceGroup>` |
| Containername | `azure storage container list -a <sourcestorageaccountname>` |
| Quellname VM VHD-Datei | `azure storage blob list --container <containerName>` |



- Sie müssen einige Entscheidungen über neue VM:   <br> -Container name   <br> VM - name   <br> VM - Größe   <br> vNet - Namen   <br> -SubNet name   <br> IP - Name   <br> -Namen
    

## <a name="login-and-set-your-subscription"></a>Anmeldung und Ihr Abonnement

1. Melden Sie die CLI.
        
        azure login

2. Sicherstellen Sie, dass Ressourcen-Manager aktiviert ist.
    
        azure config mode arm

3. Legen Sie das richtige Abonnement. "Azure-Konto Liste aller Abonnements.

        azure account set <SubscriptionId>



## <a name="stop-the-vm"></a>Beenden der VM 

Beenden und Freigeben der Quelle VM. "Azure Vm Liste eine Liste aller VMs in Ihr Abonnement und ihre Namen abrufen.
    
        azure vm stop <ResourceGroup> <VmName>
        azure vm deallocate <ResourceGroup> <VmName>




## <a name="copy-the-vhd"></a>Kopieren Sie die VHD-Datei


Kopieren die virtuellen Festplatte aus Quellspeicher auf das Ziel der `azure storage blob copy start`. In diesem Beispiel werden wir die VHD-Datei die gleichen Speicherkonto, aber einen anderen Container kopieren.

Um die virtuelle Festplatte in einen anderen Container in demselben Speicherkonto zu kopieren, geben Sie Folgendes ein:

        azure storage blob copy start https://<sourceStorageAccountName>.blob.core.windows.net:8080/<sourceContainerName>/<SourceVHDFileName.vhd> <newcontainerName>
        

## <a name="set-up-the-virtual-network-for-your-new-vm"></a>Virtuelles Netzwerk für Ihren neuen virtuellen Computer einrichten

Ein virtuelles Netzwerk und NIC für Ihren neuen virtuellen Computer einrichten. 

    azure network vnet create <ResourceGroupName> <VnetName> -l <Location>

    azure network vnet subnet create -a <address.prefix.in.CIDR/format> <ResourceGroupName> <VnetName> <SubnetName>

    azure network public-ip create <ResourceGroupName> <IpName> -l <yourLocation>

    azure network nic create <ResourceGroupName> <NicName> -k <SubnetName> -m <VnetName> -p <IpName> -l <Location>


## <a name="create-the-new-vm"></a>Erstellen von neuen VM 

Angabe des URI auf die Festplatte kopierte eingeben, wird jetzt eine VM aus der hochgeladenen Laufwerk [Resource Manager Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) oder durch die CLI erstellen:

```bash
azure vm create -n <newVMName> -l "<location>" -g <resourceGroup> -f <newNicName> -z "<vmSize>" -d https://<storageAccountName>.blob.core.windows.net/<containerName/<fileName.vhd> -y Linux
```



## <a name="next-steps"></a>Nächste Schritte

Informationen zum Azure-Befehlszeilenschnittstelle verwenden, um den neuen virtuellen Computer verwalten anzeigen Sie [Azure-CLI-Befehle für Azure-Manager](azure-cli-arm-commands.md)
