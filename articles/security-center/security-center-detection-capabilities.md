<properties
   pageTitle="Erkennungsfunktionen in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument hilft Ihnen, Azure Security Center Erkennungsfunktionen verstehen."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-detection-capabilities"></a>Azure Security Center Funktionen
Dieses Dokument beschreibt Azure Security Center erweiterte Erkennungsfunktionen, aktive Gefahren für Ihre Microsoft Azure-Ressourcen und bietet Einblicke Reaktion erforderlich.

> [AZURE.NOTE] Erweiterte Erkennung stehen in der Standard-Tier von Azure Security Center. Eine kostenlose 90-Tage-Testversion ist verfügbar. Sie können Auswahl Preise Tier in der [Sicherheitsrichtlinie](security-center-policies.md)aktualisieren. Besuchen Sie [Security Center-Seite](https://azure.microsoft.com/pricing/details/security-center/) Weitere Informationen zu Preisen. 


## <a name="responding-to-todays-threats"></a>Auf heutige Viren reagieren
Wurden Änderungen in die Bedrohungslage in den letzten 20 Jahren. In der Vergangenheit mussten in der Regel nur von einzelnen Angreifern die Verunstaltung von Webseiten sorgen, die hauptsächlich interessiert "können sehen was sie tun". Heutigen Angreifer wesentlich raffinierteren und organisiert. Sie haben oft bestimmte Finanzziele und strategischen Ziele. Sie können weitere Ressourcen, Staaten oder Kriminalität finanziert werden können.

Dies führte zu ein unübertroffenes Maß an Professionalität der Angreifer Reihen. Nicht mehr interessiert sie Web Verunstaltung. Sie sind jetzt Diebstahl von Informationen, Konten und private Daten alle auf dem Markt generieren oder eine bestimmte Position politische und militärische nutzen können möchten. Noch mehr zu als die Angreifer mit finanziellen Ziel die Angreifer, die Netzwerke sind, Infrastruktur und Personen Schaden verletzen.

Antwort bereitstellen Unternehmen häufig verschiedene Lösungen, die auf Unternehmens oder Endpunkte anhand bekannter konzentrieren. Dazu meist umfangreicher niedrig Fidelity Alerts generieren erfordern ein Security Analyst selektieren und untersuchen. Die meisten Organisationen verfügen die Zeit und Expertise Reaktion auf diese Warnung – so viele gehen unangetastet lassen.  In der Zwischenzeit entstanden Angreifer ihre Methoden untergraben viele signaturbasierten Schutz und [Cloud-Umgebung anpassen](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/). Neue Ansätze sind erforderlich zu schneller Bedrohungen erkannt und zu beschleunigen. 

## <a name="how-azure-security-center-detects-and-responds-to-threats"></a>Wie Azure Security Center erkennt und reagiert auf Gefahren

Microsoft-Sicherheitsexperten sind immer auf der Suche nach Sicherheitsrisiken. Sie haben Zugriff auf einen umfangreichen Satz von Telemetrie gewonnenen Präsenz von Microsoft in der Cloud und lokal. Diese weitreichenden und vielfältigen Sammlung von Datasets mit der Microsoft seine Produkte für lokale Verbraucher und Unternehmen sowie die online-Dienste neue Angriffe und Trends erkennen. Sicherheitscenter können daher schnell die Algorithmen wie Angreifer neue und immer raffiniertere Angriffe werden. Dadurch können Sie die Bedrohung schnelllebigen mithalten. 

Sicherheitscenter Erkennung wird automatisch Sammeln von Sicherheitsinformationen Azure Ressourcen, Netzwerk und verbundenen Partner Solutions. Es analysiert diese Informationen häufig Informationen aus mehreren Quellen Gefahren zu korrelieren. Sicherheitshinweise sind im Sicherheitscenter und wie Sie die Bedrohung zu beseitigen priorisiert.

![Sicherheitscenter Datenerfassung und Präsentation](./media/security-center-detection-capabilities/security-center-detection-capabilities-fig1.png)

Sicherheitscenter verwendet erweiterte Sicherheit Analytics Signatur Ansätze sprengen. Durchbrüche in großen Daten und [Computer lernen](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) Techniken werden genutzt, zum Auswerten von Ereignissen über die gesamte Cloud Stoff – manuelle Verfahren und Vorhersage der Entwicklung von Angriffen möglich Gefahren erkennt. Diese Sicherheit Analysen gehören: 

- **Integrierte Bedrohungsanalyse**: sucht nach bekannten Verbrecher durch Nutzung der globalen Bedrohungsanalyse von Microsoft-Produkten und Services, Microsoft Digital Verbrechen Einheit (Datencacheeinheit) der Microsoft Security Response Center (MSRC) und externe Feeds.
- **Verhaltensbasierte Analysen**: weist ein bekanntes Muster um bösartiges Verhalten zu ermitteln. 
- **Erkennen von Netzwerkanomalien**: verwendet statistische profiling historische Baseline erstellen. In Abweichung von etablierten Basislinien einem potenziellen Angriffen entspricht benachrichtigt.


### <a name="threat-intelligence"></a>Bedrohungsanalyse
Microsoft hat eine große Anzahl von globalen Bedrohungsanalyse. Telemetrie fließt aus unterschiedlichen Quellen, wie Azure, Office 365 Microsoft CRM online, Microsoft Dynamics AX, outlook.com, MSN, Microsoft Digital Verbrechen Einheit (DCU) und Microsoft Security Response Center (MSRC). Forscher erhalten auch Threat Intelligence großen Cloud-Dienstanbieter gemeinsam verwendet wird und Informationen von Dritten Threat Intelligence Feeds abonniert. Diese Informationen können Azure Security Center Sie bekannter Verbrecher Gefahren warnen. Einige Beispiele:

- **Ausgehende Kommunikation böswilliger IP-Adresse**: ausgehender Datenverkehr an einen bekannten Botnet oder Darknet wahrscheinlich gibt an, dass die Ressource kompromittiert und Angreifer es ausführen auf diesem System oder ziehen Befehle. Azure Security Center Netzwerkverkehr Microsoft bedrohungsdatenbank vergleicht und warnt Sie Kommunikation böswilliger IP-Adresse erkennt.

## <a name="behavioral-analytics"></a>Verhaltensbasierte Analysen

Verhaltensbasierte Analyse ist eine Technik, die analysiert und vergleicht die Daten einer Auflistung von bekannten Mustern. Diese Muster sind jedoch nicht einfach Signaturen. Sie werden bestimmt durch komplexe Machine learning-Algorithmen, die für große Datasets angewendet werden. Sie werden auch durch sorgfältige Analyse der böswillige Verhalten erfahrenen Analysten bestimmt. Verhaltensbasierte Analysen können Azure Security Center um gefährdete Ressourcen basierend auf der Analyse der virtuellen Protokolle, virtuelles Netzwerk Geräteprotokolle Fabric Protokolle, Absturzabbilder und anderen Quellen zu identifizieren. 

Darüber hinaus ist die Korrelation mit anderen für eine umfassende Kampagne Belege überprüft. Diese Beziehung ermöglicht Ereignisse identifizieren, die mit Indikatoren Kompromiss. Einige Beispiele:

- **Verdächtige verarbeiten Ausführung**: Angreifer verwenden verschiedene Techniken Malware unerkannt ausführen. Beispielsweise ein Angreifer möglicherweise Malware als legitime Dateien denselben Namen geben jedoch speichern Sie diese Dateien an einem alternativen Speicherort verwenden eine harmlose Datei sehr ähnlich oder Maskieren true Endung. Sicherheitscenter Modelle Prozesse Verhaltensweisen und Monitore wie zum Erkennen von Ausreißern wie diese verarbeiten.  
- **Versteckte Malware und Ausnutzung versucht**: technisch ausgereifte Malware entziehen traditionellen Antimalware-Produkte nie Schreiben auf den Datenträger oder Verschlüsseln von Softwarekomponenten auf dem Datenträger gespeichert ist.  Jedoch kann solche Malware erkannt werden mit Speicher, wie die Malware im Speicher Spuren muss funktionieren. Wenn Software abstürzt, zeichnet ein Speicherabbild einen Teil des Speichers zum Zeitpunkt des Absturzes.  Anhand des Speichers in das Speicherabbild erkennt Azure Security Center Sicherheitslücken in Software und Zugriff auf vertrauliche Daten heimlich persist-Techniken einen befallenen Computer beeinträchtigt die Leistung des Computers.
- **Seitliche Verschiebung und interne Aufklärung**: ein gefährdeten Netzwerk- und suchen/Ernte Daten bestehen oft Angreifer seitlich von Computer für andere im gleichen Netzwerk verschieben. Sicherheitscenter überwacht Prozess und Anmelden Aktivitäten erkennen versucht ein Angreifer fassen im Netzwerk wie Remotebefehl Ausführung Netzwerk überprüfen und Konto Enumeration erweitern.
- **Böswillige PowerShell-Skripts**: PowerShell genutzt wird von Angreifern Ziel virtueller Computer für verschiedene Zwecke bösartigen Code ausführen. Das Sicherheitscenter prüft PowerShell-Aktivitäten auf verdächtige Aktivitäten. 
- **Ausgehende Angriffe**: oft Angreifer Cloudressourcen mit dem Ziel weitere Angriffe mit Ressourcen. Betroffenen virtuellen Maschinen kann z. B. brute-Force-Angriffe auf andere virtuelle Computer, SPAM zu senden oder Scannen von Anschlüssen und anderen Geräten im Internet verwendet werden. Computerlernen auf Netzwerkverkehr anwenden, erkennt Sicherheitscenter, wenn ausgehende Netzwerkkommunikation Norm überschreiten. Bei SPAM-Sicherheitscenter außerdem entspricht ungewöhnliche e-Mail-Verkehr aus Office 365 überprüfen, ob die Nachricht wahrscheinlich ist schädliche oder aus einer legitimen e-Mail-Kampagne.  

### <a name="anomaly-detection"></a>Erkennen von Netzwerkanomalien

Azure Security Center verwendet auch Normalbetriebswerte Risiken identifizieren. Im Gegensatz zu verhaltensbasierten Analyse (die bekannten Mustern Daten abgeleitet abhängig), Erkennen von Netzwerkanomalien ist "personalisierte" und konzentriert sich auf die Bereitstellung sind Baselines. Maschinelles lernen angewendet, um normale Aktivitäten für die Bereitstellung zu bestimmen und Regeln werden Ausreißer Startbedingungen definieren, die ein sicherheitsrelevantes Ereignis darstellen könnte generiert. Hier ist ein Beispiel:

- **Eingehende RDP/SSH brute-force-Angriffen**: die Bereitstellung möglicherweise ausgelastet virtuelle Computer mit Benutzernamen jeden Tag und anderen virtuellen Computern, die wenige oder Benutzernamen. Azure Security Center können geplante Loginaktivität für diese virtuellen Computer bestimmen und Computer definieren, was außerhalb der normalen Aktivität lernen. Wenn die Anzahl oder die Tageszeit Benutzernamen oder den Pfad aus dem Benutzernamen angefordert werden oder andere Login-Eigenschaften erheblich von der Baseline sind, kann eine Warnung generiert. Computerlernen bestimmt wiederum bedeutsam.

## <a name="continuous-threat-intelligence-monitoring"></a>Kontinuierliche Threat Intelligence überwachen

Azure Security Center arbeitet und Wissenschaft Sicherheitsteams, die Änderung der Bedrohungslage kontinuierlich. Dazu gehören die folgenden Initiativen:

- **Threat Intelligence überwachen**: Threat Intelligence enthält Mechanismen, Indikatoren und praktische Ratschläge zu vorhandenen oder Bedrohungen folgen. Diese Informationen in der Sicherheits-Community und Microsoft überwacht kontinuierlich Threat Intelligence Feeds aus internen und externen Quellen.
- **Signal Freigabe**: aus Sicherheitsteams über breites Microsoft Cloud und lokale Dienste, Server und Client Endgeräte freigegeben und analysiert. 
- **Microsoft-Sicherheitsexperten**: kontinuierliche Engagement mit Microsoft, die spezialisierte Felder wie Forensik und web-Angriffen.
- **Erkennung optimieren**: Algorithmen sind für Kunden Datensätze und Sicherheitsexperten arbeiten mit Kunden, um die Ergebnisse zu überprüfen. True und false Positives dienen zum Verfeinern Machine Learning-Algorithmen.

Diese Zusammenwirken führen neue und verbesserte Erkennung Sie sofort nutzen können – es ist keine Aktion für Sie zu.

## <a name="see-also"></a>Siehe auch
In diesem Dokument haben Sie gelernt, wie Azure Security Center Erkennung Funktionen arbeiten. Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Planen von Azure Security Center und Betriebshandbuch](security-center-planning-and-operations-guide.md)
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Sicherheitshinweise nach Typ in Azure Security Center](security-center-alerts-type.md)
- [Sicherheit von Systemüberwachungsereignissen in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Überwachung Partner mit Azure Security Center](security-center-partner-solutions.md) – So überwachen Sie den Status Ihrer Lösungen Partner.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – finden Sie Artikel über Azure Sicherheit und Compliance.
