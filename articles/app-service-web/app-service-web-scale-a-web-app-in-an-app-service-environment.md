<properties 
    pageTitle="Wie eine Anwendung in einer App Service-Umgebung" 
    description="Skalieren einer Anwendung in einer App Service-Umgebung" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>Skalierung von apps in einer App Service-Umgebung #

In Azure App Service gibt es normalerweise drei Dinge skaliert werden können:

- Preis plan
- Arbeitskraft-Größe 
- Anzahl der Instanzen.

In einer ASE entfällt auswählen oder Ändern der Tarif.  In Funktionen ist es bereits Funktion Preisstufe.  

Bei Arbeitskraft Größen können ASE Admin Größe Compute-Ressourcen für jede Arbeitskraft Pool verwendet werden.  Damit haben Arbeitskraft Pool 1 mit P4 Datenverarbeitungsressourcen und Arbeitskraft Pool 2 mit P1 Datenverarbeitungsressourcen bei Bedarf.  Sie haben keine Größe sein.  Für details, um die Größe und die Preise Siehe das Dokument [Azure App Servicepreise][AppServicePricing].  Dadurch werden die Skalierungsoptionen für Web-apps und App Service-Pläne in einer App Service-Umgebung zu:

- Arbeitskraft-Pool Auswahl
- Anzahl der Instanzen

Ändern der Elemente erfolgt über die entsprechende Benutzeroberfläche für Ihre ASE App Service-Pläne gehosteten angezeigt.  

![][1]

Sie können keine ASP die Anzahl der verfügbaren Serverressourcen in dem Arbeitspool skalieren, denen ASP wird.  Wenn Sie Ressourcen in diesem Pool Arbeitskraft berechnen müssen möchten Sie Systemadministrator ASE hinzuzufügen.  Informationen neu konfigurieren die ASE Informationen lesen: [konfigurieren eine App Service-Umgebung][HowtoConfigureASE].  Sie möchten auch nutzen ASE Autoscale Funktionen hinzufügen Kapazität auf Zeitplan oder Metriken basieren.  Um weitere Informationen zum Konfigurieren von Skalieren für ASE Umgebung finden Sie unter [Skalieren einer App Service-Umgebung konfigurieren][ASEAutoscale].

Servicepläne Datenverarbeitungsressourcen aus anderen Arbeitskraft mit mehreren app erstellen oder derselben Arbeitspool verwenden.  Beispiel (10) verfügbaren Serverressourcen Arbeitskraft Pool 1 haben, können eine app Service-Plan (6) Compute Ressourcen erstellen und planen ein zweiter app Service, (4) compute-Ressourcen.

### <a name="scaling-the-number-of-instances"></a>Die Anzahl der Instanzen skalieren ###

Bei Ihrer Anwendung in einer App Service-Umgebung erstellen, die mit 1 beginnt.  Sie können dann zusätzliche Instanzen zusätzliche Datenverarbeitungsressourcen für Ihre Anwendung zu skalieren.   

Hat Ihre ASE genügend Kapazität ist dies einfach.  Sie gehen Ihre App Service-Plan, die Sie wollen und wählen Sites enthält.  Die Benutzeroberfläche können Sie manuell die Skalierung für ASP oder skalieren Regeln für ASP konfigurieren wird geöffnet.  Manuell Maßstab festzulegen Ihrer Anwendung einfach ***nach skalieren*** auf ***eine manuell eingegebene Instanzenzahl***.  Hier ziehen Sie den Schieberegler auf die gewünschte Menge oder geben im Feld neben dem Schieberegler ein.  

![][2] 

Skalierungsgröße Regeln für eine ASP in einer ASE funktionieren genauso wie bisher.  Wählen Sie ***CPU-Prozentsatz*** unter ***Skalieren von*** und für ASP basierend auf CPU-Prozentsatz skalieren Regeln erstellen oder komplexere Regeln mit ***Zeitplan und Leistungsregeln***erstellen.  Weitere Informationen zur Konfiguration finden Sie unter Skalieren verwenden im Handbuch hier [Skalieren eine Anwendung in Azure App Service][AppScale]. 


### <a name="worker-pool-selection"></a>Arbeitskraft-Pool Auswahl ###

Wie bereits erwähnt, erfolgt die Arbeitskraft Pool Auswahl von ASP-Benutzeroberfläche.  Öffnen Sie das Blade für ASP und Arbeitspool auswählen möchten.  Sie sehen alle Pools Arbeitskraft in Ihrer App Service-Umgebung konfiguriert haben.  Wenn Sie nur eine Arbeitskraft können sehen Sie nur einen Pool aufgeführt.  Um welche Arbeitspool ändern ASP, wählen Sie einfach dem Arbeitspool Ihre App Service-Plan zu verschieben soll.  

![][3]

Vor ASP aus einer Arbeitskraft zu einem anderen ist es wichtig, haben Sie ausreichend Kapazität für ASP.  In der Liste der Arbeitskraft Pools nicht der Arbeitskraft Name erscheint nur Sie können auch sehen, wie viele Mitarbeiter in diesem Pool Arbeitskraft verfügbar sind.  Stellen Sie sicher, dass genügend Instanzen enthalten Ihre App Service-Plan zur.  Wenn mehr Ressourcen in dem Arbeitspool, denen Sie verschieben möchten berechnen müssen, erhalten Sie den ASE-Administrator hinzufügen.  

> [AZURE.NOTE] ASP aus einer Arbeitskraft kalten führt das Verschieben beginnt der apps in ASP.  Dadurch können Anfragen an Ihre Anwendung auf neue Serverressourcen gestartet kalt ist langsam.  Der Kaltstart vermieden [Anwendung Aufwärmen Funktion] mit[ AppWarmup] in Azure App Service.  Im Artikel beschriebene Initialisieren der Anwendung-Modul funktioniert auch für Kaltstart, da Initialisierungsprozess auch aufgerufen wird, wenn apps kalt auf neue Serverressourcen gestartet. 

## <a name="getting-started"></a>Erste Schritte

Sehen Sie mit App-Dienst zunächst [Wie zum Erstellen einer App Service-Umgebung][HowtoCreateASE]

Weitere Informationen über die Plattform Azure App Service finden Sie in [Azure App Service][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
