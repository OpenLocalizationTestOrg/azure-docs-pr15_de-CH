<properties
    pageTitle="Das Capacity Management-Lösung Protokollanalyse | Microsoft Azure"
    description="Die Planung der Lösung können in Protokollanalyse die Kapazität der Hyper-V Server verwaltet von System Center Virtual Machine Manager Ihnen helfen"
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="capacity-management-solution-in-log-analytics"></a>Protokollanalyse Capacity Management-Lösung


Die Planung der Lösung können Protokollanalyse zu der Kapazität der Hyper-V Server von System Center Virtual Machine Manager verwaltet. Diese Lösung erfordert System Center Operations Manager und System Center Virtual Machine Manager. Capacity Planning ist nicht verfügbar, wenn Sie nur direkt verbundenen Agents. Sie installieren die Lösung den Operations Manager-Agent aktualisieren. Die Lösung von Leistungsindikatoren auf dem überwachten Server liest und sendet Daten an den OMS-Dienst in der Cloud verarbeiten. Logik gilt für Daten und Cloud-Dienst die Daten. Im Laufe der Zeit Verwendungsmuster identifiziert und Kapazität projiziert, basierend auf aktuellen Verbrauch.

Beispielsweise kann eine Projektion identifizieren, zusätzliche Prozessoren oder zusätzlichen Speicher für einen einzelnen Server benötigt werden. In diesem Beispiel kann die Projektion zufolge in 30 Tagen der Server zusätzlichen Speicher benötigen. Dadurch können Sie Informationen zur Aufrüstung beim nächsten Wartungsfenster des Servers, zwei Wochen auftreten.

>[AZURE.NOTE] Die Capacity Management-Lösung ist nicht Arbeitsbereiche hinzugefügt werden. Kunden mit einer installierten Kapazität-Management-Lösung können weiterhin die Lösung.  

Die Lösung beträgt aktualisiert, der folgende Kunde Probleme gemeldet:

- Anforderung von Virtual Machine Manager und Operations Manager
- Nicht anpassen-Filter auf der Grundlage von Gruppen
- Stündliche Datenaggregation nicht häufig genug
- Keine Ebene VM-Einblicke
- Zuverlässigkeit

Vorteile neuer Kapazität:

- Unterstützt detaillierte Datensammlung mit Zuverlässigkeit und Genauigkeit
- Unterstützung für Hyper-V ohne VMM
- Visualisierung der Metrik in PowerBI
- Erkenntnisse auf VM-Ebene Auslastung


## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung
Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Operations Manager ist erforderlich für die Capacity Management-Lösung.
- Virtual Machine Manager ist erforderlich für die Capacity Management-Lösung.
- Operations Manager Konnektivität mit Virtual Machine Manager (VMM) ist erforderlich. Weitere Informationen über die Systeme finden Sie unter [VMM mit Operations Manager verbinden](http://technet.microsoft.com/library/hh882396.aspx).
- Operations Manager muss Protokollanalyse verbunden sein.
- Hinzufügen der Capacity Management-Lösung für OMS Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md).  Es ist keine weitere Konfiguration erforderlich.


## <a name="capacity-management-data-collection-details"></a>Das Capacity Management Einzelheiten zur Datensammlung

Das Capacity Management sammelt Performance-Daten, Metadaten und Daten mit Agenten, die Sie aktiviert haben.

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für das Capacity Management.

| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Windows|![Nein](./media/log-analytics-capacity/oms-bullet-red.png)|![Ja](./media/log-analytics-capacity/oms-bullet-green.png)|![Nein](./media/log-analytics-capacity/oms-bullet-red.png)|            ![Ja](./media/log-analytics-capacity/oms-bullet-green.png)|![Ja](./media/log-analytics-capacity/oms-bullet-green.png)| stündlich|

Die folgende Tabelle enthält Beispiele der Datentypen von Capacity Management erfasst:

|**Datentyp**|**Felder**|
|---|---|
|Metadaten|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, Netzwerkname, IP-Adresse, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-Adresse, NetbiosDomainName, LogicalProcessors, DNS-Name, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Leistung|Objektname, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded|
|Zustand|StateChangeEventId State-ID, NewHealthState, OldHealthState, Rahmen, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="capacity-management-page"></a>Das Capacity Management-Seite


 Nach der Installation der Lösung Capacity Planning können Sie Kapazität der überwachten Server mithilfe der Kachel **Planung** auf der Seite **Übersicht** OMS anzeigen.

![Bild nebeneinander Planung](./media/log-analytics-capacity/oms-capacity01.png)

Die Kachel Öffnet das **Capacity Management** Dashboard, eine Server-Kapazität angezeigt werden. Die Seite zeigt folgenden Kacheln, die Sie anklicken können:

- *Anzahl der virtuellen Maschine*: Zeigt die Anzahl der Tage für die Kapazität des virtuellen Maschinen
- *Berechnen*: Zeigt Prozessoren und Arbeitsspeicher
- *Speicher*: Zeigt den verwendeten Speicherplatz und durchschnittliche Datenträgerlatenz
- *Suche*: Daten-Explorer, mit denen Sie für die Suche nach Daten in OMS-System

![Bild des Capacity Management-dashboard](./media/log-analytics-capacity/oms-capacity02.png)


### <a name="to-view-a-capacity-page"></a>Zum Anzeigen einer Seite Kapazität

- Auf der Seite **Übersicht** auf **Capacity Management**und klicken Sie auf **berechnen** oder **Speicher**.

## <a name="compute-page"></a>Seite zu berechnen

Dashboard **berechnet** können Microsoft Azure OMS an Kapazitätsinformationen über Auslastung, geplante Tage der Kapazität und Effizienz im Zusammenhang mit der Infrastruktur. Mithilfe den Bereich **Ausnutzung** CPU-Kern und RAM-Nutzung virtuellen Hosts anzeigen. Das Projektionswerkzeug können Sie abschätzen, wie viel Kapazität voraussichtlich für einen bestimmten Zeitraum. **Effizienz** -Bereich können Sie sehen, wie effizient Ihre virtuellen Hosts. Durch Klicken können Sie Details zum verknüpften Elemente anzeigen.

Sie können eine Arbeitsmappe für die folgenden Kategorien:

- Top Hosts mit höchsten core
- Top Hosts mit höchsten speicherauslastung
- Top Hosts ineffizient virtuelle Computer
- Top Hosts durch Nutzung
- Untere Hosts durch Nutzung

![Bild der Seite Kapazität Management berechnen](./media/log-analytics-capacity/oms-capacity03.png)


Folgende Bereiche werden auf dem Schaltpult **berechnen** angezeigt:

**Nutzung**: Ansicht CPU Core und RAM-Nutzung virtuellen Hosts.

- *Kerne verwendet*: Summe für alle Hosts (% der CPU verwendet multipliziert mit der Anzahl der physischen Kernen auf Host).
- *Kostenlose Kerne*: Gesamtanzahl der physische Kernen minus verwendeten Kerne.
- *Prozentsatz Kerne verfügbar*: physische Kernen dividiert durch die Gesamtzahl der physischen Kernen frei.
- *Virtuelle Kerne pro VM*: virtuelle Kerne im System dividiert durch die Gesamtzahl der virtuellen Computer im System insgesamt.
- *Virtuelle Kerne physischen Kernen Verhältnis*: Verhältnis der gesamten physischen Kernen für physische Kerne, die von virtuellen Computern im System verwendet werden.
- *Anzahl der virtuellen Kerne verfügbar*: virtuelle Core physischen Kernen Verhältnis verfügbaren physischen Kernen multipliziert.
- *Verwendeten Arbeitsspeicher*: Speicher, der von allen Hosts verwendet wird.
- *Arbeitsspeicher*: Gesamter physikalischer Speicher Minus Speicher verwendet.
- *Prozent Speicher verfügbar*: Arbeitsspeicher geteilt durch den gesamten physischen Speicher frei.
- *Virtueller Arbeitsspeicher pro VM*: Gesamter virtueller Arbeitsspeicher im System dividiert durch die Gesamtzahl der virtuellen Computer im System.
- *Virtueller Speicher zum physischen Speicher-Verhältnis*: Gesamter virtueller Arbeitsspeicher im System geteilt durch den gesamten physischen Speicher im System.
- *Virtueller Speicher verfügbar*: virtueller Speicher, Arbeitsspeicher Verhältnis multipliziert mit den verfügbaren physischen Speicher.

**Projektionswerkzeug**

Mit dem Projektionswerkzeug verlaufsbezogene Trends für die Ressourcenverwendung angezeigt. Dies schließt Verwendungstrends für virtuelle Computer, Speicher, Core und Speicher. Die Projektion Funktion verwendet einen Algorithmus Projektion zu wissen, wann aus den Ressourcen ausgeführt werden. Dadurch können Sie berechnen richtige Kapazität planen, damit Sie wissen, wenn Sie mehr Kapazität (wie Speicher, Kerne oder Speicher) erwerben müssen.

**Effizienz**

- *VM im Leerlauf*: mit weniger als 10 % des Speichers CPU und 10 % für den angegebenen Zeitraum.
- *VM überlastet*: mit mehr als 90 % des Speichers CPU und 90 % für den angegebenen Zeitraum.
- *Leerlauf Host*: mit weniger als 10 % des Speichers CPU und 10 % für den angegebenen Zeitraum.
- *Server überlastet*: mit mehr als 90 % des Speichers CPU und 90 % für den angegebenen Zeitraum.

### <a name="to-work-with-items-on-the-compute-page"></a>Mit Elementen auf der Seite Compute arbeiten

1. Im Schaltpult **berechnen** im Bereich **Auslastung** zeigen Sie Kapazitätsdaten CPUs und Speicher an.
2. Klicken Sie auf ein Element auf **der Suchseite** öffnen und detaillierte Informationen anzuzeigen.
3. Im Tool **Projektion** des Schiebereglers Datum eine Projektion der Kapazität angezeigt, die auf das Datum Sie wählen.
4. Zeigen Sie im Bereich **Effizienz** Effizienz Informationen zu virtuellen Maschinen und virtuellen Hosts an

## <a name="direct-attached-storage-page"></a>Direct Attached Storage-Seite

In OMS können **Direct Attached Storage** -Dashboard Sie Informationen über speicherauslastung, Leistungsdaten und voraussichtlichen Tage Festplattenkapazität anzeigen. Verwenden Sie Bereich **Ausnutzung** Speicherplatzverwendung virtuellen Hosts anzeigen. Bereich **Leistungsdaten** können virtuellen Hosts Datenträgerdurchsatz und Wartezeit angezeigt. Das Projektionswerkzeug können Sie abschätzen, wie viel Kapazität voraussichtlich für einen bestimmten Zeitraum. Durch Klicken können Sie Details zum verknüpften Elemente anzeigen.

Sie können eine Excel-Arbeitsmappe aus dieser Kapazität für die folgenden Kategorien:

- Obere Speicherplatzverwendung Host
- Durchschnittliche Wartezeit von Host oben

Folgende Bereiche werden auf der Seite **Speicher** angezeigt:

- *Nutzung*: Speicherplatzverwendung virtuellen Hosts anzuzeigen.
- *Speicherplatz insgesamt*: Summe (logischer Speicherplatz) für alle Hosts
- *Verwendet Speicherplatz*: Summe (logischer Speicherplatz) für alle Hosts
- *Verfügbarer Speicherplatz*: Speicherplatz minus Speicherplatz insgesamt
- *Prozentsatz Datenträger verwendet*: geteilt durch Gesamtspeicherplatz Speicherplatz verwendet
- *Prozentsatz Datenträger verfügbar*: Speicherplatz geteilt durch Gesamtspeicherplatz

![Bild der Seite Kapazität Management Direct-Attached-Storage](./media/log-analytics-capacity/oms-capacity04.png)

**Leistungsdaten**

Mithilfe von OMS historisch Verwendungstrends Speicherplatz angezeigt. Die Projektion Funktion verwendet einen Algorithmus zur zukünftigen Verwendung Projekt. Speicherverwendung ermöglicht insbesondere Projektion Funktion die Ausführung Speicherplatz kann Projekt. Dies hilft Ihnen die Aufbewahrung planen und wissen, wenn mehr Speicher zu kaufen.

**Projektionswerkzeug**

Mit dem Projektionswerkzeug verlaufsbezogene Trends für Ihre Datenträgerspeicher angezeigt. Die Projektion-Funktion können Sie auch beim Ausführen von Speicherplatz Projekt. Dies hilft Ihnen die richtige Kapazität planen und wissen, wann mehr Speicherplatz kaufen.

### <a name="to-work-with-items-on-the-direct-attached-storage-page"></a>Arbeiten mit Elementen auf Direct Attached Storage

1. **Direct Attached Storage** -Schaltpult können im Bereich **Ausnutzung** Sie die Auslastung Informationen anzeigen.
2. Klicken Sie auf ein verknüpftes Element öffnen Seite **Suchen** und ausführliche Informationen anzeigen.
3. Im Bereich **Leistungsdaten** können Durchsatz und Wartezeit Informationen anzeigen.
4. **Projektionswerkzeug**des Schiebereglers Datum eine Projektion der Kapazität angezeigt, die auf das Datum Sie wählen.


## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Log durchsucht Protokollanalyse](log-analytics-log-searches.md) detaillierte Kapazitätsdaten anzeigen.
