<properties
   pageTitle="Anwendung Abhängigkeitsmonitor (ADM) in Operations Management Suite (OMS) | Microsoft Azure"
   description="Anwendung Abhängigkeit Monitor (ADM) ist eine Operations Management Suite (OMS) Lösung, die automatisch erkennt Anwendungskomponenten auf Windows- und Linux-Systemen und Kommunikation zwischen Karten.  Dieser Artikel enthält Einzelheiten für ADM in Ihrer Umgebung bereitstellen und verwenden ihn in verschiedenen Szenarien."
   services="operations-management-suite"
   documentationCenter=""
   authors="daseidma"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/28/2016"
   ms.author="daseidma;bwren" />

# <a name="using-application-dependency-monitor-solution-in-operations-management-suite-oms"></a>Mithilfe von Application Abhängigkeitsmonitor Lösung in Operations Management Suite (OMS)
![Management-Warnsymbol](media/operations-management-suite-application-dependency-monitor/icon.png) Anwendung Abhängigkeit Monitor (ADM) automatisch ermittelt Komponenten auf Windows und Linux und ordnet die Kommunikation zwischen Diensten. Dadurch werden die Server angezeigt, wie Sie – als verbundener Systeme, die wichtige Dienste bereitstellen.  Anwendung Abhängigkeit zeigt Verbindungen zwischen Servern, Prozesse und benötigten Ports über TCP Verbindung Architektur ohne Konfiguration als Installation eines Agents.

Dieser Artikel beschreibt die Details der Anwendung Abhängigkeitsmonitor verwenden.  Informationen zum Konfigurieren von ADM und Onboarding-Agents finden Sie unter [Konfigurieren von Anwendung Abhängigkeitsmonitor Lösung in Operations Management Suite (OMS)](operations-management-suite-application-dependency-monitor-configure.md)

>[AZURE.NOTE]Anwendung Abhängigkeitsmonitor ist derzeit in privaten Vorschau.  Sie können Zugriff auf die privaten ADM-Vorschau an [https://aka.ms/getadm](https://aka.ms/getadm)anfordern.
>
>Bei privaten Vorschau OMS-Konten haben uneingeschränkten Zugriff auf Verwaltung  ADM-Knoten können aber Protokoll Analysedaten für AdmComputer_CL und AdmProcess_CL wie andere Lösung gemessen.
>
>Nach ADM public Preview eingibt, werden zu kostenlosen und kostenpflichtigen Kunden Einblicke und Analysen in OMS Preise planen verfügbar.  Kostenlos-Konten werden nur 5 ADM-Knoten.  Wenn Sie private Vorschau teilnehmen, nicht OMS Preise Plan registriert sind tritt ADM public Preview-Version wird ADM zu diesem Zeitpunkt deaktiviert. 


## <a name="use-cases-make-your-it-processes-dependency-aware"></a>Anwendungsbeispiele: Mach die Abhängigkeit bewusst verarbeitet

### <a name="discovery"></a>Entdeckung
ADM erstellt automatisch eine gemeinsame Referenzkarte Abhängigkeiten über Server, Prozesse und 3. Dienste.  Erkennt und ordnet alle TCP-Abhängigkeiten Identifizieren von RAS-Verbindungen Überraschung 3. Systeme hängt und Abhängigkeiten traditionellen dunkle Bereiche Ihres Netzwerks wie DNS und Active Directory.  ADM ermittelt fehlgeschlagene Netzwerkschnittstellen, die die verwalteten Systeme zu helfen, potenzielle Server Fehlkonfiguration Dienstausfälle und Netzwerkprobleme identifizieren.

### <a name="incident-management"></a>Incident Management
ADM kann Problemursachen zu Rätselraten, zeigt, wie Systeme verbunden sind und einander beeinflussen.  Zusätzlich fehlgeschlagene Verbindungen zu verbundenen Clients Informationen falsch Lastenausgleich überraschend oder übermäßige Belastung wichtige Dienste identifizieren und Clients wie Entwickler Produktionssysteme mit rogue.  Integrierte Workflows mit OMS ändern können Sie sehen, ob ein Änderungsereignis auf einem Back-End-Computer auch oder Service erläutert Ursache eines Ereignisses.

### <a name="migration-assurance"></a>Migration Assurance
ADM können Sie effektiv planen, beschleunigen und Azure Migrationen überprüfen nicht zurückbleibt und keine Überraschung Ausfällen sicherstellen.  Sie können alle voneinander entdecken zusammen, Systemkonfiguration und Kapazität bewerten und identifizieren oder ein laufendes System dient noch Benutzer ist ein Kandidat für die Stilllegung statt Migration.  Danach verschieben können Sie überprüfen, Client und Identität überprüfen, dass Systeme und Kunden verbunden sind.  Wenn Ihr Subnetz Planung und Firewall-Definitionen Fragen zeigen Verbindungsfehler ADM-Maps für Systeme Ihnen die Konnektivität.

### <a name="business-continuity"></a>Business Continuity
Verwenden Sie Azure Site Recovery ist benötigen Hilfe Wiederherstellung Reihenfolge Ihrer Anwendung Umgebung ADM definieren automatisch zeigen Sie um sicherzustellen, dass wie Systeme abhängig Wiederherstellungsplans zuverlässig.  Kritische auswählen und seinen Clients anzeigen, können Sie die Front-End-Systeme identifizieren, die wiederhergestellt werden soll, erst, dass kritische Server wiederhergestellt ist.  Dagegen können Sie durch ein unternehmenswichtiger Server Back-End Abhängigkeiten betrachtet diese Systeme identifizieren, die wiederhergestellt werden müssen, bevor der Fokus wiederhergestellt wird.

### <a name="patch-management"></a>Patch-Management
ADM verbessert die Verwendung von OMS Systemanalyse Update zeigen die anderen Teams und Servern für den Dienst verlassen, dass Sie im Voraus benachrichtigen können vor Ihrer Systeme Patches nach unten.  ADM ergänzt die Patchverwaltung in OMS zeigt, ob die Dienste verfügbar und ordnungsgemäß verbunden werden korrigiert und neu gestartet. 


## <a name="mapping-overview"></a>Zuordnung (Übersicht)
ADM-Agents sammeln Informationen zu sämtlichen Prozessen TCP-Verbindung auf dem Server, auf dem sie installiert sind, sowie Einzelheiten über eingehenden und ausgehenden Verbindungen für jeden Prozess.  Liste der Computer auf der linken Seite der ADM-Lösung verwenden, können Computer mit ADM kompilierbar über einen ausgewählten Zeitraum visualisieren ausgewählt werden.  Computer Abhängigkeit wird der Fokus auf einem bestimmten Computer und alle Computer, die direkten TCP-Clients oder Server des Computers anzeigen.

![ADM-Übersicht](media/operations-management-suite-application-dependency-monitor/adm-overview.png)

Computer können der Karte ausgeführten Prozesse mit aktiven Netzwerk während des ausgewählten Zeitraums anzeigen erweitert werden.  Bei ein Remotecomputer mit einer ADM-Erweiterung Prozessdetails anzeigen werden nur die Kommunikation mit dem Fokus Prozesse angezeigt.  Die Anzahl der in den Fokus Computer ohne Agents Front-End-Computer wird auf der linken Seite der Prozesse beim Verbinden mit.  Wenn der Fokus Computer einer Verbindung zu einem Back-End-Computer ohne Agenten stellt, Back-End mit einem Knoten in der Karte die IPv4-Adresse dargestellt, und der Knoten erweitert einzelne Ports und Dienste, denen der Fokus Computer kommuniziert.

Standardmäßig anzeigen ADM-Karten die letzten 10 Minuten Abhängigkeitsinformationen.  Zeit-Steuerelemente im linken oberen verwenden, können Karten Zeit reicht bis abgefragt werden auf eine Stunde, wie Abhängigkeiten haben sich in der Vergangenheit z. B. während eines Vorfalls oder vor einer Änderung angezeigt.    ADM ist 30 Tage bezahlten Arbeitsbereiche und 7 Tage kostenlose Arbeitsbereiche Daten.

![Computer-Karte mit ausgewählten Eigenschaften](media/operations-management-suite-application-dependency-monitor/machine-map.png)

## <a name="failed-connections"></a>Verbindungsfehler
Verbindungsfehler werden ADM-Maps zu Prozessen und Computern angezeigt, mit einem gestrichelten roten Linie angezeigt Wenn ein Client-System ist ein Prozess oder Port.  Verbindungsfehler werden von einem anderen System bereitgestellte ADM-Agent gemeldet, wenn das System der Verbindungsversuch fehlgeschlagen ist.  ADM misst das ablesen, die Verbindung nicht TCP-Sockets.  Dies kann durch eine Firewall eine Fehlkonfiguration der Client, Server oder einen Remotedienst nicht verfügbar sein. 

![Verbindungsfehler](media/operations-management-suite-application-dependency-monitor/failed-connections.png)

Verbindungsfehler helfen bei der Problembehandlung Migration Validierung Sicherheitsanalyse und architektonische Verständnis verstehen.  Manchmal Fehler Verbindungen sind harmlos, aber sie oft darauf direkt ein Problem wie ein Failoverumgebung plötzlich nicht erreichbar.. oder zwei Anwendungsebenen nicht nach der Migration Cloud sprechen.  Im Bild oben IIS und WebSphere ausführen, aber kann keine Verbindung herstellen. 

## <a name="computer-and-process-properties"></a>Computer und Prozesseigenschaften
Beim Navigieren einer ADM-Karte können Sie Maschinen und Prozesse zu zusätzlichen Kontext über ihre Eigenschaften auswählen.  Computer Informationen DNS-Namen, IPv4-Adressen CPU- und Kapazität, VM-Typ, Betriebssystemversion, letzten Neustart Zeit und deren Agenten OMS und ADM-IDs.

Prozessdetails werden vom Betriebssystem ausgeführte Prozesse, einschließlich Prozessnamen, Beschreibung, Benutzername und Domäne (unter Windows), Firmenname, Produktname, Produktversion, Arbeitsverzeichnis, Befehlszeile und Prozess Startzeit Metadaten gesammelt.

![Prozesseigenschaften](media/operations-management-suite-application-dependency-monitor/process-properties.png)

Die Zusammenfassung enthält zusätzliche Informationen zu diesem Prozess Konnektivität, einschließlich gebundenen Ports, eingehende und ausgehende Verbindungen und Verbindung fehlgeschlagen. 

![Zusammenfassung](media/operations-management-suite-application-dependency-monitor/process-summary.png)

## <a name="oms-change-tracking-integration"></a>Versionsvergleich OMS-Integration
ADM Integration Change Tracking erfolgt automatisch, wenn beide Projektmappen aktiviert und OMS Arbeitsbereich konfiguriert.

Das Zusammenfassungsfeld Computer gibt an, ob Ereignisse Change Tracking auf dem ausgewählten Computer während des ausgewählten Zeitraums aufgetreten sind.

![Computer-Übersicht](media/operations-management-suite-application-dependency-monitor/machine-summary.png)

Die Änderung nachverfolgen Maschinen zeigt eine Liste aller Änderungen neueste zuerst mit einem Link zur Protokolldatei suchen Weitere Informationen analysieren.
![Change Tracking-Maschinen](media/operations-management-suite-application-dependency-monitor/machine-change-tracking.png)

Drilldown Ansicht der Konfiguration Ereignis folgt nach **Protokollanalyse anzeigen**auswählen.
![Änderungsereignis Konfiguration](media/operations-management-suite-application-dependency-monitor/configuration-change-event.png)


## <a name="log-analytics-records"></a>Analytics-Datensätze
ADM Computer Lager Daten steht für die [Suche](../log-analytics/log-analytics-log-searches.md) in Protokollanalyse.  Dies kann, einschließlich Planung, Kapazitätsanalyse Discovery und ad-hoc-Performance troubleshooting Szenarien angewendet werden. 

Ein Datensatz ist pro Stunde für jeden eindeutigen Computernamen und Prozess neben Datensätze generiert beim Prozess oder Computer startet oder auf-beherbergt, ADM  Diese Einträge haben die Eigenschaften in der folgenden Tabelle. 

Gibt es intern generierte Eigenschaften, eindeutige Prozessen und Computern zu identifizieren:

- PersistentKey_s durch die Prozesskonfiguration eindeutig definiert ist, z. B. Befehlszeile und Benutzer-ID.  Ist für einen bestimmten Computer eindeutig, jedoch kann auf Computern wiederholt werden.
- ProcessId_s und ComputerId_s sind ADM-Modell.



### <a name="admcomputercl-records"></a>AdmComputer_CL Datensätze
Datensätze mit **AdmComputer_CL** enthalten Daten für Server mit ADM.  Diese Einträge haben die Eigenschaften in der folgenden Tabelle.  

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *AdmComputer_CL* |
| SourceSystem | *OpsManager* |
| ComputerName_s | Windows- oder Linux-Computernamen |
| CPUSpeed_d | CPU-Geschwindigkeit in MHz |
| DnsNames_s | Liste aller DNS-Namen für diesen computer |
| IPv4s_s | Liste aller IPv4-Adressen von diesem computer |
| IPv6s_s | Liste aller IPv6-Adressen von diesem Computer verwendet.  (ADM bezeichnet die IPv6-Adressen jedoch erkennt keine IPv6 abhängig.) |
| Is64Bit_b | True oder false basierend auf OS |
| MachineId_s | Eine interne GUID eindeutig für einen OMS-Arbeitsbereich  |
| OperatingSystemFamily_s | Windows oder Linux |
| OperatingSystemVersion_s | Lange Zeichenfolge von OS-version |
| TimeGenerated | Datum und Uhrzeit der Erstellung des Datensatzes. |
| TotalCPUs_d | CPU-Kerne |
| TotalPhysicalMemory_d | Speicherplatz in MB |
| VirtualMachine_b | True oder false basiert, ob OS VM-Gast |
| VirtualMachineID_g | Hyper-V-VM-ID |
| VirtualMachineName_g | Hyper-V-VM-Name |
| VirtualMachineType_s | Hyper-v, Vmware, Xen, Kvm, Ldom, Lpar, Virtualpc |


### <a name="admprocesscl-type-records"></a>AdmProcess_CL Datensätze 
Datensätze mit **AdmProcess_CL** haben Inventardaten TCP verbundenen Prozesse auf Servern mit ADM.  Diese Einträge haben die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *AdmProcess_CL* |
| SourceSystem | *OpsManager* |
| CommandLine_s | Vollständige Befehlszeile des Prozesses |
| CompanyName_s | Firmenname (aus Windows PE oder Linux RPM) |
| Description_s | Lange Beschreibung (aus Windows PE oder Linux RPM) |
| FileVersion_s | Version der ausführbaren Datei (von Windows PE, nur Windows) |
| FirstPid_d | OS-Prozess-ID |
| InternalName_s | Ausführbare Datei internen Namen (aus Windows PE nur Windows) |
| MachineId_s | Interne GUID eindeutig für einen OMS-Arbeitsbereich  |
| Name_s | Der Name des ausführbaren Prozesses |
| Path_s | Dateisystempfad ausführbare Prozess |
| PersistentKey_s | Interne GUID in diesem Computer eindeutig |
| PoolId_d | Interne ID für Prozesse auf Befehl aggregieren. |
| ProcessId_s | Interne GUID eindeutig für einen OMS-Arbeitsbereich  |
| ProductName_s | Produkt-Zeichenfolge (aus Windows PE oder Linux RPM) |
| ProductVersion_s | Zeichenfolge (aus Windows PE oder Linux RPM) Produkt |
| StartTime_t | Prozess-Startzeit auf lokale Systemuhr |
| TimeGenerated | Datum und Uhrzeit der Erstellung des Datensatzes. |
| UserDomain_s | Domäne der Besitzer des Prozesses (nur Windows) |
| UserName_s | Name der Besitzer des Prozesses (nur Windows) |
| WorkingDirectory_s | Prozess-Arbeitsverzeichnis |


## <a name="sample-log-searches"></a>Beispiel-Protokoll suchen

### <a name="list-the-physical-memory-capacity-of-all-managed-computers"></a>Liste der physischen Speicherkapazität von allen verwalteten Computern. 
Typ = AdmComputer_CL | Wählen Sie TotalPhysicalMemory_d ComputerName_s | Dedup ComputerName_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-01.png)

### <a name="list-computer-name-dns-ip-and-os-version"></a>Liste von Computernamen, DNS, IP- und OS-Version.
Typ = AdmComputer_CL | Wählen Sie ComputerName_s, OperatingSystemVersion_s, DnsNames_s, IPv4s_s | Dedup ComputerName_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-02.png)

### <a name="find-all-processes-with-sql-in-the-command-line"></a>Suchen Sie aller Prozesse mit "Sql" in der Befehlszeile
Typ = AdmProcess_CL CommandLine_s = \*Sql\* | Dedup ProcessId_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-03.png)

### <a name="after-viewing-event-data-for-given-process-use-its-machine-id-to-retrieve-the-computers-name"></a>Nach dem Anzeigen von Daten für verarbeiten Sie, die Rechner ID der Computernamen ab
Type = "M!m-9bb187fa-e522-5f73-66d2-211164dc4e2b" AdmComputer_CL | Unterschiedliche ComputerName_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-04.png)

### <a name="list-all-computers-running-sql"></a>Listen Sie alle SQL-Computern
Typ = AdmComputer_CL MachineId_s IN {Type = AdmProcess_CL \*Sql\* | Unterschiedliche MachineId_s} | Unterschiedliche ComputerName_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-05.png)

### <a name="list-of-all-unique-product-versions-of-curl-in-my-datacenter"></a>Liste aller eindeutigen Produktversionen curl in meinem Rechenzentrum
Typ = AdmProcess_CL Name_s = Curl | Unterschiedliche ProductVersion_s

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-06.png)

### <a name="create-a-computer-group-of-all-computers-running-centos"></a>Erstellen einer Computergruppe aller Computer unter CentOS

![ADM-Abfragebeispiel](media/operations-management-suite-application-dependency-monitor/adm-example-07.png)



## <a name="diagnostic-and-usage-data"></a>Diagnose-und Nutzungsdaten
Microsoft sammelt automatisch Verwendung und Leistung durch die Verwendung der Anwendung Abhängigkeit Überwachungsdienst. Microsoft verwendet diese Daten zur Verfügung und verbessert die Qualität und Integrität der Anwendung Abhängigkeit Überwachungsdienst. Daten enthält Informationen über die Konfiguration des Betriebssystems und Version sowie IP-Adresse, DNS-Namen und Name der Arbeitsstation um genaue und effiziente Fehlerbehebung bereitstellen. Wir sammeln keine Namen, Adressen oder andere Kontaktinformationen.

Weitere Informationen über die Datensammlung und Verwendung finden Sie unter der [Microsoft Online Services-Datenschutzrichtlinie](hhttps://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Suchvorgänge protokollieren](../log-analytics/log-analytics-log-searches.md] in Log Analytics to retrieve data collected by Application Dependency Monitor.)
