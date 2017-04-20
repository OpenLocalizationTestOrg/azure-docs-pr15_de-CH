<properties
   pageTitle="Konfigurieren Sie ein Gateway zum Hosten mehrerer Websites in Azure-Portal vorhandenen | Microsoft Azure"
   description="Diese Seite enthält Informationen zum Konfigurieren eines vorhandenen Gateways Azure-Anwendung zum Hosten mehrere ASP.NET-Webanwendungen auf dem gleichen Gateway mit Azure-Portal."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Konfigurieren einer vorhandenen Anwendung Gateways für mehrere ASP.NET-Webanwendungen hosten

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-multisite-portal.md)
- [Azure Ressourcenmanager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

Mehrere Site hostet, können Sie mehrere Web-Anwendung auf dem gleichen Application Gateway bereitstellen. Es basiert auf der Host-Header in der eingehenden HTTP-Anforderung bestimmen die Listener Datenverkehr erhalten. Der Listener leitet Datenverkehr an den entsprechenden Back-End-Pool Regeln Definition des Gateways konfiguriert. In ASP.NET-Webanwendungen SSL aktiviert verwendet Application Gateway Server Name Angabe (SNI) Erweiterung auswählen den richtigen Listener für Web-Verkehr. Eine häufige Verwendung mehrerer Websitehost wird Saldo Anfragen für andere Webdomänen zu anderen Back-End-Server geladen. Entsprechend können mehreren untergeordneten Domänen der Stammdomäne dieselbe auch auf derselben Anwendungsgateway gehostet werden.

## <a name="scenario"></a>Szenario

Im folgenden Beispiel Application Gateway dient Datenverkehr für contoso.com und fabrikam.com mit zwei Back-End-Server: Contoso Server und Fabrikam Server. Ähnliches Setup konnte Host Unterdomänen wie app.contoso.com und blog.contoso.com verwendet werden.

![Szenario für mehrere Standorte][multisite]

## <a name="before-you-begin"></a>Bevor Sie beginnen

Dieses Szenario unterstützt mehrere Standorte ein vorhandenes Gateway. Um dieses Szenario zu vervollständigen muss Szenario ein Gateway der vorhandenen Konfiguration zur Verfügung. Besuchen Sie [erstellen ein Gateway mit dem Portal](./application-gateway-create-gateway-portal.md) , um grundlegende Gateway im Portal erstellen.

## <a name="requirements"></a>Vorschriften

- **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. IP-Adressen sollten entweder das virtuelle Netzwerk-Subnetz angehören oder sollte eine öffentliche IP-Adresse/VIP. FQDN kann auch verwendet werden.
- **Back-End-pooleinstellungen:** Jeder Pool hat wie Port, Protokoll und cookiebasierte Affinität. Diese sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
- **Front-End-Port:** Dieser Port ist der öffentliche Port, der auf dem Anwendungsgateway geöffnet wird. Datenverkehr trifft dieser Anschluss und dann auf einen Back-End-Server umgeleitet wird.
- **Listener:** Der Listener verfügt über einen Front-End-Port ein Protokoll (Http oder Https, diese Werte sind Groß-/Kleinschreibung) und SSL-Zertifikat (wenn offload Konfigurieren von SSL). Für aktivierte Anwendung Multisite-Gateways werden Hostnamen und SNI Indikatoren ebenfalls hinzugefügt.
- **Regel:** Bindet den Listener Pool Back-End-Server und die Back-End-Serverpool der Datenverkehr weitergeleitet werden soll, einen bestimmten Listener trifft definiert.
- **Zertifikate:** Jeder Listener erfordert ein eindeutiges Zertifikat, in diesem Beispiel werden 2 Listener für mehrere Standorte erstellt. Zwei PFX-Zertifikate und Kennwörter für sie erstellt werden müssen.

## <a name="create-an-application-gateway"></a>Ein Gateway erstellen

Es folgen die Schritte Application Gateway aktualisieren:

1. Erstellen Sie Back-End-Pools für jede Site.
2. Erstellen Sie einen neuen Listener für jede Site Application Gateway unterstützt.
3. Erstellen Sie Regeln zum Zuordnen jeder Listener mit den entsprechenden Back-End.

## <a name="create-back-end-pools-for-each-site"></a>Back-End-Pools für jeden Standort erstellen

Back-End-Pool für jede Site, Application Gateway wird Unterstützung erforderlich, in diesem Fall 2 erstellt werden, für contoso11.com und für fabrikam11.com.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu einer vorhandenen Anwendungsgateway im Azure-Portal (https://portal.azure.com). Wählen Sie **Back-End-Pools aus** und auf **Hinzufügen**

![Hinzufügen von Back-End-pools][7]

### <a name="step-2"></a>Schritt 2

Geben Sie die Informationen für die Back-End-Pool **pool1**, Hinzufügen von IP-Adressen oder vollqualifizierte Domänennamen für Back-End-Server, und klicken Sie auf **OK**

![Back-End-Pool pool1 settings][8]

### <a name="step-3"></a>Schritt 3

Klicken Sie auf den Back-End-Pools auf **Hinzufügen** , um eine zusätzliche Back-End-Pool **pool2**, Hinzufügen von IP-Adressen oder vollqualifizierte DOMÄNENNAMEN für Back-End-Server hinzufügen und klicken Sie auf **OK**

![Back-End-Pool pool2 settings][9]

### <a name="create-listeners-for-each-back-end"></a>Für jede Backend-Listener erstellen

Application Gateway basiert auf HTTP 1.1 Hostheader hosten mehrere Websites auf dieselbe öffentliche IP-Adresse und Port. Grundlegende Listener erstellt im Portal enthält diese Eigenschaft nicht.

### <a name="step-1"></a>Schritt 1

**Listener** auf dem vorhandenen Application Gateway klicken Sie und **mehrere Standorte** zum ersten Listener hinzufügen.

![Listener Übersicht blade][1]

### <a name="step-2"></a>Schritt 2

Füllen Sie die Informationen für den Listener In diesem Beispiel SSL-Beendigung ist, erstellen Sie einen neuen Front-End-Port. Hochladen Sie .pfx-Zertifikat für SSL-Beendigung. Der einzige Unterschied im Vergleich zu standard grundlegende Listener Blade Blatt ist der Hostname.

![Listener Eigenschaften blade][2]

### <a name="step-3"></a>Schritt 3

Auf **mehrere Standorte** und anderen Listeners wie im vorherigen Schritt für die zweite Website beschrieben. Müssen Sie ein anderes Zertifikat für den zweiten Listener verwenden. Der einzige Unterschied im Vergleich zu standard grundlegende Listener Blade Blatt ist der Hostname. Geben Sie die Informationen für den Listener, und klicken Sie auf **OK**.

![Listener Eigenschaften blade][3]

> [AZURE.NOTE] Listener in der Azure-Portal Application Gateway ist eine zeitintensive Aufgabe, zwei Listener in diesem Szenario erstellt einige Zeit dauern. Nach Abschluss Listener anzeigen im Portal wie in der folgenden Abbildung dargestellt.

![Listener-Übersicht][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>Erstellen von Regeln zum Back-End-Pools Listener zuordnen

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu einer vorhandenen Anwendungsgateway im Azure-Portal (https://portal.azure.com). Wählen Sie **Regeln** und wählen Sie das vorhandene Standard Regel **regel1** und klicken Sie auf **Bearbeiten**.

### <a name="step-2"></a>Schritt 2

Füllen Sie das Blade Regeln wie in der folgenden Abbildung dargestellt. Auswählen der ersten Listener und erste und auf **Speichern** , wenn Sie fertig sind.

![Regel bearbeiten][6]

### <a name="step-3"></a>Schritt 3

Klicken Sie auf **Grundregel** 2. Regel erstellen. Füllen Sie das Formular mit der zweiten Listener zweite Back-End-Pool, und klicken Sie auf **OK** , speichern.

![grundlegende Blatt hinzufügen][10]

Dies schließt ein vorhandenes Gateway mit Unterstützung für mehrere Standorte über Azure-Portal konfigurieren.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Ihre Webseiten mit [Application Gateway - Web Application Firewall](application-gateway-webapplicationfirewall-overview.md) schützen

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png