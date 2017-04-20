<properties
   pageTitle="IIS-Protokolle Protokollanalyse | Microsoft Azure"
   description="Internet Information Services (IIS) speichert Benutzeraktivität in Protokolldateien Protokollanalyse erfasst werden können.  Dieser Artikel beschreibt mehrere IIS-Protokolle und Details des in OMS-Repository erstellten Datensätze konfigurieren."
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

# <a name="iis-logs-in-log-analytics"></a>IIS-Protokolle Protokollanalyse
Internet Information Services (IIS) speichert Benutzeraktivität in Protokolldateien Protokollanalyse erfasst werden können.  

![IIS-Protokolle](media/log-analytics-data-sources-iis-logs/overview.png)

## <a name="configuring-iis-logs"></a>Konfigurieren von IIS protokolliert
Protokollanalyse erfasst Einträge von Protokolldateien von IIS, so Sie [IIS für die Protokollierung konfigurieren müssen](https://technet.microsoft.com/library/hh831775.aspx).

Protokollanalyse nur unterstützt IIS-Protokolldateien im W3C-Format gespeichert und benutzerdefinierte Felder oder IIS Advanced Logging nicht unterstützt.  
Protokollanalyse erfasst keine Protokolle im systemeigenen Format NCSA oder IIS.

Konfigurieren Sie IIS-Protokolle in Protokollanalyse aus dem [Menü Daten in Analytics Einstellungen](log-analytics-data-sources.md#configuring-data-sources).  Es ist keine Konfiguration erforderlich als auswählen **gesammelt W3C-Format IIS-Protokolldateien**.

Empfohlen, wenn Sie IIS Log zu aktivieren, die Einstellung IIS Rollover auf jedem Server konfigurieren sollte.


## <a name="data-collection"></a>Datensammlung

Protokollanalyse erfasst IIS-Protokolleinträge aus jeder Quelle verbundenen ca. 15 Minuten.  Der Agent zeichnet stattdessen in jedes Ereignisprotokoll aus erfasst.  Wenn der Agent offline geht, sammelt dann Protokollanalyse Ereignisse aus dem letzten, auch wenn diese Ereignisse erstellt wurden, während der Agent offline war.


## <a name="iis-log-record-properties"></a>Eigenschaften von IIS log

IIS-Protokolleinträge ein **W3CIISLog** und die Eigenschaften in der folgenden Tabelle:

| Eigenschaft | Beschreibung |
|:--|:--|
| Computer | Name des Computers, der das Ereignis entnommen wurde. |
| Überweisung | IP-Adresse des Clients. |
| csMethod | Methode der Anforderung GET oder POST. |
| csReferer | Website, dass der Benutzer einen Link zur aktuellen Website folgen. |
| csUserAgent | Der Browsertyp des Clients. |
| csUserName | Name des authentifizierten Benutzers, der Zugriff auf den Server. Anonyme Benutzer werden durch einen Bindestrich angegeben. |
| csUriStem | Ziel der Anforderung wie einer Webseite. |
| csUriQuery | Abfrage ggf., dass der Client versuchte auszuführen. |
| "Verwaltungsgruppenname" | Name der Verwaltungsgruppe für Operations Manager-Agents.  Für andere Agenten ist AOI -\<Arbeitsplatz-ID\> |
| RemoteIPCountry | Land der IP-Adresse des Clients. |
| RemoteIPLatitude | Breite der Client-IP-Adresse. |
| RemoteIPLongitude | Länge der Client-IP-Adresse. |
| scStatus | HTTP-Statuscode. |
| scSubStatus | Untergeordneter Fehlercode. |
| scWin32Status | Windows-Statuscode. |
| sIP | IP-Adresse des Webservers. |
| SourceSystem  | OpsMgr |
| sPort | Port auf dem Server Client verbunden. |
| sSiteName | Name der IIS-Website. |
| TimeGenerated | Datum und Uhrzeit der Eintrag protokolliert wurde. |
| TimeTaken | Zeitdauer für die Anforderung in Millisekunden. |

## <a name="log-searches-with-iis-logs"></a>Protokolldatei suchen mit IIS-Protokolle

Die folgende Tabelle bietet unterschiedliche Beispiele für Abfragen protokollieren, die Protokolleinträge von IIS abrufen.

| Abfrage | Beschreibung |
|:--|:--|
| Typ = IISLog | Alle IIS-Protokolleinträge. |
| Typ = IISLog EventLevelName = Fehler | Alle Windows-Ereignisse mit Schweregrad des Fehlers. |
| Typ = W3CIISLog & #124; Überweisung count() messen | Anzahl von IIS Protokolleinträge von Client-IP-Adresse. |
| Typ = W3CIISLog CsHost = "www.contoso.com" & #124; Count() CsUriStem messen | Anzahl von IIS Protokolleinträge URL für den Host www.contoso.com. |
| Typ = W3CIISLog & #124; Messen Sie Sum(csBytes) Computer & #124; Top 500000| Von jedem IIS-Computer empfangene Bytes insgesamt. |

## <a name="next-steps"></a>Nächste Schritte

- Konfigurieren Sie Protokollanalyse anderen [Datenquellen](log-analytics-data-sources.md) zur Analyse sammeln.
- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Daten aus Datenquellen und Solutions analysieren.
- Konfigurieren Sie Alarme in Protokollanalyse proaktiv benachrichtigt Sie über wichtige Zustände in IIS-Protokolle gefunden.
