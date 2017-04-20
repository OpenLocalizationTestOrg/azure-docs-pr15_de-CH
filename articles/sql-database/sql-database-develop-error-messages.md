<properties
    pageTitle="Fehlercodes für SQL - Datenbank-Verbindungsfehler | Microsoft Azure"
    description="Enthält Informationen Sie zum SQL-Fehlercodes für Clientanwendungen SQL Datenbank Verbindung Fehlermeldungen kopieren Datenbankproblemen und allgemeine Fehler. "
    keywords="SQL-Fehlercode Sql Access aufgetreten, Sql-Fehlercodes"
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="annemill"/>


# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>Fehlercodes für Clientanwendungen SQL Datenbank SQL: Datenbank-Verbindungsfehler und andere Probleme


<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


Dieser Artikel listet die Fehlercodes SQL für SQL-Datenbank-Clientanwendungen, einschließlich Datenbank-Verbindungsfehler, vorübergehende Fehler (auch als vorübergehende Fehler) Governance Ressourcenfehler, Kopieren von Datenbankproblemen, elastischen Pool und andere Fehler. Die meisten Kategorien sind von Azure SQL-Datenbank und gelten nicht für Microsoft SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Datenbank-Verbindungsfehler, vorübergehende Fehler und andere temporäre Fehler

Die folgende Tabelle deckt die SQL-Fehlercodes für Verlust Verbindungsfehler und andere vorübergehende Fehler auftreten, wenn die Anwendung versucht, SQL-Datenbank. Erste Einführende Lernprogramme zum Azure SQL-Datenbank herstellen, finden Sie unter [Herstellen einer Verbindung mit einer Azure SQL-Datenbank](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Am häufigsten verwendete Datenbank-Verbindungsfehler und vorübergehende Fehler

Die Azure-Infrastruktur kann dynamisch Server bei hohe Arbeitslasten in der SQL-Datenbankdienst neu konfigurieren.  Dieses dynamischen Verhaltens möglicherweise Ihr Clientprogramm verliert die Verbindung mit SQL-Datenbank. Diese Art von Fehler ist *vorübergehender Fehler*aufgerufen.

Verfügt Ihr Clientprogramm Wiederholungslogik, können sie versuchen, eine Verbindung nach dem vorübergehenden Fehler Zeit selbst korrigieren.  Wir empfehlen, für 5 Sekunden vor dem ersten Wiederholungsversuch verzögern. Wiederholen nach weniger als 5 Sekunden Risiken überwältigend Cloud-Dienst. Für jede nachfolgende Wiederholungsintervalle Verzögerung exponentiell sollte, maximal 60 Sekunden.

Vorübergehende Fehler manifest in der Regel als eine der folgenden Fehlermeldungen aus der:

- Datenbank < Db_name > auf < Azure_instance >-Server ist derzeit nicht verfügbar. Versuchen Sie die Verbindung später. Wenn das Problem weiterhin besteht, Kundensupport und Ihnen die Sessionid Verfolgung der < Nummer >

- Datenbank < Db_name > auf < Azure_instance >-Server ist derzeit nicht verfügbar. Versuchen Sie die Verbindung später. Wenn das Problem weiterhin besteht, Kundensupport und Ihnen die Sessionid Verfolgung der < Nummer >. (Microsoft SQL Server, Fehler: 40613)

- Eine vorhandene Verbindung wurde vom Remotehost geschlossen.

- System.Data.Entity.Core.EntityCommandExecutionException: Fehler beim Ausführen der Command-Definition. Details finden Sie die innere Ausnahme. ---> System.Data.SqlClient.SqlException: ein Übertragungsfehler aufgetreten beim Empfang von Ergebnissen vom Server. (Anbieter: Session Provider Fehler: 19: physische Verbindung kann nicht verwendet werden)

Beispiele für Wiederholungslogik finden Sie unter:

- [Bibliotheken für SQL-Datenbank und SQL Server-Verbindung](sql-database-libraries.md) 
- [Aktionen in SQL Datenbank Verbindung und vorübergehende Fehler beheben](sql-database-connectivity-issues.md)

Eine Erläuterung des *Zeitraums blockieren* Clients ADO.NET verwenden ist in [SQL Server Connection Pooling (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)verfügbar.

### <a name="transient-fault-error-codes"></a>Vorübergehender Fehler-Fehlercodes

Die folgenden Fehler sind vorübergehend und sollte in der Anwendungslogik wiederholt 

| Fehlercode | Schweregrad | Beschreibung |
| ---: | ---: | :--- |
| 4060 | 16 | Datenbank kann nicht geöffnet werden "% & #x2a; ls" von der Anmeldung angeforderte. Die Anmeldung ist fehlgeschlagen. |
|40197|17|Der Dienst Fehler verarbeitet die Anforderung. Bitte versuchen Sie es erneut. Fehlercode: %d.<br/><br/>Diese Fehlermeldung erhalten, wenn der Dienst ausgefallen, Software oder Hardware-Upgrades, Hardwarefehler oder Failover Problemen ist. Der Fehlercode (%d) die Meldung Fehler 40197 eingebettet enthält zusätzliche Informationen über die Art oder Failover aufgetreten ist. Beispiele für die Meldung Fehler 40197 Codes eingebettet werden Fehler sind 40020, 40143, 40166 und 40540.<br/><br/>Wiederherstellen der Verbindung mit dem SQL-Datenbankserver werden Sie automatisch auf eine fehlerfreie Kopie der Datenbank verbunden. Die Anwendung muss catch 40197, Fehlerprotokoll eingebettete Fehlercode: (%d) in der Nachricht für die Problembehandlung und SQL-Datenbank wiederherstellen die Ressourcen stehen, bis die Verbindung erneut versuchen.|
|40501|20|Der Dienst ist ausgelastet. Wiederholen Sie die Anforderung nach 10 Sekunden. Vorfall-ID: %ls. Code: %d.<br/><br/>*Hinweis:* Weitere Informationen finden Sie unter:<br/>• [Ressourcengrenzen Azure SQL-Datenbank](sql-database-resource-limits.md).
|40613|17|Datenbank '%. & #x2a; ls' auf Server '% & #x2a; ls' ist zurzeit nicht verfügbar. Versuchen Sie die Verbindung später. Wenn das Problem weiterhin besteht, Kundensupport und Verfolgung Sitzungsbezeichner von Ihnen '% & #x2a; ls'.|
|49918|16|Anforderung kann nicht verarbeitet werden. Nicht genügend Ressourcen zum Verarbeiten der Anforderung.<br/><br/>Der Dienst ist ausgelastet. Wiederholen Sie die Anforderung später. |
|49919|16|Nicht Prozess erstellen oder Aktualisieren der Anforderung. Zu viele erstellen oder Vorgänge für Abonnements aktualisieren "% ld".<br/><br/>Der Dienst ist ausgelastet Verarbeitung mehrerer erstellen oder Aktualisieren von Anfragen für Ihr Abonnement oder Server. Anfragen sind für die Optimierung von Ressourcen gesperrt. [Sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) für ausstehende Vorgänge Abfragen. Warten Sie bis erstellen oder aktualisieren Anfragen sind ausstehenden Anfragen löschen und wiederholen Sie die Anforderung später. |
|49920|16|Anforderung kann nicht verarbeitet werden. Zu viele Vorgänge für Abonnement "% ld".<br/><br/>Der Dienst ist ausgelastet verarbeitet mehrere Anfragen für dieses Abonnement. Anfragen sind für die Optimierung von Ressourcen gesperrt. [Sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) Vorgangsstatus abzufragen. Warten, bis alle ausstehenden Anfragen sind oder löschen Sie eine ausstehenden Anfragen und wiederholen Sie die Anforderung später. |

## <a name="database-copy-errors"></a>Datenbankfehler kopieren

Die folgenden Fehler können auftreten, beim Kopieren einer Azure SQL-Datenbank. Weitere Informationen finden Sie unter [einer Azure SQL-Datenbank kopieren](sql-database-copy.md).


|Fehlercode|Schweregrad|Beschreibung|
|---:|---:|:---|
|40635|16|Client-IP-Adresse "% & #x2a; ls" vorübergehend deaktiviert.|
|40637|16|Erstellen Sie Datenbankkopie ist derzeit deaktiviert.|
|40561|16|Fehler beim Kopieren der Datenbank. Entweder die Quell-oder die Zieldatenbank ist nicht vorhanden.|
|40562|16|Fehler beim Kopieren der Datenbank. Die Datenbank wurde gelöscht.|
|40563|16|Fehler beim Kopieren der Datenbank. Die Datenbank wurde gelöscht.|
|40564|16|Datenbank konnte aufgrund eines internen Fehlers nicht kopiert werden. Datenbank löschen, und versuchen Sie es erneut.|
|40565|16|Fehler beim Kopieren der Datenbank. Darf nicht mehr als 1 gleichzeitige Datenbankkopie aus derselben Quelle. Datenbank löschen, und versuchen Sie es erneut.|
|40566|16|Datenbank konnte aufgrund eines internen Fehlers nicht kopiert werden. Datenbank löschen, und versuchen Sie es erneut.|
|40567|16|Datenbank konnte aufgrund eines internen Fehlers nicht kopiert werden. Datenbank löschen, und versuchen Sie es erneut.|
|40568|16|Fehler beim Kopieren der Datenbank. Datenbank ist nicht mehr verfügbar. Datenbank löschen, und versuchen Sie es erneut.|
|40569|16|Fehler beim Kopieren der Datenbank. Datenbank ist nicht mehr verfügbar. Datenbank löschen, und versuchen Sie es erneut.|
|40570|16|Datenbank konnte aufgrund eines internen Fehlers nicht kopiert werden. Datenbank löschen, und versuchen Sie es erneut.|
|40571|16|Datenbank konnte aufgrund eines internen Fehlers nicht kopiert werden. Datenbank löschen, und versuchen Sie es erneut.|

## <a name="resource-governance-errors"></a>Ressourcenfehler governance

Die folgenden Fehler werden durch übermäßige Verwendung von Ressourcen bei der Arbeit mit Azure SQL-Datenbank verursacht. Zum Beispiel:

- Eine Transaktion wurde zu lange.
- Eine Transaktion wird zu viele Sperren.
- Eine Anwendung wird zu viel Arbeitsspeicher verbrauchen.
- Eine Anwendung verbraucht viel `TempDb` Speicherplatz.

Verwandte Themen:

* Ausführlichere Informationen finden Sie hier: [Ressourcengrenzen Azure SQL-Datenbank](sql-database-resource-limits.md)

|Fehlercode|Schweregrad|Beschreibung|
|---:|---:|:---|
|10928|20|Ressourcen-ID: %d. Die %s für die Datenbank beträgt %d und erreicht wurde. Weitere Informationen finden Sie unter [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Die Ressourcen-ID gibt die Ressource an, die erreicht hat. Für Arbeitsthreads, die Ressourcen-ID = 1. Für Sessions, die Ressourcen-ID = 2.<br/><br/>*Hinweis:* Weitere Informationen zu dieser Fehlermeldung und Verfahren zur Problembehebung finden Sie unter:<br/>• [Ressourcengrenzen Azure SQL-Datenbank](sql-database-resource-limits.md). |
|10929|20|Ressourcen-ID: %d. %S Mindestgarantiefonds ist der maximale Wert beträgt %d und aktuelle Verwendung für die Datenbank ist %d. Jedoch ist der Server zurzeit ausgelastet größer als %d Anfragen für diese Datenbank zu unterstützen. Weitere Informationen finden Sie unter [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Andernfalls versuchen Sie es später erneut.<br/><br/>Die Ressourcen-ID gibt die Ressource an, die erreicht hat. Für Arbeitsthreads, die Ressourcen-ID = 1. Für Sessions, die Ressourcen-ID = 2.<br/><br/>*Hinweis:* Weitere Informationen zu dieser Fehlermeldung und Verfahren zur Problembehebung finden Sie unter:<br/>• [Ressourcengrenzen Azure SQL-Datenbank](sql-database-resource-limits.md).|
|40544|20|Die Datenbank hat das Größenkontingent erreicht. Partition löschen Daten, Indizes löschen oder der Dokumentation Lösungsmöglichkeiten.|
|40549|16|Sitzung wird beendet, haben eine lange andauernde Transaktion. Versuchen Sie Ihre Transaktion verkürzen.|
|40550|16|Die Sitzung wurde beendet, da zu viele Sperren erlangt hat. Versuchen Sie lesen oder Ändern von weniger Zeilen in einer einzigen Transaktion.|
|40551|16|Die Sitzung wurde wegen übermäßiger beendet `TEMPDB` Verwendung. Versuchen Sie Ihre Abfrage, um die temporäre Tabelle Speicherplatz reduziert.<br/><br/>*Tipp:* Wenn Sie temporäre Objekte verwenden, sparen Platz in der `TEMPDB` Datenbank temporäre Objekte löschen, nachdem sie von der Sitzung nicht mehr benötigt werden.|
|40552|16|Die Sitzung wurde wegen Protokollspeicher übermäßig Transaktion beendet. Versuchen Sie, weniger Zeilen in einer einzigen Transaktion ändern.<br/><br/>*Tipp:* Ausführen Bulk fügt mit den `bcp.exe` Dienstprogramm oder `System.Data.SqlClient.SqlBulkCopy` Klasse, verwenden Sie die `-b batchsize` oder `BatchSize` Optionen zum Einschränken der Anzahl der Zeilen in jeder Transaktion an den Server kopiert. Neuaufbau von Index mit der `ALTER INDEX` -Anweisung, verwenden Sie die `REBUILD WITH ONLINE = ON` Option.|
|40553|16|Die Sitzung wurde wegen übermäßiger Speicherverbrauch beendet. Versuchen Sie Ihre Abfrage weniger Zeilen verarbeiten.<br/><br/>*Tipp:* Die Anzahl der `ORDER BY` und `GROUP BY` Vorgänge im Transact-SQL-Code benötigt weniger Speicher Ihrer Abfrage.|

## <a name="elastic-pool-errors"></a>Elastische Pool-Fehlern

Die folgenden Fehler beziehen sich auf das Erstellen und Verwenden von Bändern Pools.

| Fehlernummer | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
| :-- | :-- | :-- | :-- | :-- | :-- |
| 1132 | EX_RESOURCE | Elastische Pool hat Speichergrenzwert erreicht. Die Speicherplatzverwendung elastischen Pool kann (%d) MB nicht überschreiten. | Elastische Pool Speicherplatz in MB. | Beim Versuch, Daten in eine Datenbank schreiben, die Speicherkapazität des elastischen Pools erreicht. | Erhöhen Sie DTUs möglichst elastische Pool, um die Speicherkapazität zu erhöhen, verringern der beanspruchte einzelner Datenbanken im elastischen Pool, oder entfernen Sie Datenbanken aus elastischen Pool. |
| 10929 | EX_USER | %S Mindestgarantiefonds ist der maximale Wert beträgt %d und aktuelle Verwendung für die Datenbank ist %d. Jedoch ist der Server zurzeit ausgelastet größer als %d Anfragen für diese Datenbank zu unterstützen. [Http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) Hilfe anzeigen Andernfalls versuchen Sie es später erneut. | DTU Minuten pro Datenbank; DTU Max. pro Datenbank | Die Gesamtzahl der gleichzeitigen Arbeitskräfte (Anfragen) in allen Datenbanken im elastischen Pool versucht Pool überschreiten. | DTUs elastische Pool möglichst um erhöhen die Arbeitskraft erhöhen, oder Datenbanken aus elastischen Pool entfernen. |
| 40844 | EX_USER | Datenbank '%ls' auf Server '%ls' ist eine Datenbank '%ls' Edition im elastischen Pool und kann keine fortlaufende Kopie Geschäftsbeziehung. | Name der Datenbank, Datenbankedition Servername | Eine StartDatabaseCopy für einen Premium-Db im elastischen Pool ausgegeben wird. | Demnächst |
| 40857 | EX_USER | Elastische Pool für Server nicht gefunden: '%ls', elastische Pool-Name: '%ls'. | Name des Servers; Name des elastischen | Angegebene elastische Pool ist auf dem angegebenen Server nicht vorhanden. | Geben Sie einen gültigen elastische Pool-Namen. |
| 40858 | EX_USER | Elastischen Pool '%ls' auf dem Server bereits vorhanden: '%ls' | elastische Poolnamen Servername | Angegebene elastische Pool bereits angegebenen logischen Server. | Bereitstellen Sie neue elastische Poolnamen. |
| 40859 | EX_USER | Elastische Pool unterstützt Dienstebene '%ls' nicht. | elastische Pool Dienstebene | Angegebene Dienstebene wird für die Bereitstellung von elastischen Pool nicht unterstützt. | Bieten Sie korrekte Ausgabe oder lassen Sie Dienstebene Dienstebene Standard verwenden. |
| 40860 | EX_USER | Kombination von elastischen Pool '%ls' und Service Ziel '%ls' ist ungültig. | elastische Poolnamen; auf objektiven Dienstname | Flexible Service und Ziel kann zusammen angegeben, wenn Dienst Ziel als 'ElasticPool' angegeben ist. | Geben Sie korrekte aus elastischen Pool und Service-Ziel. |
| 40861 | EX_USER | Die Database Edition ' %. *-Sicht nicht anders als die Dienstebene elastischen Pool ist ' %.* ls. | Datenbankedition elastischen Pool Dienstebene | Die Database Edition unterscheidet sich die Dienstebene elastischen Pool. | Bitte geben Sie eine Database Edition Dienstebene elastischen Pool unterscheidet.  Beachten Sie, dass die Database Edition muss nicht angegeben werden. |
| 40862 | EX_USER | Elastische Poolname muss elastischen Pool Service Ziel angegeben werden. | Keine | Elastische Pool Service Ziel bestimmt elastischen Pool nicht eindeutig. | Geben Sie elastischen Pool elastischen Pool Service-Ziel verwenden. |
| 40864 | EX_USER | DTUs für elastische Pool muß mindestens (%d) DTUs für Service-Tier "%. * ls'. | DTUs für elastische Pool; elastische Pool Dienstebene. | Versuch der DTUs elastische Pool unterhalb des Mindestbetrags. | Wiederholen Sie die DTUs elastischen pool auf mindestens den minimalen Grenzwert festlegen. |
| 40865 | EX_USER | DTUs für elastische Pool nicht überschreiten (%d) DTUs für Service-Tier "%. * ls'. | DTUs für elastische Pool; elastische Pool Dienstebene. | Versuch der DTUs elastische Pool über die maximale. | Wiederholen Sie nicht größer als die maximale DTUs für elastische Pool festlegen. |
| 40867 | EX_USER | DTU pro Datenbank müssen mindestens (%d) für Service-Tier "%. * ls'. | DTU Max. pro Datenbank; elastische Pool Dienstebene | Versuch DTU Max pro Datenbank unter das unterstützte Limit. | Erwägen Sie elastischen Pool Dienstebene, die die gewünschte unterstützt. |
| 40868 | EX_USER | DTU pro Datenbank nicht überschreiten (%d) für Service-Tier "%. * ls'. | DTU Max. pro Datenbank; elastische Pool Dienstebene. | Versuch DTU Max pro Datenbank unterstützten überschritten. | Erwägen Sie elastischen Pool Dienstebene, die die gewünschte unterstützt. |
| 40870 | EX_USER | DTU Minuten pro Datenbank nicht überschreiten (%d) für Service-Tier "%. * ls'. | DTU Minuten pro Datenbank; elastische Pool Dienstebene. | Versuch DTU Minuten pro Datenbank unterstützten überschritten. | Erwägen Sie elastischen Pool Dienstebene, die die gewünschte unterstützt. |
| 40873 | EX_USER | Die Anzahl der Datenbanken (%d) und DTU min pro Datenbank (%d) darf DTUs elastische Pool (%d) nicht überschreiten. | Anzahl der Datenbanken im elastischen Pool; DTU Minuten pro Datenbank; DTUs elastische Pool. | Versuch, DTU min für Datenbanken im elastischen Pool angeben, die DTUs elastische Pool überschreitet. | Erhöhen Sie DTUs elastische Pools oder verringern Sie DTU Minuten pro Datenbank, oder verringern Sie die Anzahl der Datenbanken im elastischen Pool. |
| 40877 | EX_USER | Elastischer Pool kann nicht gelöscht werden, sofern sie keine Datenbanken enthalten. | Keine | Elastische Pool enthält eine oder mehrere Datenbanken und kann nicht gelöscht werden. | Entfernen Sie Datenbanken aus elastischen Pool löschen. |
| 40881 | EX_USER | Elastische Pool ' %. * ls wurde erreicht seine Datenbank zählen.  Die Datenbank zählen elastische Pool kann nicht (%d) für elastische Pool mit (%d) DTUs überschritten. | Name des elastischen Pool; Datenbank die maximale Prozessanzahl elastischen Pool, e DTUs Ressourcenpool. | Erstellen oder elastischen Pool Count Datenbankgröße des elastischen Pools erreicht Datenbank hinzufügen möchten. | Erhöhen Sie DTUs elastische Pool möglichst Erhöhung der Datenbankgröße, oder entfernen Sie Datenbanken aus elastischen Pool. |
| 40889 | EX_USER | DTUs oder Speichergrenzwert für elastischen Pool ' %. * ls können nicht verringert werden, da, nicht genügend Speicherplatz für die Datenbanken bereitstellen möchten. | Name des elastischen Pool. | Versucht, den Speichergrenzwert elastische Pool unter der Speicherverbrauch zu reduzieren. | Verringern Sie die Speicherplatzverwendung durch einzelne Datenbanken im elastischen Pool oder entfernen Sie Datenbanken aus dem Pool, um die DTUs oder Speicherkapazität verringern. |
| 40891 | EX_USER | DTU Minuten pro Datenbank (%d) darf die maximale DTU pro Datenbank (%d) nicht überschreiten. | DTU Minuten pro Datenbank; DTU pro Datenbank. | Versuch der DTU Minuten pro Datenbank höher als die maximale DTU pro Datenbank. | Stellen Sie sicher, DTU min pro Datenbanken DTU Max pro Datenbank nicht überschreiten. |
| TBD | EX_USER | Die Speichergröße für eine einzelne Datenbank in einer elastischen darf nicht die maximale zulässige Größe überschreiten ' %. * ls service-Tier elastischen Pool. | elastische Pool Dienstebene | Die maximale Größe für die Datenbank überschreitet die maximale Größe Dienstebene elastischen Pool. | Legen Sie die maximale Größe der Datenbank im Rahmen der Dienstebene elastischen Pool die maximale Größe an. |

Verwandte Themen:

* [Erstellen Sie einen Datenbankpool elastische (C#)](sql-database-elastic-pool-create-csharp.md) 
* [Verwalten einer elastischen Datenbankpool (C#)](sql-database-elastic-pool-manage-csharp.md) 
* [Erstellen Sie einen elastischen Datenbankpool (PowerShell)](sql-database-elastic-pool-create-powershell.md) 
* [Überwachen und Verwalten einer elastischen Datenbank (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Allgemeine Fehler

Die folgenden Fehler fallen keine vorherigen Kategorien.

|Fehlercode|Schweregrad|Beschreibung|
|---:|---:|:---|
|15006|16|<AdministratorLogin>ist kein gültiger Name, da er ungültige Zeichen enthält.|
|18452|14|Anmeldung fehlgeschlagen. Die Anmeldung wird von einer nicht vertrauenswürdigen Domäne und kann verwendet werden, mit Windows authentication.% & #x2a; ls (Windows-Benutzernamen werden in dieser Version von SQL Server nicht unterstützt.)|
|18456|14|Fehler bei der Anmeldung für Benutzer ' % #x2a;ls'.%. & #x2a % ls. & #x2a; ls (Fehler bei der Anmeldung für den Benutzer "% & #x2a; ls". Die Änderung des Kennworts ist fehlgeschlagen. Änderung des Kennworts bei der Anmeldung nicht in dieser Version von SQL Server werden.)|
|18470|14|Fehler bei der Anmeldung für Benutzer '% & #x2a; ls'. Ursache: Das Konto ist disabled.% & #x2a; ls|
|40014|16|Datenbanken können nicht in derselben Transaktion verwendet werden.|
|40054|16|Tabellen ohne einen gruppierten Index werden in dieser Version von SQL Server nicht unterstützt. Erstellen eines gruppierten Indexes und versuchen Sie es erneut.|
|40133|15|Dieser Vorgang wird in dieser Version von SQL Server nicht unterstützt.|
|40506|16|Angegebene SID ist ungültig für diese Version von SQL Server.|
|40507|16|' % & #x2a;-Sicht nicht mit dieser Version von SQL Server aufgerufen werden.|
|40508|16|USE-Anweisung wird nicht unterstützt, zum Wechseln zwischen Datenbanken. Verwenden Sie eine neue Verbindung mit einer anderen Datenbank verbinden.|
|40510|16|Anweisung '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt|
|40511|16|Integrierte Funktion '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40512|16|Veraltete Funktion '%ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40513|16|Server Variable '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40514|16|'%ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40515|16|Verweis auf Datenbank und Server Namen in '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40516|16|Globale temporäre Objekte werden in dieser Version von SQL Server nicht unterstützt.|
|40517|16|Option-Schlüsselwort oder Anweisung '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40518|16|DBCC-Befehl '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40520|16|S_MSG sicherungsfähigen Klasse '% s' wird in dieser Version von SQL Server nicht unterstützt.|
|40521|16|S_MSG sicherungsfähigen Klasse '% s' wird im Bereich in dieser Version von SQL Server nicht unterstützt.|
|40522|16|Typ der Prinzipal '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40523|16|Implizite '% & #x2a; ls' erstellt wird in dieser Version von SQL Server nicht unterstützt. Erstellen Sie den Benutzer vor der Verwendung explizit.|
|40524|16|Datentyp '% & #x2a; ls' wird in dieser Version von SQL Server nicht unterstützt.|
|40525|16|MIT '%.ls' ist in dieser Version von SQL Server nicht unterstützt.|
|40526|16|' % & #x2a; ls Rowsetprovider in dieser Version von SQL Server nicht unterstützt.|
|40527|16|Verbindungsserver werden in dieser Version von SQL Server nicht unterstützt.|
|40528|16|Benutzer können Zertifikate und asymmetrische Schlüssel Windows Anmeldungen in dieser Version von SQL Server zugeordnet werden.|
|40529|16|Integrierte Funktion '% & #x2a; ls' in Identitätswechsel Kontext wird nicht in dieser Version von SQL Server.|
|40532|11|Server kann nicht geöffnet werden "% & #x2a; ls" von der Anmeldung angeforderte. Die Anmeldung ist fehlgeschlagen.|
|40553|16|Die Sitzung wurde wegen übermäßiger Speicherverbrauch beendet. Versuchen Sie Ihre Abfrage weniger Zeilen verarbeiten.<br/><br/>*Hinweis:* Die Anzahl der `ORDER BY` und `GROUP BY` Vorgänge im Transact-SQL-Code verringert den Speicherbedarf Ihrer Abfrage.|
|40604|16|Konnte nicht CREATE/ALTER DATABASE, da es das Kontingent des Servers überschreiten würde.|
|40606|16|Anfügen von Datenbanken wird in dieser Version von SQL Server nicht unterstützt.|
|40607|16|Windows-Benutzernamen werden in dieser Version von SQL Server nicht unterstützt.|
|40611|16|Server können höchstens 128 Firewallregeln definiert haben.|
|40614|16|Start-IP-Adresse der Firewall-Regel kann die End-IP-Adresse nicht überschreiten.|
|40615|16|Server von der Anmeldung angeforderte "{0}' kann nicht geöffnet werden. Client-IP-Adresse '{1}' darf nicht auf den Server zugreifen.  Zugriff SQL Datenbank Portal oder Sp_set_firewall_rule auf der master-Datenbank erstellen Sie eine Firewall-Regel für diese IP-Adresse oder den Adressbereich ausgeführt.  Es dauert bis zu fünf Minuten, damit die Änderung wirksam wird.|
|40617|16|Der Name der Firewall, die mit <rule name> ist zu lang. Maximale Länge beträgt 128.|
|40618|16|Der Firewallregelname darf nicht leer sein.|
|40620|16|Fehler bei der Anmeldung für den Benutzer "% & #x2a; ls". Die Änderung des Kennworts ist fehlgeschlagen. Änderung des Kennworts bei der Anmeldung wird in dieser Version von SQL Server nicht unterstützt.|
|40627|20|Auf Server '{0}' und die Datenbank '{1}' wird ausgeführt. Warten Sie einen Moment, bevor Sie erneut versuchen.|
|40630|16|Fehler beim Überprüfen. Das Kennwort erfüllt nicht erfordern, ist zu kurz.|
|40631|16|Das angegebene Kennwort ist zu lang. Das Kennwort sollte nicht mehr als 128 Zeichen enthalten.|
|40632|16|Fehler beim Überprüfen. Das Kennwort erfüllt nicht erfordern, ist nicht komplex genug.|
|40636|16|Können keine reservierten Datenbankname '% & #x2a; ls' in diesem Vorgang.|
|40638|16|Ungültiges Abonnement-Id < Abonnement-Id >. Abonnement ist nicht vorhanden.|
|40639|16|Anforderung nicht Schema entsprechen: <schema error>.|
|40640|20|Der Server hat eine unerwartete Ausnahme.|
|40641|16|Der angegebene Pfad ist ungültig.|
|40642|17|Der Server ist zurzeit ausgelastet. Bitte versuchen Sie es später erneut.|
|40643|16|Der angegebene X-ms-Version-Header-Wert ist ungültig.|
|40644|14|Fehler beim Autorisieren des Zugriffs auf das angegebene Abonnement.|
|40645|16|Servername <servername> darf nicht leer oder null sein. Es kann nur bestehen aus Kleinbuchstaben 'a'-'Z', die Ziffern 0-9 und Bindestriche. Der Bindestrich möglicherweise nicht oder im Namen Spur.|
|40646|16|Abonnement-ID darf nicht leer sein.|
|40647|16|Abonnement < Abonnement-Id hat keinen Server Servername.|
|40648|17|Zu viele Anfragen wurden durchgeführt. Bitte später erneut versuchen.|
|40649|16|Ungültiger Inhaltstyp angegeben. Nur Application/Xml unterstützt.|
|40650|16|< Abonnement-Id > Abonnement ist nicht vorhanden oder kann nicht für den Vorgang.|
|40651|16|Fehler beim Server erstellt werden, da das Abonnement < Abonnement-Id > deaktiviert ist.|
|40652|16|Kann nicht verschoben oder Server erstellen. Abonnement < Abonnement-Id > überschreitet Server Kontingent.|
|40671|17|Kommunikationsfehler zwischen dem Gateway und Service Management. Bitte später erneut versuchen.|
|40852|16|Datenbank kann nicht geöffnet werden ' %. *! auf Server ' %.* von der Anmeldung angeforderte-Sicht. Zugriff auf die Datenbank ist nur zulässig mit aktivierter Sicherheit Verbindungszeichenfolge. Zugriff auf die Datenbank, ändern Sie die Verbindungszeichenfolgen enthalten sichere Server FQDN - 'Servername'.database.windows .net sollte auf 'Servername'.database geändert werden. `secure`. Windows.|
|45168|16|SQL Azure-System unter Last und setzt eine Obergrenze für gleichzeitige DB CRUD-Operationen für einen einzelnen Server (z.B. Datenbank erstellen). In der Fehlermeldung angegebene Server hat die maximale Anzahl der aktiven Verbindungen überschritten. Versuchen Sie es erneut.|
|45169|16|SQL Azure-System unter Last und setzt eine Obergrenze für die Anzahl gleichzeitiger Server CRUD-Operationen für ein einzelnes Abonnement (z. B. Server erstellen). In der Fehlermeldung angegebene Abonnement wurde die maximale Anzahl der aktiven Verbindungen überschritten, und die Anforderung wurde verweigert. Versuchen Sie es erneut.|

## <a name="related-links"></a>Verwandte links

- [SQL Azure Database General Grenzen und Richtlinien](sql-database-general-limitations.md)
- [Azure SQL-Datenbank Ressourcengrenzen](sql-database-resource-limits.md)
