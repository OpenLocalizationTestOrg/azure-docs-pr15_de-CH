<properties
    pageTitle="Versionsvergleich Lösung Protokollanalyse | Microsoft Azure"
    description="Können Sie die Konfiguration Change Tracking-Lösung in Protokollanalyse können Sie leicht erkennen, Software und Netzwerkdiensten Änderungen in Ihrer Umgebung – identifizieren diese Konfiguration geändert wird, können Sie Probleme ermitteln."
    services="operations-management-suite"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operations-management-suite"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="change-tracking-solution-in-log-analytics"></a>Tracking-Lösung Protokollanalyse ändern


Können Sie die Konfiguration Change Tracking-Lösung in Protokollanalyse können Sie leicht erkennen, Software und Windows-Dienste geändert und Linux-Daemon Änderungen in Ihrer Umgebung – identifizieren diese Konfiguration geändert wird, können Sie Probleme ermitteln.

Sie installieren die Lösung, um den Typ des Agents aktualisieren, die Sie installiert haben. Änderungen Software installiert und Windows-Dienste auf den überwachten Servern lesen und die Daten werden an den Protokollanalyse-Dienst in der Cloud zur Verarbeitung gesendet. Logik wird angewendet, um die empfangenen Daten und Cloud-Dienst die Daten. Wenn Unterschiede festgestellt, werden Server mit im Änderungsprotokoll Dashboard angezeigt. Mithilfe der Informationen im Schaltpult Change Tracking sehen Sie einfach die Änderungen, die in Ihrer Serverinfrastruktur erstellt wurden.

## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung
Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Operations Manager ist für das Änderungsprotokoll Lösung erforderlich.
- Windows oder Operations Manager-Agent benötigen auf jedem Computer Sie Änderungen überwacht werden soll.
- Hinzufügen der Lösung Change Tracking OMS Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md).  Es ist keine weitere Konfiguration erforderlich.


## <a name="change-tracking-data-collection-details"></a>Einzelheiten zur Datensammlung Überwachung ändern

Versionsvergleich sammelt Softwareinventur und Windows Service Metadaten mit Agenten, die Sie aktiviert haben.

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für das Änderungsprotokoll.

| Plattform | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Nein](./media/log-analytics-change-tracking/oms-bullet-red.png)|            ![Nein](./media/log-analytics-change-tracking/oms-bullet-red.png)|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)| stündlich|

## <a name="use-change-tracking"></a>Änderungsprotokoll verwenden

Nach der Installation der Lösung können Sie Summary Änderungen für die überwachten Server mit **Change Tracking** nebeneinander auf der Seite **Übersicht** OMS anzeigen.

![Bild nebeneinander Change Tracking](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

Sie können sich Ihre Infrastruktur und Drilldown zu Details für folgende anzeigen:

- Bearbeitung von Konfigurationstyp für Software und Windows-Dienste
- Software wird auf Programme und Updates für einzelne Server
- Anzahl der softwareänderungen für jede Anwendung
- Windows Änderungen für einzelne Server

![Bild des Dashboards Change Tracking](./media/log-analytics-change-tracking/oms-changetracking01.png)

![Bild des Dashboards Change Tracking](./media/log-analytics-change-tracking/oms-changetracking02.png)

### <a name="to-view-changes-for-any-change-type"></a>Anzeigen ändern Änderung für alle

1. Klicken Sie auf der Seite **Übersicht** **Change Tracking** .
2. Lesen Sie auf dem Dashboard **Ändern verfolgen** die Zusammenfassung in einem Blade Typ ändern und dann auf eine detaillierte Informationen auf der Suchseite **Protokoll** anzeigen.
3. Auf Suchseiten Protokoll Ergebnisse Zeit detaillierte und Suchverlauf Protokoll angezeigt. Sie können auch Filtern Facetten um die Ergebnisse einzuschränken.

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Log durchsucht Protokollanalyse](log-analytics-log-searches.md) detaillierte Daten im Änderungsprotokoll anzeigen.
