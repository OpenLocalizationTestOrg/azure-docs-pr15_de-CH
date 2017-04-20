<properties 
   pageTitle="Windows-Ereignisprotokolle in Protokollanalyse | Microsoft Azure"
   description="Windows-Ereignisprotokolle sind eines der am häufigsten verwendeten Protokollanalyse Datenquellen.  Dieser Artikel beschreibt die Auflistung Windows-Ereignisprotokolle und Details des in OMS-Repository erstellten Datensätze konfigurieren."
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

# <a name="windows-event-log-data-sources-in-log-analytics"></a>Windows-Ereignisprotokoll-Datenquellen in Protokollanalyse

Windows-Ereignisprotokolle sind eines der am häufigsten verwendeten [Datenquellen](log-analytics-data-sources.md) für Windows-Agenten verwendet, da dies die Methode die meisten Informationen und Fehler.  Sammeln von Ereignissen von Standardprotokolle wie System- und neben benutzerdefinierte Protokolle erstellt durch Programme, die Sie überwachen möchten.

![Windows-Ereignisse](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Konfigurieren von Windows-Ereignisprotokolle

Konfigurieren Sie Windows-Ereignisprotokolle im [Menü Daten in Analytics Einstellungen](log-analytics-data-sources.md#configuring-data-sources).

Protokollanalyse erfasst nur Ereignisse von den Windows-Ereignisprotokollen, die in den Einstellungen angegeben sind.  Hinzufügen ein neues Protokolls im Namen der Protokolldateien eingeben und auf **+**.  Für jedes Protokoll werden nur Ereignisse mit der ausgewählten Schweregrade gesammelt.  Überprüfen Sie die Schweregrade für das Protokoll, das Sie erfassen möchten.  Sie können keine weiteren Kriterien zum Filtern von Ereignissen bereitstellen.

![Konfigurieren von Windows-Ereignissen](media/log-analytics-data-sources-windows-events/configure.png)


## <a name="data-collection"></a>Datensammlung

Protokollanalyse erfasst jedes Ereignis, das ausgewählten Schweregrad aus überwachten Ereignisprotokoll entspricht das Ereignis erstellt wurde.  Der Agent wird stattdessen in jedem Ereignis Protokoll aufzeichnen, die aus erfasst.  Wenn der Agent einen Zeitraum offline geht, dann erfasst Protokollanalyse Ereignisse aus dem letzten, auch wenn diese Ereignisse erstellt wurden, während der Agent offline war.


## <a name="windows-event-records-properties"></a>Windows Ereigniseigenschaften Datensätze

Windows-Ereignisprotokoll ein **Ereignis** und die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Computer            | Name des Computers, der das Ereignis entnommen wurde. |
| EventCategory       | Kategorie des Ereignisses. |
| EventData           | Alle Daten im raw-Format. |
| EventID             | Nummer des Ereignisses. |
| EventLevel          | Schweregrad des Ereignisses in numerische Form. |
| EventLevelName      | Schweregrad des Ereignisses in Form von Text. |
| EventLog            | Name des Ereignisses, das das Ereignis entnommen wurde. |
| ParameterXml        | Ereignis-Parameterwerte im XML-Format. |
| "Verwaltungsgruppenname" | Name der Verwaltungsgruppe für SCOM-Agenten.  Für andere Agenten ist dies AOI<workspace ID> |
| RenderedDescription | Beschreibung des Ereignisses mit Parameterwerten |
| Quelle              | Die Quelle des Ereignisses. |
| SourceSystem  | Typ des Agenten gesammelten das Ereignis aus. <br> OpsManager-Windows Agent, direkt verbinden oder SCOM <br> Linux-Linux-Agenten  <br> AzureStorage – Azure Diagnostics |
| TimeGenerated       | Datum und Uhrzeit der Erstellung des Ereignisses in Windows. |
| Benutzername            | Der Benutzername des Kontos, das das Ereignis protokolliert. |



## <a name="log-searches-with-windows-events"></a>Suchvorgänge mit Windows-Ereignisse protokollieren

Die folgende Tabelle bietet unterschiedliche Beispiele für Protokoll suchen, die Windows-Ereignisprotokoll abrufen.

| Abfrage | Beschreibung |
|:--|:--|
| Typ = Ereignis | Alle Windows-Ereignisse. |
| Typ = Ereignis EventLevelName = Fehler | Alle Windows-Ereignisse mit Schweregrad des Fehlers. |
| Typ = Ereignis & #124; Measure count() nach Quelle | Anzahl von Windows-Ereignisse nach Quelle. |
| Typ = Ereignis EventLevelName = Fehler & #124; Measure count() nach Quelle | Anzahl von Windows-Fehlerereignisse Quelle. |

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie Protokollanalyse anderen [Datenquellen](log-analytics-data-sources.md) zur Analyse sammeln.
- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Daten aus Datenquellen und Solutions analysieren.  
- Verwenden Sie [Benutzerdefinierte Felder](log-analytics-custom-fields.md) die Ereignisdatensätze in einzelne Felder analysiert.
- [Sammlung von Leistungsindikatoren](log-analytics-data-sources-performance-counters.md) von Windows-Agenten zu konfigurieren.