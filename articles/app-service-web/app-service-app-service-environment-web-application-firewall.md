<properties 
    pageTitle="Konfigurieren von Web Application Firewall (AAF) für App Service-Umgebung" 
    description="Informationen Sie zum Konfigurieren einer Web Application Firewall vor Ihrer App Service-Umgebung." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfigurieren von Web Application Firewall (AAF) für App Service-Umgebung

## <a name="overview"></a>Übersicht ##
Web Application Firewalls wie [Barracuda WAF für Azure](https://www.barracuda.com/programs/azure) , die auf den [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) schützt Ihre ASP.NET-Webanwendungen anhand Webverkehr SQL-Injektionen, Cross-Site Scripting Malware Uploads & Anwendung DDoS und andere Angriffe blockieren eingehender. Es prüft auch die Antworten von den Back-End-Webservern für Data Loss Prevention (DLP). Zusammen mit der Isolierung und weitere Skalierung von App Service-Umgebungen bereitgestellt, bietet dies eine ideale Umgebung zum Host Business wichtige Web Applications, die böswillige Anfragen und hohe Volumen.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Setup ##
Für dieses Dokument, das wir konfigurieren ausgeglichen unsere App Service-Umgebung hinter mehrere Last Instanzen von Barracuda WAF nur Datenverkehr von der WAF kann die App Service-Umgebung und kann nicht aus der DMZ zugegriffen werden. Wir haben auch Azure Traffic Manager vor unserer Barracuda WAF Instanzen für den Lastenausgleich in Azure Rechenzentren und Regionen. Eine hohe Diagramm der Installation sieht wie unten angezeigt.

![Architektur][Architecture] 

> Hinweis: Mit der Einführung von [ILB Unterstützung für App Service-Umgebung](app-service-environment-with-internal-load-balancer.md)können Sie ASE aus der DMZ zugegriffen werden und nur für das private Netzwerk konfigurieren. 

## <a name="configuring-your-app-service-environment"></a>Konfigurieren Ihre App Service-Umgebung ##
Konfigurieren einer App Service-Umgebung finden Sie in der [Dokumentation](app-service-web-how-to-create-an-app-service-environment.md) zum Thema. Haben Sie eine App Service-Umgebung erstellt, können Sie [Web-Apps](app-service-web-overview.md), [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) und [Mobile Apps](../app-service-mobile/app-service-mobile-value-prop.md) in dieser Umgebung erstellen, die alle hinter der WAF, die wir im nächsten Abschnitt Konfigurieren geschützt.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfigurieren des Barracuda WAF Cloud-Dienstes ##
Barracuda hat einen [ausführlichen Artikel](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) zur Bereitstellung der WAF auf einem virtuellen Computer in Azure. Aber da wir Redundanz möchten und keine einzelne Fehlerquelle, mindestens 2 WAF Instanz VMs in derselben Cloud-Dienst bereitstellen, wenn diese Anweisungen möchten.

### <a name="adding-endpoints-to-cloud-service"></a>Endpunkte Cloud-Dienst hinzufügen ###
Haben Sie 2 oder mehr WAF VM-Instanzen in Ihrem Clouddienst können [Azure-Portal](https://portal.azure.com/) Sie HTTP und HTTPS Endpunkte hinzufügen, die von der Anwendung verwendet werden, wie in der folgenden Abbildung dargestellt.

![Endpunkt konfigurieren][ConfigureEndpoint]

Wenn die Anwendung andere Endpunkte verwenden, müssen Sie zu dieser Liste hinzufügen. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfigurieren von Barracuda WAF über die Management-Portal ###
WAF Barracuda verwendet TCP-Port 8000 Konfiguration durch das Management-Portal. Wir haben mehrere Instanzen der WAF VMs müssen Sie wiederholen die Schritte für jede VM-Instanz. 


> Hinweis: Nach WAF Konfiguration abgeschlossen haben, entfernen Sie TCP-8000-Endpunkt der WAF VMs, um Ihre WAF zu schützen.

Fügen Sie Management-Endpunkt hinzu, wie unten gezeigt die Barracuda WAF konfigurieren.

![Management-Endpunkt hinzufügen][AddManagementEndpoint]
 
Verwenden Sie einen Browser, um Management-Endpunkt für den Cloud-Dienst zu suchen. Wenn Ihr Cloud-Dienst test.cloudapp.net aufgerufen wird, würde Sie diesen Endpunkt auf Http://test.cloudapp.net:8000 zugreifen. Sollten Sie eine Anmeldeseite sehen wie unten, Anmeldung, in der WAF VM-Installationsphase angegebenen, Anmeldeinformationen verwenden.

![Management-Anmeldeseite][ManagementLoginPage]

Einmal einloggen Dashboard wie in der Abbildung unten sehen Sie, die grundlegende statistische Angaben zu den WAF Schutz.

![Management-Dashboard][ManagementDashboard]

Klicken auf der Registerkarte Dienste können Sie die WAF für Dienste konfigurieren schützen. Weitere Informationen zum Konfigurieren der Barracuda WAF finden Sie in [der Dokumentation](https://techlib.barracuda.com/waf/getstarted1). Im Beispiel unten ein Azure Web App wurde mit Datenverkehr über HTTP und HTTPS konfiguriert.

![Management Services hinzufügen][ManagementAddServices]

> Hinweis: Je nach Konfiguration Ihrer Anwendung und welche Funktionen in Ihrer App Service-Umgebung verwendet werden, Sie müssen für TCP Ports 80 und 443, z. B. weiterleiten, wenn Sie SSL IP Setup für eine Web-Anwendung. Eine Liste der Umgebungen App verwendete Netzwerkports finden Sie unter [Kontrolle eingehender Datenverkehr des](app-service-app-service-environment-control-inbound-traffic.md) Netzwerkports Dokumentation.

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurieren von Microsoft Azure Traffic Manager (OPTIONAL) ##
Ist die Anwendung in mehreren Regionen laden möchten Gleichgewicht hinter [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Fügen Sie dazu einen Endpunkt im [klassischen Azure-Portal](https://manage.azure.com) mithilfe des Cloud-Dienst die WAF im Profil Traffic Manager wie in der folgenden Abbildung dargestellt. 

![Traffic Manager-Endpunkt][TrafficManagerEndpoint]

Wenn die Anwendung Authentifizierung erfordert, sicherzustellen Sie, dass Sie eine Ressource, die Authentifizierung für Traffic Manager ping für Ihre Anwendung erfordert. Wie unten dargestellt, können Sie die URL im Abschnitt Konfigurieren im [klassischen Azure-Portal](https://manage.azure.com) konfigurieren.

![Traffic Manager konfigurieren][ConfigureTrafficManager]

Zum Weiterleiten von Pings Traffic Manager von der WAF Ihrer Anwendung müssen Sie Setup Website-Übersetzung auf Ihre Barracuda WAF weiterleiten, der Anwendung, wie im folgenden Beispiel gezeigt.

![Website-Übersetzung][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Sichern des Datenverkehrs App Service-Umgebung mithilfe von Netzwerk-Sicherheitsgruppen (NSG)##
Führen Sie das [Steuerelement eingehender Datenverkehr Dokumentation](app-service-app-service-environment-control-inbound-traffic.md) Einschränkung des Datenverkehrs an Ihrer App Service-Umgebung aus der WAF nur in die VIP-Adresse des Cloud-Diensts. Hier ist ein Beispiel-Powershell-Befehl zur Durchführung dieser Aufgabe für TCP-Port 80.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Ersetzen Sie die SourceAddressPrefix mit der virtuellen IP-Adresse (VIP) Ihre WAF Cloud-Dienst.

> Hinweis: Die VIP des Cloud-Diensts ändert beim Löschen und neu Cloud-Dienst erstellen. Vergewissern Sie sich die IP-Adresse im Netzwerk Ressourcengruppe danach aktualisieren. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
