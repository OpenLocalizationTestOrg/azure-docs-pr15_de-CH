<properties
   pageTitle="Erstellen Sie ein Gateway mit Web Application Firewall über das Portal | Microsoft Azure"
   description="Erstellen Sie ein Gateway mit Web Application Firewall mithilfe der Portalwebsite"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
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

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>Erstellen Sie ein Gateway mit Web Application Firewall mithilfe der Portalwebsite

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-web-application-firewall-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-web-application-firewall-powershell.md)

Die Web Application Firewall (AAF) in Azure Application Gateway verhindert allgemeine webbasierten Angriffen wie SQL Injection, Cross-Site scripting-Angriffe und Session Hijacking ASP.NET-Webanwendungen. Anwendung schützt viele OWASP Top 10 häufigen Web Sicherheitslücken.

Azure Application Gateway ist eine Layer 7-Lastenausgleich. Es bietet Failover, Performance-routing HTTP-Anfragen zwischen verschiedenen Servern, ob sie in der Cloud oder lokal sind.
Anwendung bietet viele Application Delivery Controller (ADC)-Features einschließlich HTTP-Lastenausgleich, cookiebasierte sitzungsaffinität, Secure Sockets Layer (SSL) ausgelagert, benutzerdefinierten Zustand Prüfpunkte, Unterstützung für mehrere Standorte und viele andere.
Eine vollständige Liste der unterstützten Funktionen finden Sie auf [Gateway Anwendungsübersicht](application-gateway-introduction.md)

## <a name="scenarios"></a>Szenarien

In diesem Artikel werden zwei Szenarien:

Im ersten Szenario erfahren Sie [ein vorhandenes Gateway Web Application Firewall hinzufügen](#add-web-application-firewall-to-an-existing-application-gateway).

Im zweiten Szenario lernen Sie [ein Gateway mit Web Application Firewall erstellen](#create-an-application-gateway-with-web-application-firewall)

![Szenario-Beispiel][scenario]

>[AZURE.NOTE] Zusätzliche Konfiguration des Application Gateway einschließlich benutzerdefinierte Prüfpunkte, Back-End-Pool Adressen und zusätzliche Regeln nach Anwendungsgateway konfiguriert und nicht während der ursprünglichen Bereitstellung konfiguriert sind.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Azure Application Gateway benötigt ein eigenen Subnetz. Beim Erstellen eines virtuellen Netzwerks sicherzustellen Sie, dass genügend Adressraum mehrere Subnetze haben. Wenn Sie einen Gateway zu einem Subnet bereitstellen, können nur weitere Gateways im Subnetz hinzugefügt werden.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Ein Gateway der vorhandenen Web Application Firewall hinzufügen

Dieses Szenario wird ein vorhandenes Gateway Web Application Firewall im Schutzmodus Unterstützung aktualisiert.

### <a name="step-1"></a>Schritt 1

Zum Azure-Portal, wählen Sie eine vorhandene Anwendungsgateway.

![Application Gateway erstellen][1]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf **Konfiguration** und aktualisieren Sie Standardgatewayeinstellungen Anwendung. Klicken Sie auf **Speichern**

Das Aktualisieren ein vorhandenen Anwendung Gateways unterstützen Web Application Firewall sind:

- **Tier** - Ebene ausgewählt muss **WAF** Web Application Firewall unterstützt werden.
- **Größe SKU** - diese Einstellung entspricht der Application Gateway mit Web Application Firewall sind Optionen (**Mittel** und **Groß**).
- **Firewall-Status** - diese Einstellung deaktiviert oder Web Application Firewall aktiviert.
- **Firewallmodus** - diese Einstellung ist wie Web Application Firewall mit böswilliger Datenverkehr. **Erkennungsmodus** protokolliert nur Ereignisse, **Schutzmodus** protokolliert Ereignisse und böswilligen Datenverkehr beendet.

![Blade mit Standardeinstellungen][2]

>[AZURE.NOTE] Um Web Application Firewallprotokolle, Diagnose muss aktiviert und ApplicationGatewayFirewallLog ausgewählt. Zu Testzwecken kann ein Instanzenzahl von 1 ausgewählt werden. Es ist wichtig, dass jede Instanz unter zwei Instanzen fällt nicht unter die SLA und werden daher nicht empfohlen. Kleine Gateways sind nicht verfügbar, wenn Web Application Firewall verwenden.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Erstellen Sie ein Gateway mit Web Application firewall

Dieses Szenario wird:

- Erstellen Sie mittlere Web Application Firewall Application Gateway mit zwei Instanzen.
- Erstellen Sie ein virtuelles Netzwerk mit dem Namen AdatumAppGatewayVNET mit einem 10.0.0.0/16 reservierten CIDR-Block.
- Erstellen Sie ein Subnetz namens Appgatewaysubnet, die 10.0.0.0/28 als CIDR-Blocks verwendet.
- Konfigurieren eines Zertifikats für SSL-Verschiebung.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu der Azure-Portal, klicken Sie auf **neu** > **Netzwerk** > **Application Gateway**

![Application Gateway erstellen][1-1]

### <a name="step-2"></a>Schritt 2

Anschließend füllen Sie Basisinformationen Application Gateway. Achten Sie darauf, dass **WAF** als die Ebene auswählen. Klicken Sie auf **OK**

Informationen, die für die Standardeinstellungen sind:

- **Name** - der Name der Application Gateway.
- **Tier** - Tier Application Gateway zur Verfügung stehen (**Standard-** und **WAF**). Web Application Firewall ist nur in der WAF verfügbar.
- **Größe der SKU** - diese Einstellung entspricht das Application Gateway sind Optionen (**Mittel** und **Groß**).
- **Anzahl der Instanzen** - die Anzahl der Instanzen, dieser Wert sollte eine Zahl zwischen **2** und **10**.
- **Ressourcengruppe** - Ressourcengruppe zu Application Gateway kann eine Ressourcengruppe oder eine neue.
- **Standort** - Bereich für Application Gateway ist derselbe Speicherort in der Ressourcengruppe. *Der Speicherort ist wichtiger als das virtuelle Netzwerk und öffentliche IP-Adresse muss in demselben Speicherort wie das Gateway*.

![Blade mit Standardeinstellungen][2-2]

>[AZURE.NOTE] Zu Testzwecken kann ein Instanzenzahl von 1 ausgewählt werden. Es ist wichtig, dass jede Instanz unter zwei Instanzen fällt nicht unter die SLA und werden daher nicht empfohlen. Kleine Gateways werden Webanwendungsszenarien Firewall nicht unterstützt.

### <a name="step-3"></a>Schritt 3

Die Standardeinstellungen definiert, besteht der nächste Schritt definiert das virtuelle Netzwerk verwendet werden. Das virtuelle Netzwerk beinhaltet die Anwendung Application Gateway für Lastenausgleich.

Klicken Sie auf **Wählen Sie ein virtuelles Netzwerk** um das virtuelle Netzwerk zu konfigurieren.

![Blade mit Einstellungen für Application gateway][3]

### <a name="step-4"></a>Schritt 4

**Virtuelles Netzwerk wählen Sie** Blatt klicken Sie auf **Neu erstellen**

Zwar nicht in diesem Szenario erläutert, konnte ein vorhandenes virtuelles Netzwerk ausgewählt werden.  Ein vorhandenes virtuelles Netzwerk verwendet wird, ist es wichtig zu wissen, dass das virtuelle Netzwerk eine leere Subnetz oder einem Subnetz nur Gateway Anwendungsressourcen verwendet werden.

![virtuelles Netzwerk Blade auswählen][4]

### <a name="step-5"></a>Schritt 5

Füllen Sie die Informationen in das Blade **Virtuelles Netzwerk erstellen** gemäß der obigen Beschreibung des [Szenarios](#scenario) .

![Erstellen Sie virtuelles Netzwerk Blade mit Angaben][5]

### <a name="step-6"></a>Schritt 6

Nachdem das virtuelle Netzwerk erstellt wurde, besteht der nächste Schritt die Front-End-IP-Adresse Anwendungsgateway definieren. An diesem Punkt ist die Wahl zwischen einer öffentlichen oder einer privaten IP-Adresse der Front-End. Die Wahl hängt davon ab, ob die Anwendung Internet ist vor oder interne nur. Dieses Szenario geht eine öffentliche IP-Adresse. Wählen Sie eine private IP-Adresse kann **Private** Schaltfläche geklickt werden. Eine automatisch zugewiesene IP-Adresse ausgewählt, oder klicken Sie das Kontrollkästchen **Auswählen einer bestimmten privaten IP-Adresse** , um einen manuell eingeben.

Klicken Sie auf **Wählen Sie eine öffentliche IP-Adresse**. Ist eine vorhandene öffentliche IP-Adresse zu diesem Zeitpunkt ausgewählt werden können, in diesem Szenario erstellen Sie eine neue öffentliche IP-Adresse. Klicken Sie auf **neu erstellen**

![Wählen Sie öffentliche IP-Adresse blade][6]

### <a name="step-7"></a>Schritt 7

Anschließend geben Sie der öffentlichen IP-Adresse einen Namen und klicken Sie auf **OK**

![Erstellen Sie öffentliche IP-Adresse blade][7]

### <a name="step-8"></a>Schritt 8

Anschließend richten Sie die Listenerkonfiguration.  Wenn **http** verwendet wird, ist nichts mehr um zu konfigurieren und **OK** geklickt werden kann. Verwendung von **Https** sind weitere Konfigurationsschritte erforderlich.

Verwendung von **Https**ist ein Zertifikat erforderlich. Der private Schlüssel des Zertifikats ist erforderlich, damit eine PFX-Datei Exportieren des Zertifikats muss angegeben werden und das Kennwort für die Datei.

Klicken Sie auf **HTTPS**, klicken Sie auf **das Ordnersymbol neben **Hochladen PFX Zertifikat** Textbox** .
Navigieren Sie zu PFX-Zertifikatsdatei auf Ihrem System. Ausgewählt ist, geben Sie dem Zertifikat einen Namen, und geben Sie das Kennwort für die PFX-Datei.

Danach klicken Sie auf **OK** , um die Einstellungen für Application Gateway zu überprüfen.

![Listener-Konfigurationsabschnitt auf Settings][8]

### <a name="step-9"></a>Schritt 9

Konfigurieren der **WAF** bestimmten.

- **Firewall-Status** - diese Einstellung schaltet WAF ein oder aus.
- **Firewallmodus** – diese Einstellung bestimmt, dass böswilliger Datenverkehr WAF Aktionen übernimmt. Bei **Erkennung** wird nur Datenverkehr protokolliert.  Bei **Prevention** Datenverkehr protokolliert und mit 403 Unauthorized beendet.

![Web Application Firewall settings][9]

### <a name="step-10"></a>Schritt 10

Überprüfen Sie die Seite Zusammenfassung, und klicken Sie auf **OK**.  Jetzt wird das Application Gateway eingereiht und erstellt.

### <a name="step-12"></a>Schritt 12

Erstellte Anwendungsgateway navigieren Sie im Portal weiterhin Configuration Application Gateway.

![Application Gateway Ressourcenansicht][10]

Diese Schritte erstellen Basisanwendungsgruppe Gateway mit Standardeinstellungen für Listener, Pool-Back-End, Back-End-HTTP-Einstellungen und Regeln. Sie können diese Einstellungen die Bereitstellung anpassen, wenn die Bereitstellung erfolgreich war

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum Konfigurieren des Diagnoseprotokolls erkannt oder verhindert werden Ereignisse mit Web Application Firewall auf [Application Gateway Diagnose](application-gateway-diagnostics.md) protokollieren

Erfahren Sie, wie benutzerdefinierte Health Prüfpunkte erstellen [benutzerdefinierte Integritätstest erstellen](application-gateway-create-probe-portal.md)

Erfahren Sie mehr zu konfigurieren SSL-Abladung teure SSL-Entschlüsselung von Webserver auf [Konfigurieren SSL-Verschiebung](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png