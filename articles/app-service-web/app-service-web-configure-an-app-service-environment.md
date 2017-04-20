<properties
    pageTitle="So konfigurieren Sie eine App Service-Umgebung | Microsoft Azure"
    description="Konfiguration, Management und Überwachung der App Service-Umgebungen"
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


# <a name="configuring-an-app-service-environment"></a>Konfigurieren einer App Service-Umgebung #

## <a name="overview"></a>Übersicht ##

Auf einer hohen Ebene besteht aus einer Azure App Service-Umgebung mehrere Hauptkomponenten:

- Compute-Ressourcen, die in der App Service-Umgebung gehosteten Dienst ausführen
- Speicher
- Eine Datenbank
- Ein Classic(V1) oder Ressource Manager(V2) Azure Virtual Network (VNet) 
- Ein Subnetz mit der App Service-Umgebung ausführen

### <a name="compute-resources"></a>Compute-Ressourcen

Sie verwenden die Serverressourcen für vier Ressourcenpools.  Jede App Service-Umgebung (ASE) hat front-Ends und drei mögliche Arbeitskraft Pools. Sie müssen alle drei workerpools--verwenden Sie gegebenenfalls nur können Sie ein oder zwei.

Hosts in Resource Pools (front-Ends und Arbeitskräfte) sind nicht direkt an Mieter. Remote Desktop Protocol (RDP) Sie können nicht verbinden, ihre Bereitstellung ändern oder fungieren als Administrator auf.

Sie können Resource Pool Menge und Größe festlegen. In einer ASE haben Sie vier Optionen, P1 durch P4 beschriftet sind. Details zu diesen Größen und ihre Preise finden Sie in der [App Preisen](../app-service/app-service-value-prop-what-is.md).
Die Menge oder die Größe ändern, wird eine Skalierung aufgerufen.  Nur eine Skalierung kann gleichzeitig ausgeführt werden.

**Frontends**: front-Ends sind HTTP/HTTPS-Endpunkte für apps, die die ASE gehalten werden. Arbeitslasten in front-Ends nicht ausgeführt werden.

- Eine ASE beginnt mit zwei P2s, die für Test-/Arbeitslasten und einfachen Produktions-Workloads. Wir empfehlen P3s für mittlere schwere Produktions-Workloads.
- Bei mittlerer bis hoher Produktionsarbeitslasten empfohlen sind mindestens vier P3s werden ausreichend front-Ends beim Auftreten von Wartungsarbeiten ausführen. Geplante Wartungsaktivitäten bringen Sie eine front-End gleichzeitig. Dadurch insgesamt verfügbare Front-End-Kapazität bei Wartungsarbeiten.
- Sie hinzufügen nicht sofort eine neue Front-End-Instanz. Sie können zwischen 2 bis 3 Stunden bereitgestellt werden.
- Für weitere Skalierung optimieren, sollten Sie Prozentwert der CPU, Speicher in Prozent und aktive Anfragen Metriken für den Front-End-Pool überwachen. Sind die Prozentsätze CPU oder Arbeitsspeicher über 70 % beim P3s ausführen, fügen Sie weitere front-Ends. Wenn der aktiven Anfragen Wert 15.000 20.000 Anfragen pro Frontend Durchschnitt, sollten Sie mehrere front-End hinzufügen. Das Ziel ist zu CPU- und Prozentsätze unter 70 % und aktive Anfragen unter 15.000 Anfragen pro Vorderseite Mittelung enden, wenn P3s ausführen.  

**Mitarbeiter**: Arbeitskräfte, wo Ihre apps tatsächlich ausgeführt werden. Beim Skalieren Ihrer App Service-Pläne, die von Arbeitskräften im Pool zugeordnete Arbeitskraft verwendet.

- Arbeitskräfte können nicht sofort hinzuzufügen. Sie können von 2 bis 3 Stunden bereitstellen, egal wie viele hinzugefügt wird.
- Skalierungsgröße des Compute-Ressourcen für jede Pool dauert 2 bis 3 Stunden pro Domäne aktualisieren. Ein ASE umfasst 20 aktualisierungsdomänen. Skaliert die computegröße Arbeitspool mit 10 Instanzen konnte zwischen 20 und 30 Stunden dauern.
- Wenn Sie die Größe der computeressourcen, die in einer Arbeitskraft ändern, verursachen Sie Kaltstart Apps, die in diesem Arbeitspool ausführen.

Soll am schnellsten Ressourcengröße Berechnen eines Pools Arbeitskraft ändern, keine apps ausgeführt wird:

- Verkleinern Sie die Anzahl der Instanzen auf 0. Es dauert ca. 30 Minuten Instanzen freigeben.
- Wählen Sie die neue computegröße und Anzahl der Instanzen. Von hier aus dauert es 2 bis 3 Stunden.

Benötigen Ihre apps Compute Ressource größer können nicht Sie die vorherige Anleitung nutzen. Statt die Größe des dem Arbeitspool, die diese apps hostet, können anderen Arbeitspool mit der gewünschten Größe füllen und bewegen Ihre apps auf diesem Pool.

- Erstellen Sie die zusätzlichen Instanzen von erforderlichen computegröße in einer anderen Arbeitskraft pool Dies wird von 2 bis 3 Stunden dauern.
- Weisen Sie Ihre App Service-Pläne zu, die apps hosten, die eine größere Größe neu konfigurierten Arbeitspool. Dies ist eine schnelle Operation, die weniger als eine Minute dauern sollte.  
- Verkleinern Sie erste Arbeitspool, wenn diese nicht verwendeten Instanzen nicht mehr benötigen. Dieser Vorgang dauert ca. 30 Minuten.

**Skalierung**: ist eines der Tools, die Ihnen helfen, Ihren Ressourcenverbrauch berechnen verwalten automatische Skalierung. Können automatische Skalierung für Front-End- oder Arbeitskraft. Sie können Elemente wie Ihre Instanzen jeder Pool morgens und abends reduzieren. Oder vielleicht Instanzen unter einen bestimmten Schwellenwert sinkt die Anzahl der Mitarbeiter in einer Arbeitskraft verfügbar sind.

Möchten Sie skalieren Regeln um Compute Resource Pool Metriken und dabei die Bereitstellung erfordert Zeit. Weitere Informationen zu App Service-Umgebungen Skalierung finden Sie unter [Skalieren in einer App Service-Umgebung konfigurieren][ASEAutoscale].

### <a name="storage"></a>Speicher

Jede ASE ist mit 500 GB Speicher konfiguriert. Dieser Speicherplatz ist für alle apps ASE verwendet. Dieser Speicherplatz ist Teil der ASE und derzeit kann nicht umgestellt werden, um Speicherplatz verwenden. Wenn Sie Korrekturen für Ihr virtuelles Netzwerkrouting Sicherheit vornehmen, müssen Sie weiterhin Zugriff auf Azure-Speicher - oder ASE kann nicht ausgeführt werden.

### <a name="database"></a>Datenbank

Die Datenbank enthält Informationen, die zum Definieren der Umgebung sowie Details der apps, die darin arbeiten. Dies gehört zu Abonnements Azure statt. Es ist keine direkte Möglichkeit zum Bearbeiten müssen. Wenn Sie Korrekturen für Ihr virtuelles Netzwerkrouting Sicherheit vornehmen, müssen Sie weiterhin Zugriff auf SQL Azure - oder ASE kann nicht ausgeführt werden.

### <a name="network"></a>Netzwerk

Das VNet mit der ASE kann sein bei der Erstellung der ASE voraus gemacht. Beim Erstellen des Subnetzes während der Erstellung ASE erzwingt ASE in derselben Ressourcengruppe als virtuelles Netzwerk. Benötigen Sie die Ressourcengruppe von der ASE unterscheidet, das VNet verwendet, müssen Sie Ihre ASE Resource Manager Vorlage erstellen.

Es gibt einige Einschränkungen auf das virtuelle Netzwerk für eine ASE:
 
- Das virtuelle Netzwerk muss ein regionaler virtuelles Netzwerk.
- Es muss ein Subnetz mit 8 oder mehr Adressen, wo die ASE bereitgestellt.
- Nachdem ein Subnetz zum Hosten einer ASE verwendet wird, kann der Adressbereich des Subnetzes geändert werden. Aus diesem Grund empfiehlt das Subnetz mindestens 64 Adressen alle ASE Erweiterungskapazität enthält.
- Es kann nichts in dem Subnetz, aber ASE.

Im Gegensatz zu den gehosteten Dienst, ASE [virtuelles Netzwerk] enthält[ virtualnetwork] und Subnetz Benutzer gesteuert.  Sie können Ihr virtuelle Netzwerk über die virtuellen Netzwerk-Benutzeroberfläche oder PowerShell verwalten.  Eine ASE kann in einer klassischen oder Resource Manager VNet bereitgestellt werden.  Portal und API-Funktionen sind geringfügig zwischen Classic und Ressourcenmanager VNets jedoch die ASE genügt.

VNet, mit der eine ASE hosten, können entweder privaten RFC1918 IP-Adressen oder IP-Adressbereich verwenden.  Möchten Sie einen IP-Adressbereich zu verwenden, die nicht durch RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) müssen Sie Ihre VNet und Subnetz die ASE vor ASE-Erstellung verwendet werden.

Da diese Funktion Azure App Service in Ihr virtuelles Netzwerk platziert, bedeutet dies, dass Ihre apps in Ihrem ASE gehostet werden jetzt auf Ressourcen zugreifen können, die über ExpressRoute oder zwischen Standorten virtuelle private Netzwerke (VPNs) direkt verfügbar sind. Apps, die in Ihrer App Service-Umgebung erfordern keine zusätzliche Netzwerkfunktionen für das virtuelle Netzwerk verfügbaren Ressourcen zugreifen, die Ihre App Service-Umgebung befindet. Dies bedeutet, dass Sie VNET-Integration oder Hybridverbindungen Ressourcen in bzw. an das virtuelle Netzwerk zu verwenden müssen. Dennoch können beiden Funktionen zum Zugriff auf Ressourcen in Netzwerken, die nicht mit dem virtuellen Netzwerk verbunden sind.

VNET-Integration können Sie ein virtuelles Netzwerk integrieren, die in Ihrem Abonnement jedoch nicht an das virtuelle Netzwerk Ihre ASE ist verbunden. Auch können Hybrid-Verbindungen auf Ressourcen zugreifen, die in anderen Netzwerken, wie Sie normalerweise.  

Wenn Sie Ihr virtuelle Netzwerk mit einem ExpressRoute VPN konfiguriert haben, beachten Sie einige routing muss, die eine ASE. Einige benutzerdefinierte Route (UDR)-Konfigurationen, die nicht mit einer ASE sind. Weitere Informationen zum Ausführen einer ASE in ein virtuelles Netzwerk mit ExpressRoute, finden Sie unter [Ausführen einer App Service-Umgebung in einem virtuellen Netzwerk mit ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Sichern des eingehenden Datenverkehrs

Es gibt zwei primäre Methoden des eingehenden Datenverkehr an Ihre ASE.  Sie können Network Security Groups(NSGs) steuern, welche IP Adressen möglich Ihre ASE hier beschriebenen [Datenverkehr in einer App Service-Umgebung steuern](app-service-app-service-environment-control-inbound-traffic.md) und die ASE mit einer internen laden Balancer(ILB) konfigurieren.  Diese Funktionen können auch zusammen verwendet werden, Zugriff NSGs, Ihre ILB ASE verwenden soll.

Beim Erstellen einer ASE erstellt er eine VIP in Ihrem VNet.  Es gibt zwei VIP-Arten, externe und interne.  Beim Erstellen einer ASE mit externen VIP können Ihre apps in Ihrem ASE über eine Internet-routbare IP-Adresse zugegriffen werden. Wenn Sie intern die ASE auswählen werden mit einem ILB konfiguriert werden und werden nicht direkt Internet zugegriffen werden kann.  Ein ILB ASE benötigt noch einen externen VIP aber dient nur für Azure Management und Wartung.  

Während ILB ASE erstellen Sie die Subdomäne ILB ASE verwendet und müssen DNS für die untergeordnete Domäne verwalten, die Sie angeben.  Da Sie den Subdomainnamen auch das Zertifikat für den HTTPS-Zugriff verwalten festlegen müssen.  Nach der Erstellung ASE werden Sie aufgefordert, das Zertifikat.  Um Informationen zum Erstellen und Verwenden einer ILB ASE mehr [mithilfe einer internen Lastenausgleich mit einer App Service-Umgebung][ILBASE]. 


## <a name="portal"></a>Portal

Verwalten und Überwachen Ihrer App Service-Umgebung mithilfe der Benutzeroberfläche in Azure-Portal. Haben Sie eine ASE, sind Sie wahrscheinlich das App Service-Symbol der Sidebar. Dieses Symbol wird zum Darstellen von App Service-Umgebungen im Azure-Portal:

![Symbol für App Service-Umgebung][1]

Die Benutzeroberfläche öffnen, die alle Ihre App Service-Umgebungen aufgelistet, Sie können das Symbol oder das Chevron (">" Symbol) am unteren Rand der Seitenleiste App Service-Umgebungen auswählen. Durch Auswahl einer der aufgeführten Asen, öffnen Sie die Benutzeroberfläche zum Überwachen und verwalten.

![Benutzeroberfläche für die Überwachung und Verwaltung Ihrer App Service-Umgebung][2]

Diese erste Blade zeigt einige Eigenschaften der ASE mit metrischen Diagramm pro Ressourcenpool. Einige Eigenschaften, die im Block **Essentials** angezeigt sind auch das Blatt geöffnet werden, die ihm zugeordneten Hyperlinks. Beispielsweise können Sie den Namen des **Virtuellen Netzwerks** auf der Benutzeroberfläche das virtuelle Netzwerk aus Ihrem ASE öffnen auswählen. **App Service-Pläne** und **Apps** öffnen klingen, in denen diese Elemente aufgeführt, die in Ihrem ASE.  

### <a name="monitoring"></a>Überwachung

Die Diagramme können verschiedene Performance-Werte jeder Resource Pool. Für den Front-End-Pool können Sie die durchschnittliche CPU- und überwachen. Arbeitskraft-Pools können Sie überwachen die Menge verwendet wird und die verfügbare Menge.

Mehrere nutzen App können Pläne der Arbeitskräfte in einer Arbeitskraft. Die Arbeitslast ist nicht auf die gleiche Weise verteilt, wie Front-End-Server so CPU und Speicherverwendung nicht viel Informationen. Es ist wichtiger wie viele Arbeitskräfte, die Sie verwendet haben und sind verfügbar, besonders, wenn Sie dieses System für andere verwalten.  

Sie können auch alle Metriken verfolgt werden können in den Diagrammen Alarme einrichten. Einrichten von Alerts hier funktioniert genauso wie in App Service. Sie können eine Warnung Teil UI **Alerts** oder Drilldown Metrik Benutzeroberfläche und **Warnung hinzufügen**auswählen festlegen.

![Metriken Benutzeroberfläche][3]

Die soeben erläuterten Metriken sind Metriken App Service-Umgebung. Gibt Kriterien, die auf Ebene der App Plan verfügbar sind. Dies ist Überwachung CPU- und sinnvoll macht.

Ein ASE sind App Service-Pläne dedizierten App Service. Das bedeutet, dass nur apps, die auf den Hosts zugeordnet sind App Serviceplan apps, App Service-Plan ausgeführt werden. Für Details auf Ihren App Service-Plan bringen Sie Ihre App Service-Plan von Listen in ASE-Benutzeroberfläche oder **Durchsuchen App Service-Pläne** (alle Listen).   

### <a name="settings"></a>Einstellungen

In ASE-Blade ist **ein Einstellungen, die mehrere wichtige Funktionen enthält** :

**Einstellung** > **Eigenschaften**: Blatt **Einstellungen** automatisch geöffnet, wenn die ASE-Blade bringen. Am Anfang ist **Eigenschaften**. Zählen der Elemente hier in **Essentials**finden Sie redundant, aber sehr nützlich ist **Virtuelle IP-Adresse**als auch **Ausgehende IP-Adressen**.

![Blatt Einstellungen und Eigenschaften][4]

**Einstellung** > **Adressen**: beim Erstellen einer Anwendung IP-Secure Sockets Layer (SSL) in Ihrem ASE benötigen Sie eine SSL IP-Adresse. Um eine zu erhalten, benötigt Ihr ASE SSL IP Adressen, die er besitzt, die zugewiesen werden können. Beim Erstellen eine ASE hat eine SSL IP-Adresse für diesen Zweck jedoch weitere hinzufügen. Berechnet einen für SSL zusätzliche IP Adressen Siehe [App Servicepreise] [ AppServicePricing] (im Abschnitt SSL-Verbindungen). Der zusätzliche Preis ist IP SSL-Preis.

**Einstellung** > **Front-End-Pool** / **Arbeitskraft Pools**: jeder dieser Resource Pool-Blades bietet die Möglichkeit, Informationen im Ressourcenpool neben Steuerelemente, Ressourcenpool vollständig skalieren.  

Base-Blade für jede Ressourcenpool enthält ein Diagramm mit Metriken für den Ressourcenpool. Wie Diagramme aus dem Blade ASE Sie können ins Diagramm mit Warnungen wie gewünscht einrichten. Festlegen einer Warnung ASE Blatt für einen bestimmten Resource Pool ist dasselbe wie aus dem Ressourcenpool. Arbeitskraft-Pool **Settings** Blatt haben Sie Zugriff auf alle Apps oder App Service-Pläne, die in diesem Arbeitspool ausführen.

![Arbeitskraft-Pool Einstellungsbenutzeroberfläche][5]

### <a name="portal-scale-capabilities"></a>Portal Skalierung Funktionen  

Es gibt drei Dezimalstellen Vorgänge:

- Ändern der Anzahl der IP-Adressen in ASE, die für IP-SSL-Verwendung verfügbar sind.
- Ändern der Größe der computeressource, die in einem Ressourcenpool verwendet.
- Ändern der Anzahl der Serverressourcen, die entweder manuell oder über automatische Skalierung in einem Ressourcenpool verwendet werden.

Im Portal werden drei Arten steuern, wie viele Server, die in Ressourcen-Pools:

- Eine Skalierung aus dem Hauptmenü ASE Blade oben. Sie können mehrere skaliert Konfiguration in der Front-End- und Arbeitskraft ändern. Sie sind alle in einem Vorgang angewendet.
- Eine manuelle Skalierung Ressource Pool **Skalierung** Blatt unter **Einstellungen**.
- Skalierung von einzelnen Resource Pool **Skalierung** Blade eingerichtet.

Um die Skalierung auf die ASE verwenden, ziehen Sie den Schieberegler auf die Menge und speichern Sie. Diese Benutzeroberfläche unterstützt auch ändern.  

![Skalierung Benutzeroberfläche][6]

Um die manuelle oder automatische Skalierung Funktionen in einem bestimmten Resource Pool verwenden, rufen Sie die **Einstellungsseite** > **Front-End-Pool** / **Arbeitskraft Pools** entsprechend. Öffnen Sie den Pool, die Sie ändern möchten. **Standardeinstellungen**zum > **Skalieren** oder **Einstellungen** > **Skalieren**. **Dezentrales** Blade können Sie Instanz Menge steuern. **Skalieren** können Ressourcen Größe.  

![Einstellungen UI][7]

## <a name="fault-tolerance-considerations"></a>Fehlertoleranz-Hinweise

Sie können eine App Service-Umgebung um bis zu 55 insgesamt Serverressourcen verwenden. Dieser 55 Compute-Ressourcen können nur 50 Arbeitslasten verwendet werden. Dies hat zwei Seiten. Es ist mindestens 2 Front-End-Serverressourcen.  Somit bis zu 53 Arbeitspool Reservierung unterstützen. Um Fehlertoleranz bereitzustellen, müssen Sie eine zusätzliche computeressource, die nach den folgenden Regeln zugeordnet:

- Jede Arbeitspool benötigt mindestens 1 Weitere Compute-Ressourcen, die nicht zur eine Arbeitslast zugewiesen werden.
- Wenn Compute-Ressourcen in einer Arbeitskraft einen bestimmten Wert überschreitet, ist eine andere computeressource für Fehlertoleranz erforderlich. Dies ist nicht der Fall im Front-End-Pool.

Innerhalb eines einzelnen Worker-Pools sind Vorschriften Fehlertoleranz für einen angegebenen Wert X Ressourcen eine Arbeitspool zugeordnet:

- Wenn X zwischen 2 und 20 ist, ist die nutzbare computeressourcen, die für Arbeitslasten 1.
- X ist 21 bis 40 die Menge nutzbaren computeressourcen, die für Arbeitslasten 2.
- X ist 41 bis 53 die Menge nutzbaren computeressourcen, die für Arbeitslasten x-3.

Minimale Platzbedarf hat 2 Front-End-Servern und 2 Arbeitnehmer.  Mit Aussagen dann sind hier Beispiele verdeutlichen:  

- Haben Sie 30 Mitarbeiter in einem Pool kann 28 Arbeitslasten verwendet werden.
- Haben Sie 2 Arbeitnehmer in einem Pool kann 1 Arbeitslasten verwendet werden.
- Haben Sie 20 Arbeitnehmern in einem Pool kann 19 Arbeitslasten verwendet werden.  
- Haben Sie 21 Arbeitskräfte in einem Pool kann dann noch nur 19-Arbeitslasten verwendet werden.  

Fehlertoleranz Aspekt ist wichtig, aber es Bedenken über bestimmte Schwellenwerte skalieren müssen. Wenn Sie mehr Kapazität von 20 Instanzen hinzufügen möchten, fahren Sie mit 22 oder höher da 21 nicht mehr Kapazität hinzufügen. Das gleiche gilt geht über 40, 42 wird die nächste Nummer, die Kapazität hinzugefügt.  

## <a name="deleting-an-app-service-environment"></a>Löschen einer App Service-Umgebung ##

Wenn Sie eine App Service-Umgebung löschen möchten, verwenden Sie die Aktion **Löschen** einfach oben Blade App Service-Umgebung. Wenn Sie dies tun, werden Sie aufgefordert, Ihre App Service-Umgebung zu bestätigen, dass Sie wirklich dazu geben. Beachten Sie, dass beim Löschen einer App Service-Umgebung sowie der Inhalt darin löschen.  

![Löschen einer App Service-Umgebung Benutzeroberfläche][9]  

## <a name="getting-started"></a>Erste Schritte

Zunächst mit App-Dienst finden Sie unter [Erstellen einer App Service-Umgebung](app-service-web-how-to-create-an-app-service-environment.md).

Weitere Informationen über die Plattform Azure App Service finden Sie in [Azure App Service](../app-service/app-service-value-prop-what-is.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
