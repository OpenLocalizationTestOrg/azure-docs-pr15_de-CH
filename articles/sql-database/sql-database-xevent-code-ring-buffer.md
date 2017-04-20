<properties 
    pageTitle="XEvent Ringpuffer Code SQL Datenbank | Microsoft Azure" 
    description="Enthält ein Codebeispiel Transact-SQL, die einfach und schnell mithilfe des Ziels Ringpuffer in Azure SQL-Datenbank besteht." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Ring Puffer Zielcode für erweiterte Ereignisse in SQL-Datenbank

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Soll ein vollständiges Codebeispiel am einfachsten schnelle Erfassung und Bericht Informationen für eine erweiterte Ereignis während eines Tests. Die einfachste erweiterte Daten soll [Ringpuffer Ziel](http://msdn.microsoft.com/library/ff878182.aspx).


Dieses Thema enthält ein Codebeispiel Transact-SQL, die:


1. Erstellt eine Tabelle mit Daten.

2. Erstellt eine Sitzung für eine erweiterte Veranstaltung, nämlich **sqlserver.sql_statement_starting**.
    - Das Ereignis ist auf SQL-Anweisung, die eine bestimmte Zeichenfolge Update enthalten: **Anweisung wie 'UPDATE TabEmployee %'**.
    - Wählt ein Ziel vom Typ Ringpuffer, nämlich **package0.ring_buffer**die Ausgabe des Ereignisses an.

3. Startet die Sitzung.

4. Gibt ein paar einfache SQL UPDATE-Anweisung.

5. Stellt eine SQL SELECT ereignisausgabe aus dem Ringpuffer abgerufen.
    - **Sys.dm_xe_database_session_targets** und andere dynamische Verwaltungsansichten (DMVs) verknüpft.

6. Beendet die Sitzung.

7. Löscht Ringpuffer Ziel, um die Ressourcen freizugeben.

8. Löscht die Sitzung und die Tabelle Demo.


## <a name="prerequisites"></a>Erforderliche Komponenten


- Ein Azure-Konto und ein Abonnement. Sie können sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)anmelden.


- Jede Datenbank können Sie eine Tabelle in.
 - Optional können Sie [erstellen eine Datenbank **AdventureWorksLT** ](sql-database-get-started.md) in Minuten.


- SQL Server Management Studio (ssms.exe) im Idealfall die letzte monatliche Updateversion. Sie können die neueste ssms.exe aus:
 - Thema [SQL Server Management Studio herunterzuladen](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Einen Link zum Download.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Codebeispiel


Geringfügige Änderung kann das folgende Codebeispiel Ringpuffer Azure SQL-Datenbank oder Microsoft SQL Server ausgeführt werden. Der Unterschied ist das Vorhandensein des Knotens 'voran _Datenbank' Namen einige dynamische Verwaltungsansichten (DMVs) in der FROM-Klausel in Schritt 5 verwendet. Zum Beispiel:

- Sys.dm_xe**voran _Datenbank**_session_targets
- Sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Ring Pufferinhalt


Ssms.exe verwendet, um das Codebeispiel auszuführen.


Zum Anzeigen der Ergebnisse die Zelle unter der Spalte Kopfzeile **Target_data_XML**ausgewählt.

Dann geklickt im Ergebnisbereich wir die Zelle in der Spalte Kopfzeile **Target_data_XML**. Klicken Sie im ssms.exe in der Inhalt der Ergebniszelle, als XML angezeigt wurde Registerkarte eine andere Datei erstellt.


Die Ausgabe ist im folgenden dargestellt. Lange sieht es jedoch nur zwei **<event>** Elemente.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Freigeben Sie der Ringpuffer Ressourcen


Wenn der Ringpuffer abgeschlossen haben, können Sie entfernen und gibt die Ressourcen Ausstellen einer **ALTER** wie folgt:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


Die Definition der Veranstaltung aktualisiert, aber nicht gelöscht. Später können Sie eine weitere Instanz des Ringpuffers Ihre Veranstaltung hinzufügen:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Weitere Informationen


Die primäre erweiterte Ereignisse Azure SQL-Datenbank ist:


- [Erweiterte Ereignis Aspekte in SQL-Datenbank](sql-database-xevent-db-diff-from-svr.md), die einige Aspekte der erweiterten Ereignissen gegenübergestellt, die zwischen Azure SQL-Datenbank oder Microsoft SQL Server unterscheiden.


Themenauswahl Sample Code für erweiterte Ereignisse sind unter den folgenden Links verfügbar. Allerdings müssen Sie regelmäßig Beispiele, um festzustellen, ob das Beispiel Microsoft SQL Server im Vergleich zu Azure SQL-Datenbank verwendet. Dann Sie entscheiden können, ob geringfügige erforderlich sind, um das Beispiel auszuführen.


- Beispiel für Azure SQL-Datenbank: [Datei Zielcode für erweiterte Ereignisse in SQL-Datenbank](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
