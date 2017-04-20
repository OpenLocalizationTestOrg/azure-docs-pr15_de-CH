## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>Bereitstellen Sie ARM-Vorlage mithilfe der Azure-CLI

Gehen Sie wie folgt vor Bereitstellung ARM-Vorlage mithilfe von Azure CLI herunterladen.

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../articles/xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie die **`azure config mode`** Befehl Ressourcen-Manager-Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    New mode is arm

3. Führen Sie bei Bedarf die **`azure group create`** eine neue Ressourcengruppe erstellen, wie unten dargestellt. Beachten Sie die Ausgabe des Befehls. Die Liste nach die Ausgabe der Parameter erläutert. Weitere Informationen zu Ressourcengruppen Überblick [Azure Ressourcen-Manager](../articles/resource-group-overview.md).

        azure group create -n TestRG -l centralus

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (bzw. -Namen)**. Name für neue Ressourcengruppe. In diesem Szenario *TestRG*.
    - **(oder -Speicherort)**. Azure-Region, in dem die neue Ressourcengruppe erstellt werden. In diesem Szenario *Centralus*.

4. Ausführen der **`azure group deployment create`** -Cmdlet zum Bereitstellen neuen Vnets mithilfe der Vorlage und Parameter Dateien heruntergeladen und über geändert. Die Liste nach die Ausgabe der Parameter erläutert.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **g (oder -Ressourcengruppen)**. Name der Ressourcengruppe die neue VNet wird in erstellt.
    - **-f (oder -Vorlage-Datei)**. Pfad zu der Vorlagendatei ARM.
    - **-e (oder -Parameter-Datei)**. Pfad zu der Datei ARM.

5. Führen Sie die **`azure network vnet show`** Befehl, um die Eigenschaften des neuen Vnet wie unten dargestellt.

        azure network vnet show -g TestRG -n TestVNet

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
