<properties
    pageTitle="Überwachen von VMware Lösung Protokollanalyse | Microsoft Azure"
    description="Überwachen von VMware Lösung helfen kann Protokolle verwalten und Überwachen von ESXi Hosts erfahren."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="banders"/>

# <a name="vmware-monitoring-preview-solution-in-log-analytics"></a>Protokollanalyse Lösung VMware überwachen (Vorschau)

Die Überwachung von VMware Lösung Protokollanalyse ist eine Lösung, die eine zentrale Protokollierung und Überwachungsmethode für große VMware Protokolle erstellen. Dieser Artikel beschreibt, wie Sie behandeln, erfassen und verwalten ESXi-Hosts an einem Standort mit der Lösung. Mit der Lösung sehen Sie detaillierte Daten für alle ESXi-Hosts an einem Standort. Sie sehen wichtigsten zählt, Status und Trends der VM und ESXi Hosts ESXi Host Protokolle bereitgestellt. Sie können durch anzeigen und Durchsuchen von zentralen ESXi protokolliert beheben. Und Alarme basierend auf Suchabfragen Protokoll erstellen.

Die Lösung verwendet systemeigene Syslog Funktionalität ESXi Host Push Daten auf VM OMS hat. Die Lösung Schreiben nicht, Dateien in Syslog innerhalb des Ziels VM. Der OMS-Agent öffnet Port 1514 und dies überwacht. Nach die Daten Erhalt legt der OMS-Agent Daten OMS.

## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung

Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Fügen Sie die Überwachung von VMware Lösung OMS Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md).

#### <a name="supported-vmware-esxi-hosts"></a>Unterstützte VMware ESXi-hosts
vSphere ESXi Host 5.5 und 6.0

#### <a name="prepare-a-linux-server"></a>Vorbereiten eines Linux-Servers
Erstellen Sie dem Betriebssystem Linux VM Syslog Daten von ESXi-Hosts empfangen. [OMS-Linux-Agent](log-analytics-linux-agents.md) ist der Sammelpunkt für alle ESXi Host Syslog. Mehrere ESXi-Hosts können Sie Protokolle auf einem Linux-Server, wie im folgenden Beispiel weiterleiten.  

   ![Syslog-Fluss](./media/log-analytics-vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Konfigurieren von Syslog-Auflistung

1. Richten Sie Syslog-Weiterleitung für VSphere. Detaillierte Informationen zum Syslog-Weiterleitung einrichten, finden Sie unter [Konfigurieren Syslog ESXi 5.x und 6.0 (2003322)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322). Zur **Konfiguration des ESXi** > **Software** > **Erweiterte Einstellungen** > **Syslog**.
  ![vsphereconfig](./media/log-analytics-vmware/vsphere1.png)  

2. Fügen Sie im Feld *Syslog.global.logHost* Linux-Server und Port Number *1514*. Beispielsweise `tcp://hostname:1514` oder`tcp://123.456.789.101:1514`

3. Öffnen Sie die ESXi Hostfirewall für Syslog. **ESXi Konfiguration** > **Software** > **Sicherheitsprofil** > **Firewall** und **Eigenschaften**.  

    ![vspherefw](./media/log-analytics-vmware/vsphere2.png)  

    ![vspherefwproperties](./media/log-analytics-vmware/vsphere3.png)  

4. Überprüfen Sie vSphere Konsole überprüfen, Syslog ordnungsgemäß eingerichtet ist. Bestätigen der ESXI Host, Port **1514** konfiguriert.

5. Testen Sie die Konnektivität zwischen dem Linux-Server und ESXi Host mithilfe der `nc` Befehl ESXi Host. Zum Beispiel:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

6. Downloaden und Installieren der OMS-Agent für Linux für Linux-Server. Weitere Informationen finden Sie in der [Dokumentation für OMS-Agent für Linux](https://github.com/Microsoft/OMS-Agent-for-Linux).

7. Nachdem der OMS für Linux Agent wechseln Sie zum Verzeichnis /etc/opt/microsoft/omsagent/sysconf/omsagent.d, und kopieren Sie die Datei vmware_esxi.conf in das Verzeichnis /etc/opt/microsoft/omsagent/conf/omsagent.d und die Änderung, Gruppe Besitzer und Berechtigungen für die Datei. Zum Beispiel:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```

8.  Starten der OMS-Agent für Linux `sudo /opt/microsoft/omsagent/bin/service_control restart`.

9. Im Portal OMS suchen Protokoll für `Type=VMware_CL`. Wenn OMS Syslog-Daten sammelt, beibehalten Syslog-Format. Im Portal werden einige spezifische Felder wie *Hostname* und *ProcessName*erfasst.  

    ![Typ](./media/log-analytics-vmware/type.png)  

    Sind Ihre Suchergebnissen anzeigen wie in der Abbildung oben sind festlegen Dashboard Lösung OMS VMware Überwachung verwenden.  

## <a name="vmware-data-collection-details"></a>VMware Einzelheiten zur Datensammlung

Überwachen von VMware Lösung sammelt verschiedene Leistung Metriken und Protokoll ESXi-Hosts verwenden die OMS-Agenten für Linux aktiviert haben.

Tabelle von Datenerfassungsmethoden und andere Details wie Daten gesammelt werden.

| Plattform | OMS-Agenten für Linux | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Linux|![Ja](./media/log-analytics-vmware/oms-bullet-green.png)|![Nein](./media/log-analytics-vmware/oms-bullet-red.png)|![Nein](./media/log-analytics-vmware/oms-bullet-red.png)|            ![Nein](./media/log-analytics-containers/oms-bullet-red.png)|![Nein](./media/log-analytics-vmware/oms-bullet-red.png)| alle 3 Minuten|


In der folgenden Tabelle Beispiele für Datenfelder von VMware überwachen Lösung gesammelt:

| Feldname | Beschreibung |
| --- | --- |
| Device_s| VMware-Speichergeräte |
| ESXIFailure_s | die Fehlertypen |
| EventTime_t | Zeit Ereignis aufgetreten ist |
| HostName_s | ESXi Hostnamen |
| Operation_s | VM erstellen oder Löschen von VM |
| ProcessName_s | Ereignisname |
| ResourceId_s | Name des VMware-Hosts |
| ResourceLocation_s | VMware |
| ResourceName_s | VMware |
| ResourceType_s | Hyper-V |
| SCSIStatus_s | VMware SCSI-status |
| SyslogMessage_s | Syslog-Daten |
| UserName_s | Benutzer erstellt oder gelöscht VM |
| VMName_s | Name des virtuellen Computers |
| Computer | Host-computer |
| TimeGenerated | Zeit der erstellten Daten |
| DataCenter_s | VMware-Rechenzentrum |
| StorageLatency_s | Speicher-Wartezeit (ms) |

## <a name="vmware-monitoring-solution-overview"></a>Lösungsübersicht VMware-Überwachung

Die VMware-Kachel wird in OMS-Portal. Gibt einen allgemeinen Fehler. Wenn Sie die Kachel klicken, gelangen Sie in eine Dashboard-Ansicht.

![nebeneinander](./media/log-analytics-vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Navigieren Sie in der Dashboardansicht

In der Übersichtsansicht **VMware** Blades organisiert:

- Fehlerzähler Status
- Top Host Ereignis zählt
- Anzahl der wichtigsten
- Virtual Machine-Aktivitäten
- ESXi Veranstaltungen Datenträger


![Solution1](./media/log-analytics-vmware/solutionview1-1.png)

![Lösung2](./media/log-analytics-vmware/solutionview1-2.png)

Klicken Sie auf alle Blades Protokollanalyse Suchbereich öffnen, das detaillierte Informationen für das Blade anzeigt.

Hier können Sie die Suchabfrage nach etwas bestimmtem ändern bearbeiten. Finden Sie ein Lernprogramm über die Grundlagen der OMS-Suche der [OMS Protokoll suchen Lernprogramm.](log-analytics-log-searches.md)

#### <a name="find-esxi-host-events"></a>ESXi Host Ereignisse

Ein einzelner ESXi Host generiert mehrere Protokolle basierend auf ihrer Prozesse. Überwachen von VMware Lösung zentralisiert werden und Ereignis zählt zusammengefasst. Diese zentrale Ansicht können Sie verstehen, welche ESXi Host eine hohe Anzahl von Ereignissen und welche Ereignisse in Ihrer Umgebung am häufigsten auftreten.

![Ereignis](./media/log-analytics-vmware/events.png)

Sie können anzeigen Weitere ein ESXi Host oder ein Ereignistyp.

Beim Anklicken eines Hostnamens ESXi hier Informationen vom ESXi Host. Fügen Sie ggf. mit dem Ereignistyp zurückgeliefert `“ProcessName_s=EVENT TYPE”` in Ihrer Suchabfrage. Sie können im Suchfilter **ProcessName** auswählen. Das beschränkt die Informationen.

![Bohren](./media/log-analytics-vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Hohe VM Aktivitäten

Ein virtueller Computer können erstellt und auf irgendwelche ESXi gelöscht. Es empfiehlt sich ein Administrator wie viele VMs identifizieren ein ESXi Host erstellt. Diese wiederum, können Leistung und Kapazität planen. Verfolgen von VM-Aktivitätsereignisse ist entscheidend, wenn Ihre Umgebung verwalten.

![Bohren](./media/log-analytics-vmware/vmactivities1.png)

Ggf. zusätzlicher ESXi Host VM erstellen Daten klicken Sie auf einen Hostnamen ESXi.

![Bohren](./media/log-analytics-vmware/createvm.png)

#### <a name="common-search-queries"></a>Häufige Suchabfragen

Die Lösung umfasst andere Abfragen, mit denen Sie Ihre Hosts ESXi hohen Speicherplatz Speicherlatenz und Pfadausfall verwalten können.

![Abfragen](./media/log-analytics-vmware/queries.png)

#### <a name="save-queries"></a>Speichern von Abfragen

Speichern von Suchabfragen ist ein Standardfeature in OMS und können Sie Fragen zu halten, die nützlichsten finde. Speichern sie durch Klicken auf **Favoriten**nach Erstellen einer Abfrage, die Sie hilfreich finden. Eine gespeicherte Abfrage können Sie es später auf [Mein Dashboard](log-analytics-dashboards.md) jederzeit wiederverwenden können Sie benutzerdefinierte Dashboards erstellen.

![DockerDashboardView](./media/log-analytics-vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Alarme aus Abfragen erstellen

Nach dem Erstellen der Abfragen sollten Sie Abfragen verwenden Sie benachrichtigen, wenn bestimmte Ereignisse eintreten. Anzeigen Sie [Alarme in Protokollanalyse](log-analytics-alerts.md) Informationen zu alerts Beispiele für Abfragen und andere Beispiele für die Warnung Blogbeitrag der [VMware Monitor verwenden OMS-Protokollanalyse](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) .

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>Was muss ich auf die ESXi host festlegen? Welche Auswirkung haben sie auf Meine aktuellen Umgebung?
Die Lösung verwendet systemeigene ESXi Host Syslog Mechanismus weiterleiten. Sie brauchen keine zusätzlichen Microsoft-Software auf dem ESXi Host Protokolle erfassen. Sie müssen eine geringe Auswirkung auf die vorhandene Umgebung. Allerdings müssen Sie Syslog-Weiterleitung festlegen ESXI Funktionalität.

### <a name="do-i-need-to-restart-my-esxi-host"></a>Mein ESXi Host Neustart erforderlich?
Nein. Dieser Vorgang erfordert einen Neustart nicht. VSphere manchmal nicht korrekt auf das Syslog aktualisiert. In diesem Fall ESXi Host melden und Syslog laden. In diesem Fall müssen Sie Neustart des Servers so dabei auf Ihre Umgebung.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-oms"></a>Kann erhöhen oder Verringern der Protokolldaten OMS gesendet?
Ja, Sie können. ESXi Hostebene Einstellungen können in vSphere. Protokoll Auflistung basiert auf *Info* . Also wenn Sie VM-Erstellung oder Löschung überwachen möchten, müssen Sie die Ebene *Info* Hostd. Weitere Informationen finden Sie in der [VMware-Knowledgebase](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658).

### <a name="why-is-hostd-not-providing-data-to-oms-my-log-setting-is-set-to-info"></a>Warum bietet Hostd keine Daten zu OMS? Informationen meiner Einstellung fest.
Gab es ein Fehler ESXi Host für Syslog-Zeitstempel. Weitere Informationen finden Sie in der [VMware-Knowledgebase](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202). Nach Anwendung die Lösung sollten Hostd Normal.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Kann mehrere ESXi Hosts Weiterleiten von Syslog-Daten einer einzelnen VM mit Omsagent haben?
Ja. Sie können mehrere ESXi Weiterleiten einer einzelnen VM mit Omsagent hostet.

### <a name="why-dont-i-see-data-flowing-into-oms"></a>Warum wird in OMS Daten angezeigt?

Es kann mehrere Gründe:

- ESXi Host darum nicht korrekt Daten mit Omsagent VM. So testen:
    1. Um zu bestätigen, den ESXi Host ssh melden Sie an und führen Sie den folgenden Befehl:`nc -z ipaddressofVM 1514`

        Wenn dies nicht erfolgreich ist, vSphere in der erweiterten Konfiguration sind möglicherweise nicht korrekt. Informationen zum Einrichten der ESXi Host für Syslog-Weiterleitung finden Sie unter [Konfigurieren Syslog-Auflistung](#configure-syslog-collection) .

    2. Syslog-Port Verbindung erfolgreich jedoch immer noch keine Daten sehen, laden Sie das Syslog ESXi Host mit ssh den folgenden Befehl ausführen:` esxcli system syslog reload`

- Die VM mit OMS-Agent wurde nicht ordnungsgemäß festgelegt. Um dies zu testen, führen Sie die folgenden Schritte:
    1. OMS hört Port 1514 und legt Daten OMS. Um sicherzustellen, dass es geöffnet ist, führen Sie den folgenden Befehl ein:`netstat -a | grep 1514`
    2. Port sollte angezeigt werden `1514/tcp` öffnen. Wenn nicht, überprüfen Sie die Omsagent richtig installiert ist. Wenn die Portinformationen nicht angezeigt wird, ist nicht der Syslog-Port auf dem virtuellen Computer geöffnet.
        1. Überprüfen, ob der OMS-Agent ausgeführt wird `ps -ef | grep oms`. Wenn er nicht ausgeführt wird, starten Sie den Prozess mit dem Befehl` sudo /opt/microsoft/omsagent/bin/service_control start`
        2. Öffnen der `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` Datei.

            Stellen Sie sicher, dass die richtigen Benutzer und Gruppe gilt, wie:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

            Wenn die Datei nicht vorhanden oder Benutzer und Gruppe falsch, Abhilfemaßnahmen ergreifen von [einem Linux-Server vorbereiten](#prepare-a-linux-server).

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Protokoll suchen](log-analytics-log-searches.md) in Protokollanalyse VMware Host Detaildaten anzeigen.
- [Erstellen Sie eigene Dashboards](log-analytics-dashboards.md) mit VMware Server-Daten.
- [Alerts erstellen](log-analytics-alerts.md) beim Auftreten bestimmter Ereignisse der VMware-Hosts.
