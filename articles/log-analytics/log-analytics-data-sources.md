<properties 
   pageTitle="Datenquellen in Protokollanalyse | Microsoft Azure"
   description="Definieren Sie Datenquellen Daten Protokollanalyse erfasst von Agenten und anderen Quellen verbunden.  Dieser Artikel beschreibt das Konzept wie Protokollanalyse verwendet Datenquellen wird erläutert, wie sie konfigurieren und bietet eine Übersicht über die verschiedenen Datenquellen."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="data-sources-in-log-analytics"></a>Datenquellen in Protokollanalyse

Protokollanalyse verbundenen Datenquellen im Arbeitsbereich OMS Daten erfasst und in OMS-Repository gespeichert.  Die Datenquellen, die Sie konfigurieren aus gesammelten Daten bestimmt.  Daten im Repository OMS ist als Datensätze gespeichert.  Jede Datenquelle erstellt Datensätze eines bestimmten Typs mit jeder einen eigenen Satz von Eigenschaften.

![Analytics-Datensammlung protokollieren](./media/log-analytics-data-sources/overview.png)

Datenquellen sind anders als auch Daten aus Quellen verbunden und Erstellen von Datensätzen im Repository OMS OMS Lösungen.  Projektmappen können im Arbeitsbereich von Lösungskatalog hinzugefügt werden und normalerweise weitere Analysetools im OMS-Portal bereitstellen.  

## <a name="summary-of-data-sources"></a>Übersicht über Datenquellen

Die derzeit im Protokollanalyse Datenquellen werden in der folgenden Tabelle aufgeführt.  Jede hat eine Verknüpfung zu einem separaten Artikel ins Detail für diese Datenquelle.

| Datenquelle | Ereignistyp | Beschreibung |
|:--|:--|:--|
| [Benutzerdefinierte Protokolle](log-analytics-data-sources-custom-logs.md) | \<LogName\>_CL | Textdateien auf Windows- oder Linux-Agents enthält Informationen. |
| [Windows-Ereignisprotokolle](log-analytics-data-sources-windows-events.md) | Ereignis | Auf Computern unter Windows gesammelten Ereignisse aus dem Ereignisprotokoll. |
| [Windows-Leistungsindikatoren](log-analytics-data-sources-performance-counters.md) | Leistung | Leistungsindikatoren von Windows-Computern gesammelt werden. |
| [Linux-Leistungsindikatoren](log-analytics-data-sources-performance-counters.md) | Leistung | Leistungsindikatoren von Linux-Computern. |
| [IIS-Protokolle](log-analytics-data-sources-iis-logs.md) | W3CIISLog | Internet-Informationsdienste protokolliert im W3C-Format. |
| [Syslog](log-analytics-data-sources-syslog.md) | Syslog | Syslog-Ereignisse auf Computern Windows oder Linux. |

## <a name="configuring-data-sources"></a>Konfigurieren von Datenquellen

Konfigurieren von Datenquellen aus dem Menü **Daten** Protokollanalyse **Settings**.  Jede Konfiguration wird an alle verbundenen Datenquellen in Ihrem Arbeitsbereich OMS übermittelt.  Diese Konfiguration kann nicht derzeit Agenten ausschließen.

![Konfigurieren von Windows-Ereignissen](./media/log-analytics-data-sources/configure-events.png)

2. Wählen Sie in der Konsole OMS Kachel **Einstellungen aus**
3. **Daten**auswählen.
4. Klicken Sie auf die Datenquelle konfigurieren.
5. Finden Sie in der Dokumentation für jede Datenquelle in der obigen Tabelle für ihre Konfiguration.

## <a name="data-collection"></a>Datensammlung

Data Source-Konfigurationen werden auf Agents, die direkt mit OMS innerhalb weniger Minuten verbunden sind.  Die angegebenen Daten vom Agent erfasst und Intervallen für jede Datenquelle direkt an Protokollanalyse geliefert.  Dokumentation für jede Datenquelle für diese Angaben.

Für System Center Operations Manager (SCOM) Agents in einer Verwaltungsgruppe verbundenen sind Data Source Konfigurationen managementpacks übersetzt und an die Verwaltungsgruppe 5 Minuten standardmäßig.  Der Agent des Management Packs wie jede andere downloads und sammelt die angegebenen Daten. Je nach Datenquelle werden entweder an den Verwaltungsserver gesendet die Daten der Protokollanalyse weiterleitet, oder der Agent sendet die Daten an Protokollanalyse ohne Server Management. [Einzelheiten zur Datensammlung für OMS-Features und Solutions](log-analytics-add-solutions.md#data-collection-details-for-oms-features-and-solutions) Einzelheiten finden Sie unter.  Lesen Sie Details SCOM und OMS und ändern die Häufigkeit dieser Konfiguration [Konfigurieren](log-analytics-om-agents.md)Integration mit System Center Operations Manager übermittelt.

## <a name="log-analytics-records"></a>Analytics-Datensätze

Alle Daten der Protokollanalyse ist in OMS-Repository als Datensätze gespeichert.  Datensätze von verschiedenen Datenquellen erfasst haben einen eigenen Satz von Eigenschaften und die **Typ** -Eigenschaft identifiziert werden.  Für jeden Datensatztyp finden Sie unter Dokumentation für jede Datenquelle und Lösung für Details.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Solutions](log-analytics-add-solutions.md) funktionell Protokollanalyse auch OMS-Repository sammeln Daten.
- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Daten aus Datenquellen und Solutions analysieren.  
- Konfigurieren Sie [Alerts](log-analytics-alerts.md) Sie kritische Daten aus Datenquellen und Solutions proaktiv benachrichtigt.
