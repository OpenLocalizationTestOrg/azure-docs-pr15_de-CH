<properties
   pageTitle="Alarme Endpoint Protection Gesundheit in Azure Security Center | Microsoft Azure"
   description="Dieses Dokument veranschaulicht der Azure Security Center Empfehlung **Health Alerts Endgeräteschutz zu beheben**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="resolve-endpoint-protection-health-alerts-in-azure-security-center"></a>Alarme Endpoint Protection Gesundheit in Azure Security Center

Azure Security Center wird empfohlen, erkannte Endpoint Protection Health Alerts lösen.  Security Center können Sie ermitteln, welche virtuellen Computer (VMs) Endpoint Protection Fehler und wie viele Fehler.

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung. Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. **Empfohlene Blade**wählen Sie **Endpoint Protection lösen Health Alarme aus**
![Endpoint Protection Health Alarme][1]

2. Daraufhin das Blade **Endpoint Protection Fehler** der VMs mit Fehlern und die Anzahl der Fehler für jeden VM aufgelistet sind. Wählen Sie einen virtuellen Computer aus.
![EndPoint Protection Fehler][2]

3. Eine **Liste der Fehler** -Blade für den ausgewählten VM mit einer Liste der Fehler wird geöffnet. Auswählen eines Fehlers zu informieren. Daraufhin wird ein Blatt mit Informationen über den ausgewählten Fehler.
![Liste Fehler][3]
  ![Ereignis][4]

## <a name="see-also"></a>Siehe auch

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md)– Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md)– erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Sicherheit von Systemüberwachungsereignissen in Azure Security Center](security-center-monitoring.md)erfahren Sie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md)- Informationen verwalten und Sicherheitshinweise auf.
- [Überwachung Partner Solutions mit Azure Security Center](security-center-partner-solutions.md) erfahren Sie den Status Ihrer Lösungen Partner überwachen.
- [Azure Security Center FAQ](security-center-faq.md)- finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/)- Sicherheit von Azure-Neuigkeiten und Informationen.

<!--Image references-->
[1]: ./media/security-center-resolve-endpoint-protection/resolve-endpoint-protection.png
[2]: ./media/security-center-resolve-endpoint-protection/endpoint-protection-failure.png
[3]: ./media/security-center-resolve-endpoint-protection/failure-list.png
[4]: ./media/security-center-resolve-endpoint-protection/failure-event.png
