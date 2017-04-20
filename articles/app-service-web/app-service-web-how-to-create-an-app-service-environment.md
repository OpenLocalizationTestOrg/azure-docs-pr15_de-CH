<properties 
    pageTitle="Erstellen eine App Service-Umgebung" 
    description="Fluss Beschreibung app Service-Umgebungen erstellen" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>Erstellen eine App Service-Umgebung #

### <a name="overview"></a>Übersicht ###

App Service-Umgebung (ASE) sind eine Premium-Service von Azure App Service, erweiterte Konfigurationsfunktionen liefert, die nicht in der Multi-Tenant-Stempel.  ASE-Funktion stellt im Wesentlichen Azure App Service in virtuelle Netzwerke.  Ein besseres Verständnis der Funktionen App Umgebungen lesen [einer App Service-Umgebung] zu[ WhatisASE] Dokumentation.

### <a name="before-you-create-your-ase"></a>Bevor Sie Ihre ASE erstellen ###

Es ist wichtig zu der Dinge, die Sie ändern können.  Sind diese Aspekte, die Sie über Ihre ASE ändern können, nachdem es erstellt wurde:

- Speicherort
- Abonnement
- Ressourcengruppe
- VNet verwendet
- Subnetz verwendet 
- Subnet-Größe

Ein VNet auswählen und Angeben eines Subnetzes sicherstellen wird für zukünftiges Wachstum ausreicht.  

### <a name="creating-an-app-service-environment"></a>Erstellen einer App Service-Umgebung ###

Gibt es zwei Möglichkeiten, die ASE-Erstellung Benutzeroberfläche.  Es ist gefundenes Azure Marketplace für ***App Service-Umgebung*** oder mit neu -> Web + Mobile -> App Service-Umgebung.  So erstellen Sie eine ASE

1. Geben Sie den Namen der ASE.  Für ASE angegebenen Namen wird erstellt in ASE Apps verwendet werden.  Die ASE Appsvcenvdemo ist dann wäre der Subdomainnamen. *appsvcenvdemo.p.azurewebsites.net*.  Wenn Sie also eine Anwendung mit dem Namen *Mytestapp* erstellt würde am *mytestapp.appsvcenvdemo.p.azurewebsites.net*adressiert werden.  Leerzeichen können nicht im Namen der ASE.  Verwenden Sie Großbuchstaben im Namen werden der Domänennamen der gesamten Kleinbuchstabe mit diesem Namen.  Verwenden Sie ein ILB dann Ihre ASE wird nicht in der Domäne nennen wird stattdessen während der Erstellung ASE ausdrücklich

    ![][1]

2. Wählen Sie Ihr Abonnement.  Die ASE Abonnements ist eine alle apps, ASE mit erstellt werden.  Sie können nicht Ihre ASE ein VNet versehen, die in ein anderes Abonnement

3. Wählen Sie oder geben Sie eine neue Ressourcengruppe.  Die ASE zur Ressourcengruppe muss übereinstimmen, die für das VNet verwendet wird.  Wenn Sie eine vorhandene VNet auswählen, wird entsprechend der Ihr VNet Gruppenauswahl Ressource für Ihre ASE aktualisiert.

    ![][2]

4. Treffen Sie Ihre Auswahl virtuelles Netzwerk und Standort.  Sie können ein neues VNet erstellen, oder wählen eine vorhandene VNet.  Wenn Sie ein neues VNet auswählen, können Sie einen Namen und Speicherort angeben. Das neue VNet müssen 192.268.250.0/23 Bereich Adresse und eine Subnetzmaske **Standard** , die als 192.168.250.0/24 definiert.  Sie können auch einen vorhandenen klassischen oder Resource Manager VNet auswählen.  Die VIP Auswahl bestimmt die ASE direkt aus dem Internet (extern) zugegriffen werden kann oder wenn es eine interne Load Balancer (ILB).  Lernen über sie mehr [mithilfe einer internen Lastenausgleich mit einer App Service-Umgebung][ILBASE].  Wenn Sie einen externen VIP auswählen können wie viele externe IP-Adressen wählen Sie System mit IPSSL Zwecken erstellt wird.  Wenn Sie interne auswählen müssen Sie die Domäne angeben, die ASE.  Asen können in virtuellen Netzwerken bereitgestellt werden, die *entweder* öffentliche Adressbereiche *oder* RFC1918 Adressräume (d. h. verwenden Private Adressen).  Um ein virtuelles Netzwerk mit öffentlichen Adressbereich verwenden, müssen Sie VNet voraus erstellen.  Wenn Sie eine vorhandene VNet auswählen müssen Sie ein neues Subnetz beim ASE erstellen erstellen.  **Sie können nicht vorab erstellten Subnetz im Portal verwenden.  Können eine ASE mit einem bereits vorhandenen Subnetz die ASE Resource Manager Vorlage erstellen.**  Eine ASE aus eine Vorlage mit Informationen, [Erstellen einer App Service-Umgebung aus Vorlage] erstellen[ ILBAseTemplate] und [eine ILB App Service-Umgebung aus Vorlage erstellen][ASEfromTemplate].

### <a name="details"></a>Details ###

Eine ASE wird mit 2 Front-Ends und 2 erstellt.  Front-Ends wie HTTP/HTTPS-Endpunkte und Datenverkehr Arbeitskräfte sind die Rollen, die Ihre apps host senden.   Passen Sie die Menge nach der Erstellung ASE, und können auch Regeln auf diese Ressourcenpools skalieren.  Weitere Details manuelle Skalierung, Management und Überwachung einer App Service-Umgebung finden Sie hier: [Konfigurieren einer App Service-Umgebung][ASEConfig] 

Eine ASE kann im Subnetz ASE verwendet.  Das Subnetz kann für etwas anderes als ASE verwendet werden

### <a name="after-app-service-environment-creation"></a>Nach der Erstellung der App Service-Umgebung ###

Nach der Erstellung ASE können Sie:

- Menge des Front-Ends (minimum: 2)
- Menge der Arbeitnehmer (minimum: 2)
- Menge von IP-Adressen für IP-SSL
- Front-Ends oder Arbeitskräfte verwendete Ressource Größen berechnen (Front-End-Mindestgröße ist P2)


Mehr Details manuelle Skalierung, Management und Überwachung der App Service-Umgebungen hier: [Konfigurieren einer App Service-Umgebung][ASEConfig] 

Informationen über automatische Skalierung ist eine Anleitung: [Skalieren einer App Service-Umgebung konfigurieren][ASEAutoscale]

Gibt es weitere Faktoren, die nicht zur Anpassung der Datenbank und Speicher sind.  Diese sind von Azure behandelt und mit dem System.  Storage System unterstützt bis zu 500 GB für die gesamte Anwendung Service-Umgebung und die Datenbank wird von Azure Bedarf durch die Skalierung des Systems.


## <a name="getting-started"></a>Erste Schritte
Alle Artikel und -die App Service-Umgebungen in der [Readme-Datei für Anwendungsumgebungen](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Zunächst mit App-Dienst finden Sie unter [Einführung in App Service-Umgebungen][WhatisASE]

Weitere Informationen über die Plattform Azure App Service finden Sie in [Azure App Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
