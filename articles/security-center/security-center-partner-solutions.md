<properties
   pageTitle="Verwalten von Partner Solutions in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument führt Sie durch wie Azure Security Center Monitor auf einen Blick können der Status Ihrer Lösungen Partner mit Abonnements Azure integriert."
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
   ms.date="10/26/2016"
   ms.author="terrylan"/>

# <a name="monitoring-partner-solutions-with-azure-security-center"></a>Überwachung mit Azure Security Center partner

Dieses Dokument führt Sie durch das Überwachen des Status Ihrer Lösungen Partner in Azure Security Center.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung. Dies ist nicht Schritt.

## <a name="monitoring-partner-solutions"></a>Überwachung partner

Die **Partner Solutions** Kachel auf der **Security Center** kann Monitor auf einen Blick der Status Ihrer Lösungen Partner mit Abonnements Azure integriert.
![Partner Solutions Kachel][1]

Die **Partner Solutions** Kachel zeigt die Anzahl der Partner Solutions und Statusübersicht für Lösungen.

Der **STATUS** einer Partner-Lösung möglich:

- Geschützt (Grün) - Es gibt keinen Zustand.
- Fehlerhaft (Rot) - Es ist, die unmittelbare Aufmerksamkeit erfordert.
- Beendet (Orange) reporting - Lösung mehr reporting ihre Gesundheit.
- Unbekannte Schutzstatus (Orange) - ist der Zustand der Lösung aufgrund einer fehlgeschlagenen Prozesses vorhandenen Lösung eine neue Ressource hinzufügen unbekannt.
- Nicht gemeldet (grau) - Lösung hat nicht gemeldet, alles noch eine Lösung Status eventuell nur verbunden wurde und dennoch bereitstellen.

Keine Lösungen integriert mit Ihrem Abonnement wird die Kachel angegeben werden, daß keine Lösungen. Auswählen der Kachel **Partner Solutions** aktivieren **Recommendations** Blade Partner Security Solutions bereitstellen zu öffnen.
![Kein Partner solutions][2]

Um den Zustand Ihrer Partner Lösungen anzuzeigen:

1. Wählen Sie das Musterelement **Partner Solutions** . Eine-Blade wird eine Liste der Partner Solutions Security Center verbunden.
![Partner solutions][3]

2. Wählen Sie eine partnerlösung. In diesem Beispiel können die **F5-WAF2** -Lösung wählen.  Eine-Blade wird geöffnet und zeigt der Status Partner und der Projektmappe zugeordneten Ressourcen. Wählen Sie **Projektmappe Konsole** Erfahrung der Partner für diese Lösung öffnen.
![Partner-Lösung detail][4]

3. **F5-WAF2** Blatt zurück, und **Link Anwendung**auswählen. **Link-Applikationen** Blatt wird geöffnet. Hier können Sie die partnerlösung Ressourcen verbinden.
![Link-Ressourcen Partner Lösung][5]

## <a name="see-also"></a>Siehe auch
In diesem Dokument wurden zum **Partner Solutions** Element im Sicherheitscenter. Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Sicherheit von Systemüberwachungsereignissen in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) – Informationen zum Verwalten und Sicherheitshinweise auf.
- [Häufig gestellte Fragen zur Azure Security Center](security-center-faq.md) – häufig gestellte Fragen zur Verwendung des Dienstes suchen.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Sicherheit von Azure-Neuigkeiten und Informationen.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png
