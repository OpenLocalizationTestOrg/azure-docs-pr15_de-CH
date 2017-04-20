<properties
   pageTitle="NSGs im klassischen Modus mithilfe der Azure-CLI erstellen | Microsoft Azure"
   description="Informationen Sie zum Erstellen und Bereitstellen von NSGs im klassischen Modus mithilfe der Azure-CLI"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>NSGs (klassisch) in der Azure-CLI erstellen

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Dieser Artikel behandelt das klassische Bereitstellungsmodell. Sie können auch [NSGs in der Ressourcen-Manager-Bereitstellungsmodell erstellen](virtual-networks-create-nsg-arm-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Azure-CLI Beispielbefehle unten erwarten eine einfache Umgebung bereits auf das Szenario. Wenn die Befehle ausführen, wie sie in diesem Dokument angezeigt werden soll, zunächst erstellen Sie die Umgebung durch [Erstellen einer VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>NSG für front-End-Subnetz erstellen
Gehen Sie folgendermaßen vor um eine NSG namens benannte **NSG FrontEnd** basierend auf dem Szenario zu erstellen.

1. Wenn Azure CLI noch nie verwendet haben, finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md) und gehen Sie bis zu dem Punkt, wo Sie Ihre Azure-Konto und Ihr Abonnement auswählen.

2. Führen Sie die **`azure config mode`** Befehl klassischen Modus wechseln, wie unten dargestellt.

        azure config mode asm

    Erwartete Ausgabe:

        info:    New mode is asm

3. Führen Sie die **`azure network nsg create`** Befehl eine NSG erstellen.

        azure network nsg create -l uswest -n NSG-FrontEnd

    Erwartete Ausgabe:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parameter:

    - **(oder -Speicherort)**. Azure-Region, in dem neue NSG erstellt werden. In diesem Szenario *Westus*.
    - **-n (bzw. -Namen)**. Name für neue NSG. In diesem Szenario *NSG FrontEnd*.

4. Führen Sie die **`azure network nsg rule create`** Befehl eine Regel erstellen, die Zugriff auf Port 3389 (RDP) über das Internet ermöglicht.

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Erwartete Ausgabe:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Parameter:

    - **-a (oder Nsg-Namen)**. Name des NSG in dem die Regel erstellt wird. In diesem Szenario *NSG FrontEnd*.
    - **-n (bzw. -Namen)**. Name für die neue Regel. In diesem Szenario *Rdp-Regel*.
    - **-c (oder -Aktion)**. Zugriffsebene für die Regel (verweigern oder zulassen).
    - **-p (oder -Protokoll)**. Protokoll (Tcp, Udp oder *) für die Regel.
    - **-r (oder -Typ)**. Richtung der Verbindung (eingehend oder ausgehend).
    - **-y (oder -Priorität)**. Priorität für die Regel.
    - **-f (oder -Quelle Adresspräfix)**. Quelle-Adresspräfix CIDR oder Standard-Tags.
    - **-o (oder -Anschluss Quellbereich)**. Ursprungs-Port oder Portbereich.
    - **-e (oder -Ziel Adresspräfix)**. Ziel-Adresspräfix CIDR oder Standard-Tags.
    - **-u (oder -Anschluss Zielbereich)**. Ziel-Port oder Portbereich.

5. Führen Sie die **`azure network nsg rule create`** Befehl eine Regel erstellen, die Zugriff auf Port 80 (HTTP) über das Internet ermöglicht.

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Erwartete Putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Führen Sie die **`azure network nsg subnet add`** Befehl die NSG front-End-Subnetz verknüpfen.

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    Erwartete Ausgabe:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>NSG für Back-End-Subnetz erstellen
Gehen Sie folgendermaßen vor um eine NSG namens benannte *NSG-BackEnd* basierend auf dem Szenario zu erstellen.

3. Führen Sie die **`azure network nsg create`** Befehl eine NSG erstellen.

        azure network nsg create -l uswest -n NSG-BackEnd

    Erwartete Ausgabe:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parameter:

    - **(oder -Speicherort)**. Azure-Region, in dem neue NSG erstellt werden. In diesem Szenario *Westus*.
    - **-n (bzw. -Namen)**. Name für neue NSG. In diesem Szenario *NSG FrontEnd*.

4. Führen Sie die **`azure network nsg rule create`** Befehl, um eine Regel erstellen, die Zugriff vom front-End-Subnetz Port 1433 (SQL) ermöglicht.

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Erwartete Ausgabe:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. Führen Sie die **`azure network nsg rule create`** Befehl eine Regel erstellen, die Zugriff auf das Internet verweigert.

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    Erwartete Putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Führen Sie die **`azure network nsg subnet add`** Befehl die NSG Back-End-Subnetz verknüpfen.

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    Erwartete Ausgabe:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
