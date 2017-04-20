<properties
    pageTitle="OMS OMS-Gateway mit Computern und Geräten herstellen | Microsoft Azure"
    description="Verbinden Sie OMS-verwalteten Geräten und Computern Operations Manager überwacht OMS Gateway Daten an den OMS-Dienst gesendet, wenn sie nicht über Internetzugang verfügen."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>
<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="banders"/>

# <a name="connect-computers-and-devices-to-oms-using-the-oms-gateway"></a>OMS OMS-Gateway mit herstellen Sie Computern und Geräten

Dieses Dokument beschreibt, wie die OMS-verwalteten Geräten und System Center Operations Manager SCOM überwachten Computer Daten an den OMS-Dienst senden können, wenn sie nicht verfügen. OMS-Gateways können die Daten sammeln und OMS-Dienst in ihrem Namen an.

Das Gateway ist ein HTTP-Proxy weiterleiten, der HTTP-tunneling Befehl HTTP CONNECT unterstützt. Das Gateway verarbeitet bis zu 2000 OMS gleichzeitig verbundenen Geräte auf eine 4-Core-CPU mit Windows 16-GB-Server ausgeführt.

Beispielsweise kann Ihr Unternehmen oder großen Unternehmen Server mit Netzwerkkonnektivität aber möglicherweise keinen Internetzugang. Beispielsweise können Sie viele Verkaufsstellen (POS) Geräte mit keine direkte Kontrolle haben. Und in einem anderen Beispiel Operations Manager OMS-Gateway als Proxy-Server verwenden. In diesen Beispielen übertragen OMS-Gateway Daten von den Agents, die auf diesen Servern oder POS-Geräte zu OMS installiert sind.

Statt jedes einzelnen Agenten senden Daten direkt OMS und eine direkte Verbindung zum Internet erforderlich alle Agent stattdessen senden über einen einzelnen Computer mit Internetanschluss. Der Computer ist, Sie installieren und das Gateway. In diesem Szenario können Sie Agents auf Computern installieren, in denen Sie Daten sammeln möchten. Das Gateway überträgt Daten von den Agents auf OMS direkt-Gateway werden die übertragenen Daten nicht analysiert.

OMS-Gateway überwachen und analysieren, Leistung oder Daten für den Server, auf dem installiert, installieren Sie OMS-Agent auf dem Computer, dem Gateway auch installiert ist.

Das Gateway muss Zugriff auf das Internet OMS Daten hochladen. Jeder Agent müssen über Netzwerkkonnektivität zum Gateway, dass Agents automatisch Daten vom Gateway übertragen können. Am besten installieren Sie das Gateway nicht auf einem Computer, der auch ein Domänencontroller ist.

Hier ist ein Diagramm, der Datenfluss von direkten Agenten OMS zeigt.

![direkte Agent-Diagramm](./media/log-analytics-oms-gateway/direct-agent-diagram.png)

Abbildung den Datenfluss von Operations Manager zu OMS zeigt Inseln

![Operations Manager-Diagramm](./media/log-analytics-oms-gateway/scom-mgt-server.png)

## <a name="install-the-oms-gateway"></a>OMS-Gateway installieren

Installieren dieses Gateway ersetzt frühere Versionen des Gateways (Log Analytics Spediteur) installiert haben.

Voraussetzung: .net Framework 4.5, Windows Server 2012 R2 SP1 und höher

1. Die neueste Version des OMS-Gateways aus dem [Microsoft Download Center](http://download.microsoft.com/download/2/5/C/25CF992A-0347-4765-BD7D-D45D5B27F92C/OMS%20Gateway.msi)herunterladen.
2. Doppelklicken Sie zum Starten der Installation auf **OMS-Gateway.msi**.
3. Auf der Seite Willkommen, **Weiter**.  
    ![Gateway-Setup-Assistent](./media/log-analytics-oms-gateway/gateway-wizard01.png)
4. Wählen Sie auf der Seite **Lizenzvertrag zustimmen** , zu den EULA ein, und klicken Sie dann auf **Weiter**.
5. Auf der Port und Proxy-Adresse:
    1. Geben Sie die TCP-Portnummer des Gateways verwendet werden. Setup wird diese Port-Nummer von Windows-Firewall geöffnet. Der Standardwert ist 8080.
    Der gültige Bereich die Portnummer ist 1-65535. Wenn die Eingabe nicht in diesem Bereich, wird eine Fehlermeldung angezeigt.
    2. Wenn der Server, auf dem Gateway installiert, einen Proxy, geben Sie optional die Proxy-Adresse, Gateway eine Verbindung herstellen muss. Beispielsweise `http://myorgname.corp.contoso.com:80` Wenn leer, das Gateway versucht, direkt mit dem Internet herstellen. Anderenfalls verbindet das Gateway an den Proxy. Wenn der Proxyserver Authentifizierung erfordert, geben Sie Ihren Benutzernamen und Ihr Kennwort.
        ![Gateway-Assistent-Proxykonfiguration](./media/log-analytics-oms-gateway/gateway-wizard02.png)  
    3. Klicken Sie auf **Weiter**
6. Wenn Sie nicht Microsoft Updates aktiviert haben, wird die Microsoft Update angezeigt, in dem Sie Microsoft-Updates aktivieren können. Treffen Sie eine Auswahl, und klicken Sie dann auf **Weiter**. Andernfalls mit dem nächsten Schritt fortfahren.
7. Lassen Sie auf der Seite Zielordner Standard Ordner **%ProgramFiles%\OMS Gateway** oder geben Sie die gewünschte Gateway installieren, und klicken Sie dann auf **Weiter**.
8. Klicken Sie auf der Seite bereit zum Installieren **Installieren**. User Account Control möglicherweise anfordernden Berechtigung zur Installation angezeigt. Wenn dies der Fall ist, klicken Sie auf **Ja**.
9. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**. Sie können überprüfen, dass der Dienst läuft das Snap-in services.msc öffnen und **OMS-Gateway** in der Liste der Dienste angezeigt.  
    ![Services – OMS-Gateway](./media/log-analytics-oms-gateway/gateway-service.png)

## <a name="install-an-agent-on-devices"></a>Installieren eines Agents auf Geräten

Bei Bedarf finden Sie [Verbinden Windows-Computern Protokollanalyse](log-analytics-windows-agents.md) Informationen direkt installieren Agents verbunden. Dieser Artikel beschreibt die Installation des Agents mit einem Setup-Assistenten oder über die Befehlszeile.

## <a name="configure-oms-agents"></a>OMS-Agents konfigurieren

Siehe [Konfigurieren Proxy- und Firewall Settings mit Microsoft Monitoring Agent](log-analytics-proxy-firewall.md) Informationen zum Konfigurieren eines Agents für einen Proxyserver ist diesem Gateway.

Operations Manager-Agents senden Daten wie Operations Manager Alarme, Konfiguration Bewertung Instanzenraum und Kapazitätsdaten über Management-Server. Andere große Daten, wie IIS-Protokolle, Leistung und Sicherheit werden direkt an den OMS-Gateway gesendet. Eine vollständige Liste der durch jeden Kanal gesendeten Daten finden Sie unter [Lösungen Lösungskatalog Protokollanalyse hinzufügen](log-analytics-add-solutions.md) .

>[AZURE.NOTE]
Das Gateway mit Netzwerklastenausgleich verwendet, finden Sie unter [Netzwerklastenausgleich optional konfigurieren](#optionally-configure-network-load-balancing).

## <a name="configure-a-scom-proxy-server"></a>Konfigurieren eines Proxyservers SCOM

Operations Manager hinzufügen Gateway fungieren als Proxy-Server konfigurieren. Beim Aktualisieren der Proxy-Konfigurations wird die Proxykonfiguration automatisch an alle Agenten in Operations Manager reporting angewendet.

Um das Gateway zu Operations Manager unterstützt, müssen Sie:

- Microsoft Monitoring Agent (Agent-Version – **8.0.10900.0** oder höher) auf dem Gateway-Server installiert und konfiguriert für OMS-Arbeitsbereiche, mit denen Sie kommunizieren möchten.
- Das Gateway muss Internetkonnektivität oder einen Proxy-Server, die angeschlossen werden.

### <a name="to-configure-scom-for-the-gateway"></a>SCOM für das Gateway konfigurieren

1. Öffnen Sie die Operations Manager-Konsole unter **Operations Management Suite**, klicken Sie auf **Verbindung** und klicken Sie auf **Proxyserver konfigurieren**:  
    ![Operations Manager-Proxyserver konfigurieren](./media/log-analytics-oms-gateway/scom01.png)
2. Wählen Sie **Proxyserver Operations Management Suite auf** , und geben Sie die IP-Adresse des Gatewayservers OMS. Sicherstellen, dass Sie mit den `http://` Präfix:  
    ![Operations Manager-Serveradresse](./media/log-analytics-oms-gateway/scom02.png)
3. Klicken Sie auf **Fertig stellen**. OMS-Arbeitsbereich ist der Operations Manager-Server verbunden.

## <a name="configure-network-load-balancing"></a>Konfigurieren des Netzwerklastenausgleichs

Sie können das Gateway für hohe Verfügbarkeit mit Netzwerklastenausgleich einen Cluster erstellen. Cluster verwaltet den Datenfluss von Agenten durch Umleiten der angeforderten Verbindungen von Microsoft überwachen Agents über die Knoten hinweg. Ein Gateway-Server ausfällt, wird der Datenverkehr an andere Knoten umgeleitet.

1. Netzwerklastenausgleich-Manager öffnen und einen Cluster erstellen.
2. Maustaste auf den Cluster vor dem Hinzufügen von Gateways und wählen **Clustereigenschaften.** Konfigurieren Sie den Cluster eine eigene IP-Adresse:  
    ![Netzwerklastenausgleich-Manager-Cluster-IP-Adressen](./media/log-analytics-oms-gateway/nlb01.png)
3. Anschließen einen OMS-Gatewayserver mit Microsoft Monitoring Agent installiert, mit der rechten Maustaste die Cluster IP-Adresse und dann auf **Host zum Cluster hinzufügen**.  
    ![Netzwerk laden Netzwerklastenausgleich-Manager – hinzufügen Host dem Cluster](./media/log-analytics-oms-gateway/nlb02.png)
4. Geben Sie die IP-Adresse des Gateway-Server, die Sie verbinden möchten:  
    ![Network Load Balancing Manager – Host dem Cluster hinzufügen: Verbinden](./media/log-analytics-oms-gateway/nlb03.png)
5. Müssen Sie auf Computern ohne Internet-Verbindung die IP-Adresse des Clusters verwenden, wenn Sie **Microsoft Monitoring Agent-Eigenschaften**konfigurieren:  
    ![Microsoft Agent Eigenschaften-Proxyeinstellungen](./media/log-analytics-oms-gateway/nlb04.png)

## <a name="configure-for-automation-hybrid-workers"></a>Konfigurieren Sie für Automatisierung Hybrid Arbeitskräfte

Haben Sie Automatisierung Hybrid Arbeitskräfte in Ihrer Umgebung bieten folgendermaßen manuell, temporäre Abhilfe Gateway zur Unterstützung konfigurieren.

In den folgenden Schritten müssen Sie die Azure-Region kennen sich Automation-Konto befindet. Um den Speicherort zu suchen:

1. Mit der [Azure-Portal](https://portal.azure.com/)anmelden.
2. Wählen Sie den Dienst Azure Automation.
3. Wählen Sie das entsprechende Azure Automation.
4. **Klicken Sie unter**Umgebung anzeigen  
    ![Azure-Portal – Automatisierung Speicherort](./media/log-analytics-oms-gateway/location.png)

Identifizieren des URL für jeden Standort anhand der folgenden Tabellen:

**Auftrag Runtime Data Service URLs**

| **Speicherort** | **URL** |
| --- | --- |
| Norden der USA – zentral | nks-Jobruntimedata-Prod-su1.azure-automation.net |
| Westeuropa | Wir-Jobruntimedata-Prod-su1.azure-automation.net |
| Südlichen zentralen USA | Scus-Jobruntimedata-Prod-su1.azure-automation.net |
| Osten der USA | EU-Jobruntimedata-Prod-su1.azure-automation.net |
| Kanada Central | cc-Jobruntimedata-Prod-su1.azure-automation.net |
| Nordeuropa | Ne-Jobruntimedata-Prod-su1.azure-automation.net |
| Südostasien | SEA-Jobruntimedata-Prod-su1.azure-automation.net |
| Zentrale Indien | CID-Jobruntimedata-Prod-su1.azure-automation.net |
| Japan | Jpe-Jobruntimedata-Prod-su1.azure-automation.net |
| Australien | ASE-Jobruntimedata-Prod-su1.azure-automation.net |

**Agent-Dienst-URLs**

| **Speicherort** | **URL** |
| --- | --- |
| Norden der USA – zentral | nks-Agentservice-FA-1.azure-automation.net |
| Westeuropa | Wir-Agentservice-FA-1.azure-automation.net |
| Südlichen zentralen USA | Scus-Agentservice-FA-1.azure-automation.net |
| Osten der USA | eus2-Agentservice-FA-1.azure-automation.net |
| Kanada Central | cc-Agentservice-FA-1.azure-automation.net |
| Nordeuropa | Ne-Agentservice-FA-1.azure-automation.net |
| Südostasien | SEA-Agentservice-FA-1.azure-automation.net |
| Zentrale Indien | CID-Agentservice-FA-1.azure-automation.net |
| Japan | Jpe-Agentservice-FA-1.azure-automation.net |
| Australien | ASE-Agentservice-FA-1.azure-automation.net |

Verwenden des Computers als Arbeitnehmer Hybrid automatisch registriert wird zum Patchen mit Update Management-Lösung, folgendermaßen:

1. Die zulässige Hostliste OMS-Gateways URLs Dienst Runtime Daten hinzufügen. Zum Beispiel: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Dienst dem OMS Gateway mit dem folgenden PowerShell-Cmdlet:`Restart-Service OMSGatewayService`

Ist Ihrem Computer auf-an Bord Azure Automatisierung mithilfe des Cmdlets Hybrid Arbeitskraft Registrierung gehen Sie folgendermaßen vor:

1. Hinzufügen der Agent-Registrierung URL erlaubt Host OMS-Gateways. Zum Beispiel:`Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Die zulässige Hostliste OMS-Gateways URLs Dienst Runtime Daten hinzufügen. Zum Beispiel: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Dienst dem OMS-Gateways.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Nützliche PowerShell-cmdlets

Cmdlets können Sie Aufgaben, die der OMS-Gateway-Konfigurationen zu aktualisieren. Bevor Sie sie verwenden, müssen Sie:

1. OMS-Gateway (MSI) installieren.
2. PowerShell-Fenster zu öffnen.
3. Geben Sie diesen Befehl, um das Modul zu importieren:`Import-Module OMSGateway`
4. Kein Fehler im vorherigen Schritt wurde das Modul importiert und die Cmdlets verwendet werden. Typ`Get-Module OMSGateway`
5. Nachdem Sie mit den Cmdlets geändert haben, sicherstellen Sie, dass der Gateway-Dienst neu gestartet.

Wenn Sie in Schritt 3 eine Fehlermeldung erhalten, war nicht das Modul importiert. Der Fehler kann auftreten, wenn PowerShell das Modul finden kann. Sie finden es in der Gateway-Installationspfad: c:\Programme\Microsoft c:\Programme\Microsoft OMS Gateway\PowerShell.

| **Cmdlets** | **Parameter** | **Beschreibung** | **Beispiele** |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` | Schlüssel (erforderlich) <br> Wert | Die Konfiguration des Dienstes | `Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` | Schlüssel | Ruft die Konfiguration des Dienstes | `Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` | Adresse <br> Benutzername <br> Kennwort | Legt die Adresse und Anmeldeinformationen Relay (Upstreamproxy) | 1. richten Sie Antwort Proxy und Anmeldeinformationen`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. Legen Sie Antwort Proxy, die Authentifizierung:`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3 deaktivieren Antwort-Proxyeinstellung, also ohne Antwort Proxy:`Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` |   | Ruft die Adresse des Relay (Upstreamproxy) | `Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` | Host (erforderlich) | Fügt zur Liste zugelassenen | `Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHos`t | Host (erforderlich) | Entfernt den Host aus der Liste der zugelassenen | `Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` |   | Ruft den derzeit zulässigen Host (nur lokal konfigurierten Host, enthalten nicht automatisch heruntergeladenen zulässigen Hosts) | `Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` | Betreff (erforderlich) | Fügt das Clientzertifikat unter der Liste der zugelassenen | `Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` | Betreff (erforderlich) | Die Liste der zulässigen entfernt Zertifikatantragsteller client | `Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificat`e |   | Ruft die derzeit zulässigen Zertifikatantragsteller (nur die lokal konfigurierten zulässigen Themen enthalten nicht automatisch heruntergeladene zulässige Themen) | `Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshoot"></a>Problembehandlung bei

Wir empfehlen Sie OMS-Agent auf Computern installieren, die Gateways installiert. Den Agent können Sie Ereignisse erfassen, die vom Gateway protokolliert werden.

![Ereignisanzeige – OMS Gateway-Protokoll](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS-Gateways Ereignis-IDs und Beschreibung**

Die folgende Tabelle zeigt die Ereignis-IDs und für OMS Gateway Protokollereignisse.

| **ID** | **Beschreibung** |
| --- | --- |
| 400 | Anwendungsfehler, die keine bestimmte ID |
| 401 | Falsche Konfiguration. Beispiel: ListenPort = "Text" anstatt Integer |
| 402 | Ausnahme beim Analysieren der TLS-Handshake-Nachrichten |
| 403 | Netzwerkfehler. Beispiel: kann keine Verbindung mit dem Zielserver |
| 100 | Allgemeine Informationen |
| 101 | Dienst wurde gestartet |
| 102 | Dienst wurde beendet |
| 103 | Einen HTTP CONNECT-Befehl empfangen vom client |
| 104 | Nicht HTTP CONNECT Befehl |
| 105 | Zielserver ist nicht in der Liste der zugelassenen oder Zielport nicht sicheren Port (443) <br> <br> Sicherstellen Sie, dass der MMA-Agent auf dem Gateway-Server und Agenten mit dem Gateway kommunizieren mit der Protokollanalyse Arbeitsbereich verbunden sind.|
| 105 | Fehler beim TcpConnection – ungültige Clientzertifikat: CN = Gateway <br><br> Überprüfen Sie Folgendes: <br> <br> & #149; Verwenden Sie ein Gateway mit 1.0.395.0 oder höher. <br> & #149; MMA-Agent auf dem Gateway-Server und Agenten mit dem Gateway kommunizieren sind mit dem gleichen Protokollanalyse Arbeitsbereich verbunden. |
| 106 | Aus irgendeinem Grund, die TLS-Sitzung verdächtige und abgelehnt |
| 107 | Die TLS-Sitzung wurde bestätigt |

**Leistungsindikatoren sammeln**

In der folgenden Tabelle werden die verfügbaren Leistungsindikatoren für OMS-Gateway Sie können mit Systemmonitor Leistungsindikatoren hinzufügen.

| **Name** | **Beschreibung** |
| --- | --- |
| OMS-Gateway-aktiv-Clientverbindung | Anzahl der aktiven Clientverbindungen Netzwerk (TCP) |
| Anzahl der OMS-Gateway-Fehler | Anzahl der Fehler |
| OMS-Client Gateway/verbunden | Anzahl der verbundenen clients |
| Anzahl der OMS-Gateway-Ablehnung | Anzahl der Ablehnung durch TLS Validierungsfehler |

![OMS-Gateway-Leistungsindikatoren](./media/log-analytics-oms-gateway/counters.png)


## <a name="get-assistance"></a>Hilfe

Wenn Sie Azure-Portal angemeldet sind, können Sie ein Amtshilfeersuchen mit OMS-Gateway oder Azure Service oder Funktion eines Dienstes erstellen.
Hilfe, klicken Sie auf das Fragezeichen oben rechts des Portals, und klicken Sie auf **neue support-Anfragen**. Schließen Sie das neue Anforderungsformular.

![Neue Anfrage](./media/log-analytics-oms-gateway/support.png)

Sie können auch Feedback zu OMS oder Protokollanalyse [Microsoft Azure Bewertungsportal](https://feedback.azure.com/forums/267889)lassen.

## <a name="next-steps"></a>Nächste Schritte

- [Hinzufügen von Datenquellen](log-analytics-data-sources.md) Daten aus Quellen verbunden im Arbeitsbereich OMS und OMS-Repository gespeichert.
