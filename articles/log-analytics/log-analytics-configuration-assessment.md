<properties
    pageTitle="Lösung in Protokollanalyse Konfiguration | Microsoft Azure"
    description="Die Lösung Konfiguration in Protokollanalyse stellt ausführliche Informationen über den aktuellen Status Ihrer Serverinfrastruktur System Center Operations Manager bei Verwendung von Operations Manager-Agenten oder ein Operations Manager-Verwaltungsgruppe."
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

# <a name="configuration-assessment-solution-in-log-analytics"></a>Konfiguration in Protokollanalyse Bewertung

Die Lösung Konfiguration in Protokollanalyse hilft Ihnen Server Konfigurationsprobleme durch Warnungen und Vorschläge wissen.

System Center Operations Manager erfordert. Konfiguration Bewertung ist nicht verfügbar, wenn Sie nur direkt verbundenen Agents.

Anzeigen von Informationen in Konfiguration Lösung erfordert Silverlight Plug-In für Ihren Browser.

>[AZURE.NOTE] 5. Juli 2016 Lösung die Konfiguration kann nicht hinzugefügt werden Protokollanalyse Arbeitsbereiche und dieser Lösung werden nicht mehr vorhandenen Benutzer zur nach dem 1. August 2016. Für Kunden mit dieser Lösung für SQL Server oder Active Directory empfehlen wir Sie stattdessen die [SQL Server Assessment](log-analytics-sql-assessment.md), [Active Directory-Bewertung](log-analytics-ad-assessment.md) und [Active Directory-Replikationsstatus](log-analytics-ad-replication-status.md) . Für Kunden Konfiguration Bewertung für Windows Hyper-V und System Center Virtual Machine Manager empfiehlt der Ereignissammlung verwenden und tracking-Funktionen eine ganzheitliche Ansicht der Probleme in Ihrer Umgebung zu ändern.

![Konfiguration Bewertung Kachel](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Konfigurationsdaten von überwachten Servern gesammelt und an den OMS-Dienst in der Cloud zur Verarbeitung gesendet wird. Logik wird angewendet, um die empfangenen Daten und Cloud-Dienst die Daten. Verarbeitete Daten für den Server für die folgenden Bereiche angezeigt:

- **Warnung:** Konfigurationsbezogene, proaktiven Alarme, die für die überwachten Server ausgelöst wird. Diese werden durch Microsoft Customer und Support (CSS) mit best Practices aus dem Feld erstellte Regeln erzeugt.
- **Wissen Empfehlung:** Knowledge Base-Artikel, die für Arbeitslasten empfohlen werden, die in Ihrer Infrastruktur gefunden wird. Diese werden automatisch vorgeschlagen, je nach Konfiguration durch den Einsatz der computerlernen.
- **Server und Arbeitslast analysiert:** Zeigt die Server und Arbeitslasten OMS überwacht werden.

![Konfiguration Bewertung dashboard](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### <a name="technologies-you-can-analyze-with-configuration-assessment"></a>Technologies können Sie Konfiguration Bewertung analysieren.

OMS-Konfiguration Bewertung analysiert die folgenden Arbeitslasten:

- Windows Server 2012 und Microsoft Hyper-V Server 2012
- Windows Server 2008 und Windows Server 2008 R2, einschließlich:
    - Active Directory
    - Hyper-V-host
    - Allgemeine Betriebssystem
- SQL Server 2008 oder höher
    - SQL Server-Datenbank-Engine
- Microsoft SharePoint 2010
- Microsoft Exchange Server 2010 und Microsoft Exchange Server 2013
- Microsoft Lync Server 2013 und Lync Server 2010
- System Center 2012 SP1-Virtual Machine Manager

Die folgenden 32-Bit- und 64-Bit-Editionen werden für SQL Server Analysis unterstützt:

- SQL Server 2016 - alle Versionen
- SQL Server 2014 - alle Versionen
- SQL Server-2008 und 2008 R2 - alle Versionen

Die SQL Server-Datenbank-Engine wird auf allen unterstützten Editionen analysiert. Darüber hinaus wird die 32-Bit-Edition von SQL Server im WOW64-Implementierung unterstützt.

## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung
Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Operations Manager ist für die Lösung Konfiguration erforderlich.
- Operations Manager-Agent benötigen auf jedem Computer Sie zu dessen Konfiguration werden soll.
- OMS Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md)fügen Sie die Lösung Konfiguration hinzu.  Es ist keine weitere Konfiguration erforderlich.

## <a name="configuration-assessment-data-collection-details"></a>Konfiguration Bewertung Einzelheiten zur Datensammlung

Konfiguration Bewertung sammelt Konfigurationsdaten, Metadaten und Daten mit Agenten, die Sie aktiviert haben.

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für Konfiguration.

| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Windows|![Nein](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Nein](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|            ![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| zweimal täglich|

Die folgende Tabelle enthält Beispiele von Konfiguration Bewertung gesammelten Datentypen:

|**Datentyp**|**Felder**|
|---|---|
|Konfiguration|CustomerID AgentID %EntityID, ManagedTypeID, ManagedTypePropertyID, CurrentValue ChangeDate|
|Metadaten|BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, Netzwerkname, IP-Adresse, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-Adresse, NetbiosDomainName, LogicalProcessors, DNS-Name, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Zustand|StateChangeEventId State-ID, NewHealthState, OldHealthState, Rahmen, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="configuration-assessment-alerts"></a>Konfiguration Bewertung alerts
Sie können und von der Konfiguration mit der Benachrichtigungsseite Alerts verwalten. Warnung informiert das Problem erkannt wurde, die Ursache und Lösung des Problems. Sie enthalten auch Informationen über Konfigurationen in Ihrer Umgebung, die Leistungsprobleme verursachen können.

![Warnungsansicht](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] Konfiguration Bewertung Alarme unterscheiden Alarme in Protokollanalyse. Anzeigen von Alarmen erfordert Silverlight Plug-In für Ihren Browser.

Wenn Sie ein Element oder eine Kategorie auf der Benachrichtigungsseite auswählen, sehen Sie eine Liste von Servern oder Arbeitslasten mit Alarme, die auf jedes Element angewendet.

![Liste](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Jede Warnung enthält einen Link zu einem Artikel der Microsoft Knowledge Base. Diese Artikel bieten zusätzliche Informationen.

>[AZURE.TIP] Standardmäßig ist die maximale Anzahl von Warnungen angezeigt 2.000. Um weitere Alerts anzuzeigen, klicken Sie auf die Benachrichtigung oberhalb der Liste der Warnungen.

Klicken Sie auf ein Element in der Liste im KB-Artikel anzeigen, mit der Hilfe Sie die Ursache für das Problem zu beheben, die die Warnung generiert.

![KB-Artikel](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

Das Verwalten von Warnregeln Regeln oder Klassen Regeln ignorieren.

![Warnregeln verwalten](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## <a name="knowledge-recommendations"></a>Empfohlene Kenntnisse
Beim Anzeigen von Knowledge Vorschläge Ihnen angezeigt Suchergebnissen mit Microsoft KB-Artikel empfohlen für Arbeitslasten und Computern, die zusätzliche Informationen bereitstellen.

![Suchergebnisse für Knowledge-Empfehlung](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## <a name="servers-and-workloads-analyzed"></a>Server und Arbeitslast analysiert
Beim Anzeigen von Knowledge Vorschläge Ihnen angezeigt Suchergebnissen Auflisten aller Server und Arbeitslasten OMS von Operations Manager bekannt sind.

![Server und Arbeitslasten](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Log durchsucht Protokollanalyse](log-analytics-log-searches.md) detaillierte Konfigurationsdaten anzeigen.
