<properties 
    pageTitle="Überwachen und Verwalten von Azure Data Factory Rohrleitungen" 
    description="Enthält Informationen zum Verwenden von Überwachung und Management App überwachen und Verwalten von Azure Data Factorys und Rohrleitungen." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="spelluru"/>

# <a name="monitor-and-manage-azure-data-factory-pipelines-using-new-monitoring-and-management-app"></a>Überwachen und Verwalten von Azure Data Factory Rohrleitungen neue Überwachung und Anwendung
> [AZURE.SELECTOR]
- [Mithilfe von Azure Portal-Azure PowerShell](data-factory-monitor-manage-pipelines.md)
- [Überwachung und Management-Anwendung](data-factory-monitor-manage-app.md)

Dieser Artikel beschreibt zu überwachen, verwalten und Debuggen Pipelines, Alarme zu Fehlern mit **Überwachung und Anwendung**benachrichtigt. Sie können auch die folgenden lernen zur Überwachung und Verwaltung App Video.
   

> [AZURE.VIDEO azure-data-factory-monitoring-and-managing-big-data-piplines]
      
## <a name="launching-the-monitoring-and-management-app-a"></a>Starten die Überwachung und Verwaltung der Anwendung ein
Um den Monitor und die Anwendung zu starten, klicken Sie auf **Überwachen und verwalten** nebeneinander auf Blatt **DATA FACTORY** für die Daten-Factory.

![Überwachen der Kachel auf Data Factory](./media/data-factory-monitor-manage-app/MonitoringAppTile.png) 

Sie sollten sehen, Überwachung und Management-Anwendung in einem separaten Registerkarte-Fenster gestartet.  

![Überwachung und Management-Anwendung](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [AZURE.NOTE] Wenn Sie sehen, dass der Webbrowser "Autorisieren..." fest, deaktivieren Sie **Drittanbieter - Cookies blockieren und Sitedaten** Einstellung deaktivieren bzw. (oder) behalten Sie aktiviert bei erstellen Sie eine Ausnahme für **login.microsoftonline.com** und wiederholen Sie die Anwendung erneut starten.


Wenn Windows Aktivität in der Liste unten nicht angezeigt wird, klicken Sie auf die Schaltfläche **Aktualisieren** auf der Symbolleiste, um die Liste zu aktualisieren. Darüber hinaus legen Sie die richtigen Werte für die Filter **Startzeit** und **Endzeit** .  


## <a name="understanding-the-monitoring-and-management-app"></a>Grundlagen der Überwachung und Verwaltung der Anwendung
Drei Registerkarten stehen (**Resource Explorer** **Ansichten Überwachung**und **Alarme**) auf der linken Seite und die erste Registerkarte (Resource Explorer) ist standardmäßig aktiviert. 

### <a name="resource-explorer"></a>Ressourcen-Explorer
Angezeigt: 

- Resource Explorer- **Strukturansicht** im linken Bereich.
- **In der Diagrammansicht** oben.
- **Aktivität** Fensterliste unten im mittleren Bereich.
- **Eigenschaften**/**Aktivität Fenster Explorer** -Registerkarten im rechten Bereich. 

Im Ressourcen-Explorer alle Ressourcen (Rohrleitungen, Datasets verknüpfte Dienste) im Werk Daten in einer Strukturansicht angezeigt. Beim Auswählen eines Objekts im Ressourcen-Explorer Beachten Sie Folgendes: 

- zugeordnete Daten Factory Entität wird in der Diagrammansicht hervorgehoben.
- zugeordnete Aktivität Windows (klicken Sie auf [hier](data-factory-scheduling-and-execution.md) Aktivität Windows Informationen) werden in der Liste Aktivität unten hervorgehoben.  
- Eigenschaften des ausgewählten Objekts im Eigenschaftenfenster im rechten Fensterbereich. 
- JSON-Definition des ausgewählten Objekts, falls zutreffend. Beispiel: ein verknüpftes oder ein Dataset oder eine Rohrleitung. 

![Ressourcen-Explorer](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Siehe [Planung und Ausführung](data-factory-scheduling-and-execution.md) Weitere ausführliche grundlegende Informationen zur aktivitätenfenster. 

### <a name="diagram-view"></a>Diagramm anzeigen
Die Diagrammansicht Data Factory bietet einen zentralen Konsole überwachen und Verwalten der Data Factory und ihre Ressourcen. Bei Auswahl einer Entity Data Factory (Dataset/Pipeline) in der Diagrammansicht Beachten Sie Folgendes:
 
- Data Factory Entität wird in der Strukturansicht ausgewählt.
- zugeordnete Aktivität Windows werden in der Liste Aktivität hervorgehoben.
- Eigenschaften des ausgewählten Objekts im Eigenschaftenfenster

Wenn die Pipeline (nicht im angehaltenen Zustand) aktiviert ist, wird sie mit einer grünen Linie angezeigt. 

![Pipeline ausgeführt](./media/data-factory-monitor-manage-app/PipelineRunning.png)

Sie bemerken, dass es drei Schaltflächen für die Pipeline in der Diagrammansicht. Die zweite Schaltfläche können Sie die Pipeline anhalten. Anhalten nicht beendet die laufenden Aktivitäten und lassen Sie sie abschließen. Die dritte Schaltfläche hält die Pipeline und die vorhandenen Aktivitäten beendet. Erste Schaltfläche wird die Pipeline. Wenn die Pipeline angehalten wird, bemerken Sie Farbwechsel für die Pipeline folgendermaßen anordnen.

![Kachel anhalten/fortsetzen](./media/data-factory-monitor-manage-app/SuspendResumeOnTile.png)

Sie können Mehrfachauswahl mindestens zwei Rohrleitungen (STRG) und Befehlsleisten-Schaltflächen zum Anhalten/Fortsetzen von mehrere Pipelines gleichzeitig verwenden.

![Auf anhalten/fortsetzen](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

Sie können die Aktivitäten in der Pipeline anzeigen auf der Kachel Pipeline und auf **offene pipeline**

![Pipeline-Menü](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

In der Pipelineansicht geöffneten finden Sie unter alle Aktivitäten in der Pipeline. In diesem Beispiel gibt es nur eine Aktivität: Kopieraktivität. Um zur vorherigen Ansicht zurückzukehren, klicken Sie auf Namen in der Breadcrumb-Menü oben Daten.

![Geöffnete Pipeline](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

In der Ansicht Wenn Sie auf ein ausgabedataset oder beim Bewegen der Maus über das ausgabedataset anzeigen Aktivität Windows Popup für dataset

![Aktivität Windows popup](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

Klicken Sie auf eine aktivitätenfenster Details in **das Eigenschaftenfenster im rechten Fensterbereich** angezeigt. 

![Fenstereigenschaften](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

Rechten wechseln Sie zur Registerkarte **Aktivität Fenster Explorer** , um weitere Details anzuzeigen.

![Aktivität Windows Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png) 

Sie sehen auch für alle Aktivitäten im Abschnitt **Versuche** versuchen **Variablen aufgelöst** . 

![Aufgelöste Variablen](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Wechseln Sie zur Registerkarte **Skript** JSON Skriptdefinition für das ausgewählte Objekt an.   

![Skriptregisterkarte](./media/data-factory-monitor-manage-app/ScriptTab.png)

Aktivität Windows an drei Stellen sehen:

- Aktivität Windows Popup-Fenster in der Diagrammansicht (mittlerer Bereich).
- Aktivität Fenster Explorer im rechten Fensterbereich.
- Windows-Aktivitätenliste im unteren Bereich.

Aktivität Windows Popup und Aktivität Windows Explorer können Sie auf vorherige und nächste Woche mit Pfeilen nach links und rechts blättern.

![Aktivität Fenster Explorer links/rechts-Pfeile](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Am unteren Rand der Ansicht finden Sie unter Schaltflächen zum Vergrößern, verkleinern, Zoom anpassen, 100 % zoomen, Layout zu sperren. Schaltfläche Layout Sperren verhindert ein versehentliches Verschieben von Tabellen und Rohrleitungen in der Diagrammansicht und ist standardmäßig ON. Elemente im Diagramm verschieben und es deaktivieren können. Wenn sie erneut können die letzte Schaltfläche Sie Tabellen und Rohrleitungen automatisch positionieren. Sie können auch vergrößern / verkleinern mit Mausrad.

![Diagramm-Ansicht Zoom-Befehle](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)


### <a name="activity-windows-list"></a>Windows Aktivitätenliste
Die Aktivität Fensterliste im unteren mittleren Bereich zeigt alle Aktivitäten Fenster für das Dataset im Ressourcen-Explorer oder Diagramm ausgewählten. Die Liste ist in absteigender Reihenfolge, d.h. das neueste aktivitätenfenster oben angezeigt. 

![Windows Aktivitätenliste](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Diese Liste nicht automatisch aktualisiert, so verwenden Sie die Aktualisierungsschaltfläche auf der Symbolleiste manuell aktualisieren.  


Aktivität-Windows können einen der folgenden Status aufweisen:

<table>
<tr>
    <th align="left">Status</th><th align="left">Untergeordneter</th><th align="left">Beschreibung</th>
</tr>
<tr>
    <td rowspan="8">Warten</td><td>ScheduleTime</td><td>Die Zeit gekommen nicht das Fenster Aktivität ausführen.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Vorgelagerten Dependencies sind nicht bereit.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Die Serverressourcen sind nicht verfügbar.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Alle Aktivitätsinstanzen sind andere Aktivität Windows ausgeführt.</td>
</tr>
<tr>
<td>ActivityResume</td><td>Aktivität wird angehalten und Windows Aktivität kann nicht ausgeführt werden, bis er fortgesetzt wird.</td>
</tr>
<tr>
<td>Wiederholen</td><td>Ausführung der Aktivitäten wird wiederholt.</td>
</tr>
<tr>
<td>Validierung</td><td>Überprüfung wurde noch nicht gestartet.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Warten auf Prüfung wiederholt werden.</td>
</tr>
<tr>
<tr
<td rowspan="2">In Bearbeitung</td><td>Überprüfen</td><td>Validierung in Bearbeitung.</td>
</tr>
<td></td>
<td>Im aktivitätenfenster wird verarbeitet.</td>
</tr>
<tr>
<td rowspan="4">Fehler</td><td>TimedOut</td><td>Ausführung hat länger gedauert als, Aktivität zulässig ist.</td>
</tr>
<tr>
<td>Abgebrochen</td><td>Durch den Benutzer abgebrochen.</td>
</tr>
<tr>
<td>Validierung</td><td>Fehler beim Überprüfen.</td>
</tr>
<tr>
<td></td><td>Fehler beim Erstellen oder das aktivitätenfenster überprüfen.</td>
</tr>
<td>Bereit</td><td></td><td>Im aktivitätenfenster ist für die Verwendung.</td>
</tr>
<tr>
<td>Übersprungen</td><td></td><td>Im aktivitätenfenster wird nicht verarbeitet.</td>
</tr>
<tr>
<td>Keine</td><td></td><td>Ein aktivitätenfenster, die mit einem anderen Status verwendet wurde zurückgesetzt.</td>
</tr>
</table>


Beim Klicken auf eine aktivitätenfenster Details **Aktivität Windows Explorer** oder **Eigenschaften** im Fenster rechts angezeigt.

![Aktivität Windows Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Aktivität Windows aktualisieren  
Die Details werden nicht automatisch aktualisiert, damit **Aktualisieren** verwenden button (zweiten) auf der Befehlsleiste auf die Aktivitätsliste Windows manuell aktualisieren.  
 

### <a name="properties-window"></a>Eigenschaftenfenster
Das Fenster Eigenschaften wird im rechten Bereich App Überwachung und Verwaltung. 

![Eigenschaftenfenster](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Eigenschaften für im Ressourcen-Explorer (Verzeichnisansicht) (oder) Diagramm anzeigen (oder) Windows Aktivitätsliste ausgewählten Elements angezeigt. 

### <a name="activity-window-explorer"></a>Aktivität Windows Explorer

**Aktivität Windows Explorer** -Fenster wird im rechten Bereich Überwachung und Anwendung. Details im aktivitätenfenster in der Aktivität Windows Popup oder Aktivität Windows gewählten angezeigt. 

![Aktivität Windows Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

In der Kalenderansicht oben auf können Sie eine andere Aktivität Fenster wechseln. Sie können auch **Links**/**rechts** Schaltflächen Aktivität Windows die vorherige/nächste Woche anzeigen.

Den Symbolleisten-Schaltflächen im unteren aktivitätenfenster **erneut** oder **Aktualisieren** können die Details im Bereich. 

### <a name="script"></a>Skript 
Die Registerkarte **Skript** können Sie die JSON-Definition der ausgewählten Daten Factory Entität (verknüpften Serviceartikel Dataset und Pipeline) anzeigen. 

![Skriptregisterkarte](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="using-system-views"></a>Mithilfe von Ansichten
Überwachung und Management App enthält vordefinierte Ansichten (**aktuelle Aktivität Windows**, **Fehler Aktivität Windows** **Windows Aktivität In Bearbeitung**), die Windows für Daten Werk/Fehler/in-Fortschritte Aktivität angezeigt werden. 

Wechseln Sie zur Registerkarte **Überwachung Ansichten** links durch Anklicken. 

![Registerkarte Überwachung](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Zurzeit stehen drei Ansichten unterstützt. Aktuelle Aktivität Windows (oder) fehlerhafte Aktivität Windows (oder) Windows laufende Aktivität in der Aktivität (unten im mittleren Bereich) wählen. 

Bei Auswahl der Option **aktuelle Aktivität Windows** alle aktuellen Aktivitäten Windows in absteigender Reihenfolge der **letzten Versuch Zeit**angezeigt. 

**Fehler Aktivität Windows** -Ansicht können Sie alle fehlgeschlagenen Aktivität Fenster in der Liste angezeigt. Wählen Sie eine fehlerhafte aktivitätenfenster in der Liste Details im **Eigenschaften** -Fenster (oder) **Aktivität Windows Explorer aus** Sie können auch Protokolle für fehlgeschlagene Aktivitätsfenster. 


## <a name="sorting-and-filtering-activity-windows"></a>Sortieren und Filtern von Aktivität windows
Ändern der **Startzeit** und **Endzeit** in der Befehlsleiste an Aktivität-Fenster. Nach dem Ändern von Start- und Endzeit klicken Sie neben der Endzeit die Aktivität Liste aktualisieren.

![Start- und Endzeiten](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [AZURE.NOTE] Im UTC-Format in die Überwachung und Anwendung sind immer aktuell. 

In der **Fensterliste Aktivität**klicken Sie auf den Namen einer Spalte (z. B.: Status). 

![Aktivität Windows-spaltenmenü](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

Sie können Folgendes:

- Sortieren in aufsteigender Reihenfolge.
- Sortieren in absteigender Reihenfolge.
- Filtern Sie nach einem oder mehreren Werten (bereit, wartet usw..)

Wenn Sie einen Filter für eine Spalte angeben, sehen Sie die Filterschaltfläche für die Spalte an, dass die Werte in der Spalte Gefilterte Werte sind aktiviert. 

![In Spalte Aktivität Windows Liste filtern](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

Das gleiche Popupfenster können Filter deaktivieren. Zum Löschen aller Filter für die Aktivitätsliste Windows klicken Sie Filter löschen auf der Befehlsleiste. 

![Löschen Sie aller Filter in Aktivität Fensterliste](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)


## <a name="performing-batch-actions"></a>Batch-Aktionen

### <a name="rerun-selected-activity-windows"></a>Ausgewählte Aktivität Windows erneut
Wählen Sie eine aktivitätenfenster, klicken Sie auf den Pfeil nach unten für die erste Befehlsschaltfläche Bar und **erneut** / **mit upstream in Pipeline ausführen**. Wählen Sie Option **mit upstream in Pipeline ausführen** führt alle übergeordneten Aktivität Windows erneut. 
    ![Ein aktivitätenfenster erneut](./media/data-factory-monitor-manage-app/ReRunSlice.png)

Außerdem können Sie Windows mehrere Aktivitäten in der Liste auswählen und gleichzeitig erneut. Aktivität Fenster je nach Status filtern möchten (zum Beispiel: **Fehler**) und starten Sie Windows fehlerhafte Aktivität nach dem Problem, bei dem die Aktivität Windows nicht, korrigieren. Siehe folgende Abschnitte für Details zur Aktivität Fenster in der Liste filtern.  

### <a name="pauseresume-multiple-pipelines"></a>Mehrere Pipelines anhalten/fortsetzen
Sie können Mehrfachauswahl mehrere Rohrleitungen (STRG) und Anhalten/sie gleichzeitig fortsetzen Befehlsleistenschaltflächen (rotes Rechteck in der folgenden Abbildung hervorgehoben) mit.

![Auf Unterbrechung/Wiederaufnahme](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="creating-alerts"></a>Erstellen von alerts 
Die Seite Alerts können Sie eine Warnung anzeigen/bearbeiten/löschen bestehende Alerts erstellen. Sie können auch aktivieren/deaktivieren einer Warnung. Klicken Sie die Seite ALerts Registerkarte Alarme.

![Registerkarte Alerts](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Erstellen eine Warnung

1. Klicken Sie auf **Warnung hinzufügen** eine Warnung hinzufügen. Die Seite Details angezeigt. 

    ![Alerts - Seite erstellen](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
1. Geben Sie den **Namen** und die **Beschreibung** der Warnung, und klicken Sie auf **Weiter**. Sie sollten die **Filter** angezeigt.

    ![Alerts - Seite Filter erstellen](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)

2. Wählen Sie **Ereignis**, **Status**und **untergeordneter** (optional) auf dem Data Factory Service Sie benachrichtigen soll, und klicken Sie auf **Weiter**. Sie sollten die **Empfänger** angezeigt.

    ![Alerts - Empfänger-Seite erstellen](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png) 
3. **E-Mail-Abonnement-Admins** die Option oder geben Sie **zusätzliche Administrator e-Mail**und klicken Sie auf **Fertig stellen**. Die Warnung in der Liste sollte angezeigt werden. 
    
    ![Alarmliste](./media/data-factory-monitor-manage-app/AlertsList.png)

In der Liste Warnungen mithilfe der Schaltflächen die Warnung bearbeiten/löschen/aktivieren/deaktivieren einer Warnung zugeordnet. 

### <a name="eventstatussubstatus"></a>Status/Ereignis/untergeordneter
Die folgende Tabelle enthält eine Liste der verfügbaren Ereignisse und Status (und Unterstatus).

Ereignisname | Status | Sub-status
-------------- | ------ | ----------
Aktivität ausführen gestartet | Gestartet | Starten
Aktivität fertig ausgeführt | Erfolgreich | Erfolgreich 
Aktivität fertig ausgeführt | Fehler| Fehlerhafte Ressource: Zuteilung<br/><br/>Fehlerhafte Ausführung<br/><br/>Zeitlimit<br/><br/>Fehlgeschlagenen Validierung<br/><br/>Abgebrochen
Erstellen Sie bei Bedarf HDI Cluster gestartet | Gestartet | &nbsp; |
Bei Bedarf HDI Cluster wurde erfolgreich erstellt. | Erfolgreich | &nbsp; |
Bei Bedarf HDI Cluster gelöscht | Erfolgreich | &nbsp; |
### <a name="to-editdeletedisable-an-alert"></a>Bearbeiten/Löschen/Deaktivieren einer Warnung


![Alerts-Schaltflächen](./media/data-factory-monitor-manage-app/AlertButtons.png)



    
 


