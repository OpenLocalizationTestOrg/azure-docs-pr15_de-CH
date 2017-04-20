<properties
   pageTitle="Behandlung Sicherheitsvorfall in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument hilft Ihnen, Azure Security Center-Funktionen verwenden, um Sicherheitsvorfälle."
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
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="handling-security-incident-in-azure-security-center"></a>Behandlung Sicherheitsvorfall in Azure Security Center 
Selektieren und Untersuchen von Sicherheitshinweisen können für sogar die erfahrensten Sicherheitsanalysten, und viele kaum noch wissen, wo Sie beginnen. Mithilfe von [Analysen](security-center-detection-capabilities.md) zum Verknüpfen von Informationen zwischen verschiedenen [Sicherheitshinweise](security-center-managing-and-responding-alerts.md)Security Center bieten Ihnen eine Einzelansicht ein Angriff Kampagne und aller zugehörigen Alerts – schnell verstehen, welche Aktionen der Angreifer hat und welche Ressourcen betroffen sind.

Dieses Dokument erläutert das Security alert-Funktion im Sicherheitscenter verwenden Behandlung von Sicherheitsvorfällen.


## <a name="what-is-a-security-incident"></a>Was ist ein sicherheitsrelevantes Ereignis?

Im Sicherheitscenter ist ein sicherheitsrelevantes Ereignis eine Aggregation alle Alerts für eine Ressource mit [kill Kette](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) ausrichten. Ereignisse werden in den [Sicherheitshinweise](security-center-managing-and-responding-alerts.md) Kachel und Blade. Ein Vorfall zeigen die Liste der verwandten Warnungen, die Sie mehr Informationen über jedes Vorkommen können.

## <a name="managing-security-incidents"></a>Verwalten von Sicherheitsvorfällen

Überprüfen Sie anhand der Kachel Security Alerts aktuelle Sicherheitsvorfälle. Zugreifen Sie Azure, und gehen Sie zum Anzeigen weiterer Details zu jedem Sicherheitsvorfall:

1. Dashboard-Sicherheitscenter wird **Sicherheitshinweise** nebeneinander angezeigt.

    ![Sicherheitshinweise nebeneinander im Sicherheitscenter](./media/security-center-incident/security-center-incident-fig1.png)

2.  Klicken Sie auf diese Kachel und ein sicherheitsrelevantes Ereignis erkannt, erscheint unter dem Security Alerts Diagramm wie folgt erweitern:

    ![Sicherheitsvorfall](./media/security-center-incident/security-center-incident-fig2.png)

3.  Beachten Sie, dass Sicherheit Vorfall Beschreibung ein anderes Symbol im Vergleich zu anderen Alarme. Klicken sie auf Weitere Details zu diesem Ereignis anzeigen.

    ![Sicherheitsvorfall](./media/security-center-incident/security-center-incident-fig3.png)

4.  Über den **Vorfall** Blade Sie weitere finden Informationen diesen Vorfall umfasst eine vollständige Beschreibung Schweregrad (ist in diesem Fall hoch), den aktuellen Zustand (in diesem Fall ist noch *aktiv*, d. h. der Benutzer eine Aktion vorgenommen hat nicht *zu schließen* -hierzu mit der rechten Maustaste auf den Vorfall **Sicherheitshinweise** Blatt) , angegriffen Ressource (in diesem Fall *VM1*) die Behebung Schritte für den Vorfall und im unteren Bereich der Alarme, die diesen Vorfall Waren haben. Möchten Sie mehr Informationen über jede Warnung, wird Klick und ein weiteres Blatt geöffnet, wie unten dargestellt:

    ![Sicherheitsvorfall](./media/security-center-incident/security-center-incident-fig4.png)

Die Informationen auf dieser unterschiedlich Warnung. Lesen Sie [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) für Weitere Informationen zu dieser Warnung. Einige wichtige Hinweise zu dieser Funktion:

- Ein neuer Filter können Sie die Ansicht nur Vorfall und nur Alerts anpassen. 
- Dieselbe Warnung kann als Teil eines Ereignisses (falls zutreffend) sowie als Standalone-Warnung angezeigt. 
- Schließen eines Vorfalls werden verwandte Alerts nicht schließen.

## <a name="see-also"></a>Siehe auch

In diesem Dokument haben Sie gelernt, wie Security Incident Funktion im Sicherheitscenter. Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Azure Security Center Funktionen](security-center-detection-capabilities.md)
- [Planen von Azure Security Center und Betriebshandbuch](security-center-planning-and-operations-guide.md)
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Azure Security Center FAQ](security-center-faq.md)- finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)– Blog suchen Beiträge über Azure Sicherheit und Compliance.
