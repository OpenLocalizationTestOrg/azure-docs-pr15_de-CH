<properties
    pageTitle="Systemmonitor-Lösung in OMS Netzwerk | Microsoft Azure"
    description="Netzwerk-Performance Monitor können Sie die Leistung des Netzwerke in real-time zu überwachen erkennen und Leistungsengpässe Netzwerk suchen."
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
    ms.date="07/28/2016"
    ms.author="banders"/>

# <a name="network-performance-monitor-preview-solution-in-oms"></a>Netzwerk-Performance-Monitor (Vorschau) Lösung OMS

>[AZURE.NOTE] Dies ist eine [Vorschau Lösung](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Dieses Dokument beschreibt die Lösung Netzwerkleistungsmonitor OMS können Sie die Leistung des Netzwerke in real-time zu überwachen erkennen und suchen Netzwerk wie einzurichten Leistungsengpässe. Mit der Lösung Netzwerkleistungsmonitor können Sie Datenverlust und Wartezeit zwischen zwei Netzwerken Subnets oder Server überwachen. Netzwerkleistungsmonitor erkennt Netzwerkprobleme wie Verkehr Blackholing routing Fehler und Probleme bei der konventionellen Netzes Methoden nicht erkennen können. Netzwerkleistungsmonitor löst Alarme und benachrichtigt, wenn ein Schwellenwert für eine verletzt wird. Diese Schwellenwerte können vom System automatisch erlernt oder können sie benutzerdefinierte Warnregeln verwenden. Netzwerkleistungsmonitor gewährleistet der Netzwerk-Performance-Probleme rechtzeitig erkannt und lokalisiert die Ursache des Problems mit einem bestimmten Netzwerksegment oder Gerät.

Sie können Netzwerkprobleme mit dem lösungsdashboard erkennen die zusammengefassten Informationen über Ihr Netzwerk sowie Network Health Ereignisse, fehlerhafte Netzwerklinks Subnetz Links, die hoher Paketverlust und Latenz werden angezeigt. Sie können eine Netzwerk Verbindung an den aktuellen Status des Subnetzes Links sowie Links zum Knoten Detailinformationen. Sie können auch Verlaufsdaten Trend und Latenz im Netzwerk, Subnetz und Knoten-Ebene anzeigen. Erkennt vorübergehende Netzwerkprobleme anzeigen historisch Trenddiagramme Paketverlust und Latenz und Netzwerkengpässe eine topologische Karte suchen. Das Diagramm interaktiv Topologie können Sie Hop Hop Netzwerkrouten visualisieren und die Ursache des Problems ermitteln. Wie andere Lösungen können Sie Protokolldateien nach Analytics Anforderungen zum Erstellen benutzerdefinierter Berichte basierend auf Daten Netzwerkleistungsmonitor.

Die Lösung verwendet synthetische Transaktionen als primären Mechanismus zum Erkennen von Netzwerkfehlern. So können Sie es ohne für ein bestimmtes Netzwerkgerät Hersteller oder Modell verwenden. In lokalen Cloud (IaaS) und hybride Umgebung funktioniert. Die Lösung erkennt automatisch die Netzwerktopologie und verschiedene Routen in Ihrem Netzwerk.

Normale Überwachung Netzwerkprodukte auf die Netzwerk-Gerät (Router, Switches usw.) Überwachung sondern nicht Einblicke in die Qualität der Netzwerkkonnektivität zwischen zwei Punkten Netzwerkleistungsmonitor ist.

### <a name="using-the-solution-standalone"></a>Mithilfe des eigenständigen Lösung

Möchten Sie die Qualität der Netzwerkanschlüsse zwischen kritischen Arbeitslasten überwachen, können Netzwerke, Rechenzentren oder Standorte, dann Netzwerkleistungsmonitor Lösung selbst Sie Konnektivität zwischen überwachen:

- mehrere Rechenzentren oder Office-Standorte, die über ein öffentliches oder privates Netzwerk verbunden sind
- kritische Arbeitslasten, die Geschäftsanwendungen ausführen
- öffentlichen Cloud-Dienste wie Microsoft Azure oder Amazon Web Services (AWS) für lokale Netzwerke, wenn Sie (Neuerung) verfügbar und Gateways konfiguriert Kommunikation zwischen lokalen Netzwerken und Cloud
- Azure und lokalen Netzwerken Verwendung Express Route

### <a name="using-the-solution-with-other-networking-tools"></a>Verwenden die Lösung mit andere Netzwerktools

Wenn Sie eine branchenspezifische Anwendung überwachen möchten, können Sie als Companion Lösung für andere Netzwerktools Netzwerkleistungsmonitor Lösung. Ein langsames Netzwerk kann langsam Applikationen und Netzwerkleistungsmonitor können Sie Performance-Probleme untersuchen, die zugrunde liegende Netzwerkprobleme verursacht werden. Da die Lösung nicht Zugriff auf Netzwerkgeräte benötigen, muss dem Anwendungsadministrator ein Netzwerk Team zu informieren, wie das Netzwerk Applikationen auswirkt.

Auch wenn Sie bereits andere Netzwerk-Überwachungstools investieren, kann die Lösung diese Tools ergänzen da traditionelle Netzwerk Überwachung Solutions Einblicke Leistungsmetriken wie Datenverlust und Wartezeit für End-to-End-Netzwerk nicht bereitstellen.  Die Netzwerkleistungsmonitor Lösung hilft diese Lücke.


## <a name="installing-and-configuring-agents-for-the-solution"></a>Installieren und Konfigurieren von Agents für die Lösung

Verwenden Sie grundlegende Prozesse zum Installieren von Agents [Verbinden Windows-Computern Protokollanalyse](log-analytics-windows-agents.md) und [Protokollanalyse Operations Manager herstellen](log-analytics-om-agents.md).

>[AZURE.NOTE]
Sie müssen mindestens 2 Agents installieren, um genügend Daten zur Untersuchung und Überwachung Ihrer Netzwerkressourcen. Andernfalls bleibt Lösung Konfigurieren von Status, bis installieren und konfigurieren Sie weitere Agents.

### <a name="where-to-install-the-agents"></a>Wo die Agents installieren

Bevor Sie Agents installieren, sollten Sie die Topologie des Netzwerks und welche Teile des Netzwerks, das Sie überwachen möchten. Wir empfehlen die Installation mehrere Agents für jedes Subnetz, das Sie überwachen möchten. Also für jedes Subnetz, das Sie überwachen möchten, wählen Sie Server oder virtuelle Computer, und installieren Sie den Agent auf.

Wenn Sie die Topologie Ihres Netzwerks unsicher sind, installieren Sie die Agenten auf Servern mit kritischen Arbeitslasten in der Netzwerk-Performance überwachen soll. Angenommen, möchten Sie eine Verbindung zwischen einem Webserver und einem Server mit SQL Server an. In diesem Beispiel würden Sie einen Agent auf beiden Servern installieren.

Agenten überwachen Netzwerkkonnektivität (Links) zwischen Hosts nicht die Hosts. Damit eine Überwachung Sie Agents auf beide Endpunkte dieser Verknüpfung installieren müssen.

### <a name="configure-agents"></a>Konfigurieren von agents

Nach der Installation von Agents müssen Sie Öffnen von Firewallports für diese Computer um sicherzustellen, dass Agenten kommunizieren können. Herunterladen und dann [EnableRules.ps1 PowerShell-Skript](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) ohne Parameter im PowerShell-Fenster mit Administratorrechten ausführen müssen

Das Skript wird erstellt der Netzwerkleistungsmonitor erforderliche Registrierungsschlüssel und Windows-Firewallregeln Agenten mit TCP-Verbindungen erstellen können. Vom Skript erstellte Registrierungsschlüssel angeben, ob sich die Debugprotokolle und den Pfad für die Datei Protokolle. Außerdem definiert den Agent TCP-Port für die Kommunikation verwendet. Die Werte für diese Schlüssel werden automatisch durch das Skript festgelegt, damit diese Schlüssel nicht manuell ändern müssen.

Standardmäßig geöffnet wird 8084. Können Sie einen benutzerdefinierten Port mit dem Parameter `portNumber` für das Skript. Allerdings sollte denselben Port auf allen Computern verwendet werden, auf dem das Skript ausgeführt.

>[AZURE.NOTE] Das EnableRules.ps1-Skript konfiguriert Windows Firewall-Regeln nur auf dem Computer, auf dem das Skript ausgeführt wird. Haben Sie eine Netzwerkfirewall, sollten Sie sicherstellen, dass Datenverkehr für TCP-Port verwendeten Netzwerkleistungsmonitor ermöglicht.


## <a name="configuring-the-solution"></a>Konfiguration der Lösung

Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung.

1. Die Netzwerkleistungsmonitor Lösung erhält Daten von Computern unter Windows Server 2008 SP 1 oder höher oder Windows 7 SP1 oder höher, die die gleichen Vorschriften wie Microsoft Monitoring Agent (MMA).
2. Hinzufügen der Lösung Netzwerkleistungsmonitor OMS Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md).  
  ![Netzwerk-Performance Monitor-symbol](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. Im Portal OMS sehen Sie eine neue Tile Ins **Netzwerkleistungsmonitor** *erfordert zusätzliche Konfiguration*angezeigt. Sie müssen die Konfiguration der Lösung hinzufügen Netzwerke basierend auf Subnetze und Knoten, die vom Agenten erkannt. Klicken Sie auf **Network Performance Monitor** Konfigurieren des Standard-Netzwerks.  
  ![erfordert zusätzliche Konfiguration](./media/log-analytics-network-performance-monitor/npm-config.png)


### <a name="configure-the-solution-with-a-default-network"></a>Konfigurieren Sie die Projektmappe mit einem Standardnetzwerk

Auf der Konfigurationsseite sehen Sie ein Netzwerk mit dem Namen **Standard**. Wenn Sie Netzwerke definiert haben, werden alle automatisch erkannt Subnetze im Standard-Netzwerk platziert.

Erstellt ein Netzwerk ein Subnetz hinzufügen und das Subnetz aus dem Standard-Netzwerk entfernt. Löschen ein Netzwerks werden automatisch alle Subnetze mit dem Standardnetzwerk zurückgegeben.

Standard-Netzwerk ist also der Container für alle Subnetze, die nicht in einem benutzerdefinierten Netzwerk enthalten sind. Sie können nicht bearbeiten oder Löschen von Standard-Netzwerk. Das System bleibt. Sie können jedoch beliebig viele Netzwerke erstellen.

In den meisten Fällen die Subnetze in Ihrem Unternehmen in mehr als einem Netzwerk angeordnet und erstellen Sie ein oder mehrere Netzwerke Subnets logisch zu gruppieren.

### <a name="create-new-networks"></a>Neue Netzwerke erstellen

Ein Netzwerk Netzwerkleistungsmonitor ist ein Container für Subnetze. Ein Netzwerk kann mit Namen erstellen und Subnetze im Netzwerk hinzufügen. Angenommen, ein Netzwerk mit dem Namen *"Building1"* und fügen Sie die Subnetze oder ein *DMZ* -Netzwerk erstellen und fügen Sie alle Subnetze demilitarisierte Zone zu diesem Netzwerk gehören.

#### <a name="to-create-a-new-network"></a>Ein neues Netzwerk erstellen

1. Klicken Sie auf **Netzwerk hinzufügen** , und geben Sie den Netzwerknamen und die Beschreibung.
2.  Wählen Sie ein oder mehrere Subnetze, und klicken Sie auf **Hinzufügen**.
3. Klicken Sie auf **Speichern** , um die Konfiguration zu speichern.  
  ![Netzwerk hinzufügen](./media/log-analytics-network-performance-monitor/npm-add-network.png)



### <a name="wait-for-data-aggregation"></a>Warten auf Daten

Nachdem Sie die Konfiguration zum ersten Mal gespeichert haben, startet die Lösung Sammeln von Network Packet Verlust und Wartezeit zwischen den Knoten, in denen Agents installiert sind. Dieser Vorgang kann eine Weile dauern, manchmal über 30 Minuten. Während dieses Zustands meldet den Netzwerkleistungsmonitor untereinander die Übersichtsseite *Datenaggregation im Prozess*.

![Datenaggregation in Bearbeitung](./media/log-analytics-network-performance-monitor/npm-aggregation.png)


Wenn Daten hochgeladen wurde, sehen Sie die Netzwerkleistungsmonitor Kachel aktualisiert mit Daten.

![Netzwerk-Systemmonitor Kachel](./media/log-analytics-network-performance-monitor/npm-tile.png)

Klicken Sie zum Anzeigen des Dashboards Netzwerkleistungsmonitor.

![Netzwerk-Systemmonitor dashboard](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Bearbeiten Sie Überwachung für Subnetze

Alle Subnetze dem mindestens ein Agent installiert wurde werden auf der Registerkarte **Subnetze** auf der Konfigurationsseite aufgeführt.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Aktivieren oder Deaktivieren der Überwachung für bestimmte Subnetze

1. Aktivieren oder deaktivieren Sie das Kontrollkästchen die **Subnetz-ID** und stellen Sie sicher, dass ausgewählte, gegebenenfalls gelöschten **Überwachung** wird. Sie können aktivieren oder Deaktivieren von mehreren Subnetzen. Wenn deaktiviert, werden Teilnetze Agenten stoppen Pingen dort aktualisiert werden nicht überwacht.
2. Wählen Sie die Knoten, die Sie überwachen, für ein bestimmtes Subnetz Subnetzes aus der Liste auswählen und die erforderlichen Knoten zwischen den Listen nicht überwachte und überwachte Knoten möchten.
Sie können das Subnetz ggf. eine benutzerdefinierte **Beschreibung** hinzufügen.
3. Klicken Sie auf **Speichern** , um die Konfiguration zu speichern.  
  ![Subnetz bearbeiten](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Wählen Sie Knoten überwachen

Alle Knoten, die kein installiert Agent werden auf der Registerkarte **Knoten** aufgeführt.

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Aktivieren oder Deaktivieren der Überwachung für Knoten

1. Aktivieren oder Deaktivieren der Knoten zu überwachen Überwachung beenden.
2. Klicken Sie auf **für die Überwachung**oder deaktivieren Sie, gegebenenfalls.
3. Klicken Sie auf **Speichern**.  
  ![Aktivieren der Überwachung von Knoten](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)


### <a name="set-monitoring-rules"></a>Regeln für die Überwachung festlegen

Netzwerkleistungsmonitor generiert Systemereignisse über die Verbindung zwischen zwei Knoten oder Subnetz oder Netzwerk-Links, wenn ein Schwellenwert überschritten wird. Diese Schwellenwerte können vom System automatisch erlernt oder Sie benutzerdefinierte Warnregeln konfigurieren.

*Standardregel* wird vom System erstellt und erstellt ein Ereignis immer Verlust oder Wartezeit zwischen zwei beliebigen Netzwerke oder Subnetz Verstöße links Schwellenwert System haben. Sie können die Standardregel zu deaktivieren, und erstellen Sie benutzerdefinierte Regeln für die Überwachung

#### <a name="to-create-custom-monitoring-rules"></a>Erstellen von benutzerdefinierten Regeln für die Überwachung

1. Klicken Sie in der Registerkarte **Monitor** auf **Regel hinzufügen** und geben Sie den Regelnamen und eine Beschreibung.
2. Wählen Sie die Netzwerk- oder Subnetz Links aus den Listen zu überwachen.
3. Zuerst wählen Sie das Netzwerk in dem erste Subnetz/s an befindet aus Netzwerk und wählen Sie die Subnetz/s aus entsprechenden Subnetz.
Wählen Sie **alle Teilnetze** , wenn alle Teilnetze eine überwachen möchten. Wählen Sie entsprechend der anderen Subnetz/s an. Und klicken Sie auf **Ausnahme hinzufügen** , um bestimmte Subnetz Links vorgenommenen Auswahl Überwachung ausnehmen.
4. Wenn Sie gewählte Systemereignisse Elemente erstellen möchten, deaktivieren Sie **Systemüberwachungsereignissen links fallen diese Regel aktivieren**.
5. Wählen Sie die Überwachung.
Eingabe Schwellenwerte können Sie benutzerdefinierte Schwellenwerte für Health Generierung festlegen. Wenn der Wert der Bedingung der Schwellenwert für das ausgewählte Netzwerk-Subnetz Paar überschreitet, wird ein Ereignis generiert.
6. Klicken Sie auf **Speichern** , um die Konfiguration zu speichern.  
  ![Benutzerdefinierte Überwachung Regel erstellen](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)


## <a name="data-collection-details"></a>Einzelheiten zur Datensammlung

Netzwerk-Systemmonitor verwendet TCP-SYN-SYNACK-Handshake Bestätigungspakete Verlust sammeln und Wartezeit Informationen und Traceroute ist auch Topologieinformationen abgerufen.

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für Netzwerkleistungsmonitor.

| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
| Windows |![Ja](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Ja](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Nein](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|            ![Nein](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|![Nein](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)| TCP-Handshakes gesendet alle 5 Sekunden Daten alle 3 Minuten |

Die Lösung nutzt synthetische Transaktionen zu den Zustand des Netzwerks. OMS-Agenten installiert zu verschiedenen Netzwerk Exchange TCP-Pakete mit und dabei lernen Roundtrip Time und Pakete verloren gehen. Jeder Agent führt regelmäßig auch eine traceroute auf andere Agenten zu verschiedenen Routen im Netzwerk, die getestet werden müssen. Mithilfe dieser Daten können die Agents Netzwerklatenz und Verlustzahlen Paket abzuleiten. Die Tests werden alle fünf Sekunden wiederholt und Daten zusammengefasst für einen Zeitraum von drei Minuten die Agents vor dem Hochladen auf OMS.

>[AZURE.NOTE] Obwohl Agents häufig miteinander kommunizieren, werden sie nicht viel Netzwerkverkehr während der Durchführung der Tests generiert. Agents basieren auf TCP-SYN-SYNACK-Handshake Bestätigungspakete Verlust und Wartezeit – keine Daten Pakete ausgetauscht werden. Während dieses Vorgangs Agenten kommunizieren nur bei Bedarf und Kommunikationstopologie Agent wurde optimiert, um den Netzwerkverkehr zu reduzieren.

## <a name="using-the-solution"></a>Die Lösung

Dieser Abschnitt erläutert das Dashboard Funktionen und deren Verwendung.

### <a name="solution-overview-tile"></a>Übersicht über die Kachel Lösung

Nach Aktivierung die Netzwerkleistungsmonitor Lösung bietet die Lösung Kachel auf OMS eine schnelle Übersicht über den Netzwerkstatus. Ein Ringdiagramm zeigt die Anzahl der fehlerfreien und fehlerhaften Subnetz Links angezeigt. Beim Anklicken der Kachel wird das lösungsdashboard geöffnet.

![Netzwerk-Systemmonitor Kachel](./media/log-analytics-network-performance-monitor/npm-tile.png)


### <a name="network-performance-monitor-solution-dashboard"></a>Netzwerk-Systemmonitor lösungsdashboard

Blatt **Netzwerk Zusammenfassung** zeigt eine Zusammenfassung der Netzwerke und deren relative Größe. Dies folgt Kacheln mit insgesamt Netzwerklinks, Subnetz Hyperlinks und Pfade (ein Pfad besteht aus zwei Hosts mit Agenten und die Hops zwischen IP-Adressen) System.

**Top Netzwerk Systemereignisse** Blade enthält eine Liste der letzten Systemüberwachungsereignissen und Alarme und die Zeit seit das Ereignis hat. Ein Ereignis oder eine Warnung wird generiert, wenn Pakete verloren gehen oder Latenz einer Verknüpfung Netzwerk oder Subnetz einen Schwellenwert überschreitet.

Blade **Fehlerhafte Netzwerk-Links oben** zeigt eine Liste der fehlerhaften Netzwerk-Links. Dies sind die Netzwerklinks, die mindestens Gesundheit Ereignis für sie haben.

Blades **Oben Subnetz Links am Verlust** und **Subnetz Links die Latenzzeit** zeigt die Links oben Subnetz Paketverlust und Links oben Subnetz Latenz. Hohe Latenz oder gewisse Paketverlust auf bestimmte Netzwerklinks zu erwarten. Links in der Top-10-Listen jedoch nicht als fehlerhaft markiert.

**Allgemeine Abfragen** Blade enthält eine Reihe von Suchabfragen, die Rohdaten Netzwerküberwachungssoftware Daten direkt abgerufen werden. Diese Abfragen können als Ausgangspunkt zum Erstellen von Abfragen für benutzerdefinierte Berichte.

![Netzwerk-Systemmonitor dashboard](./media/log-analytics-network-performance-monitor/npm-dash01.png)


### <a name="drill-down-for-depth"></a>Drilldown für Tiefe

Sie können verschiedene Links Schaltpult Lösung aufgliedern tiefer in jedem Anwendungsbereich klicken. Beispielsweise, wenn eine Warnung oder eine fehlerhafte Netzwerklink in der persönlichen Menüleiste angezeigt wird, können Sie sie näher untersuchen klicken. Sie werden zu einer Seite weitergeleitet, die die Subnetz-Links für den Netzwerk-Link enthält. Sie werden den Verlust, Latenzzeit und Health Status jedes Subnetz Verknüpfung anzuzeigen und schnell herauszufinden, welche Links Subnetz verursachen das Problem. Sie können **Knoten Links anzeigen** , um alle Knoten Links für den fehlerhaften Subnetz Link anzuzeigen klicken. Dann finden Sie einzelne Knoten-Links und fehlerhaften Knoten Links finden.

Klicken Sie auf **Ansicht Topologie** Hop Hop Topologie Routen zwischen den Quell- und Knoten anzeigen. Fehlerhafte Routen oder Abschnitte werden rot angezeigt, damit Sie schnell das Problem auf einen bestimmten Teil des Netzwerks identifizieren können.

![Drilldown-Daten](./media/log-analytics-network-performance-monitor/npm-drill.png)


#### <a name="trend-charts"></a>Trenddiagramme

Auf jeder Ebene, Drilldown, sehen Sie den Trend und eine Wartezeit. Trenddiagramme stehen für Subnetz und Knoten. Sie können das Zeitintervall für das Diagramm zeichnen mit dem Zeitsteuerelement am oberen Rand des Diagramms ändern.

Trenddiagramme Anzeigen der Leistung eine Historisch gesehen. Einige Netzwerkprobleme sind vorübergehend Natur und schwer zu nur durch den aktuellen Zustand des Netzwerks. Deswegen Probleme können schnell Oberfläche und verschwindet, bevor jemand bemerkt nur, um zu einem späteren Zeitpunkt wieder angezeigt. Vorübergehende Probleme können auch für Anwendungsadministratoren schwierig, da die Probleme oft Fläche als unerklärlichen Anstieg der Reaktionszeit der Anwendung, selbst wenn alle Komponenten reibungslos angezeigt.

Sie können solche Probleme einfach durch einen Trend Diagramm, in dem das Problem als eine plötzliche Anstieg in Netzwerklatenz oder Paketverlust erscheint, erkennen.

![Trenddiagramm](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Hop Hop-Topologie

Network Performance Monitor zeigt die Topologie Hop Hop zwischen zwei Knoten in einer interaktiven Replikationstopologie. Sie können zum Anzeigen der Replikationstopologie Knoten Verbindung auswählen und dann auf **Ansicht Topologie**. Außerdem können Sie **Pfade** nebeneinander auf dem Schaltpult auf topologischen anzeigen. Wenn Sie **Pfade** auf dem Dashboard klicken, müssen Sie im linken Bedienfeld die Quell- und Ziel-Knoten auswählen und dann auf **Fläche** um die Routen zwischen den beiden Knoten darzustellen.

Die topologische Karte zeigt an, wie viele Routen zwischen zwei Knoten und sind Pfade Datenpakete nutzen. Netzwerk-Performance-Engpässe sind rot auf der topologischen Karte gekennzeichnet. Fehlerhafte Netzwerkanschluss oder fehlerhafte Netzwerkgeräte finden farbige Elemente auf der topologischen Karte ansehen.

Wenn Sie einen Knoten oder ein Mauszeiger darüber auf der topologischen Karte klicken, sehen Sie die Eigenschaften eines Knotens wie FQDN und die IP-Adresse. Klicken Sie auf einen Hop, ob diese IP-Adresse ist. Markieren bestimmte Routen durch Löschen und dann nur die Routen, die auf der Karte zu markieren. Sie können im vergrößern oder Verkleinern der Replikationstopologie mit Mausrad.

Beachten Sie, dass die in der Tabelle aufgeführte Topologie Layer 3 Topologie enthält Layer 2-Geräte und Anschlüsse.

![Hop Hop-Topologie](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Fehlerlokalisierung

Netzwerkleistungsmonitor ist die Engpässe im Netzwerk ohne die Netzwerkgeräte finden. Basierend auf den Daten, die vom Netzwerk und Algorithmen im Diagramm Netzwerk sammelt, macht Netzwerkleistungsmonitor probabilistischen Vorkalkulation Teil des Netzwerks, die wahrscheinlich die Ursache des Problems.

Diese Vorgehensweise empfiehlt sich, wann die Engpässe im Netzwerk Zugriff auf Abschnitte nicht verfügbar, da keine Daten zu Netzwerkgeräten wie Routern und Switches benötigt. Dies ist auch nützlich, wenn Hops zwischen zwei Knoten nicht in der administrativen Kontrolle. Beispielsweise können die Hops ISP-Router sein.

### <a name="log-analytics-search"></a>Protokollanalyse suchen

Alle Daten, die grafisch durch das Dashboard Netzwerkleistungsmonitor gemacht und Drilldown-Seiten steht auch in Protokollanalyse suchen. Die Daten mithilfe der Abfragesprache Suche Abfragen können und benutzerdefinierte Berichte Exportieren der Daten in Excel oder PowerBI erstellen. **Allgemeine Abfragen** Blade im Dashboard hat einige Abfragen, die Sie als Ausgangspunkt zum Erstellen eigener Abfragen und Berichte verwenden können.

![Suchabfragen](./media/log-analytics-network-performance-monitor/npm-queries.png)


## <a name="investigate-the-root-cause-of-a-health-alert"></a>Untersuchen Sie die Ursache einer Warnung health

Da Netzwerkleistungsmonitor lesen haben, betrachten wir eine einfache Untersuchung der Ursache für ein Ereignis.

1. Auf der Seite Übersicht erhalten Sie einen schnellen Überblick über den Zustand Ihres Netzwerks anhand der **Netzwerkleistungsmonitor** nebeneinander. Beachten Sie, dass aus überwachten 80 Teilnetze Links, 43 fehlerhaft sind. Dies garantiert Untersuchung. Klicken Sie zum Anzeigen des Dashboards Lösung.  
  ![Netzwerk-Systemmonitor Kachel](./media/log-analytics-network-performance-monitor/npm-investigation01.png)

2. In dem folgenden Bild sehen stehen derzeit 4 Systemüberwachungsereignissen und 4 Netzwerklinks sind fehlerhaft. Möchten das Problem untersuchen und klicken auf **Sharepoint-Web-** Netzwerk, das die Ursache des Problems herauszufinden.  
  ![fehlerhaften Link Beispiel](./media/log-analytics-network-performance-monitor/npm-investigation02.png)

3. Drilldown-Seite zeigt alle Subnetz Links in **Sharepoint -** Weblink Netzwerk. Sie werden feststellen, dass sowohl die Subnetz-links die Latenz, Netzwerkverbindung fehlerhaft Schwellenwert überschritten hat. Sie sehen auch die Latenz Trends die Subnetz-Links. Sie können die Auswahl Steuerelement im Diagramm auf den erforderlichen Zeitraum konzentrieren. Sie sehen die Tageszeit als Wartezeit Höhepunkt erreicht hat. Später können Sie Protokolle für diesen Zeitraum zu dem Problem suchen. Klicken Sie auf **Ansicht Knoten Links** weiter aufgliedern.  
  ![Fehlerhafte Subnetz Links-Beispiel](./media/log-analytics-network-performance-monitor/npm-investigation03.png)

4.  Ähnlich der vorherigen Seite listet die Drilldown-Seite für die Verknüpfung eines bestimmten Subnetzes der einzelnen Knoten Verbindungen. Ähnliche Aktionen wie im vorherigen Schritt getan haben. Klicken Sie auf **Ansicht Topologie** die Topologie 2 Knoten anzeigen.  
  ![Fehlerhafte Knoten Links-Beispiel](./media/log-analytics-network-performance-monitor/npm-investigation04.png)

5. Alle Pfade zwischen 2 ausgewählten Knoten werden auf der topologischen Karte dargestellt. Sie können die Topologie Hop Hop zwischen zwei Knoten auf der topologischen Karte anzeigen. Bietet Ihnen ein klares Bild davon, wie viele Routen zwischen zwei Knoten und gibt Pfade Datenpakete nehmen. Netzwerk-Performance-Engpässe sind rot markiert. Fehlerhafte Netzwerkanschluss oder fehlerhafte Netzwerkgeräte finden farbige Elemente auf der topologischen Karte ansehen.  
  ![Fehlerhafte Topologie Beispiel](./media/log-analytics-network-performance-monitor/npm-investigation05.png)

6. Verlust, Latenz und die Anzahl der Hops in jeder Pfad können im **Detailbereich Pfad** überprüft werden. In diesem Beispiel sehen Sie, dass 3 fehlerhafte Pfade wie im Bereich vorhanden sind. Verwenden Sie die Bildlaufleiste, um die Details dieser fehlerhafte Pfade.  Mithilfe der Kontrollkästchen einen der Pfade auswählen, sodass die Topologie nur ein Pfad gezeichnet wird. Das Mausrad können Sie vergrößern oder Verkleinern der Replikationstopologie.

  In der Abbildung klar erkennbar, die Ursache der Problembereiche zu bestimmten Abschnitt des Netzwerks der Pfade und Hops Rot. Klicken auf einen Knoten in der Topologie zeigt die Eigenschaften des Knotens, einschließlich den FQDN und die IP-Adresse. Klicken auf einen Hop zeigt die IP-Adresse des Hop.  
  ![Fehlerhafte Topologie - Beispiel der Pfad-details](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="next-steps"></a>Nächste Schritte

- [Suche Protokolle](log-analytics-log-searches.md) , Netzwerk-Performance-Datensätze anzeigen.
