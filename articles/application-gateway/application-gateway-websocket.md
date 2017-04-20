<properties
   pageTitle="Gateway WebSocket-Unterstützung | Microsoft Azure"
   description="Diese Seite enthält eine Übersicht über die Application Gateway WebSocket-Unterstützung."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Gateway WebSocket-Unterstützung

Application Gateway bietet systemeigene Unterstützung für WebSocket über alle Gateways. Es gibt keine konfigurierbare Einstellung selektiv aktivieren oder deaktivieren Sie die WebSocket-Unterstützung. Sie können weiterhin mit einer standardmäßigen HTTPListener auf port 80/443 WebSocket-Datenverkehr empfangen. WebSocket-Datenverkehr wird dann WebSocket-aktivierten Backend-Server mit den entsprechenden Back-End-Pool gemäß Anwendungsregeln Gateway weitergeleitet. Standardisierte [RFC6455](https://tools.ietf.org/html/rfc6455) WebSocket-Protokoll ermöglicht eine Vollduplex Kommunikation zwischen Server und Client über langer TCP-Verbindung. Dieses Feature ermöglicht eine interaktive Kommunikation zwischen Webserver und Client ohne Abfrage als HTTP-basierte Implementierung erforderlich sein kann.  WebSocket haben Gemeinkosten niedrig im Gegensatz zu HTTP und dieselbe TCP-Verbindung für mehrere Anforderung/Antworten, wodurch eine effizientere Nutzung der Ressourcen verwenden können. WebSocket-Protokolle sollen über herkömmliche HTTP-Ports 80 und 443 arbeiten.

Back-End-Server muss auf Application Gateway Prüfpunkte in [Health Prüfpunkt](application-gateway-probe-overview.md) Übersicht beschriebenen reagieren. Application Gateway Gesundheit Prüfpunkte werden nur HTTP/HTTPS, bedeutet dies, dass alle Back-End-Server für HTTP-Prüfpunkte für Application Gateway für den WebSocket-Datenverkehr auf dem Server reagieren muss.

## <a name="listener-configuration-element"></a>Listener-Konfigurationselement

Vorhandene HTTPListener kann zur WebSocket-Unterstützung verwendet werden. Folgt ein Ausschnitt aus einer HttpListeners Element eine Beispielvorlagendatei. Sie benötigen HTTP- und HTTPS-Listener zu WebSocket WebSocket-Datenverkehr sichern. Ebenso können Sie das [Portal](application-gateway-create-gateway-portal.md) oder [PowerShell](application-gateway-create-gateway-arm.md) ein Gateway mit Listener erstellen auf port 80/443 WebSocket-Datenverkehr unterstützt.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
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
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>Konfiguration der BackendAddressPool, BackendHttpSetting und Routing

Definieren Sie einen Back-End-Pool aktiviert WebSocket-Server sollte BackendAddressPool verwendet werden. BackendHttpSetting mit Back-End festzulegen Anschluss 80/443. Eigenschaften für Cookie-basierte Affinität und RequestTimeouts sind nicht für WebSocket-Datenverkehr. Es ist keine Änderung Routingregel erforderlich. Routingregel, 'Basic' weiterhin verwendet werden, um die entsprechenden Listener an den entsprechenden Back-End-Adresspool binden. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>Back-End-WebSocket-aktiviert

Backend muss eine HTTP/HTTPS-Webserver auf den konfigurierten port (in der Regel 80/443) für WebSocket arbeiten. Diese Anforderung ist da WebSocket-Protokoll den ursprünglichen Handshake WebSocket-Protokoll als ein Headerfeld HTTP Upgrade zu muss.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Grund dafür ist, dass Application Gateway Backend-Integritätstest HTTP/HTTPS-Protokolle unterstützt. Reagiert der Datenbankserver nicht auf HTTP/HTTPS Prüfpunkte, hätte aus Back-End-Pool und keine Anfragen WebSocket-Anfragen, dieses Backend zu erreichen.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie nach dem Kennenlernen WebSocket-Unterstützung Einstieg in eine WebSocket-fähigen Anwendung [ein Gateway erstellen](application-gateway-create-gateway.md) .
