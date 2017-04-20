## <a name="traffic-manager-profile"></a>Traffic Manager-Profil

Traffic Manager und dessen Endpunkt untergeordnete Ressource aktivieren DNS-Endpunkte in Azure und Azure routing. Verteilermethoden Richtlinie datenverkehrsverteilung unterliegen. Traffic Manager ermöglicht außerdem Endpunkt Zustand überwacht werden und entsprechend umgeleitet Verkehr basierend auf dem Zustand des Endpunkts. 

| Eigenschaft | Beschreibung |
|---|---|
|**trafficRoutingMethod**| Mögliche Werte sind *, *gewichtet*und *Priorität* * | 
| **dnsConfig** | FQDN für das Profil | 
| **Protokoll** | Protokoll überwacht, sind mögliche Werte *HTTP* und *HTTPS*|
| **Anschluss** | Überwachen von port |  
| **Pfad** | Pfad überwachen |
| **Endpunkte** |  Container für Ressourcen | 

### <a name="endpoint"></a>Endpunkt 

Ein Endpunkt ist eine untergeordnete Ressource Profil Traffic Manager. Stellt einen Dienst oder Webdienst-Endpunkt an die Benutzer Datenverkehr verteilt wird anhand der konfigurierten Richtlinie in der Traffic Manager-Profil. 

| Eigenschaft | Beschreibung | 
|---|---| 
| **Typ** |  Der Typ des Endpunkts, mögliche Werte sind *Azure Ende*, *Externe Endpunkt*und *Endpunkt geschachtelt* | 
| **targetResourceId** |  öffentliche IP-Adresse eines Endpunkts Service oder Web. Dies ist ein Azure oder externe Endpunkt. | 
| **Gewicht** | Endpunkt Gewicht Verkehrsmanagement. | 
| **Priorität** | Priorität des Endpunkts definiert eine Failover-Aktion |

Beispiel der Traffic Manager im Json-Format: 


        {
            "apiVersion": "[variables('tmApiVersion')]",
            "type": "Microsoft.Network/trafficManagerProfiles",
            "name": "VMEndpointExample",
            "location": "global",
            "dependsOn": [
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '0')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '1')]",
                "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'), '2')]",
            ],
            "properties": {
                "profileStatus": "Enabled",
                "trafficRoutingMethod": "Weighted",
                "dnsConfig": {
                    "relativeName": "[parameters('dnsname')]",
                    "ttl": 30
                },
                "monitorConfig": {
                    "protocol": "http",
                    "port": 80,
                    "path": "/"
                },
                "endpoints": [
                    {
                        "name": "endpoint0",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 0))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint1",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 1))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    },
                    {
                        "name": "endpoint2",
                        "type": "Microsoft.Network/trafficManagerProfiles/azureEndpoints",
                        "properties": {
                            "targetResourceId": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('publicIPAddressName'), 2))]",
                            "endpointStatus": "Enabled",
                            "weight": 1
                        }
                    }
                ]
            }
        }

 
## <a name="additional-resources"></a>Zusätzliche Ressourcen

Weitere Informationen finden Sie bei [REST API-Dokumentation für Traffic Manager](https://msdn.microsoft.com/library/azure/mt163664.aspx) .
