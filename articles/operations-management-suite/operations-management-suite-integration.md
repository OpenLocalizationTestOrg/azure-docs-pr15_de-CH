<properties
   pageTitle="Integration in Operations Management Suite (OMS) | Microsoft Azure"
   description="Neben den standardmäßigen Features eines OMS, Integration mit anderen Programmen Management und Dienste eine Hybrid-Management Umgebung zu benutzerdefinierten Szenarien in Ihrer Umgebung eindeutig oder zu einer benutzerdefinierten Management für Ihre Kunden.  Dieser Artikel bietet einen Überblick über die verschiedenen Optionen für die Integration OMS mit Links zu Artikeln mit detaillierten technischen Informationen."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="bwren" />

# <a name="integrating-with-operations-management-suite-oms"></a>Integration in Operations Management Suite (OMS)

Operations Management Suite ist Microsoft Cloud-basierten IT Management Lösung, verwalten und schützen Ihre lokalen und cloud-Infrastruktur.  Neben den standardmäßigen Features eines OMS, Integration mit anderen Programmen Management und Dienste eine Hybrid-Management Umgebung zu benutzerdefinierten Szenarien in Ihrer Umgebung eindeutig oder zu einer benutzerdefinierten Management für Ihre Kunden.  Dieser Artikel bietet einen Überblick über die verschiedenen Optionen für die Integration OMS-Services mit Links zu Artikeln mit detaillierten technischen Informationen. 



## <a name="log-analytics"></a>Protokollanalyse
Protokollanalyse erfasst Daten werden in einem Repository gespeichert, die in Azure gehostet wird.  Alle Daten im Repository steht im Protokoll suchen mit Blick auf große Datenmengen.  Integration Bedarf möglicherweise Repository mit neuen Daten für die Analyse zur Verfügung stellen zu füllen oder Daten im Repository eine neue Visualisierung oder ein anderes Management-Tool integriert.

Jedes Datenelement im Repository wird als Datensatz gespeichert.  Beim Füllen des Repository sollten Sie Benutzer mit dem Datensatztyp, den Projektmappe verwendet und eine Beschreibung der Eigenschaften bereitstellen.  Beim Abrufen von Daten benötigen Sie diese Informationen über die Arbeit mit Daten.

![Auffüllen das OMS-repository](media/operations-management-suite-integration/repository.png)


### <a name="populate-the-log-analytics-repository"></a>Füllen Sie das Protokollanalyse-Repository auf
Es gibt mehrere Methoden zum Auffüllen von OMS-Repository.  Die Methode hängt von Faktoren wie befindet sich die Quelldaten, das Format der Daten und die Clients unterstützt werden müssen.  Wenn Daten im Repository gespeichert ist, macht es keinen Unterschied wie erfasst wurde.

Die folgenden Abschnitte beschreiben die verschiedenen Optionen zum Auffüllen des OMS-Repository.

#### <a name="connected-sources-and-data-sources"></a>Verbundene Datenquellen und Datenquellen 
Verbunden sind die Speicherorte, in denen Daten für OMS-Repository abgerufen werden können.  Datenquellen verbunden Quellen ausführen und definieren die Daten, die gesammelt werden.  Wenn die Anwendung Daten zu dieser Datenquellen schreibt, können dann Sie sie sammeln die Datenquelle konfiguriert.  Z. B. wenn die Anwendung Syslog-Ereignisse erstellt, können dann sie Syslog-Datenquelle auf einem Linux-Agent gesammelt werden.

- [Datenquellen in Protokollanalyse](../log-analytics/log-analytics-data-sources.md)

#### <a name="solutions"></a>Solutions

Solutions erweitern die Funktionen von OMS.  Eine Lösung sammeln Daten aus der verbundenen Datenquelle oder es möglicherweise analysieren Datensätze im Repository zusammengestellt.  Jede Lösung von Microsoft ist einen einzelnen Artikel, der die Details der Daten bereitstellt, die sammelt.

- [Projektmappen in Protokollanalyse](../log-analytics/log-analytics-add-solutions.md)



#### <a name="http-data-collector-api"></a>Datensammler HTTP-API

Log Analytics HTTP Data Collector-API ist eine REST-API, die JSON-Daten der Protokollanalyse-Repository hinzufügen.  Diese API kann genutzt werden, wenn eine Anwendung, die Daten über die anderen Datenquellen oder Lösungen nicht.  Hiermit können Sie das Repository von einem beliebigen Client aufgefüllt wird, die die API aufrufen und nicht termingerecht Auflistung der Datenquelle oder Lösung.

- [Log Analytics HTTP Datensammler API](../log-analytics/log-analytics-data-collector-api.md)


### <a name="retrieve-data-from-the-log-analytics-repository"></a>Abrufen von Daten aus dem Repository Protokollanalyse

Es gibt mehrere Methoden zum Abrufen von Daten aus dem OMS-Repository.  Sie möchten Benutzer über die Konsole OMS Datenabruf und bieten verschiedene Visualisierung und Analyse.  Sie können auch Daten aus einem externen Prozess wie ein anderes Management abrufen.

#### <a name="log-searches"></a>Protokolldatei suchen

Alle Daten im Repository OMS steht Log durchsucht.  Benutzer können ihre eigenen ad-hoc-Analyse der OMS-Konsole ausführen oder erstellen Sie ein Dashboard mit einer Visualisierung für ein bestimmtes Protokoll suchen.  Projektmappen können benutzerdefinierte Ansichten mit Visualisierung auf Grundlage der vordefinierten Suche enthalten.  Die Protokoll-Such-API können Datenzugriff in OMS-Repository von einer externen Anwendung oder Management-Tools.  

- [Log durchsucht Protokollanalyse](../log-analytics/log-analytics-log-searches.md)
- [Protokollanalyse anmelden Suche REST-API](../log-analytics/log-analytics-log-search-api.md)
- [Log Analytics cmdlets](https://msdn.microsoft.com/library/mt188224.aspx)



#### <a name="custom-views"></a>Benutzerdefinierte Ansichten 
Ansicht-Designer können Sie benutzerdefinierte Ansichten der OMS-Konsole, die Benutzer mit Visualisierung und Analyse der Daten in der Projektmappe erstellen.  Jede Ansicht enthält eine Fläche, die auf der Hauptseite der Konsole und eine beliebige Anzahl Teile Visualisierung, die auf Protokoll suchen, die Sie definieren angezeigt.
  
- [Log Analytics Ansicht-Designer](../log-analytics/log-analytics-view-designer.md)


#### <a name="power-bi"></a>Power BI

Protokollanalyse exportieren automatisch Daten aus dem OMS-Repository in Power BI damit die Visualisierung und Analyse-Tools nutzen können.  Damit die Daten auf dem neuesten Stand ist, werden dieser Export nach einem Zeitplan durchgeführt. 

- [Power BI Protokollanalyse-Datenexport](../log-analytics/log-analytics-powerbi.md)




## <a name="automation"></a>Automatisierung

OMS automatisieren Prozesse Daten reagieren oder andere Verwaltungsfunktionen ausführen.  Können sie Daten aus der Anwendung und OMS-Repository einfügen oder die Korrektur eines bekannten Problems auf Daten im Repository können automatisieren. 

![Automatisierung](media/operations-management-suite-integration/automate.png)

### <a name="runbooks"></a>Runbooks

Runbooks in Azure Automation PowerShell-Skripts und Workflows in Azure Cloud ausführen.  Sie können Ressourcen in Azure oder anderen Ressourcen, die auf die Cloud verwalten.  Runbooks kann in einem lokalen Rechenzentrum mit Hybrid Runbook Worker auch ausgeführt werden.  Sie können ein Runbook Azure-Portal oder externe Prozesse mit einer Reihe von Methoden der Automation API oder PowerShell starten.

- [Starten Sie ein Runbook in Azure Automation](../automation/automation-starting-a-runbook.md)
- [Azure Automation-cmdlets](https://msdn.microsoft.com/library/dn690262.aspx)
- [Automatisierung REST-API](https://msdn.microsoft.com/library/mt662285.aspx)
- [Automatisierung .NET](https://msdn.microsoft.com//library/mt465763.aspx)

### <a name="alerts"></a>Alarme

Warnregeln werden automatisch Protokolldateien sucht nach einem Zeitplan ausgeführt.  Wenn die Ergebnisse bestimmte Kriterien kann resultierende Warnung ein Runbook in Azure Automation starten oder Webhook einen externen Prozess starten kann.  Beide Antworten zählen die Details der Warnung, die Daten in der Protokolldatei Suche zurückgegeben.

- [Alarme in Protokollanalyse](../log-analytics/log-analytics-alerts.md)
- [Log Analytics Alert-API](../log-analytics/log-analytics-api-alerts.md)


## <a name="backup-and-site-recovery"></a>Sicherung und Wiederherstellung von Sites

Azure Backup und Site Recovery Dienstleistungen schützen Ihre Unternehmensdaten und die Verfügbarkeit von Servern und Applikationen.  Sie können diese Dienste so Szenarien wie backup Dienstleistungen für Ihre Anwendung oder Initiierung eines Failovers eines virtuellen Computers nutzen.

- [Azure Backup-cmdlets](https://msdn.microsoft.com/library/mt619253.aspx)
- [Azure Site Recovery REST-API](https://msdn.microsoft.com/library/azure/mt750497.aspx)
- [Azure Site Recovery Cmdlets](https://msdn.microsoft.com/library/mt637930.aspx)

## <a name="custom-solutions"></a>Lösungen

Sie können in einer benutzerdefinierten Lösung im Arbeitsbereich oder im Arbeitsbereich des Kunden Integrationslogik kapseln.  Ihre Lösung zählen Integration Methoden in diesem Artikel neben anderen Ressourcen ein umfassendes Management-Szenario.  Die Ressourcen in der Lösung werden gepackt, wird die Projektmappe entfernt, alle Ressourcen, die Erstellung der OMS-Arbeitsbereich und Azure-Abonnement entfernt wurden.

Die Lösung konnte beispielsweise ein Runbook Automatisierung zum Sammeln und Verarbeiten von Daten und füllen Sie dann Protokollanalyse Repository über HTTP Data Collector-API.  Sie können eine benutzerdefinierte Ansicht auch, die präsentiert und analysiert Daten.  

- Benutzerdefinierte Lösungen (in Kürze verfügbar)    

## <a name="next-steps"></a>Nächste Schritte
- [OMS-SDK](operations-management-suite-sdk.md) technische Informationen zum Automatisieren von OMS Dienste verweisen.  
