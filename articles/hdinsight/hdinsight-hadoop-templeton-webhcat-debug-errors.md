<properties
 pageTitle="Verstehen und Beheben von Fehlern auf HDInsight WebHCat"
 description="Erfahren Sie, wie über häufige Fehler von WebHCat auf HDInsight zurückgegeben und deren Behebung."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
 tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Verstehen und Lösen von WebHCat (Templeton) Fehler HDInsight

Wenn Sie Arbeit mit HDInsight, WebHCat (ehemals Templeton,) erhalten Sie Fehler. Dieses Dokument enthält Hinweise auf häufige Fehler – warum sie auftreten und was Sie tun können, um sie zu beheben.

##<a name="what-is-webhcat"></a>Was ist WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) ist eine REST-API für [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), einer Tabelle und Storage Management für Hadoop. WebHCat auf HDInsight Cluster standardmäßig aktiviert und wird von verschiedenen Tools zu Aufträge Auftragsstatus usw. ohne Anmeldung zum Cluster verwendet.

##<a name="modifying-configuration"></a>Konfiguration ändern

> [AZURE.IMPORTANT] Einige der in diesem Dokument aufgeführten Fehler auftreten, weil konfigurierten Maximalwert überschritten wurde. Bei der Auflösung Schritt erwähnt, einen Wert zu ändern, müssen Sie folgende Änderung ausführen verwenden:

* Für **Windows** -Clustern: Skript-Aktion verwenden, um den Wert während der Erstellung des Clusters zu konfigurieren. Weitere Informationen finden Sie unter [entwickeln Skriptaktionen](hdinsight-hadoop-script-actions.md).

* Für **Linux** -Clustern: mit Ambari Web oder REST-API den Wert ändern. Weitere Informationen finden Sie unter [Verwalten von HDInsight Ambari verwenden](hdinsight-hadoop-manage-ambari.md)

###<a name="default-configuration"></a>Standardkonfiguration

Folgende sind Konfiguration Standardwerte, die Leistungseinbußen WebHCat oder Fehlern führen, wenn diese Grenzwerte:

| Einstellung | Funktionsweise | Standardwert |
| ------- | ------------ | ------------- |
| [yarn.Scheduler.Capacity.Maximum-Anwendung][maximum-applications] | Die maximale Anzahl der Aufträge, die gleichzeitig aktiv sein können (ausstehende oder ausgeführt) | 10.000 |
| [templeton.Exec.Max-Prozesse][max-procs] | Die maximale Anzahl der Anfragen gleichzeitig verarbeitet werden können | 20 |
| [MapReduce.jobhistory.Max Alter ms][max-age-ms] | Die Anzahl der Tage, die Historie beibehalten werden | 7 Tage |

##<a name="too-many-requests"></a>Zu viele Anfragen

**HTTP-Statuscode**: 429

| Ursache | Auflösung |
| ----- | ---------- |
| Sie haben die maximalen gleichzeitigen Anforderungen von WebHCat pro Minute (Standardeinstellung 20) überschritten. | Workload um sicherzustellen, dass Sie nicht senden mehr als die maximale Anzahl von gleichzeitigen Anfragen verringern oder erhöhen Sie den Grenzwert gleichzeitiger Anfrage ändern `templeton.exec.max-procs`. Weitere Informationen finden Sie in [Konfiguration ändern](#modifying-configuration) |

##<a name="server-unavailable"></a>Server nicht verfügbar

**HTTP-Statuscode**: 503

| Ursache | Auflösung |
| ---------------- | ------------------- |
| Dies geschieht normalerweise bei einem Failover zwischen primären und sekundären Hauptknoten für den cluster | Warten Sie zwei Minuten, und wiederholen Sie den Vorgang |

##<a name="bad-request-content-could-not-find-job"></a>Ungültige Anforderung Inhalt: Auftrag nicht gefunden

**HTTP-Statuscode**: 400

| Ursache | Auflösung |
| ---------------- | ------------------- |
| Auftragsdetails haben durch den Auftragsverlauf cleaner bereinigt | Die Standardaufbewahrungsdauer für Auftragsverlauf beträgt 7 Tage. Dies kann geändert werden, indem `mapreduce.jobhistory.max-age-ms`. Weitere Informationen finden Sie in [Konfiguration ändern](#modifying-configuration) |
| Auftrag wurde durch ein Failover beendet | Bewerbung für bis zu zwei Minuten wiederholen |
| Ungültige Auftrags-Id wurde verwendet. | Überprüfen Sie, ob die Auftrags-Id richtig ist |

##<a name="bad-gateway"></a>Fehlerhaftes gateway

**HTTP-Statuscode**: 502

| Ursache | Auflösung |
| ---------------- | ------------------- |
| Interne Garbagecollection erfolgt innerhalb des WebHCat-Prozesses | Beenden oder starten Sie den Dienst WebHCat die Garbagecollection warten |
| Timeout beim Warten auf eine Antwort vom Ressourcen-Manager-Dienst. Dies kann auftreten, wenn die Anzahl der aktiven Anwendung konfigurierten Maximalwert (standardmäßig 10.000) geht | Warten auf laufende Aufträge abgeschlossen oder Erhöhen der gleichzeitigen Arbeit ändern `yarn.scheduler.capacity.maximum-applications`. Weitere Informationen finden Sie in [Konfiguration ändern](#modifying-configuration)  |
| Wenn alle Aufträge über den Aufruf [erhalten "/ Jobs"](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) beim Abrufen `Fields` soll`*` | Nicht *Alle* Auftragsdetails abzurufen, verwenden Sie stattdessen `jobid` zum Abrufen von Details für Projekte, die nur bestimmte Einzelvorgangsnummer größer. Oder nicht verwenden`Fields` |
| Der WebHCat-Dienst ist nicht verfügbar bei Hauptknoten failover | Warten Sie zwei Minuten, und wiederholen Sie den Vorgang |
| Mehr als 500 ausstehende Aufträge über WebHCat gesendet | Warten Sie, bis der aktuell ausstehenden Druckaufträge abgeschlossen haben, bevor weitere Aufträge |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 
