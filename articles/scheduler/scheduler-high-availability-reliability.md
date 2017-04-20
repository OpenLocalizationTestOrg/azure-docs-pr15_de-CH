<properties
 pageTitle="Planer für hohe Verfügbarkeit und Zuverlässigkeit"
 description="Planer für hohe Verfügbarkeit und Zuverlässigkeit"
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
 ms.topic="article"
 ms.date="08/16/2016"
 ms.author="deli"/>


# <a name="scheduler-high-availability-and-reliability"></a>Planer für hohe Verfügbarkeit und Zuverlässigkeit

## <a name="azure-scheduler-high-availability"></a>Azure Scheduler hoher Verfügbarkeit

Als ein Kern Azure Platform-Dienst Azure Scheduler ist hoch verfügbar und bietet Geo redundante Bereitstellung und Geo regionale Auftrag Replikation.

### <a name="geo-redundant-service-deployment"></a>Geo-redundante Bereitstellung

Azure Scheduler wird über die Benutzeroberfläche in jedem Geo Region in Azure heute. Die Liste der Regionen Azure Scheduler für ist [hier aufgeführt](https://azure.microsoft.com/regions/#services). Wenn ein Rechenzentrum in einem gehosteten Bereich gerendert, sind Failover-Funktionen von Azure Scheduler, der Dienst von einem anderen Rechenzentrum verfügbar ist.

### <a name="geo-regional-job-replication"></a>Geo-regionalen Auftrag Replikation

Nicht nur ist der Azure-Planer-Anforderungen aber eigene Arbeit zur auch geografisch repliziert wird. Bei ein Ausfall in einer Region, Azure Scheduler Failover und stellt sicher, dass der Auftrag aus einem anderen Rechenzentrum gepaarten geografische Region ausgeführt wird.

Wenn Sie einen Auftrag im südlichen zentralen USA erstellt haben, repliziert Azure Scheduler beispielsweise automatisch den Auftrag in US North Central. Wenn ein Fehler im südlichen zentralen USA, sorgt Azure Scheduler der Auftrag von US North Central ausgeführt wird. 

![][1]

Daher sorgt Azure Scheduler Daten innerhalb der gleichen größeren geografischen Region bei Azure Fehler. Daher müssen Sie Ihre Arbeit einfach hinzufügen hohen Verfügbarkeit nicht duplizieren – Azure Scheduler Hochverfügbarkeits-Funktionen für Ihre Aufträge automatisch bereitgestellt.

## <a name="azure-scheduler-reliability"></a>Azure Scheduler Zuverlässigkeit

Azure Scheduler eigene hohe Verfügbarkeit gewährleistet und unterschiedliche Benutzer erstellte Projekte benötigt. Ihr Projekt kann z. B. einen HTTP-Endpunkt aufzurufen, der nicht verfügbar ist. Azure Scheduler versucht jedoch Ihre Arbeit erfolgreich mit alternativen mit Fehler ausgeführt. Azure Scheduler wird auf zwei Arten:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Konfigurierbare wiederholen Richtlinie über "RetryPolicy"

Azure Scheduler ermöglicht das Konfigurieren einer Richtlinie "Wiederholen". Wenn ein Auftrag fehlschlägt, versucht Planer den Auftrag erneut vier weitere Male in 30-Sekunden-Intervallen. Konfigurieren Sie können diese wiederholungsrichtlinie aggressiver (z. B. zehn Mal in Intervallen von 30 Sekunden) oder lockerer (z. B. zweimal in täglichen Intervallen.)

Beispielsweise wenn dies helfen kann können Sie einen Auftrag erstellen, der einmal pro Woche ausgeführt wird und einen HTTP-Endpunkt aufruft. Ist der HTTP-Endpunkt Sie Stunden bei der Taskausführung, möchten Sie nicht warten Sie eine Woche für den Auftrag erneut ausführen, da auch die Standardrichtlinie Wiederholung fehl. In solchen Fällen können alle 30 Sekunden wiederholungsrichtlinie standard (beispielsweise) alle drei Stunden wiederholen erneut konfigurieren.

Weitere Informationen zum Konfigurieren einer Richtlinie "Wiederholen" finden Sie in [RetryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternative Endpunkt anspruchsvolle über "ErrorAction"

Bleibt der Zielendpunkt für Ihren Azure Scheduler Job nicht erreichbar, wird Azure Scheduler Alternative zur Fehlerbehandlung Endpunkt nach dessen Wiederholung Politik. Wenn ein alternativer zur Fehlerbehandlung Endpunkt konfiguriert ist, wird Sie Azure Scheduler. Mit einem anderen Endpunkt stellen eigene hochverfügbare angesichts Fehler.

Als ein Beispiel in der Abbildung folgt Azure Scheduler Unternehmenspolitik "Wiederholen" um einen Webdienst in New York treffen. Wenn die Versuche fehlschlagen, überprüft, ob Alternative gibt. Dann weiter und Anfragen auf die alternative mit der gleichen Wiederholung beginnt.

![][2]

Beachten Sie, dass dieselbe wiederholungsrichtlinie der ursprünglichen Aktion und alternative Fehleraktion. Es ist möglich, die alternative Fehleraktion Aktionstyp Aktionstyp die Hauptaktion unterscheiden. Z. B. während hauptsächlich einen HTTP-Endpunkt aufrufen kann, die Fehleraktion stattdessen möglicherweise eine Speicherwarteschlange Service Bus-Warteschlange oder Service Bus Thema Aktion, das Protokollieren von Fehlern.

Finden Sie Informationen zum einen anderen Endpunkt konfigurieren [ErrorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Siehe auch

 [Was ist der Taskplaner?](scheduler-intro.md)

 [Azure Scheduler Konzepte, Terminologie und Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Planer im Azure-portal](scheduler-get-started-portal.md)

 [Pläne und Fakturierung in Azure Scheduler](scheduler-plans-billing.md)

 [Wie Sie komplexe Abfolgepläne und erweiterte Serie mit Azure Scheduler erstellen](scheduler-advanced-complexity.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler-PowerShell-Cmdlets verweisen](scheduler-powershell-reference.md)

 [Azure Scheduler Grenzen, Standards und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)


[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
