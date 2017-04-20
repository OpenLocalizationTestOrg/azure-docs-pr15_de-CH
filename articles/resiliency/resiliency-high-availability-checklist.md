<properties
   pageTitle="Hohe Verfügbarkeit: Checkliste | Microsoft Azure"
   description="Eine kurze Prüfliste Einstellungen und Aktionen zu verbessern die Verfügbarkeit Applikationen Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="high-availability-checklist"></a>Hohe Verfügbarkeit: Checkliste
Einer der Vorteile der Verwendung von Azure ist die Verfügbarkeit und Skalierbarkeit der Anwendung mit Hilfe der Cloud zu erhöhen. Um sicherzustellen, dass Sie die meisten dieser Optionen werden die folgende Checkliste soll Sie mit einigen Grundlagen public Key Infrastructure um sicherzustellen, dass Ihre Anwendung sind. 

>[AZURE.NOTE] Die meisten der Vorschläge Dinge, die jederzeit in Ihrer Anwendung implementiert werden kann und daher für "Quick Fixes". Die beste langfristige Lösung ist häufig ein Anwendungsdesign, das für die Cloud erstellt wird.  Eine Prüfliste für diese (Weitere Design-orientierte Bereiche, lesen Sie bitte unsere [Verfügbarkeitscheckliste](../best-practices-availability-checklist.md).

###<a name="are-you-using-traffic-manager-in-front-of-your-resources"></a>Verwenden Sie Traffic Manager vor Ihrer Ressourcen?
Mit Traffic Manager hilft Datenverkehr Sie Internet Azure-Regionen oder Azure und lokalen Speicherorten. Sie können eine Reihe von Gründen einschließlich Latenz und Verfügbarkeit dazu. Möchten Sie weitere Informationen zur Verwendung von Traffic Manager Ihre Flexibilität erhöhen und den Verkehr in mehrere Regionen, lesen Sie [VMs in mehrere Rechenzentren in Azure für hohe Verfügbarkeit ausgeführt](../guidance/guidance-compute-multiple-datacenters.md).

__Was geschieht, wenn Sie Traffic Manager verwenden?__ Wenn Sie Traffic Manager vor Ihrer Anwendung verwenden, sind Sie für die Ressourcen eines einzelnen Bereichs auf. Dies beschränkt die Skalierung, erhöht die Latenz für Benutzer, die nicht in der Nähe der ausgewählten Region und Ihr Schutz bei einem regionalen langwierige reduziert.

###<a name="have-you-avoided-using-a-single-virtual-machine-for-any-role"></a>Haben Sie mit einer einzigen virtuellen Maschine für jede Rolle vermieden?
Ein guter Entwurf vermeidet eine einzelne Fehlerquelle. Dies ist wichtig, alle Service Design (lokal oder in der Cloud) jedoch besonders in der Cloud Sie Skalierbarkeits- und Stabilität aber scaling-out (Hinzufügen von virtuellen Computern) erhöhen können, anstatt (mit leistungsfähiger VM). Wenn Sie weitere Informationen über skalierbare Anwendung entwerfen möchten, lesen Sie [hohe Verfügbarkeit bei Microsoft Azure](resiliency-high-availability-azure-applications.md).

__Was passiert, wenn einen einzelnen virtuellen Computer für eine Rolle?__ Ein Computer ist eine einzelne Fehlerquelle und ist nicht verfügbar für [Azure Virtual Machine Service Level Agreement](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/). Im besten Fall wird die Anwendung einwandfrei ausgeführt, aber das ist ein robustes Design, fällt nicht unter Azure Virtual Machine SLA und alle Einzelpunkt-Versagen erhöht das Risiko von Ausfallzeiten, wenn etwas nicht.

###<a name="are-you-using-a-load-balancer-in-front-of-your-applications-internet-facing-vms"></a>Verwenden Sie ein Lastenausgleich vor der Anwendung Internetzugriff VMs?
Lastenausgleich können Sie eingehenden Datenverkehr zur Anwendung auf einer beliebigen Anzahl von Computern verteilt. Sie können Software Maschinen aus der Lastenausgleich jederzeit die mit virtuellen Maschinen (und auch mit virtuellen Skalierung Automatische Skalierung) können Sie problemlos erhöht oder VM-Fehler. Ggf. Weitere Informationen zum Lastenausgleich lesen Sie [Azure Lastenausgleich Übersicht](../load-balancer/load-balancer-overview.md) und [mehrere VMs auf Azure für Skalierbarkeits- und ausgeführt](../guidance/guidance-compute-multi-vm.md).

__Was geschieht, wenn ein Lastenausgleich vor der Internetzugriff VMs Verwendung?__ Ohne ein Lastenausgleich wird möglicherweise nicht skalieren (mehr virtuelle Computer hinzufügen) und nur die Option skalieren (Vergrößern des virtuellen Computers Web-Schnittstelle). Sie stellen auch eine einzelne Fehlerquelle mit diesem virtuellen Computer. Sie müssen auch beachten, wenn Sie einen Computer mit Internetzugriff verloren neu ordnen den DNS-Eintrag auf dem neuen Computer gestartet werden DNS-Code schreiben.

###<a name="are-you-using-availability-sets-for-your-stateless-application-and-web-servers"></a>Sind Sie Verfügbarkeit für statusfreie Anwendung und Web Server werden?
Die Computer macht in der gleichen Anwendungsebene in einem Verfügbarkeit Ihre VMs für Azure VM SLA. Teil einer Verfügbarkeit auch gewährleistet, dass Ihre Computer sind in unterschiedliche Domänen (d. h. anderen Hostcomputern, die zu unterschiedlichen Zeiten gepatcht) und Domänen (d. h. switch-Hostcomputern, die eine gemeinsame Stromversorgung und Netzwerk). Ohne in einem Verfügbarkeit der VMs auf dem gleichen Host befinden und daher möglicherweise eine einzelne Fehlerquelle für Sie nicht sichtbar ist. Möchten Sie weitere Informationen über die Verfügbarkeit Ihrer virtuellen Computer mit Verfügbarkeit, lesen Sie [verwalten die Verfügbarkeit der virtuellen Maschinen](../virtual-machines/virtual-machines-windows-manage-availability.md).

__Was geschieht, wenn Sie eine Verfügbarkeit mit statusfreie Anwendung und Webserver festlegen verwenden?__ Nicht mit einer Verfügbarkeit festlegen bedeutet, dass Sie zu Azure VM SLA nicht. Dies bedeutet auch, dass Computer in dieser Ebene der Anwendung alle offline gehen, wenn ein Update der Host-Computer (der Computer, die der virtuelle Computer hostet Verwendung) oder eine allgemeine Hardwarefehler.

###<a name="are-you-using-virtual-machine-scale-sets-vmss-for-your-stateless-application-or-web-servers"></a>Verwenden Sie Virtual Machine Maßstab legt (VMSS) für die statusfreie Anwendung oder Web-Server?
Ein guter Entwurf skalierbarer und ausfallsicherer verwendet VMSS sicherstellen, dass Sie vergrößern/die Anzahl der Computer in einer Ebene der Anwendung (z. B. der Webebene) verkleinern können. VMSS können Sie definieren, wie die Anwendungsebene skaliert (hinzufügen oder Entfernen von Servern basierend auf ausgewählten Kriterien). Wenn Sie weitere Informationen zur Verwendung von Azure Virtual Machine Maßstab legt Ihre Stabilität bei Traffic-Spitzen erhöhen möchten, lesen Sie [Virtual Machine Skalierung legt Übersicht](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

__Was geschieht, wenn Sie uns einen virtuellen Skalierung festlegen mit statusfreie Anwendung des Webservers nicht?__ Ohne ein VMSS begrenzen Sie Ihre Fähigkeit ohne Grenzen und um Ressourcen optimal einsetzen. Eine Design, die VMSS fehlt hat eine Skalierung Obergrenze, die zusätzlichen Code (oder manuell) behandelt werden. Diese mangelnde ein VMSS bedeutet auch, dass die Anwendung nicht einfach hinzufügen und Entfernen von Maschinen (unabhängig von der Größe) helfen Spitzen Datenverkehr verarbeiten (solche während einer Beförderung oder Website/Anwendung/Produkt virale geht).

###<a name="are-you-using-premium-storage-and-separate-storage-accounts-for-each-of-your-virtual-machines"></a>Verwenden Premium-Speicher und getrennten Konten für jeden virtuellen Computer Sie?
In diesem Zusammenhang empfiehlt es sich, Premium-Speicher für die Produktion virtuellen Computer verwenden. Darüber hinaus sollten Sie sicherstellen, dass ein separates Speicherkonto für jeden virtuellen Computer verwendet werden (Dies gilt für kleine Installationen. In größeren Systemen wieder verwendbare Speicherkonten für mehrere Computer aber ist ein Lastenausgleich, die durchgeführt werden, um sicherzustellen, dass Sie über aktualisierungsdomänen und Ebenen der Anwendung ausgeglichen). Möchten Sie weitere Informationen über Azure Storage Performance und Skalierung, lesen Sie [Microsoft Azure Storage Performance und Skalierung Prüfliste](../storage/storage-performance-checklist.md).

__Was geschieht, wenn Sie nicht getrennten Konten für jeden virtuellen Computer verwenden?__ Ein Speicherkonto, wie viele Ressourcen ist eine einzelne Fehlerquelle. Sind, zwar viele Schutz und Flexibilität der Azure-Speicher ist eine einzelne Fehlerquelle nie ein gutes Design. Beispielsweise werden Zugriffsrechte, beschädigt, Konto ein Speichergrenzwert erreicht wird oder ein [IOPS beschränken](../azure-subscription-service-limits.md#virtual-machine-disk-limits) erreicht ist, alle virtuellen Computer mit diesem Storage-Konto beeinträchtigt. Außerdem ist eine langwierige, die Stempel Speicher auswirkt, der diesem bestimmten Speicherkonto enthält hätte Sie mehrere virtuelle Computer beeinträchtigt.

###<a name="are-you-using-a-load-balancer-or-a-queue-between-each-tier-of-your-application"></a>Verwenden Sie einen Lastenausgleich oder einer Warteschlange zwischen jeder Ebene der Anwendung?
Mit Lastenausgleich oder Warteschlangen zwischen jeder Ebene der Anwendung können Sie problemlos jede Anwendungsebene einfach und unabhängig voneinander skalieren. Wählen Sie zwischen diesen anhand der Wartezeit Komplexität und Verteilung (d. h. wie weit Sie Ihre Anwendung verteilen) benötigt. Im Allgemeinen Warteschlangen in der Regel höheren Latenz und Komplexität jedoch profitieren Sie stabiler wird und Sie die Anwendung über größere Bereiche verteilen (z. B. Regionen). Möchten Sie weitere Informationen zur Verwendung von internen Lastenausgleich oder Warteschlangen lesen [Interner Lastenausgleich Übersicht](../load-balancer/load-balancer-internal-overview.md) und [Azure-Warteschlangen und Service Bus Warteschlangen - verglichen und gegenübergestellt](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

__Was geschieht, wenn Sie einen Lastenausgleich oder Warteschlange zwischen jeder Anwendungsebene verwenden?__ Ohne Lastenausgleich oder Warteschlange ist zwischen den einzelnen Ebenen der Anwendung es schwierig die Anwendung nach oben oder unten und die Last auf mehrere Computer verteilen. Andernfalls führen zu über oder unter Bereitstellung von Ressourcen und Risiken Ausfallzeiten und benutzerfreundlich, haben Sie unerwartete Änderung Datenverkehr oder Systemausfällen.
 
###<a name="are-your-sql-databases-using-active-geo-replication"></a>Verwenden der SQL-Datenbanken aktive Geo-Replikation? 
Aktive Geo-Replikation können Sie bis zu 4 lesbare sekundäre Datenbanken in der gleichen oder andere Bereiche konfigurieren. Sekundäre Datenbanken stehen eine serviceunterbrechung oder eine Verbindung mit der primären Datenbank nicht möglich. Lesen Sie ggf. Weitere Informationen zu SQL-Datenbank aktive Geo-Replikation [Übersicht: SQL Datenbank aktive Geo-Replikation](../sql-database/sql-database-geo-replication-overview.md).
 
 __Was geschieht, wenn Sie aktive Geo-Replikation mit SQL-Datenbanken verwenden?__ Ohne aktive Geo-Replikation, wenn die primäre Datenbank immer offline geschaltet wird (geplante Wartungsarbeiten langwierige Hardwarefehler, usw.) der Anwendungsdatenbank werden offline bis die primäre Datenbank wieder online in einem ordnungsgemäßen Zustand zu bringen. 
 
###<a name="are-you-using-a-cache-azure-redis-cache-in-front-of-your-databases"></a>Verwenden Sie einen Cache (Azure Redis Cache) vor Ihrer Datenbanken?
Wenn die Anwendung eine hohe Datenbank laden, sind die meisten Datenbankaufrufe liest, können die Geschwindigkeit der Anwendung erhöhen und verringern Sie die Auslastung der Datenbank durch die Implementierung Zwischenspeichern Ebene vor der Datenbank diese Lesevorgänge abnimmt. Sie erhöhen die Geschwindigkeit der Anwendung und der Datenbank laden (wodurch die Skalierung behandeln) indem Zwischenspeichern Schicht vor Ihrer Datenbank verringern. Wenn Azure Redis Cache mehr erfahren möchten, lesen Sie [Zwischenspeichern Anleitung](../best-practices-caching.md).
 
 __Was geschieht, wenn Sie einen Cache vor Ihrer Datenbank verwenden?__ Datenbank leistungsstark genug, um die Auslastung zu behandeln ist setzen Sie auf dann die Anwendung normal reagiert, obwohl dies bedeuten kann, dass bei niedrigeren Sie für einen Datenbankcomputer Zahlen, die als erforderlich ist. Datenbank nicht leistungsfähige ist genug, um die Last dann starten Sie schlechte Benutzer mit Ihrer Anwendung (Latenz, Timeouts und Ausfallzeiten) auftreten.
 
###<a name="have-you-contacted-microsoft-azure-support-if-you-are-expecting-a-high-scale-event"></a>Haben Sie Microsoft Azure Support kontaktiert, wenn ein Ereignis hohe Skalierung erwarten?
Azure-Support hilft Ihnen Ihre Grenzen Service mit hoher Auslastung der geplanten Ereignisse (wie Einführung neuer Produkte oder Feiertagen) erhöhen. Azure-Support kann möglicherweise auch mit Experten herstellen, die Hilfe Anwendungsentwurf Kundenbetreuungsteam überprüfen und Ihnen die beste Lösung für hohe Skalierung Ereignis Wunsch finden können. Möchten Sie weitere Informationen zu Azure unterstützt, lesen Sie [Azure unterstützt FAQs](https://azure.microsoft.com/support/faq/).

__Was passiert bei Azure-Unterstützung nicht für ein Ereignis stark kontaktieren?__ Wenn Sie nicht kommunizieren oder planen, riskieren ein Ereignis mit hohem Datenverkehr schlagen bestimmte [Azure Services beschränkt](../azure-subscription-service-limits.md) und so eine benutzerfreundlich (oder noch schlimmer, Ausfallzeiten) während der Veranstaltung. Diese Risiken helfen Architektur Testberichte und Kommunikation vor Überspannung.

###<a name="are-you-using-a-content-delivery-network-azure-cdn-in-front-of-your-web-facing-storage-blobs-and-static-assets"></a>Verwenden Sie ein Content Delivery Network (CDN Azure) vor Web verbundene Speicher Blobs und statische Ressourcen?
Ein CDN können Sie entlasten Server durch Zwischenspeichern des Inhalts im CDN POP/Edge-Standorten, die auf der ganzen Welt befinden. Hierzu können Sie verringern die Latenz, Skalierbarkeit erhöhen, verringern die Serverlast und als Teil einer Strategie zum Schutz vor DoS-Angriffen service(DOS). Wenn Sie weitere Informationen zur Verwendung von Azure CDN Ihre Flexibilität erhöhen und verringern die Latenz Kunden möchten, lesen Sie die [Übersicht von Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md).

__Was geschieht, wenn Sie eine CDN verwenden?__ Wenn Sie einem CDN verwenden dann Datenverkehr Kunde kommt direkt auf Ihre Ressourcen. Daher finden Sie höhere Lasten auf Ihren Servern wird die Skalierung. Außerdem können Kunden höhere Wartezeiten CDNs Standorten weltweit bieten, die wahrscheinlich näher an Ihre Kunden auftreten.

##<a name="next-steps"></a>Nächste Schritte:
Möchten Sie lesen Sie weitere Informationen zum Entwurf einer Anwendung für hohe Verfügbarkeit lesen Sie [hohe Verfügbarkeit bei Microsoft Azure](resiliency-high-availability-azure-applications.md).
