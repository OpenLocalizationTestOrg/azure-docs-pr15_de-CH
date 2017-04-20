## <a name="scenario"></a>Szenario

Zur Veranschaulichung Decision erstellen, wird dieses Dokument folgenden Szenario verwenden.

![BESCHREIBUNG](./media/virtual-network-create-udr-scenario-include/figure1.png)

In diesem Szenario erstellen eine UDR für die *Front-End-Subnetz* und anderen UDR für *Back-End-Subnetz* Sie wie nachfolgend beschrieben: 

- **UDR-FrontEnd**. Front-End-UDR wird mit dem *Front-End* -Subnetz und eine Route enthalten:  
    - **RouteToBackend**. Diese Route wird alle Datenverkehr an das Back-End-Subnetz **FW1** virtuellen Computer senden.
- **UDR-BackEnd**. Back-End UDR wird mit dem *Back-End-* Subnetz und eine Route enthalten: 
    - **RouteToFrontend**. Diese Route wird alle Datenverkehr an das front-End-Subnetz **FW1** virtuellen Computer senden.

Die Kombination dieser Routen wird sichergestellt, dass alle Datenverkehr von einem Teilnetz zu einem anderen virtuellen Computer **FW1** weitergeleitet werden als virtuelle Appliance dient. Sie müssen für diesen virtuellen Computer, um sicherzustellen, dass Datenverkehr an andere VMs erhalten IP-Weiterleitung aktivieren.
