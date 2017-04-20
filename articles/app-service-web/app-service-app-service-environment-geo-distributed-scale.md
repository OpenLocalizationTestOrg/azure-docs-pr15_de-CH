<properties 
    pageTitle="Geo verteilt mit App Service-Umgebung" 
    description="Erfahren Sie, wie apps Traffic Manager und App Service-Umgebungen Geo-Verteilung mit der horizontalen Skalierung." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>Geo verteilt mit App Service-Umgebung

## <a name="overview"></a>Übersicht ##
Anwendungsszenarien hohe Skalierung erfordern können eine einzelne Bereitstellung von einer Anwendung zur Compute Ressourcenkapazität überschreiten.  Abstimmung Applikationen sind Sportereignisse und Fernsehen Veranstaltungen Beispiele für Szenarien, die extrem hohe Skalierung erfordern. Hohe Skalierung kann erfüllt sein von apps mit mehreren app-Bereitstellungen in einem einzigen Bereich und Regionen, extremer Auslastung Anforderungen gemacht horizontal skalieren.

App Service-Umgebung sind eine ideale Plattform für die horizontale Skalierung.  Einmal können einer App Service-Umgebung Konfiguration ausgewählt wurde, die eine bekannte Anforderungsrate unterstützen Entwickler zusätzliche App Service-Umgebungen "Ausstechform" Weise zu einer gewünschten Maximale Belastbarkeit bereitstellen.

Angenommen Sie, eine Anwendung auf einer App Service-Umgebung Konfiguration getestet um 20K Anfragen pro Sekunde (RPS).  Ist die gewünschte maximale Belastbarkeit 100 K RPS können fünf (5) Anwendung Umgebungen erstellt und um sicherzustellen, dass die Anwendung die maximale geplante Last verarbeiten kann konfiguriert werden.

Da Kunden normalerweise apps mit einer benutzerdefinierten (oder personalisiertes) Domäne zugreifen, benötigen Entwickler app Anfragen über alle Instanzen App Service-Umgebung verteilen.  Hervorragend dazu zu der benutzerdefinierten Domäne mit einem [Azure Traffic Manager-Profil]ist[AzureTrafficManagerProfile].  Traffic Manager-Profil kann alle einzelnen App Service-Umgebungen Zeitpunkt konfiguriert werden.  Traffic Manager verarbeitet automatisch Verteilen von Kunden aller App Service-Umgebungen basierend auf den Lastenausgleich bei Traffic Manager-Profil.  Dieser Ansatz funktioniert unabhängig davon, ob alle App Service-Umgebungen in einer Region Azure bereitgestellt oder mehrere Azure Regionen weltweit eingesetzt.

Da Kunden apps durch personalisiertes Domäne zugreifen, sind Kunden nicht die Anzahl der App Service-Umgebungen Ausführen einer app.  Entwickler können daher schnell und einfach hinzufügen und entfernen, App Service-Umgebungen beobachteten Auslastung abhängig.

Das konzeptionelle Diagramm unten zeigt eine Anwendung in drei App-Dienst in einem einzigen Bereich horizontal skalieren.

![Grundlegende Architektur][ConceptualArchitecture] 

Im Rest dieses Themas durchläuft die Schritte beim Einrichten einer verteilten Topologie für die Beispiel-app mit mehreren App Service-Umgebungen.

## <a name="planning-the-topology"></a>Planung der Topologie ##
Vor der Erstellung einer verteilten Anwendung Speicherbedarf hilft es einige Teile Informationen voraus.

- **Benutzerdefinierte Domäne für die Anwendung:**  Was ist Domainnamen, mit denen Kunden die app zugreifen?  Die Beispiel-app ist der benutzerdefinierte Name *www.scalableasedemo.com*
- **Traffic Manager-Domäne:**  Ein Domänenname muss gewählt werden, wenn ein [Azure Traffic Manager-Profil]erstellen[AzureTrafficManagerProfile].  Dieser Name wird mit dem Suffix *trafficmanager.net* sich von Traffic Manager verwaltet einen Domäneneintrag kombiniert werden.  Für die Beispiel-app ist der gewählte Name *skalierbare ase-Demo*.  Daher wird der vollständigen Domänennamen, der von Traffic Manager verwaltet *skalierbare ase-demo.trafficmanager.net*.
- **Strategie für die Skalierung der app Speicherbedarf:**  Wird der Speicherbedarf der Anwendung mehrere App Service-Umgebungen in einer Region verteilt?  Mehrere Bereiche?  Ein-Sortimente beider Ansätze?  Die Entscheidung sollte basieren auf woher Kundenverkehr sowie der Rest einer Anwendung unterstützen Back-End-Infrastruktur wie gut skaliert werden kann.  Beispielsweise kann mit einer 100 % statusfreie Anwendung eine app massiv skaliert werden mithilfe einer Kombination von mehreren App Umgebungen pro Azure Region multipliziert App Service-Umgebungen in mehreren Regionen Azure bereitgestellt.  Mit 15 öffentlichen Azure wählen können Kunden wirklich eine Präsenz weltweit hyper-Anwendung erstellen.  Für die Beispielapp für diesen Artikel verwendet wurden drei App Service-Umgebungen in einer einzigen Azure Region (südlichen zentralen USA) erstellt.
- **Benennungskonvention für die App Service:**  Jede App Service-Umgebung muss einen eindeutigen Namen.  Über ein oder zwei App Service-Umgebungen ist es hilfreich, eine Benennungskonvention zum Identifizieren der einzelnen App Service-Umgebung.  Die Beispiel-app wurde eine einfache Benennungskonvention verwendet.  Die Namen der drei App Service sind *fe1ase*, *fe2ase*und *fe3ase*.
- **Benennungskonvention für apps:**  Da mehrere Instanzen der Anwendung bereitgestellt werden, ist ein für jede Instanz der bereitgestellten Anwendung erforderlich.  Eine ist wenig bekannte aber besonders App Service-Umgebung, dass dieselbe app Name in mehreren App-Dienst verwendet werden kann.  Seit jeder App Service-Umgebung eindeutig Domänensuffix können Entwickler genau dieselbe app Namen in jeder Umgebung verwenden.  Ein Entwickler kann beispielsweise folgendermaßen apps besitzen: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*.  Für die Beispiel-app hat jede Instanz app auch einen eindeutigen Namen.  Namen der app verwendet werden *webfrontend1*, *webfrontend2*und *webfrontend3*.


## <a name="setting-up-the-traffic-manager-profile"></a>Traffic Manager-Profil einrichten ##
Wenn mehrere Instanzen einer Anwendung auf mehrere App Service-Umgebung bereitgestellt werden, können die einzelnen app-Instanzen mit Traffic Manager registriert.  Profil benötigt für die Beispiel-app Traffic Manager für *skalierbare ase-demo.trafficmanager.net* , die Kunden zum bereitgestellten app-Instanzen weiterleiten kann:

- **webfrontend1.fe1ase.p.azurewebsites.net:**  Eine Instanz der Beispiel-app auf die erste App Service-Umgebung bereitgestellt.
- **webfrontend2.fe2ase.p.azurewebsites.net:**  Eine Instanz der Beispiel-app auf die zweite App Service-Umgebung bereitgestellt.
- **webfrontend3.fe3ase.p.azurewebsites.net:**  Eine Instanz der Beispiel-app auf die dritte App Service-Umgebung bereitgestellt.

Mehrere Azure App-Endpunkte alle in **derselben** Azure-Region sich am einfachsten mit Powershell [Azure Resource Manager Traffic Manager unterstützt][ARMTrafficManager].  

Der erste Schritt ist ein Azure Traffic Manager-Profil erstellen.  Der folgende Code zeigt, wie das Profil für die Beispiel-app erstellt wurde:

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Beachten Sie wie der Parameter *RelativeDnsName* *skalierbare ase*Demo gesetzt wurde.  Dies ist wie Domain Name *skalierbare ase-demo.trafficmanager.net* erstellt und ein Traffic Manager-Profil zugeordnet.

*TrafficRoutingMethod* -Parameter definiert die Lastenausgleichsrichtlinie verwendeten Traffic Manager, wie sich Kunden Laden aller verfügbaren Endpunkte.  In diesem Beispiel *gewichtet* wurde Methode ausgewählt.  Dies führt zu Kundenanfragen verteilt über alle Endpunkte registrierte Anwendung basierend auf der relativen Gewichtung jeder Endpunkt zugeordnet. 

Mit dem Profil erstellt wird wird jeweils app Profil als systemeigene Azure Endpunkt hinzugefügt.  Der folgende Code Ruft einen Verweis auf jeden front-End-WebApp und fügt jede Anwendung als Traffic Manager-Endpunkt durch den *TargetResourceId* -Parameter.


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
Es ist ein Aufruf *Hinzufügen AzureTrafficManagerEndpointConfig* für jede einzelne app-Instanz.  Der Parameter *TargetResourceId* in Powershell-Befehl verweist auf eine der drei bereitgestellten app-Instanzen.  Traffic Manager-Profil wird alle drei Endpunkte im Profil registriert Last verteilt.

Alle drei Endpunkte verwenden denselben Wert (10) für den Parameter *Gewicht* .  Dies führt zu verbreiten Kundenanfragen Traffic Manager für alle drei app-Instanzen relativ gleichmäßig. 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Zeigt benutzerdefinierte Anwendungsdomäne Traffic Manager-Domäne ##
Der letzte Schritt ist die benutzerdefinierte Domäne App Traffic Manager-Domäne verweisen.  Die Beispiel-app bedeutet für *skalierbare ase demo.trafficmanager.net* *www.scalableasedemo.com* auf.  Dieser Schritt muss mit dem Domain-Registrar abgeschlossen werden, die benutzerdefinierte Domäne verwaltet.  

Mithilfe des Registrators Domäne, zeichnet ein CNAME muss erstellt werden, der die benutzerdefinierte Domäne Traffic Manager-Domäne verweist.  Die folgende Abbildung zeigt ein Beispiel für das Aussehen dieser CNAME-Konfiguration:

![CNAME für benutzerdefinierte Domäne][CNAMEforCustomDomain] 

Obwohl in diesem Thema nicht behandelt, daran jeweils einzelne app muss die benutzerdefinierte Domäne sowie mit registriert.  Andernfalls Wenn die Anwendung keinen benutzerdefinierte Domäne registriert die Anwendung eine Anforderung für eine app-Instanz gemacht, schlägt die Anforderung fehl.  

In diesem Beispiel wird die benutzerdefinierte Domäne *www.scalableasedemo.com*und jede Anwendungsinstanz wurde der benutzerdefinierten Domäne zugeordnet.

![Benutzerdefinierte Domäne][CustomDomain] 

Eine Zusammenfassung von Azure App Service apps eine benutzerdefinierte Domäne registrieren, finden Sie im folgenden Artikel auf [benutzerdefinierte Domänen registrieren][RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Probieren Sie die Topologie verteilt ##
Das Endergebnis der Traffic Manager und DNS-Konfiguration ist, dass Anfragen für *www.scalableasedemo.com* die folgende Sequenz durchlaufen werden:

1. Ein Browser oder das Gerät wird ein DNS-Lookup für *www.scalableasedemo.com* machen.
2. CNAME-Eintrag in der Domain-Registrar wird DNS-Lookup, Azure Traffic Manager weitergeleitet.
3. Ein DNS-Lookup für *skalierbare ase-demo.trafficmanager.net* gegen die Azure Traffic Manager DNS-Server erfolgt.
4. Anhand der Lastenausgleichsrichtlinie ( *TrafficRoutingMethod* Parameters verwendete beim Traffic Manager-Profil erstellen) Traffic Manager Wählen eines konfigurierten Endpunkte und FQDN dem Endpunkt an den Browser oder das Gerät zurück.
5.  Da der FQDN des Endpunkts der Url einer Instanz einer Anwendung auf eine App Service-Umgebung, fragt der Browser oder das Gerät einen Microsoft Azure DNS-Server den vollqualifizierten Domänennamen in eine IP-Adresse auflösen. 
6. Der Browser oder das Gerät sendet die Anforderung HTTP/S IP-Adresse.  
7. Die Anforderung erreichen eines app-Instanzen eines App Service-Umgebung.

Konsole Bild zeigt eine DNS-Suche für die Beispiel-app benutzerdefinierte Domäne erfolgreich aufgelöst, eine app-Instanz auf einem der drei Beispiel App Service (in diesem Fall die zweite der drei App Service):

![DNS-Suche][DNSLookup] 

## <a name="additional-links-and-information"></a>Zusätzliche Links und Informationen ##
Alle Artikel und -die App Service-Umgebungen in der [Readme-Datei für Anwendungsumgebungen](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

Dokumentation der Powershell [Azure Resource Manager Traffic Manager unterstützt][ARMTrafficManager].  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
