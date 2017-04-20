<properties
    pageTitle="Sicherheit von Analytics anmelden | Microsoft Azure"
    description="Erfahren Sie, wie Protokollanalyse schützt Ihre Privatsphäre und Ihre Daten schützt."
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
    ms.date="09/23/2016"
    ms.author="banders"/>

# <a name="log-analytics-data-security"></a>Melden Sie sich Datenschutz Analytics

Microsoft ist bestrebt, Ihre Privatsphäre zu schützen und sichern Ihre Daten, und Software und Services, die Ihnen die IT-Infrastruktur Ihrer Organisation. Wir wissen, dass beim Übertragen der Daten vertrauen strenge Sicherheit erfordert. Microsoft befolgt strenge Richtlinien für Compliance und Sicherheit – von einem codieren.

Sichern und Schützen von Daten ist bei Microsoft. Bitte kontaktieren Sie uns Fragen, Vorschläge oder Fragen folgende einschließlich unsere Sicherheitsrichtlinien auf [Azure support-Optionen](http://azure.microsoft.com/support/options/).

Dieser Artikel beschreibt, wie Daten gesammelt, verarbeitet und Protokollanalyse in Operations Management Suite (OMS) gesichert. Agents können mit dem Webdienst verbinden, System Center Operations Manager zum Sammeln von Daten oder Abrufen von Daten von Azure Diagnostics für die Verwendung durch Protokollanalyse. Daten erfolgt über das Internet mit zertifikatbasierte Authentifizierung und SSL 3 an den Protokollanalyse-Dienst in Microsoft Azure gehostet wird. Daten werden vom Agenten komprimiert, bevor es gesendet wird.

Der Protokollanalyse-Dienst verwaltet die Cloud-basierte Daten sicher mithilfe der folgenden Methoden:

- Trennung von Daten
- datenaufbewahrung
- physische Sicherheit
- Incident management
- Compliance
- Sicherheitszertifikate standards


## <a name="data-segregation"></a>Trennung von Daten

Daten logisch auf jede Komponente in OMS-Dienst getrennt. Alle Daten pro Unternehmen gekennzeichnet ist. Diese Kennzeichnung im gesamten Datenlebenszyklus beibehalten und wird auf jeder Ebene des Diensts erzwungen. Jeder Kunde hat ein dediziertes Azure Blob, das die langfristige Daten

## <a name="data-retention"></a>Datenaufbewahrung

Indizierte Suche Daten gespeichert und nach der Zahlungsplan beibehalten. Weitere Informationen finden Sie in der [Protokolldatei Analytics Preise](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft Löscht Daten 30 Tage nach der OMS-Arbeitsbereich ist. Microsoft löscht auch das Azure Speicherkonto, wo sich die Daten befinden. Wenn Daten entfernt werden, werden keine physischen Laufwerken zerstört.

Die folgende Tabelle enthält einige der verfügbaren Lösungen OMS und welche Daten sie sammeln.

| **Lösung** | **Datentypen** |
| --- | --- |
| Konfiguration-Bewertung | Konfigurationsdaten, Metadaten und Daten |
| Planen der Kapazität | Daten und Metadaten |
| Antimalware | Konfigurationsdaten und Metadaten |
| Systemanalyse aktualisieren | Metadaten und Zustandsdaten |
| Protokoll-Management | Benutzerdefinierte Ereignisprotokolle, Windows-Ereignisprotokollen oder IIS-Protokolle |
| Versionsvergleich | Softwareinventur und Windows-Dienst-Metadaten |
| SQL und Active Directory-Bewertung | WMI-Daten, Registrierung, Leistungsdaten und dynamic SQL Server Management Ergebnisse anzeigen |

Die folgende Tabelle zeigt Beispiele für Datentypen:

| **Datentyp** | **Felder** |
| --- | --- |
| Warnung | Warnung Name, Beschreibung der Warnung, BaseManagedEntityId, Problem-ID, IsMonitorAlert, RuleId, bei, Priorität, Schweregrad, Kategorie, Besitzer, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Konfiguration | CustomerID AgentID %EntityID, ManagedTypeID, ManagedTypePropertyID, CurrentValue ChangeDate |
| Ereignis | EventId, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, Anzahl, Kategorie, ChannelLevel, LoggingComputer, EventData, EventParameter, TimeGenerated, TimeAdded <br>**Hinweis:** Wenn Sie Ereignisse mit benutzerdefinierten Feldern in das Windows-Ereignisprotokoll protokollieren, sammelt OMS. |
| Metadaten | BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, Netzwerkname, IP-Adresse, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-Adresse, NetbiosDomainName, LogicalProcessors, DNS-Name, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Leistung | Objektname, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Zustand | StateChangeEventId State-ID, NewHealthState, OldHealthState, Rahmen, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Physische Sicherheit

Protokollanalyse in OMS-Dienst von Microsoft-Mitarbeitern besetzt und alle Aktivitäten werden protokolliert und überwacht werden können. Der Dienst wird vollständig in Azure und den Azure common engineering Criteria entspricht. Sie können Details über die physische Sicherheit von Azure Ressourcen auf 18 [Microsoft Azure-Sicherheitsüberblick](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf)anzeigen. Physische Zugriffsrechte für sichere Bereiche sind innerhalb eines für jeden geändert, die mehr Verantwortung für OMS-Dienst, einschließlich und Beendigung verfügt. Über die globalen Infrastruktur finden Sie in [Microsoft-Rechenzentren](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx)verwenden.

## <a name="incident-management"></a>Incident management

OMS wurde einen Incident Managementprozess, der alle Microsoft-Dienste an. Zusammenfassend wir:

- Verwenden Sie ein Modell Verantwortung, gehört ein Teil der Verantwortung für die Sicherheit Microsoft gehört ein Teil der Kunden
- Verwalten von Azure Sicherheitsvorfälle
  - Eine Anfrage an das erste Anzeichen einer Untersuchung zu erkennen
  - Bewerten der Auswirkung und Schweregrad des Vorfalls ein Teammitglied auf Abruf Vorfällen. Basierend auf die Bewertung kann nicht führen oder weitere Eskalation an das Sicherheitsteam.
  - Incident Response Sicherheitsexperten zu den technischen oder forensische Untersuchung, Kapselung, Ausgleich und Abhilfe Strategien zu diagnostizieren. Wenn das Team glaubt, dass Daten auf eine rechtswidrige oder nicht autorisierte Person ausgesetzt werden, beginnt parallele Ausführung von Kunden Vorfall Benachrichtigungsvorgang parallel.  
  - Stabilisieren und Wiederherstellung nach dem Vorfall. Das Sicherheitsteam erstellt einen Wiederherstellungsplan zum Beheben des Problems. Schritte zur Schadensbegrenzung Katastrophen wie die betroffenen Systeme isolieren können sofort und Diagnose auftreten. Mehr Begriff Gegenmaßnahmen können geplant werden, die nach Ablauf die Gefahr auftreten.  
  - Schließen Sie des Vorfalls und durchführen Sie Post-Mortem. Das Sicherheitsteam erstellt eine Post-Mortem-, die die Informationen des Vorfalls zu Richtlinien, Verfahren und Prozesse eine Wiederholung des Ereignisses zu ändern.
- Kunden von Sicherheitsvorfällen
  - Bestimmen Sie den Umfang der betroffenen Kunden und jeder Mitteilung möglichst detaillierte beeinträchtigt wird
  - Erstellen Sie einen Hinweis zu Kunden mit genügend Informationen, damit sie am Ende eine Untersuchung durchführen und alle eingegangenen an die Endbenutzer haben erfüllen bei den Benachrichtigungsprozess nicht zu verzögern.
  - Bestätigen und den Vorfall Bedarf deklarieren.
  - Benachrichtigen Sie Kunden mit einem Vorfall ohne unangemessene Verzögerung und rechtliche oder vertragliche Verpflichtung. Benachrichtigung für Sicherheitsvorfälle werden eine oder mehrere Administratoren des Kunden mittels zugestellt wählt Microsoft einschließlich e-Mail.
- Readiness Team und Training durchführen
  - Die Mitarbeiter von Microsoft sind Sicherheit und Bewusstsein Ausbildung leichter identifizieren und melden verdächtige Sicherheitsprobleme erforderlich.  
  - Microsoft Azure Service arbeitenden Operatoren haben zusätzlich Training Pflichten ihren Zugriff auf vertrauliche Systeme mit Kundendaten.
  - Microsoft erhalten Security Response Fachausbildung für ihre Rollen


Bei jeder Kundendaten benachrichtigen wir jeden Kunden innerhalb eines Tages. Verlust von Daten ist jedoch nie mit OMS aufgetreten. Darüber hinaus pflegen wir Kopien der Daten erstellt wurde und geografisch verteilt.

Weitere Informationen über die Reaktion von Microsoft auf Sicherheitsvorfälle finden Sie unter [Microsoft Azure Security Response in der Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Compliance

Die OMS-Software Entwicklungs- und Teams Informationen Sicherheits- und Governance-Programm unterstützt die Business und wie [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) und [Microsoft Trust Center Compliance](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx)Gesetze und Vorschriften befolgt. Wie OMS richtet Sicherheitserfordernisse Sicherheitskontrollen identifiziert, verwaltet und Risiken überwacht werden auch dort beschrieben. Jährlich führen wir eine Überprüfung der Richtlinien, Standards, Verfahren und Richtlinien.

Jedes Mitglied des Entwicklungsteams OMS erhält offizielle Schulung. Verwenden Sie intern ein Versionskontrollsystem für Softwareentwicklung. Das Versionskontrollsystem jedes Softwareprojekt geschützt.

Microsoft hat ein Sicherheits- und Compliance-Team, überwacht und analysiert alle Microsoft-Dienste. Information Security Officer bis das Team und nicht mit der Technik, die OMS entwickeln verknüpft sind. Der Sicherheitsbeauftragte haben eigene Management und unabhängige Bewertung der Produkte und Dienstleistungen für Sicherheit und Compliance führen.

Microsoft Verwaltungsrat per und Bericht über alle Informationen Programme von Microsoft.

Das OMS Software-Entwicklung und Service-Team arbeitet mit Microsoft Legal und Compliance-Teams und andere verschiedener Zertifizierungen erwerben.

## <a name="security-standards-certifications"></a>Sicherheitszertifikate standards

Protokollanalyse OMS erfüllen derzeit die folgenden Sicherheitsstandards:

- [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm) und [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498) kompatibel
- Zahlung Card Industry (PCI-Compliance) Datensicherheitsstandard (PCI DSS) vom PCI Security Standards Council.
- [Service Organisation Steuerelemente (SOC) 1 1 und SOC 2 Typ1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2)
- Windows Common Engineering Criteria
- Microsoft Trustworthy Computing-Zertifizierung
- Als Azure Service einhalten Azure Auflagen Komponenten, die OMS verwendet. Mehr auf [Microsoft Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).


## <a name="cloud-computing-security-data-flow"></a>Cloud computing Sicherheitsdatenfluss
Das folgende Diagramm zeigt eine Cloud-Sicherheitsarchitektur als den Informationsfluss in Ihrem Unternehmen und wie es geschützt wird, ist der Protokollanalyse Service letztendlich sehen Sie im Portal OMS verschoben. Weitere Informationen zu jedem Schritt folgt das Diagramm.

![Bild der OMS-Datensammlung und Sicherheit](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Melden Sie sich für Protokollanalyse und Sammeln von Daten

Für Ihre Organisation Protokollanalyse Daten senden konfigurieren Sie Windows-Agenten, Agents auf Azure virtuellen Maschinen oder OMS-Agenten für Linux ausgeführt. Wenn Sie Operations Manager-Agents verwenden, werden Sie einen Konfigurations-Assistenten konfigurieren in der Betriebskonsole verwenden. Benutzer (die Sie einzelne Benutzer oder eine Gruppe von Personen möglicherweise) mindestens OMS-Konten (OMS Arbeitsbereiche) erstellen und registrieren Agents mithilfe eines der folgenden Konten:


- [Organisations-ID](../active-directory/sign-up-organization.md)

- [Microsoft-Konto - Outlook Office Live, MSN](http://www.microsoft.com/account/default.aspx)

OMS-Arbeitsbereich ist, Daten gesammelt, aggregiert, analysiert und dargestellt werden. Arbeitsbereich wird hauptsächlich als Mittel zur Partitionierung von Daten und jede Workspace ist eindeutig. Sie möchten z. B. Produktionsdaten mit einer OMS-Arbeitsbereich verwaltet und Testdaten mit einem anderen Arbeitsbereich verwaltet haben. Arbeitsbereiche dazu ein Administrator Steuern des Zugriffs auf die Daten. Jeder Arbeitsbereich kann mehrere Benutzerkonten zugeordnet und jedes Benutzerkonto kann mehrere OMS-Arbeitsbereich zugreifen. Erstellen Sie Arbeitsbereiche Datacenter Region abhängig. Jeder Arbeitsbereich auf anderen Rechenzentren in der Region hauptsächlich für OMS Verfügbarkeit repliziert.

Wenn der Konfigurations-Assistent abgeschlossen ist, stellt jede Verwaltungsgruppe Operations Manager für Operations Manager eine Verbindung mit der Protokollanalyse-Dienst. Sie verwenden dann den Assistenten zum Hinzufügen von Computern auswählen, welche Computer in der Verwaltungsgruppe Daten an den Dienst senden dürfen. Für andere Agenten verbinden die einzelnen sicher an den OMS-Dienst.

Die gesamte Kommunikation zwischen verbundenen Systemen und Protokollanalyse verschlüsselt.  Das Protokoll TLS (HTTPS) wird für die Verschlüsselung verwendet.  Das Microsoft SDL Verfahren wird damit Protokollanalyse stets mit den neuesten Fortschritten kryptografische Protokolle ist.

Jede dieser Agent sammelt Protokollanalyse. Typ der Daten hängt die Typen von Projektmappen verwendet. Sie sehen eine Datensammlung Lösungen [von Lösungskatalog Protokollanalyse hinzufügen](log-analytics-add-solutions.md). Detailliertere Informationen ist außerdem für die meisten Projektmappen verfügbar. Eine Lösung ist ein Bündel von vordefinierten Ansichten, Suchabfragen Protokoll Datenregeln und Verarbeitungslogik. Protokollanalyse können nur Administratoren eine Projektmappe importieren. Nach dem Importieren der Lösung bewegt die Operations Manager-Verwaltungsserver (falls verwendet) und anschließend alle Agents, die Sie ausgewählt haben. Die Agents, sammeln die Daten.

## <a name="2-send-data-from-agents"></a>2. senden Sie Daten von agents

Registrieren Sie alle Agents mit einer Registrierung und eine sichere Verbindung zwischen dem Agent und Protokollanalyse Service mit zertifikatbasierte Authentifizierung und SSL-Port 443. OMS verwendet einen geheimen Speicher zum Generieren und Verwalten von Schlüsseln. Private Schlüssel werden alle 90 Tage gedreht werden in Azure und von Azure Operationen, die strikte Einhaltung behördlicher Vorgehensweisen verwaltet werden.

Mit Operations Manager registrieren Sie einen Arbeitsbereich mit der Protokollanalyse und eine sichere HTTPS-Verbindung zwischen dem Operations Manager-Verwaltungsserver hergestellt.

Für Windows-Agenten in Azure virtuellen Maschinen ausgeführt wird ein Schlüssel schreibgeschützten Speicher verwendet, Ereignisse in Azure Tabellen lesen.

Wenn alle Agenten der Dienst aus irgendeinem Grund kommunizieren kann, die gesammelten Daten werden lokal in einen temporären Cache und der Verwaltungsserver die Daten 8 Minuten 2 Stunden erneut versucht. Das Betriebssystem Anmeldeinformationsspeicher der Agent zwischengespeicherten Daten geschützt. Wenn der Dienst die Daten nach 2 Stunden verarbeiten kann, werden die Agents Daten Warteschlange. Wenn die Warteschlange voll ist, beginnt OMS Datentypen mit Daten löschen. Die Begrenzung für Anforderungswarteschlange Agent ist ein Registrierungsschlüssel ändern, falls erforderlich. Daten werden komprimiert und Service für lokale Datenbanken umgehen, damit Sie nicht alle hinzugefügt wird an. Nach dem Senden der Daten wird aus dem Cache entfernt.

Wie oben beschrieben, werden Daten von Agenten über SSL an Microsoft Azure-Datencentern gesendet. ExpressRoute können Sie optional zusätzlichen Sicherheit für die Daten bereitzustellen. ExpressRoute kann eine direkte Azure aus vorhandenen WAN-Verbindung wie eine Multi-Protocol label switching (MPLS) VPN von einem Netzwerkdienstanbieter bereitgestellt. Weitere Informationen finden Sie unter [ExpressRoute](https://azure.microsoft.com/services/expressroute/).


## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. der Protokollanalyse-Dienst empfängt und verarbeitet Daten

Der Protokollanalyse-Dienst gewährleistet, dass Daten aus einer vertrauenswürdigen Quelle von Zertifikaten und die Datenintegrität mit Azure Authentifizierung überprüft. Unverarbeiteten Rohdaten werden als Blob in [Microsoft Azure Storage](../storage/storage-introduction.md) gespeichert und nicht verschlüsselt. Jede Azure-Speicher-Blob hat jedoch einen Satz der Schlüssel nur für diesen Benutzer. Der Typ der gespeicherten Daten hängt die Typen von Projektmappen, die importiert und zum Sammeln von Daten. Der Protokollanalyse-Dienst verarbeitet die unformatierten Daten für den Azure-Speicher-Blog.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Protokollanalyse für den Datenzugriff

Sie können anmelden, Protokollanalyse im Portal OMS organisatorische Konto oder Microsoft-Konto bereits haben eingerichtet. Gesamten Datenverkehr zwischen OMS-Portal und Protokollanalyse OMS wird über einen sicheren HTTPS-Kanal gesendet. OMS-Portal verwenden, wird eine Sitzung-ID auf dem Benutzer-Client (Webbrowser) generiert und Daten in einem lokalen Cache gespeichert, bis die Sitzung beendet wird. Wenn beendet, wird der Cache gelöscht. Clientseitige Cookies, die keine persönlich identifizierbaren Informationen enthalten, werden nicht automatisch entfernt. Sitzungscookies HTTPOnly gekennzeichnet und gesichert werden. Nach der festgelegten Wartezeit wird Portal OMS-Sitzung beendet.

Über das Portal OMS Daten exportieren in eine CSV-Datei und Daten suchen-APIs zugreifen. CSV-Export ist auf 50.000 Zeilen pro exportieren und API-Daten auf 5.000 Zeilen pro suchen.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren mehr über Protokollanalyse und sich innerhalb von Minuten [beginnen mit der Protokollanalyse](log-analytics-get-started.md) .
