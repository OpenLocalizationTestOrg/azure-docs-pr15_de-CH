<properties
   pageTitle="Warnung-Lösung in Operations Management Suite (OMS) | Microsoft Azure"
   description="Die Alert-Management-Lösung in Protokollanalyse können Sie alle Warnungen in Ihrer Umgebung zu analysieren.  Neben Konsolidierung in OMS generierte Warnungen, importiert Alarme aus verbundenen System Center Operations Manager (SCOM) Verwaltungsgruppen in Protokollanalyse."
   services="log-analytics"
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
   ms.date="10/06/2016"
   ms.author="bwren" />

# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Warnung-Lösung in Operations Management Suite (OMS)

![Management-Warnsymbol](media/log-analytics-solution-alert-management/icon.png) Alert-Management-Lösung können Sie alle Warnungen in Ihrer Umgebung zu analysieren.  Neben Konsolidierung in OMS generierte Warnungen, importiert Alarme aus verbundenen System Center Operations Manager (SCOM) Verwaltungsgruppen in Protokollanalyse.  In einer Umgebung mit mehreren Verwaltungsgruppen wird die Alert-Management eine konsolidierte Ansicht der Alarme aus allen Management Lösung.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Zum Importieren von Alerts von SCOM erfordert diese Lösung eine Verbindung zwischen den OMS-Arbeitsbereich und einer SCOM Verwaltungsgruppe mithilfe der Schritte in [Protokollanalyse Operations Manager verbinden](log-analytics-om-agents.md).  


## <a name="configuration"></a>Konfiguration

Arbeitsbereich OMS mit [Add](log-analytics-add-solutions.md)Solutions beschriebenen Prozess fügen Sie Alert-Management-Lösung hinzu.  Es ist keine weitere Konfiguration erforderlich.

## <a name="management-packs"></a>Managementpacks

Wenn die Verwaltungsgruppe SCOM OMS Arbeitsbereich verbunden ist, werden dann die folgenden managementpacks in SCOM installiert, wenn diese Lösung hinzufügen.  Es ist keine Konfiguration oder Wartung dieser Management Packs erforderlich.  

- Alert-Management für Microsoft System Center Advisor (Microsoft.IntelligencePacks.AlertManagement)

Weitere Hinweise, wie Lösung managementpacks aktualisiert werden finden Sie unter [Protokollanalyse Operations Manager herstellen](log-analytics-om-agents.md).

## <a name="data-collection"></a>Datensammlung

### <a name="agents"></a>Agenten

Die folgende Tabelle beschreibt die verbundenen Quellen, die diese Lösung unterstützt.

| Verbundene Datenquelle | Unterstützung | Beschreibung |
|:--|:--|:--|
| [Windows-Agenten](log-analytics-windows-agents.md) | Nein | Direkte Windows-Agenten SCOM Alerts generieren. |
| [Linux-Agenten](log-analytics-linux-agents.md) | Nein | Direkte Linux-Agenten SCOM Alerts generieren. |
| [SCOM Verwaltungsgruppe](log-analytics-om-agents.md) | Ja | SCOM-Agenten generierten Warnungen werden an die Verwaltungsgruppe gesendet und dann an Protokollanalyse.<br><br>Eine direkte Verbindung von SCOM-Agenten zum Protokollanalyse ist nicht erforderlich. Daten werden aus der Verwaltungsgruppe OMS-Repository weitergeleitet. |
| [Azure Storage-Konto](log-analytics-azure-storage.md) | Nein | SCOM Alerts sind nicht in Azure Storage-Konten gespeichert. |

### <a name="collection-frequency"></a>Häufigkeit der Collection

In OMS generierte Warnungen sind sofort zur Lösung.  Daten werden aus der Verwaltungsgruppe SCOM an Protokollanalyse 3 Minuten gesendet.  

## <a name="using-the-solution"></a>Die Lösung

Arbeitsbereich OMS Alert-Management-Lösung hinzugefügt, wird Ihr Dashboard OMS Kachel **Alert-Management** hinzugefügt.  Diese Kachel zeigt die Anzahl und grafisch die Anzahl der aktiven Alarme, die innerhalb der letzten 24 Stunden generiert wurden.  Dieser Zeitraum kann nicht geändert werden.

![Alert Management-Kachel](media/log-analytics-solution-alert-management/tile.png)

Klicken Sie auf die **Alert-Management** Kachel **Alert-Management** Dashboard zu öffnen.  Das Schaltpult enthält die Spalten in der folgenden Tabelle.  Jede Spalte listet die Top zehn Alarme nach Anzahl der Spalte Kriterien für den angegebenen Bereich und Zeitraum.  Sie können ein Protokoll suchen ausführen, die gesamte Liste durch Klicken auf **Alle** am unteren Rand der Spalte oder durch Klicken auf die Spaltenüberschrift.

| Spalte| Beschreibung |
|:--|:--|
| Alarme | Alle Warnungen mit dem Schweregrad Kritisch Warnungsname gruppiert.  Klicken Sie auf eine Warnung eine Protokolldatei Suche alle Datensätze für den Alarm zurück. |
| Warnungen | Alle Warnungen mit dem Schweregrad Warnung Namen gruppiert.  Klicken Sie auf eine Warnung eine Protokolldatei Suche alle Datensätze für den Alarm zurück. |
| Aktive SCOM Alerts | Alle SCOM Alerts mit einem Mitgliedstaat anderen als *geschlossen* gruppiert nach Quelle die Warnung ausgelöst wurde. |
| Alle aktiven Alarme | Alle Warnungen sämtliche Schweregrade Warnungsname gruppiert. Enthält nur SCOM Alarmen mit einem anderen Staat als *geschlossen*.|

Wenn Bildlauf nach rechts Listet das Dashboard mehrere allgemeine Abfragen Sie auf klicken, um eine [Protokolldatei suchen](log-analytics-log-searches.md) für Daten ausführen.

![Alert Management-dashboard](media/log-analytics-solution-alert-management/dashboard.png)

## <a name="scope-and-time-range"></a>Umfang und Zeitraum

Bereich der Alarme analysiert die Alert-Management-Lösung ist in allen verbundenen Verwaltungsgruppen, die innerhalb der letzten 7 Tage.  

![Alert Management-Bereich](media/log-analytics-solution-alert-management/scope.png)

- Klicken Sie in der Analyse enthaltenen Verwaltungsgruppen **Bereich** oben im Schaltpult.  Sie können entweder **Global** für alle verbundenen Verwaltungsgruppen oder **Verwaltungsgruppe von** einer einzelnen Verwaltungsgruppe auswählen auswählen.

- Ändern Sie den Zeitraum der Alarme wählen Sie oben das Dashboard **basierend auf** .  Sie können innerhalb der letzten 7 Tage, 1 Tag oder 6 Stunden generierte Warnungen auswählen.  Oder wählen Sie **Benutzerdefiniert** und geben Sie einen benutzerdefinierten Datumsbereich.

## <a name="log-analytics-records"></a>Analytics-Datensätze

Alert-Management-Lösung analysiert alle Datensätze mit einer **Warnung**.  Außerdem wird Alerts von SCOM importieren und erstellt einen entsprechenden Datensatz für jede mit einer **Warnung** und SourceSystem **OpsManager**.  Diese Einträge haben die Eigenschaften in der folgenden Tabelle.  

| Eigenschaft | Beschreibung |
|:--|:--|
| Typ | *Warnung* |
| SourceSystem | *OpsManager* |
| AlertContext | Details des Datenelements, der im XML-Format generiert die Warnung ausgelöst hat. |
| AlertDescription | Beschreibung der Warnung. |
| AlertId | GUID der Warnung. |
| RepeatCount | Name der Warnung. |
| AlertPriority | Priorität der Warnung an. |
| AlertSeverity | Der Schweregrad der Warnung. |
| AlertState | Aktuellen Auflösungsstatus der Warnung. |
| LastModifiedBy | Name des Benutzers, der die Warnung zuletzt geändert. |
| "Verwaltungsgruppenname" | Name der Verwaltungsgruppe, in dem die Warnung generiert wurde. |
| RepeatCount | Anzahl der erstellten dieselbe Warnung für dasselbe Objekt seit aufgelöst wird überwacht. |
| ResolvedBy | Name des Benutzers, der die Warnung aufgelöst hat. Leer, wenn die Warnung noch nicht aufgelöst wurde. |
| SourceDisplayName | Anzeigename des Überwachungsobjekt die Warnung ausgelöst wurde. |
| SourceFullName | Vollständiger Name des Überwachungsobjekt die Warnung ausgelöst wurde. |
| TicketId | Ticket-ID für die Warnung, wenn ein Prozess zum Zuweisen von Tickets für Alarme SCOM-Umgebung integriert ist.  Leer kein Ticket-ID zugewiesen. |
| TimeGenerated | Datum und Uhrzeit der Erstellung die Benachrichtigung. |
| TimeLastModified | Datum und Uhrzeit der letzten der Warnung Änderung. |
| TimeRaised | Datum und Uhrzeit, zu der die Warnung generiert wurde. |
| TimeResolved | Datum und Uhrzeit der Auflösung der Warnung. Leer, wenn die Warnung noch nicht aufgelöst wurde. |

## <a name="sample-log-searches"></a>Beispiel-Protokoll suchen

Die folgende Tabelle enthält Beispiel Protokoll sucht Warnungsdatensätze Lösung gesammelt.  

| Abfrage | Beschreibung |
|:--|:--|
| Typ = Warnung SourceSystem = OpsManager AlertSeverity = Fehler TimeRaised > jetzt 24 Stunden | Während der letzten 24 Stunden kritische Alarme |
| Typ = Warnung AlertSeverity = Warnung TimeRaised > jetzt 24 Stunden | Warnungen während der letzten 24 Stunden  |
| Typ = Warnung SourceSystem = OpsManager AlertState! = geschlossenen TimeRaised > jetzt 24 Stunden & #124; Anzahl von SourceDisplayName count() messen | Mit aktiven Warnungen während der letzten 24 Stunden |
| Typ = Warnung SourceSystem = OpsManager AlertSeverity = Fehler TimeRaised > jetzt 24 Stunden AlertState! = geschlossen | Während der letzten 24 Stunden aktiv sind Alarme |
| Typ = Warnung SourceSystem = OpsManager TimeRaised > jetzt 24 Stunden AlertState = geschlossen | Während der letzten 24 Stunden die jetzt geschlossen Alerts |
| Typ = Warnung SourceSystem = OpsManager TimeRaised > jetzt 1 Tag & #124; Anzahl von AlertSeverity count() messen | Während den letzten 24 Stunden nach Schweregrad gruppiert Alerts |
| Typ = Warnung SourceSystem = OpsManager TimeRaised > jetzt 1 Tag & #124; RepeatCount Absteigend sortieren | Während der letzten 1 Tag die Wiederholungsanzahl Wert sortiert Alerts |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Alarme in Protokollanalyse](log-analytics-alerts.md) Details zum Generieren von Alerts von Protokollanalyse.
