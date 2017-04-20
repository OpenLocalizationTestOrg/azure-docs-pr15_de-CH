<properties
    pageTitle="Virtuelles Netzwerk mit PowerShell herstellen Sie Ihrer app"
    description="Hinweise und virtuelle Netzwerke mit PowerShell arbeiten"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="wpickett"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="ccompy"/>

# <a name="connect-your-app-to-your-virtual-network-by-using-powershell"></a>Virtuelles Netzwerk mit PowerShell herstellen Sie Ihrer app #

## <a name="overview"></a>Übersicht ##

In Azure App Service können Sie Ihre app (Web, Mobil oder API) in Ihrem Abonnement Azure virtuellen Netzwerk (VNet). Dieses Feature heißt VNet-Integration. Die VNet-Integration mit der Funktion App Service-Umgebung verwechseln nicht dem eine Instanz von Azure App Service im virtuellen Netzwerk ausführen können.

Die VNet-Integration hat eine Benutzeroberfläche (UI) in das neue Portal, mit denen Sie virtuelle Netzwerke integrieren, die mit dem klassischen Bereitstellungsmodell oder Bereitstellungsmodell Azure-Ressourcen-Manager bereitgestellt werden. Wenn Sie die Funktion erfahren möchten, finden Sie unter [Integrieren Ihrer Anwendung mit einer Azure virtual Network](web-sites-integrate-with-vnet.md).

Dieser Artikel ist nicht zur Verwendung die Benutzeroberfläche sondern Integration mit PowerShell aktivieren. Da die Befehle für jedes Bereitstellungsmodell unterschiedlich sind, wurde dieser Artikel einen Abschnitt für jedes Bereitstellungsmodell.  

Bevor Sie mit diesem Artikel fortfahren, sicherzustellen Sie, dass Sie:

- Die neuesten Azure PowerShell SDK installiert. Sie können dies mit dem Webplattform-Installer installieren.
- Eine Anwendung in Azure App Dienst Standard oder Premium-SKU.

## <a name="classic-virtual-networks"></a>Klassische virtuelle Netzwerke ##

Dieser Abschnitt erläutert drei Aufgaben für virtuelle Netzwerke, die das klassische Bereitstellungsmodell verwenden:

1. Verbinden Sie Ihre app zu einem vorhandenen virtuellen Netzwerk, das Gateway und Punkt-zu-Standort-Verbindung konfiguriert.
1. Virtuelle Integration Netzwerkinformationen für Ihre Anwendung zu aktualisieren.
1. Trennen Sie Ihre Anwendung aus dem virtuellen Netzwerk

### <a name="connect-an-app-to-a-classic-vnet"></a>Verbinden einer Anwendung mit klassischen VNet ###

Um eine Anwendung zu einem virtuellen Netzwerk verbinden, folgendermaßen Sie drei:

1. Web app erklären Sie, dass sie ein bestimmtes virtuelles Netzwerk beitreten. Die Anwendung generiert ein Zertifikat, das Punkt-zu-Standort-Verbindung mit dem virtuellen Netzwerk gewährt wird.
1. Hochladen von Web app Zertifikat an das virtuelle Netzwerk, und rufen Sie den Punkt-zu-Standort-VPN-Paket-URI.
1. Aktualisieren Sie virtuelle Netzwerkschnittstelle Web app mit Punkt-zu-Standort-Paket-URI.

Die erste und dritte Schritt vollständig skriptfähig sind, jedoch im zweite Schritt muss eine einmalige, manuelle Aktion Portal oder Access so **PLATZIEREN** oder **Patches** auf den Endpunkt des virtuellen Azure-Ressourcen-Manager. Support von Azure aktiviert haben. Bevor Sie beginnen, stellen Sie sicher, dass Sie ein klassisches virtuelles Netzwerk haben, Punkt-zu-Standort-Verbindung bereits aktiviert und bereitgestellte Gateway. Das Gateway erstellen und Punkt-zu-Standort-Verbindung aktivieren, müssen Sie das Portal verwenden, wie auf [einem VPN-Gateway][createvpngateway].

Das klassische virtuelle Netzwerk muss im gleichen Abonnement als Ihren App Service-Plan, der die Anwendung enthält, die Integration mit.

##### <a name="set-up-azure-powershell-sdk"></a>Einrichten von Azure PowerShell SDK #####

Öffnen Sie ein PowerShell und richten Sie Ihre Azure-Konto und Ihr Abonnement mit:

    Login-AzureRmAccount

Dieser Befehl öffnet zu Azure-Anmeldeinformationen aufgefordert. Nach der Anmeldung, verwenden Sie einen der folgenden Befehle das Abonnement aus, dem Sie verwenden möchten. Stellen Sie sicher, dass Sie das Abonnement, die im virtuellen Netzwerk und App Service-Plan sind verwenden.

    Select-AzureRmSubscription –SubscriptionName [WebAppSubscriptionName]

oder

    Select-AzureRmSubscription –SubscriptionId [WebAppSubscriptionId]

##### <a name="variables-used-in-this-article"></a>In diesem Artikel verwendeten Variablen #####

Um Befehle zu vereinfachen, setzen wir eine **$Configuration** -PowerShell-Variable mit der Konfiguration.

Definieren Sie eine Variable wie folgt in PowerShell mit folgenden Parametern:

    $Configuration = @{}
    $Configuration.WebAppResourceGroup = "[Your web app resource group]"
    $Configuration.WebAppName = "[Your web app name]"
    $Configuration.VnetSubscriptionId = "[Your vnet subscription id]"
    $Configuration.VnetResourceGroup = "[Your vnet resource group]"
    $Configuration.VnetName = "[Your vnet name]"

App-Speicherort sollte die Position ohne Leerzeichen. Westen der USA ist beispielsweise Westus.

    $Configuration.WebAppLocation = "[Your web app Location]"

Das nächste Element ist, das Zertifikat geschrieben werden soll. Es sollte einen beschreibbaren Pfad auf dem lokalen Computer. Achten Sie, CER Ende.

    $Configuration.GeneratedCertificatePath = "[C:\Path\To\Certificate.cer]"

Um zu sehen was Sie, geben Sie **$Configuration**.

    > $Configuration

    Name                           Value
    ----                           -----
    GeneratedCertificatePath       C:\vnetcert.cer
    VnetSubscriptionId             efc239a4-88f9-2c5e-a9a1-3034c21ad496
    WebAppResourceGroup            vnetdemo-rg
    VnetResourceGroup              testase1-rg
    VnetName                       TestNetwork
    WebAppName                     vnetintdemoapp
    WebAppLocation                 centralus

Im Rest dieses Abschnitts wird vorausgesetzt, dass Sie eine Variable als gerade beschrieben.

##### <a name="declare-the-virtual-network-to-the-app"></a>Deklarieren Sie das virtuelle Netzwerk an die Anwendung #####

Verwenden Sie den folgenden Befehl an der Anwendung mitteilt, dass er dieses virtuelle Netzwerk verwenden. Dadurch wird die Anwendung erforderlichen Zertifikate generiert:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -PropertyObject @{"VnetResourceId" = "/subscriptions/$($Configuration.VnetSubscriptionId)/resourceGroups/$($Configuration.VnetResourceGroup)/providers/Microsoft.ClassicNetwork/virtualNetworks/$($Configuration.VnetName)"} -Location $Configuration.WebAppLocation -ApiVersion 2015-07-01

Wenn dieser Befehl erfolgreich ausgeführt wird, sollte **$vnet** eine Variable **Eigenschaften** enthalten. Variable **Eigenschaften** sollte einen Fingerabdruck des Zertifikats und der Daten.

##### <a name="upload-the-web-app-certificate-to-the-virtual-network"></a>Hochladen Sie Web app Zertifikat mit dem virtuellen Netzwerk #####

Ein Handbuch ist einmal Schritt für jedes Abonnement und virtuelles Netzwerk zusammen. Wenn Sie apps im Abonnement ein virtuelles Netzwerk a verbinden, müssen Sie also erst unabhängig davon, wie viele apps Sie konfigurieren diesen Schritt ausführen. Wenn Sie eine neue Anwendung in ein anderes virtuelles Netzwerk hinzufügen, müssen Sie erneut. Der Grund dafür ist, dass mehrere Zertifikate Ebene Abonnement Azure App Service generiert, und die Gruppe wird einmal für jedes virtuelle Netzwerk apps herstellen.

Die Zertifikate werden bereits festgelegt haben diese Schritte oder integriert im gleichen virtuellen Netzwerk mithilfe der Portalwebsite.

Der erste Schritt ist die CER-Datei generieren. Der zweite Schritt ist die CER-Datei mit dem virtuellen Netzwerk hochladen. Führen Sie die folgenden Befehle aus den API-Aufruf in vorherigen Schritt CER-Datei zu generieren.

    $certBytes = [System.Convert]::FromBase64String($vnet.Properties.certBlob)
    [System.IO.File]::WriteAllBytes("$($Configuration.GeneratedCertificatePath)", $certBytes)

Das Zertifikat finden Sie in der Lage, **$Configuration.GeneratedCertificatePath** gibt.

Verwenden Sie das Zertifikat manuell hochladen, [Azure-Portal] [ azureportal] und **(Classic) durchsuchen virtuelles Netzwerk** > **VPN-Verbindungen** > **Punkt-zu-Standort-** > **Zertifikate verwalten**. Laden Sie hier Ihr Zertifikat.

##### <a name="get-the-point-to-site-package"></a>Das Punkt-zu-Standort-Paket #####

Der nächste Schritt beim Einrichten einer Verbindungs virtuelles Netzwerk eine Webanwendung wird das Punkt-zu-Standort-Paket und Ihrer Anwendung bereitstellen.

Speichern Sie die folgende Vorlage in einer Datei namens GetNetworkPackageUri.json irgendwo auf Ihrem Computer, beispielsweise C:\Azure\Templates\GetNetworkPackageUri.json.

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "certData": {
                "type": "string"
            },
            "certThumbprint": {
                "type": "string"
            },
            "networkName": {
                "type": "string"
            }
        },
        "variables": {
            "legacyVnetName": "[concat('Group ', resourceGroup().name, ' ', parameters('networkName'))]"
            },
            "resources": [
            ],
        "outputs" : {
            "PackageUri" :
            {
            "value" : "[listPackage(resourceId('Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates', parameters('networkName'), 'primary', parameters('certThumbprint')), '2014-06-01').packageUri]", "type" : "string"
            }
        }
    }


Festlegen Sie Parameter:

    $parameters = @{"certData" = $vnet.Properties.certBlob ;
    certThumbprint = $vnet.Properties.certThumbprint ;
    "networkName" = $Configuration.VnetName }

Rufen Sie das Skript:

    $output = New-AzureRmResourceGroupDeployment -Name unused -ResourceGroupName $Configuration.VnetResourceGroup -TemplateParameterObject $parameters -TemplateFile C:\PATH\TO\GetNetworkPackageUri.json


Die Variable **$output. Outputs.packageUri** enthält nun den Paket-URI zu Ihrer Anwendung.

##### <a name="upload-the-point-to-site-package-to-your-app"></a>Hochladen Sie Punkt-zu-Standort-Paket für Ihre Anwendung #####

Der letzte Schritt ist die Anwendung dieses Paket zu. Führen Sie einfach den nächsten Befehl:

    $vnet = New-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)/primary" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-07-01 -PropertyObject @{"VnetName" = $Configuration.VnetName ; "VpnPackageUri" = $($output.Outputs.packageUri).Value } -Location $Configuration.WebAppLocation

Werden aufgefordert zu bestätigen, dass Sie eine vorhandene Ressource überschreiben, müssen Sie es zulassen.

Wenn dieser Befehl erfolgreich ausgeführt wird, sollte Ihre Anwendung jetzt an das virtuelle Netzwerk verbunden. Bestätigen Sie erfolgreich zur Konsole app und geben Sie Folgendes ein:

    SET WEBSITE_

Ist eine Umgebungsvariable namens WEBSITE_VNETNAME mit dem Wert, der den Namen des virtuellen Zielnetzwerks übereinstimmt, haben alle Konfigurationen erfolgreich.

### <a name="update-classic-vnet-integration-information"></a>Klassische VNet Integrationsinformationen aktualisieren ###

Aktualisieren oder Ihre Daten synchronisieren, wiederholen die Schritte, die Sie im ersten Schritt erstellt die Integration befolgt. Diese Schritte sind:

1. Definieren Sie die Konfigurationsinformationen.
1. Deklarieren Sie das virtuelle Netzwerk an die Anwendung.
1. Punkt-zu-Standort-Paket zu erhalten.
1. Laden Sie das Punkt-zu-Standort-Paket für Ihre Anwendung.

### <a name="disconnect-your-app-from-a-classic-vnet"></a>Klassische VNet Ihrer Anwendung trennen ###

So trennen Sie die app benötigen Sie die Konfigurationsinformationen während virtuelle Vernetzung festgelegt wurde. Mithilfe dieser Informationen wird dann ein Befehl Ihrer Anwendung aus dem virtuellen Netzwerk trennen.

    $vnet = Remove-AzureRmResource -Name "$($Configuration.WebAppName)/$($Configuration.VnetName)" -ResourceGroupName $Configuration.WebAppResourceGroup -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-07-01

## <a name="resource-manager-virtual-networks"></a>Ressourcen-Manager virtuelle Netzwerke ##

Ressourcen-Manager virtuelle Netzwerke haben Azure-Ressourcen-Manager-APIs vereinfachen die einige klassische virtuelle Netzwerke gegenüber. Wir haben eine Skript, mit denen Sie die folgenden Aufgaben ausführen:

- Erstellen Sie ein virtuelles Netzwerk Ressourcen-Manager und integrieren Sie Ihre app.
- Erstellen eines Gateways, Konfigurieren von Punkt-zu-Standort-Verbindung in einem bereits vorhandenen virtuellen Netzwerk Ressourcen-Manager und dann Ihre app integrieren.
- Ein bereits vorhandener Ressourcen-Manager virtuelle Netzwerk ein Gateway und Punkt-zu-Standort-Verbindung aktiviert integrieren Sie die app.
- Trennen Sie Ihre Anwendung aus dem virtuellen Netzwerk

### <a name="resource-manager-vnet-app-service-integration-script"></a>Ressourcenmanager VNet App Service Integration Skript ###

Das folgende Skript kopieren und in einer Datei speichern. Möchten Sie das Skript verwenden, sollten Sie gerne Dinge Ressourcenmanager virtual Network Einrichtung zu lernen.


    function ReadHostWithDefault($message, $default)
    {
        $result = Read-Host "$message [$default]"
        if($result -eq "")
        {
            $result = $default
        }
            return $result
        }

    function PromptCustom($title, $optionValues, $optionDescriptions)
    {
        Write-Host $title
        Write-Host
        $a = @()
        for($i = 0; $i -lt $optionValues.Length; $i++){
            Write-Host "$($i+1))" $optionDescriptions[$i]
        }
        Write-Host

        while($true)
        {
            Write-Host "Choose an option: "
            $option = Read-Host
            $option = $option -as [int]

            if($option -ge 1 -and $option -le $optionValues.Length)
            {
                return $optionValues[$option-1]
            }
        }
    }

    function PromptYesNo($title, $message, $default = 0)
    {
        $yes = New-Object System.Management.Automation.Host.ChoiceDescription "&Yes", ""
        $no = New-Object System.Management.Automation.Host.ChoiceDescription "&No", ""
        $options = [System.Management.Automation.Host.ChoiceDescription[]]($yes, $no)
        $result = $host.ui.PromptForChoice($title, $message, $options, $default)
        return $result
    }

    function CreateVnet($resourceGroupName, $vnetName, $vnetAddressSpace, $vnetGatewayAddressSpace, $location)
    {
        Write-Host "Creating a new VNET"
        $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
        New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix $vnetAddressSpace -Subnet $gatewaySubnet
    }

    function CreateVnetGateway($resourceGroupName, $vnetName, $vnetIpName, $location, $vnetIpConfigName, $vnetGatewayName, $certificateData, $vnetPointToSiteAddressSpace)
    {
        $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName
        $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

        Write-Host "Creating a public IP address for this VNET"
        $pip = New-AzureRmPublicIpAddress -Name $vnetIpName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $vnetIpConfigName -Subnet $subnet -PublicIpAddress $pip

        Write-Host "Adding a root certificate to this VNET"
        $root = New-AzureRmVpnClientRootCertificate -Name "AppServiceCertificate.cer" -PublicCertData $certificateData

        Write-Host "Creating Azure VNET Gateway. This may take up to an hour."
        New-AzureRmVirtualNetworkGateway -Name $vnetGatewayName -ResourceGroupName $resourceGroupName -Location $location -IpConfigurations $ipconf -GatewayType Vpn -VpnType RouteBased -EnableBgp $false -GatewaySku Basic -VpnClientAddressPool $vnetPointToSiteAddressSpace -VpnClientRootCertificates $root
    }

    function AddNewVnet($subscriptionId, $webAppResourceGroup, $webAppName)
    {
        Write-Host "Adding a new Vnet"
        Write-Host
        $vnetName = Read-Host "Specify a name for this Virtual Network"

        $vnetGatewayName="$($vnetName)-gateway"
        $vnetIpName="$($vnetName)-ip"
        $vnetIpConfigName="$($vnetName)-ipconf"

        # Virtual Network settings
        $vnetAddressSpace="10.0.0.0/8"
        $vnetGatewayAddressSpace="10.5.0.0/16"
        $vnetPointToSiteAddressSpace="172.16.0.0/16"

        $changeRequested = 0
        $resourceGroupName = $webAppResourceGroup

        while($changeRequested -eq 0)
        {
            Write-Host
            Write-Host "Currently, I will create a VNET with the following settings:"
            Write-Host
            Write-Host "Virtual Network Name: $vnetName"
            Write-Host "Resource Group Name:  $resourceGroupName"
            Write-Host "Gateway Name: $vnetGatewayName"
            Write-Host "Vnet IP name: $vnetIpName"
            Write-Host "Vnet IP config name:  $vnetIpConfigName"
            Write-Host "Address Space:$vnetAddressSpace"
            Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
            Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
            Write-Host
            $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

            if($changeRequested -eq 0)
            {
                $vnetName = ReadHostWithDefault "Virtual Network Name" $vnetName
                $resourceGroupName = ReadHostWithDefault "Resource Group Name" $resourceGroupName
                $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                $vnetAddressSpace = ReadHostWithDefault "Vnet Address Space" $vnetAddressSpace
                $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
            }
        }

        $ErrorActionPreference = "Stop";

        # We create the virtual network and add it here. The way this works is:
        # 1) Add the VNET association to the App. This allows the App to generate certificates, etc. for the VNET.
        # 2) Create the VNET and VNET gateway, add the certificates, create the public IP, etc., required for the gateway
        # 3) Get the VPN package from the gateway and pass it back to the App.

        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup
        $location = $webApp.Location

        Write-Host "Creating App association to VNET"
        $propertiesObject = @{
         "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($resourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
        }
        $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        CreateVnet $resourceGroupName $vnetName $vnetAddressSpace $vnetGatewayAddressSpace $location

        CreateVnetGateway $resourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $resourceGroupName -VirtualNetworkGatewayName $vnetGatewayName -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $VirtualNetworkName; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnetName)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $webAppResourceGroup -Force

        Write-Host "Finished!"
    }

    function AddExistingVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $ErrorActionPreference = "Stop";

        # At this point, the gateway should be able to be joined to an App, but may require some minor tweaking. We will declare to the App now to use this VNET
        Write-Host "Getting App information"
        $webApp = Get-AzureRmResource -ResourceName $webAppName -ResourceType "Microsoft.Web/sites" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $location = $webApp.Location

        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"
        }

        # Display existing vnets
        $vnets = Get-AzureRmVirtualNetwork
        $vnetNames = @()
        foreach($vnet in $vnets)
        {
            $vnetNames += $vnet.Name
        }

        Write-Host
        $vnet = PromptCustom "Select a VNET to integrate with" $vnets $vnetNames

        # We need to check if this VNET is able to be joined to a App, based on following criteria
            # If there is no gateway, we can create one.
            # If there is a gateway:
                # It must be of type Vpn
                # It must be of VpnType RouteBased
                # If it doesn't have the right certificate, we will need to add it.
                # If it doesn't have a point-to-site range, we will need to add it.

        $gatewaySubnet = $vnet.Subnets | Where-Object { $_.Name -eq "GatewaySubnet" }

        if($gatewaySubnet -eq $null -or $gatewaySubnet.IpConfigurations -eq $null -or $gatewaySubnet.IpConfigurations.Count -eq 0)
        {
            $ErrorActionPreference = "Continue";
            # There is no gateway. We need to create one.
            Write-Host "This Virtual Network has no gateway. I will need to create one."

            $vnetName = $vnet.Name
            $vnetGatewayName="$($vnetName)-gateway"
            $vnetIpName="$($vnetName)-ip"
            $vnetIpConfigName="$($vnetName)-ipconf"

            # Virtual Network settings
            $vnetAddressSpace="10.0.0.0/8"
            $vnetGatewayAddressSpace="10.5.0.0/16"
            $vnetPointToSiteAddressSpace="172.16.0.0/16"

            $changeRequested = 0

            Write-Host "Your VNET is in the address space $($vnet.AddressSpace.AddressPrefixes), with the following Subnets:"
            foreach($subnet in $vnet.Subnets)
            {
                Write-Host "$($subnet.Name): $($subnet.AddressPrefix)"
            }

            $vnetGatewayAddressSpace = Read-Host "Please choose a GatewaySubnet address space"

            while($changeRequested -eq 0)
            {
                Write-Host
                Write-Host "Currently, I will create a VNET gateway with the following settings:"
                Write-Host
                Write-Host "Virtual Network Name: $vnetName"
                Write-Host "Resource Group Name:  $($vnet.ResourceGroupName)"
                Write-Host "Gateway Name: $vnetGatewayName"
                Write-Host "Vnet IP name: $vnetIpName"
                Write-Host "Vnet IP config name:  $vnetIpConfigName"
                Write-Host "Address Space:$($vnet.AddressSpace.AddressPrefixes)"
                Write-Host "Gateway Address Space:$vnetGatewayAddressSpace"
                Write-Host "Point-To-Site Address Space:  $vnetPointToSiteAddressSpace"
                Write-Host
                $changeRequested = PromptYesNo "" "Do you wish to change these settings?" 1

                if($changeRequested -eq 0)
                {
                    $vnetGatewayName = ReadHostWithDefault "Vnet Gateway Name" $vnetGatewayName
                    $vnetIpName = ReadHostWithDefault "Vnet IP name" $vnetIpName
                    $vnetIpConfigName = ReadHostWithDefault "Vnet IP configuration name" $vnetIpConfigName
                    $vnetGatewayAddressSpace = ReadHostWithDefault "Vnet Gateway Address Space" $vnetGatewayAddressSpace
                    $vnetPointToSiteAddressSpace = ReadHostWithDefault "Vnet Point-to-site Address Space" $vnetPointToSiteAddressSpace
                }
            }

            $ErrorActionPreference = "Stop";

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnetName)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # If there is no gateway subnet, we need to create one.
            if($gatewaySubnet -eq $null)
            {
                $gatewaySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix $vnetGatewayAddressSpace
                $vnet.Subnets.Add($gatewaySubnet);
                Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
            }

            CreateVnetGateway $vnet.ResourceGroupName $vnetName $vnetIpName $location $vnetIpConfigName $vnetGatewayName $virtualNetwork.Properties.CertBlob $vnetPointToSiteAddressSpace

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $vnetGatewayName
        }
        else
        {
            $uriParts = $gatewaySubnet.IpConfigurations[0].Id.Split('/')
            $gatewayResourceGroup = $uriParts[4]
            $gatewayName = $uriParts[8]

            $gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $vnet.ResourceGroupName -Name $gatewayName

            # validate gateway types, etc.
            if($gateway.GatewayType -ne "Vpn")
            {
                Write-Error "This gateway is not of the Vpn type. It cannot be joined to an App."
                return
            }

            if($gateway.VpnType -ne "RouteBased")
            {
                Write-Error "This gateways Vpn type is not RouteBased. It cannot be joined to an App."
                return
            }

            if($gateway.VpnClientConfiguration -eq $null -or $gateway.VpnClientConfiguration.VpnClientAddressPool -eq $null)
            {
                Write-Host "This gateway does not have a Point-to-site Address Range. Please specify one in CIDR notation, e.g. 10.0.0.0/8"
                $pointToSiteAddress = Read-Host "Point-To-Site Address Space"
                Set-AzureRmVirtualNetworkGatewayVpnClientConfig -VirtualNetworkGateway $gateway.Name -VpnClientAddressPool $pointToSiteAddress
            }

            Write-Host "Creating App association to VNET"
            $propertiesObject = @{
             "vnetResourceId" = "/subscriptions/$($subscriptionId)/resourceGroups/$($vnet.ResourceGroupName)/providers/Microsoft.Network/virtualNetworks/$($vnet.Name)"
            }

            $virtualNetwork = New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

            # We need to check if the certificate here exists in the gateway.
            $certificates = $gateway.VpnClientConfiguration.VpnClientRootCertificates

            $certFound = $false
            foreach($certificate in $certificates)
            {
                if($certificate.PublicCertData -eq $virtualNetwork.Properties.CertBlob)
                {
                    $certFound = $true
                    break
                }
            }

            if(-not $certFound)
            {
                Write-Host "Adding certificate"
                Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName "AppServiceCertificate.cer" -PublicCertData $virtualNetwork.Properties.CertBlob -VirtualNetworkGatewayName $gateway.Name
            }
        }

        # Now finish joining by getting the VPN package and giving it to the App
        Write-Host "Retrieving VPN Package and supplying to App"
        $packageUri = Get-AzureRmVpnClientPackage -ResourceGroupName $vnet.ResourceGroupName -VirtualNetworkGatewayName $gateway.Name -ProcessorArchitecture Amd64

        # Put the VPN client configuration package onto the App
        $PropertiesObject = @{
        "vnetName" = $vnet.Name; "vpnPackageUri" = $packageUri
        }

        New-AzureRmResource -Location $location -Properties $PropertiesObject -ResourceName "$($webAppName)/$($vnet.Name)/primary" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections/gateways" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName -Force

        Write-Host "Finished!"
    }

    function RemoveVnet($subscriptionId, $resourceGroupName, $webAppName)
    {
        $webAppConfig = Get-AzureRmResource -ResourceName "$($webAppName)/web" -ResourceType "Microsoft.Web/sites/config" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        $currentVnet = $webAppConfig.Properties.VnetName
        if($currentVnet -ne $null -and $currentVnet -ne "")
        {
            Write-Host "Currently connected to VNET $currentVnet"

            Remove-AzureRmResource -ResourceName "$($webAppName)/$($currentVnet)" -ResourceType "Microsoft.Web/sites/virtualNetworkConnections" -ApiVersion 2015-08-01 -ResourceGroupName $resourceGroupName
        }
          else
        {
          Write-Host "Not connected to a VNET."
        }
    }

    Write-Host "Please Login"
    Login-AzureRmAccount

    # Choose subscription. If there's only one we will choose automatically

    $subs = Get-AzureRmSubscription
    $subscriptionId = ""

    if($subs.Length -eq 0)
    {
        Write-Error "No subscriptions bound to this account."
        return
    }

    if($subs.Length -eq 1)
    {
        $subscriptionId = $subs[0].SubscriptionId
    }
    else
    {
        $subscriptionChoices = @()
        $subscriptionValues = @()

        foreach($subscription in $subs){
        $subscriptionChoices += "$($subscription.SubscriptionName) ($($subscription.SubscriptionId))";
        $subscriptionValues += ($subscription.SubscriptionId);
        }

        $subscriptionId = PromptCustom "Choose a subscription" $subscriptionValues $subscriptionChoices
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $resourceGroup = Read-Host "Please enter the Resource Group of your App"

    $appName = Read-Host "Please enter the Name of your App"

    $options = @("Add a NEW Virtual Network to an App", "Add an EXISTING Virtual Network to an App", "Remove a Virtual Network from an App");
    $optionValues = @(0, 1, 2)
    $option = PromptCustom "What do you want to do?" $optionValues $options

    if($option -eq 0)
    {
        AddNewVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 1)
    {
        AddExistingVnet $subscriptionId $resourceGroup $appName
    }
    if($option -eq 2)
    {
        RemoveVnet $subscriptionId $resourceGroup $appName
    }

Speichern Sie das Skript. In diesem Artikel heißt es V2VnetAllinOne.ps1, aber einen anderen Namen verwenden. Es sind keine Argumente für dieses Skript. Sie führen diese. Erstes Skript tun wird aufgefordert, anmelden. Nach der Anmeldung, das Skript ruft Details zu Ihrem Konto ab und gibt eine Liste von Abonnements. Die Anforderung Ihre Anmeldeinformationen nicht zählen, sieht Ausführung des ersten Skripts:

    PS C:\Users\ccompy\Documents\VNET> .\V2VnetAllInOne.ps1
    Please Login

    Environment           : AzureCloud
    Account               : ccompy@microsoft.com
    TenantId              : 722278f-fef1-499f-91ab-2323d011db47
    SubscriptionId        : af5358e1-acac-2c90-a9eb-722190abf47a
    CurrentStorageAccount :

    Choose a subscription

    1) Demo-Abonnement (af5358e1-acac-2c90-a9eb-722190abf47a)
    2) MS-Test (a5350f55-dd5a-41ec-2ddb-ff7b911bb2ef)
    3) Lila Demo Abonnement (2d4c99a4-57f9-4d5e-a0a1-0034c52db59d)

    Wählen Sie eine Option: 3

    Konto: ccompy@microsoft.com Umgebung: AzureCloud Abonnement: 2d4c99a4-57f9-4d5e-a0a1-0034c52db59d Mieter: 722278f-fef1-499f-91ab-2323d011db47

    Geben Sie der Ressourcengruppe der App: Rg Hcdemo Geben Sie den Namen der Anwendung: v2vnetpowershell Was möchten Sie tun?

    1) Eine Anwendung ein neues virtuelles Netzwerk hinzufügen
    2) Fügen Sie ein VORHANDENES virtuelles Netzwerk App
    3) Ein virtuelles Netzwerk aus einer Anwendung entfernen

Der Rest dieses Abschnitts beschreibt jede dieser drei Optionen.

### <a name="create-a-resource-manager-vnet-and-integrate-with-it"></a>Erstellen Sie ein Ressourcen-Manager-VNet und integrieren Sie ###

Erstellen Sie ein neues virtuelles Netzwerk, das Ressourcen-Manager Bereitstellung verwendet, Ihre app integrieren und wählen **1) eine Anwendung ein neues virtuelles Netzwerk hinzufügen**. Dies ist der Name des virtuellen Netzwerks abgefragt. In meinem Fall verwendet Siehe Folgendes, den Namen v2pshell.

Das Skript enthält die Details über das virtuelle Netzwerk erstellt wird. Wenn ich will, kann ich die Werte ändern. In diesem Beispiel Ausführung wurde ein virtuelles Netzwerk Folgendes erstellt:

    Virtual Network Name:         v2pshell
    Resource Group Name:          hcdemo-rg
    Gateway Name:                 v2pshell-gateway
    Vnet IP name:                 v2pshell-ip
    Vnet IP config name:          v2pshell-ipconf
    Address Space:                10.0.0.0/8
    Gateway Address Space:        10.5.0.0/16
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):

Wenn Sie die Werte ändern möchten, geben Sie **Y** und ändern. Wenn Sie mit virtuellen Netzwerk zufrieden sind, geben Sie **N** ein oder drücken Sie Eingabe, wenn Sie gefragt werden, die Einstellungen. Von dort bis zum Abschluss des Skripts erfahren Sie einige was ' i bis virtuelles Netzwerk-Gateway zu starten. Dieser Schritt kann bis zu einer Stunde dauern. Während dieser Phase keine Statusanzeige gibt, aber das Skript informiert Sie, wenn das Gateway erstellt wurde.

Wenn das Skript abgeschlossen ist, wird es **beendet**sagen. An diesem Punkt müssen Sie ein virtuelles Netzwerk Ressourcen-Manager, die Namen und ausgewählten Optionen. Dieses neue virtuelle Netzwerk wird auch mit Ihrer Anwendung integriert werden.

### <a name="integrate-your-app-with-a-preexisting-resource-manager-vnet"></a>Eine vorhandene Ressourcen-Manager VNet integrieren Sie app ###

Wenn Sie Wenn Sie ein virtuelles Netzwerk Ressourcen-Manager bereitstellen, Gateway- oder Punkt-zu-Standort-Verbindung über nicht, mit einem vorhandenen virtuellen Netzwerk integrieren sind, wird das Skript, die eingerichtet. Wenn das VNET bereits Dinge eingerichtet wurde, springt das Skript zu app-Integration. Wählen Sie zum Starten dieses **2) eine Anwendung ein VORHANDENES virtuelles Netzwerk hinzufügen**.

Diese Option funktioniert nur, wenn Sie ein bereits vorhandenes Ressourcenmanager virtuelles Netzwerk verfügen, in das Abonnement Ihrer app. Nachdem Sie die Option auswählen, wird eine Liste der Ressourcen-Manager virtuelle Netzwerke angezeigt.   

    Select a VNET to integrate with

    1) v2demonetwork
    2) v2pshell
    3) v2vnetintdemo
    4) v2asenetwork
    5) v2pshell2

    Wählen Sie eine Option: 5

Wählen Sie das virtuelle Netzwerk zu integrieren. Haben Sie bereits ein Gateway, die Punkt-zu-Standort-Verbindung aktiviert ist, wird das Skript einfach Ihre app mit virtuellen Netzwerk integrieren. Wenn Sie kein Gateway haben, müssen Sie das gatewaysubnetz angeben. Ihr gatewaysubnetz muss im Adressraum virtuellen Netzwerk und nicht in ein anderes Subnetz. Wenn Sie ein virtuelles Netzwerk ohne Gateway und diesen Schritt ausführen, Aussehen Dinge wie folgt:

    This Virtual Network has no gateway. I will need to create one.
    Your VNET is in the address space 172.16.0.0/16, with the following Subnets:
    default: 172.16.0.0/24
    Please choose a GatewaySubnet address space: 172.16.1.0/26

In diesem Beispiel wurde einen virtuelles Netzwerk-Gateway, das Folgendes erstellt:

    Virtual Network Name:         v2pshell2
    Resource Group Name:          vnetdemo-rg
    Gateway Name:                 v2pshell2-gateway
    Vnet IP name:                 v2pshell2-ip
    Vnet IP config name:          v2pshell2-ipconf
    Address Space:                172.16.0.0/16
    Gateway Address Space:        172.16.1.0/26
    Point-To-Site Address Space:  172.16.0.0/12

    Do you wish to change these settings?
    [Y] Yes  [N] No  [?] Help (default is "N"):
    Creating App association to VNET

Wenn Sie diese Einstellung ändern möchten, können Sie tun. Andernfalls drücken Sie die EINGABETASTE und das Skript wird erstellen Ihr Gateway und Ihre Anwendung virtuelle Netzwerk. Erstellungszeit Gateway ist jedoch immer noch eine Stunde, so stellen Sie sicher, dass Sie im Hinterkopf behalten. Wenn alles fertig ist, wird das Skript **beendet**sagen.

### <a name="disconnect-your-app-from-a-resource-manager-vnet"></a>Ein Ressourcen-Manager-VNet Ihrer Anwendung trennen ###

Virtuelles Netzwerk trennen Ihrer app nicht beeinträchtigt das Gateway oder Punkt-zu-Standort-Verbindung deaktivieren. Sie können nach allen es etwas verwenden. Es ist auch nicht anderen apps anderen trennen eingegebenen. Wählen Sie zum Ausführen dieser Aktion **3) eine Anwendung ein virtuelles Netzwerk entfernen**. Wenn Sie dies tun, sehen Sie etwa Folgendes:

    Currently connected to VNET v2pshell

    Confirm
    Are you sure you want to delete the following resource:
    /subscriptions/edcc99a4-b7f9-4b5e-a9a1-3034c51db496/resourceGroups/hcdemo-rg/providers/Microsoft.Web/sites/v2vnetpowers
    hell/virtualNetworkConnections/v2pshell
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Obwohl das Skript löschen sagt, wird nicht das virtuelle Netzwerk gelöscht. Die Integration wird gerade entfernt. Bestätigen Sie, dass dies ist, was Sie tun möchten, der Befehl schnell verarbeitet, und erfahren Sie **True** , wenn es fertig ist.

<!--Links-->
[createvpngateway]: http://azure.microsoft.com/documentation/articles/vpn-gateway-point-to-site-create/
[azureportal]: http://portal.azure.com
