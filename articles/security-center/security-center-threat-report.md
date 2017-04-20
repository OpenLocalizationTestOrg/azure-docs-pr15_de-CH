<properties
   pageTitle="Azure Security Center Threat Intelligence Report | Microsoft Azure"
   description="Dieses Dokument hilft Ihnen Azure Security Center Bedrohung intelligenten Berichten eine Untersuchung mit Informationen über eine Warnung."
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
   ms.date="10/17/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-threat-intelligence-report"></a>Azure Security Center Threat Intelligence Report
Dieses Dokument wird erläutert, wie Azure Security Center Bedrohung intelligenten Berichten können erfahren Sie mehr über eine Bedrohung, die eine Warnung generiert.

## <a name="what-is-a-threat-intelligence-report"></a>Was ist ein Threat Intelligence-Bericht?
Sicherheitscenter Erkennung funktioniert Sicherheitsinformationen von Azure Ressourcen, Netzwerk und verbundenen Partner Solutions überwachen. Es analysiert diese Informationen häufig Informationen aus mehreren Quellen Gefahren zu korrelieren. Dieser Prozess ist Teil Sicherheitscenter [Erkennungsfunktionen](security-center-detection-capabilities.md). 

Beim Sicherheitscenter eine Bedrohung erkennt, wird eine [Warnung](security-center-managing-and-responding-alerts.md)ausgelöst enthält detaillierte Informationen zu einem bestimmten Ereignis, einschließlich Vorschläge für die Behebung. Unterstützen Vorfällen untersuchen und Beheben von Risiken, Security Center einen Threat Intelligence Bericht mit Informationen über die Bedrohung, die Informationen wie erkannt wurde, enthält die: 

- Seine Identität oder Assoziationen (sofern diese Informationen verfügbar sind)
- Angreifer Ziele
- Aktuelle und ältere angriffskampagnen (sofern diese Informationen verfügbar sind)
- Angreifer Taktiken, Tools und Verfahren
- Zugeordneten Indikatoren Kompromiss (IoC) URLs wie Hashwerte
- Viktimologie der Branche und geographische Verbreitung, um zu ermitteln, ob Ihre Azure Ressourcen gefährdet sind
- Informationen zur Risikominderung und Behebung

>[AZURE.NOTE] Die Menge der Informationen in einem bestimmten Bericht variieren; die Detailebene basiert auf Aktivität und Verbreitung der Malware.

Sicherheitscenter hat drei Arten von bedrohungsberichte Angriff variieren können. Die Berichte sind:

- **Aktivitätsbericht Gruppe**: bietet tieftauchgänge Angreifer, Ziele und Taktiken.
- **Kampagnenbericht**: konzentriert sich auf Details zu bestimmten Angriffe. 
- **Zusammenfassung der Bedrohung**: alle Elemente in der vorherigen zwei Berichte.

Diese Art von Informationen ist nützlich bei [Sicherheitsvorfällen](security-center-incident-response.md) besteht eine laufende Untersuchung die Quelle des Angriffs, der Angreifer Motivation und wie dieses Problem vorwärts zu verstehen. 

## <a name="how-to-access-the-threat-intelligence-report"></a>Wie Threat Intelligence Bericht zugreifen?

Sie können Ihre aktuelle Alerts anhand der Kachel **Sicherheitshinweise** überprüfen. Öffnen Sie Azure-Portal, und gehen Sie zum Anzeigen weiterer Details zu jeder Warnung:

1. Dashboard-Sicherheitscenter wird **Sicherheitshinweise** nebeneinander angezeigt.

2. Klicken um Blade **Sicherheitshinweise** öffnen, das Weitere Informationen über Alarme enthält, und Sicherheitshinweis zu mehr Informationen auf.

    ![Sicherheitshinweise](./media/security-center-threat-report/security-center-threat-report-fig1.png)

3. In diesem Fall zeigt das Blade **verdächtigen Prozesse ausgeführt** Details zur jeweiligen Warnung wie in der folgenden Abbildung dargestellt:

    ![Security alert bestätigungsbrief](./media/security-center-threat-report/security-center-threat-report-fig2.png)

4.  Die Menge der Informationen, die für jede Warnung variiert je nach Art der Warnung. Im Feld **Berichte** müssen Sie eine Verknüpfung zu der Threat Intelligence Report. Klicken Sie auf, und ein anderes Browserfenster erscheint mit PDF.

    ![Speicherauswahl](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Hier können Sie download die PDF-Datei für diesen Bericht und Weitere Informationen das Sicherheitsproblem entdeckt wurde und anhand der Aktionen.

## <a name="see-also"></a>Siehe auch

In diesem Dokument haben Sie gelernt, wie Azure Security Center Bedrohung intelligenten Berichten eine Untersuchung über Sicherheitshinweise helfen können. Erfahren Sie mehr über Azure Security Center finden Sie hier:

- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md). Häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Nutzung von Azure Security Center bei Vorfällen](security-center-incident-response.md)
- [Azure Security Center Funktionen](security-center-detection-capabilities.md)
- [Azure Security Center Planung und Betrieb führen](security-center-planning-and-operations-guide.md). Informationen Sie zu Vorüberlegungen Azure Security Center zu verstehen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md). Informationen Sie zum Verwalten und Sicherheitshinweise auf.
- [Behandlung Sicherheitsvorfall in Azure Security Center](security-center-incident.md)
- [Sicherheit von Azure-Blog](http://blogs.msdn.com/b/azuresecurity/). Finde Blogbeiträge zur Azure Sicherheit und Compliance.
