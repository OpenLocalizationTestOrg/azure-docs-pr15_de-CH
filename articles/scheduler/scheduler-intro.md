<properties
 pageTitle="Was ist Azure Scheduler? | Microsoft Azure"
 description="Azure Scheduler können Sie Aktionen in der Cloud ausgeführt deklarativ beschreiben. Es plant und die Aktionen automatisch ausgeführt."
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="what-is-azure-scheduler"></a>Was ist Azure Scheduler?

Azure Scheduler können Sie Aktionen in der Cloud ausgeführt deklarativ beschreiben. Es plant und die Aktionen automatisch ausgeführt.  Planer wird [der Azure-Portal](scheduler-get-started-portal.md), Code, [REST-API](https://msdn.microsoft.com/library/mt629143.aspx)oder Azure PowerShell.

Planer erstellt, verwaltet und Arbeit aufruft.  Planer keine Arbeitslasten hosten oder kein Code ausgeführt. IT nur _Ruft_ Code an anderer Stelle gehostet, Azure, vor Ort oder bei einem anderen Anbieter. Ruft über HTTP, HTTPS, eine Speicherwarteschlange, Service Bus-Warteschlange oder ein Thema Service Bus.

Planer plant [Aufträge](scheduler-concepts-terms.md), hält einem Verlauf der Ausführung Auftragsergebnisse eine kann und deterministisch und zuverlässig plant Workloads ausgeführt werden. Azure Webaufträge (Bestandteil der Web-Apps in Azure App Service) und andere Azure Terminplanungsfunktionen verwenden Planer im Hintergrund. [Planer REST-API](https://msdn.microsoft.com/library/mt629143.aspx) unterstützt die Kommunikation für diese Aktionen verwalten. So unterstützt Planer, problemlos [komplexe Abfolgepläne und erweiterte Serie](scheduler-advanced-complexity.md) .

Es gibt mehrere Szenarien, die sich auf die Verwendung des Planers eignen. Zum Beispiel:

+ _Programmaktionen Wiederkehrend:_ In regelmäßigen Abständen sammeln Daten von Twitter Feed.
+ _Tägliche Wartung:_ Täglich Beschneiden von Protokollen, Backups und anderen Wartungsaufgaben ausführen. Beispielsweise kann ein Administrator die Datenbank um 1:00 Uhr gesichert jeden Tag für die nächsten neun Monate.

Zeitplan ermöglicht das Erstellen, aktualisieren, löschen, anzeigen und verwalten Projekte und [Sammlungen Projekt](scheduler-concepts-terms.md) programmgesteuert mithilfe von Skripts und im Portal.

## <a name="see-also"></a>Siehe auch

 [Azure Scheduler Konzepte, Terminologie und Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Planer im Azure-portal](scheduler-get-started-portal.md)

 [Pläne und Fakturierung in Azure Scheduler](scheduler-plans-billing.md)

 [Wie Sie komplexe Abfolgepläne und erweiterte Serie mit Azure Scheduler erstellen](scheduler-advanced-complexity.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler-PowerShell-Cmdlets verweisen](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Azure Scheduler Grenzen, Standards und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)
