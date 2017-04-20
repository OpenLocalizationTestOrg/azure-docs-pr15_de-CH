## <a name="service-chaining---transit-through-peered-vnet"></a>Service Chaining - Transit hervorragendem VNet

Obwohl die Verwendung von systemrouten Datenverkehr automatisch für die Bereitstellung vereinfacht, sind in dem Sie das routing von Paketen über ein virtuelles Gerät steuern möchten.
In diesem Szenario gibt es zwei VNets in einem Abonnement, HubVNet und VNet1 wie unten beschrieben. Virtuelle Appliance(NVA) im VNet HubVNet Netzwerk bereitstellen. Nach dem Einrichten des peering zwischen HubVNet und VNet1 VNet, können Sie Benutzer definierten Routen festlegen und des nächsten Hop zum NVA in der HubVNet.

![NVA Übertragung](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [AZURE.NOTE] Der Einfachheit halber Angenommen Sie werden hier alle VNets im gleichen Abonnement. Aber auch für Cross-Abonnement Szenario funktioniert.

Die Schlüsseleigenschaft Übertragung routing aktivieren ist der Parameter "Weitergeleitet Datenverkehr zulassen". Dadurch annehmen und Senden von Datenverkehr vom und zum NVA in hervorragendem VNet.  
