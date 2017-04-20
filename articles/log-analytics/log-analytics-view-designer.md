<properties
    pageTitle="Analytics Ansicht-Designer anmelden | Microsoft Azure"
    description="Ansicht-Designer im Protokollanalyse können Sie benutzerdefinierte Ansichten erstellen, in der OMS-Konsole, die verschiedene Visualisierung von Daten in die OMS-Repository enthalten. Dieser Artikel enthält eine Übersicht über Ansicht-Designer und Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer"></a>Log Analytics Ansicht-Designer
Ansicht-Designer im Protokollanalyse können Sie benutzerdefinierte Ansichten erstellen, in der OMS-Konsole, die verschiedene Visualisierung von Daten in die OMS-Repository enthalten. Dieser Artikel enthält eine Übersicht über Ansicht-Designer und Verfahren zum Erstellen und Bearbeiten von benutzerdefinierten Ansichten.

Andere Artikel für Ansicht-Designer verfügbar sind:

- [Kachel Referenz](log-analytics-view-designer-tiles.md) - Verweis Einstellungen für alle Kacheln in benutzerdefinierten Ansichten verwendet. 
- [Visualisierung Teilereferenz](log-analytics-view-designer-parts.md) - Verweis Einstellungen für alle Kacheln in benutzerdefinierten Ansichten verwendet. 


## <a name="concepts"></a>Konzepte
Mit dem Ansicht-Designer erstellte Ansichten enthalten die Elemente in der folgenden Tabelle.

| Teil | Beschreibung |
|:--|:--|
| Nebeneinander | Schaltpult Protokoll Analytics Übersicht angezeigt.  Enthält eine visuelle Zusammenfassung Informationen in die benutzerdefinierte Ansicht.  Verschiedene Kachel bieten unterschiedliche Datensätze im Repository OMS-Visualisierung.  Klicken Sie auf die Kachel die benutzerdefinierte Ansicht öffnen. |
| Benutzerdefinierte Ansicht | Klickt der Benutzer auf der Kachel angezeigt.  Enthält ein oder mehrere Teile Visualisierung. |
| Visualisierung Teile | Visualisierung der Daten im Repository OMS basierend auf [Protokoll suchen](log-analytics-log-searches.md).  Die meisten Teile enthält einen Header mit einer Visualisierung auf hoher Ebene sowie eine Liste der Top-Ergebnisse.  Anderen Typen können unterschiedliche Datensätze im Repository OMS-Visualisierung.  Klicken Sie auf Elemente in der Protokolldatei mit detaillierten Datensätze suchen. |

![Übersicht über Designer](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Ansicht-Designer im Arbeitsbereich hinzufügen
Ansicht-Designer in der Vorschau ist, müssen Sie es im Arbeitsbereich **Vorschaufeatures** in **den Einstellungen der OMS-Portal** auswählen hinzufügen.

![Vorschaumodus aktivieren](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Erstellen und Bearbeiten von Ansichten

### <a name="create-a-new-view"></a>Erstellen einer neuen Ansicht
Öffnet eine neue Ansicht im **Ansicht-Designer** auf der Kachel Ansicht-Designer im Hauptmenü OMS-Dashboard.

![Designer nebeneinander anzeigen](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Eine vorhandene Ansicht bearbeiten
Bearbeiten Sie eine vorhandene Ansicht im Ansicht-Designer öffnen der Ansicht auf der Kachel im Hauptmenü OMS-Dashboard.  Klicken Sie dann auf die Schaltfläche **Bearbeiten** , um die Ansicht im Ansicht-Designer zu öffnen.

![Bearbeiten einer Ansicht](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klonen einer vorhandenen Ansicht
Beim Klonen einer Ansicht erstellt eine neue Ansicht und Ansicht-Designer geöffnet.  Die neue Ansicht haben den gleichen Namen wie das Original mit "angehängte am Ende kopieren".  Klonen eine Ansicht öffnen Sie die vorhandene Sicht auf die Kachel im Hauptmenü OMS-Dashboard.  Klicken Sie dann auf **Clone** -Schaltfläche, um die Ansicht im Ansicht-Designer zu öffnen.

![Klonen einer Ansicht](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Vorhandene Ansicht löschen
Um eine vorhandene Ansicht zu löschen, öffnen Sie die Ansicht auf der Kachel im Hauptmenü OMS-Dashboard.  Klicken Sie auf die Schaltfläche **Bearbeiten** , um die Ansicht im Ansicht-Designer zu öffnen, und klicken Sie auf **Ansicht löschen**.

![Löschen einer Ansicht](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Exportieren einer vorhandenen Ansicht
Sie können eine Ansicht JSON-Datei exportieren, die in einem anderen Arbeitsbereich importieren oder in einer [Vorlage Azure-Ressourcen-Manager](../resource-group-authoring-templates.md)verwenden.  Um eine vorhandene Ansicht zu exportieren, öffnen Sie auf der Kachel im Hauptmenü OMS-Dashboard.  Klicken Sie dann auf die Schaltfläche **Exportieren** , um eine Datei in den Browser Download-Ordner erstellen.  Der Name der Datei werden der Name der Ansicht mit der Erweiterung *Omsview*.

![Exportieren einer Ansicht](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Importieren einer vorhandenen Ansicht
Sie können eine *Omsview* -Datei, die Sie von einer anderen Verwaltungsgruppe exportiert.  Zum Importieren einer vorhandenen Ansicht müssen Sie zunächst erstellen Sie eine neue Ansicht.  Klicken Sie auf " **Importieren** " und wählen Sie die Datei *Omsview* .  Die Konfiguration in der Datei werden in die vorhandene Ansicht kopiert.

![Exportieren einer Ansicht](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Arbeiten mit dem Ansicht-Designer
Ansicht-Designer besteht aus drei Bereichen.  Im **Entwurfsbereich** stellt die benutzerdefinierte Ansicht.  Wenn Sie Kacheln und Teile im Bereich **Steuerelement** im **Entwurfsbereich** der Ansicht hinzugefügt werden hinzufügen.  **Eigenschaftenbereich** werden die Eigenschaften für die Kachel oder ausgewählte Teil angezeigt.

![Ansicht-Designer](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurieren der Ansicht nebeneinander
Eine benutzerdefinierte Ansicht können nur ein Element.  Registerkarte **nebeneinander** im **Steuerelement** auf das aktuelle Element oder wählen einen anderen aus.  **Eigenschaftenbereich** werden die Eigenschaften für das aktuelle Fenster angezeigt.  Konfigurieren Sie die Kachel Eigenschaften detaillierte Angaben [Nebeneinander Verweis](log-analytics-view-designer-tiles.md) , und klicken Sie auf **anwenden** , um zu speichern.

### <a name="configure-visualization-parts"></a>Visualisierung Teile konfigurieren
Eine Ansicht kann eine beliebige Anzahl Visualisierung Teile enthalten.  Wählen Sie die Registerkarte **Ansicht** und Visualisierung Teil der Ansicht hinzu.  **Eigenschaftenbereich** werden die Eigenschaften für das ausgewählte Teil angezeigt.  Konfigurieren Sie die Eigenschaften anzeigen detaillierte Angaben [Teilereferenz Visualisierung](log-analytics-view-designer-parts.md) und auf **Übernehmen** speichern.

### <a name="delete-a-visualization-part"></a>Löschen einer Visualisierung
Durch Klicken auf die Schaltfläche **X** oben rechts Teil können Sie Visualisierung Teil aus der Ansicht entfernen.

### <a name="rearrange-visualization-parts"></a>Visualisierung Teile neu anordnen
Ansichten haben nur eine Zeile Visualisierung teilen.  Ändern Sie vorhandene Teile in einer Ansicht durch Klicken und ziehen sie an eine neue Position.


## <a name="next-steps"></a>Nächste Schritte

- Die benutzerdefinierte Ansicht [Kacheln](log-analytics-view-designer-tiles.md) hinzufügen.
- Die benutzerdefinierte Ansicht [Visualisierung Teile](log-analytics-view-designer-parts.md) hinzufügen.
