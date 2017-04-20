
<properties
   pageTitle="Azure Hinweise | Patterns & Practices | Microsoft Azure"
   description="Best Practices und Leitfaden für Azure"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Azure Anleitung

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Microsoft Patterns & Practices-Team ist Teil der Azure Customer Advisory Team. Unser Ziel soll Entwickler, Architekten und IT-Experten auf der Plattform Microsoft Azure erfolgreich zu sein. Wir entwickeln Leitfäden, die Vorgehensweisen zum Erstellen von Cloudlösungen Azure zeigt.

## <a name="checklists"></a>Prüflisten

Diese Listen sind eine kurze Referenz zum Überprüfen der grundlegenden Aspekte der Verfügbarkeit und Skalierung. 

- [Verfügbarkeit: Checkliste][AvailabilityChecklist] 

    Eine Zusammenfassung der empfohlenen Methoden für die Sicherstellung der Verfügbarkeit.

- [Skalierbar-Checkliste][ScalabilityChecklist]

    Eine Zusammenfassung der empfohlenen Methoden für die Implementierung skalierbarer Dienste und Datenmanagement behandeln.

## <a name="best-practices-articles"></a>Artikel "Best practices"

Dieser Artikel enthält eine ausführliche Erläuterung Konzepte im Zusammenhang mit Cloud computing. 

- [API-Design][APIDesign] 

    Eine Diskussion Entwurfsaspekte berücksichtigen beim Entwerfen einer Webs-API.

- [API-Implementierung][APIImplementation] 

    Eine Reihe von Vorgehensweisen und Veröffentlichen einer Webs-API.

- [API-Sicherheitsleitfaden](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    Eine Erläuterung der Authentifizierung und Autorisierung betrifft (z. B. Tokentypen, Autorisierung Protokolle Autorisierung Flows und Threat Mitigation).

- [Automatische Skalierung Anleitung][AutoscalingGuidance] 

    Eine Zusammenfassung der Aspekte der Skalierung Lösungen ohne manuellen Eingriff.

- [Hintergrund Aufträge Anleitung][BackgroundJobsGuidance] 

    Eine Beschreibung der verfügbaren Optionen und empfohlene Vorgehensweisen für die Implementierung im Hintergrund unabhängig vom Vordergrund oder interaktiven Vorgänge ausgeführt werden sollen.

- [Anleitung für Content Delivery Network (CDN)][CDNGuidance] 

    Leitfaden und mit dem CDN Belastung Ihrer Anwendung minimieren und maximieren, Verfügbarkeit und Leistung empfohlen.

- [Zwischenspeichern der Anleitung][CachingGuidance] 

    Eine Zusammenfassung zum Zwischenspeichern verbessert die Leistung und Skalierung eines Systems.

- [Daten Partitioning Anleitung][DataPartitioningGuidance]

    Strategien, um Partitionsdaten mit können skalierbar, verbessern Konflikte verringern und die Performance optimiert.

- [Hinweise zur Überwachung und Diagnose][MonitoringandDiagnosticsGuidance] 

    Anleitung zum Nachverfolgen, wie die Benutzer Ihr System nutzen Ressourcen verfolgen und im Allgemeinen überwacht den Zustand und die Leistung Ihres Systems.

- [Empfohlene Namenskonventionen][naming-conventions] 

    Empfohlene Namenskonventionen für Azure-Ressourcen.

- [Allgemeine Anleitung wiederholen][RetryGeneralGuidance] 

    Diskussion der allgemeinen Konzepte für vorübergehende Fehler behandeln.

- [Wiederholen Sie dienstspezifische Anleitung][RetryServiceSpecificGuidance]

    Eine Zusammenfassung der Wiederholung Funktionen vieler Azure Services, einschließlich Informationen zur verwenden, anpassen oder Erweitern der Wiederholungsmechanismus für diesen Dienst.

## <a name="scenario-guides"></a>Szenario-Handbücher

- [Elasticsearch auf Windows Azure ausgeführte][elasticsearch] 
    
    Elasticsearch ist eine hochgradig skalierbare Open-Source-Suchmaschine und Datenbank. Eignet sich für Situationen, in denen schnelle Analyse und Erkennung von Informationen in großen Datasets. Dieser Leitfaden untersucht einige wichtige Aspekte bei einen Cluster Elasticsearch entwerfen.

- [Identitätsmanagement für mandantenfähigen Applikationen][identity-multitenant] 
    
    Tenancy ist eine Architektur, die mehrere Mandanten eine Anwendung freigeben wobei voneinander isoliert sind. Diese Anleitung zeigt, wie Sie Verwaltung von Benutzeridentitäten in einer mehrinstanzenfähigen Anwendung mit [Azure Active Directory] [ AzureAD] Anmeldung und Authentifizierung.
    
- [Big Data entwickeln](https://msdn.microsoft.com/library/dn749874.aspx)

    Dieses Handbuch untersucht die Verwendung von HDInsight für Szenarien wie iterative Untersuchung als ein Datawarehouse für ETL-Prozesse und Integration in vorhandene BI-Systeme. Darüber hinaus Anleitung zum Verständnis der Konzepte von großen Daten planen und Entwerfen big Data-Lösungen und Implementieren dieser Lösung.
    
## <a name="patterns"></a>Muster

- [Cloud-Entwurfsmuster: Vorschreibende Architektur Hinweise Cloudanwendungen](https://msdn.microsoft.com/library/dn568099.aspx)

    Cloud-Entwurfsmuster ist eine Bibliothek von Entwurfsmuster und damit verbundenen Anwendungsleitlinien Themen. Diese Hervorhebung davon Muster zeigt, wie jedes in Cloudarchitekturen passt anwenden.
    
- [Optimieren der Leistung von Cloudanwendungen](https://github.com/mspnp/performance-optimization)

    Diese Anleitung gilt eine Untersuchung allgemeine Anti-Muster, die apps unter Belastung skalieren behindern. Sie enthält Beispiele acht Antimuster und [Performance Analysis Primer](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md) und eine Anleitung zur [Leistung Kennzahlen](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md).

## <a name="reference-architectures"></a>Referenzarchitekturen

Unsere Referenzarchitekturen werden nach Szenario angeordnet.
Jede einzelne Architektur bietet empfohlenen Vorgehensweisen und Schritte und eine ausführbare Komponente, die die Empfehlung verkörpert.

Die aktuelle Bibliothek Referenzarchitekturen ist unter [http://aka.ms/architecture](http://aka.ms/architecture)verfügbar.

## <a name="resiliency-guidance"></a>Resiliency Anleitung

Diese Themen beschreiben die Anwendung entwerfen, die Fehler in einer verteilten Cloud-Umgebung sind.   

- [Übersicht über die Stabilität][ResiliencyOvervew]

     Wie auf der Azure-Plattform erstellen, die Wiederherstellung nach einem Ausfall und weiterhin funktionieren. Beschreibt einen strukturierten Ansatz zu Flexibilität von Entwurf, Implementierung, Bereitstellung und Betrieb.

- [Resiliency-Checkliste][resiliency-checklist]

    Eine Prüfliste mit empfohlenen, mit denen Sie planen eine Vielzahl Fehler, die auftreten können.

- [Fehleranalyse Modus][resiliency-fma] 

    Modus Fehleranalyse (FMA) ist ein Prozess zum Erstellen von Stabilität in einem System identifiziert mögliche Fehlerquellen. Als Ausgangspunkt für den Prozess FMA enthält dieser Artikel einen Katalog von möglichen Fehlermodi und ihre Schutzmaßnahmen. 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

