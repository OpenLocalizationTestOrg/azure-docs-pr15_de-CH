## <a name="scenario"></a>Szenario

Dieses Dokument führt eine Bereitstellung, die mehrere Netzwerkkarten in VMs in einem bestimmten Szenario verwendet. In diesem Szenario müssen Sie eine zweistufige IaaS Arbeitslast in Azure gehostet. Jede Ebene wird in einem eigenen Teilnetz in einem virtuellen Netzwerk (VNet) bereitgestellt. Front-End-Ebene besteht aus mehreren Webservern gruppiert ein Lastenausgleich für hohe Verfügbarkeit festgelegt. Die Back-End-Ebene besteht aus mehreren Datenbankservern. Diese Datenbankserver werden mit zwei Netzwerkkarten, eine für den Datenbankzugriff für Management bereitgestellt. Das Szenario auch Netzwerk-Sicherheitsgruppen (NSGs) für jedes Subnetz zulässige Datenverkehr steuern und NIC in der Bereitstellung. Folgende Abbildung zeigt die grundlegende Architektur von diesem Szenario.  

![MultiNIC-Szenario](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

