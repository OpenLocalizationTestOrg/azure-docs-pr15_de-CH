## <a name="virtual-network"></a>Virtuelles Netzwerk
Virtuelle Netzwerke (VNET) und Subnetze Ressourcen definieren eine Sicherheitsgrenze für Arbeitslasten in Azure ausgeführt. Ein VNet kennzeichnet eine Auflistung von Adressräumen als CIDR-Blocks. 

>[AZURE.NOTE] Administratoren sind vertraut mit CIDR-Notation. Wenn Sie nicht mit CIDR [erfahren Sie mehr über](http://whatismyipaddress.com/cidr)vertraut sind.

![VNet mit mehreren Subnetzen](./media/resource-groups-networking/Figure4.png)

VNets enthalten die folgenden Eigenschaften.

|Eigenschaft|Beschreibung|Beispielwerte|
|---|---|---|
|**addressSpace**|Auflistung von VNet in CIDR-Notation bilden Adresspräfixe|192.168.0.0/16|
|**Subnetze**|Auflistung der Subnetze, die das VNet bilden|[Subnetze](#Subnets) unten anzeigen|
|**IP-Adresse**|Dem Objekt zugewiesene IP-Adresse. Dies ist eine schreibgeschützte Eigenschaft.|104.42.233.77|

### <a name="subnets"></a>Subnetze
Ein Subnetz ist ein VNet untergeordnete Ressource und hilft Segmente Adressräume innerhalb eines CIDR-Blocks mit IP-Adresspräfixe definieren. NICs können Subnetze hinzugefügt und VMs bietet Konnektivität für unterschiedliche Workloads verbunden.

Subnetze enthält die folgenden Eigenschaften. 

|Eigenschaft|Beschreibung|Beispielwerte|
|---|---|---|
|**addressPrefix**|Einzelnes Adresspräfix bilden das Subnetz in CIDR-notation|192.168.1.0/24|
|**networkSecurityGroup**|NSG im Subnetz zugewiesen|Siehe [NSGs](#Network-Security-Group)|
|**routeTable**|Route-Tabelle auf dem Subnetz angewendet|Siehe [UDR](#Route-table)|
|**ipConfigurations**|Auflistung der IP bei Objekte Netzwerkkarten mit dem Subnetz verbunden|Siehe [UDR](#Route-table)|


Beispiel-VNet im JSON-Format:

    {
        "name": "TestVNet",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet",
        "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
        "type": "Microsoft.Network/virtualNetworks",
        "location": "westus",
        "tags": {
            "displayName": "VNet"
        },
        "properties": {
            "provisioningState": "Succeeded",
            "resourceGuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            "addressSpace": {
                "addressPrefixes": [
                    "192.168.0.0/16"
                ]
            },
            "subnets": [
                {
                    "name": "FrontEnd",
                    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                    "properties": {
                        "provisioningState": "Succeeded",
                        "addressPrefix": "192.168.1.0/24",
                        "networkSecurityGroup": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "routeTable": {
                            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                        },
                        "ipConfigurations": [
                            {
                                "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                            },
                            ...]
                    }
                },
                ...]
        }
    }

### <a name="additional-resources"></a>Zusätzliche Ressourcen

- Weitere Informationen zu [VNet](../articles/virtual-network/virtual-networks-overview.md)zu erhalten.
- Lesen Sie die [REST API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt163650.aspx) für VNets.
- Lesen Sie die [REST API-Referenzdokumentation](https://msdn.microsoft.com/library/azure/mt163618.aspx) für Subnetze.