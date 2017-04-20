<properties
    pageTitle="Verbinden von Linux-Computern mit Protokollanalyse | Microsoft Azure"
    description="Protokollanalyse können Sie sammeln und Bearbeiten von Linux-Computern generiert."
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

# <a name="connect-linux-computers-to-log-analytics"></a>Protokollanalyse Linux-Computer herstellen

Protokollanalyse können Sie sammeln und Bearbeiten von Linux-Computern generiert. Daten von Linux zu OMS hinzufügen, können Sie Linux-Systeme und containerlösungen Andockfenster unabhängig davon, wo sich die Computer befinden – überall. So können die Datenquellen im lokalen Datencenter als physische Server, virtuelle Computer in einer Cloud-gehosteten Dienst Amazon Web Services (AWS) oder Microsoft Azure oder auch der Laptop auf Ihrem Schreibtisch befinden. Außerdem sammelt OMS auch von Windows-Computern ebenso eine wirklich hybride IT-Umgebung unterstützt.

Sie können anzeigen und Verwalten von Daten aus allen Quellen mit Protokollanalyse OMS mit einem einzigen Management-Portal. Dadurch müssen viele verschiedene Systeme macht es leicht zu verwenden und Sie exportieren können alle Daten zu den Business Analytics-Lösung bereits System überwachen.

Dieser Artikel ist ein Schnellstartübersicht, mit denen Sie sammeln und Verwalten von Daten für Ihre Linux-Computer mit der OMS-Agent für Linux. Ausführliche technische Informationen wie Proxyserverkonfiguration Informationen Reststoffe Metriken und benutzerdefinierte JSON-Datenquellen finden Sie Information [OMS-Agent für Linux (Übersicht)](https://github.com/Microsoft/OMS-Agent-for-Linux) und [OMS-Agent für Linux vollständige Dokumentation](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md) auf Github.


Derzeit können Sie folgende Daten von Linux-Computern sammeln:

- Performance-Kennzahlen
- Syslog-Ereignisse
- Alerts von Nagios und Zabbix
- Andockfenster Container Leistungsmetrik, Lager und Protokolle

## <a name="supported-linux-versions"></a>Unterstützte Linux-Versionen

X86 und X64-Versionen sind auf einer Vielzahl von Linux-Distributionen offiziell unterstützt. OMS-Agenten für Linux kann jedoch auch in anderen Distributionen nicht aufgeführt ausgeführt.

- Amazon Linux 2012.09 bis 2015.09
- CentOS Linux 5, 6 und 7
- Oracle Linux 5, 6 und 7
- Red Hat Enterprise Linux Server 5, 6 und 7
- Debian GNU/Linux 6, 7 und 8
- Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10
- SUSE Linux Enterprise Server 11 und 12

## <a name="oms-agent-for-linux"></a>OMS-Agenten für Linux
Operations Management Suite Agent für Linux umfasst mehrere Pakete. Die Release-Datei enthält die folgenden Pakete durch Ausführen von Shell-Paket mit `--extract`.

**Paket** | **Version** | **Beschreibung**
----------- | ----------- | --------------
omsagent | 1.1.0 | Operations Management Suite Agent für Linux
omsconfig | 1.1.1 | Konfigurations-Agent für den OMS-Agent
OMI | 1.0.8.3 | Offene Infrastruktur (OMI) – eine einfache CIM Server
SCX | 1.6.2 | OMI CIM Anbieter für Betriebssystem-Performance-Kennzahlen
Apache-cimprov | 1.0.0 | Anbieter für OMI Apache HTTP Server-Leistung. Nur installiert, wenn Apache HTTP Server erkannt wird.
MySQL-cimprov | 1.0.0 | Anbieter für OMI MySQL-Server-Leistung. Nur installiert, wenn MySQL-MariaDB Server erkannt wird.
Andockfenster cimprov | 0.1.0 | Anbieter für OMI Andockfenster. Nur installiert, wenn Andockfenster erkannt wird.

### <a name="additional-installation-artifacts"></a>Zusätzliche Artefakte
Nach der Installation des OMS-Agents für Linux Pakete werden Neukonfiguration zusätzliche systemweit angewendet. Diese Artefakte werden entfernt, wenn das Omsagent-Paket deinstalliert wird.
- Ein berechtigter Benutzer namens: `omsagent` erstellt. Dies ist das Konto als Omsagent-Daemon ausgeführt wird
- Sudoers "include"-Datei erstellt am /etc/sudoers.d/omsagent autorisiert diese Omsagent Daemons Syslog und Omsagent neu starten. Sudo "include" Direktiven in der installierten Version von Sudo nicht unterstützt, werden diese Einträge in/etc/sudoers geschrieben.
- Syslog-Konfiguration wird geändert, um eine Teilmenge von Ereignissen an dem Agent weiter. Weitere Informationen finden Sie im Abschnitt **Konfigurieren der Datensammlung**

### <a name="linux-data-collection-details"></a>Einzelheiten zur Datensammlung Linux

Tabelle von Datenerfassungsmethoden und andere Details wie Daten gesammelt werden.

| Quelle | Direkte Agent | SCOM-Agenten | Azure-Speicher | SCOM erforderlich? | SCOM-Agentdaten per Verwaltungsgruppe | Häufigkeit der Collection |
|---|---|---|---|---|---|---|
|Zabbix|![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|1 minute|
|Nagios|![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|Bei der Ankunft|
|Syslog|![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|von Azure-Speicher: 10 Minuten; vom Agent: Ankunft|
|Linux-Leistungsindikatoren|![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|Wie geplant, mindestens 10 Sekunden|
|Versionsvergleich|![Ja](./media/log-analytics-linux-agents/oms-bullet-green.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|            ![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|![Nein](./media/log-analytics-linux-agents/oms-bullet-red.png)|stündlich|



### <a name="package-requirements"></a>Paket-Vorschriften
| **Erforderliches Paket**  | **Beschreibung**   | **Mindestens erforderliche version**|
|--------------------- | --------------------- | -------------------|
|Glibc |    GNU C-Bibliothek   | 2,5-12|
|OpenSSL    | OpenSSL-Bibliotheken | 0.9.8E oder 1.0|
|Aufrollen | WebClient Aufrollen | 7.15.5
|Python-ctypes |Funktionsbibliotheken | n/a|
|PAM | Pluggable Authentication Module  |n/a |

>[AZURE.NOTE] Rsyslog oder Syslog-ng müssen Syslog-Nachrichten sammeln. Der standardmäßige Syslog Daemon auf Version 5 von Red Hat Enterprise Linux CentOS und Oracle Linux Version (Sysklog) ist für Syslog-Ereignissammlung nicht unterstützt. Syslog aus dieser Version dieser Distributionen Datensammlung, sollten Rsyslog Daemon installiert und konfiguriert Sysklog ersetzen.

## <a name="quick-install"></a>Schnelle Installation

Führen Sie folgende Befehle auf der Omsagent, die Prüfsumme überprüft dann installieren und den Agent. Befehle sind für 64-Bit. Die Arbeitsplatz-ID und Primärschlüssel finden im OMS-Portal unter **Einstellungen** auf der Registerkarte **Quellen verbunden** .

![Arbeitsbereich-details](./media/log-analytics-linux-agents/oms-direct-agent-primary-key.png)

```
wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/v1.1.0-28/omsagent-1.1.0-28.universal.x64.sh
sha256sum ./omsagent-1.1.0-28.universal.x64.sh
sudo sh ./omsagent-1.1.0-28.universal.x64.sh --upgrade -w <YOUR OMS WORKSPACE ID> -s <YOUR OMS WORKSPACE PRIMARY KEY>
```

Es gibt eine Vielzahl von anderen Methoden installieren Sie den Agent und. Erfahren Sie mehr über diese Schritte [zum Installieren des OMS-Agenten für Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux).

Sie können auch [Azure video-Anleitung](https://www.youtube.com/watch?v=mF1wtHPEzT0)anzeigen.

## <a name="choose-your-linux-data-collection-method"></a>Wählen Sie Ihre Auflistung Linux Daten

Wie wählen Sie die Datentypen, die Sie sammeln möchten, hängt OMS-Portal verwenden möchten oder wenn Sie verschiedene Konfigurationsdateien direkt auf den Linux-Clients zu bearbeiten. Wenn Sie das Portal verwenden, wird die Konfiguration automatisch für alle Linux-Clients gesendet. Benötigen Sie verschiedene Konfigurationen für verschiedene Linux-Clients müssen Sie Client einzeln – bearbeiten oder Alternative PowerShell DSC, Chef oder Marionette verwenden.

Sie können die Syslog-Ereignisse und Leistungsindikatoren sammeln Konfigurationsdateien auf dem Linux-Computer verwenden möchten. *Wenn Sie Datensammlung Konfigurieren von Agent-Konfigurationsdateien bearbeiten möchten, sollten Sie die zentrale Konfiguration deaktivieren.*  Anleitung unten Datensammlung in der Agent-Konfigurationsdateien konfigurieren sowie zentrale Konfiguration für alle OMS-Agenten für Linux oder einzelne Computer deaktiviert wurde.

### <a name="disable-oms-management-for-an-individual-linux-computer"></a>OMS-Verwaltung für einen einzelnen Linux-Computer deaktivieren

Zentralisierte Datensammlung Konfigurationsdaten ist für einen einzelnen Linux-Computer mit dem Skript OMS_MetaConfigHelper.py deaktiviert. Dies ist hilfreich, wenn eine Gruppe von Computern eine spezielle Konfiguration haben.

Zentralisierte Konfiguration deaktivieren:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```

Zentralisierte Konfiguration wieder zu aktivieren:

```
sudo /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py –enable
```

## <a name="linux-performance-counters"></a>Linux-Leistungsindikatoren

Linux-Leistungsindikatoren sind mit Windows-Leistungsindikatoren – beide funktionieren ähnlich. Die folgenden Verfahren können Sie hinzufügen und konfigurieren. Nachdem sie OMS hinzugefügt werden, werden Sie alle 30 Sekunden Daten gesammelt.

### <a name="to-add-a-linux-performance-counter-in-oms"></a>Einen Linux-Leistungsindikator OMS hinzufügen

1. Zum Konfigurieren von OMS-Agenten für Linux mit OMS-Portal auf der Linux-Leistungsindikatoren hinzufügen, klicken Sie auf **Daten**.  
2. Klicken Sie auf der Seite **Einstellungen** unter **Daten** auf **Linux-Leistungsindikatoren** wählen Sie, oder geben Sie den Leistungsindikator, den Sie hinzufügen möchten.  
    ![Daten](./media/log-analytics-linux-agents/oms-settings-data01.png)
3. Wenn Sie den vollständigen Namen des Zählers nicht kennen, können Sie beginnen, einen Teil des Namens eingeben und verfügbaren Leistungsindikatoren aufgeführt. Wenn Sie den Indikator, den Sie hinzufügen möchten finden, klicken Sie in der Liste, und klicken Sie dann auf das Pluszeichen, um den Leistungsindikator hinzuzufügen.
4. Nach dem Hinzufügen des Leistungsindikators in der Liste Leistungsindikatoren mit farbigen Balken hervorgehoben angezeigt.
5. Standardmäßig ist die Option **anwenden unter Konfiguration auf meinem Computer** ausgewählt. Wenn Sie Konfigurationsdaten senden deaktivieren möchten, deaktivieren Sie die Auswahl.
6. Ändern von Leistungsindikatoren haben, am unteren Rand der Seite klicken Sie auf **Speichern** , um die Änderung abzuschließen. Die Konfiguration vorgenommenen haben werden gesendet an alle OMS-Agenten für Linux OMS, normalerweise innerhalb von 5 Minuten registriert werden.

### <a name="configure-linux-performance-counters-in-oms"></a>Linux-Leistungsindikatoren in OMS konfigurieren

Sie können eine bestimmte Instanz für jeden Leistungsindikator, Windows-Leistungsindikatoren. Für Linux-Leistungsindikatoren gilt unabhängig Instanz eines Leistungsindikators, den Sie auswählen für alle untergeordneten Indikatoren des übergeordneten Leistungsindikators. Die folgende Tabelle zeigt die allgemeinen Instanzen auf Linux- und Windows-Leistungsindikatoren.

| **Name der Instanz** | **Bedeutung** |
| --- | --- |
| \_Insgesamt | Summe aller Instanzen |
| \* | Alle Instanzen |
| (/ & #124; / Var) | Instanzen mit dem Namen übereinstimmt: / oder var |


Ebenso gilt für einen Leistungsindikator übergeordnete Auswahl Messintervalls für alle untergeordneten Indikatoren. In anderen Worten werden alle untergeordneten Zähler Messintervalle und Instanzen miteinander verbunden.

### <a name="add-and-configure-performance-metrics-with-linux"></a>Hinzufügen und Konfigurieren von Leistungsdaten mit Linux

Die Konfiguration in /etc/opt/microsoft/omsagent/conf/omsagent.conf werden Leistungsmetriken sammeln gesteuert. Weitere [Verfügbare Leistungsindikatoren](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#appendix-available-performance-metrics) Klassen und OMS-Agent für Linux.

Jedes Objekt oder Kategorie von Leistungsindikatoren sammeln festzulegen in der Konfigurationsdatei als einzelne `<source>` Element. Die Syntax folgt dem nachstehenden Muster.

```
<source>
  type oms_omi  
  object_name "Processor"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

Die konfigurierbaren Parameter dieses Elements sind:

- **Objekt\_Namen**: Objektname für die Auflistung.
- **Instanz\_Regex**: einen *regulären Ausdruck* definiert Instanzen sammeln. Wert: `.*` alle Instanzen gibt. Nur sammeln Prozessor Metriken für die \_insgesamt Instanz geben Sie `_Total`. Sammeln Sie Prozessmetriken nur Crond oder Sshd Instanzen geben Sie: `(crond|sshd)`.
- **Zähler\_Name\_Regex**: einen *regulären Ausdruck* definiert die (für Objekt) Leistungsindikatoren sammeln. Sammeln Sie alle Leistungsindikatoren für das Objekt angeben: `.*`. Um nur Swap Space Leistungsindikatoren für das Objekt zu erfassen, können Sie Folgendes angeben:`.+Swap.+`
- **Intervall:**: die Häufigkeit, mit der Leistungsindikatoren gesammelt werden.

Die Standardkonfiguration für Leistungsmetriken ist:

```
<source>
  type oms_omi
  object_name "Physical Disk"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Logical Disk"
  instance_regex ".*
  counter_name_regex ".*"
  interval 5m
</source>

<source>
  type oms_omi
  object_name "Processor"
  instance_regex ".*
  counter_name_regex ".*"
  interval 30s
</source>

<source>
  type oms_omi
  object_name "Memory"
  instance_regex ".*"
  counter_name_regex ".*"
  interval 30s
</source>

```

### <a name="enable-mysql-performance-counters-using-linux-commands"></a>Aktivieren Sie MySQL-Leistungsindikatoren Linux Befehle

MySQL Server oder MariaDB Server auf dem Computer erkannt wird, wenn das Omsagent-Paket installiert ist, wird eine Systemmonitor-Anbieter für MySQL-Server installiert. Dieser Anbieter eine Verbindung zum lokalen MySQL-MariaDB Server Leistungsstatistiken verfügbar machen. Sie müssen MySQL Anmeldeinformationen so konfigurieren, dass der Anbieter der MySQL-Server zugreifen kann.

Um ein Standardbenutzerkonto für MySQL Server auf Localhost zu definieren, verwenden Sie den folgenden Befehl wird.

>[AZURE.NOTE] Die Datei muss das Omsagent-Konto lesbar sein. Ausführen des Befehls Mycimprovauth als Omsgent wird empfohlen.


```
sudo su omsagent -c '/opt/microsoft/mysql-cimprov/bin/mycimprovauth default 127.0.0.1 <username> <password>'

sudo service omiserverd restart
```


Auch Sie können die erforderlichen MySQL Anmeldeinformationen in einer Datei durch Erstellen der Datei: /var/opt/microsoft/mysql-cimprov/auth/omsagent/mysql-auth. Weitere Informationen zum Verwalten der MySQL-Anmeldeinformationen Überwachungskonto für die Mysql-Auth-Datei finden Sie unter [Verwalten von MySQL Überwachung Anmeldeinformationen in der Datei](#manage-mysql-monitoring-credentials-in-the-authentication-file).

Informationen zum Objektberechtigungen von MySQL-Benutzer von MySQL Server Leistungsdaten finden Sie in der [Datenbankberechtigungen für MySQL-Leistungsindikatoren erforderlich](#database-permissions-required-for-mysql-performance-counters) .

### <a name="enable-apache-http-server-performance-counters-using-linux-commands"></a>Aktivieren Sie Apache HTTP Server Leistungsindikatoren Linux Befehle

Apache HTTP Server auf dem Computer erkannt wird, wenn das Omsagent-Paket installiert ist, wird eine Systemmonitor-Anbieter für Apache HTTP Server automatisch installiert. Dieser Anbieter beruht auf einem Apache "Module", die in Apache HTTP Server geladen werden muss, um Performance-Daten zugreifen.

Laden Sie das Modul mit dem folgenden Befehl:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -c
```

Führen Sie den folgenden Befehl, um die Überwachung Apache-Modul entladen:

```
sudo /opt/microsoft/apache-cimprov/bin/apache_config.sh -u
```
### <a name="to-view-performance-data-with-log-analytics"></a>Anzeigen von Leistungsdaten mit Protokollanalyse

1. Klicken Sie im Portal Operations Management Suite Protokoll suchen.
2. Geben Sie in der Suchleiste `* (Type=Perf)` alle Leistungsindikatoren anzeigen.


Da OMS auch Windows-Leistungsindikatordaten gesammelt, sollten Sie die Suche auf Linux Daten Bereich ab. So würden dabei Leistungsdaten für eine Beispiel-Linux-Server mit dem Namen Chorizo21 angezeigt.

```
Type=Perf Computer=chorizo*
```

![in den Suchergebnissen angezeigten Beispielserver](./media/log-analytics-linux-agents/oms-perfsearch01.png)

In den Ergebnissen können Sie **Metriken** Anzeige für Daten die Leistungsindikatoren klicken. Echtzeit-Daten werden als Diagramme für jeden Zähler angezeigt.

![Metriken](./media/log-analytics-linux-agents/oms-perfmetrics01.png)


## <a name="syslog"></a>Syslog

Syslog ist ein Ereignis protokollieren wie Windows-Ereignisprotokolle, arbeiten beide entsprechend OMS angezeigt.

### <a name="to-add-a-new-linux-syslog-facility-in-oms"></a>Neue Linux Syslog-Einrichtung in OMS hinzufügen

1. Auf der Seite **Einstellungen** unter **Daten** **Syslog** auf und dann auf das Pluszeichen neben Geben Sie die Syslog-Funktion, die Sie hinzufügen möchten.
    ![Linux-syslog](./media/log-analytics-linux-agents/oms-linuxsyslog01.png)
2.  Wenn Sie den vollständigen Namen des Objekts kennen, können Sie beginnen, einen Teil des Namens eingeben und eine Liste der verfügbaren Syslog-Funktionen erscheint. Wenn Sie die Syslog-Funktion, die Sie hinzufügen möchten gefunden, klicken Sie in der Liste, und klicken Sie auf das Plussymbol, um die Syslog-Funktion hinzufügen.
3.  Nach dem Hinzufügen der Funktion erscheint in der Liste hervorgehoben mit farbigen Balken. Wählen Sie als Nächstes die Schweregrade (Informationskategorien Syslog Facility), die Sie sammeln möchten.
4.  Klicken Sie am unteren Rand der Seite auf **Speichern** , um die Änderung abzuschließen. Die Konfiguration vorgenommenen haben werden gesendet an alle OMS-Agenten für Linux OMS, normalerweise innerhalb von 5 Minuten registriert werden.


### <a name="configure-linux-syslog-facilities-in-linux"></a>Konfigurieren von Linux Syslog-Funktionen unter Linux

Syslog-Ereignisse werden von Syslog Daemon, z. B. Rsyslog oder Syslog-ng an einen lokalen Anschluss gesendet, denen der Agent überwacht. Standardmäßig Port 25224. Wenn der Agent installiert ist, wird eine Standardkonfiguration Syslog angewendet. Diese befindet sich unter:


Rsyslog: /etc/rsyslog.d/rsyslog-oms.conf

Syslog-ng: /etc/syslog-ng/syslog-ng.conf


Die Standardkonfiguration OMS-Agent Syslog uploads Syslog-Ereignisse mit einem Schweregrad von Warnung aus allen.

>[AZURE.NOTE] Bearbeiten von Syslog-Konfiguration muss Syslog Daemon für wirksam wird neu gestartet.

Syslog Standardkonfiguration für OMS-Agent für Linux für OMS ist:

#### <a name="rsyslog"></a>Rsyslog

```
kern.warning       @127.0.0.1:25224
user.warning       @127.0.0.1:25224
daemon.warning     @127.0.0.1:25224
auth.warning       @127.0.0.1:25224
syslog.warning     @127.0.0.1:25224
uucp.warning       @127.0.0.1:25224
authpriv.warning   @127.0.0.1:25224
ftp.warning        @127.0.0.1:25224
cron.warning       @127.0.0.1:25224
local0.warning     @127.0.0.1:25224
local1.warning     @127.0.0.1:25224
local2.warning     @127.0.0.1:25224
local3.warning     @127.0.0.1:25224
local4.warning     @127.0.0.1:25224
local5.warning     @127.0.0.1:25224
local6.warning     @127.0.0.1:25224
local7.warning     @127.0.0.1:25224
```

#### <a name="syslog-ng"></a>Syslog-ng

```
#OMS_facility = all
filter f_warning_oms { level(warning); };
destination warning_oms { tcp("127.0.0.1" port(25224)); };
log { source(src); filter(f_warning_oms); destination(warning_oms); };
```

### <a name="to-view-all-syslog-events-with-log-analytics"></a>Zeigen Sie alle Ereignisse an Syslog mit Protokollanalyse

1. Klicken Sie im Portal Operations Management Suite **Protokoll suchen** .
2. **Verwaltung** gruppieren wählen Sie eine vordefinierte Syslog-Suche, und wählen Sie eine auszuführende.

Dieses Beispiel zeigt alle Syslog-Ereignisse.

![Syslog Ereignisse im Protokoll suchen](./media/log-analytics-linux-agents/oms-linux-syslog.png)

Jetzt können Sie in Suchergebnissen anzeigen.

## <a name="linux-alerts"></a>Linux-Alarme

Wenn Sie Nagios oder Zabbix die Linux-Computer verwalten, erhalten OMS von diesen Tools generierten Warnungen. Es gibt derzeit keine Methode eingehende Warnung über das Portal OMS konfigurieren. Stattdessen müssen Sie eine Konfigurationsdatei starten Versand OMS bearbeiten.



### <a name="collect-alerts-from-nagios"></a>Alarme aus Nagios sammeln

Zum Sammeln von Alerts von Nagios Server müssen Sie die folgende Konfiguration ändern.

1. Der Benutzer **Omsagent** Leseberechtigungen gewähren der Nagios-Protokolldatei (z. B. /var/log/nagios/nagios.log/var/log/nagios/nagios.log). Vorausgesetzt, die Datei nagios.log gehört der Gruppe **Nagios** Benutzer **Omsagent** **Nagios** Gruppe hinzufügen können.

    ```
    sudo usermod –a -G nagios omsagent
    ```

2. Ändern Sie die Datei omsagent.confconfiguration (/ etc/opt/microsoft/omsagent/conf/omsagent.conf). Sicherstellen Sie, dass die folgenden Einträge vorhanden und nicht auskommentiert werden:

    ```
    <source>
    type tail
    #Update path to point to your nagios.log
    path /var/log/nagios/nagios.log
    format none
    tag oms.nagios
    </source>

    <filter oms.nagios>
    type filter_nagios_log
    </filter>
    ```

3. Starten Sie Omsagent-Daemon:

    ```
    sudo service omsagent restart
    ```

### <a name="collect-alerts-from-zabbix"></a>Alarme aus Zabbix sammeln

Zum Sammeln von Alerts von Zabbix Server werden ähnliche Schritte für Nagios oben ausführen nur einen Benutzer und ein Kennwort in *Klartext*angeben müssen. Dies ist nicht ideal, aber wahrscheinlich bald ändern. Zur Behebung dieses Problems wird empfohlen, den Benutzer erstellen und diese Berechtigung nur überwachen.

Ein Beispiel-Abschnitt der Konfigurationsdatei omsagent.conf (/ etc/opt/microsoft/omsagent/conf/omsagent.conf) für Zabbix sollte wie folgt aussehen:

```
<source>
  type zabbix_alerts
  run_interval 1m
  tag oms.zabbix
  zabbix_url http://localhost/zabbix/api_jsonrpc.php
  zabbix_username Admin
  zabbix_password zabbix
</source>

```

### <a name="view-alerts-in-log-analytics-search"></a>Anzeigen von Benachrichtigungen Protokollanalyse Suche

Nachdem Sie Linux Computern OMS Benachrichtigungen konfiguriert haben, können einige einfache Protokoll Suchabfragen Sie die Warnungsansicht. Suche Abfragebeispiel gibt die aufgezeichnete Alarme generiert wurden. Beispielsweise tritt eine Art von Problem in Ihrer IT-Infrastruktur, dann Ergebnisse für die folgende Beispielabfrage möglicherweise, wo das Problem stammen könnten. Und Sie können problemlos Bohren, Alarme Quellsystem zur Einschränkung der Untersuchung. Der Vorteil ist, dass Sie nicht unbedingt verschiedene Management-Systeme ab gehen – vorausgesetzt, Ihre Alerts OMS gesendet werden, können Sie es starten.

```
Type=Alert
```

#### <a name="to-view-all-nagios-alerts-with-log-analytics"></a>Alle Nagios Warnungen Protokollanalyse anzeigen
1. Klicken Sie im Portal Operations Management Suite **Protokoll suchen** .
2. Geben Sie in der Leiste Abfrage der folgenden Abfrage

    ```
    Type=Alert SourceSystem=Nagios
    ```
![Nagios Warnung im Protokoll suchen angezeigt](./media/log-analytics-linux-agents/oms-linux-nagios-alerts.png)

Nachdem der Suchergebnisse können Sie weitere Details wie *AlertState*anzeigen.

### <a name="to-view-all-zabbix-alerts-with-log-analytics"></a>Alle Zabbix Warnungen Protokollanalyse anzeigen
1. Klicken Sie im Portal Operations Management Suite **Protokoll suchen** .
2. Geben Sie in der Leiste Abfrage der folgenden Abfrage

    ```
    Type=Alert SourceSystem=Zabbix
    ```
![Zabbix Warnung im Protokoll suchen angezeigt](./media/log-analytics-linux-agents/oms-linux-zabbix-alerts.png)

Nachdem der Suchergebnisse können Sie weitere Details wie *repeatCount*anzeigen.


## <a name="compatibility-with-system-center-operations-manager"></a>Kompatibilität mit System Center Operations Manager

OMS-Agenten für Linux teilt Agent Binärdateien mit System Center Operations Manager-Agent. Installation der OMS-Agent für Linux auf einem System gerade von Operations Manager verwaltet aktualisiert OMI und SCX-Pakete auf dem Computer auf eine neuere Version. Der OMS-Agent für Linux und System Center 2012 R2 sind kompatibel. Allerdings **System Center 2012 SP1 und früheren Versionen sind derzeit nicht unterstützten oder kompatibel mit dem OMS-Agent für Linux.**

>[AZURE.NOTE] Wenn der OMS-Agent für Linux auf einem Computer, der zurzeit nicht von Operations Manager verwaltet wird installiert und später den Computer mit Operations Manager verwalten möchten, müssen Sie vor der Computerermittlung OMI Konfiguration ändern. **Dieser Schritt ist nicht erforderlich, wenn der Operations Manager-Agent für Linux vor der OMS-Agent installiert ist.**

### <a name="to-enable-the-oms-agent-for-linux-to-communicate-with-operations-manager"></a>OMS-Agenten für Linux Kommunikation mit Operations Manager aktivieren

1. Bearbeiten Sie die Datei /etc/opt/omi/conf/omiserver.conf
2. Stellen Sie sicher, dass die Zeile beginnt mit **Httpsport =** Port 1270 definiert. Wie`httpsport=1270`
3. Starten Sie den Server OMI:

    ```
    service omiserver restart or systemctl restart omiserver
    ```




## <a name="database-permissions-required-for-mysql-performance-counters"></a>Datenbankberechtigungen für MySQL-Leistungsindikatoren

Eine Überwachung MySQL-Benutzer zu gewähren, müssen gewährt die Berechtigung GRANT Option"sowie das Privileg.

Damit Benutzer MySQL zurückzugebenden Daten den Benutzer benötigen Zugriff auf die folgenden Abfragen:

```
SHOW GLOBAL STATUS;
SHOW GLOBAL VARIABLES:
```

Neben dieser Abfragen der MySQL muss Benutzer SELECT-Zugriff auf die folgenden Standardtabellen:

- INFORMATION_SCHEMA
- MySQL

Diese Berechtigungen können erteilt werden, durch Ausführen der folgenden Befehle erteilen.

```
GRANT SELECT ON information_schema.* TO ‘monuser’@’localhost’;
GRANT SELECT ON mysql.* TO ‘monuser’@’localhost’;
```

## <a name="manage-mysql-monitoring-credentials-in-the-authentication-file"></a>Verwalten von MySQL Überwachung Anmeldeinformationen in der Datei

In den folgenden Abschnitten können Sie MySQL Anmeldeinformationen verwalten.

### <a name="configure-the-mysql-omi-provider"></a>MySQL OMI Anbieter konfigurieren

MySQL OMI Anbieter erfordert einen vorkonfigurierten MySQL-Benutzer und MySQL-Client-Bibliotheken Abfragen die Performance-Diagnose-Informationen aus der MySQL-Instanz installiert.

### <a name="mysql-omi-authentication-file"></a>MySQL OMI Datei

MySQL OMI Anbieter verwendet eine Datei welche Bind-Adresse und port MySQL Instanz überwacht und welche Anmeldeinformationen um Metriken zu verwenden. Während der Installation MySQL OMI Anbieter MySQL my.cnf Konfigurationsdateien (Standardspeicherorte) Bind-Adresse und Port scan und teilweise OMI MySQL Datei festgelegt.

Zum Abschließen einer MySQL-Server-Instanz überwachen fügen Sie eine vorgenerierte MySQL OMI Datei in das richtige Verzeichnis hinzu.

### <a name="authentication-file-format"></a>Authentifizierung-Dateiformat

Die MySQL OMI Datei ist eine Textdatei, die Informationen über:

- Anschluss
- BIND-Adresse
- MySQL-Benutzername
- Base64-codiertes Kennwort

Die Datei MySQL OMI gewährt Berechtigungen für Lese-/Schreibzugriff nur Linux-Benutzer, die es generiert.

```
[Port]=[Bind-Address], [username], [Base64 encoded Password]
(Port)=(Bind-Address), (username), (Base64 encoded Password)
(Port)=(Bind-Address), (username), (Base64 encoded Password)
AutoUpdate=[true|false]
```

Standard MySQL OMI Datei enthält eine Standardinstanz und eine Anschlussnummer, je nachdem, welche Informationen verfügbar und analysierten aus der gefundenen MySQL-Konfigurationsdatei.

Die Standardinstanz ist zu verwalten mehrerer MySQL-Instanzen auf einem Linux-Server einfacher und Instanz Port 0 bezeichnet. Alle hinzugefügte Instanzen erbt Eigenschaften von der Standardinstanz. Beispielsweise wenn MySQL-auf Port '3308 Instanz' der Standardinstanz Bind-Adresse, Benutzername und Base64-codierte Kennwort dienen zu überwachen die Instanz auf 3308. Wenn die Instanz auf 3308 eine andere Adresse und den gleichen MySQL Benutzername und Kennwort verwendet der Respecification der Bind-Adresse ist erforderlich, die anderen Eigenschaften geerbt.

Beispiele für die Datei folgendermaßen aussehen.

Standardinstanz und mit Port 3308:

```
0=127.0.0.1, myuser, cnBwdA==3308=, ,AutoUpdate=true
```

Standardinstanz und mit Port 3308 + verschiedene Base64 codiert Kennwort:

```
0=127.0.0.1, myuser, cnBwdA==3308=127.0.1.1, , AutoUpdate=true
```


| **Eigenschaft** | **Beschreibung** |
| --- | --- |
| Anschluss | Anschluss stellt den aktuellen Anschluss, dem MySQL-Instanz überwacht.  Port 0 bedeutet, dass die folgenden Eigenschaften für die Standardinstanz verwendet werden. |
| BIND-Adresse | Binden der aktuellen MySQL Bind-Adresse ist |
| Benutzername | Dieser Benutzername der MySQL-Benutzer die MySQL-Server-Instanz überwachen möchten. |
| Base64-codiertes Kennwort | Dies ist das Kennwort in Base64 codiert Überwachung MySQL-Benutzer. |
| AutoUpdate | Bei MySQL OMI Anbieter der Anbieter suchen in der my.cnf-Datei Scannen und überschreiben die Datei OMI MySQL. Legen Sie dieses Flag auf true oder false je nach erforderlichen Updates für die MySQL OMI Datei. |

#### <a name="authentication-file-location"></a>Speicherort der Authentifizierung

Die Datei OMI MySQL sollte am folgenden Speicherort und mit dem Namen "Mysql-Auth":

/var/OPT/Microsoft/MySQL-cimprov/AUTH/omsagent/MySQL-auth

Die Datei (und Auth-Omsagent Verzeichnis) sollte der Omsagent Benutzer gehören.

## <a name="agent-logs"></a>Agentenprotokolle

Die Protokolle für den OMS-Agent für Linux ist:

/ Var/opt/Microsoft/Omsagent/Log /

Die Protokolle für den OMS-Agent für Linux für Omsconfig (Agentenkonfiguration) Programm ist:

/ Var/opt/Microsoft/Omsconfig/Log /

Protokolle für OMI und SCX (die Performance Metrikdaten) ist:

/ Var/opt/Omi/Log/und /var/opt/microsoft/scx/log

## <a name="troubleshooting-the-oms-agent-for-linux"></a>Problembehandlung bei der OMS-Agent für Linux

Verwenden Sie die folgende Informationen diagnostizieren und beheben Probleme.

Wenn keine Informationen zur Problembehandlung in diesem Abschnitt hilft auch können die folgenden Ressourcen Sie um das Problem zu beheben.

- Kunden mit Premier Support eine Supportanfrage über [Premier](https://premier.microsoft.com/) anmelden kann
- Kunden mit Supportverträgen Azure können Support-Anfragen in [Azure-Portal](https://manage.windowsazure.com/?getsupport=true) anmelden.
- Datei ein [GitHub Problem](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)
- Bewertungsportal Ideen und erstellen Sie einen Fehler melden [http://aka.ms/opinsightsfeedback](http://aka.ms/opinsightsfeedback)

### <a name="important-log-locations"></a>Wichtige Protokolldatei

Datei | Pfad
---- | -----
OMS-Agenten für Linux-Protokolldatei | `/var/opt/microsoft/omsagent/log/omsagent.log `
OMS-Konfiguration Protokolldatei | `/var/opt/microsoft/omsconfig/omsconfig.log`

### <a name="important-configuration-files"></a>Wichtige Dateien

Kategorie | Speicherort
----- | -----
Syslog | `/etc/syslog-ng/syslog-ng.conf`oder `/etc/rsyslog.conf` oder`/etc/rsyslog.d/95-omsagent.conf`
Leistung, Nagios, Zabbix, OMS-Ausgabe und Allgemein | `/etc/opt/microsoft/omsagent/conf/omsagent.conf`
Zusätzliche Konfigurationen | `/etc/opt/microsoft/omsagent/conf.d/*.conf`

>[AZURE.NOTE] Bearbeiten der Konfigurationsdateien für Leistungsindikatoren und Syslog werden überschrieben, wenn OMS-Portal-Konfiguration aktiviert ist. Konfiguration im OMS-Portal (alle Knoten) deaktivieren oder für einzelne Knoten durch Folgendes ausführen:

```
sudo su omsagent -c /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable
```


### <a name="enable-debug-logging"></a>Debug-Protokollierung aktivieren

Zum Aktivieren der Debugprotokollierung können Sie OMS Ausgabe Plugin und ausführliche Ausgabe.

#### <a name="oms-output-plugin"></a>OMS-Ausgabe-plugin

FluentD kann das Plugin Protokollierungsstufen für verschiedene Protokollebenen für ein- und Ausgaben an. Um eine andere Ebene für OMS Ausgabe anzugeben, bearbeiten Sie allgemeine Agentenkonfiguration in der `/etc/opt/microsoft/omsagent/conf/omsagent.conf` Datei.

Am Ende der Konfigurationsdatei ändern die `log_level` -Eigenschaft von `info` , `debug`.

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

Debug-Protokollierung ermöglicht gespeicherten Uploads getrennt nach Typ, Anzahl der Datenelemente und Zeit senden OMS-Dienst finden Sie unter.

*Beispiel aktiviert Debug-Protokoll:*
```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

#### <a name="verbose-output"></a>Ausführliche Ausgabe
Anstatt OMS Ausgabe Plugin können auch ausgegeben Daten direkt an `stdout`, das in OMS-Agenten für Linux-Protokolldatei sichtbar ist.

In OMS Allgemein Agent-Konfigurationsdatei auf `/etc/opt/microsoft/omsagent/conf/omsagent.conf`, Auskommentieren der OMS output-Plug-in Hinzufügen einer `#` vor jeder Zeile.

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Unter Plugin Ausgabe entfernen Sie den Kommentar im Abschnitt Entfernen der `#` Symbol am Anfang jeder Zeile.

```
<match **>
  type stdout
</match>
```

### <a name="forwarded-syslog-messages-do-not-appear-in-the-log"></a>Weitergeleitete Syslog-Nachrichten werden nicht im Protokoll angezeigt.

#### <a name="probable-causes"></a>Mögliche Ursachen

- Die Konfiguration auf dem Linux-Server angewendet lässt nicht mehrere gesendeten Anlagen und/oder Protokollebenen
- Syslog wird nicht ordnungsgemäß auf dem Linux-Server weitergeleitet.
- Die Anzahl der Nachrichten pro Sekunde weitergeleitet sind zu groß für die Basiskonfiguration des OMS-Agenten für Linux behandeln

#### <a name="resolutions"></a>Auflösung

- Überprüfen Sie, ob die Konfiguration im Portal OMS Syslog alle Funktionen und die richtige Protokoll
  - **OMS-Portal > Settings > Daten > Syslog**
-  Überprüfen, systemeigene Syslog Daemons messaging (`rsyslog`, `syslog-ng`) weitergeleiteten Nachrichten empfangen
- Um sicherzustellen, dass Nachrichten nicht blockiert werden Firewalleinstellungen auf Syslog-server
-  Simulieren eine Nachricht Syslog OMS mit den `logger` Befehl - Beispiel:
  - `logger -p local0.err "This is my test message"`

### <a name="problems-connecting-to-oms-when-using-a-proxy"></a>Probleme mit OMS bei Verwendung eines Proxys

#### <a name="probable-causes"></a>Mögliche Ursachen

- Der Proxy angegeben beim Installieren und Konfigurieren des Agents falsch ist
- Die OMS-Endpunkte sind nicht Whitelistested im Rechenzentrum

#### <a name="resolutions"></a>Auflösung

- Installieren Sie den OMS-Agent für Linux verwenden den folgenden Befehl mit der Option `-v` aktiviert. Dadurch werden ausführliche Ausgabe des Agenten über Proxy an den OMS-Dienst.
  - `/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`
  - Lesen Sie die Dokumentation für OMS-Proxy an [den Agent für die Verwendung mit einem HTTP-Proxyserver konfigurieren](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#configuring-the-agent-for-use-with-an-http-proxy-server)
- Überprüfen Sie, ob die folgenden OMS Dienstendpunkte White

Agent-Ressource | Anschlüsse
---- | ----
& #42;. ODS.opinsights.Azure.com | Port 443
& #42;. OMS.opinsights.Azure.com | Port 443
ODS.systemcenteradvisor.com | Port 443
& #42;.blob.core.windows.net/ | Port 443

### <a name="a-403-error-is-displayed-when-onboarding"></a>Ein 403-Fehler wird angezeigt, wenn Onboarding

#### <a name="probable-causes"></a>Mögliche Ursachen

- Datum und Uhrzeit sind falsch auf Linux-Server
- Die Arbeitsplatz-ID und Arbeitsbereich Schlüssel sind falsch

#### <a name="resolution"></a>Auflösung

- Überprüfen Sie die Zeit auf dem Linux-Server mit der `date` Befehl. Wenn die Daten größer oder kleiner als 15 Minuten ab dem aktuellen Zeitpunkt, schlägt Onboarding. Aktualisieren Sie zum Beheben dieses das Datum und/oder die Zeitzone des Linux Servers ein.
- Die neueste Version des OMS-Agenten für Linux benachrichtigt, wenn ein Zeitunterschied Onboarding fehlschlagen
- RE-integrierte mit der richtigen Arbeitsbereich ID und Arbeitsbereich Schlüssel. Weitere Informationen finden Sie unter [Onboarding mithilfe der Befehlszeile](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) .

### <a name="a-500-error-or-404-error-appears-in-the-log-file-after-onboarding"></a>Fehler 500 oder 404-Fehler in der Protokolldatei nach Onboarding angezeigt

Dies ist ein bekanntes Problem, das beim ersten Upload von Linux-Daten in einem Arbeitsbereich OMS auftritt. Dies wirkt sich nicht auf Daten oder andere Probleme aus. Fehler beim zunächst ignorieren Onboarding.

### <a name="nagios-data-does-not-appear-in-the-oms-portal"></a>Nagios Daten werden nicht in die OMS-Portal angezeigt.

#### <a name="probable-causes"></a>Mögliche Ursachen
- Omsagent-Benutzer ist nicht berechtigt, lesen Sie in der Protokolldatei Nagios
- Die Abschnitte Nagios Quelle und Filter sind in der Datei omsagent.conf noch kommentiert

#### <a name="resolutions"></a>Auflösung

- Fügen Sie den Benutzer Omsagent aus der Nagios-Datei lesen. Weitere Informationen finden Sie unter [Alerts Nagios](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts) .
- OMS-Agent für Linux Allgemein Konfigurationsdatei auf `/etc/opt/microsoft/omsagent/conf/omsagent.conf`, sicherzustellen, dass **sowohl** die Quelle Nagios und Filter Abschnitte enthalten Kommentare entfernt, wie im folgenden Beispiel.

```
<source>
  type tail
  path /var/log/nagios/nagios.log
  format none
  tag oms.nagios
</source>

<filter oms.nagios>
  type filter_nagios_log
</filter>
```


### <a name="linux-data-doesnt-appear-in-the-oms-portal"></a>Linux-Daten nicht in die OMS-Portal angezeigt.

#### <a name="probable-causes"></a>Mögliche Ursachen

- Onboarding OMS-Dienst fehlgeschlagen
- Verbindung mit dem OMS-Dienst blockiert
- Der OMS-Agent für Linux-Daten wird unterstützt

#### <a name="resolutions"></a>Auflösung

- Überprüfen Sie diese Onboarding OMS-Dienst wurde erfolgreich überprüft wurde, ob die `/etc/opt/microsoft/omsagent/conf/omsadmin.conf` vorhanden ist.
- RE-integrierten omsadmin.sh verwenden. Weitere Informationen finden Sie unter [Onboarding mithilfe der Befehlszeile](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) .
- Wenn Sie einen Proxy verwenden, Problembehandlung Schritte Proxy verwenden
- In einigen Fällen Wenn der OMS-Agent für Linux mit OMS-Dienst kommunizieren kann Daten auf dem Agent auf vollständige Puffergröße 50 MB gesichert. Starten der OMS-Agent für Linux entweder die `service omsagent restart` oder `systemctl restart omsagent` Befehle.
  >[AZURE.NOTE] Dieses Problem wurde in Agent Version 1.1.0-28 und höher behoben.

### <a name="syslog-linux-performance-counter-configuration-is-not-applied-in-the-oms-portal"></a>Syslog Linux Performance Counter Konfiguration gilt nicht im OMS-portal

#### <a name="probable-causes"></a>Mögliche Ursachen

- Der Agent im OMS-Agent für Linux hat nicht die neueste Konfiguration OMS-Portal abgerufen.
- Die geänderten Einstellungen im Portal werden nicht angewendet.

#### <a name="resolutions"></a>Auflösung

`omsconfig`der Agent im OMS-Agent wird für Linux, die OMS Portalkonfiguration ändert alle 5 Minuten abgerufen. Diese Konfiguration wird angewendet auf den OMS-Agenten für Linux-Konfigurationsdateien entfernt `/etc/opt/microsoft/omsagent/conf/omsagent.conf`.

- In einigen Fällen der OMS-Agent für Linux-Konfigurations-Agent mit der Portalkonfiguration Dienst resultierende in der aktuellen Konfiguration nicht angewendet möglicherweise nicht.
- Überprüfen Sie, ob die `omsconfig` mit folgendem Agent installiert ist:
  - `dpkg --list omsconfig`oder`rpm -qi omsconfig`
  - Falls nicht installiert, installieren Sie die neueste Version des OMS für Linux

- Überprüfen Sie, ob die `omsconfig` Agent mit OMS-Dienst kommunizieren kann
  - Führen Sie die `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` Befehl
    - Der obige Befehl gibt der Konfiguration der Agent ruft über das Portal einschließlich Syslog Einstellungen, Linux-Leistungsindikatoren und benutzerdefinierte Protokolle
    - Wenn der obige Befehl fehlschlägt, führen Sie die `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` Befehl. Dieser Befehl zwingt den Agenten Omsconfig Kommunikation mit OMS-Dienst abrufen die aktuelle Konfiguration.


### <a name="custom-linux-log-data-does-not-appear-in-the-oms-portal"></a>Benutzerdefinierte Linux-Protokolldaten erscheint nicht im OMS-Portal

#### <a name="probable-causes"></a>Mögliche Ursachen

- Onboarding OMS-Dienst fehlgeschlagen
- Die Einstellung **übernehmen die folgende Konfiguration auf Linux Servern** wurde nicht ausgewählt
- Omsconfig hat die neuesten benutzerdefiniertes Protokoll über das Portal nicht abgeholt.
- Die `omsagent` wird auf dem benutzerdefinierten Protokoll durch ein Berechtigungsproblem oder `omsagent` wurde nicht gefunden. In diesem Fall wird die folgende Ausgabe angezeigt:
  - `[DATETIME] [warn]: file not found. Continuing without tailing it.`
  - `[DATETIME] [error]: file not accessible by omsagent.`
- Dies ist ein bekanntes Problem mit die Wettlaufsituation OMS-Agent für Linux Version 1.1.0-217 behoben wurde

#### <a name="resolutions"></a>Auflösung
- Überprüfen, ob diesem, erfolgreich ermittelt haben, ob die `/etc/opt/microsoft/omsagent/conf/omsadmin.conf` existiert.
  - Falls erforderlich, erneut mithilfe der Befehlszeile omsadmin.sh an Bord. Weitere Informationen finden Sie unter [Onboarding mithilfe der Befehlszeile](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line) .
- OMS-Portal unter **Einstellungen** auf der Registerkarte **Daten** sicher, dass die Einstellung **gelten folgende Konfiguration Linux Servern** aktiviert ist  
  ![Konfiguration übernehmen](./media/log-analytics-linux-agents/customloglinuxenabled.png)

- Überprüfen Sie, ob die `omsconfig` Agent mit OMS-Dienst kommunizieren kann
  - Führen Sie die `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'` Befehl
  - Der obige Befehl gibt der Konfiguration der Agent ruft über das Portal einschließlich Syslog Einstellungen, Linux-Leistungsindikatoren und benutzerdefinierte Protokolle
  - Wenn der obige Befehl fehlschlägt, führen Sie die `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py` Befehl. Dieser Befehl zwingt den Agenten Omsconfig Kommunikation mit OMS-Dienst und die neueste Konfiguration abrufen.


Statt den OMS-Agenten für linuxbenutzer als privilegierter Benutzer `root`, OMS-Agenten für Linux wird als das `omsagent` Benutzer. In den meisten Fällen muss explizit Berechtigungen der Benutzer erteilt werden bestimmte Dateien lesen.

Berechtigung auf `omsagent` Benutzer führen Sie die folgenden Befehle:

1. Hinzufügen der `omsagent` Benutzer mit einer bestimmten Gruppe`sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Universelle Lesezugriff auf die erforderliche Datei mit`sudo chmod -R ugo+rw <FILE DIRECTORY>`

Es ist ein bekanntes Problem mit die Wettlaufsituation OMS-Agent für Linux Version 1.1.0-217 behoben wurde. Nach dem Aktualisieren auf die neueste Agent führen Sie den folgenden Befehl auf die neueste Version des Plugins Ausgabe:

```
sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/conf/omsagent.conf
```

## <a name="known-limitations"></a>Einschränkungen
Überprüfen Sie in den folgenden Abschnitten erfahren Sie aktuelle Grenzen des OMS-Agenten für Linux.

### <a name="azure-diagnostics"></a>Azure-Diagnose

Für virtuelle Linux-Computer in Azure ausgeführt zusätzliche Schritte müssen Daten zu Azure-Diagnose und Operations Management Suite. Diagnose-Erweiterung für Linux **Version 2.2** ist erforderlich für die Kompatibilität mit der OMS-Agent für Linux.

Weitere Informationen zur Installation und Konfiguration von Diagnose-Erweiterung für Linux finden Sie unter [Verwenden der Azure-CLI-Befehl Linux Diagnose Erweiterung aktivieren](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md#use-the-azure-cli-command-to-enable-the-linux-diagnostic-extension).

**Aktualisieren der Linux-Diagnose-Erweiterung von 2.0 2.2 Azure CLI asm:**

```
azure vm extension set -u <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.0
azure vm extension set <vm_name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

**ARM**

```
azure vm extension set -u <resource-group> <vm-name> Microsoft.Insights.VMDiagnosticsSettings Microsoft.OSTCExtensions 2.0
azure vm extension set <resource-group> <vm-name> LinuxDiagnostic Microsoft.OSTCExtensions 2.2 --private-config-path PrivateConfig.json
```

Beispiele Befehl verweisen eine Datei namens PrivateConfig.json. Das Format dieser Datei sollte das folgende Beispiel ähneln.

```
    {
    "storageAccountName":"the storage account to receive data",
    "storageAccountKey":"the key of the account"
    }
```

### <a name="sysklog-is-not-supported"></a>Sysklog wird nicht unterstützt.

Rsyslog oder Syslog-ng müssen Syslog-Nachrichten sammeln. Der standardmäßige Syslog Daemon auf Version 5 von Red Hat Enterprise Linux CentOS und Oracle Linux Version (Sysklog) ist für Syslog-Ereignissammlung nicht unterstützt. Syslog aus dieser Version dieser Distributionen Datensammlung, sollten Rsyslog Daemon installiert und konfiguriert Sysklog ersetzen. Weitere Informationen zum Austauschen von Sysklog mit Rsyslog finden Sie in der [neu erstellten Rsyslog RPM installiert](http://wiki.rsyslog.com/index.php/Rsyslog_on_CentOS_success_story#Install_the_newly_built_rsyslog_RPM).

## <a name="next-steps"></a>Nächste Schritte

- [Protokollanalyse hinzufügen Lösungen Lösungskatalog](log-analytics-add-solutions.md) Funktionen und Daten.
- Vertraut mit [Protokoll suchen](log-analytics-log-searches.md) Solutions ausführlichen Informationen anzeigen.
- Verwenden Sie [Dashboards](log-analytics-dashboards.md) zu speichern eigene Suchvorgänge.
