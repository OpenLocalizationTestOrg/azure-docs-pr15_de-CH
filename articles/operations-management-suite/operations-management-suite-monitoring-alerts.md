<properties 
   pageTitle="Warnung Management in Microsoft Produkte | Microsoft Azure"
   description="Eine Warnung gibt ein Problem, das Aufmerksamkeit des Administrators erfordert.  Dieser Artikel beschreibt die Unterschiede in wie Alerts erstellt und verwaltet in System Center Operations Manager (SCOM) und Protokollanalyse und bietet optimalen Nutzung der beiden Produkte für eine hybride alert-Management-Strategie." 
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
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="managing-alerts-with-microsoft-monitoring"></a>Mit Microsoft Alerts verwalten 

Eine Warnung gibt ein Problem, das Aufmerksamkeit des Administrators erfordert.  Gibt es Unterschiede zwischen System Center Operations Manager (SCOM) und Protokollanalyse in Operations Management Suite (OMS) wie Alerts erstellt werden, wie sie verwaltet und analysiert werden und wie Sie benachrichtigt werden, dass ein schwerwiegendes Problem erkannt wurde.

## <a name="alerts-in-operations-manager"></a>Warnung in Operations Manager
Alarme in SCOM entstehen durch einzelne Regeln oder Monitore an ein bestimmtes Problem.  Ein Monitor kann eine Warnung generieren, tritt es einen Fehlerzustand während eine Regel einige wichtiges Problem anzugeben, die nicht direkt auf den Zustand des verwalteten Objekts wird eine Warnung generieren kann.  Managementpacks enthalten eine Vielzahl von Workflows, die für die Anwendung oder den Dienst sie verwalten Warnregeln erstellen.  Teil der Konfiguration eines neuen Management Packs ist, um sicherzustellen, dass Sie übermäßige Alarme für Probleme erhalten, die Sie als kritisch betrachten nicht optimieren.

![SCOM Warnungsansicht](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM bietet Alerts mit einem Status von Administratoren geändert werden kann, um das Problem vollständiges alert-Management.  Wenn das Problem behoben wurde, legt der Administrator Warnung geschlossen zu diesem, die Zeitpunkt nicht mehr angezeigt wird in der aktive Warnungen anzeigen.  Von Monitoren generierten Warnungen können automatisch behoben werden, gibt der Monitor in einen fehlerfreien Zustand.

![Warnungsstatus](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Alarme in Protokollanalyse
Eine Warnung in Protokollanalyse Protokoll Abfrage erstellt, die automatisch in regelmäßigen Abständen ausgeführt wird.  Sie können eine Warnregel aus Protokoll Abfragen erstellen.  Wenn die Abfrage Ergebnisse, die von Ihnen angegebenen Kriterien entsprechen zurückgibt, wird eine Warnung erstellt.  Möglicherweise einer bestimmten Abfrage, die eine Warnung erstellt, wenn ein bestimmtes Ereignis erkannt wird, oder können Sie eine allgemeine Abfrage alle Fehlerereignis eine bestimmte Anwendung sucht.

Log Analytics Alerts OMS-Repository als Ereignis geschrieben und Log-Abfrage abgerufen werden.  Status wie SCOM keinen, damit Sie angeben können, wenn das Problem behoben wurde.

![OMS-Warnung](media/operations-management-suite-monitoring-alerts/oms-alert.png)

SCOM Protokollanalyse als Datenquelle verwendet wird, werden SCOM Alerts OMS-Repository geschrieben, erstellt und geändert werden.  

![SCOM-Warnung](media/operations-management-suite-monitoring-alerts/scom-alert.png)

[Alert-Management-Lösung](http://technet.microsoft.com/library/mt484092.aspx) bietet eine Zusammenfassung der aktiven Warnungen und mehrere allgemeine Abfragen unterschiedliche Abrufen von Alarmen festgelegt.  Dadurch effizienter Analyse Warnungen als Bericht in SCOM.  Sie können Drilldown aus der Zusammenfassung mit Detaildaten und ad-hoc-Abfragen rufen Sie unterschiedliche Alarme.

![Alert-Management-Lösung](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Benachrichtigung
In SCOM Benachrichtigungen senden eine e-Mail- oder Text auf Alarme, die bestimmte Kriterien entsprechen.  Sie können verschiedene Benachrichtigungsabonnements, die von anderen Personen benachrichtigt Kriterien wie das Objekt überwacht wird, den Schweregrad der Warnung, die Art des Problems festgestellt oder die Uhrzeit erstellen.

Einige Abonnements können eine vollständige Anmeldung Strategie für eine große Anzahl von managementpacks verwendet werden.

![SCOM alerts](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Protokollanalyse können über e-Mail benachrichtigt, dass eine Warnung erstellt wurde, indem Sie eine e-Mail-Benachrichtigungsaktion auf jede [Regel](http://technet.microsoft.com/library/mt614775.aspx).  Die Fähigkeit von SCOM abonnieren mehrere Warnungen mit einer einzelnen Regel keinen.  Sie möchten eigene Warnungsregeln erstellen, da keine OMS eine vorkonfigurierte bereitstellt.

![Analytics Alerts anmelden](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Sie können vollständig SCOM Alarme in Protokollanalyse jedoch nicht verwalten, da Sie nur in der Betriebskonsole ändern können.  Protokollanalyse eignet sich als alert-Management bearbeiten aber mit Analysetools, SCOM allein nicht haben.

## <a name="alert-remediation"></a>Warnung Remediation
[Remediation](http://technet.microsoft.com/library/mt614775.aspx) verweist Versuch von einer Warnung angegebene Problem automatisch beheben.
  
SCOM können Sie auf einen Monitor in einem fehlerhaften Zustand Diagnose und Wiederherstellung ausführen.  Dies geschieht zum Erstellen von Warnung Monitor gleichzeitig.  Diagnose und Wiederherstellung werden normalerweise als Skript implementiert, die auf dem Agent ausgeführt wird.  Eine Diagnose versucht, sammeln Sie weitere Informationen über das erkannte Problem während eine Wiederherstellung versucht, das Problem zu beheben.

Protokollanalyse ermöglicht eine [Azure Automation Runbook](https://azure.microsoft.com/documentation/services/automation/) starten oder eine Webhook als Antwort auf eine Warnung Protokollanalyse aufrufen.  Runbooks können komplexen Logik in PowerShell enthalten.  Das Skript in Azure ausgeführt wird und Azure Ressourcen oder externe Ressourcen aus der Cloud zugreifen kann.  Azure-Automatisierung muss auf einem Server im lokalen Datencenter Runbooks ausführen, aber dieses Feature ist derzeit nicht verfügbar beim Starten von Runbooks Protokollanalyse Alerts auf.

Sowohl Recovery in SCOM Runbooks in OMS PowerShell-Skripts enthalten, aber Recovery sind schwieriger zu verwalten, da sie in einem Management Pack enthalten sein müssen.  Runbooks werden in Azure Automation zum Erstellen bietet, testen und Verwalten von Runbooks gespeichert.

Bei Verwendung von SCOM als Datenquelle für Protokollanalyse motivieren Sie eine Protokollanalyse Warnung mithilfe einer Protokollabfrage später SCOM in OMS-Repository gespeichert.  Dies wäre ein Runbook Azure Automation als Antwort auf eine Warnung SCOM ausgeführt werden.  Natürlich da Runbook in Azure ausgeführt wird, wäre das keine geeignete Strategie für Recovery für lokale Probleme.

## <a name="next-steps"></a>Nächste Schritte

- Lernen Sie die Details der [Warnung in System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).