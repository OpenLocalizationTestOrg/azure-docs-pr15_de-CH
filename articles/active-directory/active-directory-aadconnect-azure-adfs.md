<properties
    pageTitle="Active Directory-Verbunddienste in Azure | Microsoft Azure"
    description="In diesem Dokument erfahren Sie, wie Sie AD FS in Azure für hohe Verfügbarkeit bereitstellen."
    keywords="Bereitstellen von AD FS in Azure, Bereitstellen von Azure Adfs Azure Adfs, Azure Ad fs Adfs bereitstellen, Bereitstellen von Ad fs Adfs in Azure bereitstellen Adfs in Azure, Bereitstellen von AD FS Azure Adfs Azure, Einführung in AD FS, Azure, AD FS in Azure Iaas, ADFS Adfs in Azure verschieben"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="anandy;billmath"/>

# <a name="ad-fs-deployment-in-azure"></a>AD FS-Bereitstellung in Azure 

AD FS bietet vereinfachten, gesicherte Identitätsverbund und Web einmaliges Anmelden (SSO) Funktionen. Föderation mit Azure AD oder Office 365 ermöglicht lokale Anmeldeinformationen authentifizieren und Zugriff auf alle Ressourcen in der Cloud. Dadurch wird eine hohe Verfügbarkeit AD FS Infrastruktur um Zugang zu Ressourcen sowohl lokal und in der Cloud. Bereitstellen von AD FS in Azure können die hohe Verfügbarkeit mit minimalem Aufwand zu erreichen.
Einige Vorteile der Bereitstellung von AD FS in Azure, einige davon sind unten aufgeführt:

* **Hohe Verfügbarkeit** – gewährleistet mit Azure Verfügbarkeit legt eine Infrastruktur mit hoher verfügbare.
* **Einfache Skalierung** – mehr Performance benötigen? Einfache Migration zu leistungsfähigeren Computer mit nur wenigen Klicks in Azure
* **Cross-Geo Redundanz** – Azure Geo Redundanz können Sie sicher sein, ist Ihre Infrastruktur hochverfügbare weltweit
* **Einfach zu verwalten** – ist mit stark vereinfachtes Management Azure-Portal Verwaltung Ihrer Infrastruktur sehr einfach und problemlos 

## <a name="design-principles"></a>Entwurfsprinzipien

![Bereitstellungsentwurf](./media/active-directory-aadconnect-azure-adfs/deployment.png)

Das Diagramm oben zeigt die empfohlene grundlegende Topologie Bereitstellen einer AD FS-Infrastruktur Azure. Die Prinzipien der verschiedenen Komponenten der Topologie sind:

* **DC / ADFS-Server**: Wenn Sie weniger als 1.000 Benutzer können ADFS-Rolle einfach auf den Domänencontrollern installieren. Bereitstellen Sie möchten Sie keine Leistungseinbußen auf den Domänencontrollern oder wenn mehr als 1.000 Benutzer anschließend AD FS auf separaten Servern.
* **WAP-Server** sind Anwendungsproxy Web Server bereitstellen, damit Benutzer AD FS erreichen können, wenn sie nicht im Unternehmensnetzwerk auch sind.
* **DMZ**: The Web Application Proxy Server in der DMZ platziert und TCP/443 Zugriff zwischen der DMZ und interne Subnetz zulässig ist.
* **Balancers laden**: zur Sicherstellung hoher Verfügbarkeit von AD FS und Web Application Proxy Server empfehlen wir internen Lastenausgleich für AD FS-Servern und Azure Lastenausgleich für Web Application Proxy Server.
* **Verfügbarkeit legt**: Redundanz der AD FS-Bereitstellung wird empfohlen, mindestens zwei virtuelle Computer in einer Verfügbarkeit für ähnliche Aufgaben gruppieren. Diese Konfiguration stellt sicher, dass während ein wartungsereignis geplant oder ungeplant mindestens einen virtuellen Computer verfügbar ist
* **Speicherkonten**: Es wird empfohlen, zwei Speicherkonten verfügen. Ein einzelnes Speicherkonto kann eine einzelne Fehlerquelle erstellen und kann die Bereitstellung einer unwahrscheinlich verfügbar, in dem das Speicherkonto ausfällt. Zwei Speicherkonten können ein Speicherkonto für jede Verwerfungslinie zuordnen.
* **Netzwerk-Trennung**: Web Application Proxy Server in einem separaten DMZ-Netzwerk bereitgestellt werden soll. Zwei Subnetzen ein virtuelles Netzwerk unterteilen können und Bereitstellung Web Application Proxy Server in einem isolierten Subnetz. Sie können einfach Konfigurieren der Network Security Group für jedes Subnetz und nur erforderliche Kommunikation zwischen zwei Subnetzen. Weitere Details erhalten pro Bereitstellungsszenario unten

##<a name="steps-to-deploy-ad-fs-in-azure"></a>Schritte zum Bereitstellen von AD FS in Azure

Die in diesem Abschnitt erwähnten Schritte des Leitfaden für die Bereitstellung der unten dargestellten AD FS-Infrastruktur in Azure.

### <a name="1-deploying-the-network"></a>1. Bereitstellen im Netzwerk

Wie oben dargelegt, können Sie entweder zwei Subnetzen in ein virtuelles Netzwerk erstellen oder aber zwei vollständig unterschiedliche virtuelle Netzwerke (VNet) erstellt. In diesem Artikel Bereitstellen eines virtuellen Netzwerks konzentrieren und zwei Subnetzen unterteilen. Dies ist derzeit ein einfacherer Ansatz wie zwei separate VNets VNet VNet-Gateway für die Kommunikation benötigen.

**1.1 virtuelles Netzwerk erstellen**

![Virtuelles Netzwerk erstellen](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)
    
In Azure-Portal können virtuelle Netzwerk auswählen und das virtuelle Netzwerk und ein Subnetz mit nur einem Klick bereitstellen. INT Subnetz auch definiert und ist jetzt für virtuelle Computer hinzugefügt werden.
Im nächste Schritt wird ein anderes Subnetz, d. h. das DMZ-Subnetz hinzugefügt werden. Subnetz DMZ einfach erstellen

* Wählen Sie das neu erstellte Netzwerk
* Wählen Sie in den Eigenschaften des Subnetz
* Im Subnetz Bereich klicken Sie auf die Schaltfläche hinzufügen
* Die Auskünfte Subnetz Name und Adresse Speicherplatz im Subnetz erstellen

![Subnetz](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)


![Subnetz DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2 erstellen Netzwerk von Sicherheitsgruppen**

Eine Netzwerk-Sicherheitsgruppe (NSG) enthält eine Liste der Zugriffssteuerungsliste (ACL) Regeln, die zulassen oder verweigern auf VM-Instanzen in einem virtuellen Netzwerk. NSGs können Subnets oder einzelne VM-Instanzen innerhalb eines Subnetzes zugeordnet werden. Wenn eine NSG einem Subnetz zugeordnet ist, Regeln die ACL auf alle Instanzen virtueller Computer in diesem Subnetz.
Für diese Anleitung erstellen wir zwei NSGs: für ein internes Netzwerk und einer DMZ. Sie ist NSG_INT und NSG_DMZ bzw. gekennzeichnet.

![NSG erstellen](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Nach dem Erstellen die NSG werden 0 ein- und 0 ausgehende Regeln. Sobald die Rollen auf den jeweiligen Servern installiert und funktionsfähig sind, können die eingehenden und ausgehenden Regeln nach der gewünschten Sicherheitsstufe erfolgen.

![NSG initialisieren](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Nach der Erstellung der NSGs Subnetz NSG_INT zuordnen INT und NSG_DMZ mit Subnetz DMZ. Nachstehend finden Sie ein Beispiel-Screenshot:

![NSG konfigurieren](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Klicken Sie auf Subnets Teilnetze Fenster
* Wählen Sie das Subnetz der NSG zugeordnet 

Nach der Konfiguration sollte der Anzeige Subnetze unter aussehen:

![Subnetze nach NSG](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3 Verbindung zu lokalen erstellen**

Wir benötigen eine Verbindung zum lokalen Domänencontroller (DC) in Azure bereitstellen. Azure bietet Konnektivität zum Azure-Infrastruktur Ihrer lokalen Infrastruktur herstellen.

* Punkt-zu-Standort
* Virtuelles Netzwerk-Standorten
* ExpressRoute

Es wird empfohlen, die ExpressRoute verwenden. ExpressRoute können Sie private Verbindung zwischen Azure-Datencentern und Infrastruktur vor Ort oder in einer Umgebung mit gemeinsam. ExpressRoute-Verbindung gehen nicht über das öffentliche Internet. Sie bieten mehr Zuverlässigkeit, beschleunigt, niedriger Latenz und höhere Sicherheit als normalen Verbindungen über das Internet.
Obwohl empfohlen wird, ExpressRoute verwenden, können Sie eine Verbindungsmethode für Ihre Organisation am besten geeignet. Erfahren Sie mehr über ExpressRoute und die verschiedenen ExpressRoute mit Konnektivitätsoptionen lesen Sie [ExpressRoute technische Übersicht](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. erstellen Sie Speicherkonten

Um hohe Verfügbarkeit und ein einzelnes Speicherkonto Abhängigkeit zu vermeiden, können Sie zwei Speicherkonten erstellen. Unterteilen Sie Computer in jedem Satz Verfügbarkeit in zwei Gruppen, und weisen Sie jeder Gruppe einen getrennten Konto.

![Storage-Konten erstellen](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. erstellen Sie Verfügbarkeit

Erstellen Sie für jede Rolle (DC-AD FS und WAP) Verfügbarkeit, die 2 Computer mindestens enthalten. Dadurch höhere Verfügbarkeit für jede Rolle. Beim Erstellen der Verfügbarkeit wird unbedingt Folgendes festlegen:
* **Fehlerdomänen**: virtuelle Computer in derselben Fehlerdomäne gemeinsam dieselbe Stromversorgung und Netzwerk-Switch. Mindestens 2 Fehlerdomänen werden empfohlen. Der Standardwert ist 3 und lassen Sie es für die Bereitstellung
* **Aktualisieren von Domänen**: Maschinen zur selben Domäne aktualisieren werden während einer Aktualisierung gestartet. Mindestens 2 aktualisierungsdomänen möchten. Der Standardwert ist 5, und lassen Sie es für die Bereitstellung

![Verfügbarkeit-sets](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

Erstellen der folgenden Verfügbarkeit

| Festlegen der Verfügbarkeit | Rolle | Fehlerdomänen | Aktualisieren von Domänen |
|:----------------:|:----:|:-----------:|:-----------|
| contosodcset | DC-ADFS | 3 | 5 |
| contosowapset | WAP | 3 | 5 |

### <a name="4--deploy-virtual-machines"></a>4. Bereitstellung virtueller Maschinen
Im nächste Schritt wird auf virtuellen Computern bereitstellen, die die verschiedenen Rollen in Ihrer Infrastruktur hostet. Mindestens zwei Computer sollten in jedem Satz Verfügbarkeit. Erstellen Sie sechs virtuelle Computer für die einfache Bereitstellung.

| Computer | Rolle | Subnetz | Festlegen der Verfügbarkeit | Konto | IP-Adresse |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|contosodc1|DC-ADFS|INT|contosodcset|contososac1|Statische|
|contosodc2|DC-ADFS|INT|contosodcset|contososac2|Statische|
|contosowap1|WAP|DMZ|contosowapset|contososac1|Statische|
|contosowap2|WAP|DMZ|contosowapset|contososac2|Statische|

Wie Sie vielleicht bemerkt haben, wurde keine NSG angegeben. Dies ist da Azure NSG auf Subnetzebene verwenden kann. Dann können Sie mit einzelnen NSG entweder im Subnetz sonst NIC-Objekt zugeordneten Computer den Netzwerkverkehr steuern. Lesen Sie weitere [Neuigkeiten Network Security Group (NSG)](https://aka.ms/Azure/NSG).
Statische IP-Adresse wird empfohlen, wenn Sie DNS verwalten. Sie können Azure DNS verwenden und stattdessen die DNS-Datensätze für Ihre Domäne beziehen sich auf die neuen Computer durch ihre Azure FQDNs.
Ihre virtuellen Bereich sollte unten Aussehen nach Abschluss der Bereitstellung:

![Virtuelle Computer bereitgestellt](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. Konfiguration des Domänencontrollers / AD FS-Server
 Um jede eingehende Anforderung zu authentifizieren, müssen AD FS-Domänencontroller. Um kostspielige Reise von Azure lokalen Domänencontroller für die Authentifizierung zu speichern, wird empfohlen, ein Replikat des Domänencontrollers in Azure bereitstellen. Um hohe Verfügbarkeit zu erreichen, wird empfohlen, einer Verfügbarkeit von mindestens 2-Domänencontroller erstellen.

|Domänencontroller|Rolle|Konto|
|:-----:|:-----:|:-----:|
|contosodc1|Replikat|contososac1|
|contosodc2|Replikat|contososac2|

* Fördern Sie die beiden Server als Replikat-Domänencontroller in DNS
* Konfigurieren der AD FS-Server die AD FS-Rolle mithilfe des Server-Managers installieren.

###<a name="6---deploying-internal-load-balancer-ilb"></a>6. Bereitstellen von internen Lastenausgleich (ILB)

**6.1 Erstellen der ILB**

Zum Bereitstellen einer ILB hinzufügen wählen Lastenausgleich in der Azure-Portal auf (+).
>[AZURE.NOTE] Wenn **Lastenausgleich** nicht im Menü angezeigt wird, klicken Sie unten links vom Portal und blättern **Suchen** bis **Zum Lastenausgleich**angezeigt.  Klicken Sie auf den gelben Stern zum Menü hinzufügen. Wählen Sie jetzt das neue Load Balancer Symbol Fenster Konfiguration des Lastenausgleich zunächst.

![Wechseln Sie zum Lastenausgleich](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Name**: geeignete Benennen der Lastenausgleich
* **Schema**: Dieses System zum Lastenausgleich vor den AD FS-Servern platziert und soll für interne Netzwerkanschlüsse, "Internal" Wählen
* **Virtuelles Netzwerk**: Wählen Sie das virtuelle Netzwerk, in dem Sie Ihre AD FS bereitstellen,
* **Subnetzmaske**: Wählen Sie das interne Subnetz
* **IP-Adresszuweisung**: dynamische

![Interne Lastenausgleich](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)
 
Nach Anklicken erstellen und die ILB bereitgestellt wird, sollte in der Liste der Lastenausgleich angezeigt:

![Lastenausgleich nach ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)
 
Als Nächstes ist der Pool-Back-End und Back-End-Prüfpunkt.

**6.2 ILB Back-End-Pool konfigurieren**

Wählen Sie das neu erstellte ILB im Lastenausgleich. Bereich "Settings" wird geöffnet. 
1.  Einstellungspanel Back-End-Pools auswählen
2.  Klicken Sie im Fenster Add Back-End-Pool auf virtuelle Computer hinzufügen
3.  Mit einem Bereich wird angezeigt, in dem Sie Verfügbarkeit auswählen können
4.  Wählen Sie AD FS Verfügbarkeit

![Konfigurieren Sie ILB Back-End-pool](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)
 
**6.3 Prüfpunkt konfigurieren**

Wählen Sie in der Optionsleiste ILB Prüfpunkte.
1.  Klicken Sie auf Hinzufügen
2.  Angaben für die Probe ein. **Name**: Name b Prüfpunkt. **Protokoll**: TCP-c. **Port**: 443 (HTTPS) d. **Intervall**: 5 (Standardwert) – Dies ist das Intervall an dem Prüfpunkt ILB Maschinen im Back-End-Pool e. **Fehlerhafte Grenzwert**: 2 (Standard Val Ue) – Dies ist die aufeinander folgenden Prüfpunkt Fehler nach dem ILB ein Computers in der Back-End-Pool deklarieren, nicht mehr reagiert und nicht mehr Datenverkehr zu senden.

![Konfigurieren Sie die ILB probe](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)
 
**6.4 Lastenausgleichscluster Regeln**

Um den Datenverkehr effizient zu verteilen, sollte die ILB mit Lastenausgleich Regeln konfiguriert werden. Um einen Lastenausgleich Regel erstellen 
1.  Wählen Sie Load balancing Regel aus der Einstellung der ILB
2.  Klicken Sie auf den Lastenausgleich Regel Bereich hinzufügen
3.  In der Regel Bereich Lastenausgleich durch Hinzufügen einer. **Name**: Geben Sie einen Namen für die Regel b. **Protokoll**: TCP c wählen. **Port**: 443 d. **Back-End-Port**: 443 e. **Back-End-Pool**: Wählen Sie den Pool für AD FS früheren f cluster erstellt. **Probe**: AD FS-Server zuvor erstellten Prüfpunkt aktivieren

![Konfigurieren von Netzwerklastenausgleich Regeln ILB](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5 DNS mit ILB aktualisieren**

Ihr DNS-Server gehen und CNAME für die ILB erstellen. Der CNAME-Eintrag sollte für den Verbunddienst die IP-Adresse die IP-Adresse der ILB auf. Zum Beispiel ILB DIP-Adresse 10.3.0.8 und der Verbunddienst installiert ist fs.contoso.com, erstellen Sie einen CNAME für fs.contoso.com auf 10.3.0.8.
Dadurch wird sichergestellt, dass alle Kommunikation fs.contoso.com Ende um die ILB und entsprechend weitergeleitet.

###<a name="7---configuring-the-web-application-proxy-server"></a>7. konfigurieren Web Application Proxy server

**7.1 konfigurieren Web Application Proxy Server zu AD FS-Server**

Um sicherzustellen, dass Web Application Proxy Server die AD FS-Server hinter der ILB erreichen, erstellen Sie einen Datensatz in der %systemroot%\system32\drivers\etc\hosts für die ILB. Beachten Sie, dass der distinguished Name (DN) Federation Service Name beispielsweise fs.contoso.com sollte. Und IP-Eintrag sollte die ILB IP-Adresse (wie im Beispiel 10.3.0.8).

**7.2 installieren die Rolle Web Application Proxy**

Nachdem Sie sichergestellt haben, dass Web Application Proxy Server die AD FS-Server hinter ILB erreichen können, können Sie weiter die Web Application Proxy Server installieren. Anwendungsproxy Webserver sind nicht der Domäne angehören. Installieren Sie Web Application Proxy Rollen auf dem Web Application Proxy durch Auswählen der Funktion. Der Servermanager führt Sie der WAP-Installation.
Lesen Sie weitere Informationen zum Bereitstellen von WAP [Web Application Proxy Server installieren und konfigurieren](https://technet.microsoft.com/library/dn383662.aspx).

###<a name="8---deploying-the-internet-facing-public-load-balancer"></a>8. Bereitstellen der (öffentliche) zum Lastenausgleich mit Internetzugriff

**8.1 erstellen Sie (Public) zum Lastenausgleich mit Internetzugriff**
 
Wählen Sie in Azure-Portal Lastenausgleich, und klicken Sie dann auf Hinzufügen. Geben Sie folgende Informationen im Fenster erstellen Load balancer
1. **Name**: Name des Lastenausgleichs
2. **Schema**: öffentliche – veranlaßt Azure, dass dieses System zum Lastenausgleich eine öffentliche Adresse benötigen.
3. **IP-Adresse**: Erstellen Sie eine neue IP-Adresse (dynamisch)

![Internet mit Lastenausgleich](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Nach der Bereitstellung wird der Lastenausgleich Load Balancers Liste angezeigt.

![Load Balancer Liste](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)
 
**8.2 Zuweisen einer DNS-Bezeichnung öffentliche IP-Adresse**

Klicken Sie auf die neu erstellte Load Balancer im Bereich Load Balancers Bereich Konfiguration bringen. Führen Sie die folgenden Schritte zum Konfigurieren von DNS-Bezeichnung für die öffentliche IP:
1.  Klicken Sie auf die öffentliche IP-Adresse. Dies öffnet den Bereich für die öffentliche IP-Adresse und die
2.  Klicken Sie auf Konfiguration
3.  Geben Sie eine DNS-Bezeichnung. Dadurch werden öffentlichen DNS-Bezeichnung, die Sie überall beispielsweise contosofs.westus.cloudapp.azure.com zugreifen können. Sie können einen Eintrag im externen DNS für den Verbunddienst (wie fs.contoso.com), die DNS-Bezeichnung des externen Lastenausgleich (contosofs.westus.cloudapp.azure.com) aufgelöst wird.

![Konfigurieren Sie Lastenausgleich mit Internetzugriff](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Konfigurieren Sie Lastenausgleich (DNS) für den Internetzugriff](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3 Konfigurieren von Back-End-Pool für Lastenausgleich Internet Facing (öffentlich)** 

Führen Sie die gleichen Schritte wie interne Lastenausgleich konfigurieren den Back-End-Pool für Internet Facing (Public) Lastenausgleich Verfügbarkeit legen Sie der WAP-Server erstellen. Beispielsweise Contosowapset.

![Konfigurieren Sie Internet Facing Lastenausgleich Back-End-pool](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)
 
**8.4 Prüfpunkt konfigurieren**

Führen Sie die gleichen Schritte wie interne Lastenausgleich Konfigurieren des Prüfpunkts für den Back-End-Serverpool WAP konfigurieren.

![Probe des Internet Facing Lastenausgleich konfigurieren](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)
 
**8.5. Lastenausgleichscluster Regeln**

Führen Sie die gleichen Schritte wie ILB Lastenausgleich Regel für TCP 443 konfigurieren.

![Konfigurieren von Netzwerklastenausgleich Regeln Internet Facing Lastenausgleich](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)
 
###<a name="9---securing-the-network"></a>9. Sichern des Netzwerks

**9.1 Sichern interne Subnetz**

Insgesamt benötigen Sie die folgenden Regeln effiziente interne Subnetz (in der Reihenfolge der unten aufgelisteten) sichern

|Regel|Beschreibung|Fluss|
|:----|:----|:------:|
|AllowHTTPSFromDMZ| HTTPS-Kommunikation von DMZ zulassen | Eingehende |
|DenyInternetOutbound| Kein Zugriff auf internet | Ausgehende |

![Zugriffsregeln INT (eingehend)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[Kommentar]: <> (![INT Zugriffsregeln (eingehend)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [Kommentar]: <> (![INT Zugriffsregeln (ausgehend)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
 
**9.2 sichern das DMZ-Subnetz**

|Regel|Beschreibung|Fluss|
|:----|:----|:------:|
|AllowHTTPSFromInternet| HTTPS vom Internet zur DMZ zulassen | Eingehende|
|DenyInternetOutbound|  Alles außer HTTPS Internet blockiert | Ausgehende |

![EXT Zugriffsregeln (eingehend)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[Kommentar]: <> (![EXT Zugriffsregeln (eingehend)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [Kommentar]: <> (![EXT Zugriffsregeln (ausgehend)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

>[AZURE.NOTE] Wenn Clientbenutzer Authentifizierung Zertifikat (ClientTLS Authentifizierung mit X509 Benutzerzertifikate) ist erforderlich, erfordert AD FS TCP Port 49443 für eingehenden Datenverkehr aktiviert sein.

###<a name="10--test-the-ad-fs-sign-in"></a>10. testen Sie 10. die AD FS-Anmeldung

Am einfachsten ist AD FS Testen mithilfe der Seite IdpInitiatedSignon.aspx. Um die IdpInitiatedSignOn der AD FS-Eigenschaften aktivieren muss. Gehen Sie die AD FS-Installation überprüfen
1.  Führen Sie die folgenden Cmdlet auf dem AD FS PowerShell auf aktiviert festgelegt.
    $True Set-AdfsProperties - EnableIdPInitiatedSignonPage 
2.  Von jeder externe Computer Zugriff https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx  
3.  Die AD FS-Seite sollte angezeigt werden wie folgt:

![Test-Anmeldeseite](./media/active-directory-aadconnect-azure-adfs/test1.png)

Bei erfolgreicher Anmeldung bieten es eine Erfolgsmeldung wie folgt:

![Test erfolgreich](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Vorlage für die Bereitstellung von AD FS in Azure

Die Vorlage stellt 6 Computer Setup 2 Domänencontroller AD FS und WAP.

[AD FS in Azure Bereitstellungsvorlage](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Sie können ein vorhandenes virtuelles Netzwerk verwenden oder ein neues VNET beim Bereitstellen dieser Vorlage erstellen. Die verschiedenen Parameter für die Anpassung der Bereitstellung mit der Beschreibung der Verwendung des Parameters im Bereitstellungsprozess sind. 

| Parameter | Beschreibung |
|:--------|:-----|
|Speicherort| Der Bereich Ressourcen in z.B. Osten uns bereitstellen. |
|StorageAccountType| Der Typ des Speicherkontos erstellt|
|VirtualNetworkUsage| Gibt an, ob ein neues virtuelles Netzwerk erstellt werden oder eine bestehende verwenden|
|VirtualNetworkName| Der Name des virtuellen Netzwerks zu erstellen, auf vorhandenen oder neuen virtuellen Netzwerkverwendung obligatorisch|
|VirtualNetworkResourceGroupName| Gibt den Namen der Ressourcengruppe, vorhandene virtuelle Netzwerk befindet. Wenn Sie ein vorhandenes virtuelles Netzwerk verwenden, wird ein erforderlicher Parameter die Bereitstellung die ID des virtuellen Netzwerks finden|
|VirtualNetworkAddressRange| Bereich von neue VNET erforderlich, wenn ein neues virtuelles Netzwerk erstellen|
|InternalSubnetName| Der Name des internen Subnetzes auf beide virtuellen Netzwerk Verwendungsoptionen (neu oder vorhanden) obligatorisch|
|InternalSubnetAddressRange| Bereich des internen Subnetzes enthält die Domänencontroller und ADFS Server obligatorisch ein neues virtuelles Netzwerk erstellen.|
|DMZSubnetAddressRange| Der Adressbereich von dmz Subnetz enthält die Windows-Anwendung-Proxyserver, obligatorisch ein neues virtuelles Netzwerk erstellen.|
|DMZSubnetName| Der Name des internen Subnetzes auf beide virtuellen Netzwerk Verwendungsoptionen (neu oder vorhanden) obligatorisch. |
|ADDC01NICIPAddress| Die interne IP-Adresse des ersten Domänencontrollers diese IP-Adresse des Domänencontrollers wird statisch zugewiesen werden und muss eine gültige IP-Adresse innerhalb des internen Subnetzes|
|ADDC02NICIPAddress| Die interne IP-Adresse des zweiten Domänencontrollers diese IP-Adresse des Domänencontrollers wird statisch zugewiesen werden und muss eine gültige IP-Adresse innerhalb des internen Subnetzes|
|ADFS01NICIPAddress| Die interne IP-Adresse des ersten ADFS-Server diese IP-Adresse statisch zugewiesen wird ADFS-Server und muss eine gültige IP-Adresse innerhalb des internen Subnetzes|
|ADFS02NICIPAddress| Die interne IP-Adresse des zweiten ADFS-Server diese IP-Adresse statisch zugewiesen wird ADFS-Server und muss eine gültige IP-Adresse innerhalb des internen Subnetzes|
|WAP01NICIPAddress| Die interne IP-Adresse des ersten Servers WAP diese IP-Adresse der WAP-Server werden statisch zugewiesen werden und muss eine gültige IP-Adresse der DMZ-Subnetz|
|WAP02NICIPAddress| Die interne IP-Adresse des zweiten WAP-Server diese IP-Adresse statisch zugewiesen, WAP-Server und muss eine gültige IP-Adresse der DMZ-Subnetz|
|ADFSLoadBalancerPrivateIPAddress| Die interne IP-Adresse des ADFS-Lastenausgleich diese IP-Adresse statisch zugewiesen, Lastenausgleich und muss eine gültige IP-Adresse innerhalb des internen Subnetzes|
|ADDCVMNamePrefix| Virtual Machine Präfix für Domänencontroller|
|ADFSVMNamePrefix| Virtual Machine Präfix für ADFS-Server|
|WAPVMNamePrefix| Virtual Machine Präfix für WAP-Server|
|ADDCVMSize| Die Vm-Größe der Domänencontroller|
|ADFSVMSize| Die Vm-Größe der ADFS-Server|
|WAPVMSize| Virtueller Speicher WAP-Server|
|AdminUserName| Der Name des lokalen Administrators der virtuellen Computer|
|AdminPassword| Das Kennwort für das lokale Administratorkonto der virtuellen Computer|

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Verfügbarkeit-Sets](https://aka.ms/Azure/Availability ) 
* [Azure Lastenausgleich](https://aka.ms/Azure/ILB)
* [Interne Lastenausgleich](https://aka.ms/Azure/ILB/Internal)
* [Internet mit Lastenausgleich](https://aka.ms/Azure/ILB/Internet)
* [Speicherkonten](https://aka.ms/Azure/Storage )
* [Azure Virtual Networks](https://aka.ms/Azure/VNet)
* [AD FS und Proxy Hyperlinks](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Nächste Schritte

* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
* [Konfigurieren und Verwalten von Ihrem AD FS mit Azure AD verbinden](active-directory-aadconnectfed-whatis.md)
* [Hohe Verfügbarkeit zwischen geografischen AD FS-Bereitstellung in Azure mit Azure Traffic Manager](active-directory-adfs-in-azure-with-azure-traffic-manager.md)




