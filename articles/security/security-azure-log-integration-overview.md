<properties
   pageTitle="Einführung in Microsoft Azure Protokoll Integration | Microsoft Azure"
   description="Azure Protokoll Integration, seine wichtigsten Funktionen und Funktionsweise erläutert."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# <a name="introduction-to-microsoft-azure-log-integration-preview"></a>Einführung in Microsoft Azure Protokoll Integration (Vorschau)

Azure Protokoll Integration, seine wichtigsten Funktionen und Funktionsweise erläutert.

## <a name="overview"></a>Übersicht

Plattform als Service (PaaS) und Infrastructure as a Service (IaaS) in Azure gehostet generieren eine große Datenmenge im Sicherheitsprotokolle. Diese Protokolle enthalten wichtigen Informationen, der Intelligenz und leistungsstarke Einblick in Richtlinienverstöße, internen und externen Gefahren, Einhaltung behördlicher Auflagen und Anomalien im Netzwerk, Host und Benutzeraktivitäten bereitstellen kann.

Azure Protokoll-Integration können Sie unformatierte Protokollen von Azure Ressourcen in der lokalen Sicherheitsinformations- und Ereignismanagement (SIEM) integriert. Azure Protokoll Integration sammelt Azure-Diagnose von Fenster *(Bündel)* virtuellen Maschinen sowie Diagnose von Partner Solutions wie Web Application Firewall (WAF). Diese Integration ermöglicht ein einheitliches Dashboard für alle Ihre Daten lokal oder in der Cloud, damit Sie sammeln, Korrelieren, analysieren und für sicherheitsrelevante Ereignisse benachrichtigen.

![Integration von Azure Protokoll][1]

## <a name="what-logs-can-i-integrate"></a>Welche Protokolle können werden integriert?

Azure führt ausführliche Protokollierung für jede Azure Service. Diese Protokolle werden von zwei Haupttypen unterteilt:

- **Control-Management-Protokolle**, die Sichtbarkeit in Azure Ressourcen-Manager erstellen, UPDATE und DELETE-Operationen. Azure Überwachungsprotokolle ist ein Beispiel für dieses Protokoll.
- **Ebene Datenprotokollen**Einblick in die Ereignisse im Rahmen der Nutzung der Azure-Ressource geben. Beispiele für diese Art des Protokolls sind die Windows System, Sicherheit und Anwendung in einer virtuellen Maschine protokolliert.

Azure Protokoll Integration unterstützt derzeit die Integration von Azure Überwachungsprotokolle, virtuellen Protokolle und Warnungen Azure Security Center.

Zur Integration von Azure Protokoll Fragen senden Sie eine e-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument wurden Azure Protokoll Integration vorgestellt. Erfahren Sie mehr über Azure Protokoll Integration und Protokolle unterstützt, finden Sie hier:

- [Microsoft Azure Protokoll Integration für Azure-Protokolle (Preview)](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center für Details, das System und Installationshinweise Azure Protokoll Integration.
- [Erste Schritte mit Azure Protokoll Integration](security-azure-log-integration-get-started.md) – in diesem Lernprogramm führt Sie durch Installation von Azure Protokoll Integration und Protokolle von Azure-Speicher, Azure Prüfprotokolle und Sicherheitshinweise des Sicherheitscenters integrieren.
- [Konfigurationsschritte für Partner](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – dieser Blogbeitrag zeigt Azure Protokoll Integration mit Splunk ArcSight HP und IBM QRadar zu konfigurieren.
- [Azure Protokoll Integration häufig gestellte Fragen (FAQ)](security-azure-log-integration-faq.md) - dieser FAQ beantwortet Fragen zur Azure Protokoll Integration.
- [Sicherheitshinweise des Sicherheitscenters Integration mit Azure anmelden Integration](../security-center/security-center-integrating-alerts-with-log-integration.md) – dieses Dokument zeigt, wie Synchronisierung Sicherheitshinweise des Sicherheitscenters mit virtuellen Sicherheitsereignisse werden von Azure Diagnostics und Azure Überwachungsprotokolle der Protokollanalyse oder SIEM-Lösung.
- [Neue Funktionen für Azure Diagnostics und Azure Überwachungsprotokolle](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) in diesem Blogbeitrag lernen Sie Überwachungsprotokolle Azure und andere Funktionen Einblicke in die Vorgänge der Azure-Ressourcen.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png
