<properties
    pageTitle="Erstellen und Verwenden einer internen Lastenausgleich mit einer App Service-Umgebung | Microsoft Azure"
    description="Erstellen und Verwenden einer ASE mit einer ILB"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Verwenden einer internen Lastenausgleich mit einer App Service-Umgebung #

App Service Environments(ASE) Feature ist Option Premium Service von Azure App Service, erweiterte Konfigurationsfunktionen liefert, die in der Multi-Tenant-Stempel nicht verfügbar ist.  ASE-Funktion stellt im Wesentlichen Azure App Service in Ihrem virtuellen Azure-Network(VNet).  Ein besseres Verständnis der Funktionen App Umgebungen lesen [einer App Service-Umgebung] zu[ WhatisASE] Dokumentation.  Sie kennen die Vorteile des Betriebs in einem VNet lesen [Azure Virtual Network FAQ][virtualnetwork].  


## <a name="overview"></a>Übersicht ##


Eine ASE kann mit einem Internet zugänglich oder IP-Adresse in Ihrem VNet bereitgestellt werden.  Um eine VNet-Adresse die IP-Adresse festzulegen, müssen Sie Ihre ASE mit einer internen laden Balancer(ILB) bereitstellen.  Die ASE mit einem ILB konfiguriert bieten Ihnen:

- eigene Domäne oder Unterdomäne.  Um zu erleichtern, übernimmt dieses untergeordnete Domäne, aber Sie können sie in beiden Fällen.  
- das Zertifikat für HTTPS
- DNS-Management für Ihre Domäne.  


Im Gegenzug können Sie Folgendes:

- Host-Intranetanwendungen wie Business Applications in der Wolke, die über eine Website oder ExpressRoute VPN zugreifen
- Host-apps in der Cloud, die nicht im öffentlichen DNS-Servern
- Erstellen Sie front-End-apps sicher integrieren können Internet isoliert Backend-apps


#### <a name="disabled-functionality"></a>Deaktivierte Funktionen ####

Es gibt einige Dinge, die Ihnen die Verwendung einer ILB ASE.  Diese gehören:

- IPSSL verwenden
- Zuweisen von IP-Adressen bestimmte Apps
- Kauf und ein Zertifikat mit einer Anwendung über das Portal.  Sie können natürlich Zertifikate direkt mit einer Zertifizierungsstelle erhalten und Ihre Apps nicht über Azure-Portal verwenden.


## <a name="creating-an-ilb-ase"></a>Erstellen einer ILB ASE ##

Erstellen einer ILB ASE unterscheidet sich nicht wesentlich normalerweise eine ASE erstellen.  Finden Sie mehr Informationen über das Erstellen einer ASE [zum Erstellen einer App Service-Umgebung][HowtoCreateASE].  Der Prozess zum Erstellen einer ILB ASE ist ein VNet während der Erstellung ASE erstellen oder Auswählen einer vorhandenen VNet identisch.  So erstellen Sie eine ILB ASE 

1.  Wählen Sie im Azure-Portal **Neu -> Web + Mobile-App Service-Umgebung >**
2.  Wählen Sie Ihr Abonnement
3.  Wählen Sie oder erstellen Sie eine Ressourcengruppe
4.  Wählen Sie oder erstellen Sie ein VNet
5.  Erstellen Sie ein Subnetz ein, wenn ein VNet
6.  Wählen Sie **virtuellen Netzwerkadresse-> VNet Konfiguration** und festlegen VIP Internal
7.  Namen Sie Subdomäne (dies die Subdomäne erstellt diese ASE Apps verwendet werden)
8.  Wählen Sie Ok und erstellen


![][1]


In virtuelles Netzwerk Blade ist eine VNet Konfigurationsoption.  Zwischen der externen VIP oder interne VIP wählen können.  Der Standardwert ist extern.  Haben Sie auf externe verwendet die ASE einen VIP Internet zugegriffen werden kann.  Wählt intern wird die ASE mit ILB eine IP-Adresse in Ihrem VNet konfiguriert.  


Nach Auswahl intern mehr IP-Adressen der ASE hinzufügen entfernt und stattdessen müssen Sie die Domäne der ASE bereitstellen.  In einer ASE mit externen VIP Namen ASE in der Subdomäne apps erstellt, ASE dient.  
Wenn Ihre ASE ***Contosotest*** und Ihre app war, ASE aufgerufen wurde ***Mytest*** dann die Subdomäne Format ***contosotest.p.azurewebsites.net*** sein und die URL für die Anwendung wäre ***mytest.contosotest.p.azurewebsites.net***.  
Wenn Sie interne VIP-Typ festlegen, wird Ihr ASE-Name nicht in der Subdomäne ASE verwendet.  Die untergeordnete Domäne angeben explizit.  Wenn Ihre Subdomain ***contoso.corp.net*** und Sie eine app, ASE mit dem Namen ***Timereporting*** wäre die URL für die Anwendung ***timereporting.contoso.corp.net***.


## <a name="apps-in-an-ilb-ase"></a>Apps in einer ILB ASE ##

Erstellen einer Anwendung in einer ILB ASE entspricht normalerweise eine Anwendung in einer ASE erstellen.  

1. Wählen Sie in Azure-Portal **Neu -> Web + Mobile-Web >** **Mobile** oder **API-App**
2. Name der Anwendung
2. Abonnement auswählen
3. Wählen Sie oder erstellen Sie die Ressourcengruppe
4. Wählen Sie oder erstellen Sie App Service Plan(ASP).  Erstellen einer neuen ASP dann wählen Sie Ihre ASE als Speicherort und dem Arbeitspool sollten Sie ASP erstellt werden.  Wenn Sie ASP wählen Sie Ihre ASE der und dem Arbeitspool.  Wenn Sie den Namen der Anwendung angeben, dass die Domäne unter Namen der untergeordneten Domäne für Ihre ASE Fassung sehen.   
5. Wählen Sie erstellen.  Wählen Sie ggf. die Anwendung auf dem Dashboard das Kontrollkästchen **Pin Dashboard** .  

![][2]


Unter dem Namen app ruft der Subdomainnamen Unterdomäne der ASE entsprechend aktualisiert.  


## <a name="post-ilb-ase-creation-validation"></a>ILB ASE Erstellung Validierung buchen ##

Ein ILB ASE ist etwas anders als nicht - ILB ASE.  Wie bereits erwähnt, müssen Sie ein eigenes DNS verwalten und haben auch Ihr eigenes Zertifikat für HTTPS-Verbindung bereitstellen.  


Nach dem Erstellen der ASE sehen Sie, dass die Domäne der Domäne zeigt angegebene und ein neues Element in Menü **Einstellung** **ILB Zertifikat**bezeichnet.  ASE ist ein selbstsigniertes Zertifikat erstellt HTTPS Testen erleichtert.  Das Portal erfahren Sie Ihr eigenes Zertifikat für HTTPS bereitstellen müssen, aber dies soll Ihnen ein Zertifikat, das mit der Domäne.  

![][3]


Wenn Sie einfach ausprobieren sind und nicht wissen, wie Sie ein Zertifikat erstellen, können die IIS-MMC-Konsole-Anwendung Sie ein selbstsigniertes Zertifikat erstellen.  Erstellt werden als PFX-Datei exportieren und dann in ILB Zertifikat Benutzeroberfläche hochladen. Beim Zugriff auf eine Website mit einem selbstsignierten Zertifikat gesichert erhalten Browser eine Warnung Sie die Website, zugreifen, Sicherheit durch die Unfähigkeit, das Zertifikat zu überprüfen.  Um diese Warnung zu vermeiden sollten Sie ein ordnungsgemäß signiertes Zertifikat, das Ihre Subdomain entspricht und hat eine Kette von Vertrauensstellungen, die vom Browser erkannt wird.

![][6]

Möchten Sie mit Ihrer eigenen Zertifikate und Testen HTTP- und HTTPS-Zugriff auf Ihre ASE:

1.  ASE-Benutzeroberfläche nach der Erstellung ASE zur **ASE -> Optionen -> ILB Zertifikate**
2.  ILB Zertifikat auswählen Zertifikat Pfx-Datei und Kennwort.  Dieser Schritt etwas Zeit in Anspruch nimmt, und die Nachricht, die eine Skalierung ausgeführt wird angezeigt.
3.  ILB-Adresse für die ASE abrufen (**ASE -> Eigenschaften -> virtuelle IP-Adresse**)
4.  Nach der Erstellung eine Webanwendung in ASE erstellen 
5.  Erstellen Sie einen virtuellen Computer haben Sie in dieser VNET (nicht im selben Teilnetz Pause ASE oder Punkte)
6.  DNS für Ihre Domäne festgelegt.  Sie verwenden einen Platzhalter mit der Unterdomäne in DNS oder möchten Sie einige einfache Tests, bearbeiten Sie die Datei Hosts die VM Web app Name VIP-Adresse festgelegt.  Die ASE hätte den Subdomainnamen. ilbase.com und Web app Mytestapp vorgenommen, so dass würde mytestapp.ilbase.com am dann in der Hostsdatei fest.  (Unter Windows steht die Hosts-Datei C:\Windows\System32\drivers\etc\)
7.  Verwenden Sie einen Browser auf die VM und http://mytestapp.ilbase.com (oder was Web app-Namen Ihrer Domäne ist)
8.  Verwenden Sie einen Browser auf die VM und fahren mit https://mytestapp.ilbase.com Sie die mangelnde Sicherheit akzeptieren, wenn ein selbstsigniertes Zertifikat verwenden müssen.  


Die IP-Adresse für die ILB wird in den Eigenschaften der virtuellen IP-Adresse aufgeführt.

![][4]


## <a name="using-an-ilb-ase"></a>Mithilfe einer ILB ASE ##

#### <a name="network-security-groups"></a>Netzwerk-Sicherheitsgruppen ####

Ein ILB ASE ermöglicht Netzwerkisolation für Ihre apps apps nicht oder sogar im Internet bekannt sind.  Dies ist hervorragend zum Hosten von Intranetsites wie von Business Applications.  Sie müssen auch Zugriff weiter können noch Network Security Groups(NSGs) Sie zum Steuern des Zugriffs auf Netzwerkebene. 


NSGs verwenden Sie Zugriff weiter einschränken möchten, müssen Sie sicherstellen, dass keine Kommunikation unterbrechen, die ASE Betrieb.  Auch wenn HTTP/HTTPS-Zugriff nur über ASE verwendet ILB abhängig ASE weiterhin außerhalb der VNet.  Um anzuzeigen welche Netzwerkzugriff erforderlich Suchen der Informationen im Dokument auf [Steuerung eingehender Datenverkehr zu einer App Service-Umgebung] ist[ ControlInbound] und das Dokument im [Netzwerk Konfigurationsdetails für App Service-Umgebungen mit ExpressRoute][ExpressRoute].  


Konfigurieren der NSGs müssen Sie die IP-Adresse kennen, die Ihre ASE Verwalten von Azure verwendet wird.  IP-Adresse ist auch der ausgehende IP-Adresse der ASE Internetanfragen macht.  Zu den IP-Adresse **-> Eigenschaften** zur und die **Ausgehende IP-Adresse**.  

![][5]


#### <a name="general-ilb-ase-management"></a>Allgemeine ILB ASE-management ####

Verwalten einer ILB ASE entspricht weitgehend eine ASE normalerweise verwalten.  Sie müssen Arbeitskraft Pools Weitere ASP Instanzen und Skalierung von Front-End-Servern vermehrt HTTP/HTTPS-Datenverkehr behandelt skalieren.  Für allgemeine Informationen über die Verwaltung der ASE, lesen Sie das Dokument zum [Konfigurieren einer App Service-Umgebung][ASEConfig].  


Die zusätzlichen Elemente sind Zertifikat und DNS-Management.  Sie müssen erhalten und Hochladen Zertifikat für HTTPS nach ILB ASE-Erstellung und bevor es abläuft.  Da Azure base Domäne besitzt, bieten wir für Asen mit externen VIP Zertifikate.  Da anhand einer ILB ASE Subdomäne alles sein kann, müssen Sie Ihr eigenes Zertifikat für HTTPS bereitstellen. 


#### <a name="dns-configuration"></a>DNS-Konfiguration ####

Beim Verwenden einer externen VIP DNS von Azure verwaltet wird.  Jede Anwendung in Ihre ASE erstellt wird Azure DNS automatisch öffentliche DNS-Server ist.  In einer ILB ASE müssen Sie ein eigenes DNS verwalten.  Für eine bestimmte Domäne wie contoso.corp.net müssen Sie DNS A erstellen, die auf Ihre Adresse ILB:

    * 
    *.SCM ftp veröffentlichen 


## <a name="getting-started"></a>Erste Schritte
Alle Artikel und -die App Service-Umgebungen in der [Readme-Datei für Anwendungsumgebungen](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Zunächst mit App-Dienst finden Sie unter [Einführung in App Service-Umgebungen][WhatisASE]

Weitere Informationen über die Plattform Azure App Service finden Sie in [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
