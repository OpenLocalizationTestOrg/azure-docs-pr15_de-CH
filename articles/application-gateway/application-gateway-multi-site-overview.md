<properties
   pageTitle="Verwaltung mehrerer Sites auf Application Gateway | Microsoft Azure"
   description="Diese Seite Überblick Application Gateway Multi-Site-Support."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-multiple-site-hosting"></a>Application Gateway mehrere Websitehost

Mehrere Website hosten können Sie mehrere Web-Anwendung auf demselben Application Gateway konfigurieren. Diese Funktion ermöglicht eine effizientere Topologie für die Bereitstellung konfigurieren, indem Sie eine Application-Gateway bis zu 20 Websites hinzufügen. Jede Website kann an Back-End-Pool weitergeleitet werden. Im folgenden Beispiel dient die Anwendungsgateway Datenverkehr für contoso.com und fabrikam.com aus zwei Back-End-Server namens ContosoServerPool und FabrikamServerPool.

![imageURLroute](./media/application-gateway-multi-site-overview/multisite.png)

Anfragen für http://contoso.com an ContosoServerPool weitergeleitet und http://fabrikam.com an FabrikamServerPool weitergeleitet.

Ebenso können zwei untergeordnete Domänen derselben übergeordneten Domäne in derselben Anwendung gatewaybereitstellung gehostet werden. Verwendung von untergeordneten Domänen kann http://blog.contoso.com und http://app.contoso.com auf einer einzigen Anwendung gatewaybereitstellung gehostet gehören.

## <a name="host-headers-and-server-name-indication-sni"></a>Hostheader und Server Namen Angabe (SNI)

Gibt es drei verbreitete Mechanismen zum Aktivieren mehrerer Websitehost auf dieselbe Infrastruktur.

1. Hosten Sie mehrere ASP.NET-Webanwendungen jeder eine eindeutige IP-Adresse.
2. Hostnamen verwenden die gleiche IP-Adresse mehrere ASP.NET-Webanwendungen hosten.
3. Verwenden Sie verschiedene Ports mehrere ASP.NET-Webanwendungen auf die IP-Adresse.

Ein Gateway wird derzeit eine einzelne öffentliche IP-Adresse, die es für Datenverkehr überwacht. Daher unterstützt mehrere Anträge, wird jeder mit seiner eigenen IP-Adresse derzeit nicht unterstützt. Application Gateway unterstützt hosten mehrere Anträge auf jede andere Ports überwacht dieses Szenario müsste die Anwendung auf nicht standardmäßigen Ports akzeptieren und wird häufig nicht gewünschten Application Gateway basiert auf HTTP 1.1 Hostheader hosten mehrere Websites auf dieselbe öffentliche IP-Adresse und Port. Die Websites Application Gateway können auch Unterstützung für SSL Offload Server Name Angabe (SNI) TLS-Erweiterung. Dabei bedeutet, dass die Client-Browser und Back-End-Webfarm HTTP/1.1 und TLS Erweiterung unterstützen muss, wie in RFC 6066 definiert.

## <a name="listener-configuration-element"></a>Listener-Konfigurationselement

Vorhandenen HTTPListener-Konfigurationselement ist verbessert und Host-Namen und Server Namen Anzeigeelemente, unterstützt von Application Gateway für den Datenverkehr an den entsprechenden Back-End-Pool wird. Im folgenden Codebeispiel ist der Ausschnitt HttpListeners Element Vorlagendatei.

    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener1",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                        "HostName": "contoso.com",
                        "RequireServerNameIndication": "true"
                    }
                },
                {
                    "name": "appGatewayHttpListener2",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                        "HostName": "fabrikam.com",
                        "RequireServerNameIndication": "false"
                    }
                }
            ],




Für eine Bereitstellung mit Ende Vorlage besuchen Sie [Ressourcenmanager Vorlage mehrere Site hostet](https://github.com/Azure/azure-quickstart-templates/blob/master/201-application-gateway-multihosting) .

## <a name="routing-rule"></a>Routingregel

Es ist keine Änderung der Routingregel erforderlich. Die Routingregel 'Basic' sollte weiterhin den entsprechenden Standort Listener an den entsprechenden Back-End-Adresspool binden ausgewählt werden.

    "requestRoutingRules": [
    {
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener1')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    },
    {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener2')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/FabrikamServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }
    ]

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie nach dem lernen über mehrere Site hostet, [erstellen ein Gateway mit mehreren Site hostet](application-gateway-create-multisite-azureresourcemanager-powershell.md) ein Gateway mit Unterstützung von mehreren Web-Anwendung erstellen.
