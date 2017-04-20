<properties
   pageTitle="Application Gateway Bad Gateway (502) zur Fehlerbehebung | Microsoft Azure"
   description="Informationen Sie zur Problembehandlung beim Application Gateway 502"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Fehlerbehebung in Application Gateway fehlerhaftes gateway

## <a name="overview"></a>Übersicht

Nach der Konfiguration ein Gateway Azure, treten möglicherweise, Fehler gehört "Server-Fehler: 502 - Webserver erhielt eine ungültige Antwort als Gateway oder Proxy Server". Dies kann durch die folgenden wichtigsten Gründe auftreten:

- Azure Application Gateway Back-End-Pool ist nicht konfiguriert oder leer.
- VMs oder Instanzen in VM festzulegen sind.
- Backend-VMs oder Instanzen VM festlegen reagieren auf Standard-Integritätstest nicht.
- Ungültige oder fehlerhafte Konfiguration der benutzerdefinierten Gesundheit untersucht.
- Zeit oder Verbindungsprobleme mit Benutzeranfragen anfordern.

## <a name="empty-backendaddresspool"></a>Leere BackendAddressPool

### <a name="cause"></a>Ursache

Wenn Application Gateway keine VMs oder VM Skalierung festlegen im Back-End-Adresspool konfiguriert wurde, kann keinen Kundenwunsch route und löst einen fehlerhaftes Gateway-Fehler.

### <a name="solution"></a>Lösung

Sicherstellen Sie, dass der Back-End-Adresspool nicht leer ist. Dies kann entweder über PowerShell, CLI oder Portal erfolgen.

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

Die Ausgabe der vorherigen Cmdlet sollte nicht leere Backend-Adresspool. Nachfolgend ist ein Beispiel, in dem zwei Pools, kehren mit Backend-VMs FQDN- oder IP-Adressen konfiguriert sind. Bereitstellungsstatus der BackendAddressPool muss 'erfolgreich sein".
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>Fehlerhafte Instanzen BackendAddressPool

### <a name="cause"></a>Ursache

Wenn alle Instanzen des BackendAddressPool fehlerhaft sind, müssen Application Gateway keine Backend auf Anforderung an Benutzer weiterleiten. Dies kann auch der Fall sein, wenn Backend-Instanzen sind jedoch nicht die erforderliche Anwendung bereitgestellt haben.

### <a name="solution"></a>Lösung

Damit fehlerfrei sind und die Anwendung ordnungsgemäß konfiguriert. Prüfen Sie die Backend-Instanzen einer anderen VM in demselben VNet auf einen Ping reagieren. Mit einem öffentlichen Endpunkt konfiguriert, sicher, dass eine Browseranforderung an die Anwendung gewartet wird.

## <a name="problems-with-default-health-probe"></a>Probleme mit Standard-Integritätstest

### <a name="cause"></a>Ursache

Fehler 502 kann auch häufig Indikatoren ist standardmäßig Integritätstest nicht zu Backend-VMs. Beim Application Gateway Instanz bereitgestellt wird, wird automatisch einen Prüfpunkt Standard Zustand jedes BackendAddressPool Eigenschaften von der BackendHttpSetting konfiguriert. Dieser Prüfpunkt festgelegt ist keine Benutzereingabe erforderlich. Wenn eine Regel für Lastenausgleich konfiguriert ist, eine Zuordnung zwischen einem BackendHttpSetting und einem BackendAddressPool erfolgt. Standard-Prüfpunkt für jede dieser Verbände konfiguriert und Application Gateway initiiert eine regelmäßige Health Check Verbindung mit jeweils BackendAddressPool am Anschluss im BackendHttpSetting-Element angegeben. Folgende Tabelle enthält die Werte der Integritätstest Standard.


|Prüfpunkt-Eigenschaft | Wert | Beschreibung|
|---|---|---|
| Prüfpunkt-URL| http://127.0.0.1/ | URL-Pfad |
| Intervall | 30 | Prüfpunkt-Intervall in Sekunden |
| Zeitlimit  | 30 | Prüfpunkt Timeout in Sekunden |
| Fehlerhafte Schwellenwert | 3 | Prüfpunkt Wiederholungsanzahl. Back-End-Server ist unten markiert, aufeinander folgende Prüfpunkt Fehlerzähler fehlerhaften Schwellenwert erreicht. |

### <a name="solution"></a>Lösung

- Sicherstellen Sie, dass die Standardwebsite ist konfiguriert und hört 127.0.0.1.
- Wenn BackendHttpSetting einen anderen Port als 80 angegeben ist, sollte die Standardwebsite auf diesem Port überwachen konfiguriert werden.
- Der Aufruf von Http://127.0.0.1:port sollte einen Ergebnis HTTP-Code 200 zurück. Dies sollte dem Timeout von 30 Sekunden zurückgegeben werden.
- Sicherstellen Sie, dass Anschluss konfiguriert ist und es gibt keine Firewall-Regeln oder Azure Netzwerksicherheitsgruppen blockieren eingehenden oder ausgehenden Datenverkehr auf Port konfiguriert.
- Sicherstellen Sie Azure klassische VMs oder Cloud-Dienst öffentliche IP-Adresse und FQDN verwendet wird, dass die entsprechenden [Endpunkt](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md) geöffnet wird.
- Wenn die VM über Azure-Ressourcen-Manager konfiguriert ist und außerhalb der VNet, wo Application Gateway bereitgestellt wird, müssen [Netzwerk-Sicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) Zugriff auf den gewünschten Port konfiguriert werden.


## <a name="problems-with-custom-health-probe"></a>Probleme mit benutzerdefinierten Zustand

### <a name="cause"></a>Ursache

Benutzerdefinierte Health Prüfpunkte ermöglichen zusätzliche Flexibilität bei der Überprüfung Verhalten Standard. Bei benutzerdefinierten Prüfpunkte können Benutzer Intervall Prüfpunkt, die URL und Pfad zu testen und wie viele Fehlgeschlagene Antworten an die Back-End-Pool-Instanz als fehlerhaft markiert. Die folgenden Eigenschaften hinzugefügt werden.

|Prüfpunkt-Eigenschaft| Beschreibung|
|---|---|
| Name | Name des Prüfpunkts. Dieser Name dient der Prüfpunkt im Back-End HTTP-Einstellungen auf. |
| Protokoll | Protokoll den Prüfpunkt senden. Der Prüfpunkt wird im Back-End HTTP-Einstellungen definiert Protokoll verwenden. |
| Host |  Host-Name des Prüfpunkts senden. Nur anwendbar, wenn mehrere Standorte auf Application Gateway konfiguriert ist. Dies unterscheidet sich von VM-Hostnamen.  |
| Pfad | Relativer Pfad des Prüfpunkts. Der gültige Pfad beginnt mit '/'. Der Prüfpunkt wird gesendet, um \<Protokoll\>://\<Server\>:\<Port\>\<Pfad\> |
| Intervall | Prüfpunkt Intervall in Sekunden an. Dies ist das Zeitintervall zwischen zwei aufeinander folgende Stichproben.|
| Zeitlimit | Prüfpunkt Timeout in Sekunden an. Der Prüfpunkt wird als fehlgeschlagen, wenn eine gültige Antwort nicht innerhalb dieses Timeouts eingeht gekennzeichnet. |
| Fehlerhafte Schwellenwert | Prüfpunkt Wiederholungsanzahl. Back-End-Server ist unten markiert, aufeinander folgende Prüfpunkt Fehlerzähler fehlerhaften Schwellenwert erreicht. |


### <a name="solution"></a>Lösung

Überprüfen Sie, ob benutzerdefinierte Health Prüfpunkt wie in der vorhergehenden Tabelle konfiguriert ist. Neben der vorherigen Schritte zur Problembehandlung auch folgende Punkte sicher:

- Sicherstellen Sie, dass der Prüfpunkt gemäß [Handbuch](application-gateway-create-probe-ps.md)richtig angegeben ist.
- Wenn Application Gateway für eine einzelne Site konfiguriert ist, sollte standardmäßig Host Name als "127.0.0.1" angegeben werden, wenn andernfalls benutzerdefinierte Probe konfiguriert.
- Stellen Sie sicher, dass einen Aufruf von http://\<Server\>:\<Port\>\<Pfad\> gibt einen Ergebniscode HTTP 200.
- Sicherstellen Sie, dass Intervall, Timeout und UnhealtyThreshold innerhalb der zulässigen Bereiche.

## <a name="request-time-out"></a>Timeout der Anforderung

### <a name="cause"></a>Ursache

Beim Empfang ein Application Gateway gilt konfigurierten Regeln für die Anforderung und leitet sie an Back-End-Pool-Instanz. Eine konfigurierbare Zeitspanne eine Antwort vom Backend-Instanz wartet. Standardmäßig ist dieses Intervall **30 Sekunden**. Wenn Application Gateway von Back-End-Anwendung in diesem Intervall keine Antwort erhalten, werden Benutzer Fehler 502 angezeigt.

### <a name="solution"></a>Lösung

Application Gateway kann Benutzer diese Einstellung über BackendHttpSetting, die in verschiedenen Pools angewendet werden können. Andere Backend-Pools können verschiedene BackendHttpSetting und damit andere Anforderung wird konfiguriert.

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>Nächste Schritte

Wenn die vorhergehenden Schritte das Problem nicht beheben, öffnen Sie ein [support-Ticket](https://azure.microsoft.com/support/options/).
