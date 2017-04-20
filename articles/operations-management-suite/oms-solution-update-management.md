<properties
    pageTitle="Management-Lösung OMS aktualisieren | Microsoft Azure"
    description="Dieser Artikel soll Sie verstehen, wie diese Lösung zum Verwalten von Updates für Windows und Linux-Computer."
    services="operations-management-suite"
    documentationCenter=""
    authors="MGoedtel"
    manager="jwhit"
    editor=""
    />
<tags
    ms.service="operations-management-suite"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="magoedte"/>

# <a name="update-management-solution-in-omsmediaoms-solution-update-managementupdate-management-solution-iconpng-update-management-solution-in-oms"></a>![Lösung für die Verwaltung in OMS](./media/oms-solution-update-management/update-management-solution-icon.png) Lösung für die Verwaltung in OMS

Update Management-Lösung in OMS können Sie Updates für Windows und Linux-Computer verwalten.  Können Sie schnell den Status der verfügbaren Updates auf alle Agentcomputer bewerten und starten die Installation von erforderlichen Updates für Server. 

## <a name="prerequisites"></a>Erforderliche Komponenten

-   Windows-Agenten müssen entweder mit einem Windows Server Update Services (WSUS)-Server kommunizieren oder auf Microsoft Update konfiguriert werden.  

    >[AZURE.NOTE] Der Windows-Agent kann gleichzeitig von System Center Configuration Manager verwaltet werden.  
  
-   Linux-Agenten müssen eine Update-Repository zugreifen.  OMS-Agenten für Linux kann von [GitHub](https://github.com/microsoft/oms-agent-for-linux)heruntergeladen werden. 

## <a name="configuration"></a>Konfiguration

Die folgenden Schritte Update Management-Lösung den OMS-Arbeitsbereich und hinzufügen Linux-Agenten.  Windows-Agenten werden ohne zusätzliche Konfiguration automatisch hinzugefügt.

1.  OMS Arbeitsbereich mithilfe der Schritte im Lösungskatalog [Hinzufügen OMS Solutions](../log-analytics/log-analytics-add-solutions.md) fügen Sie Update Management-Lösung hinzu.  
2.  Wählen Sie im Portal OMS **Einstellungen** und **Datenquellen verbunden**.  Beachten Sie die **Arbeitsplatz-ID** und dem **Primärschlüssel** oder **sekundären Schlüssel**.
3.  Die folgenden Schritte für jeden Linux-Computer.

    ein.  Installieren Sie die neueste Version des OMS-Agenten für Linux durch Ausführen der folgenden Befehle.  Ersetzen Sie <Workspace ID> mit der ID Arbeitsbereich und <Key> mit dem primären oder sekundären Schlüssel.

        cd ~
        wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.2.0-75/omsagent-1.2.0-75.universal.x64.sh
        sudo bash omsagent-1.2.0-75.universal.x64.sh --upgrade -w <Workspace ID> -s <Key>

     b. Führen Sie den folgenden Befehl, um den Agenten zu entfernen.

        sudo bash omsagent-1.2.0-75.universal.x64.sh --purge

## <a name="management-packs"></a>Managementpacks

Wenn die System Center Operations Manager-Verwaltungsgruppe OMS Arbeitsbereich verbunden ist, werden dann die folgenden managementpacks in Operations Manager installiert, wenn diese Projektmappe hinzufügen. Es ist keine Konfiguration oder Wartung dieser Management Packs erforderlich. 

-   Microsoft System Center Advisor Bewertung Intelligence Updatepaket (Microsoft.IntelligencePacks.UpdateAssessment)
-   Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
-   Bereitstellung MP aktualisieren

Weitere Hinweise, wie Lösung managementpacks aktualisiert werden finden Sie unter [Protokollanalyse Operations Manager herstellen](../log-analytics/log-analytics-om-agents.md).

## <a name="data-collection"></a>Datensammlung

### <a name="supported-agents"></a>Unterstützte agents

Die folgende Tabelle beschreibt die verbundenen Quellen, die diese Lösung unterstützt.

Verbundene Datenquelle | Unterstützt | Beschreibung|
----------|----------|----------|
Windows-Agenten | Ja | Die Lösung sammelt Informationen über System-Updates von Windows-Agenten und Installation von erforderlichen Updates initiiert.|
Linux-Agenten | Ja | Die Lösung sammelt Informationen über System-Updates von Linux-Agenten.|
Operations Manager-Verwaltungsgruppe | Ja | Die Lösung sammelt Informationen über System-Updates von Agents in einer Verwaltungsgruppe verbunden.<br>Eine direkte Verbindung von Operations Manager-Agent zu Protokollanalyse ist nicht erforderlich. Daten werden aus der Verwaltungsgruppe OMS-Repository weitergeleitet.|
Azure Storage-Konto | Nein | Azure-Speicher enthält keine Informationen über System-Updates.|  

### <a name="collection-frequency"></a>Häufigkeit der Collection

Für jeden verwalteten Computer ist ein Scan zweimal täglich ausgeführt.  Wenn ein Update installiert ist, werden die Informationen innerhalb von 15 Minuten aktualisiert.  

Für jeden verwalteten Computer Linux wird eine Überprüfung alle 3 Stunden ausgeführt.  

## <a name="using-the-solution"></a>Die Lösung

Arbeitsbereich OMS Update Management-Lösung hinzugefügt, wird Ihr Dashboard OMS Kachel **Verwaltung** hinzugefügt. Diese Kachel zeigt Anzahl und grafisch die Anzahl der Computer in der Umgebung aktuell eine System-Updates.<br><br>
![Update Management Zusammenfassung Kachel](media/oms-solution-update-management/update-management-summary-tile.png)  

## <a name="viewing-update-assessments"></a>Anzeige aktualisieren Assessments

Klicken Sie auf die **Verwaltung** Kachel **Update Management** -Dashboard zu öffnen. Das Schaltpult enthält die Spalten in der folgenden Tabelle. Jede Spalte enthält diese Spalte Kriterien für den angegebenen Bereich und Zeitraum bis zu zehn Elemente. Sie können eine protokollsuche ausführen, die sämtliche Datensätze zurückgibt, indem **Alle** am unteren Rand der Spalte oder klicken auf die Spaltenüberschrift.

Spalte | Beschreibung|
----------|----------|
**Computer fehlen Updates** ||
Wichtige und Sicherheitsupdates | Listet die Anzahl der Updates die Top zehn Computer fehlen updates sortiert sind nicht vorhanden. Klicken Sie auf einen Computernamen Protokoll suchen wieder alle Datensätze für diesen Computer zu aktualisieren.|
Kritische und Sicherheitsupdates, die älter als 30 Tage| Bezeichnet die Anzahl der Computer fehlen wichtige und Sicherheitsupdates gruppiert die Zeitspanne seit der Veröffentlichung des Updates. Klicken Sie auf einen der Einträge Protokoll Suche alle fehlenden und wichtigen Updates wieder.|
**Fehlende Updates erforderlich**||
Wichtige und Sicherheitsupdates | Listet Klassifikationen Updates Computer fehlen die Anzahl der Computer fehlen Updates in der Kategorie sortiert. Klicken Sie auf eine Klassifizierung zum Protokoll suchen wieder alle Datensätze Klassifikation zu aktualisieren.|
**Update-Installationen**||
Update-Installationen | Anzahl der aktuell geplanten Update-Installationen und die Dauer bis zum nächsten geplanten Ausführung.  Klicken Sie auf die Kachel anzeigen, laufenden und abgeschlossenen Updates oder eine neue Bereitstellung planen.|  
<br>  
![Summary Update Management-Dashboard](./media/oms-solution-update-management/update-management-deployment-dashboard.png)<br>  
<br>
![Update Management Dashboard Computer anzeigen](./media/oms-solution-update-management/update-management-assessment-computer-view.png)<br>  
<br>
![Management Dashboard Paketansicht aktualisieren](./media/oms-solution-update-management/update-management-assessment-package-view.png)<br>  

## <a name="installing-updates"></a>Installieren von updates

Nach Updates für alle Windows-Computer in Ihrer Umgebung beurteilt haben, können Sie Updates installiert eine *Bereitstellung*erstellen müssen.  Eine Bereitstellung ist eine geplante Installation von erforderlichen Updates für ein oder mehrere Windows-Computer.  Sie geben das Datum und die Zeit für die Bereitstellung zusätzlich einen Computer oder eine Gruppe von Computern einbezogen werden soll.  

Updates werden von Runbooks in Azure Automation installiert.  Diese Runbooks nicht aktuell angezeigt, und sie brauchen keine Konfigurationen.  Beim Erstellen einer Bereitstellung aktualisieren erstellt es einen Zeitplan, beginnt ein Runbook Mastern zum angegebenen Zeitpunkt für eingeschlossene Computer.  Dieser master Runbook startet eine untergeordnete Runbook auf jeder Windows-Agent, die Installation von erforderlichen Updates ausführt.  

### <a name="viewing-update-deployments"></a>Anzeigen von Update-Installationen

Klicken Sie auf die **Bereitstellung** Kachel zum Anzeigen der Liste der vorhandenen Bereitstellung auf aktualisieren.  Sie werden nach Status **geplant**, **ausgeführt**und **abgeschlossen**gruppiert.<br><br> ![Aktualisieren der Seite Bereitstellung planen](./media/oms-solution-update-management/update-updatedeployment-schedule-page.png)<br>  

In der folgenden Tabelle werden die Eigenschaften für jede Bereitstellung beschrieben.

Eigenschaft | Beschreibung|
----------|----------|
Name | Name der Bereitstellung des Updates.|
Zeitplan | Typ des Zeitplans.  *OneTime* ist derzeit der einzige mögliche Wert.|
Startzeit|Datum und Uhrzeit der Bereitstellung beginnen soll.|
Dauer | Anzahl der Minuten, die Bereitstellung wird ausgeführt.  Wenn innerhalb dieses Zeitraums nicht alle Updates installiert wurden, müssen die verbleibenden Updates bis zur nächsten Aktualisierung Bereitstellung warten.|
Server | Anzahl der Computer, die von der Bereitstellung betroffen.|
Status | Aktuellen Status der Bereitstellung des Updates.<br><br>Mögliche Werte sind:<br>-Nicht gestartet<br>-Ausführung<br>-Abgeschlossen|  

Klicken Sie auf eine Bereitstellung an der Detailbildschirm enthält die Spalten in der folgenden Tabelle.  Diese Spalten werden nicht ausgefüllt werden, wenn die Bereitstellung noch nicht gestartet wurde.<br>

Spalte | Beschreibung|
----------|----------|
**Führt**||
Erfolgreich abgeschlossen | Listet die Anzahl der Computer in der Bereitstellung von Status.  Klicken Sie auf Status auf Protokoll suchen wieder alle Datensätze mit diesem Status für die Bereitstellung aktualisieren aktualisieren.|
Computer-Installationsstatus| Listet die Computer in der Bereitstellung und Anteil Updates erfolgreich installiert. Klicken Sie auf einen der Einträge Protokoll Suche alle fehlenden und wichtigen Updates wieder.|
**Instanz Ergebnisse**||
Instanz-Installationsstatus | Listet Klassifikationen Updates Computer fehlen die Anzahl der Computer fehlen Updates in der Kategorie sortiert. Klicken Sie auf einen Computer Protokoll suchen wieder alle Datensätze für diesen Computer zu aktualisieren.|  
<br><br> ![Übersicht über Bereitstellung Ergebnisse](./media/oms-solution-update-management/update-la-updaterunresults-page.png)

### <a name="creating-an-update-deployment"></a>Erstellen einer Bereitstellung

Erstellen Sie eine neue Bereitstellung durch Klicken auf die Schaltfläche am oberen Rand des Bildschirms auf der Seite **Neue Bereitstellung** **Hinzufügen** .  Sie müssen Werte für die Eigenschaften in der folgenden Tabelle angeben.

Eigenschaft | Beschreibung|
----------|----------|
Name | Eindeutigen Namen für die Bereitstellung ein.|
Zeitzone | Zeitzone für die Startzeit.|
Startzeit | Datum und Uhrzeit, um die Bereitstellung zu starten.|
Dauer | Anzahl der Minuten, die Bereitstellung wird ausgeführt.  Wenn innerhalb dieses Zeitraums nicht alle Updates installiert wurden, müssen die verbleibenden Updates bis zur nächsten Aktualisierung Bereitstellung warten.|
Computer | Namen von Computern oder Computergruppen hinzufügen in der Bereitstellung.  Wählen Sie einen oder mehrere Einträge aus der Dropdownliste aus.|
<br><br> ![Neue Bereitstellung Aktualisierungsseite](./media/oms-solution-update-management/update-newupdaterun-page.png)

### <a name="time-range"></a>Zeitraum

Standardmäßig ist der Gültigkeitsbereich Update Management-Lösung analysiert Daten aus allen verbundenen Verwaltungsgruppen, die innerhalb des letzten 24 Stunden. 

Ändern Sie den Zeitraum der Daten wählen Sie oben das Dashboard **basierend auf** . Sie können Datensätze erstellt oder aktualisiert, die innerhalb der letzten 7 Tage, 1 Tag oder 6 Stunden auswählen. Oder wählen Sie **Benutzerdefiniert** und geben Sie einen benutzerdefinierten Datumsbereich.<br><br> ![Benutzerdefinierte Zeitraum Option](./media/oms-solution-update-management/update-la-time-range-scope-databasedon.png)  

## <a name="log-analytics-records"></a>Analytics-Datensätze

Update Management-Lösung erstellt zwei Arten von Datensätzen in die OMS-Repository.

### <a name="update-records"></a>Aktualisieren von Datensätzen

Ein Datensatz mit einem **Update** entsteht für jedes Update, das installiert oder auf jedem Computer erforderlich ist. Aktualisieren von Datensätzen haben die Eigenschaften in der folgenden Tabelle.

Eigenschaft | Beschreibung|
----------|----------|
Typ | *Update*|
SourceSystem | Die Quelle, die Installation des Updates genehmigt.<br>Mögliche Werte sind:<br>-Microsoft Update<br>-Windows Update<br>-SCCM<br>-Linux Servern (abgerufen vom Paket-Manager)|
Genehmigt | Gibt an, ob das Update zur Installation genehmigt wurden.<br> Für Linux-Server ist derzeit als optionale OMS Patches nicht verwaltet ist.|
Klassifizierung für Windows | Klassifizierung des Updates.<br>Mögliche Werte sind:<br>-Applikationen<br>-Wichtige Updates<br>-Definitionsupdates<br>-Feature Packs<br>-Sicherheits-Updates<br>-Service Packs<br>-Update Rollups<br>-Updates|
Klassifizierung für Linux | Toskaners des Updates.<br>Mögliche Werte sind:<br>-Wichtige Updates<br>-Sicherheits-Updates<br>-Andere Updates|
Computer | Name des Computers.|
InstallTimeAvailable | Gibt an, ob die Installationszeit andere Agents ab, die das Update installiert.|
InstallTimePredictionSeconds | Geschätzte Installationszeit in Sekunden basierend auf andere Agents, die das Update installiert.|
KBID | ID des KB-Artikels, der das Update beschreibt.|
"Verwaltungsgruppenname" | Name der Verwaltungsgruppe für SCOM-Agenten.  Für andere Agenten ist AOI -<workspace ID>.|
MSRCBulletinID | ID des Microsoft Security Bulletin beschreibt das Update.|
MSRCSeverity | Schweregrad des Microsoft Security Bulletins.<br>Mögliche Werte sind:<br>-Wichtige<br>-Wichtig<br>-Mittel|
Optionale | Gibt an, ob das Update optional.|
Produkt | Name des Produkts, für das Update ist.  Klicken Sie auf **Ansicht** , um Artikel in einem Browser öffnen.|
PackageSeverity | Der Schweregrad der Linux-Distribution-Anbietern gemeldet in diesem Update behoben. | 
PublishDate | Datum und Uhrzeit, zu der das Update installiert wurde.|
RebootBehavior | Gibt an, ob das Update einen Neustart erzwingt.<br>Mögliche Werte sind:<br>-Canrequestreboot<br>-Neverreboots|
RevisionNumber | Revisionsnummer des Updates.|
SourceComputerId | GUID zur eindeutigen Identifizierung des Computers.|
TimeGenerated | Datum und Uhrzeit der letzten Aktualisierung des Datensatzes.|
Titel | Titel des Updates.|
UpdateID | GUID zur eindeutigen Identifizierung des Updates.|
UpdateState | Gibt an, ob auf diesem Computer installiert ist.<br>Mögliche Werte sind:<br>-Installiert - ist Update auf diesem Computer installiert.<br>-Bedarf - Update nicht installiert und ist auf diesem Computer erforderlich.|  

<br>
Wenn Sie alle Protokolldateien suchen, die Datensätze mit einem **Update** **Updates** Ansicht auswählen können gibt, die Zusammenfassen von Updates von der Suche zurückgegebenen Kacheln angezeigt. Klicken Sie auf die Einträge **fehlt und angewendeten Updates** und **erforderliche und optionale Updates** zu die Ansicht, dass mehrere Updates Bereich angeordnet. Wählen Sie die **Liste** oder **Tabelle** die einzelnen Datensätze zurückgegeben.<br> 

![Protokollansicht Suche Update mit Datensatz aktualisieren](./media/oms-solution-update-management/update-la-view-updates.png)  

In **der Tabellenansicht** können Sie **KBID** für alle Datensätze mit dem KB-Artikel einen Browser öffnen klicken. Dadurch können Sie die Details eines bestimmten Updates schnell lesen.<br> 

![Protokolldatei suchen Tabellenansicht mit Kacheln Datensatztyp Updates](./media/oms-solution-update-management/update-la-view-table.png)

In **der Listenansicht** klicken Sie auf den Link **Anzeigen** neben KBID KB-Artikel öffnen.<br>

![Protokolldatei suchen Listenansicht mit Kacheln Datensatztyp Updates](./media/oms-solution-update-management/update-la-view-list.png)


###<a name="updatesummary-records"></a>UpdateSummary Datensätze

Ein Datensatz mit einem **UpdateSummary** wird für jeden Agent-Computer erstellt. Dieser Datensatz wird jedes Mal aktualisiert der Computer durchsucht nach Updates. **UpdateSummary** Datensätze haben die Eigenschaften in der folgenden Tabelle.

Eigenschaft | Beschreibung|
----------|----------|
Typ | UpdateSummary|
SourceSystem | OpsManager |
Computer | Name des Computers.|
CriticalUpdatesMissing | Anzahl der kritischen Updates auf dem Computer fehlt.|
"Verwaltungsgruppenname" | Name der Verwaltungsgruppe für SCOM-Agenten. Für andere Agenten ist AOI -<workspace ID>.|
NETRuntimeVersion | Die Version von .NET Runtime auf dem Computer installiert.|
OldestMissingSecurityUpdateBucket | Bucket Zeit kategorisieren, seit die älteste fehlende Sicherheit auf diesem Computer wurde veröffentlicht.<br>Mögliche Werte sind:<br>-Ältere<br>-180 Tagen<br>-150 Tage<br>-120 Tagen<br>-90 Tagen<br>-60 Tage<br>-30 Tage<br>-Letzte|
OldestMissingSecurityUpdateInDays | Anzahl der Tage seit der ältesten fehlenden Sicherheitsupdates auf diesem Computer wurde veröffentlicht.|
OsVersion | Version des Betriebssystems auf dem Computer installiert.|
OtherUpdatesMissing | Anzahl der anderen auf dem Computer nicht installiert.|
SecurityUpdatesMissing | Anzahl der Sicherheitsupdates fehlen auf dem Computer.|
SourceComputerId | GUID zur eindeutigen Identifizierung des Computers.|
TimeGenerated | Datum und Uhrzeit der letzten Aktualisierung des Datensatzes.|
TotalUpdatesMissing |Gesamtzahl der auf dem Computer nicht installiert.|
WindowsUpdateAgentVersion | Versionsnummer des Windows Update-Agent auf dem Computer.|
WindowsUpdateSetting | Einstellung für wie der Computer wichtige Updates installieren.<br>Mögliche Werte sind:<br>-Deaktiviert<br>-Vor der Installation benachrichtigen<br>-Geplante installation|
WSUSServer | URL der WSUS-Server, wenn der Computer eine konfiguriert.|  

## <a name="sample-log-searches"></a>Beispiel-Protokoll suchen

Die folgende Tabelle enthält Beispiel Protokoll sucht Datensätze aktualisieren diese Lösung gesammelt. 

Abfrage | Beschreibung|
----------|----------|
Alle Computer mit fehlenden updates | Typ = Update UpdateState = Optional erforderlich = False & #124; Wählen Sie Computer Titel KBID, Klassifizierung, UpdateSeverity, PublishedDate|
Fehlende Updates für Computer "COMPUTER01.contoso.com" (ersetzen durch Ihre eigenen Computernamen) | Typ = Update UpdateState = Optional erforderlich = false Computer = "COMPUTER01.contoso.com" & #124; Wählen Sie Computer Titel KBID, Produkt, UpdateSeverity, PublishedDate|
Alle Computer mit kritischen und Sicherheitsupdates | Typ = Update UpdateState = Optional erforderlich = False (Klassifizierung = "Sicherheitsupdates" oder Klassifizierung "Wichtige Updates" =)|
Kritisch oder Sicherheit Maschinen, Updates manuell angewendet werden, erforderliche updates | Typ = Update UpdateState = Optional erforderlich = False (Klassifizierung = "Sicherheitsupdates" oder Klassifizierung "Wichtige Updates" =) Computer IN {Typ = UpdateSummary WindowsUpdateSetting = manuelle & #124; Verschiedene Computer} & #124; Unterschiedliche KBID|
Fehlerereignisse für Computer, auf denen wichtige fehlende oder Security Updates erforderlich | Typ = Ereignis EventLevelName = Fehler im Computer {Typ = Update (Klassifizierung = "Sicherheitsupdates" oder Klassifizierung "Wichtige Updates" =) UpdateState = Optional erforderlich = False & #124; Verschiedene Computer}|
Alle Computer mit Update-rollups | Typ = Optional Update = false Klassifizierung = "Updaterollups" UpdateState = benötigt & #124; Wählen Sie Computer Titel KBID, Klassifizierung, UpdateSeverity, PublishedDate|
Unterschiedliche fehlende Updates auf allen Computern | Typ = Update UpdateState = Optional erforderlich = False & #124; Unterschiedliche Titel|
WSUS Computermitgliedschaft | Typ = UpdateSummary & #124; Measure count() von WSUSServer|
Automatische Konfiguration | Typ = UpdateSummary & #124; Measure count() von WindowsUpdateSetting|
Computer mit Automatische Updates deaktiviert | Typ = UpdateSummary WindowsUpdateSetting = manuell|  
Liste aller Linux-Computer die ein Aktualisierungspaket verfügbar | Typ = Update und OSType = Linux und UpdateState! = "Nicht erforderlich" & #124; count() Computer messen|
Liste aller Linux-Computer die ein Aktualisierungspaket verfügbar die Schwachstelle kritisch oder Sicherheit Adressen | Typ = Update und OSType = Linux und UpdateState! = "Nicht erforderlich" und (Klassifizierung = oder Klassifizierung "Wichtige Updates" = "Sicherheitsupdates") & #124; count() Computer messen|
Liste aller Pakete, die eine Aktualisierung | Typ = Update und OSType = Linux und UpdateState! = "Nicht erforderlich"|
Liste aller Pakete, die eine Aktualisierung der kritischen oder Sicherheit Sicherheitslücke behebt | Typ = Update und OSType = Linux und UpdateState! = "Nicht erforderlich" und (Klassifizierung = oder Klassifizierung "Wichtige Updates" = "Sicherheitsupdates")|
Liste aller "Ubuntu" Computer mit Update verfügbar | Typ = Update und OSType = Linux und OSName = Ubuntu & #124; count() Computer messen|

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie Protokoll suchen in [Protokollanalyse](../log-analytics/log-analytics-log-searches.md) detaillierten Update-Daten anzeigen.

- [Erstellen Sie eigene Dashboards](../log-analytics/log-analytics-dashboards.md) zeigen Kompatibilität für verwaltete Computer.

- [Alerts erstellen](../log-analytics/log-analytics-alerts.md) , wenn wichtige Updates festgestellt werden, vom Computer oder einem Computer fehlt verfügt über automatische Updates deaktiviert.  




