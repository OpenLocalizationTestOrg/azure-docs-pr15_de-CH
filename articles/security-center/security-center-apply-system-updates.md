<properties
   pageTitle="Systemupdates in Azure Security Center anwenden | Microsoft Azure"
   description="Dieses Dokument veranschaulicht die Implementierung Azure Security Center Recommendations **System installieren** und **nach System-Updates neu starten**."
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

# <a name="apply-system-updates-in-azure-security-center"></a>Systemupdates in Azure Security Center anwenden

Azure Security Center überwacht täglich Windows und Linux VMs (VMs) für Betriebssystem fehlt. Sicherheitscenter Ruft eine Liste der verfügbaren Sicherheitsupdates und Updates von Windows Update oder Windows Server Update Services (WSUS) je nach Dienst auf Windows VM konfiguriert.  Sicherheitscenter prüft auch die neuesten Updates in Linux-Systeme. VM Systemupdate fehlen, wird das Sicherheitscenter empfiehlt Systemupdates anzuwenden

> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung.  Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Wählen Sie Blatt **Recommendations** **Systemupdates übernehmen**.
![System-Updates anwenden][1]

2. **Systemupdates übernehmen** Blade wird eine Liste von VMs Systemupdates fehlt. Wählen Sie einen virtuellen Computer.
![Wählen Sie einen virtuellen Computer][2]

3. Eine-Blade wird eine Liste der fehlenden Updates für diesen virtuellen Computer. Wählen Sie ein System aktualisieren. In diesem Beispiel wählen wir KB3156016.
![Fehlende Sicherheitsupdates][3]

4. Gehen Sie in das **Sicherheitsupdate** Blade mit fehlenden Updates.
![Sicherheitsupdate][4]

## <a name="reboot-after-system-updates"></a>Neustart nach System-updates

5. Zurück zu Blade **Recommendations** . Ein neuer Eintrag wurde erstellt, nachdem Systemupdates aufgerufen **Neustart nach System-Updates**angewendet. Dieser Eintrag ermöglicht müssen VM zum Abschließen der Anwendung System-Updates neu starten.
![Neustart nach System-updates][5]

6. Wählen Sie **nach System-Updates neu starten**. Daraufhin wird **abgeschlossen System-Updates ein Neustart bevorsteht** Blade mit einer Liste der VMs, die Sie neu starten, um die übernehmen System Updates abzuschließen.
![Neustart ausstehend][6]

Starten Sie die VM von Azure, den Prozess abzuschließen.

## <a name="see-also"></a>Siehe auch

Erfahren Sie mehr über das Sicherheitscenter finden Sie hier:

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center](security-center-policies.md) – Informationen zum Konfigurieren von Sicherheitsrichtlinien für Ihren Azure-Abonnements und Ressourcengruppen.
- [Verwalten von Sicherheitsaspekte in Azure Security Center](security-center-recommendations.md) – erfahren Sie, wie Recommendations Azure Ressourcen schützen.
- [Security Health monitoring in Azure Security Center](security-center-monitoring.md) – erfahren Sie, wie Ihre Azure Ressourcen überwachen.
- [Verwalten von und reagieren auf Sicherheitshinweise in Azure Security Center](security-center-managing-and-responding-alerts.md) - Informationen verwalten und Sicherheitshinweise auf.
- [Überwachung Partner Solutions mit Azure Security Center](security-center-partner-solutions.md) erfahren Sie den Status Ihrer Lösungen Partner überwachen.
- [Azure Security Center FAQ](security-center-faq.md) - finden Sie häufig gestellte Fragen zur Verwendung des Dienstes.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – Blog suchen Beiträge über Azure Sicherheit und Compliance.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/recommendation.png
[2]:./media/security-center-apply-system-updates/select-vm.png
[3]: ./media/security-center-apply-system-updates/missing-security-updates.png
[4]: ./media/security-center-apply-system-updates/security-update.png
[5]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[6]: ./media/security-center-apply-system-updates/restart-pending.png
