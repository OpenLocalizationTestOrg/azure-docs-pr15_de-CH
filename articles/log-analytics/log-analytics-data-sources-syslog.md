<properties 
   pageTitle="Syslog-Nachrichten Protokollanalyse | Microsoft Azure"
   description="Syslog ist ein Ereignis protokollieren für Linux.   Dieser Artikel beschreibt die Protokollanalyse und Datensätze im Repository OMS erstellten Auflistung von Syslog-Nachrichten konfigurieren."
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
   ms.date="09/06/2016"
   ms.author="bwren" />


# <a name="syslog-data-sources-in-log-analytics"></a>Syslog-Datenquellen in Protokollanalyse

Syslog ist ein Ereignis protokollieren für Linux.  Anwendung sendet Nachrichten, die auf dem lokalen Computer gespeichert oder eine Syslog-Kollektor übermittelt.  OMS-Agent für Linux installiert ist, wird den lokalen Syslog Daemon zum Weiterleiten von Nachrichten an den Agent konfiguriert.  Der Agent sendet die Nachricht dann an Protokollanalyse, ein entsprechender Datensatz in der OMS-Repository erstellt wird.  

> [AZURE.NOTE]Protokollanalyse unterstützt mehrere Nachrichten von Rsyslog oder ng Syslog. Der standardmäßige Syslog Daemon auf Version 5 von Red Hat Enterprise Linux CentOS und Oracle Linux Version (Sysklog) ist für Syslog-Ereignissammlung nicht unterstützt. Syslog aus dieser Version dieser Distributionen Datensammlung, sollten [Rsyslog Daemon](http://rsyslog.com) installiert und konfiguriert Sysklog ersetzen.

![Syslog-Auflistung](media/log-analytics-data-sources-syslog/overview.png)


## <a name="configuring-syslog"></a>Konfigurieren von Syslog
OMS-Agenten für Linux erfasst nur mit Funktionen und Schweregrade, die in der Konfiguration angegeben werden.  Sie können Syslog OMS-Portal oder Konfigurationsdateien auf Linux-Agents verwalten.


### <a name="configure-syslog-in-the-oms-portal"></a>Syslog OMS-Portal konfigurieren

Konfigurieren Sie Syslog aus dem [Menü Daten in Analytics Einstellungen](log-analytics-data-sources.md#configuring-data-sources).  Diese Konfiguration ist die Konfigurationsdatei auf jedem Linux-Agent übermittelt.

Sie können eine neue Anlage im Namen eingeben und auf **+**.  Für jede Anlage werden nur Nachrichten mit ausgewählten Schweregrade gesammelt.  Überprüfen Sie die Schweregrade für die Anlage, die Sie sammeln möchten.  Sie können keine weiteren Kriterien zum Filtern von Nachrichten angeben.

![Syslog konfigurieren](media/log-analytics-data-sources-syslog/configure.png)


Standardmäßig werden alle Neukonfiguration automatisch an alle Agenten abgelegt.  Wenn Sie Syslog für jeden Linux-Agent manuell konfigurieren möchten, deaktivieren Sie das *Übernehmen unter Konfiguration auf Linux-Computern*aus.


### <a name="configure-syslog-on-linux-agent"></a>Konfigurieren von Syslog auf Linux-agent

Wenn [OMS-Agent auf einem Linux-Client installiert ist](log-analytics-linux-agents.md), wird es installiert Syslog-Standardkonfigurationsdatei, die definiert der Einrichtung und des Schweregrads Nachrichten gesammelt werden.  Sie können diese Datei, um die Konfiguration zu ändern.  Die Konfigurationsdatei ist abhängig von Syslog Daemon, den den Client installiert hat.

> [AZURE.NOTE] Bearbeiten von Syslog-Konfiguration muss Syslog Daemon für wirksam wird neu gestartet.

#### <a name="rsyslog"></a>rsyslog

Die Konfigurationsdatei für Rsyslog ist unter **/etc/rsyslog.d/95-omsagent.conf**.  Nachfolgend den Standardinhalt.  Syslog-Nachrichten für alle mit Stufe Warnung vom lokalen Agent gesendet werden gesammelt.

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

Der Abschnitt der Konfigurationsdatei entfernen, um eine zu entfernen.  Sie können die Schweregrade, die für eine bestimmte Anlage erfasst werden, indem die Anlage Eintrag ändern.  Würde ändern Sie Begrenzung Anlage Benutzer Nachrichten mit dem Schweregrad Fehler oder höher, der Datei die folgende Zeile:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>Syslog-ng

Die Konfigurationsdatei für Rsyslog befindet sich unter **/etc/syslog-ng/syslog-ng.conf**.  Nachfolgend den Standardinhalt.  Syslog Nachrichten aus dem lokalen Agent für alle und alle Schweregrade werden gesammelt.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };
    
    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };
    
    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };
    
    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };
    
    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };
    
    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };
    
    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Der Abschnitt der Konfigurationsdatei entfernen, um eine zu entfernen.  Sie können die Schweregrade, die für einen bestimmten Standort erfasst werden, indem Sie aus der Liste entfernen.  Würde ändern Sie um Benutzer Einrichtung nur Warnung und wichtige Nachrichten zu beschränken, diesen Abschnitt der Konfigurationsdatei wie folgt:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>Syslog-Port ändern

Der OMS-Agent überwacht Syslog-Nachrichten auf dem lokalen Client am Port 25224.  Der OMS-Agent-Konfigurationsdatei am **/etc/opt/microsoft/omsagent/conf/omsagent.conf**Abschnitt hinzufügen können Sie diesen Port ändern.  Ersetzen Sie 25224 in **der Einreise** durch die gewünschte Portnummer.  Beachten Sie, dass Sie auch die Konfigurationsdatei für den Syslog Daemon Nachrichten an diesen Port ändern müssen.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Datensammlung

Der OMS-Agent überwacht Syslog-Nachrichten auf dem lokalen Client am Port 25224. Die Konfigurationsdatei für den Syslog Daemon leitet Syslog-Nachrichten von Anwendung zu diesem Port, wo sie von Protokollanalyse erfasst werden.


## <a name="syslog-record-properties"></a>Eigenschaften von syslog

Syslog-Einträge haben ein **Syslog** und Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| Computer | Computer, auf dem das Ereignis entnommen wurde. |
| Anlage | Definiert den Teil des Systems, die die Meldung generiert hat. |
| HostIP | IP-Adresse des Systems, das Senden der Nachricht.  |
| HostName | Name des Systems, das Senden der Nachricht. |
| SeverityLevel | Schweregrad des Ereignisses. |
| SyslogMessage | Text der Nachricht. |
| Prozess-ID | Die ID des Prozesses, der die Meldung generiert hat. |
| EventTime | Datum und Uhrzeit, zu der das Ereignis generiert wurde.



## <a name="log-queries-with-syslog-records"></a>Abfragen mit Syslog protokollieren

Die folgende Tabelle bietet unterschiedliche Beispiele für Protokolldateien Abfragen, die syslog Datensätze abrufen.

| Abfrage | Beschreibung |
|:--|:--|
| Typ = Syslog | Alle Syslogs. |
| Typ = Syslog SeverityLevel = Fehler | Alle Syslog-Datensätze mit dem Schweregrad des Fehlers. |
| Typ = Syslog & #124; count() Computer messen | Anzahl der Syslog-Datensätze vom Computer. |
| Typ = Syslog & #124; Measure count() Einrichtung | Anzahl der Syslog Datensätze nach Anlage. |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Daten aus Datenquellen und Solutions analysieren. 
- Verwenden Sie [Benutzerdefinierte Felder](log-analytics-custom-fields.md) zum Analysieren von Daten aus Datensätzen Syslog in einzelne Felder.
- [Konfigurieren von Linux-Agenten](log-analytics-linux-agents.md) andere Datentypen gesammelt. 
