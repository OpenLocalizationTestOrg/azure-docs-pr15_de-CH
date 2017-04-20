<properties
   pageTitle="Erstellen Sie ein System zum Lastenausgleich im Ressourcen-Manager mithilfe einer Vorlage mit Internetzugriff | Microsoft Azure"
   description="Erstellen Sie ein System zum Lastenausgleich im Ressourcen-Manager mithilfe einer Vorlage mit Internetzugriff"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Erstellen einer Vorlage zum Lastenausgleich mit Internetzugriff

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das Bereitstellungsmodell Ressourcenmanager. Sie können auch [ein System zum Lastenausgleich mit klassischen Bereitstellungsmodell mit Internetzugriff erstellen](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Bereitstellen Sie die Vorlage mithilfe von auf Bereitstellung

Vorlage verfügbar im öffentlichen Repository verwendet Parameterdatei standardmäßig Werte mit das oben beschriebene Szenario generiert. Zum Bereitstellen dieser Vorlage auf bereitstellen, folgen [diesem Link](http://go.microsoft.com/fwlink/?LinkId=544801)auf **in Azure bereitstellen**, ersetzen die Standardparameterwerte ggf. und gehen in das Portal.

## <a name="deploy-the-template-by-using-powershell"></a>Bereitstellen der Vorlage mithilfe von PowerShell

Gehen Sie wie folgt vor Bereitstellung mithilfe von PowerShell heruntergeladene Vorlage.

1. Wenn Azure PowerShell noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../../articles/powershell-install-configure.md) und gehen Sie bis Ende Azure anmelden und Ihr Abonnement auswählen.

2. Führen Sie das Cmdlet **Neu AzureRmResourceGroupDeployment** um eine Ressourcengruppe mit der Vorlage erstellen.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Bereitstellen der Vorlage mithilfe der Azure-CLI

Um die Vorlage mithilfe der Azure-CLI bereitstellen, gehen Sie die.

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../../articles/xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **Azure Config Modus** in Ressourcen-Manager-Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    New mode is arm

3. Navigieren Sie zu [Der Schnellstart-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules)aus Ihrem Browser kopieren Sie den Inhalt der Json-Datei und fügen Sie eine neue Datei auf Ihrem Computer. In diesem Szenario würden Sie die Werte unter in einer Datei namens **c:\lb\azuredeploy.parameters.json**kopieren.
4. Führen Sie das Cmdlet **Azure Bereitstellung erstellen** zum Bereitstellen neuer Lastenausgleich mithilfe der Vorlage und Parameter-Dateien heruntergeladen und über geändert. Die Liste nach die Ausgabe der Parameter erläutert.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' -e 'c:\lb\azuredeploy.parameters.json'

## <a name="next-steps"></a>Nächste Schritte

[Beginnen Sie einen internen Lastenausgleich konfigurieren](load-balancer-get-started-ilb-arm-ps.md)

[Einen Load Balancer Verteilung Modus konfigurieren](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)
