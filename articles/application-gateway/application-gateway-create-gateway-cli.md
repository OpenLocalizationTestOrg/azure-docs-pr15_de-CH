<properties
   pageTitle="Erstellen Sie ein Gateway mit der Azure-CLI im Ressourcenmanager | Microsoft Azure"
   description="Erstellen Sie ein Gateway mithilfe der Azure-CLI in Ressourcen-Manager"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-by-using-the-azure-cli"></a>Erstellen Sie ein Gateway mit der Azure-CLI

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-gateway-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-gateway-arm.md)
- [Klassische Azure PowerShell](application-gateway-create-gateway.md)
- [Azure-Ressourcen-Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway ist eine Layer 7-Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Application Gateway zeichnet Anwendung Lieferung: HTTP Netzwerklastenausgleich, cookiebasierte sitzungsaffinität und Secure Sockets Layer (SSL) Offload benutzerdefinierten Zustand Prüfpunkte laden und Unterstützung für mehrere Standorte.

## <a name="prerequisite-install-the-azure-cli"></a>Voraussetzung: Installieren der Azure-CLI

Um die Schritte in diesem Artikel ausführen, müssen Sie [Installieren der Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows Azure CLI ()](../xplat-cli-install.md) und [Azure](../xplat-cli-connect.md)anmelden müssen. 

> [AZURE.NOTE] Wenn Sie ein Azure-Konto haben, benötigen Sie einen. Gehen Sie Zeichen für eine [kostenlose Testversion hier](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Szenario

In diesem Szenario erfahren Sie, wie ein Gateway mit Azure-Portal erstellen.

Dieses Szenario wird:

- Erstellen Sie mittlere Application Gateway mit zwei Instanzen.
- Erstellen Sie ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET mit einem 10.0.0.0/16 reservierten CIDR-Block.
- Erstellen Sie ein Subnetz namens Appgatewaysubnet, die 10.0.0.0/28 als CIDR-Blocks verwendet.
- Konfigurieren eines Zertifikats für SSL-Verschiebung.

![Szenario-Beispiel][scenario]

>[AZURE.NOTE] Zusätzliche Konfiguration des Application Gateway einschließlich benutzerdefinierte Prüfpunkte, Back-End-Pool Adressen und zusätzliche Regeln nach Anwendungsgateway konfiguriert und nicht während der ursprünglichen Bereitstellung konfiguriert sind.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Azure Application Gateway benötigt ein eigenen Subnetz. Beim Erstellen eines virtuellen Netzwerks sicherzustellen Sie, dass genügend Adressraum mehrere Subnetze haben. Wenn Sie einen Gateway zu einem Subnet bereitstellen, können nur weitere Gateways im Subnetz hinzugefügt werden.

## <a name="log-in-to-azure"></a>Melden Sie sich bei Azure

Öffnen Sie **Microsoft Azure Command Prompt**und anmelden. 

    azure login

Nach der Eingabe im vorangehenden Beispiel erfolgt ein. Navigieren Sie zu https://aka.ms/devicelogin in einem Browser die Anmeldung fortgesetzt.

![CMD mit Gerät anmelden][1]

Geben Sie im Browser den Code erhalten. Sie werden auf eine Anmeldeseite umgeleitet.

![Browser eingeben][2]

Nach der Eingabe von Code Sie angemeldet sind, schließen Sie den Browser weiterhin auf das Szenario.

![erfolgreich angemeldet][3]

## <a name="switch-to-resource-manager-mode"></a>Wechseln Sie zum Ressourcen-Manager-Modus

    azure config mode arm

## <a name="create-the-resource-group"></a>Die Ressourcengruppe erstellen

Bevor das Application Gateway erstellen, wird eine Ressourcengruppe erstellt, Application Gateway enthalten. Im folgenden wird den Befehl.

    azure group create -n AdatumAppGatewayRG -l eastus

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

Nachdem die Ressourcengruppe erstellt wurde, wird ein virtuelles Netzwerk für Application Gateway erstellt.  Im folgenden Beispiel wurde der Adressraum als 10.0.0.0/16 im vorherigen Szenario Notizen.

    azure network vnet create -n AdatumAppGatewayVNET -a 10.0.0.0/16 -g AdatumAppGatewayRG -l eastus

## <a name="create-a-subnet"></a>Erstellen Sie ein Subnetz

Nachdem das virtuelle Netzwerk erstellt wurde, wird ein Subnetz für Application Gateway hinzugefügt.  Wenn Sie mit einem Web app im gleichen virtuellen Netzwerk als Anwendungsgateway gehostet Application Gateway verwenden möchten, müssen Sie ausreichend Platz für ein anderes Subnetz.

    azure network vnet subnet create -g AdatumAppGatewayRG -n Appgatewaysubnet -v AdatumAppGatewayVNET -a 10.0.0.0/28 

## <a name="create-the-application-gateway"></a>Das Application Gateway erstellen

Nachdem das virtuelle Netzwerk und Subnetz erstellt wurden, sind die erforderlichen Komponenten für das Application Gateway abgeschlossen. Außerdem ein zuvor exportierten PFX-Zertifikat und das Kennwort für das Zertifikat für den folgenden Schritt erforderlich sind: die IP-Adressen für die Back-End-IP-Adressen für den Back-End-Server sind. Diese Werte können private IP-Adressen im virtuellen Netzwerk, öffentliche IP-Adressen oder vollqualifizierte Domänennamen für Ihre Back-End-Server sein.

    azure network application-gateway create -n AdatumAppGateway -l eastus -g AdatumAppGatewayRG -e AdatumAppGatewayVNET -m Appgatewaysubnet -r 134.170.185.46,134.170.188.221,134.170.185.50 -y c:\AdatumAppGateway\adatumcert.pfx -x P@ssw0rd -z 2 -a Standard_Medium -w Basic -j 443 -f Enabled -o 80 -i http -b https -u Standard

> [AZURE.NOTE] Eine Liste von Parametern, die während der Erstellung der folgenden Befehl erhalten: **Azure Netzwerkgateway-Anwendung erstellen – Hilfe**.

Dieses Beispiel erstellt eine Basisanwendungsgruppe Gateway mit Standardeinstellungen für Listener, Pool-Back-End, Back-End-HTTP-Einstellungen und Regeln. Außerdem wird SSL-Verschiebung konfiguriert. Sie können diese Einstellungen die Bereitstellung anpassen, wenn die Bereitstellung erfolgreich war.
Wenn Sie bereits die Webanwendung definiert die Back-End-Pool in den vorhergehenden Schritten erstellt, Lastenausgleich beginnt.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie benutzerdefinierte Health Prüfpunkte erstellen [benutzerdefinierte Integritätstest erstellen](application-gateway-create-probe-portal.md)

Erfahren Sie mehr zu konfigurieren SSL-Abladung teure SSL-Entschlüsselung von Webserver auf [Konfigurieren SSL-Verschiebung](application-gateway-ssl-arm.md)

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png