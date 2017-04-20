<properties
   pageTitle="Logik-Apps für lokale datengateway installieren | Microsoft Azure"
   description="Informationen zum lokalen Daten-Gateways für die Verwendung in einer Anwendung Logik installieren."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# <a name="install-the-on-premises-data-gateway-for-logic-apps"></a>Installieren Sie lokale datengateway Logik Apps

## <a name="installation-and-configuration"></a>Installation und Konfiguration

### <a name="prerequisites"></a>Erforderliche Komponenten

Minimum:

* 4.5 .NET Framework
* 64-Bit-Version von Windows 7 oder Windows Server 2008 R2 (oder höher)

Empfohlen:

* 8-Core-CPU
* 8 GB Speicher
* 64-Bit-Version von Windows 2012 R2 (oder höher)

Verwandte Aspekte:

* Ein Gateway kann nicht auf einem Domänencontroller installiert werden.
* Sie sollte kein Gateway auf einem Computer installieren, so einen Laptop, der im Ruhezustand deaktiviert werden kann, oder nicht mit dem Internet verbunden, da das Gateway unter diesen Umständen ausgeführt werden kann. Darüber hinaus kann über ein drahtloses Netzwerk Gateway Leistung beeinträchtigt.

### <a name="install-a-gateway"></a>Ein Gateway installieren

Sie können das [Installationsprogramm für die lokalen datengateway](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409)erhalten.

**Lokale datengateway** den Modus angeben, melden Sie sich mit Ihrer Arbeit oder Schule Konto und dann entweder ein neues Gateway konfigurieren migrieren, wiederherstellen oder eines vorhandenen Gateways übernehmen.

* Um ein Gateway zu konfigurieren, geben Sie einen **Namen** und einen **Wiederherstellungsschlüssel**und klicken Sie oder tippen Sie **Konfigurieren**.

    Geben Sie einen Wiederherstellungsschlüssel, der mindestens 8 Zeichen enthält und an einem sicheren Ort aufbewahren. Sie benötigen diesen Schlüssel möchten Sie migrieren, wiederherstellen oder Gateway übernehmen.

* Zum Migrieren, wiederherstellen oder Übernehmen eines vorhandenen Gateways bieten Sie den Wiederherstellungsschlüssel beim Gateway angegeben wurde.

### <a name="restart-the-gateway"></a>Starten Sie das gateway

Das Gateway als Windows-Dienst ausgeführt wird und wie mit allen anderen Windows-Diensten können starten und beenden Sie es auf mehrere Arten. Sie können z. B. Öffnen Sie ein Eingabeaufforderungsfenster mit erhöhten Berechtigungen auf dem Computer, auf dem Gateway ausgeführt wird, und führen Sie einen der folgenden Befehle:

* Führen Sie diesen Befehl, um den Dienst zu beenden:

    `net stop PBIEgwService`

* Führen Sie diesen Befehl, um den Dienst zu starten:

    `net start PBIEgwService`

### <a name="configure-a-firewall-or-proxy"></a>Konfigurieren Sie eine Firewall oder einen Proxyserver

Informationen zu Proxyinformationen für Ihr Gateway finden Sie unter [Proxyeinstellungen konfigurieren](https://powerbi.microsoft.com/en-us/documentation/powerbi-gateway-proxy/).

Sie können überprüfen, ob Ihre Firewall oder Proxy Verbindungen blockiert werden möglicherweise durch Ausführen des folgenden Befehls von PowerShell aufgefordert. Verbindung zu Azure Service Bus wird getestet. Nur tests Netzwerkkonnektivität und hat nichts mit der Cloud-Serverdienst oder Gateway. Um festzustellen, ob Ihr Computer das Internet eigentlich kann erleichtert.

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

Die Ergebnisse sollten in diesem Beispiel ähneln. Wenn **TcpTestSucceeded** nicht zutrifft, können Sie durch eine Firewall blockiert.

```
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Ggf. vollständig ersetzen Sie **ComputerName** und **Port** -Werte durch finden Sie weiter unten in diesem Thema unter [Ports konfigurieren](#configure-ports) .

Die Firewall kann auch die Verbindung blockiert, die Azure Service Bus Azure Rechenzentren ermöglicht. Wenn dies der Fall ist, sollten Sie zur weißen Liste (Entsperren) alle IP-Adressen für Ihre Region für diese Rechenzentren. Sie erhalten eine Liste der [Azure-IP-Adressen](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="configure-ports"></a>Konfigurieren von ports

Das Gateway erstellt eine ausgehende Verbindung zu Azure Service Bus. Kommuniziert auf ausgehende Ports: TCP 443 (Standard), 5671 5672, 9350 durch 9354. Das Gateway erforderlich keine eingehenden Ports.

Erfahren Sie mehr über [Hybrid-Lösungen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| DOMÄNENNAMEN | AUSGEHENDE PORTS | BESCHREIBUNG |
| ----- | ------ | ------ |
| *. analysis.windows.net | 443 | HTTPS |
| *. login.windows.net | 443 | HTTPS |
| *. servicebus.windows.net |5671 5672 | Erweiterte Message Queuing Protocol (AMQP) |
| *. servicebus.windows.net | 443 9350 9354 | Listener Service Bus Relay über TCP (443 für Zugriffskontrolle token Übernahme erforderlich) |
| *. frontend.clouddatahub.net | 443 | HTTPS |
| *. von Core.Windows.NET befinden. | 443 | HTTPS |
| Login.microsoftonline.com | 443 | HTTPS |
| *. msftncsi.com | 443 | Verwendet, um Internetkonnektivität zu testen, ob das Gateway von Power BI-Dienst nicht erreichbar ist. |

Benötigen Sie weiße Liste IP-Adressen anstelle der Domänen, können der [Liste der Microsoft Azure Datacenter IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653)Sie downloaden und verwenden. In einigen Fällen Azure Service Bus Verbindungen mit IP-Adresse anstelle der vollqualifizierte Domänennamen erfolgen.

### <a name="sign-in-account"></a>Anmelden Konto

Benutzer anmelden entweder eine Arbeit oder Schule Konto. Dies ist Ihre Organisation Konto. Wenn Sie für ein Office 365-Angebot angemeldet und nicht die eigentliche e-Mail, sieht es wie jeff@contoso.onmicrosoft.com. Ihr Konto in einer Cloud-Dienst wird in einen Mandanten in Azure Active Directory (AAD) gespeichert. In den meisten Fällen wird Ihr Konto AAD UPN die e-Mail-Adresse überein.

### <a name="windows-service-account"></a>Windows-Dienstkonto

Das lokalen Daten-Gateway konfiguriert NT-SERVICE\PBIEgwService für die Windows-Anmeldeinformationen verwenden. Standardmäßig hat es die Anmelden als Dienst. Dies ist im Kontext des Computers, auf dem Gateway installiert sind.

Dies ist nicht das Konto eine Verbindung zu lokalen Datenquellen oder mit dem Sie sich anmelden Clouddiensten Arbeits- oder Schulcomputer Konto.

##<a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="general"></a>Allgemein

**Frage**: welche Datenquellen unterstützt das Gateway?<br/>
**Antwort**: ab diesem Schreiben von SQL Server.

**Frage**: kann ich ein Gateway für Datenquellen in der Cloud wie SQL Azure? <br/>
**Antwort**: Nein. Ein Gateway verbindet mit lokalen-Datenquellen.

**Frage**: wie wird der aktuelle Windows-Dienst genannt?<br/>
**Antwort**: Dienste, wird das Gateway BI Enterprise Gatewaydienst aufgerufen.

**Frage**: sind alle eingehenden Verbindungen zum Gateway von Cloud? <br/>
**Antwort**: Nein. Das Gateway verwendet Azure Service Bus Ausgeh.

**Frage**: Was geschieht, wenn ich ausgehende Verbindungen blockieren? Was muss ich öffnen? <br/>
**Antwort**: Siehe Ports und Hosts das Gateway verwendet.


**Frage**: muss das Gateway auf demselben Computer wie der Datenquelle installiert werden? <br/>
**Antwort**: Nein. Das Gateway verbindet, der Datenquelle die Verbindungsinformationen bereitgestellt wurde. Das Gateway als eine Clientanwendung in diesem Sinne vorstellen. Sie müssen nur den Servernamen herstellen können, die bereitgestellt wurden.


**Frage**: Was ist die Wartezeit für die Ausführung von Abfragen an eine Datenquelle vom Gateway? Was ist die Architektur? <br/>
**Antwort**: um die Netzwerkwartezeit zu reduzieren, installieren Sie das Gateway so nahe wie möglich der Datenquelle. Installation von Gateway auf der tatsächlichen Datenquelle wird die Latenz minimieren. Betrachten Sie die Data Centers. Wenn Ihr Dienst ist West US-Rechenzentrum und SQL Server in einer VM Azure gehostet haben, sollten Sie beispielsweise Azure VM im Westen der USA haben. Wartezeit minimieren und vermeiden Ausgang Zuschläge auf der Azure-VM.


**Frage**: Gibt es Anforderungen für Netzwerkbandbreite? <br/>
**Antwort**: Es wird empfohlen, guten Durchsatz für die Verbindung. Jede Umgebung ist anders, und die gesendeten Datenmenge die Ergebnisse auswirken. ExpressRoute konnte mit Hilfe einer Durchsatz zwischen lokalen und Azure Rechenzentren garantieren.

Drittanbietertool Geschwindigkeitstest Azure app können zu beurteilen, was der Durchsatz ist.


**Frage**: kann Gateway Windows-Dienst mit Azure Active Directory-Konto ausführen? <br/>
**Antwort**: Nein. Windows-Dienst muss ein gültiges Windowskonto verfügen. Standardmäßig werden sie mit Dienst-SID NT SERVICE\PBIEgwService ausgeführt.


**Frage**: wie werden Ergebnisse in die Cloud? <br/>
**Antwort**: Dies geschieht durch Azure Service Bus. Weitere Informationen finden Sie unter Funktionsweise.


**Frage**: wo Anmeldeinformationen gespeichert werden? <br/>
**Antwort**: die Anmeldeinformationen, die für eine Datenquelle eingeben werden im Cloud-Gatewaydienst verschlüsselt gespeichert. Die Anmeldeinformationen werden auf dem Betriebsgelände Gateway entschlüsselt.

### <a name="high-availabilitydisaster-recovery"></a>Hohe Verfügbarkeit/Wiederherstellung

**Frage**: Gibt es Pläne für hohe Verfügbarkeit Szenarien mit dem Gateway? <br/>
**Antwort**: die Roadmap ist, aber wir haben eine Zeitachse noch.


**Frage**: Welche Optionen für die Wiederherstellung verfügbar sind? <br/>
**Antwort**: verwenden den Wiederherstellungsschlüssel wiederherstellen oder ein Gateway. Geben Sie bei der Installation des Gateways den Wiederherstellungsschlüssel.


**Frage**: Was ist der Vorteil der Wiederherstellungsschlüssel? <br/>
**Antwort**: Es ermöglicht das Migrieren oder gatewayeinstellungen nach einem Notfall wiederherzustellen.

### <a name="troubleshooting"></a>Problembehandlung

**Frage**: sind die Protokolle Gateway? <br/>
**Antwort**: finden Sie weiter unten in diesem Thema unter Tools.


**Frage**: wie kann ich feststellen, welche Abfragen werden für lokale Datenquelle gesendet? <br/>
**Antwort**: Abfrage Tracing umfasst die Anfragen aktivieren. Beachten Sie wieder auf den ursprünglichen Wert nach der Problembehandlung ändern. Abfrage-Protokollierung aktiviert werden die Protokolle größer führen.

Sie können auch Tools suchen, die die Datenquelle für Verfolgung Abfragen. Beispielsweise können Sie erweiterte Ereignisse oder SQL Profiler für SQL Server und Analysis Services.

## <a name="how-the-gateway-works"></a>Wie funktioniert das gateway

Wenn ein Benutzer ein Element interagiert, die zu einer lokalen Datenquelle verbunden:

1. Cloud-Dienst erstellt eine Abfrage mit den verschlüsselten Anmeldeinformationen für die Datenquelle und sendet die Abfrage an die Warteschlange für das Gateway verarbeitet.
1. Der Dienst analysiert die Abfrage und überträgt die Anforderung auf Azure Service Bus.
1. Das lokalen datengateway fragt Azure Service Bus für Anfragen.
1. Das Gateway Ruft die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt eine Verbindung mit Datenquellen mit diesen Anmeldeinformationen.
1. Das Gateway sendet die Abfrage an der Datenquelle ausgeführt.
1. Die Ergebnisse werden von der Datenquelle zum Gateway und Cloud-Dienst gesendet. Der Dienst verwendet dann die Ergebnisse.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="update-to-the-latest-version"></a>Aktualisieren Sie auf die neueste version

Viele Probleme kann auftreten, wird die Gateway-Version veraltet.  Es wird allgemein empfohlen, achten Sie auf die neueste Version.  Wenn Sie das Gateway für einen Monat oder länger nicht aktualisiert haben, möchten Sie installieren Sie die neueste Version des Gateways zu sehen, ob Sie das Problem reproduzieren können.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users-"></a>: Fehler Gruppe Benutzer hinzu. (-2147463168 PBIEgwService-Leistungsprotokollbenutzer)

Diesen Fehler erhalten, wenn Sie das Gateway auf einem Domänencontroller installieren, das nicht unterstützt wird. Sie müssen das Gateway auf einem Computer bereitstellen, der ein Domänencontroller ist.

## <a name="tools"></a>Tools

### <a name="collecting-logs-from-the-gateway-configurator"></a>Erfassung von Protokollen aus der Gateway-Konfiguration

Sie können mehrere Protokolle für das Gateway sammeln. Beginnen Sie immer mit den Protokollen.

#### <a name="installer-logs"></a>Installer-Protokolle

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Konfiguration-Protokolle

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Unternehmens-Gateway-Dienstprotokollen

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Ereignisprotokolle

Die Protokolle Data Management Gateway und PowerBIGateway sind unter **Anwendung und Dienstprotokolle**.

### <a name="fiddler-trace"></a>Fiddler-Trace

[Fiddler](http://www.telerik.com/fiddler) ist ein kostenloses Tool von Telerik, die HTTP-Verkehr überwacht.  Sie können die Rückseite und service her mit Power BI aus dem Client-Computer. Dies kann Fehler und Weitere Informationen anzeigen.

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen Sie eine lokale Verbindung mit Logik Apps](app-service-logic-gateway-connection.md)
- [Enterprise-Integrationsfunktionen](app-service-logic-enterprise-integration-overview.md)
- [Logik Apps connectors](../connectors/apis-list.md)