<properties
   pageTitle="Resiliency Checkliste | Microsoft Azure"
   description="Prüfliste erfahren, die für Stabilität Probleme beim Entwurf."
   services=""
   documentationCenter="na"
   authors="petertaylor9999"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="petertay"/>

# <a name="azure-resiliency-guidance-resiliency-checklist"></a>Azure Resiliency Anleitung: Resiliency-Checkliste

Entwerfen der Anwendung Robustheit erfordert Planung und Verringerung der verschiedenen Fehler, die auftreten können. Überprüfen Sie die Elemente in dieser Prüfliste für den Anwendungsentwurf stabiler machen.

## <a name="requirements"></a>Vorschriften

- **Definieren der Verfügbarkeit des Kunden.** Ihr Kunde Verfügbarkeit an Komponenten in Ihrer Anwendung und dies wirkt sich auf die Anwendung. Abkommen von Ihrem Kunden für die Verfügbarkeitsziele jedes Ihrer Anwendung, andernfalls der Entwurf nicht erwartet der Kunde entsprechen. Weitere Informationen finden Sie im Abschnitt [Definition der Stabilität](guidance-resiliency-overview.md#defining-your-resiliency-requirements) des [Entwurfs einer stabilen Anwendung für Azure](guidance-resiliency-overview.md) Dokuments.

## <a name="failure-mode-analysis"></a>Fehleranalyse Modus

- **Führen Sie eine Fehleranalyse Modus (FMA) für die Anwendung.** FMA ist ein Prozess zum Erstellen von Stabilität in einer Anwendung frühzeitig in der Entwurfsphase. Die Ziele einer FMA gehören:  

    - Welche Arten von Fehlern auftreten kann, eine Anwendung zu identifizieren. 
    - Erfassen Sie Effekte und potenzielle Auswirkung der einzelnen Fehler in der Anwendung.
    - Wiederherstellungsstrategien zu identifizieren. 

    Weitere Informationen finden Sie unter [sowie Anträge Azure entwerfen: Fehleranalyse Modus][fma].  

## <a name="application"></a>Anwendung

- **Bereitstellen Sie mehrere Instanzen von Diensten.** Dienste zwangsläufig nicht, und wenn die Anwendung eine Instanz eines Dienstes hängt zwangsläufig schlägt auch. Um mehrere Instanzen für [Azure App Service](../app-service/app-service-value-prop-what-is.md)bereitzustellen, wählen Sie eine [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) , die mehrere Instanzen bietet. Konfigurieren Sie für Azure Cloud Services die Rollen [mehrere Instanzen](../cloud-services/cloud-services-choose-me.md#scaling-and-management)verwenden. [Azure virtuelle Maschinen (VMs)](../virtual-machines/virtual-machines-windows-about.md)sicher, dass der VM-Architektur mehrere VM umfasst und jede VM ein [Verfügbarkeit]gehört[availability-sets].   

- **Verwenden Sie ein Lastenausgleich Anfragen zu verteilen.** Ein Lastenausgleich verteilt Anträge auf fehlerfreie Dienstinstanzen Drehung fehlerhafte Instanzen entfernen. Wenn Ihr Dienst Azure App Service oder Azure Cloud Services verwendet, ist es bereits Lastenausgleich für Sie. Wenn Ihre Azure VMs Anwendung müssen Sie ein System zum Lastenausgleich bereitstellen. Finden Sie die Einzelheiten [Azure Lastenausgleich](../load-balancer/load-balancer-overview.md) . 

- **Konfigurieren Sie Azure Anwendungsgateways um mehrere Instanzen verwenden.** Je nach Anwendung kann ein [Azure Application Gateway](../application-gateway/application-gateway-introduction.md) besser zu Anfragen an die Anwendung Dienste geeignet. Jedoch einzelne Instanzen des Dienstes Application Gateway keine SLA garantiert ist es möglich, dass Ihre Anwendung nicht konnte, schlägt die Application Gateway-Instanz. Bereitstellen Sie mittleren oder größeren Application Gateway mehrmals garantieren die Verfügbarkeit des Dienstes gemäß [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/).

- **Mit Verfügbarkeit legt für jede Anwendungsebene**. Platzieren Instanzen in einer [Verfügbarkeit] [ availability-sets] Verbindung mit mindestens einer VM-Instanz im Rahmen der [SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_2/)garantiert. Wenn der virtuelle Computer keiner Verfügbarkeit nicht garantiert Schutz und kann fehlschlagen alle oder gleichzeitig aktualisiert werden. 

- **Erwägen Sie die Anwendung in mehreren Regionen.** Wenn die Anwendung auf einen einzelnen Bereich bereitgestellt, in dem seltenen Fall die Region verfügbar, Ihre Anwendung auch nicht verfügbar. Dies kann gemäß SLA für die Anwendung nicht akzeptabel sein. In diesem Fall erwägen Sie, die Anwendung und die Dienste über mehrere Bereiche. Eine Bereitstellung mit mehreren können eine aktive Muster (Anfragen auf mehrere aktive Instanzen verteilt) oder ein Aktiv / Passiv (wobei "warme" Instanz Reserve bei Ausfall die primäre Instanz). Wir empfehlen, mehrere Instanzen der Anwendung Dienste über regionale-Paare bereitstellen. Weitere Informationen finden Sie unter [Business Continuity und Disaster Recovery (BCDR): Azure kombiniert Bereiche](../best-practices-availability-paired-regions.md).

- **Implementieren Sie Resiliency Steuerelementmuster für Remotevorgänge gegebenenfalls.** Kommunikation zwischen Remotedienste die Anwendung abhängt, schlägt Kommunikationspfad unweigerlich fehl. Sind mehrere Fehler, konnte die verbleibenden fehlerfreien Instanzen der Anwendung Dienste Anfragen überlastet. Sind mehrere Muster bei häufig auftretenden Fehler, einschließlich der Timeout-Muster [Wiederholen Muster][retry-pattern], [Schutzschalter] [ circuit-breaker] Muster und andere. Weitere Informationen finden Sie unter [entwerfen sowie Anträge Azure](guidance-resiliency-overview.md#implementing-resiliency-strategies). 

- **Verwenden Sie automatische Skalierung reagieren zu laden.** Wenn die Anwendung nicht automatisch als Last Skalierung konfiguriert ist, kann die Anwendung Dienste fehl, wenn sie mit Benutzeranfragen gesättigt. Weitere Informationen finden Sie unter:

    - Allgemein: [Skalierbarkeit Prüfliste](../best-practices-scalability-checklist.md) 
    - Azure App Service: [Instanzenzahl manuell oder automatisch skalieren][app-service-autoscale]
    - Cloud-Services: [wie automatische Skalierung einen Cloud-Dienst][cloud-service-autoscale]
    - Virtuelle Maschinen: [Automatische Skalierung und Virtual Machine Maßstab legt fest][vmss-autoscale]


- **Implementieren Sie asynchrone Vorgänge nach Möglichkeit.** Synchrone Vorgänge können Ressourcen des und andere Vorgänge blockieren, während der Aufrufer wartet, bis der Vorgang abgeschlossen. Jeder Teil der Anwendung asynchrone Vorgänge möglichst zu entwerfen. Weitere Informationen zum Implementieren von asynchronen Programmierung in C# finden Sie unter [asynchrone Programmierung mit Async und await][asynchronous-c-sharp].

- **Verwenden Sie Azure Traffic Manager für die Anwendung Datenverkehr an andere Regionen.**  [Azure Traffic Manager] [ traffic-manager] führt Lastenausgleich auf DNS und kann Datenverkehr in verschiedenen Regionen basierend auf dem [Netzwerkverkehr routing] [ traffic-manager-routing] -Methode angeben und die Endpunkte der Anwendung. 

- **Konfigurieren Sie und Testen Sie der Gesundheit Prüfpunkte für Lastenausgleich und Traffic Manager.** Sicherstellen Sie, dass Ihre Gesundheit Logik wichtige Teile des Systems überprüft und Gesundheit Prüfpunkte entsprechend reagiert.

    - Die Gesundheit Prüfpunkten [Azure Traffic Manager] [ traffic-manager] und [Azure Lastenausgleich] [ load-balancer] einer bestimmten Funktion. Für Traffic Manager der Integritätstest bestimmt, ob Failover zu einem anderen Bereich. Für einen Lastenausgleich bestimmt, ob die Drehung eine VM entfernen.      

    - Ein Prüfpunkt Traffic Manager prüfen Ihre Gesundheit Endpunkt wichtigen Abhängigkeiten, innerhalb derselben Region bereitgestellt werden und deren Fehler auslösen soll ein Failover auf einen anderen Bereich.  

    - Für einen Lastenausgleich melden Health Endpunkt den Zustand des virtuellen Computers. Enthalten Sie keine andere Ebenen oder externe Dienste. Andernfalls verursacht ein Fehler, der außerhalb der VM Lastenausgleich Drehung die VM entfernen.

    - Health monitoring in Ihre Anwendung implementieren Siehe [Health Endpunkt Überwachung Muster](https://msdn.microsoft.com/library/dn589789.aspx).

- **Drittanbieter-Dienste zu überwachen.** Wenn Ihre Anwendung Drittanbieterdienste abhängig ist, identifizieren, wo wie diese Dienste von Drittanbietern können fehlschlagen, und welche Fehler Wirkung auf die Anwendung haben. Ein Drittanbieter darf keinen Überwachung und Diagnose, so es ist wichtig, die Aufrufe der Anmeldung und des Anwendungszustands entsprechen und Diagnoseprotokoll einen eindeutigen Bezeichner. Weitere Informationen zu optimalen Verfahren für die Überwachung und Diagnose finden Sie [Hinweise zur Überwachung und Diagnose] [ monitoring-and-diagnostics-guidance] Dokument.

- **Sicherstellen Sie, dass alle Drittanbieter, die Sie nutzen eine SLA.** Wenn Ihre Anwendung einen Drittanbieterdienst hängt jedoch Drittanbieter keine Verfügbarkeit in Form einer SLA garantiert kann auch die Verfügbarkeit der Anwendung garantiert werden. Die SLA ist nur so gut wie die kleinsten verfügbaren Komponente der Anwendung.


## <a name="data-management"></a>Datenmanagement

- **Verstehen der Replikationsmethoden für die Anwendung von Datenquellen.** Ihre Anwendungsdaten in verschiedenen Datenquellen gespeichert und anderen Anforderungen. Bewerten Sie die Replikationsmethoden für jede Art von Datenspeicher in Azure, [Speicherreplikation Azure](../storage/storage-redundancy.md) und [SQL Datenbank aktive Geo-Replikation](../sql-database/sql-database-geo-replication-overview.md) , um sicherzustellen, dass die Anwendung Daten sind.

- **Sicherstellen Sie, dass kein einzelnes Benutzerkonto auf Produktion und Backup-Daten zugreifen.** Daten-Backups gefährdet sind, ein einzelnes Benutzerkonto hat Schreibberechtigung für Produktion und backup-Quellen. Ein böswilliger Benutzer könnte absichtlich alle Ihre Daten löschen, während normaler Benutzer versehentlich löschen kann. Entwerfen Sie Ihre Anwendung Berechtigungen für jedes Benutzerkonto einschränken, damit nur die Benutzer, die Schreibzugriff erfordern Schreibzugriff und nur für Produktion oder Backup, jedoch nicht beide ist.

- **Dokument die Datenquelle ein Failover und Failback verarbeiten und testen.** Bei die Datenquelle katastrophal, die aus müssen eine menschliche Vermittlungsstelle folgen dokumentierten Hinweise auf neue Datenquelle. Haben die dokumentierten Schritte Fehler wird ein Operator nicht erfolgreich einhalten und ein Failover der Ressource. Testen Sie die Anleitung, um sicherzustellen, dass ein Operator folgen sie erfolgreich ein Failover und Failback der Datenquelle regelmäßig.

- **Überprüfen Sie Ihre Daten-Backups.** Prüfen Sie regelmäßig, dass Ihre backup-Daten ist ein Skript überprüft die Datenintegrität, Schema und Abfragen. Es ist kein Backup ist es nicht für Datenquellen wiederherstellen. Protokollieren Sie und melden Sie alle Inkonsistenzen so der Sicherungsdienst repariert werden kann.

- **Erwägen Sie einen Geo-redundant Storage-Kontotyp.** Daten in Azure Storage-Konto werden immer lokal repliziert. Allerdings werden mehrere Replikationsstrategien aus, wenn ein Speicherkonto bereitgestellt wird. Wählen Sie [Azure Lesezugriff Geo redundante Speicherung (RA-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) schützt Ihre Anwendung für den seltenen Fall wird eine Region nicht verfügbar.

    > [AZURE.NOTE] Für virtuelle Computer nicht auf RA-GRS Replikation VM Datenträger (VHD-Dateien) wiederhergestellt. Verwenden Sie stattdessen [Azure Backup][azure-backup].   

## <a name="operations"></a>Vorgänge

- **Überwachung und Alarmierung best Practices für Ihre Anwendung implementieren.** Ohne ordnungsgemäße Überwachung, Diagnose und warnen ist keine Möglichkeit, Fehler in der Anwendung erkennen und benachrichtigt einen Operator, um sie zu beheben. Weitere Informationen zu optimalen Verfahren finden Sie unter [Hinweise zur Überwachung und Diagnose] [ monitoring-and-diagnostics-guidance] Dokument.

- **Messen Sie Remoteaufruf Statistiken und machen Sie Informationen für das Anwendungsteam verfügbar.**  Nicht verfolgen Remoteaufruf Statistiken in Echtzeit meldet und eine einfache Möglichkeit, diese Informationen bereitzustellen, müssen das Betriebsteam eine sofortige Ansicht in den Zustand der Anwendung nicht. Und wenn Sie nur durchschnittliche Remoteaufruf Zeit gemessen, Sie nicht genügend Informationen zum Anzeigen gibt der Services. Remoteaufruf Metriken wie Latenz, Durchsatz und Fehler in Perzentile 99 und 95 zusammenfassen. Führen Sie statistische Kennzahlen aufzudecken jedes Quantil innerhalb Fehler.

- **Nachverfolgen von vorübergehenden Ausnahmen und Versuche in einem angemessenen Zeitrahmen.** Verfolgen und überwachen vorübergehende Ausnahmen und Wiederholungsversuche mit der Zeit, kann ein Problem oder Fehler durch die Anwendung Wiederholungslogik verborgen sind. D. h. die Überwachung und Protokollierung nur zeigt Erfolg oder Fehlschlag einer Operation, die Tatsache, dass die Operation aufgrund Ausnahmen mehrmals wiederholt werden ausgeblendet. Ein Trend zunehmender Ausnahmen Zeitverlauf gibt an, dass der Dienst ein Problem fehlschlagen. Weitere Informationen finden Sie unter [Service-spezifische wiederholen][retry-service-guidance].

- **Implementieren Sie ein Frühwarnsystem, das einen Operator aufmerksam macht.** Identifizieren der Leistungskennzahlen Indikatoren des Anwendungszustands, vorübergehende Ausnahmen und remote aufrufen Latenz und jeweils geeignete Schwellenwerte festgelegt. Senden Sie eine Benachrichtigung an Vorgänge, wenn der Schwellenwert erreicht ist. Diese Grenzwerte auf Ebenen, die Probleme identifizieren, bevor sie kritisch und Recovery Antwort erforderlich.

- **Dokumentieren Sie den Freigabeprozess für Ihre Anwendung.** Ohne ausführliche Version Prozessdokumentation möglicherweise ein Operator fehlerhafte Aktualisierung bereitstellen oder für die Anwendung nicht ordnungsgemäß konfigurieren. Klar definieren Dokumentieren der Freigabe und dem gesamten Betriebsteam verfügbar ist. Best Practices für die Bereitstellung Ihrer Anwendung sowie in der [Bereitstellung sowie] detaillierte[ guidance-resilient-deployment] Abschnitt der Stabilität Leitlinien.

- **Stellen Sie sicher, dass mehrere Personen im Team, die Anwendung überwachen und Ausführen von manuellen Wiederherstellungsschritte geschult ist.** Wenn Sie nur einen Operator im Team haben, kann die Anwendung Schritte beginnen, wird dieser Person eine einzelne Fehlerquelle. Erkennung und Wiederherstellung mehrerer Personen schulen und sicher immer mindestens aktiv.

- **Automatisieren der Anwendung Bereitstellung.** Wenn Ihre Mitarbeiter die Anwendung manuell bereitstellen, kann menschlicher Fehler die Bereitstellung fehl. Weitere Informationen zu optimalen Verfahren für die Automatisierung der Bereitstellung finden Sie unter [sowie Bereitstellung] [ guidance-resilient-deployment] Abschnitt der Stabilität Leitlinien.

- **Entwerfen der Veröffentlichungsprozess Verfügbarkeit maximieren.** Erfordert Ihre Freigabe Services während der Bereitstellung offline gehen, ist die Anwendung erst verfügbar, sie wieder online geschaltet. Verwenden Sie [Blaugrün](http://martinfowler.com/bliki/BlueGreenDeployment.html) oder [Kanarischen release](http://martinfowler.com/bliki/CanaryRelease.html) Bereitstellung Verfahren zum Bereitstellen der Anwendung für die Produktion. Beide Verfahren umfassen Versionscode neben Produktionscode bereitstellen, damit Benutzer Freigabecode Produktionscode im Falle eines Fehlers umgeleitet werden können. Weitere Informationen finden Sie unter [sowie Bereitstellung] [ guidance-resilient-deployment] Abschnitt der Stabilität Leitlinien.

- **Protokollierung und Überwachung des Deployments Ihrer Anwendung.** Verwenden Sie stufenweise Bereitstellungstechniken wie Blaugrün oder Kanarischen Versionen mehr als eine Version der Anwendung in der Produktion ausgeführt werden. Sollte ein Problem auftreten, ist es wichtig festzustellen, welche Version der Anwendung ein Problem verursacht. Eine robuste Protokollierung Strategie möglichst viele versionsspezifische Informationen erfassen. 

- **Stellen Sie sicher, dass Ihre Anwendung nicht gegen [Azure-Abonnement beschränkt](../azure-subscription-service-limits.md).** Bestimmte Ressource Anzahl von Ressourcengruppen, die Anzahl der Kerne und Anzahl Speicherkonten ist Azure-Abonnements eingeschränkt.  Wenn Ihre Anforderungen Azure-Abonnement überschreiten, Erstellen einer anderen Azure-Abonnement und Bereitstellung ausreichende Ressourcen vorhanden.

- **Stellen Sie sicher, dass Ihre Anwendung nicht gegen [pro Dienst beschränkt](../azure-subscription-service-limits.md).** Einzelne Azure Services ist Verbrauch eingeschränkt &mdash; z. B. Speicher, Durchsatz, Anzahl der Verbindungen, Anfragen pro Sekunde und andere beschränkt. Die Anwendung schlägt fehl, wenn versucht wird, diese Grenzen Ressourcen verwenden. Drosselung und möglichen Ausfallzeiten für Benutzer dadurch. 

    Je nach Dienst und Ihre Anwendung häufig zu vermeiden, diese Grenzwerte (z. B. durch Auswählen einer anderen Tarif) skalieren oder Skalierung (Hinzufügen neue Instanzen).  

- **Entwerfen der Anwendung Speicherbedarf zu Azure Skalierbarkeits- und Ziele.** Azure-Speicher soll innerhalb vordefinierter Skalierbarkeits- und Performance-Ziele, die Anwendung innerhalb dieser Ziele nutzen so entwerfen. Überschreiten Sie diesen Zielen wird die Anwendung Speicher Drosselung. Um dieses Problem zu beheben, Bereitstellen Sie zusätzlicher Speicherkonten. Wenn Speicherkonto Limit gestoßen bereitstellen Sie zusätzlicher Azure-Abonnements; bereitstellen Sie zusätzlicher Speicherkonten vorhanden Weitere Informationen finden Sie unter [Azure Storage Skalierbarkeits- und Performance-Ziele](../storage/storage-scalability-targets.md).

- **Wählen Sie die richtige VM-Größe für die Anwendung.** Messen Sie tatsächliche CPU, Speicher, Festplatten und e/a-Ihrer virtuellen Computer in der Produktion zu und überprüfen Sie, ob die VM gewählte ausreichend ist. Andernfalls kann die Anwendung Kapazität Probleme nähern VMs ihre Grenzen. VM-Größen werden im Dokument [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md) ausführlich beschrieben.

- **Ob Ihre Anwendung stabil oder schwankende langfristig ist.** Wenn Ihre Arbeitslast mit der Zeit schwankt wird mit Azure VM Skalierung automatisch Skalierung die Anzahl der VM-Instanzen. Andernfalls müssen Sie manuell erhöhen oder Verringern der Anzahl der VMs. Weitere Informationen finden Sie unter [Übersicht über Virtual Machine Skalierung legt](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

- **Wählen Sie die richtige Service-Ebene für Azure SQL-Datenbank.** Ihre Anwendung Azure SQL-Datenbank sicher, dass die entsprechende Dienstebene ausgewählt haben. Wenn Sie eine Ebene, die nicht der Anwendung Datenbank Transaktion Einheit (DTU) decken kann auswählen, werden Ihre Daten verwenden gedrosselt. Weitere Informationen zur Auswahl des richtigen Service-Plans finden Sie unter der [SQL-Datenbank und Leistung: verstehen, was jeder Dienstebene für](../sql-database/sql-database-service-tiers.md) Dokument. 

- **Haben Sie einen Fallbackplan für die Bereitstellung.** Es ist möglich, dass die anwendungsbereitstellung konnte und Ihre Anwendung nicht mehr verfügbar. Entwerfen Sie einen Vorgang an eine funktionierende Version zurückgehen und Ausfallzeiten. Siehe [sowie Bereitstellung] [ guidance-resilient-deployment] Abschnitt der Stabilität Leitlinien für Weitere Informationen. 

- **Erstellen eines Prozesses für die Interaktion mit Azure-Support.** Wenn der Prozess für die Kontaktaufnahme mit [Azure unterstützt](https://azure.microsoft.com/support/plans/) nicht festgelegt ist, bevor der Bedarf an den Support wenden verlängert Ausfallzeiten wie Supportprozess zum ersten Mal navigiert wird. Schließen Sie den Prozess für die Kontaktaufnahme mit dem Support und Eskalation von Problemen als Teil der Anwendung Stabilität von Anfang an.

- **Stellen Sie sicher, dass die Anwendung nicht mehr als die maximale Anzahl von Speicherkonten pro Abonnement verwendet.** Azure ermöglicht maximal 200 Speicherkonten pro Abonnement. Wenn die Anwendung mehr Speicherkonten als derzeit in Ihrem Abonnement erfordert, müssen Sie ein neues Abonnement erstellen und weitere Speicherkonten vorhanden. Weitere Informationen finden Sie unter [Azure-Abonnement und Service Grenzen, Kontingente und Einschränkungen](../azure-subscription-service-limits.md#storage-limits).

- **Sicherstellen Sie, dass Ihre Anwendung Skalierbarkeitsziele für virtuellen Datenträger überschreitet.** Eine Neuerung Azure unterstützt eine Anzahl Datenträger abhängig von mehreren Faktoren, einschließlich virtueller Speicher und Speicherkonto anfügen. Überschreitet die Anwendung Skalierbarkeitsziele virtuellen Datenträger, Bereitstellen Sie zusätzlicher Speicherkonten und erstellen Sie die virtuellen Laufwerke vorhanden. Weitere Informationen finden Sie unter [Azure Storage Skalierbarkeits- und Performance-Ziele] (... / storage/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks)

## <a name="test"></a>Test

- **Führen Sie Failover und Failback für Ihre Anwendung aus.** Wenn Sie Failover und Failback vollständig getestet noch nicht, können Sie bestimmte, dass abhängige Dienste in der Anwendung wieder auf synchronisierte während der Wiederherstellung kommen. Sicherstellen Sie, dass die Anwendung abhängige Failover und nicht in der richtigen Reihenfolge services.

- **Führen Sie Fehlerinjektion für Ihre Anwendung aus.** Die Anwendung kann verschiedene Ursachen haben, wie Gültigkeitsdauer, Erschöpfung der Systemressourcen in einer VM oder Speicherfehler fehl. Testen Sie die Anwendung in einer Umgebung wie möglich zur Produktion, simulieren oder echte Fehler auslösen. Beispielsweise löschen Sie Zertifikate, künstlich Systemressourcen Sie, oder Speicherquelle. Überprüfen Sie die Fähigkeit Ihrer Anwendung zur Wiederherstellung aller Arten von Fehlern, allein und zusammen. Überprüfen Sie Fehler nicht weitergegeben oder cascading durch Ihr System.

- **Tests im synthetischen und realen Benutzerdaten mit.** Test- und selten identisch sind, ist wichtig, Blaugrün oder Kanarischen Bereitstellung und Testen Sie die Anwendung in der Produktion. Dadurch zu testen Sie Ihre Anwendung in der Produktion tatsächlichen Belastung bei umfassend funktionieren.

## <a name="security"></a>Sicherheit

- **Schutz auf Anwendungsebene verteilte Dienstverweigerungsangriffe (DDoS) implementieren.** Azure Services sind gegen DDos-Angriffe auf Netzwerkebene geschützt. Da es schwierig ist, true Benutzeranfragen von böswilligen Benutzer unterscheiden kann nicht, Azure Angriffe Anwendungsebene schützen. Weitere Informationen zum Schutz der Anwendungsebene DDoS-Angriffe finden Sie im Abschnitt "Schutz gegen DDoS" [Microsoft Azure Network Security](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf) (PDF-Download).

- **Das Prinzip der geringsten Rechte für den Zugriff auf die Ressourcen der Anwendung zu implementieren.** Der Standardwert für den Zugriff auf die Ressourcen der Anwendung sollte so restriktiv wie möglich sein. Erteilen Sie höhere Berechtigungen zu Genehmigung. Übermäßig Zugriff auf die Ressourcen der Anwendung in der Standardeinstellung kann ein Benutzer versehentlich oder absichtlich Löschen von Ressourcen führen. Azure bietet [Rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-built-in-roles.md) Benutzerberechtigungen verwalten, aber unbedingt Berechtigung Berechtigungen für andere Ressourcen überprüfen, ihre eigenen Berechtigungen Systeme wie SQL Server haben. 

## <a name="telemetry"></a>Telemetrie

- **Protokollieren Sie Daten die Anwendung in dieser Umgebung ausgeführt wird.** Erfassen Sie robuste Telemetriedaten, die Anwendung in dieser Umgebung ausgeführt, oder Sie haben nicht ausreichende Informationen, um die Ursache von Problemen zu diagnostizieren, während es aktiv Benutzer dient. Weitere Informationen finden Sie die Protokollierung in best Practices ist für die [Überwachung und Diagnose Anleitung] [ monitoring-and-diagnostics-guidance] Dokument.

- **Anmelden über ein asynchrones Muster zu implementieren.** Wenn Protokollierung Vorgänge synchron sind, blockiert sie Anwendungscode. Sicherstellen Sie, dass die Protokollierung Operationen als asynchrone Operationen implementiert werden. 

- **Korrelieren von Daten über Dienstgrenzen hinweg.** In einer normalen n-Tier-Anwendung kann ein mehrere Dienstgrenzen durchlaufen. Beispielsweise ein stammt normalerweise in der Webebene Geschäftsebene übergeben und schließlich auf der Datenebene beibehalten. In komplexen Szenarien kann ein zu viele verschiedene Dienste und Daten verteilt werden. Sicherstellen Sie, dass Ihre Protokollierungssystem Aufrufe über Dienstgrenzen hinweg korreliert, so die Anforderung in der Anwendung verfolgen können.

##  <a name="azure-resources"></a>Azure-Ressourcen 

- **Verwenden Sie Azure-Ressourcen-Manager auf Ressourcen.** Ressourcen-Manager Vorlagen erleichtern die Bereitstellung über PowerShell oder CLI Azure zuverlässiger Bereitstellungsprozess führt zu automatisieren. Weitere Informationen finden Sie unter [Übersicht über Azure Ressource-Manager][resource-manager].

- **Geben Sie Ressourcen aussagekräftige Namen.** Die Ressourcen sinnvolle Namen erleichtert das Suchen einer bestimmten Ressource und seine Rolle. Weitere Informationen finden Sie unter [Empfohlene Namenskonventionen für Azure-Ressourcen](guidance-naming-conventions.md) 

- **Mit rollenbasierte Zugriffskontrolle (RBAC)**. Verwenden Sie Rollenbasierten Zugriff auf Azure Ressourcen steuern, die Sie bereitstellen. RBAC können Sie Autorisierungsrollen Mitglieder Ihres Teams DevOps gegen versehentliche Löschung oder Änderung bereitgestellten Ressourcen zuweisen. Weitere Informationen finden Sie unter [Erste Schritte mit Access Management im Azure-portal](../active-directory/role-based-access-control-what-is.md) 

- **Verwenden Sie für wichtige Ressourcen wie VMs Ressourcensperren.** Ressourcensperren verhindern, dass einen Operator versehentlich eine Ressource. Weitere Informationen finden Sie unter [Sperrenressourcen Azure Ressourcenmanager](../resource-group-lock-resources.md) 

- **Regionale Paare.** Regionale dasselbe Computerpaar beim Deployment auf zwei Bereiche, wählen Sie Regionen aus. Ausfall eines umfassenden Wiederherstellung von Region aus jeder erhält. Einige Dienste wie Geo-redundanten Speicher bieten automatische Replikation gepaarten Region. Weitere Informationen finden Sie unter [Business Continuity und Disaster Recovery (BCDR): Azure kombiniert Bereiche](../best-practices-availability-paired-regions.md) 

- **Organisieren von Ressourcengruppen Funktion und Lebenszyklus**.  Im Allgemeinen sollte eine Ressourcengruppe Ressourcen, die den gleichen Lebenszyklus verwenden. Dies erleichtert die Bereitstellung verwalten, Test Bereitstellung löschen und Zuweisen von Zugriffsrechten, reduziert die Wahrscheinlichkeit, dass ein Produktionsbetrieb versehentlich gelöscht oder geändert wird. Erstellen Sie separate Ressourcengruppen für Produktion, Entwicklung und Testen Sie Umgebung. Legen Sie in einer Bereitstellung mit mehreren Ressourcen für jede Region in getrennte Ressourcengruppen. Dies erleichtert die Region erneut ohne die Region. 

## <a name="azure-services"></a>Azure-Dienste 

Folgende Prüfliste gelten für bestimmte Dienste in Azure.

###  <a name="app-service"></a>App Service 

- **Verwenden Sie Standard oder Premium-Ebene.** Diese Ebenen staging-Steckplätze unterstützen und automatisierte Backups. Weitere Informationen finden Sie in [Azure App Service-Pläne ausführliche Übersicht](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) 

- **Skalierung nach oben oder unten zu vermeiden.** Wählen Sie stattdessen ein Tier und Größe, die die Leistung normalerweise laden und [Skalierung](../app-service-web/web-sites-scale.md) Anforderungen Instanzen Änderungen Verkehrsaufkommen. Skalierung kann nach oben und unten einen Neustart der Anwendung ausgelöst.  

- **Speicherkonfiguration als app-Einstellungen.** App-Einstellungen Einstellungen wie app zu verwenden. Einstellungen Sie im Ressourcenmanager Vorlagen oder mit PowerShell anzuwenden als Teil einer automatisierten Bereitstellung / Aktualisierungsvorgang zuverlässiger ist. Weitere Informationen finden Sie unter [Configure webapps in Azure App Service](../app-service-web/web-sites-configure.md). 

- **Erstellen Sie separate App Pläne für Produktion und Test.** Verwenden Sie keine Slots für die Bereitstellung in der Produktion zum Testen.  Alle apps im gleichen App Service-Plan verwenden dieselben VM-Instanzen. Wenn Sie Produktion und Test Bereitstellung in demselben Plan können sie die Bereitstellung in der Produktion beeinträchtigen. Auslastungstests können z. B. die Produktion Website beeinträchtigen. Mit Test-Installationen in einen eigenen Plan, isolieren sie aus der Produktionsflussversion.  

- **Separate webapps von Web-APIs**. Sollten Sie die Projektmappe ein Web-Front-End- und Web-API ist in separate App Service apps zerlegen. Dieser Entwurf erleichtert die Lösung nach Arbeitslast zu zerlegen. Können ausführen Web app und die API in separate App Service-Pläne, damit sie unabhängig voneinander skaliert werden können. Wenn dieser Ebene Skalierbarkeit benötigen, können Sie Planes apps bereitstellen und verschieben sie in separaten Plänen später bei Bedarf.

- **Verwenden Sie das Sicherungsfeature App Service SQL Azure-Datenbanken sichern.** Verwenden Sie [SQL Datenbank automatisierte Backups][sql-backup]. App-Dienstes eine Sicherungskopie exportiert die Datenbank in eine SQL-.bacpac-Datei DTUs kostet.  

- **Bereitstellen Sie in staging-Steckplatz.** Erstellen Sie einen Bereitstellung Steckplatz Staging. Bereitstellen von Anwendungsupdates staging-Steckplatz und Überprüfen der Bereitstellung vor Austausch in die Produktion. Dies verringert die Wahrscheinlichkeit eine fehlerhafte Aktualisierung in der Produktion. Es wird sichergestellt, dass alle Instanzen vor in Betrieb ausgetauscht erwärmt werden. Viele Programme besitzen eine erhebliche Aufwärmdauer und -Kaltstartzeit. Weitere Informationen finden Sie unter [Stagingumgebungen für Web-apps in Azure App Service einrichten](../app-service-web/web-sites-staged-publishing.md). 

-  **Erstellen Sie einen Bereitstellung Steckplatz zweifelsfrei funktionierenden (LKG) Bereitstellung enthält.** Wenn Sie ein Update für die Produktion bereitstellen, Wechseln der vorherigen Bereitstellung in der Produktion in Steckplatz LKG Dies erleichtert das Rollback einer ungültigen Bereitstellung. Wenn Sie später ein Problem erkennen, können Sie schnell die LKG Version wiederherstellen. Weitere Informationen finden Sie unter [Azure Referenzarchitektur: einfache Anwendung](guidance-web-apps-basic.md). 

- **Das Diagnoseprotokoll aktivieren**, einschließlich der Anwendung protokollieren und Web-Server anmelden. Protokollierung ist wichtig für die Überwachung und Diagnose. Finden Sie [das Diagnoseprotokoll für Web-apps in Azure App Service aktivieren](../app-service-web/web-sites-enable-diagnostic-log.md)

- **Protokoll BLOB-Speicher**. Dies erleichtert das Erfassen und Analysieren der Daten. 

- **Erstellen Sie ein separates Speicherkonto für Protokolle.** Verwenden Sie nicht das gleiche Speicherkonto für Protokolle und Daten. Dies verhindert Protokollierung Leistung der Anwendung verringern. 

- **Überwachen der Leistung.** Verwenden Sie Systemmonitor-Service [Application Insights](../application-insights/app-insights-overview.md) oder [Neue Relikt](http://newrelic.com/) Monitor Anwendungsleistung und Belastung.  Performance-Überwachung können Sie in Echtzeit Einblick in die Anwendung. Sie können Probleme diagnostizieren und Ursachenanalyse von Fehlern. 


###  <a name="application-gateway"></a>Application Gateway 

- **Bereitstellung von mindestens zwei Instanzen.** Bereitstellen Sie Application Gateway mit mindestens zwei Instanzen. Eine einzelne Instanz ist eine einzelne Fehlerquelle. Verwenden Sie zwei oder mehr Instanzen für Redundanz und skalierbar. Um die [SLA](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_0/)zu qualifizieren, müssen Sie mindestens zwei mittlere oder größere Instanzen bereitstellen. 

### <a name="azure-search"></a>Azure Suche

- **Bereitstellung von mehreren Replikaten.** Verwenden Sie mindestens zwei Replikate lesen Sie hohe Verfügbarkeit oder drei Schreib-/ hohe Verfügbarkeit.

- **Konfigurieren Sie Indexer für Installationen mit mehreren.** Haben Sie eine Bereitstellung mit mehreren sollten Sie die Optionen für die Kontinuität indizieren.

    - Zeigen Sie die Datenquelle Geo repliziert, Indexer der einzelnen regionalen Azure-Suchdienst auf seiner lokalen Daten Quellreplikat.  

    - Ist die Datenquelle nicht geografisch repliziert, zeigen Sie mehrere Indexer an dieselbe Datenquelle, Azure Suchdienste in mehreren Regionen kontinuierlich und unabhängig von der Datenquelle indizieren. Weitere Informationen finden Sie unter [Azure Leistung und Optimierung Suchverfahren][search-optimization].

###  <a name="azure-storage"></a>Azure-Speicher 

- **Verwenden Sie für Anwendungsdaten Lesezugriff Geo redundanten Speicher (RA-GRS).** RA-GRS Speicher repliziert die Daten an einen zweiten Bereich und bietet schreibgeschützten Zugriff aus dem sekundären. Gibt Speicher Ausfall der primären, kann die Anwendung die Daten aus dem sekundären lesen. Weitere Informationen finden Sie unter [Azure Storage Replication](../storage/storage-redundancy.md).

- **Für VM-Datenträger verwenden Premium-Speicher** Weitere Informationen finden Sie unter [Storage Premium: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](../storage/storage-premium-storage.md).

- **Queue Storage backup Warteschlange zu erstellen, in einer anderen Region.** Für Queue Storage ist ein schreibgeschütztes Replikat verwenden, beschränkt, weil nicht in die Warteschlange oder Elemente entfernen. Erstellen Sie stattdessen eine Sicherung Warteschlange in ein Speicherkonto in einer anderen Region. Ist ein Ausfall Speicher, Anwendung die Sicherung Warteschlange bis die primäre Region wieder verfügbar wird. Auf diese Weise kann die Anwendung weiterhin neue Anfragen bearbeitet werden.  

###  <a name="documentdb"></a>DocumentDB 

- **Replizieren der Datenbank Regionen.** Mit einem Konto mit mehreren hat die DocumentDB-Datenbank ein Schreibvorgang Region und mehrere schreibgeschützte Bereiche. Liegt ein Fehler im Bereich schreiben, können Sie von einem anderen Replikat lesen. Das Client-SDK behandelt dies automatisch. Sie können auch Schreiben Region als einer anderen Region nicht. Weitere Informationen finden Sie unter [verteilen Daten mit DocumentDB](../documentdb/documentdb-distribute-data-globally.md).


###  <a name="sql-database"></a>SQL-Datenbank 

- **Verwenden Sie Standard oder Premium-Ebene.** Diese Ebenen bieten mehr Point-in-Time-Wiederherstellung Periode (35 Tage). Weitere Informationen finden Sie in der [SQL-Datenbank und Leistung](../sql-database/sql-database-service-tiers.md).

- **SQL-Datenbank zu überwachen.** Die Überwachung kann Angriffen oder menschliche Fehler diagnostizieren verwendet werden. Weitere Informationen finden Sie unter [Erste Schritte mit SQL Datenbank überwachen](../sql-database/sql-database-auditing-get-started.md). 

- **Verwendet aktive Geo-Replikation** Verwenden Sie aktive Geo-Replikation zum Erstellen einer lesbaren sekundäres in einen anderen Bereich.  Wenn die primäre Datenbank fehlschlägt oder einfach offline geschaltet werden, führen Sie ein manuelles Failover auf die sekundäre Datenbank.  Bis Sie Failover ist die sekundäre Datenbank schreibgeschützt.  Weitere Informationen finden Sie unter [SQL Datenbank aktive Geo-Replikation](../sql-database/sql-database-geo-replication-overview.md). 

- **Sharding verwenden**. Verwenden Sie Sharding die Datenbank horizontal zu partitionieren. Sharding bieten Fehlerisolation. Weitere Informationen finden Sie unter [Dezentrales Skalieren mit Azure SQL-Datenbank](../sql-database/sql-database-elastic-scale-introduction.md). 

- **Verwenden Sie Point-in-Time-Wiederherstellung zur Wiederherstellung von Benutzerfehlern.**  Point-in-Time wiederhergestellt, die Datenbank zu einem früheren Zeitpunkt. Weitere Informationen finden Sie unter [Wiederherstellen eine Azure SQL Datenbank automatisierter][sql-restore].

- **Verwenden Sie Geo-Wiederherstellung zur Wiederherstellung von einem Dienstausfall.** Geo-Wiederherstellung wiederhergestellt eine Datenbank von Geo-redundantes Backup.  Weitere Informationen finden Sie unter [Wiederherstellen eine Azure SQL Datenbank automatisierter][sql-restore].


###  <a name="sql-server-running-in-a-vm"></a>SQL Server (in einer VM ausgeführt)

- **Replizieren Sie die Datenbank.** Verwenden Sie SQL Server immer auf Verfügbarkeitsgruppen, um die Datenbank zu replizieren. Bietet hohe Verfügbarkeit fällt eine SQL Server-Instanz. Weitere Informationen finden Sie unter [Weitere Informationen...](guidance-compute-n-tier-vm.md) 

- **Sichern Sie die Datenbank**. Wenn Sie bereits [Azure Backup](https://azure.microsoft.com/documentation/services/backup/) Sichern Ihrer virtuellen Computer verwenden, sollten Sie [Azure Backup für SQL Server-Arbeitslasten mit DPM](../backup/backup-azure-backup-sql.md). Bei diesem Ansatz ist eine backup-Administrator-Rolle für die Organisation und einheitliche Wiederherstellungsverfahren für VMs und SQL Server. Andernfalls Sicherungsprogramm [SQL Server verwaltet Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). 


###  <a name="traffic-manager"></a>Traffic Manager 

- **Durchführen Sie manuelle Failback.** Nach einem Failover Traffic Manager manuelle Failback durchführen, sondern automatisch Failback. Überprüfen Sie bevor wieder fehlschlägt, ob alle anwendungssubsysteme fehlerfrei sind.  Andernfalls können Sie eine Situation erstellen, wo die Anwendung hin und her zwischen Rechenzentren spiegelt. Weitere Informationen finden Sie unter [VMs in mehreren Regionen ausgeführt](guidance-compute-multiple-datacenters.md).

- **Erstellen einen Zustand Prüfpunkt Endpunkt**. Erstellen Sie ein benutzerdefiniertes Endpunktverhalten über den Gesamtzustand der Anwendung Berichte. Dies ermöglicht Traffic Manager schlägt alle kritischen, nicht nur den front-End Failover. Der Endpunkt sollte HTTP-Fehlercode kritische Abhängigkeit ist fehlerhaft oder nicht erreichbar. Melden Sie Fehler für nicht kritische Dienste jedoch nicht. Andernfalls kann der Integritätstest Failover ausgelöst, bedarfsorientierten nicht fälschlich erstellen. Weitere Informationen finden Sie unter [Traffic Manager Endpunkt monitoring und Failover](../traffic-manager/traffic-manager-monitoring.md).


###  <a name="virtual-machines"></a>Virtuelle Computer 

- **Vermeiden Sie eine Arbeitslast auf einer einzelnen VM.** Eine einzelne VM-Bereitstellung ist nicht anfällig für geplante oder ungeplante Wartung. Setzen Sie stattdessen mehrere VMs eine Verfügbarkeit oder VM-Skalierungsgruppe mit Lastenausgleichsfunktion vor. Um die SLA zu qualifizieren, müssen mindestens zwei virtuellen Maschinen innerhalb Verfügbarkeit bereitgestellt werden. Weitere Informationen finden Sie in [Virtual Machine Maßstab legt Overview](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). 

- **Wenn Sie den virtuellen Computer bereitstellen Verfügbarkeit angeben.** Derzeit besteht keine Möglichkeit, eine Verfügbarkeit festgelegt, nachdem die VM bereitgestellt Ressourcenmanager VM hinzufügen. Beim Hinzufügen eine neue VM eine Verfügbarkeit festlegen müssen Sie eine Netzwerkkarte für den virtuellen Computer erstellen und Back-End-Adresspool auf dem Lastenausgleich die Netzwerkkarte hinzufügen. Andernfalls wird nicht Lastenausgleich Netzwerkdatenverkehr an diesen virtuellen Computer weiterzuleiten. 

- **Legen Sie jede Anwendungsebene in separate Verfügbarkeit festgelegt.** In einer N-Tier-Anwendung setzen VMs aus unterschiedlichen Ebenen in dieselben Verfügbarkeit. VMs in einem Verfügbarkeit werden über Fehlerdomänen (FDs) und Domänen (UD) aktualisieren. Um die Redundanz der FDs und UDs nutzen, jede VM in den Verfügbarkeit derselben Clientanforderungen verarbeiten muss. 

- **Wählen Sie die richtige VM basierend auf Leistung.** Beim Verschieben einer vorhandenen Arbeitslast in Azure beginnen Sie mit VM-Größe, die den lokalen Server am nächsten kommt. Messen Sie die tatsächliche Arbeitslast CPU, Arbeitsspeicher und Festplatte IOPS, und bei Bedarf passen Sie die Größe an. Dadurch sicherstellen, dass die Anwendung in einer Cloud-Umgebung wie erwartet verhält. Benötigen Sie mehrere Netzwerkkarten, auch über NIC Grenze für jede Größe. 

- **Verwenden Sie Premium-Speicherung für virtuelle Festplatten.** Azure Storage Premium bietet hohe Leistung, niedriger Latenz Unterstützung. Weitere Informationen finden Sie unter [Storage Premium: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](../storage/storage-premium-storage.md) VM Größe, die Premium-Speicher unterstützt. 

- **Erstellen einer getrennten Konto für jede VM.** Setzen Sie die VHDs für eine VM in einem separaten Konto. Dadurch vermeiden, dass die IOPS Grenzwerte für Speicherkonten. Weitere Informationen finden Sie unter [Azure Storage Skalierbarkeits- und Performance-Ziele](../storage/storage-scalability-targets.md). Wenn Sie eine große Anzahl von virtuellen Computern bereitstellen, jedoch über den Grenzwert pro Abonnement Speicherkonten. [Speichergrenzwerte](../azure-subscription-service-limits.md#storage-limits)anzeigen

- **Getrennten Konto für Diagnoseprotokolle erstellen**. Schreiben Sie nicht Diagnoseprotokolle in demselben Speicherkonto wie VHDs vermeiden Diagnoseprotokoll beeinflussen die IOPS für die VM-Datenträger. Diagnoseprotokolle genügt ein Standardbenutzerkonto lokal redundanten Speicher (LRS). 

- **Installieren Sie Anträge auf Datenträger nicht die Betriebssystem-Datenträger.** Andernfalls erreichen Sie maximale Größe Datenträger. 

- **Sicherungsprogramm Azure VMs sichern.** Backups schützen vor Datenverlust. Weitere Informationen finden Sie unter [Schützen Azure VMs mit einem Recovery Services](../backup/backup-azure-vms-first-look-arm.md).

- **Aktivieren Sie Diagnoseprotokolle**, einschließlich basic Zustandsmetriken, Infrastrukturprotokolle und [Boot-Diagnose][boot-diagnostics]. Boot-Diagnose können Sie Ihre virtuellen Computer in einer nicht startfähigen Zustand wird ein Startfehler analysieren. Weitere Informationen finden Sie in der [Übersicht von Azure Diagnoseprotokolle][diagnostics-logs].

- **Mit der Erweiterung AzureLogCollector**. (Nur Windows-VMs.) Diese Erweiterung Azure-Plattform Protokolle aggregiert und lädt sie Azure-Speicher ohne Operator VM Remote anmelden. Weitere Informationen finden Sie unter [AzureLogCollector-Erweiterung](../virtual-machines/virtual-machines-windows-log-collector-extension.md).


###  <a name="virtual-network"></a>Virtuelles Netzwerk 

- **Weißen oder Block hinzufügen öffentliche IP-Adressen eine NSG an das Subnetz.** Zugriff durch böswillige Benutzer oder Zugriff nur von Benutzern mit Recht auf die Anwendung zugreifen.  

- **Erstellen einer benutzerdefinierten Integritätstest.** Load Balancer Health Prüfpunkte können HTTP oder TCP testen. Wenn ein virtueller Computer einen HTTP-Server ausgeführt wird, ist HTTP-Prüfpunkt ein besserer Indikator Zustand als TCP-Prüfpunkt. Ein Prüfpunkt HTTP verwenden Sie einen benutzerdefinierten Endpunkt, der den Gesamtzustand der Anwendung, einschließlich aller wichtige Abhängigkeiten meldet. Weitere Informationen finden Sie unter [Azure Lastenausgleich Overview](../load-balancer/load-balancer-overview.md).

- **Nicht blockieren der Integritätstest.** Load Balancer Integritätstest wird von bekannten IP-Adresse 168.63.129.16 gesendet. Blockieren Sie nicht Datenverkehr von dieser IP-Firewall-Richtlinien oder Netzwerksicherheitsregeln Group (NSG). Blockieren der Integritätstest würde Lastenausgleich Drehung die VM entfernen. 

- **Aktivieren Sie Lastenausgleich Protokollierung.** Anzeigen der Protokolle wie viele VMs auf Back-End des Netzwerkverkehrs durch fehlerhaften Prüfpunkt Antworten nicht empfangen. Weitere Informationen finden Sie unter [Protokollanalyse Azure Lastenausgleich](../load-balancer/load-balancer-monitor-log.md).


<!-- links -->
[app-service-autoscale]: ../monitoring-and-diagnostics/insights-how-to-scale.md
[asynchronous-c-sharp]:https://msdn.microsoft.com/library/mt674882.aspx
[availability-sets]:../virtual-machines/virtual-machines-windows-manage-availability.md
[azure-backup]: https://azure.microsoft.com/documentation/services/backup/
[boot-diagnostics]: https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/
[circuit-breaker]: https://msdn.microsoft.com/library/dn589784.aspx
[cloud-service-autoscale]: ../cloud-services/cloud-services-how-to-scale.md
[diagnostics-logs]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[fma]: guidance-resiliency-failure-mode-analysis.md
[guidance-resilient-deployment]: guidance-resiliency-overview.md#resilient-deployment
[load-balancer]: load-balancer/load-balancer-overview.md
[monitoring-and-diagnostics-guidance]: ../best-practices-monitoring.md
[resource-manager]: ../azure-resource-manager/resource-group-overview.md
[retry-pattern]: https://msdn.microsoft.com/library/dn589788.aspx
[retry-service-guidance]: ../best-practices-retry-service-specific.md
[search-optimization]: ../search/search-performance-optimization.md
[sql-backup]: ../sql-database/sql-database-automated-backups.md
[sql-restore]: ../sql-database/sql-database-recovery-using-backups.md
[traffic-manager]: ../traffic-manager/traffic-manager-overview.md
[traffic-manager-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[vmss-autoscale]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md
