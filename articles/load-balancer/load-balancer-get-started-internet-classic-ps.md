<properties
   pageTitle="Erste Schritte beim Erstellen einer Internetschnittstelle Lastenausgleich im klassischen Modus mit PowerShell | Microsoft Azure"
   description="Erstellen Sie ein System zum Lastenausgleich im klassischen Modus mit PowerShell mit Internetzugriff"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Erste Schritte beim Erstellen einer Lastenausgleich (klassisch) in PowerShell mit Internetzugriff

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell. Sie können auch [ein Lastenausgleich mithilfe von Azure Resource Manager mit Internetzugriff erstellen](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Lastenausgleich mit PowerShell einrichten

Gehen Sie folgendermaßen vor um ein Lastenausgleich mit Powershell einzurichten:

1. Wenn Azure PowerShell noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../../articles/powershell-install-configure.md) und gehen Sie bis Ende Azure anmelden und Ihr Abonnement auswählen.


2. PowerShell-Cmdlets können Sie nach dem Erstellen eines virtuellen Computers ein Lastenausgleich auf einem virtuellen Computer im selben Clouddienst hinzufügen.

Im folgenden Beispiel fügen Sie ein Lastenausgleich bezeichnet "Webfarm" Cloud "Mytestcloud" (oder myctestcloud.cloudapp.net), Hinzufügen von Endpunkten für den Lastenausgleich zum virtuellen Computer "web1" und "web2" festgelegt. Lastenausgleich empfängt Netzwerkverkehr an Port 80 und Lastausgleich zwischen den virtuellen Computern festgelegten lokalen Endpunkt (in diesem Fall Port 80) über TCP.


### <a name="step-1"></a>Schritt 1
Erstellen der ersten Virtual Machine "web1" Lastenausgleich

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Schritt 2

Erstellen Sie einen anderen Endpunkt für den zweiten VM "web2" Load Balancer Satz Namen

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Entfernen Sie einen virtuellen Computer aus ein Lastenausgleich

AzureEndpoint entfernen können Sie einen virtuellen Endpunkt aus dem Lastenausgleich entfernen

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Nächste Schritte

Außerdem können Sie [Erste Schritte beim Erstellen einer internen Lastenausgleich](load-balancer-get-started-ilb-classic-ps.md) und welche [Verteilung Modus](load-balancer-distribution-mode.md) für ein spezifisches Load Balancer Netzwerk Verhalten konfigurieren.

Wenn Ihre Anwendung Verbindungen für Server hinter einem Lastenausgleich beibehalten werden soll, können Sie mehr über [Leerlauf TCP-Timeouteinstellungen für einen Lastenausgleich](load-balancer-tcp-idle-timeout.md)verstehen. Damit Kennenlernen Verbindung im Leerlauf Verhalten bei Verwendung der Azure-Lastenausgleich.

