<properties
    pageTitle="Erfassen einer Linux VM als Vorlage verwenden | Microsoft Azure"
    description="Informationen Sie zum Erfassen und eine Linux-basierte Azure Virtual Machine (VM mit Bereitstellungsmodell Azure-Ressourcen-Manager erstellt) ein Abbild verallgemeinern."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="iainfou"/>


# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Aufnehmen Sie eine auf Windows Azure ausgeführte virtuellen Linux-Maschine

Führen Sie die Schritte in diesem Artikel verallgemeinern und Ihre Azure virtuellen Linux-Maschine (VM) in der Ressourcen-Manager-Bereitstellungsmodell. Wenn generalize VM Sie persönliche Informationen entfernen und die VM als ein Bild verwendet werden soll. Sie zeichnen dann ein Bild verallgemeinerte virtuelle Festplatte (VHD) für das Betriebssystem, VHDs für angeschlossenen Datenträger und [Ressourcen-Manager-Vorlage](../azure-resource-manager/resource-group-overview.md) für die Bereitstellung neuer virtueller Computer. 

Erstellen virtueller Computer mithilfe des Bildes Netzwerkressourcen für jeden neuen virtuellen Computer richten Sie ein und verwenden Sie der Vorlagendatei (ein JSON- oder JSON) zur Bereitstellung von VHD-Aufnahmen. Auf diese Weise können Sie einen virtuellen Computer mit der aktuellen Softwarekonfiguration ähnlich replizieren, Bilder in Azure Marketplace verwenden.

>[AZURE.TIP]Erstellen Sie eine Kopie Ihrer vorhandenen Linux VM mit speziellen Zustand für die Sicherung oder Debuggen, finden Sie unter [Erstellen einer Kopie einer virtuellen Linux-Maschine in Azure ausgeführt](virtual-machines-linux-copy-vm.md). Und wenn Sie eine VHD Linux eine lokale VM hochladen möchten, [Hochladen und Linux VM von benutzerdefinierten Image](virtual-machines-linux-upload-vhd.md).  

## <a name="before-you-begin"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Sie folgende Vorkenntnisse mitbringen:

* **Azure VM in der Ressourcen-Manager-Bereitstellungsmodell erstellt** - Wenn Sie Linux VM erstellt haben, können Sie das [Portal](virtual-machines-linux-quick-create-portal.md), [Azure-CLI](virtual-machines-linux-quick-create-cli.md)oder [Ressourcenmanager Vorlagen](virtual-machines-linux-cli-deploy-templates.md)verwenden. 

    Konfigurieren der VM. Beispielsweise [Fügen Sie Datenträger hinzu](virtual-machines-linux-add-disk.md), Updates anwenden und Applikationen installieren. 
* **Azure-CLI** - [Azure CLI](../xplat-cli-install.md) auf einem lokalen Computer installieren.

## <a name="step-1-remove-the-azure-linux-agent"></a>Schritt 1: Entfernen des Azure Linux-Agents

Führen Sie zuerst **Waagent** Befehl mit dem Parameter **Identitätsintegrationsprodukts** auf Linux VM. Dieser Befehl löscht Dateien und Daten, die VM verallgemeinern machen. Einzelheiten finden Sie im [Benutzerhandbuch Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md).

1. Verbinden Sie mit Ihrem Linux VM mit einem SSH-Client.

2. Geben Sie im Fenster SSH den folgenden Befehl ein:

    ```
    sudo waagent -deprovision+user
    ```
    >[AZURE.NOTE] Führen Sie diesen Befehl nur auf einem virtuellen Computer, die Sie als Bild aufzeichnen möchten. Dies garantiert nicht, dass das Bild aller vertraulichen Daten gelöscht oder für die Verteilung ist.

3. Geben Sie **y** fort. Sie können die **-erzwingen** Parameter dieser Bestätigungsschritt zu.

4. Geben Sie nach Abschluss des Befehls **Beenden**. Dadurch wird den SSH-Client geschlossen.

    
## <a name="step-2-capture-the-vm"></a>Schritt 2: Erfassen der VM

Verwenden der Azure-CLI verallgemeinern und die VM. Ersetzen Sie in den folgenden Beispielen Parameternamen wird durch Ihre eigenen Werte. Beispiel-Parameternamen enthalten **MyResourceGroup**, **MyVnet**und **MyVM**.

5. Öffnen Sie von Ihrem lokalen Computer Azure CLI und [bei Azure-Abonnement](../xplat-cli-connect.md). 

6. Sicherstellen Sie, dass Ressourcen-Manager aktiviert ist.

    ```
    azure config mode arm
    ```

7. Fahren Sie die VM bereits hat mit dem folgenden Befehl:

    ```
    azure vm deallocate -g MyResourceGroup -n myVM
    ```

8. Verallgemeinern Sie die VM mit dem folgenden Befehl:

    ```
    azure vm generalize -g MyResourceGroup -n myVM
    ```

9. Jetzt führen Sie den Befehl **Azure Vm erfassen** , der die VM erfasst. Im folgenden Beispiel die VHDs Bild mit Namen **MyVHDNamePrefix**erfasst und die Option **-t** gibt den Pfad zu der Vorlage **MyTemplate.json**. 

    ```
    azure vm capture MyResourceGroup MyResourceGroup MyVHDNamePrefix -t MyTemplate.json
    ```

    >[AZURE.IMPORTANT]Bild VHD-Dateien erhalten standardmäßig in demselben Speicherkonto erstellt, die ursprüngliche VM. Verwenden Sie *dieselbe Speicherkonto* die VHDs für neue VMs zu speichern, die Sie aus dem Bild erstellen. 

6. Um eine Aufnahme finden, öffnen Sie die JSON-Vorlage in einem Texteditor. Finden Sie im **StorageProfile**das **Bild** im **Systemcontainer** der **Uri** . Beispielsweise ähnelt der URI die Betriebssystem-image`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-the-captured-image"></a>Schritt 3: Erstellen einer VM aus dem erfassten Bild
Mit einer Vorlage Linux VM erstellt verwenden Sie das Bild jetzt. Diese Schritte zeigen, wie Sie Azure-CLI und die JSON-Vorlagendatei erfasst die VM in ein neues virtuelles Netzwerk erstellen.

### <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen

Um die Vorlage zu verwenden, müssen Sie ein virtuelles Netzwerk und NIC für Ihren neuen virtuellen Computer einrichten. Wir empfehlen eine Ressourcengruppe für diesen VM-Image Speicherort am Speicherort erstellen. Ausführen von Befehlen wie die folgenden, durch Namen für Ressourcen und einem entsprechenden Azure Speicherort (in diesen Befehlen "Centralus"):

    azure group create MyResourceGroup1 -l "centralus"

    azure network vnet create MyResourceGroup1 myVnet -l "centralus"

    azure network vnet subnet create MyResourceGroup1 myVnet mySubnet

    azure network public-ip create MyResourceGroup1 myIP -l "centralus"

    azure network nic create MyResourceGroup1 myNIC -k mySubnet -m myVnet -p myIP -l "centralus"

### <a name="get-the-id-of-the-nic"></a>Rufen Sie die Id der Netzwerkkarte

Bereitstellen eine VM aus dem Bild mit JSON während der Aufnahme gespeichert, benötigen Sie die Id der Netzwerkkarte Ermitteln sie den folgenden Befehl ausführen:

    azure network nic show MyResourceGroup1 myNIC

Die **Id** der Ausgabe ähnelt`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`



### <a name="create-a-vm"></a>Erstellen Sie einen virtuellen Computer
Nun führen Sie den folgenden Befehl von VM-Aufnahme der VM erstellt. Verwenden Sie den Parameter **-f** an gespeicherten Pfad der Vorlagendatei JSON.

    azure group deployment create MyResourceGroup1 MyDeployment -f MyTemplate.json

Ausgabe des Befehls werden Sie aufgefordert, einen neuen VM-Namen, den Benutzernamen Administrator und Kennwort, und die Id der Netzwerkkarte zuvor erstellt.

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    vmName: myNewVM
    adminUserName: myAdminuser
    adminPassword: ********
    networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic

Das folgende Beispiel zeigt eine erfolgreiche Bereitstellung finden Sie:

    + Initialisieren von Vorlage Konfigurationen und Parameter
    + Erstellen einer Bereitstellungsinformationen: Vorlage Bereitstellung Xxxxxxx erstellt
    + Bereitstellung auf Abschluss warten: DeploymentName: MyDeployment Daten: ResourceGroupName: MyResourceGroup1 Daten: ProvisioningState: Daten erfolgreich: Zeitstempel: Xxxxxxx Daten: Modus: inkrementelle Daten: Namenswert


    data:    ------------------  ------------  -------------------------------------

    Daten: VmName Zeichenfolge MyNewVM


    Daten: VmSize Standard_D1 Zeichenfolge


    Daten: AdminUserName Zeichenfolge MyAdminuser


    Daten: AdminPassword SecureString undefiniert


    Daten: NetworkInterfaceId Zeichenfolge /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic Info: Bereitstellung erstellen Befehl OK

### <a name="verify-the-deployment"></a>Überprüfen der Bereitstellung

Jetzt SSH Überprüfen der Bereitstellung und verwenden die neue VM erstellten virtuellen Computer. Verbindung über SSH finden Sie die IP-Adresse des virtuellen Computers erstellt durch Ausführen des folgenden Befehls:

    azure network public-ip show MyResourceGroup1 myIP

Die öffentliche IP-Adresse wird in der Befehlsausgabe aufgeführt. Standardmäßig verbinden Sie mit Linux VM über SSH auf Port 22.

## <a name="create-additional-vms"></a>Weitere virtuelle Computer erstellen
Verwenden Sie aufgenommene Bild und Vorlage zusätzliche VMs mit den Schritten im vorherigen Abschnitt bereitstellen. VMs aus dem Abbild erstellen auch Schnellstart Vorlage oder den Befehl **Azure Vm erstellen** .

### <a name="use-the-captured-template"></a>Verwenden Sie die erfasste Vorlage

Um die Aufnahme und die Vorlage verwenden, folgendermaßen Sie (siehe vorherigen Abschnitt)

* Sicherstellen Sie, dass die VM-Image in demselben Speicherkonto, auf dem die VM VHD gehostet.
* Kopieren Sie JSON-Vorlagendatei, und geben Sie einen eindeutigen Namen für Betriebssystemdatenträger neuen VMs VHD (oder VHDs). In der **StorageProfile**, unter **Vhd** **Uri**, beispielsweise einen eindeutigen Namen für **OsDisk** wie VHD`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Erstellen Sie eine Netzwerkkarte im selben oder einem anderen virtuellen Netzwerk.
* Erstellen Sie die geänderte Vorlage JSON-Datei eine Bereitstellung in der Ressourcengruppe, in der das virtuelle Netzwerk einrichten.

### <a name="use-a-quickstart-template"></a>Schnellstart-Vorlage verwenden

Soll das Netzwerk automatisch beim eine VM aus dem Abbild erstellen, können Sie Ressourcen in einer Vorlage angeben. Siehe z. B. [101 Vm aus Benutzer Bild Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) von GitHub. Diese Vorlage erstellt eine VM das benutzerdefinierte Abbild der erforderlichen virtuellen Netzwerk, öffentliche IP-Adresse und NIC-Ressourcen. Eine exemplarische Vorgehensweise bei Verwendung der Vorlage im Azure-Portal finden Sie unter [Erstellen eines virtuellen Computers von einem benutzerdefinierten Abbild mithilfe einer Vorlage Ressourcenmanager](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-the-azure-vm-create-command"></a>Azure Vm erstellen Befehl

In der Regel ist es am einfachsten mithilfe einer Vorlage Ressourcen-Manager um einen virtuellen Computer aus dem Bild zu erstellen. Jedoch können die VM _unbedingt_ mit den **Azure Vm erstellen** Befehl **-q** (**-Bild Urn**) Parameter. Wenn Sie diese Methode verwenden, übergeben Sie auch den Parameter **-d** (**Betriebssystem-Datenträger Vhd**) Geben Sie den Speicherort der OS VHD-Datei für den neuen virtuellen Computer. Diese Datei muss Vhds Container des Speicherkontos, Bild-VHD-Datei gespeichert ist. Der Befehl übernimmt die VHD-Datei für die neue VM **Vhds** Container.

Gehen Sie bevor Sie mit dem Bild ausführen **Azure Vm zu erstellen** folgendermaßen vor:

1.  Eine Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe für die Bereitstellung zu identifizieren.

2.  Erstellen Sie eine öffentliche IP-Adressenressource und eine Ressource NIC für den neuen virtuellen Computer. Schritte auf ein virtuelles Netzwerk, öffentliche IP-Adresse und NIC mithilfe der CLI finden Sie weiter oben in diesem Artikel. (**Azure Vm erstellen** können eine NIC jedoch zusätzliche Parameter für ein virtuelles Netzwerk und Subnetz übergeben.)


Führen Sie einen Befehl, der die neue Betriebssystem-VHD-Datei und das vorhandene Bild URIs übergibt. In diesem Beispiel wird eine Größe in der Region USA OST Standard_A1 VM erstellt.

    azure vm create MyResourceGroup1 myNewVM eastus Linux -d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" -Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd" -z Standard_A1 -u myAdminname -p myPassword -f myNIC

Zusätzliche Befehlsoptionen ausführen `azure help vm create`.

## <a name="next-steps"></a>Nächste Schritte

Verwalten Ihrer virtuellen Computer mit der CLI finden Sie die Aufgaben in [Bereitstellen und managen von virtuellen Computern mithilfe von Azure-Ressourcen-Manager Vorlagen und Azure-CLI](virtual-machines-linux-cli-deploy-templates.md).
