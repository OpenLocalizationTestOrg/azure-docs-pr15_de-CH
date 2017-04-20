<properties
   pageTitle="Erstellen Sie ein Gateway über das Portal | Microsoft Azure"
   description="Erstellen Sie ein Gateway mithilfe der Portalwebsite"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-an-application-gateway-by-using-the-portal"></a>Erstellen Sie ein Gateway mit dem portal

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-gateway-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-gateway-arm.md)
- [Klassische Azure PowerShell](application-gateway-create-gateway.md)
- [Azure-Ressourcen-Manager-Vorlage](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure Application Gateway ist eine Layer 7-Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind. Application Gateway bietet viele Application Delivery Controller (ADC) Funktionen einschließlich HTTP-Lastenausgleich cookiebasierte sitzungsaffinität, Secure Sockets Layer (SSL) ausgelagert, benutzerdefinierten Zustand Prüfpunkte, Unterstützung für mehrere Standorte und viele andere. Eine vollständige Liste der unterstützten Funktionen finden Sie auf [Gateway Anwendungsübersicht](application-gateway-introduction.md)

## <a name="scenario"></a>Szenario

In diesem Szenario erfahren Sie, wie ein Gateway mit Azure-Portal erstellen.

Dieses Szenario wird:

- Erstellen Sie mittlere Application Gateway mit zwei Instanzen.
- Erstellen Sie ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET mit einem 10.0.0.0/16 reservierten CIDR-Block.
- Erstellen Sie ein Subnetz namens Appgatewaysubnet, die 10.0.0.0/28 als CIDR-Blocks verwendet.
- Konfigurieren eines Zertifikats für SSL-Verschiebung.

![Szenario-Beispiel][scenario]

>[AZURE.IMPORTANT] Zusätzliche Konfiguration des Application Gateway einschließlich benutzerdefinierte Prüfpunkte, Back-End-Pool Adressen und zusätzliche Regeln nach Anwendungsgateway konfiguriert und nicht während der ursprünglichen Bereitstellung konfiguriert sind.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Azure Application Gateway benötigt ein eigenen Subnetz. Beim Erstellen eines virtuellen Netzwerks sicherzustellen Sie, dass genügend Adressraum mehrere Subnetze haben. Wenn Sie einen Gateway zu einem Subnet bereitstellen, können nur weitere Gateways im Subnetz hinzugefügt werden.

## <a name="create-the-application-gateway"></a>Application Gateway erstellen

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu der Azure-Portal, klicken Sie auf **neu** > **Netzwerk** > **Application Gateway**

![Application Gateway erstellen][1]

### <a name="step-2"></a>Schritt 2

Anschließend füllen Sie Basisinformationen Application Gateway. Klicken Sie auf **OK**

Informationen, die für die Standardeinstellungen sind:

- **Name** - der Name der Application Gateway.
- **Tier** - Dies ist die Ebene des Anwendung-Gateways. Zwei Stufen stehen **WAF** und **Standard**. WAF kann das Web Application Firewall.
- **Größe der SKU** - diese Einstellung entspricht das Application Gateway sind Optionen (**klein**, **Mittel**und **Groß**). *Kleine ist nicht verfügbar, wenn WAF Ebene ausgewählt ist*
- **Anzahl der Instanzen** - die Anzahl der Instanzen, dieser Wert sollte eine Zahl zwischen 2 und 10.
- **Ressourcengruppe** - Ressourcengruppe zu Application Gateway kann eine Ressourcengruppe oder eine neue.
- **Standort** - Bereich für Application Gateway ist derselbe Speicherort in der Ressourcengruppe. *Der Speicherort ist wichtiger als das virtuelle Netzwerk und öffentliche IP-Adresse muss in demselben Speicherort wie das Gateway*.

![Blade mit Standardeinstellungen][2]

>[AZURE.NOTE] Zu Testzwecken kann ein Instanzenzahl von 1 ausgewählt werden. Es ist wichtig, dass jede Instanz unter zwei Instanzen fällt nicht unter die SLA und werden daher nicht empfohlen. Kleine Gateways für Test- und nicht für die Produktion verwendet werden sollen.

### <a name="step-3"></a>Schritt 3

Die Standardeinstellungen definiert, besteht der nächste Schritt definiert das virtuelle Netzwerk verwendet werden. Das virtuelle Netzwerk beinhaltet die Anwendung Application Gateway für Lastenausgleich.

Klicken Sie auf **Wählen Sie ein virtuelles Netzwerk** um das virtuelle Netzwerk zu konfigurieren.

![Blade mit Einstellungen für Application gateway][3]

### <a name="step-4"></a>Schritt 4

*Virtuelles Netzwerk wählen Sie* Blatt klicken Sie auf **Neu erstellen**

Zwar nicht in diesem Szenario erläutert, konnte ein vorhandenes virtuelles Netzwerk ausgewählt werden.  Ein vorhandenes virtuelles Netzwerk verwendet wird, ist es wichtig zu wissen, dass das virtuelle Netzwerk eine leere Subnetz oder einem Subnetz nur Gateway Anwendungsressourcen verwendet werden.

![virtuelles Netzwerk Blade auswählen][4]

### <a name="step-5"></a>Schritt 5

Füllen Sie die Informationen in das Blade **Virtuelles Netzwerk erstellen** gemäß der obigen Beschreibung des [Szenarios](#scenario) .

![Erstellen Sie virtuelles Netzwerk Blade mit Angaben][5]

### <a name="step-6"></a>Schritt 6

Nachdem das virtuelle Netzwerk erstellt wurde, besteht der nächste Schritt die Front-End-IP-Adresse Anwendungsgateway definieren. An diesem Punkt ist die Wahl zwischen einer öffentlichen oder einer privaten IP-Adresse der Front-End. Die Wahl hängt davon ab, ob die Anwendung Internet ist vor oder interne nur. Dieses Szenario geht eine öffentliche IP-Adresse. Wählen Sie eine private IP-Adresse kann **Private** Schaltfläche geklickt werden. Eine automatisch zugewiesene IP-Adresse ausgewählt, oder klicken Sie das Kontrollkästchen **Auswählen einer bestimmten privaten IP-Adresse** , um einen manuell eingeben.

### <a name="step-7"></a>Schritt 7

Klicken Sie auf **Wählen Sie eine öffentliche IP-Adresse**. Ist eine vorhandene öffentliche IP-Adresse zu diesem Zeitpunkt ausgewählt werden können, in diesem Szenario erstellen Sie eine neue öffentliche IP-Adresse. Klicken Sie auf **neu erstellen**

![Wählen Sie öffentliche IP-Adresse blade][6]

### <a name="step-8"></a>Schritt 8

Anschließend geben Sie der öffentlichen IP-Adresse einen Namen und klicken Sie auf **OK**

![Erstellen Sie öffentliche IP-Adresse blade][7]

### <a name="step-9"></a>Schritt 9

Die letzte Einstellung konfigurieren, wenn die Listenerkonfiguration ein Gateway erstellen ist.  Wenn **http** verwendet wird, ist nichts mehr um zu konfigurieren und **OK** geklickt werden kann. Verwendung von **Https** sind weitere Konfigurationsschritte erforderlich.

Verwendung von **Https**ist ein Zertifikat erforderlich. Der private Schlüssel des Zertifikats ist erforderlich, damit eine PFX-Datei Exportieren des Zertifikats und des Kennworts erforderlich.

### <a name="step-10"></a>Schritt 10

Klicken Sie auf **HTTPS**, klicken Sie auf **das Ordnersymbol neben **Hochladen PFX Zertifikat** Textbox** .
Navigieren Sie zu PFX-Zertifikatsdatei auf Ihrem System. Ausgewählt ist, geben Sie dem Zertifikat einen Namen, und geben Sie das Kennwort für die PFX-Datei.

Danach klicken Sie auf **OK** , um die Einstellungen für Application Gateway zu überprüfen.

![Listener-Konfigurationsabschnitt auf Settings][9]

### <a name="step-11"></a>Schritt 11

Überprüfen Sie die Seite Zusammenfassung, und klicken Sie auf **OK**.  Jetzt wird das Application Gateway eingereiht und erstellt.

### <a name="step-12"></a>Schritt 12

Erstellte Anwendungsgateway navigieren Sie im Portal weiterhin Configuration Application Gateway.

![Application Gateway Ressourcenansicht][10]

Diese Schritte erstellen Basisanwendungsgruppe Gateway mit Standardeinstellungen für Listener, Pool-Back-End, Back-End-HTTP-Einstellungen und Regeln. Sie können diese Einstellungen die Bereitstellung anpassen, wenn die Bereitstellung erfolgreich war.

## <a name="next-steps"></a>Nächste Schritte

In diesem Szenario wird ein Standardgateway für die Anwendung. Die nächsten Schritte sind das Application Gateway Konfigurieren von Pool-Mitglieder hinzufügen, Einstellungen ändern und Anpassen der Regeln im Gateway ordnungsgemäß funktionieren.

Erfahren Sie, wie benutzerdefinierte Health Prüfpunkte erstellen [benutzerdefinierte Integritätstest erstellen](application-gateway-create-probe-portal.md)

Erfahren Sie mehr zu konfigurieren SSL-Abladung teure SSL-Entschlüsselung von Webserver auf [Konfigurieren SSL-Verschiebung](application-gateway-ssl-portal.md)

Erfahren Sie, wie Ihr [Web Application Firewall](application-gateway-webapplicationfirewall-overview.md) eine Anwendung Gateway schützen.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
