<properties
    pageTitle="Ein Verbindungsfehler SQL, vorübergehender Fehler beheben | Microsoft Azure"
    description="Informationen Sie zum beheben, diagnostizieren und zu verhindern, dass ein Verbindungsfehler SQL oder vorübergehender Fehler in Azure SQL-Datenbank. "
    keywords="SQL-Verbindung Verbindungszeichenfolge Verbindungsprobleme, Übertragungsfehler, Verbindungsfehler"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="daleche"/>


# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Problembehandlung, diagnose und verhindern von SQL-Verbindung und vorübergehende Fehler für SQL-Datenbank

Dieser Artikel beschreibt, wie verhindert, Problembehandlung, diagnose und Minderung der Verbindungsfehler und vorübergehende Fehler die Clientanwendung erkennt bei einer Interaktion mit Azure SQL-Datenbank. Informationen Sie zum Wiederholungslogik konfigurieren, erstellen die Verbindungszeichenfolge und andere Verbindung anpassen.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Vorübergehende Fehler transiente

Übertragungsfehler - auch vorübergehender Fehler - hat eine Ursache, die bald selbst löst. Eine gelegentliche vorübergehender Fehler verursacht bei Azure System schnell Hardwareressourcen besser Lastenausgleich verschiedene Arbeitslasten verschiebt. Die meisten dieser Ereignisse Neukonfiguration führen häufig weniger als 60 Sekunden. Während dieser Zeitspanne Rekonfiguration müssen Sie Konnektivitätsprobleme zu Azure SQL-Datenbank. Applikationen Azure SQL-Datenbank herstellen soll, erwarten diese vorübergehende Fehler durch Implementieren der Wiederholungslogik in ihrem Code anstelle von Benutzern nach Anwendung dieses behandeln erstellt werden.

Wenn Ihr Clientprogramm ADO.NET verwenden, Ihr Programm zur vorübergehenden Fehler löst eine **SqlException**erzählt. **Number** -Eigenschaft vergleichbar mit der Liste im oberen Bereich des Themas vorübergehender Fehler: [SQL-Fehlercodes für Clientanwendungen SQL-Datenbank](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Verbindung und Befehl

Sie die SQL Verbindung oder wieder, je nach den folgenden erstellen:

* **Vorübergehende Fehler bei einem Verbindungsversuch**: der Verbindungsversuch sollte nach einigen Sekunden verzögert.

* **Vorübergehender Fehler auftritt, während ein SQL-Abfragebefehl**: der Befehl sollte nicht sofort wiederholt. Stattdessen sollte nach einer Verzögerung die Verbindung werden frisch hergestellt. Anschließend kann der Befehl wiederholt werden.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Wiederholungslogik vorübergehender Fehler


Clientprogramme, die gelegentlich eine vorübergehende Fehler sind robuster, wenn sie Wiederholungslogik enthalten.


Kommunikation zwischen der Anwendung und Azure SQL-Datenbank über 3rd Party Middleware Fragen Sie mit dem Lieferanten ab, ob die Middleware Wiederholungslogik für vorübergehende Fehler enthält.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Grundsätze für Wiederholung


- Versuch, eine Verbindung zu öffnen sollte wiederholt werden, wenn der Fehler vorübergehend ist.


- Eine SQL SELECT-Anweisung, die mit einem vorübergehenden Fehler sollte nicht direkt wiederholt werden.
 - Stattdessen erstellen Sie eine neue Verbindung, und wiederholen Sie auswählen.


- Fällt eine SQL UPDATE-Anweisung einen vorübergehenden Fehler, sollten vor die Aktualisierung wiederholt wird eine neue Verbindung hergestellt.
 - Die Wiederholungslogik müssen entweder die gesamte Datenbank Buchung abgeschlossen oder die gesamte Transaktion ein Rollback ausgeführt.


#### <a name="other-considerations-for-retry"></a>Weitere Hinweise zur Wiederholung


- Ein Batchprogramm, startet automatisch Geschäftszeiten und vervollständigt die vor morgen, können sehr Patienten mit langen Zeitintervalle zwischen der Wiederholungsversuche.


- Ein Programm muss die menschliche Tendenz zu lange warten berücksichtigen.
 - Allerdings muss die Lösung nicht alle paar Sekunden wiederholen da diese Richtlinie das System mit Anfragen überflutet werden kann.


#### <a name="interval-increase-between-retries"></a>Intervall Anstieg zwischen Wiederholungsversuchen



Wir empfehlen, für 5 Sekunden vor dem ersten Wiederholungsversuch verzögern. Wiederholen nach weniger als 5 Sekunden Risiken überwältigend Cloud-Dienst. Für jede nachfolgende Wiederholungsintervalle Verzögerung exponentiell sollte, maximal 60 Sekunden.

Eine Erläuterung des *Zeitraums blockieren* Clients ADO.NET verwenden ist in [SQL Server Connection Pooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)verfügbar.

Sie sollten auch eine maximale Anzahl von Wiederholungsversuchen festgelegt, bevor die Anwendung beendet.


#### <a name="code-samples-with-retry-logic"></a>Codebeispiele mit Wiederholungslogik


Codebeispiele mit Wiederholungslogik in einer Vielzahl von Programmiersprachen sind verfügbar unter:

- [Bibliotheken für SQL-Datenbank und SQL Server-Verbindung](sql-database-libraries.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Testen der Wiederholungslogik


Testen Sie die Wiederholungslogik müssen simulieren oder verursachen Fehler als korrigiert werden kann, während das Programm weiterhin ausgeführt wird.


##### <a name="test-by-disconnecting-from-the-network"></a>Testen Sie, indem Sie vom Netzwerk trennen


Eine Möglichkeit der Wiederholungslogik zu testen ist auf dem Clientcomputer vom Netzwerk trennen, während die Anwendung ausgeführt wird. Der Fehler ist:

- **SqlException.Number** = 11001
- Meldung: "kein solcher Host bekannt"


Als Teil der ersten Wiederholungsversuch Programm korrigieren die Rechtschreibfehler und dann versuchen, eine Verbindung herzustellen.


Dazu praktische, trennen Sie den Computer aus dem Netzwerk vor dem Starten des Programms. Dann erkennt das Programm, bei dem die Anwendung, zur Laufzeit Parameter:

1. Die Liste der Fehler als flüchtig berücksichtigen vorübergehend fügen Sie 11001 hinzu.
2. Versuchen Sie die erste Verbindung wie gewohnt.
3. Nachdem der Fehler abgefangen, entfernen Sie 11001 aus der Liste.
4. Anzeigen einer Meldung des Benutzers an den Computer an das Netzwerk anschließen.
 - Weitere hält die mit **Console.ReadLine** Methode oder ein Dialogfeld mit der Schaltfläche OK. Der Benutzer drückt die EINGABETASTE des Computers mit dem Netzwerk verbunden.
5. Versuchen Sie erneut zu verbinden, Erfolg erwartet.


##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Durch falsche Schreibweise Datenbank Verbindung testen


Das Programm kann Benutzernamen vor den ersten Verbindungsversuch absichtlich falsch eingegeben. Der Fehler ist:

- **SqlException.Number** = 18456
- Meldung: "Anmeldung für den Benutzer 'WRONG_MyUserName'."


Als Teil der ersten Wiederholungsversuch Programm korrigieren die Rechtschreibfehler und dann versuchen, eine Verbindung herzustellen.


Dazu praktische, konnte Ihr Programm zur Laufzeit Parameter erkannt, bei dem das Programm:

1. Die Liste der Fehler als flüchtig berücksichtigen vorübergehend fügen Sie 18456 hinzu.
2. Der Benutzername absichtlich "WRONG_" hinzufügen.
3. Nachdem der Fehler abgefangen, entfernen Sie 18456 aus der Liste.
4. Entfernen Sie "WRONG_" vom Benutzernamen
5. Versuchen Sie erneut zu verbinden, Erfolg erwartet.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>.NET SqlConnection Parameter Verbindungsversuch


Wenn das Clientprogramm **System.Data.SqlClient.SqlConnection**auf Azure SQL-Datenbank verbindet mithilfe der.NET Framework-Klasse, verwenden Sie .NET 4.6.1 oder höher, sodass die Verbindung wiederholen-Funktion nutzen zu können. Details des Features sind [hier](http://go.microsoft.com/fwlink/?linkid=393996).


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Wenn Sie die [Verbindungszeichenfolge](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) für das **SqlConnection** -Objekt erstellen, sollten Sie folgende Parameter Koordinatenwerte:

- ConnectRetryCount &nbsp; &nbsp; *(Standardwert ist 1. Bereich ist 0 bis 255.)*
- ConnectRetryInterval &nbsp; &nbsp; *(Standardwert ist 1 Sekunde. Bereich ist 1 bis 60.)*
- Verbindungstimeout &nbsp; &nbsp; *(der Standardwert beträgt 15 Sekunden. Bereich ist 0 bis 2147483647)*


Insbesondere sollten die gewählten Werte die folgenden Gleichheit wahr:

- Connection Timeout = ConnectRetryCount * ConnectionRetryInterval

Z. B. wenn die Count = 3 und Intervall = 10 Sekunden ein Timeout von nur 29 Sekunden nicht ganz das System ausreichend Zeit für die 3. und letzten Wiederholung auf Verbinden würde: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>Verbindung und Befehl


Die Parameter **ConnectRetryCount** und **ConnectRetryInterval** können die **SqlConnection** -Objekt den Verbindungsvorgang wiederholen, ohne dass stört das Programm wie die Steuerung an das Programm zurückgegeben. Wiederholungsversuche können in folgenden Situationen auftreten:

- Aufruf der mySqlConnection.Open-Methode
- Aufruf der mySqlConnection.Execute-Methode

Es ist eine Besonderheit. Vorübergehender Fehler auftritt, während die *Abfrage* ausgeführt wird, das **SqlConnection** -Objekt den Verbindungsvorgang nicht wiederholen und es sicherlich ist keine Wiederholung der Abfrage. **SqlConnection** prüft jedoch sehr schnell die Verbindung, bevor die Abfrage ausgeführt. Wenn Schnelle Prüfung eine Problem erkennt, versucht **SqlConnection** den Verbindungsvorgang. Erfolgreicher Wiederholungsversuche erhält Sie eine Abfrage für die Ausführung.


#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Werden ConnectRetryCount mit Wiederholungslogik Anwendung kombiniert?

Angenommen Sie, Ihre Anwendung benutzerdefinierte Wiederholungslogik robust. Sie können den Verbindungsvorgang 4 Mal wiederholen. **ConnectRetryInterval** und **ConnectRetryCount** 3 = zur Verbindungszeichenfolge, erhöhen Sie die Wiederholungsanzahl 4 * 3 = 12 Versuche. Sie können eine große Anzahl von Wiederholungsversuchen nicht davon.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Verbindung zu SQL Azure-Datenbank

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Verbindung: Verbindungszeichenfolge


Die Verbindungszeichenfolge für die Verbindung mit Azure SQL-Datenbank erforderlich ist geringfügig von der Zeichenfolge für die Verbindung mit Microsoft SQL Server. Sie können die Verbindungszeichenfolge für die Datenbank aus dem [Azure-Portal](https://portal.azure.com/).


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]


<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Verbindung: IP-Adresse


Konfigurieren Sie die SQL-Datenbankserver um Kommunikation von der IP-Adresse des Computers übernehmen, die die Client-Anwendung hostet. Dazu bearbeiten Firewall Settings [Azure-Portal](https://portal.azure.com/).


Sollten Sie die IP-Adresse konfigurieren, kann das Programm mit einer praktischen Fehlermeldung nicht, die erforderliche IP-Adresse angibt.


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


Weitere Informationen finden Sie unter: [wie: Konfigurieren der Firewall für SQL-Datenbank](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Verbindung: Ports


Normalerweise müssen Sie nur sicherstellen, dass Port 1433 für die ausgehende Kommunikation auf dem Computer geöffnet ist, die Client-Anwendung hostet.


Z. B. wenn das Clientprogramm auf einem windowscomputer gehostet wird, können die Windows-Firewall auf dem Server Anschluss 1433 öffnen:


1. Öffnen Sie das Bedienfeld
2. &gt;Alle Systemsteuerungselemente
3. &gt;Windows-Firewall
4. &gt;Erweitert
5. &gt;Ausgehende Regeln
6. &gt;Aktionen
7. &gt;Neue Regel


Wenn die Client-Anwendung auf einer Azure Virtual Machine (VM), sollten Sie lesen:<br/>[Ports über 1433 für ADO.NET 4.5 und SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).


Hintergrundinformationen zur Konfiguration des Ports und IP-Adresse finden Sie unter: [Azure SQL-Datenbank-Firewall](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Verbindung: ADO.NET 4.6.1


Wenn Ihr Programm ADO.NET-Klassen wie **System.Data.SqlClient.SqlConnection** zum Azure SQL-Datenbank herstellen, empfiehlt Sie.NET Framework Version 4.6.1 oder höher.


ADO.NET 4.6.1:

- Azure SQL-Datenbank ist Zuverlässigkeit beim Öffnen einer Verbindungs mit der Methode **SqlConnection.Open** . Die **Open** -Methode enthält nun die beste Aufwand Wiederholungsmechanismen auf vorübergehende Fehler, für bestimmte Fehler innerhalb Verbindungstimeout.
- Verbindungspooling unterstützt. Dies schließt eine effizientere Überprüfung das Verbindungsobjekt Programm gibt funktioniert.



Bei Verwendung ein Verbindungsobjekts aus einem Verbindungspool empfohlen Programm vorübergehend die Verbindung schließen nicht sofort verwenden. Erneut öffnen einer Verbindung ist nicht teuer ist die Möglichkeit, eine neue Verbindung erstellen.


Wenn Sie ADO.NET 4.0 oder früher wir empfehlen die Aktualisierung auf die neueste ADO.NET.

- Ab November 2015 können Sie [ADO.NET 4.6.1 herunterladen](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnose

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnose: Prüfen Sie, ob Dienstprogramme herstellen können


Wenn das Programm nicht Azure SQL-Datenbank herstellen, ist Möglichkeit eine Diagnose mit einem Hilfsprogramm zugreifen. Das Dienstprogramm verbinden idealerweise mit derselben Bibliothek, die das Programm verwendet.


Auf jedem Windows-Computer können Sie diese Dienstprogramme versuchen:

- SQL Server Management Studio (ssms.exe) mit ADO.NET verbindet.
- Sqlcmd.exe über [ODBC](http://msdn.microsoft.com/library/jj730308.aspx)her.


Sobald verbunden, testen Sie, ob eine kurze SQL SELECT Query funktioniert.


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnose: Überprüfen Sie offene ports


Angenommen, Sie vermuten, daß Verbindungsversuche durch Port Probleme. Auf Ihrem Computer können Sie das Dienstprogramm ausführen, die die Konfigurationen meldet.


Unter Linux können die folgenden Dienstprogramme hilfreich sein:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (Ändern Sie den Wert wird auf die IP-Adresse.)


Programm kann auf Windows [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) hilfreich sein. Hier ist ein Beispiel ausführen, die die Situation Anschluss einer Azure SQL-Datenbankserver abgefragt und die auf einem Laptop ausgeführt wurde:


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnose: Der Fehler protokollieren


Ein unregelmäßig auftretendes Problem ist manchmal am besten durch Erkennung von allgemein über Tage oder Wochen diagnostiziert.


Ihr Client kann bei der Diagnose durch alle entdeckten Fehler protokollieren. Sie können möglicherweise die Protokolleinträge Fehlerdaten zuordnen, die Azure SQL-Datenbank selbst intern protokolliert.


Enterprise Library 6 (EntLib60) bietet verwalteter Klassen bei Protokollierung:

- [5 - so einfach wie eine Abmeldung: mit der Logging Application Block](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnose: Systemprotokolle auf Fehler überprüfen


Hier sind einige Transact-SQL SELECT-Anweisungen, Protokolle Abfrage Fehler und andere Informationen.


| Abfrage des Protokolls | Beschreibung |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` | Die Ansicht [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) enthält Informationen über einzelne Ereignisse, einschließlich derjenigen, die vorübergehende Fehler oder Verbindungsfehler auftreten können.<br/><br/>Im Idealfall können Sie entsprechen **Start_time** oder **End_time** Werte Informationen bei Ihrem Clientprogramm Probleme.<br/><br/>**Tipp:** Sie müssen auf die **master** -Datenbank ausführen verbinden. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` | Die [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) -Ansicht bietet aggregierte Anzahl Ereignistypen für zusätzliche Diagnose.<br/><br/>**Tipp:** Sie müssen auf die **master** -Datenbank ausführen verbinden. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnose: Problem Ereignisse SQL Datenbank suchen


Sie können Einträge zum Problem Ereignisse von Azure SQL-Datenbank suchen. Führen Sie die folgende Transact-SQL SELECT-Anweisung in der **master** -Datenbank aus:


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Zurückgegebene Zeilen aus sys.fn_xe_telemetry_blob_target_read_file


Anschließend wird eine zurückgegebene Zeile könnte folgendermaßen aussehen. Null-Werte dargestellt sind häufig nicht in anderen Zeilen null.


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Enterprise Library 6


Enterprise Library 6 (EntLib60) ist ein Framework von Klassen, die robuste Clients Cloud-Services implementieren soll Ihnen Azure SQL-Datenbank-Dienst gehört. Sie können Themen gewidmet Bereiche in der EntLib60 helfen können vom ersten Besuch finden:

- [Enterprise Library 6 – April 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


Wiederholungslogik für transiente Fehlerbehandlung ist ein Bereich in der EntLib60 helfen können:

- [4 - Ausdauer, alle Erfolge Geheimnis: vorübergehender Fehler Handling Application Block verwenden](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


> [AZURE.NOTE] Der Quellcode für EntLib60 steht für öffentliche [herunterladen](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft plant keine weitere Funktionsupdates oder Wartung erfolgen auf Funktionen.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60 Klassen für vorübergehende Fehler und wiederholen


Die folgenden EntLib60 Klassen sind besonders für Wiederholungslogik. Diese sind oder unter der **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**-Namespace:

*Im namespace* *Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling* *:*

- **RetryPolicy** -Klasse
 - **ExecuteAction** -Methode


- **ExponentialBackoff** -Klasse


- **SqlDatabaseTransientErrorDetectionStrategy** -Klasse


- **ReliableSqlConnection** -Klasse
 - **ExecuteCommand** -Methode


In der **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**-Namespace:

- **AlwaysTransientErrorDetectionStrategy** -Klasse

- **NeverTransientErrorDetectionStrategy** -Klasse


Hier werden Links zu Informationen über EntLib60:

- Kostenlose [Buch herunterladen: Developer's Guide to Microsoft Enterprise Library, 2nd Edition](http://www.microsoft.com/download/details.aspx?id=41145)

- Bewährte Methoden: [Wiederholen Leitfaden](../best-practices-retry-general.md) hat eine hervorragende ausführliche Erläuterung der Wiederholungslogik.

- NuGet Download von [Enterprise Library - Anwendungsblocks vorübergehende Fehler Behandlung 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: Die Protokollierung blockieren


- Protokollierung-Block ist ein äußerst flexibles und konfigurierbares Lösung an:
 - Erstellen und Speichern von Nachrichten protokollieren in einer Vielzahl von Standorten.
 - Kategorisieren und Filtern von Nachrichten.
 - Ist nützlich zum Debuggen und verfolgen sowie für die Überwachung und allgemeine Protokollierung Kontextinformationen zu sammeln.


- Protokollierung-Block abstrahiert die Funktionalität aus dem Ziel, damit der Anwendungscode unabhängig von Ort und den Speichertyp Ziel anmelden ist.


Siehe: [5 - als einfach als fallen aus einem Protokoll: der Logging Application Block mit](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>Quellcode für EntLib60 IsTransient-Methode


Anschließend wird von der Klasse **SqlDatabaseTransientErrorDetectionStrategy** der C#-Quellcode für **IsTransient** -Methode. Der Quellcode wird Fehler wurden als vorübergehender Natur und der Wiederholung April 2013.

Zahlreiche **//comment** Zeilen wurden aus dieser Kopie Lesbarkeit besondere entfernt.


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## <a name="next-steps"></a>Nächste Schritte

- Andere allgemeine Problembehandlung Azure SQL-Datenbank-Verbindung, finden Sie unter [Problembehandlung bei Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md).

- [SQL Serververbindungs-Pooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*Erneuter* ist Apache 2.0 allgemeine Wiederholung Bibliothek geschrieben in **Python**, was Wiederholungsverhalten hinzugefügt sein lizenziert.](https://pypi.python.org/pypi/retrying)
