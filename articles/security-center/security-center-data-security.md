<properties
   pageTitle="Azure Security Center-Datenschutz | Microsoft Azure"
   description="Dieses Dokument erläutert, wie Daten verwaltet und geschützt in Azure Security Center."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-data-security"></a>Sicherheit von Azure Security Center
Damit Kunden verhindern, erkennen und reagieren auf Azure Security Center sammelt Daten über Ihre Azure Ressourcen Konfigurationsinformationen, Metadaten, Ereignisprotokolle, crash Dump- Dateien. Wir machen starke gegenüber den Datenschutz und die Sicherheit dieser Daten schützen. Microsoft befolgt strenge Richtlinien für Compliance und Sicherheit – von einem codieren. 

Dieser Artikel beschreibt, wie Daten verwaltet und geschützt in Azure Security Center.

## <a name="data-sources"></a>Datenquellen

Azure Security Center analysiert Daten aus den folgenden Quellen:

- Azure Services: Liest Informationen über die Konfiguration von Azure mit dieser Ressource Dienstanbieter bereitgestellt haben.
- Netzwerkverkehr: Lesevorgänge aufgenommenen Netzwerk-Datenverkehr Metadaten aus Microsoft Infrastruktur wie Quell-/Ziel-IP/Port Paketgröße und Netzwerkprotokoll.
- Partner Solutions: Sammelt Sicherheitshinweise integrated Partner Solutions wie Firewalls und Antimalware-Lösungen. Diese Daten werden in Azure Security Center Speicher befindet sich derzeit in den USA gespeichert.
- Die virtuellen Computer: Azure Security Center sammeln, Konfigurationsinformationen und Informationen über Sicherheitsereignisse, wie Windows-Ereignisprotokoll und Protokolle, IIS-Protokolle, Syslog-Nachrichten und Absturzabbilddateien Ihre virtuellen Maschinen mit Daten-Agenten überwachen. Abschnitt "Verwalten der Datensammlung" unten für weitere Details.  

Darüber hinaus werden Informationen Sicherheitshinweise Recommendations und Sicherheit Gesundheitsstatus in Azure Security Center Speicher befindet sich derzeit in den USA gespeichert. Dazu gehören zugehörigen Konfigurationsinformationen und Sicherheitsereignisse von Ihrer virtuellen Maschinen Sicherheitshinweis, Empfehlung oder Sicherheit Zustand Bereitstellung.

## <a name="data-protection"></a>Datenschutz

**Trennung der Daten**: Daten werden für jede Komponente der Service logisch getrennt. Alle Daten pro Unternehmen gekennzeichnet ist. Diese Kennzeichnung im gesamten Datenlebenszyklus beibehalten und wird auf jeder Ebene des Diensts erzwungen. Daten von virtuellen Computern ist darüber hinaus in Ihre Storage-Konten gespeichert.

**Datenzugriff**: um Sicherheitsaspekte und Sicherheitsrisiken untersuchen, Microsoft-Mitarbeiter Informationen gesammelt oder analysiert Azure Services, einschließlich Absturzabbilddateien zugreifen können. Absturzabbilddateien und Prozessereignisse erstellen möglicherweise versehentlich Daten oder Daten aus virtuellen Computern enthalten. Wir halten zu [Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) - [Datenschutzrichtlinie](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), die besagen, dass Microsoft nicht Daten verwenden oder Informationen zu kommerziellen Zwecken Werbung oder ähnlichen abgeleitet. Wir verwenden nur Daten als Azure Services, einschließlich Zwecke mit diesen Diensten bieten. Sie behalten alle Rechte auf Kundendaten.

**Daten**: Microsoft verwendet Muster und Bedrohungsanalyse gesehen über mehrere Mandanten zu unserer Verhütung und Funktionen; Wir tun dies Datenschutz Verpflichtungen gemäß unserer [Datenschutzgarantie](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

**Speicherort**: ein Speicherkonto angegeben, für jede Region, in dem virtuelle Computer ausgeführt werden. So können Sie zum Speichern von Daten im selben Bereich wie die virtuelle Maschine aus dem die Daten gesammelt werden. Diese Daten einschließlich Crash-Dump-Dateien werden in das Speicherkonto dauerhaft gespeichert. Der Dienst speichert auch Informationen über Sicherheitshinweise, einschließlich Warnungen integrated Partner Solutions, Vorschläge und Sicherheit Zustand in Azure Security Center Speicher derzeit in den USA befindet.

## <a name="managing-data-collection-from-virtual-machines"></a>Sammlung von virtuellen Computern verwalten

Wenn Sie Azure Security Center aktivieren möchten, eingeschaltet Datensammlung aller Abonnements. Sie können die Datensammlung im Abschnitt "Sicherheitsrichtlinie" Azure Security Center Dashboard deaktivieren. Bei eingeschaltetem Datensammlung unterstützt Azure Security Center Vorschriften Azure Monitoring Agent auf allen virtuellen Computern und keine neuen Datensätze erstellt werden. Erweiterung Azure Security Überwachung scannt auf verschiedene sicherheitsbezogene Konfigurationen und Ereignisse verfolgt in [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). Außerdem löst das Betriebssystem Ereignisprotokollereignisse Verlauf Maschine. Beispiele für solche Daten: Betriebssystem und Version, Betriebssystem-Systemprotokolle (Windows-Ereignisprotokolle) unter Prozesse, Computernamen, IP-Adressen angemeldet Benutzer und Tenant-ID. Azure Monitoring Agent liest Ereignisprotokolleinträge und ETW verfolgt und kopiert diese in das Speicherkonto für die Analyse. 

Ein Speicherkonto wird für jede Region angegeben in der virtuellen Maschinen, wo Daten von virtuellen Maschinen, die in derselben Region gespeichert haben. Dadurch für Daten im selben geografischen Gebiet für Datenschutz und Daten Hoheit. Sie können im Abschnitt "Sicherheitsrichtlinie" Azure Security Center Dashboard Speicherkonten für jeden Bereich konfigurieren.

Azure Monitoring Agent kopiert auch Absturzabbilddateien in das Speicherkonto.  Azure Security Center temporäre Kopien der Dateien für das Absturzspeicherabbild sammelt und analysiert diese Beweise Eindringversuche und erfolgreichen Angriffen.  Azure Security Center dieser Analyse der gleichen geografischen Region das Speicherkonto und löscht die temporären Kopien nach Abschluss der Analyse.

Sie können alle Agents Überwachen von Azure Security Center installiert entfernen Datensammlung von virtuellen Computern gleichzeitig deaktivieren.


## <a name="see-also"></a>Siehe auch

In diesem Dokument haben Sie wie Daten verwaltet und geschützt in Azure Security Center. Weitere Informationen zu Azure Security Center finden Sie unter:

- [Planen von Azure Security Center und Operations Guide](security-center-planning-and-operations-guide.md) – Informationen zu Vorüberlegungen Azure Security Center zu verstehen.
- [Sicherheit von Systemüberwachungsereignissen in Azure Security Center](security-center-monitoring.md) – Informationen zum Überwachen der Azure-Ressourcen
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Sicherheitshinweisen auf
- [Überwachung Partner mit Azure Security Center](security-center-partner-solutions.md) – So überwachen Sie den Status Ihrer Lösungen Partner.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Suchen über Azure Sicherheits- und Compliance-Blogbeiträge
