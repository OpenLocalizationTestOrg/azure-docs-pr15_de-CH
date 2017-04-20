<properties
   pageTitle="Agent-VM in Azure Security Center aktivieren | Microsoft Azure"
   description="Dieses Dokument veranschaulicht der Azure Security Center Empfehlung **VM-Agent aktivieren**."
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
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="enable-vm-agent-in-azure-security-center"></a>Agent-VM in Azure Security Center aktivieren

VM-Agent installiert auf virtuellen Maschinen (VMs) in [Daten](security-center-enable-data-collection.md)ermöglichen.  Azure Security Center können Sie anzeigen, welche VMs VM-Agent erforderlich und werden empfohlen, den VM-Agent auf die VMs zu aktivieren.

VM-Agent ist für virtuelle Computer standardmäßig, die aus dem Markt Azure bereitgestellt werden. Artikel [VM und Extensions – Teil2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/) enthält Informationen wie den VM-Agent installieren.


> [AZURE.NOTE] Dieses Dokument stellt den Dienst mithilfe einer beispielbereitstellung. Dies ist nicht Schritt.

## <a name="implement-the-recommendation"></a>Implementieren der Empfehlung

1. Wählen Sie im **empfohlenen Blade** **VM-Agent aktivieren**.
![VM-Agent aktivieren][1]

2. Daraufhin wird das Blade **VM Agent fehlt oder nicht reagiert**. Dieses Blatt enthält VMs, die VM-Agent benötigt. Gehen Sie das Blade den VM-Agent installieren.
![VM-Agent fehlt][2]

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
[1]: ./media/security-center-enable-vm-agent/enable-vm-agent.png
[2]: ./media/security-center-enable-vm-agent/vm-agent-is-missing.png
