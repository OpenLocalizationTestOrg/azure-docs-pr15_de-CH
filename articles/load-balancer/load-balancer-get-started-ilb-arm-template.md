<properties
   pageTitle="Erstellen einer internen Lastenausgleich mithilfe einer Vorlage im Ressourcenmanager | Microsoft Azure"
   description="Informationen Sie zum Erstellen einer internen Lastenausgleich mithilfe einer Vorlage im Ressourcen-Manager"
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

# <a name="create-an-internal-load-balancer-using-a-template"></a>Erstellen Sie ein interner Lastenausgleich mithilfe einer Vorlage

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassische Bereitstellungsmodell](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Bereitstellen Sie die Vorlage mithilfe von auf Bereitstellung

Vorlage verfügbar im öffentlichen Repository verwendet Parameterdatei standardmäßig Werte mit das oben beschriebene Szenario generiert. Zum Bereitstellen dieser Vorlage auf bereitstellen, folgen [diesem Link](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer)auf **in Azure bereitstellen**, ersetzen die Standardparameterwerte ggf. und gehen in das Portal.

## <a name="deploy-the-template-by-using-powershell"></a>Bereitstellen der Vorlage mithilfe von PowerShell

Gehen Sie wie folgt vor Bereitstellung mithilfe von PowerShell heruntergeladene Vorlage.

1. Wenn Azure PowerShell noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../../articles/powershell-install-configure.md) und gehen Sie bis Ende Azure anmelden und Ihr Abonnement auswählen.
2. Herunterladen der Datei auf Ihrer lokalen Festplatte.
3. Die Datei bearbeiten und speichern.
4. Führen Sie das Cmdlet **Neu AzureRmResourceGroupDeployment** um eine Ressourcengruppe mit der Vorlage erstellen.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Bereitstellen der Vorlage mithilfe der Azure-CLI

Um die Vorlage mithilfe der Azure-CLI bereitstellen, gehen Sie die.

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../../articles/xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.
2. Führen Sie den Befehl **Azure Config Modus** in Ressourcen-Manager-Modus wechseln, wie unten dargestellt.

        azure config mode arm

    Hier ist die erwartete Ausgabe für den Befehl oben:

        info:    New mode is arm

3. Öffnen Sie die Parameterdatei, seinen Inhalt und in einer Datei auf Ihrem Computer speichern. In diesem Beispiel werden die Datei in *parameters.json*gespeichert.

4. Führen Sie den Befehl **Azure Bereitstellung erstellen** neue interne Lastenausgleich mithilfe der Vorlage und Parameter-Dateien heruntergeladen und über geändert bereitstellen. Die Liste nach die Ausgabe der Parameter erläutert.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren Sie einen Quell-IP-Affinität mit Load Balancer Verteilung Modus](load-balancer-distribution-mode.md)

[Konfigurieren Sie im Leerlauf TCP-Timeouteinstellungen für die Lastenausgleich](load-balancer-tcp-idle-timeout.md)



