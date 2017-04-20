<properties
   pageTitle="Operations Management Suite Sicherheits- und Audit-Lösung Sicherheit | Microsoft Azure"
   description="Dieses Dokument erläutert, wie Daten verwaltet und Operations Management Suite Sicherheit und Audits Lösung gewährleistet."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>

# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Operations Management Suite Sicherheits- und Lösung Sicherheit

Damit Kunden verhindern, erkennen und Bedrohungen [Operations Management Suite (OMS) Sicherheit Audit-Lösung](operations-management-suite-overview.md) sammelt und verarbeitet Daten über Ihre Ressourcen umfasst:

- Sicherheitsprotokoll
- Event Tracing for Windows (ETW) Ereignisse
- AppLocker Überwachungsereignisse
- Windows-Firewall-Protokoll
- Erweiterte Bedrohung Analytics-Ereignisse
- Ergebnisse der baseline
- Antimalware-Bewertung
- Ergebnisse der Update-patch
- Syslogs Streams, die explizit auf dem Agent aktiviert sind

Wir machen starke gegenüber den Datenschutz und die Sicherheit dieser Daten schützen. Microsoft befolgt strenge Richtlinien für Compliance und Sicherheit – von einem codieren.
Dieser Artikel beschreibt, wie Daten verwaltet und geschützt OMS Sicherheit und Audit-Lösung.

## <a name="data-sources"></a>Datenquellen

OMS-Sicherheit und Audits Lösung analysieren Daten aus virtuellen Maschinen und Computer dem OMS-Agent installiert ist. OMS-Sicherheit und Audit-Lösung können Informationen über Sicherheitsereignisse, wie Windows-Ereignisprotokoll, Prüfprotokolle, IIS-Protokolle und Syslog-Nachrichten sammeln. Beispiele für solche Daten: Betriebssystem und Version unter Prozesse, Computernamen, IP-Adressen angemeldet Benutzer und Tenant-ID.  

## <a name="data-protection"></a>Datenschutz

**Trennung der Daten**: Daten werden für jede Komponente der Service logisch getrennt. Alle Daten pro Unternehmen gekennzeichnet ist. Diese Kennzeichnung im gesamten Datenlebenszyklus beibehalten und wird auf jeder Ebene des Diensts erzwungen. 

**Datenzugriff**: Sicherheitsaspekte und Sicherheitsrisiken untersuchen, Microsoft Personal Informationen gesammelt oder analysiert Dienste zugreifen können. Wir halten zu [Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) - [Datenschutzrichtlinie](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), die besagen, dass Microsoft nicht Daten verwenden oder Informationen zu kommerziellen Zwecken Werbung oder ähnlichen abgeleitet. Sicherheitsaspekte und Sicherheitsrisiken untersuchen, können Microsoft Personal Informationen gesammelt oder analysiert Dienste zugreifen. Wir verwenden nur Daten als Azure Services, einschließlich Zwecke mit diesen Diensten bieten. Sie behalten alle Rechte auf Ihre eigenen Daten.

**Daten**: Microsoft verwendet Muster und Bedrohungsanalyse gesehen über mehrere Mandanten zu unserer Verhütung und Funktionen; Wir tun dies Datenschutz Verpflichtungen gemäß unserer [Datenschutzgarantie](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [AZURE.NOTE] Speicherort ist Ebene Arbeitsbereich OMS während der Erstellung Workspace Bestandteil der ursprünglichen OMS Sicherheits- und Konfiguration ist konfiguriert.

## <a name="see-also"></a>Siehe auch

In diesem Dokument haben Sie wie Daten verwaltet und im OMS geschützt. OMS Sicherheits- und Lösung finden Sie unter:

- [Operations Management Suite (OMS) (Übersicht)](operations-management-suite-overview.md)
- [Überwachen und reagieren auf Sicherheitshinweise Operations Management Suite Sicherheit und Audit-Lösung](oms-security-responding-alerts.md)
- [Überwachen von Ressourcen in Operations Management Suite Sicherheit und Audit-Lösung](oms-security-monitoring-resources.md)

