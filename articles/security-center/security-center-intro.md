<properties
   pageTitle="Einführung in Azure Security Center | Microsoft Azure"
   description="Azure Security Center, seine wichtigsten Funktionen und Funktionsweise erläutert."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="terrylan"/>

# <a name="introduction-to-azure-security-center"></a>Einführung in Azure Security Center

Azure Security Center, seine wichtigsten Funktionen und Funktionsweise erläutert.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.

## <a name="what-is-azure-security-center"></a>Was ist Azure Security Center?
 Security Center können Sie verhindern, erkennen und reagieren auf Gefahren mit mehr Einblick und Kontrolle über die Sicherheit von Azure-Ressourcen. Bietet integrierte Überwachung und Policy-Management über Abonnements Azure, Gefahren, die andernfalls vielleicht übersehen würden und arbeitet mit einem ausgedehnten System Security Solutions hilft bei der Ermittlung.

##  <a name="key-capabilities"></a>Wichtige Funktionen
 Security Center bietet einfach zu verwendende und effektive Bedrohung Prävention, Aufdeckung und Reaktion Funktionen, die integriert sind in Azure. Schlüsselfunktionen sind:

| | |
|----- |-----|
| Verhindern | Überwacht den Sicherheitsstatus Ihrer Azure-Ressourcen |
| | Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen anhand Ihres Unternehmens Sicherheitserfordernisse definiert die Typen, die Sie verwenden und die Vertraulichkeit Ihrer Daten |
| | Richtliniengesteuerter verwendet Sicherheitsaspekte zu Dienstbesitzer schrittweise Implementierung erforderlichen Steuerelemente |
| | Schnell stellt Sicherheitsdienste und Geräte von Microsoft und Partnern |
| Erkennen |Automatisch erfasst und analysiert Daten aus Ihrer Azure Ressourcen, Netzwerk und Partner Solutions Antimalware-Programme und firewalls |
| | Nutzt globale Bedrohung aus Microsoft-Produkten und Services, Microsoft Digital Verbrechen Einheit (Datencacheeinheit) der Microsoft Security Response Center (MSRC) und externe feeds |
| | Erweiterte Analysefunktionen, einschließlich maschinelles lernen und Verhaltensanalyse gilt |
| Antworten | Priorisierte Sicherheitshinweise/Ereignisse enthält |
| | Einblicke in die Quelle des Angriffs und betroffene Ressourcen |
| | Vorschläge zu den aktuellen Angriff verhindern von Angriffen |

## <a name="introductory-walkthrough"></a>Einführende exemplarische Vorgehensweise
 Das Sicherheitscenter wird aus dem [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)zugreifen. [Das Portal anmelden](https://portal.azure.com), **Durchsuchen**und Blättern der **Security Center** -Option wählen oder **Security Center** -Kachel, die zuvor an das Portal Dashboard angeheftet.

![Sicherheit untereinander Azure-portal][1]

Security Center können Sie Sicherheitsrichtlinien festlegen, Sicherheitskonfigurationen überwachen und Sicherheitshinweise anzeigen.

### <a name="security-policies"></a>Sicherheitsrichtlinien

Sie können Richtlinien für Ihre Azure-Abonnements und Ressourcengruppen Anforderungen Sicherheit des Unternehmens definieren. Sie können auch anpassen die Anwendungstypen, die Sie verwenden oder die Vertraulichkeit der Daten in jedem Abonnement. Beispielsweise müssen Ressourcen für Entwicklungs- oder Testzwecke verwendet andere Sicherheitsstandards gelten als für die Produktion. Ebenso können mit geregelten Personenbezogene Daten eine höhere Sicherheit benötigen.

> [AZURE.NOTE] Um eine Sicherheitsrichtlinie auf Abonnement oder Gruppenebene Ressource zu ändern, müssen Sie der Besitzer des Abonnements oder einen Teilnehmer sein.

Blatt **Security Center** wählen Sie die Kachel **Richtlinie** eine Liste Ihrer Abonnements und Ressourcengruppen.   

![Blatt Security Center][2]

Wählen Sie auf der **Sicherheitsrichtlinie** ein Abonnement, um Details zur Richtlinie.

![Security Policy Blade-Abonnement][3]

**Datensammlung** (siehe oben) ermöglicht die Datensammlung für eine Sicherheitsrichtlinie. Bietet aktivieren:

- Tägliche Überprüfung aller virtuelle Maschinen für die Sicherheit überwachen und Vorschläge unterstützt.
- Sammeln von Sicherheitsereignissen für Analyse und Bedrohung.

**Wählen Sie ein Speicherkonto pro region** (siehe oben) können Sie für jeden Bereich in der virtuellen Computern haben das Speicherkonto wählen aus diesen virtuellen Computern gesammelt Daten. Wenn Sie ein Speicherkonto für jede Region nicht auswählen, wird es für Sie erstellt. Die gesammelten Daten ist logisch von anderen Kunden Daten aus Sicherheitsgründen isoliert.

> [AZURE.NOTE] Datensammlung und wählen ein Speicherkonto pro ist Ebene Abonnement konfiguriert.

**Prevention-Richtlinien** (siehe oben) auf dem Blatt **Prevention** auswählen **Rahmen anzeigen** können Sie die Sicherheitsbedürfnisse der Ressourcen innerhalb des Abonnements zu überwachen und empfehlen Sicherheitskontrollen anhand.

Wählen Sie anschließend eine Ressourcengruppe Richtliniendetails an.

![Blade-Ressource Sicherheitsrichtliniengruppe][4]

**Vererbung** (siehe oben) können Sie die Ressourcengruppe als definieren:

- Geerbt (Standard) die Abonnementebene geerbt werden also alle Sicherheitsrichtlinien für diese Ressourcengruppe.
- Eindeutig die Ressourcengruppe bedeutet eine benutzerdefinierte Sicherheitsrichtlinie haben. Sie müssen Änderungen unter **Rahmen anzeigen**.

> [AZURE.NOTE] Besteht ein Konflikt zwischen Abonnement Ebene und Ressourcen auf, wird die Ressource auf Gruppenrichtlinien Vorrang.

### <a name="security-recommendations"></a>Sicherheitsaspekte

 Sicherheitscenter analysiert Sicherheit von Azure Ressourcen potenzielle Sicherheitsrisiken identifizieren. Eine Liste der empfohlenen führt Sie durch den Prozess des Konfigurierens benötigten Steuerelemente. Beispiele:

- Bereitstellung von Malware zu identifizieren und entfernen bösartigen software
- Konfigurieren von Netzwerk-Sicherheitsgruppen und Regeln für die Kontrolle des Datenverkehrs zu virtuellen Maschinen
- Bereitstellung von Web Application Firewalls zur Verteidigung gegen Angriffe dieses Ziel einer Anwendung
- Bereitstellen von fehlenden Systemupdates
- OS-Konfigurationen, die nicht die empfohlene Baselines Adressierung

Klicken Sie auf die Kachel **Vorschläge** für eine Liste der empfohlenen. Klicken Sie auf jede Empfehlung, um zusätzliche Informationen anzuzeigen oder Maßnahmen zur Behebung des Problems.

![Sicherheitsaspekte in Azure Security Center][5]

### <a name="resource-health"></a>Zustand der Ressourcen

**Ressource Sicherheitsstatus** Kachel zeigt den Sicherheitsstatus der Umgebung Ressourcentyp, einschließlich virtueller Maschinen, Webapplikationen und andere Ressourcen.   

Wählen Sie einen Ressourcentyp auf der Kachel **Ressource Sicherheitsstatus** auf Weitere Informationen, einschließlich einer Liste der potenziellen Sicherheitslücken, die identifiziert wurden. (**Virtuelle Computer** wird im folgenden Beispiel aktiviert.)

![Ressourcen Health Kachel][6]

### <a name="security-alerts"></a>Sicherheitshinweise

 Sicherheitscenter automatisch erfasst, analysiert und Protokolldaten Azure Ressourcen, Netzwerk und Partner Solutions Antimalware-Programme und Firewalls integriert. Wenn Viren gefunden werden, wird eine Warnung erstellt. Beispiele für Erkennung:

- Betroffenen virtuellen Maschinen mit bekannten bösartigen IP-Adressen
- Erweiterte Malware entdeckt mithilfe der Windows-Fehlerberichterstattung
- Brute-Force-Angriffe auf virtuellen Maschinen
- Sicherheitshinweise von integrierten Antimalware-Programme und firewalls

Auf der Kachel **Sicherheitshinweise** zeigt eine Liste der priorisierten Warnungen.

![Sicherheitshinweise][7]

Auswählen einer Warnung zeigt Informationen über den Angriff und Vorschläge zur Behebung dieser.

![Warnungsdetails Sicherheit][8]

### <a name="partner-solutions"></a>Partner solutions

Die Kachel **Partner Solutions** kann Monitor auf einen Blick der Status Ihrer Lösungen Partner mit Abonnements Azure integriert. Sicherheitscenter zeigt Warnungen aus der Lösung.

Wählen Sie das Musterelement **Partner Solutions** . Eine-Blade wird eine Liste aller verbundenen Partner Solutions.

![Partner solutions][9]

## <a name="get-started"></a>Erste Schritte
Zunächst mit dem Sicherheitscenter benötigen Sie ein Abonnement für Microsoft Azure. Das Sicherheitscenter ist mit Ihrem Azure-Abonnement aktiviert. Wenn Sie nicht über ein Abonnement verfügen, können Sie eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)anmelden.

 Das Sicherheitscenter wird aus dem [Azure-Portal](https://azure.microsoft.com/features/azure-portal/)zugreifen. Anzeigen Sie das [Portal Dokumentation](https://azure.microsoft.com/documentation/services/azure-portal/) mehr

[Erste Schritte mit Azure Security Center](security-center-get-started.md) führt schnell durch die Sicherheit-Überwachung und Policy-Management-Komponenten des Sicherheitscenters.

## <a name="see-also"></a>Siehe auch
In diesem Dokument wurden Security Center, seine wichtigsten Funktionen und Schritte vorgestellt. Weitere Informationen finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Sicherheit von Systemüberwachungsereignissen in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Sicherheitshinweise auf.
- [Überwachung Partner mit Azure Security Center](security-center-partner-solutions.md) – So überwachen Sie den Status Ihrer Lösungen Partner.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure-Neuigkeiten und Informationen.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png
