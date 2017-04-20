<properties
   pageTitle="Lokale datengateway | Microsoft Azure"
   description="Ein auf lokalen Gateway ist erforderlich, wenn der Analysis Services-Server in Azure mit lokalen Datenquellen verbinden."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="on-premises-data-gateway"></a>Lokale Daten-gateway

Das lokalen Daten-Gateway fungiert als Brücke, sichere Datenübertragung zwischen lokalen Datenquellen und Azure Analysis Services-Servers in der Cloud bereitstellen.

Ein Gateway ist auf einem Computer in Ihrem Netzwerk installiert. Ein Gateway werden für jeden Azure Analysis Services-Server installiert in Azure-Abonnement haben. Beispielsweise haben Sie zwei Server in Azure-Abonnement, die lokalen Datenquellen herstellen, muss ein Gateway auf zwei verschiedenen Computern im Netzwerk installiert.

## <a name="requirements"></a>Vorschriften

**Mindestanforderungen:**

- 4.5 .NET Framework
- 64-Bit-Version von Windows 7 / Windows Server 2008 R2 (oder höher)

**Empfohlen:**

- 8-Core-CPU
- 8 GB Speicher
- 64-Bit-Version von Windows 2012 R2 (oder höher)

**Wichtig:**

- Das Gateway kann auf einem Domänencontroller installiert werden.

- Nur ein Gateway kann auf einem einzelnen Computer installiert werden.

- Installieren Sie das Gateway auf einem Computer, der verbleibt und nicht in den Standbymodus. Ist der Computer nicht auf, herstellen nicht lokalen Datenquellen Daten aktualisieren Ihre Azure Analysis Services-Server.

- Installieren Sie das Gateway nicht auf einem Computer mit dem Netzwerk drahtlos verbunden. Performance kann beeinträchtigt werden.

- Server für das Gateway ändern, die bereits konfiguriert wurde, müssen Sie installieren und konfigurieren Sie ein neues Gateway.

- In einigen Fällen möglicherweise tabellarische Modellen Herstellen einer Verbindung mit Datenquellen über systemeigene Anbieter wie SQL Server Native Client (SQLNCLI11) einen Fehler zurück. Weitere anzeigen Sie [Datasource Verbindungen](analysis-services-datasource.md)

## <a name="supported-on-premises-data-sources"></a>Unterstützte lokale Datenquellen
Vorschau unterstützt das Gateway zwischen Azure Analysis Services-Servers und vor-Ort-Datenquellen:

- SQL Server
- SQL Datawarehouse
- APS
- Oracle
- Teradata


## <a name="download"></a>Herunterladen
 [Downloaden Sie das gateway](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>Installieren und konfigurieren

1. Führen Sie Setup.

2. Wählen Sie einen Installationspfad und stimme dem Lizenzvertrag.

3. Melden Sie sich bei Azure.

4. Geben Sie Ihren Azure Analysis-Server-Namen. Sie können nur einen Server pro Gateway. Klicken Sie auf **Konfigurieren** und gehen.

    ![Melden Sie sich bei azure](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>Funktionsweise
Das Gateway wird als Windows-Dienst, **lokale Daten-Gateway**auf einem Computer im Netzwerk Ihrer Organisation ausgeführt. Gateway mit Azure Analysis Services installieren basiert auf dem gleichen Gateway für andere Dienste wie Power BI verwendet jedoch einige Unterschiede wie konfiguriert ist.

![Funktionsweise](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Abfragen und fortlaufenden Arbeit wie folgt:

1.  Eine Abfrage wird vom Cloud-Dienst mit den verschlüsselten Anmeldeinformationen für die lokale Datenquelle erstellt. Es wird dann an eine Warteschlange für das Gateway zum Verarbeiten gesendet.

2.  Gateway-Clouddienst analysiert die Abfrage und legt die Anforderung an den [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).

3.  Das lokalen datengateway fragt Azure Service Bus für Anfragen.

4.  Das Gateway Ruft die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt eine Verbindung mit Datenquellen mit diesen Anmeldeinformationen.

5.  Das Gateway sendet die Abfrage an der Datenquelle ausgeführt.

6.  Die Ergebnisse werden aus der Datenquelle an das Gateway und auf die Cloud-Dienst gesendet.

## <a name="windows-service-account"></a>Windows-Dienstkonto

Das lokalen Daten-Gateway konfiguriert *NT-SERVICE\PBIEgwService* für die Windows-Anmeldeinformationen verwenden. Standardmäßig hat er die Anmeldung als Dienst; im Rahmen der Installation das Gateway auf Computer. Diese Anmeldeinformationen ist nicht dasselbe Konto zum Verbinden mit lokalen Datenquellen oder Ihre Azure-Konto.  

Wenn Probleme mit Ihrem Proxy-Server durch Authentifizierung auftreten, möchten Windows-Dienstkonto zur Domäne ändern oder Dienstkonto.

## <a name="ports"></a>Anschlüsse

Das Gateway erstellt eine ausgehende Verbindung zu Azure Service Bus. Kommuniziert auf ausgehende Ports: TCP 443 (Standard), 5671 5672, 9350 durch 9354.  Das Gateway erfordert keine eingehenden Ports.

Sie hat Whitelist empfohlen die IP-für Ihre Datenbereich in der Firewall Adressen. Sie können [Microsoft Azure Datacenter-Liste](https://www.microsoft.com/download/details.aspx?id=41653). Diese Liste wird wöchentlich aktualisiert.

> [AZURE.NOTE]  Liste Azure Datacenter IP-Adressen sind in CIDR-Notation. 10.0.0.0/24 bedeutet z. B. nicht 10.0.0.0 bis 10.0.0.24. Erfahren Sie mehr über die [CIDR-Notation](http://whatismyipaddress.com/cidr).

Folgende sind die vollqualifizierten Domänennamen vom Gateway verwendet.

|Domänennamen|Ausgehende ports|Beschreibung|
|---|---|---|
|*. powerbi.com|80|Das Installationsprogramm verwendet HTTP.|
|*. powerbi.com|443|HTTPS|
|*. analysis.windows.net|443|HTTPS|
|*. login.windows.net|443|HTTPS|
|*. servicebus.windows.net|5671 5672|Erweiterte Message Queuing Protocol (AMQP)|
|*. servicebus.windows.net|443 9350 9354|Listener Service Bus Relay über TCP (443 für Zugriffskontrolle token Übernahme erforderlich)|
|*. frontend.clouddatahub.net|443|HTTPS|
|*. von Core.Windows.NET befinden.|443|HTTPS|
|Login.microsoftonline.com|443|HTTPS|
|*. msftncsi.com|443|Verwendet, um Internetkonnektivität zu testen, ob das Gateway von Power BI-Dienst nicht erreichbar ist.|
|*.microsoftonline p.com|443|Für die Authentifizierung, je nach Konfiguration verwendet.|


### <a name="forcing-https-communication-with-azure-service-bus"></a>HTTPS-Kommunikation mit Azure Service Bus erzwingen

Sie können Gateways für die Kommunikation mit Azure Service Bus mit HTTPS statt direkte TCP erzwingen; jedoch kann dies die Leistung erheblich beeinträchtigen. Sie müssen die Datei *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* ändern. Ändern Sie den Wert von `AutoDetect` , `Https`. Diese Datei befindet standardmäßig *c:\Programme\Microsoft Files\On lokale Daten-Gateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Problembehandlung
Hinter den Kulissen ist für lokale Daten-Gateway zur Verbindung von Azure Analysis Services zu lokalen Datenquellen dieselbe mit Power BI verwendet.

Wenn Sie Probleme beim Installieren und Konfigurieren von Gateway, unbedingt finden Sie unter [Problembehandlung bei Power BI-Gateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Wenn Sie, dass Sie ein Problem mit der Firewall haben glauben, finden Sie im Firewall oder einen Proxyserver.

Man Proxy Probleme auftreten mit dem Gateway finden Sie unter [Konfigurieren von Proxyeinstellungen für Power BI-Gateways](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md)

## <a name="next-steps"></a>Nächste Schritte
- [Verwalten von Analysis Services](analysis-services-manage.md)
- [Daten von Azure Analysis Services abrufen](analysis-services-connect.md)
