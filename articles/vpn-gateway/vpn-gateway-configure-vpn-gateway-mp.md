<properties 
   pageTitle="Konfigurieren Sie eine VPN-Gateway im klassischen Azure-Portal | Microsoft Azure"
   description="Dieser Artikel führt Sie durch Konfigurieren des virtuellen Netzwerks VPN-Gateway und Gateway routing VPN-Typ ändern."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vpn-gateway-for-the-classic-deployment-model"></a>Konfigurieren Sie eine VPN-Gateway für das klassische Bereitstellungsmodell


Wenn Sie eine sichere standortübergreifende Verbindung zwischen Azure und Ihre lokalen Speicherort erstellen möchten, müssen Sie ein VPN-Gateway konfigurieren. Im klassischen Bereitstellungsmodell Gateway kann eine der zwei VPN-routing: statisch oder dynamisch. Die gewählte hängt sowohl Ihr Netzwerkplan entwerfen und lokalen VPN-Gerät zu verwenden. 

Einige Konnektivitätsoptionen wie eine Punkt-zu-Standort-Verbindung müssen beispielsweise dynamische routing-Gateway. Wenn Sie Ihr Gateway zur Unterstützung von Punkt-zu-Standort (P2S) Verbindung und eine Standort-zu-Standort (S2S) Verbindung konfigurieren möchten, müssen Sie dynamisches routing-Gateway konfigurieren, obwohl zwischen Standorten mit entweder Routingtyp VPN-Gateway konfiguriert werden kann. 

Darüber hinaus müssen sicherstellen, dass das Gerät für die Verbindung der VPN-routing unterstützt, die Sie erstellen möchten. [VPN-Geräte](vpn-gateway-about-vpn-devices.md)finden Sie unter.


**Zu diesem Artikel** 

Dieser Artikel wurde für das klassische Bereitstellungsmodell über das [Verwaltungsportal](https://manage.windowsazure.com) (nicht der Azure-Portal) geschrieben. 

**Azure-Bereitstellung Modelle**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-overview"></a>(Übersicht)

Die folgenden Schritte führen Sie durch Konfigurieren das VPN-Gateway im klassischen Azure-Portal. Diese Schritte gelten für Gateways für virtuelle Netzwerke, die mit dem klassischen Bereitstellungsmodell erstellt wurden. Gegenwärtig sind nicht alle Konfigurationsinformationen für Gateways in Azure-Portal verfügbar. Wenn sie sind, werden wir einen neuen Satz von Anweisungen erstellen, die Azure-Portal gelten.


1. [Erstellen Sie eine VPN-Gateway für das VNet](#create-a-vpn-gateway)

1. [Informationen Sie für die VPN-Konfiguration](#gather-information-for-your-vpn-device-configuration)

1. [Konfigurieren Sie das VPN-Gerät](#configure-your-vpn-device)

1. [Überprüfen Sie Ihre lokalen Netzwerkbereiche und VPN-Gateway IP-Adresse](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### <a name="before-you-begin"></a>Bevor Sie beginnen

Bevor Sie Ihr Gateway konfigurieren, müssen Sie zunächst Ihr virtuelle Netzwerk erstellen. Schritte zum Erstellen eines virtuellen Netzwerks für standortübergreifende Verbindung finden Sie unter [konfigurieren ein virtuelles Netzwerk mit einer Standort-zu-Standort-VPN-Verbindung](vpn-gateway-site-to-site-create.md)oder [ein virtuelles Netzwerk mit einer Punkt-zu-Standort-VPN-Verbindung konfigurieren](vpn-gateway-point-to-site-create.md). Dann gehen Sie zu das VPN-Gateway konfigurieren Informationen benötigen Sie die VPN-Konfiguration. 

Wenn Sie bereits eine VPN-Gateway und VPN-routing ändern möchten, finden Sie unter [den VPN-routing für Ihr Gateway ändern](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Erstellen Sie eine VPN-gateway

1. Im [klassischen Azure-Portal](https://manage.windowsazure.com)auf der Seite **Netzwerk** sicher, dass die Statusspalte für das virtuelle Netzwerk **erstellt**wird.

1. Klicken Sie in der Spalte **Name** auf den Namen des virtuellen Netzwerks.

1. Die Seite **Dashboard** feststellen Sie, dass diese VNet Gateway konfiguriert hat. Sie sehen diesen Status besprechen Ihres Gateways konfigurieren.

![Gateway nicht erstellt](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)


Am unteren Rand der Seite klicken Sie **Gateway erstellen**. Sie können *Statische Routen* oder *Dynamische Routing*auswählen. Der VPN-routing Typ hängt einige. Beispielsweise was das VPN-Gerät unterstützt, und brauchen Sie Punkt-zu-Standort-Verbindung zu unterstützen. Überprüfen Sie [Über VPN-Geräte virtuelle Netzwerkkonnektivität](vpn-gateway-about-vpn-devices.md) überprüfen Sie den routing VPN-Typ. Erstellte Gateway können Sie zwischen Gateway VPN routing ohne erneute Erstellen das Gateway nicht ändern. Das System fordert Sie zum Bestätigen das Gateway erstellt, klicken Sie auf **Ja**.

![Gateway routing VPN-Typ](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Beim Erstellen des Gateways, Gateway-Grafik auf der Seite ändert sich in Gelb und sagt *Gateway erstellen*. Es dauert bis zu 45 Minuten für das Gateway erstellen. Warten Sie, bis das Gateway abgeschlossen ist, bevor Sie mit anderen voranbringen können.

![Gateway erstellen](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Ändert das Gateway zu *Verbinden*, können Sie die Informationen für das VPN-Gerät müssen.

![Gateway verbinden](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="gather-information-for-your-vpn-device-configuration"></a>Informationen Sie für die VPN-Konfiguration

Nach dem Erstellen das Gateway-Informationen Sie für die VPN-Konfiguration. Diese Information befindet sich auf **der Dashboardseite für das virtuelle Netzwerk** :

1. **Gateway IP-Adresse:** Die IP-Adresse finden auf der Seite **Dashboard** . Nicht möglich, bis finden nach Schlüsselaufgaben erstellen.

1. **Shared Key-** Klicken Sie am unteren Bildschirmrand **Schlüssel verwalten** . Klicken Sie auf das Symbol neben den Schlüssel in die Zwischenablage kopieren und einfügen und speichern Sie den Schlüssel. Diese Schaltfläche funktioniert nur bei ein einzigen S2S VPN-Tunnel. Haben mehrere S2S VPN-Tunnel verwenden Sie *Get virtuellen Netzwerk Gateway Shared Key* API oder PowerShell-Cmdlet.

![Schlüssel verwalten](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)


## <a name="configure-your-vpn-device"></a>Konfigurieren Sie das VPN-Gerät

Nach Abschluss der vorherigen Schritte müssen Sie oder Ihr Netzwerkadministrator VPN-Gerät konfigurieren, um die Verbindung herzustellen. Weitere Informationen zu VPN-Geräten finden Sie unter [Über VPN-Geräte virtuelle Netzwerkkonnektivität](vpn-gateway-about-vpn-devices.md) .

Nachdem VPN-Gerät konfiguriert wurde, können Sie Ihre aktualisierten Informationen auf der Dashboardseite für das VNet anzeigen.

Sie können auch einen der folgenden Befehle, um die Verbindung zu testen ausführen:

|                      | Cisco ASA             | Cisco ISR/ASR         | Juniper SSG/ISG | Juniper SRX/J                            |
|----------------------|-----------------------|-----------------------|-----------------|------------------------------------------|
| **Hauptmodus-SAs überprüfen**  | Anzeigen von Crypto Isakmp sa | Anzeigen von Crypto Isakmp sa | Ike-Cookie abrufen  | Sicherheit anzeigen Ike Security Association   |
| **Schnellmodus-SAs überprüfen** | Anzeigen Crypto Ipsec sa  | Anzeigen Crypto Ipsec sa  | sa zu erhalten          | Sicherheit IPSec-Sicherheit-Zuordnung anzeigen |


## <a name="verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>Überprüfen Sie Ihre lokalen Netzwerkbereiche und VPN-Gateway IP-Adresse

### <a name="verify-your-vpn-gateway-ip-address"></a>Überprüfen Sie Ihre VPN-Gateway IP-Adresse

Für Gateway zu verbinden muss die IP-Adresse für das VPN-Gerät ordnungsgemäß für das lokale Netzwerk konfiguriert werden, die für die standortübergreifende Konfiguration angegeben. Dies wird normalerweise während der Konfiguration von Standort zu Standort konfiguriert. Jedoch zuvor lokalen Netzwerk mit einem anderen Gerät verwendet, ob die IP-Adresse für das lokale Netzwerk geändert hat, bearbeiten Sie die korrekte Gateway-IP-Adresse angeben.

1. Überprüfen Sie die IP-Adresse des Gateways im linken Portal **Netzwerke** auf, und wählen Sie **Lokale Netzwerke** am oberen Rand der Seite. Sie sehen die VPN-Gateway-Adresse für jeden lokalen Netzwerk, die Sie erstellt haben. Bearbeiten Sie die IP-Adresse wählen Sie das VNet, und klicken Sie am unteren Rand der Seite **Bearbeiten** .

1. Auf **Ihrem lokalen Netzwerk Angaben** die IP-Adresse, und klicken Sie auf den Pfeil am unteren Rand der Seite.

1. Klicken Sie auf der Seite **Geben Sie den Adressraum** auf Häkchen unten rechts Ihre Einstellungen speichern.

### <a name="verify-the-address-ranges-for-your-local-networks"></a>Überprüfen Sie die Adressbereiche für die lokale Netzwerke

Für die richtige Datenverkehr über das Gateway in das lokale Verzeichnis müssen Sie überprüfen, dass jede IP-Adressbereich angegeben ist. Jeder Bereich muss in der Konfiguration von Azure **Lokale Netzwerke** aufgeführt. Je nach Netzwerkkonfiguration Ihres lokalen Standorts kann dies etwas große Aufgabe sein. Datenverkehr, der IP-Adresse gebunden ist, der in den aufgeführten Bereichen enthalten werden über das virtuelle Netzwerk VPN-Gateway gesendet. Die Bereiche, die Sie aufführen müssen private Bereiche werden auch überprüfen, ob die lokale Konfiguration eingehenden Datenverkehr empfangen kann möchten.

Hinzufügen oder bearbeiten die Bereiche für ein lokales Netzwerk, gehen Sie folgendermaßen vor.

1. Bearbeiten Sie die IP-Adressbereiche für ein lokales Netzwerk **Netzwerke** Portal im linken Bereich auf, und wählen Sie **Lokale Netzwerke** am oberen Rand der Seite. Im Portal am einfachsten an aufgeführten Bereiche auf der Seite **Bearbeiten** . Die Bereiche, wählen Sie das VNet und klicken am unteren Rand der Seite **Bearbeiten** .

1. Auf **Ihrem lokalen Netzwerk Angaben** ändern Sie nicht. Klicken Sie auf den Pfeil am unteren Rand der Seite.

1. Auf der Seite **Geben Sie den Adressraum** ändern Sie Netzwerk Adresse Speicherplatz. Klicken Sie auf das Häkchen, um die Konfiguration zu speichern.

## <a name="how-to-view-gateway-traffic"></a>Gateway Datenverkehr anzeigen

Sie können die Gateway- und Gateway Datenverkehr auf Virtuelles Netzwerk **Dashboard** anzeigen.

Auf der Seite **Dashboard** können Sie Folgendes anzeigen:

- Die Datenmenge, die durch das Gateway Daten und Daten übertragen.

- Die Namen der DNS-Server, die für das virtuelle Netzwerk angegeben werden.

- Die Verbindung zwischen Ihrem Gateway und dem VPN-Gerät.

- Der freigegebene Schlüssel, mit dem die gatewayverbindung zum VPN-Gerät konfigurieren.


## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Wie VPN-routing für Ihr Gateway ändern

Da einige Konfigurationen Verbindung nur für bestimmte Gateway routing verfügbar sind, finden Sie das Gateway VPN-routing Typ eines vorhandenen VPN-Gateways ändern möchten. Sie möchten z. B. Punkt-zu-Standort-Verbindung zu einer bereits vorhandenen Standort-zu-Standort-Verbindung hinzufügen statischer Gateway. Punkt-zu-Standort-Verbindung erfordern dynamische Gateway. Dies bedeutet eine P2S Verbindung haben Sie Ihr Gateway VPN-routing Typ statische dynamischen ändern.

Benötigen Sie ein Gateway VPN Routingtyp ändern, Sie löschen das vorhandene Gateway, und erstellen Sie ein neues Gateway mit neuen Routingtyp. Sie müssen das gesamte virtuelle Netzwerk zum Gateway routing ändern zu löschen.

Das Routingtyp VPN-Gateway ändern, müssen Sie überprüfen, ob das VPN-Gerät das routing unterstützt, die Sie verwenden möchten. Zum Downloaden von neuen routing-Konfiguration-Beispiel und Überprüfen der VPN-Geräte finden [Über VPN-Geräte virtuelle Netzwerkkonnektivität](vpn-gateway-about-vpn-devices.md).

>[AZURE.IMPORTANT] Wenn Sie eine virtuelles Netzwerk VPN-Gateway löschen, erscheint VIP Gateway zugewiesen. Wenn das Gateway erstellen, wird eine neue VIP es zugewiesen.

1. **Löschen Sie das vorhandene VPN-Gateway.**

    Die Seite **Dashboard** für das virtuelle Netzwerk am unteren Rand der Seite navigieren Sie und auf **Gateway löschen**. Warten Sie auf die Benachrichtigung, dass das Gateway gelöscht wurde. Nachdem Sie die auf dem Bildschirm benachrichtigt, dass Ihr Gateway gelöscht wurde, können Sie ein neues Gateway erstellen.

1. **Erstellen eines neuen VPN-Gateways.**

    Verwenden Sie das Verfahren am oberen Rand der Seite ein neues Gateway erstellen: [Erstellen Sie ein VPN-Gateway](#create-a-vpn-gateway).


## <a name="next-steps"></a>Nächste Schritte

Sie können virtuelle Computer mit dem virtuellen Netzwerk hinzufügen. Finden Sie unter [Erstellen eines benutzerdefinierten virtuellen Computers](../virtual-machines/virtual-machines-windows-classic-createportal.md).

Wenn Sie eine Punkt-zu-Standort-VPN-Verbindung konfigurieren möchten, finden Sie unter [Konfigurieren einer Punkt-zu-Standort-VPN-Verbindung](vpn-gateway-point-to-site-create.md).

 
