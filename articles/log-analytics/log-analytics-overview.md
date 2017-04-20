<properties
   pageTitle="Was ist Protokollanalyse? | Microsoft Azure"
   description="Protokollanalyse ist ein Dienst in Operations Management Suite (OMS), mit der Sie sammeln und Analysieren von operativen Daten von Ressourcen in der Cloud und lokalen Umgebung.  Dieser Artikel enthält eine kurze Übersicht über die verschiedenen Komponenten der Protokollanalyse und Links zu ausführlichen Inhalt."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="what-is-log-analytics"></a>Was ist Protokollanalyse?
Protokollanalyse ist ein Dienst [Operations Management Suite \(OMS\) ](../operations-management-suite/operations-management-suite-overview.md) , hilft Ihnen das Sammeln und Analysieren von Daten von Ressourcen in der Cloud und lokalen Umgebung. Sie erhalten in Echtzeit Einblicke mit Such- und benutzerdefinierte Dashboards Millionen von Datensätzen für alle Arbeitslasten und Server unabhängig von deren Standort leicht analysieren.


## <a name="log-analytics-components"></a>Log Analytics Komponenten
An Mitte Protokollanalyse ist OMS-Repository der Azure-Cloud gehostet wird.  In das Repository werden von verbundenen Quellen Konfigurieren von Datenquellen und Hinzufügen zu Ihrem Abonnement Daten.  Datenquellen und Solutions erstellt jedes verschiedene Datensatztypen, die einen eigenen Satz von Eigenschaften, aber möglicherweise noch analysiert zusammen in Abfragen zum Repository.  Dadurch werden Tools und Methoden verwenden, um verschiedene Arten von Daten aus verschiedenen Quellen arbeiten.


![OMS-repository](media/log-analytics-overview/overview.png)


Verbundene Datenquellen sind Computer und andere Ressourcen, die von Protokollanalyse gesammelten Daten generieren.  Dazu gehören Agenten installiert auf [Windows-](log-analytics-windows-agents.md) und [Linux](log-analytics-linux-agents.md) -Computern, die direkt oder Agents in einer [Verwaltungsgruppe System Center Operations Manager verbunden](log-analytics-om-agents.md).  Protokollanalyse können auch von [Azure-Speicher](log-analytics-azure-storage.md)Daten.

[Datenquellen](log-analytics-data-sources.md) sind die unterschiedlichen Arten von Daten aus jeder Quelle verbunden.  Dazu gehören Ereignisse und [Leistungsdaten](log-analytics-data-sources-performance-counters.md) von [Windows](log-analytics-data-sources-windows-events.md) und Linux-Agenten zusätzlich wie [IIS-Protokolle](log-analytics-data-sources-iis-logs.md)und [Protokolle benutzerdefinierten Text](log-analytics-data-sources-custom-logs.md).  Jede Datenquelle, die Sie sammeln möchten und die Konfiguration erfolgt automatisch die einzelnen verbundenen konfigurieren.


## <a name="analyzing-log-analytics-data"></a>Protokollanalyse Datenanalyse
Ihre Interaktion mit Protokollanalyse wird OMS-Portal in allen Browsern ausgeführt und bietet Zugriff auf Einstellungen und mehrere Tools zum Analysieren und auf Daten.  Über das Portal können Sie nutzen, [Protokoll suchen](log-analytics-log-searches.md) , in dem Sie Abfragen analysieren Daten [Dashboards](log-analytics-dashboards.md) mit insofern Ihrer wertvollsten suchen und [Lösungen](log-analytics-add-solutions.md) anpassen können zusätzliche Funktionen und Analysetools erstellen.

![OMS-portal](media/log-analytics-overview/portal.png)


Protokollanalyse stellt eine Abfragesyntax schnell abzurufen und Daten im Repository.  Erstellen und speichern [Protokoll Suche](log-analytics-log-searches.md) direkt im Portal OMS Datenanalyse oder Protokoll Suchvorgänge ausgeführt automatisch zum Erstellen einer Warnung, wenn die Ergebnisse der Abfrage eine wichtige Voraussetzung anzugeben.

![Protokolldatei suchen](media/log-analytics-overview/log-search.png)

Geben Sie schnell grafisch den Zustand Ihrer gesamten Umgebung können Sie Ihr [Dashboard](log-analytics-dashboards.md)Visualisierung Suche gespeicherte Protokoll hinzufügen.   

![Dashboard](media/log-analytics-overview/dashboard.png)

Um Datenanalyse außerhalb Protokollanalyse exportieren Sie die Daten in Excel oder [Power BI](log-analytics-powerbi.md) -Tools aus dem OMS-Repository.  Sie können auch [Protokoll Such-API](log-analytics-log-search-api.md) benutzerdefinierte Lösung erstellen, die Protokollanalyse Daten oder Integration mit anderen Systemen nutzen.

## <a name="solutions"></a>Solutions
Solutions hinzufügen Protokollanalyse Funktionalität.  Sie hauptsächlich in der Cloud und Analyse der Daten im Repository OMS. Sie können auch neue Datensatztypen zu definieren, die Protokolldatei suchen oder zusätzliche Benutzeroberfläche der Lösung im Dashboard OMS analysiert werden kann.  

![Tracking-Lösung ändern](media/log-analytics-overview/change-tracking.png)


Solutions stehen für eine Vielzahl von Funktionen und leicht finden Sie Lösungen und aus den Lösungskatalog [OMS Arbeitsbereich hinzufügen](log-analytics-add-solutions.md) .  Viele werden automatisch bereitgestellt und arbeiten, während andere eine Konfiguration erfordern.

![Lösungskatalog](media/log-analytics-overview/solution-gallery.png)

## <a name="log-analytics-architecture"></a>Analytics Architektur
Die Bereitstellung der Protokollanalyse sind minimal, da die zentralen Komponenten der Azure-Cloud gehostet werden.  Dazu gehören neben Services, mit denen Sie zu korrelieren und Analysieren von Daten-Repository.  Das Portal über einen Webbrowser möglich, keine Notwendigkeit für die Client-Software besteht.

Installieren Sie Agents auf [Windows-](log-analytics-windows-agents.md) und [Linux](log-analytics-linux-agents.md) -Computern besteht jedoch keine zusätzlichen Agent erforderlich für Computer bereits Mitglied einer [Verwaltungsgruppe SCOM verbunden](log-analytics-om-agents.md)sind.  SCOM-Agenten weiterhin mit Verwaltungsserver kommunizieren, die ihre Daten an Protokollanalyse weiterleitet.  Einige Lösungen erfordern jedoch Agents kommunizieren direkt mit Protokollanalyse.  Zu jeder Lösung geben die Kommunikation an.

Beim [Anmelden für Protokollanalyse](log-analytics-get-started.md)OMS-Arbeitsbereich erstellen Sie.  Arbeitsbereich vorstellen als ein OMS-Umgebung mit eigenen Daten-Repository, Datenquellen und Lösungen. Ihr Abonnement zu mehreren Umgebungen wie testen, können Sie mehrere Arbeitsbereiche erstellen.

![Analytics Architektur](media/log-analytics-overview/architecture.png)


## <a name="next-steps"></a>Nächste Schritte

- [Melden Sie sich für ein kostenloses Konto Protokollanalyse](log-analytics-get-started.md) in Ihrer Umgebung testen.
- Zeigen Sie die verschiedenen [Datenquellen](log-analytics-data-sources.md) zur Datensammlung in OMS-Repository an
- [Durchsuchen Sie die Lösung im Lösungskatalog](log-analytics-add-solutions.md) Protokollanalyse Funktionen hinzu.
