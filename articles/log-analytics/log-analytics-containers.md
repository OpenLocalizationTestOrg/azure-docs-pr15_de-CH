<properties
    pageTitle="Container Lösung Protokollanalyse | Microsoft Azure"
    description="Die Container Lösung Protokollanalyse können Sie anzeigen und Andockfenster Container Hosts an einem zentralen Ort verwalten."
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
    ms.date="10/10/2016"
    ms.author="banders"/>



# <a name="containers-preview-solution-log-analytics"></a>Container (Vorschau) Lösung Protokollanalyse

Dieser Artikel beschreibt das Einrichten und Verwenden der Container-Lösung in Protokollanalyse, wodurch Sie anzeigen und Andockfenster Container Hosts an einem zentralen Ort verwalten. Andockfenster ist eine Software-Virtualisierung Container erstellt, die Bereitstellung von Software in ihre IT-Infrastruktur automatisieren.

Die Lösung finden Sie unter der Container auf die Container-Hosts ausgeführt werden und welche Bilder in den Containern ausgeführt werden. Sie können detaillierte Überwachungsinformationen mit Befehlen mit anzeigen. Und Problembehandlung Container anzeigen und zentrale Protokolle ohne Einsicht Andockfenster Hosts suchen. Container finden, die laut und verwendeten überschüssigen Ressourcen auf einem Server. Und zentrale CPU, Speicher, und Netzwerkinformationen Auslastung und Performance für Container anzeigen.

## <a name="installing-and-configuring-the-solution"></a>Installation und Konfiguration der Lösung

Anhand der folgenden Informationen zum Installieren und Konfigurieren der Lösung.

Hinzufügen der Lösung Container OMS Arbeitsbereich mithilfe der Schritte im [Lösungskatalog Lösungen Protokollanalyse hinzufügen](log-analytics-add-solutions.md).

Es gibt zwei Methoden zum Installieren und verwenden Sie Andockfenster mit OMS:

- Auf Linux Betriebssystemen installieren Andockfenster ausführen und installieren und Konfigurieren von OMS-Agenten für Linux
- CoreOS installieren führen Sie Andockfenster aus und konfigurieren Sie die OMSAgent in einem Container ausgeführt

Überprüfen Sie unterstützten Andockfenster und Linux Betriebssystem-Versionen für den containerhost auf [GitHub](https://github.com/Microsoft/OMS-docker).

>[AZURE.IMPORTANT] Andockfenster muss ausführen **vor** Installation des [OMS-Agenten für Linux](log-analytics-linux-agents.md) auf die Container-Hosts. Wenn Sie vor der Installation Andockfenster bereits den Agent installiert haben, müssen Sie den OMS-Agenten für Linux neu installieren. Weitere Informationen über Andockfenster [Andockfenster Website](https://www.docker.com)anzeigen

Sie benötigen folgenden Einstellungen auf Container Hosts vor dem Container zu überwachen.

## <a name="configure-settings-for-the-linux-container-host"></a>Konfigurieren Sie für Linux containerhost

Nach der Installation Andockfenster verwenden Sie Folgendes für den containerhost Agent für die Verwendung mit Andockfenster konfigurieren. CoreOS unterstützt diese Methode nicht.

### <a name="to-configure-settings-for-the-container-host---systemd-suse-opensuse-centos-7x-rhel-7x-and-ubuntu-15x-and-higher"></a>Für den containerhost - Systemd konfigurieren (SUSE, OpenSUSE, CentOS 7.x RHEL 7.x und Ubuntu 15.x und höher)

1. Bearbeiten Sie docker.service um Folgendes hinzufügen:

    ```
    [Service]
    ...
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ...
    ```

2. Hinzufügen $DOCKER\_setzt &quot;ExecStart = / Usr/Bin/Andockfenster Daemon&quot; in der Datei docker.service. Im folgende Beispiel verwenden.

    ```
    [Service]
    Environment="DOCKER_OPTS=--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ExecStart=/usr/bin/docker daemon -H fd:// $DOCKER_OPTS
    ```

3. Starten Sie den Dienst Andockfenster. Zum Beispiel:

    ```
    sudo systemctl restart docker.service
    ```

### <a name="to-configure-settings-for-the-container-host---upstart-ubuntu-14x"></a>Konfigurieren für den containerhost - Upstart (Ubuntu 14.x)

1. Bearbeiten Sie /etc/default/docker, und fügen Sie Folgendes hinzu:

    ```
    DOCKER_OPTS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Speichern Sie die Datei und starten Sie die Andockfenster und OMS-Dienste.

    ```
    sudo service docker restart
    ```

### <a name="to-configure-settings-for-the-container-host---amazon-linux"></a>Für den containerhost - Amazon Linux konfigurieren

1. Bearbeiten Sie /etc/sysconfig/docker, und fügen Sie Folgendes hinzu:

    ```
    OPTIONS="--log-driver=fluentd --log-opt fluentd-address=localhost:25225"
    ```

2. Speichern Sie die Datei und starten Sie den Dienst Andockfenster.

    ```
    sudo service docker restart
    ```

## <a name="configure-settings-for-coreos-containers"></a>Konfigurieren Sie für CoreOS Container

Nach der Installation Andockfenster verwenden Sie Folgendes CoreOS zu Andockfenster erstellen einen Container. Eine unterstützte Version von Linux verwenden, einschließlich CoreOS, mit dieser Methode. Sie benötigen die [OMS-Workspace-ID und Schlüssel](log-analytics-linux-agents.md).

### <a name="to-use-oms-for-all-containers-with-coreos"></a>Mit OMS für alle Container mit CoreOS

- Starten Sie den OMS-Container, den Sie überwachen möchten. Ändern und mithilfe des folgenden Beispiels.

  ```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25224:25224/udp -p 127.0.0.1:25225:25225 --name="omsagent" --log-driver=none --restart=always microsoft/oms
```

### <a name="switching-from-using-an-installed-agent-to-one-in-a-container"></a>Mit installierten Agenten in einem Container zu wechseln

Wenn Sie zuvor den Agent direkt installiert und einen Agenten, der in einem Container verwenden, müssen Sie OMSAgent entfernen. Lesen Sie [zum Installieren des OMS-Agenten für Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md).

## <a name="containers-data-collection-details"></a>Einzelheiten zur Datensammlung Container

Container-Lösung sammelt verschiedene Leistung Metriken und Protokoll Container Hosts und Containern OMS-Agenten für Linux aktiviert haben und OMSAgent im Container.

Tabelle von Datenerfassungsmethoden und andere Details wie der Daten für Container.

| Plattform | OMS-Agenten für Linux | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Linux|![Ja](./media/log-analytics-containers/oms-bullet-green.png)|![Nein](./media/log-analytics-containers/oms-bullet-red.png)|![Nein](./media/log-analytics-containers/oms-bullet-red.png)|            ![Nein](./media/log-analytics-containers/oms-bullet-red.png)|![Nein](./media/log-analytics-containers/oms-bullet-red.png)| alle 3 Minuten|


Die folgende Tabelle zeigt Beispiele für Datentypen von Container-Lösung erfasst:

| Datentyp | Felder |
| --- | --- |
| Für Hosts und Container | Computer, Objektname, CounterName & #40; % Prozessorzeit MB Lesevorgänge, Schreibvorgänge MB Arbeitsspeicher Verwendung MB Netzwerk empfangenen Bytes, Netzwerk senden Bytes Prozessor Verwendung s Netzwerk & #41; Gegenwert, TimeGenerated, CounterPath, SourceSystem |
| Container-Lager | TimeGenerated, Computer, ContainerHostname, Bild, ImageTag, ContinerState, ExitCode, EnvironmentVar, Befehl, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID Containername |
| Container Bild Lager | TimeGenerated Computer Bild, ImageTag, ImageSize, VirtualSize, ausgeführt, angehalten, beendet, Fehler, SourceSystem, ImageID, TotalContainer |
| Container-Protokoll | TimeGenerated, Computer, Bild-ID Containernamen LogEntrySource LogEntry, SourceSystem, ContainerID |
| Container-Protokoll | TimeGenerated Computer TimeOfCommand, Bild, Befehl, SourceSystem, ContainerID |

## <a name="monitor-containers"></a>Monitor-Container

Nachdem Sie die Lösung in die OMS-Portal aktiviert haben, sehen Sie **Container** nebeneinander zeigt zusammenfassende Informationen über Container Hosts und Hosts auf Container.

![Container-Kachel](./media/log-analytics-containers/containers-title.png)

Die Kachel zeigt einen Überblick Sie wie viele Container in der Umgebung und ob sie nicht sind, ausgeführt oder beendet.

### <a name="using-the-containers-dashboard"></a>Container-Dashboard verwenden

Klicken Sie auf die Kachel **Container** . Dort sehen Sie Ansichten geordnet:

- Containerereignisse
- Fehler
- Container-Status
- Container Bild Lager
- CPU- und Leistung

Jeder Bereich im Dashboard ist eine visuelle Darstellung einer Suche, die auf Daten ausgeführt wird.

![Container-dashboard](./media/log-analytics-containers/containers-dash01.png)

![Container-dashboard](./media/log-analytics-containers/containers-dash02.png)

Das Blade **Containerstatus** klicken Sie im oberen Bereich wie unten dargestellt.

![Container-status](./media/log-analytics-containers/containers-status.png)

Protokoll suchen wird geöffnet und zeigt Informationen zu den Hosts und Containern ausgeführten.

![Protokoll Container suchen](./media/log-analytics-containers/containers-log-search.png)

Hier können Sie die Abfrage ändern, um bestimmte Informationen, die Sie interessieren. Weitere Informationen zum Protokoll suchen finden Sie [Log durchsucht Protokollanalyse](log-analytics-log-searches.md).

Beispielsweise können die Abfrage ändern, sodass er zeigt alle beendeten Container anstelle der laufenden in **angehalten** ändern **ausgeführt** , in der Abfrage.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Problembehandlung bei fehlgeschlagenen Container finden

OMS markiert einen Container als **fehlgeschlagen** , wenn mit einem Exitcode ungleich Null beendet wurde. Sie können einen Fehler und Ausfälle in der Umgebung in der Blade- **Container konnte** Überblick.

### <a name="to-find-failed-containers"></a>Fehler bei zu Containern

1. Klicken Sie auf Blatt **Containerereignisse** .  
  ![Container-Ereignisse](./media/log-analytics-containers/containers-events.png)
2. Suche wird geöffnet und zeigt den Status von Containern, ähnlich der folgenden protokolliert.  
  ![Container-Zustand](./media/log-analytics-containers/containers-container-state.png)
3. Klicken Sie den fehlgeschlagenen Wert um weitere Informationen wie Größe und Anzahl der beendet und fehlerhafte Bilder anzeigen. Erweitern Sie zum Anzeigen der Bild-ID **mehr**  
  ![Fehlgeschlagene Container](./media/log-analytics-containers/containers-state-failed.png)
4. Suchen Sie als Nächstes den Container, der das Bild ausgeführt wird. Geben Sie in der Abfrage.
  `Type=ContainerInventory <ImageID>`Die Protokolle angezeigt. Sie können einen Bildlauf durchführen fehlgeschlagenen Container angezeigt.  
  ![Fehlgeschlagene Container](./media/log-analytics-containers/containers-failed04.png)


## <a name="search-logs-for-container-data"></a>Suchprotokolle für Containerdaten

Wenn Sie einen bestimmten Fehler beheben, können sie sehen, wo es in der Umgebung auftritt. Die folgenden Protokolltypen können Sie Abfragen erstellen, um die Informationen zurückgeben möchten.

- **ContainerInventory** – mit solchen Informationen Container Position, was ihre Namen sind und welche Bilder möchten sie laufen.
- **ContainerImageInventory** – verwenden Sie diesen Typ, wenn Sie versuchen, Informationen organisiert Bild und Bildinformationen wie Bild-IDs oder Größe anzeigen.
- **ContainerLog** – dieser Typ Wenn Protokollinformationen und Posten suchen soll.
- **ContainerServiceLog** – dieser Typ beim Audit Trail Andockfenster-Daemon starten, anhalten, löschen oder Pull-Befehle finden.

### <a name="to-search-logs-for-container-data"></a>Protokolle für Containerdaten durchsuchen

- Wählen Sie ein Bild, das Sie wissen kürzlich ausgefallen und die Fehlerprotokolle finden. Zuerst ein Container, der das Bild mit **ContainerInventory** Suche ausgeführt wird. Beispielsweise suchen`Type=ContainerInventory ubuntu Failed`  
    ![Suche für Ubuntu-Container](./media/log-analytics-containers/search-ubuntu.png)

  Notieren Sie den Namen des Containers neben **Name**und Protokollen suchen. In diesem Beispiel ist `Type=ContainerLog adoring_meitner`.

**Performance-Informationen anzeigen**

Wenn Sie Abfragen sind, hilft es sehen was zunächst möglich ist. Versuchen Sie beispielsweise, um alle Daten anzuzeigen, eine umfassende Abfrage durch Eingabe der folgenden Abfrage.

```
Type=Perf
```

![Container-Leistung](./media/log-analytics-containers/containers-perf01.png)



Sie sehen dies mehr grafisch Wenn Word **Metriken** in den Suchergebnissen klicken.

![Container-Leistung](./media/log-analytics-containers/containers-perf02.png)



Sie können die Leistungsdaten Bereich, die Sie zu einem bestimmten Container durch Eingabe des Namens der Abfrage rechts sehen.

```
Type=Perf <containerName>
```

Die Liste für einen einzelnen Container erfassten Leistungsmetriken.

![Container-Leistung](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Beispiel-Protokoll Suchabfragen

Es ist häufig sinnvoll, Abfragen mit Beispiel starten und ändern sie entsprechend Ihrer Umgebung. Sie können als Ausgangspunkt mit **Bemerkenswerten Abfragen** zu erweiterten Abfragen experimentieren.

![Container Abfragen](./media/log-analytics-containers/containers-queries.png)

## <a name="saving-log-search-queries"></a>Protokoll speichern Suchabfragen

Speichern von Abfragen ist eine Standardfunktion Protokollanalyse. Speichern sie, Sie müssen die nützlichsten finde für zukünftige Verwendung.

Speichern sie durch Klicken auf **Favoriten** oben auf der Seite Protokoll nach Erstellen einer Abfrage, die Sie hilfreich finden. Dann können Sie es später auf **Mein Dashboard** bequem.

## <a name="next-steps"></a>Nächste Schritte

- [Suche Protokolle](log-analytics-log-searches.md) detaillierte Container Datensätze anzeigen.
