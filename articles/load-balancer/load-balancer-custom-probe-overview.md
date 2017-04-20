<properties
  pageTitle="Benutzerdefinierte Probes Balancer laden und Überwachung des Integritätsstatus | Microsoft Azure"
  description="Erfahren Sie, wie benutzerdefinierte Probes für Azure Lastenausgleich verwenden, um Instanzen hinter Lastenausgleich"
  services="load-balancer"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="understand-load-balancer-probes"></a>Load Balancer Prüfpunkte verstehen

Azure Lastenausgleich bietet die Möglichkeit, Server-Instanzen mit Prüfpunkten überwachen. Wenn ein Prüfpunkt nicht antwortet, beendet Lastenausgleich das neue Verbindungen fehlerhafte Instanz senden. Die vorhandenen Verbindungen sind nicht betroffen und neue Verbindungen sind gesunde Instanzen an.

Cloud-Dienstverwaltungsrollen (Worker-Rollen und Webrollen) verwendet einen Gast-Agent zum Überwachen Sonde. Benutzerdefinierte Probes TCP oder HTTP müssen konfiguriert werden, wenn Sie virtuelle Computer hinter Lastenausgleich verwenden.

## <a name="understand-probe-count-and-timeout"></a>Anzahl der Probe und Timeout verstehen

Prüfpunkt Verhalten hängt:

- Die Anzahl der erfolgreichen Prüfpunkte, die eine Instanz bezeichnet werden kann als aktiv.
- Die Anzahl der fehlgeschlagenen Prüfpunkte, die eine Instanz bezeichnet werden als inaktiv.

In SuccessFailCount festgelegten Wert für Timeout und Häufigkeit bestimmen, ob eine Instanz bestätigt ausgeführt oder nicht ausgeführt werden. Azure-Portal wird zweimal Wert der Frequenz des Zeitlimits fest.

Die Prüfpunkt Konfiguration aller Lastenausgleich Instanzen für einen Endpunkt (d. h. eine Gruppe mit Lastenausgleich) muss identisch sein. Dies bedeutet, dass eine andere Prüfpunkt Konfiguration für jede Instanz der Rolle oder eine virtuelle Maschine im selben gehosteten Dienst für einen bestimmten Endpunkt Kombination haben kann. Beispielsweise muss jede Instanz identische lokale Ports und Timeouts.

>[AZURE.IMPORTANT] Ein Prüfpunkt Lastenausgleich verwendet die IP-Adresse 168.63.129.16. Diese öffentliche IP-Adresse ermöglicht die Kommunikation mit internen Plattformressourcen für die in-your-besitzen-IP Azure Virtual Network-Szenario. Die virtuelle öffentliche IP-Adresse 168.63.129.16 in allen Regionen verwendet und werden nicht geändert. Wir empfehlen diese IP-Adresse im lokalen Firewallrichtlinien ermöglichen. Es kein Sicherheitsrisiko berücksichtigen, da die interne Azure Plattform eine Nachricht von dieser Adresse Quelle. Wenn Sie dies nicht tun, werden unerwartete Szenarien wie die Konfiguration des gleichen IP-Adressbereichs von 168.63.129.16 und IP-Adressen müssen dupliziert.

## <a name="learn-about-the-types-of-probes"></a>Informationen Sie zu den Arten von Prüfpunkten

### <a name="guest-agent-probe"></a>Gast-Agent Prüfpunkt

Dieser Prüfpunkt wird nur für Azure Cloud Services. Lastenausgleich verwendet den Gast-Agent auf dem virtuellen Computer überwacht und eine HTTP 200 OK-Antwort reagiert nur, wenn die Instanz bereit ist (also nicht in einem anderen Staat wie gebucht, Recycling oder Beenden).

Weitere Informationen finden Sie unter [Konfigurieren von Service-Definitionsdatei (Csdef) für Gesundheit Prüfpunkte](https://msdn.microsoft.com/library/azure/ee758710.aspx) oder [Erste Schritte beim Erstellen einer Internetschnittstelle Lastenausgleich für Cloud-Services](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Was macht einen Gast Agent Prüfpunkt eine Instanz als fehlerhaft markieren?

Gast-Agent nicht mit HTTP 200 OK reagieren, Lastenausgleich kennzeichnet die Instanz reagiert nicht mehr und Senden von Datenverkehr an die Instanz beendet. Lastenausgleich weiterhin die Instanz ping. Reagiert der Agent Gast eine HTTP 200, sendet Lastenausgleich Datenverkehr auf diese Instanz erneut ein.

Bei Verwendung eine Webrolle führt der Website Code normalerweise in w3wp.exe Azure nicht überwacht Stoff oder Gast-Agent. Dies bedeutet, dass Fehler in w3wp.exe (z. B. HTTP 500 Antworten), Gast-Agenten nicht gemeldet, und Lastenausgleich wird nicht die Instanz aus der Rotation.

### <a name="http-custom-probe"></a>Benutzerdefinierte HTTP-probe

Benutzerdefinierte HTTP-Lastverteiler Probe überschreibt standardmäßig Gast Agent Prüfpunkts, d. h., Ihre eigene Logik ermitteln den Zustand der Instanz der Rolle zu erstellen. Lastenausgleich Prüfpunkte standardmäßig alle 15 Sekunden den Endpunkt. Die Instanz wird als Load Balancer Fruchtfolge antwortet mit HTTP 200 innerhalb des Zeitlimits (standardmäßig 31 Sekunden).

Dies ist nützlich, wenn Sie eine eigene Logik Load Balancer Drehung Instanzen entfernen implementieren möchten. Sie könnten z. B. eine Instanz entfernt, wenn er über 90 % CPU oder Status nicht 200. Wenn Sie Webrollen haben, w3wp.exe verwenden, bedeutet das auch erhalten Sie automatische Überwachung Ihrer Website, da Fehler im Code Website Status nicht 200 Load Balancer Prüfpunkt zurückgeben.

>[AZURE.NOTE] Benutzerdefinierte HTTP-Probe unterstützt relative Pfade und HTTP-Protokoll. HTTPS wird nicht unterstützt.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Was ist eine benutzerdefinierte HTTP-Probe eine Instanz als fehlerhaft markieren?

- Die HTTP-Anwendung gibt einen HTTP-Antwortcode als 200 (403, 404 oder 500). Dies ist eine positive Bestätigung die Anwendungsinstanz sofort genommen werden sollte.
- Der HTTP-Server reagiert nicht überhaupt nach das Zeitlimit. Je nach festgelegten Timeoutwert Dies kann bedeuten, dass mehrere Prüfpunkt fordert gehen offen vor der Prüfpunkt als nicht aktiv gekennzeichnet wird (d. h. vor SuccessFailCount Prüfpunkte werden gesendet).
- Der Server beendet die Verbindung über ein TCP-Reset.

### <a name="tcp-custom-probe"></a>Benutzerdefinierte Probe TCP

TCP-Prüfpunkte initiieren anhand Dreiwege-Handshake mit definierten Port eine Verbindung.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Was macht einen benutzerdefinierten TCP-Prüfpunkt eine Instanz als fehlerhaft markieren?

- TCP-Server reagiert nicht überhaupt nach das Zeitlimit. Wenn der Prüfpunkt markiert ist, als nicht ausgeführt hängt von der Anzahl der fehlgeschlagenen Suchanfragen konfiguriert unbeantwortet markiert den Prüfpunkt nicht ausgeführt wurden.
- Der Prüfpunkt empfängt TCP die Rolleninstanz zurückgesetzt.

Weitere Informationen zum Konfigurieren einer Integritätstest HTTP oder TCP-Prüfpunkt finden Sie unter [Erste Schritte beim Erstellen einer Internetschnittstelle Lastenausgleich in Ressourcen-Manager mit PowerShell](load-balancer-get-started-internet-arm-ps.md#create-lb-rules-nat-rules-a-probe-and-a-load-balancer).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Gesunde Instanzen in Load Balancer Drehung hinzufügen

TCP und HTTP-Prüfpunkte werden als gesund und markieren die Rolleninstanz so fehlerfrei, wenn:

- Der Lastenausgleich wird eine positive Probe erstmals die VM startet.
- Anzahl SuccessFailCount (wie oben beschrieben) definiert den Wert erfolgreich Prüfpunkte, die die Rolleninstanz als fehlerfrei mark erforderlich sind. Eine Rolleninstanz entfernt wurde, muss die Anzahl der erfolgreichen, aufeinander folgende Prüfpunkte mindestens den Wert der SuccessFailCount die Rolleninstanz als aktiv markieren.

>[AZURE.NOTE] Wenn die Gesundheit eine Rolleninstanz schwankt, wartet Lastenausgleich mehr vor der Rolleninstanz im fehlerfreien Zustand. Dies erfolgt über die Richtlinie zum Schutz der Benutzer und die Infrastruktur.

## <a name="use-log-analytics-for-load-balancer"></a>Protokollanalyse für Lastenausgleich verwenden

[Protokollanalyse Lastenausgleich](load-balancer-monitor-log.md) können Prüfpunkt Zustand und Anzahl der Prüfpunkt überprüfen. Protokollierung kann mit Power BI oder Azure betriebliche Informationen zu Statistiken zum Lastenausgleich Zustand verwendet werden.
