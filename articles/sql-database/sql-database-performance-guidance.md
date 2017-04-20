<properties
    pageTitle="Azure SQL-Datenbank und Leistung für einzelne Datenbanken | Microsoft Azure"
    description="In diesem Artikel helfen Ihnen festzustellen welche Dienstebene für Ihre Anwendung auswählen. Außerdem wird empfohlen, Methoden die Anwendung von Azure SQL-Datenbank zu optimieren."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/13/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-and-performance-for-single-databases"></a>Azure SQL-Datenbank und Leistung für einzelne Datenbanken

Azure SQL-Datenbank bietet drei [Service-Tiers](sql-database-service-tiers.md): Basic, Standard und Premium. Jeder Dienstebene isoliert ausschließlich die Ressourcen, die SQL-Datenbank kann und berechenbare Performance Service-Level garantiert. In diesem Artikel bieten wir Anleitung, die Auswahl die Dienstebene für Ihre Anwendung helfen. Erörtert auch Methoden, um die Anwendung optimal Azure SQL-Datenbank zu optimieren.

>[AZURE.NOTE] Dieser Artikel konzentriert sich auf Leistung Leitfaden für einzelne Datenbanken in Azure SQL-Datenbank. Leistung zu elastische Datenbank verknüpft Siehe [und Aspekte für elastische datenbankpools](sql-database-elastic-pool-guidance.md). Beachten Sie jedoch, viele der Optimierung Vorschläge in diesem Artikel auf Datenbanken in einem Datenbankpool elastische gelten und ähnliche Performance profitieren.

Dies sind die drei Dienstebenen von Azure SQL-Datenbank, die Sie auswählen können (Leistung in Datenbank durchsatzeinheiten oder [DTUs](sql-database-what-is-a-dtu.md)gemessen wird:

- **Grundlegende**. Standard-Ebene bietet gute Leistung Vorhersagbarkeit für jede Datenbank, Stunden über Stunden. In einer einfachen Datenbank Supportressourcen ausreichend gute Leistung in einer kleinen Datenbank, die mehrere gleichzeitige Anfragen.
- **Standard**. Service-Tier bietet verbesserte Leistung Vorhersagbarkeit und die Messlatte für Datenbanken, die mehrere gleichzeitige Anfragen wie Arbeitsgruppe und Web-Applikationen. Bei der Auswahl einer Ebene Service Größe kann die datenbankanwendung basierend auf vorhersagbare Leistung Minute Minuten.
- **Premium**. Premium-Service-Tier bietet eine vorhersagbare Leistung zweite über Zweitens für jeden Premium-Datenbank. Wenn Sie Premium-Service-Tier auswählen, können Sie Ihre datenbankanwendung auf Grundlage der Spitzenlast für diese Datenbank Größe. Der Plan entfernt Fällen der Abweichung kleinere Abfragen länger Wartezeit für sensible Vorgänge zu führen kann. Dieses Modell kann die Entwicklung und Validierung Zyklen für Applikationen vereinfacht die starke Aussagen Ressourcenbedarfs maximale leistungsabweichung oder Abfragewartezeit bearbeiten möchten.

Jede Schicht Service legen Sie die Leistungsstufe haben Sie die Flexibilität für die Kapazität Zahlen müssen Sie. Sie können [Kapazität anpassen](sql-database-scale-up.md), nach oben oder unten Arbeitslast. Wenn Ihre Arbeitslast Saison zum Schulbeginn zu hoch ist, können Sie die Leistungsfähigkeit der Datenbank für einen festgelegten Zeitraum Juli bis September erhöhen. Er reduzieren endet der Hauptsaison. Sie können Zahlen durch Optimierung Ihrer Cloudumgebung, die saisonbedingten Ihres Unternehmens minimieren. Dieses Modell eignet sich auch für Software-Produkt Versionszyklen. Ein Testteam kann Kapazität zugeordnet Testläufe und Kapazität lassen, wenn sie den Test abschließen. In einem Modell Kapazität Anforderung bezahlen Sie Kapazität benötigen und vermeiden auf Ressourcen, die Sie selten verwenden.

## <a name="why-service-tiers"></a>Warum service-Tiers?

Obwohl jede Arbeitslast unterscheiden kann, soll von Dienstebenen Leistung Vorhersagbarkeit auf verschiedenen Leistung bieten. Kunden mit großen Datenbank Ressourcenbedarf können in eine weitere dedizierte Umgebung arbeiten.

### <a name="common-service-tier-use-cases"></a>Service-Tier Anwendungsfälle

#### <a name="basic"></a>Grundlegende

- **Sie sind gerade erst mit Azure SQL-Datenbank**. Oft sind benötigen keine hohe Leistung. Basic-Datenbanken sind eine ideale Umgebung für die Datenbankentwicklung zu einem günstigen Preis.
- **Sie haben eine Datenbank mit einem einzelnen Benutzer**. Programme, die normalerweise einen einzelnen Benutzer eine Datenbank zuordnen benötigen nicht hoher Parallelität und Leistung. Diese Programme sind Standard-Ebene.

#### <a name="standard"></a>Standard

- **Ihre Datenbank enthält mehrere gleichzeitige Anfragen**. Programme, die mehr als ein Benutzer gleichzeitig normalerweise service benötigen höhere Performance. Beispielsweise sind Websites, die mittlere Datenverkehr oder Abteilungsebene Applikationen, die mehr Ressourcen benötigen gute Kandidaten für die Service-Ebene.

#### <a name="premium"></a>Premium

Die meisten Premium Tier Anwendungsfälle haben eine oder mehrere der folgenden Eigenschaften:

- **Hohe laden**. Eine Anwendung, die viel CPU, Arbeitsspeicher oder Eingabe/Ausgabe (e/a) auszuführenden Vorgänge erfordert eine dedizierte, leistungsfähige. Eine Datenbankoperation mehrere CPU-Kerne für längere Zeit nutzen bekannt ist z. B. ein Kandidat für die Premium-Service-Tier.
- **Viele gleichzeitige Anfragen**. Ergeben Clientanfragen viele gleichzeitige, z. B. bei einer Website, die mit ein hohen Verkehrsaufkommen hat. Einfache und standardmäßige Dienstebenen Anzahl gleichzeitiger Abfragen pro Datenbank. Programme, die mehrere Verbindungen müsste eine entsprechende Reservierung Größe die maximale Anzahl der erforderlichen Anfragen behandelt.
- **Niedrige Latenz**. Einige Programme müssen eine Antwort aus der Datenbank in kürzester Zeit garantiert. Wenn als Teil eines größeren Kunden eine bestimmte gespeicherte Prozedur aufgerufen wird, müssen Sie eine Anforderung für eine Rückkehr von diesem Aufruf in nicht mehr als 20 Millisekunden 99 Prozent der Zeit haben. Diese Art der Vorteile von Premium-Service-Tier sicherstellen, dass die erforderliche Leistung verfügbar ist.

Service hängt, die für die SQL-Datenbank Peak Load Vorschriften für jede ressourcendimension. Einige Programme eines einzelnen Ressource verwenden, aber erhebliche Anforderungen für andere Ressourcen.

## <a name="service-tier-capabilities-and-limits"></a>Service-Tier-Funktionen und Grenzen
Jedes Service-Tier und Performance-Niveau ist und anderen Eigenschaften verknüpft. Diese Tabelle beschreibt die Eigenschaften für eine einzelne Datenbank.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

In den nächsten Abschnitten sind Informationen zur Nutzung dieser Grenzwerte beziehen.

### <a name="maximum-in-memory-oltp-storage"></a>Maximale im Arbeitsspeicher OLTP-Speicher

**Sys.dm_db_resource_stats** -Ansicht können Sie Ihre Azure im Arbeitsspeicher Speicher überwachen. Weitere Informationen zur Überwachung finden Sie unter [Monitor im Arbeitsspeicher OLTP-Speicher](sql-database-in-memory-oltp-monitoring.md).

>[AZURE.NOTE] Azure Speicher = online Transaction processing (OLTP) Vorschau ist derzeit nur für einzelne Datenbanken unterstützt. Es kann nicht in Datenbanken im elastischen datenbankpools verwenden werden.

### <a name="maximum-concurrent-requests"></a>Maximale gleichzeitige Anfragen

Um die Anzahl gleichzeitiger anzuzeigen, führen Sie diese Transact-SQL-Abfrage in der SQL-Datenbank:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Analysieren die Arbeitslast einer lokalen SQL Server-Datenbank ändern Sie diese Abfrage in bestimmten Datenbank filtern analysieren möchten. Haben Sie eine lokale Datenbank mit dem Namen MyDatabase gibt der Transact-SQL-Abfrage die Anzahl gleichzeitiger Anfragen in der Datenbank an:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Dies ist eine Momentaufnahme zu einem bestimmten Zeitpunkt. Um einen besseren Überblick über Ihre Aufgaben und gleichzeitiger Anfrage erhalten, müssen Sie viele Beispiele mit der Zeit sammeln.

### <a name="maximum-concurrent-logins"></a>Maximale gleichzeitige Benutzernamen

Sie können Ihren Benutzer- und Mustern um eine Vorstellung von der Häufigkeit der Benutzernamen analysieren. Sie können auch reale Lasten ausführen, in einer Umgebung sicherstellen, dass Sie nicht treffen diese oder andere Grenzwerte, die in diesem Artikel besprochen. Es gibt eine einzelne Abfrage oder dynamic Management View (DMV), die gleichzeitig zeigen kann Login zählt oder Geschichte.

Wenn mehrere Clients dieselbe Verbindungszeichenfolge verwenden, authentifiziert der Dienst jede Anmeldung. 10 Benutzer gleichzeitig mit einer Datenbank verbinden mit demselben Benutzernamen und Kennwort, wäre 10 gleichzeitige Benutzernamen. Dieser Grenzwert gilt nur für die Dauer der Anmeldung und Authentifizierung. Die gleichen 10 Benutzer nacheinander mit der Datenbank verbinden, wäre die Anzahl gleichzeitiger nie größer als 1.

>[AZURE.NOTE] Derzeit gilt diese Beschränkung nicht für Datenbanken im elastischen datenbankpools.

### <a name="maximum-sessions"></a>Maximale sessions

Um die Anzahl der aktuellen aktiven Sessions anzuzeigen, führen Sie diese Transact-SQL-Abfrage in der SQL-Datenbank:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Wenn Sie eine lokale SQL Server-Auslastung analysieren, ändern Sie die Abfrage auf eine bestimmte Datenbank zu konzentrieren. Diese Abfrage können Sie die möglichen Sitzung muss für die Datenbank bestimmen, wenn Sie in Azure SQL-Datenbank verschieben.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Diese Abfragen zurückzugeben Point-in-Time-Anzahl. Wenn Sie mehrere Beispiele Zeitverlauf erfassen, müssen Sie das Verstehen der Sitzung verwenden.

SQL Datenbank-Analyse erhalten Sie Verlaufsdaten Statistiken Sessions. **Resource_stats**Abfragen und mithilfe der **Active_session_count** -Spalte. Anzeigen Sie im nächsten Abschnitt Weitere Informationen in dieser Ansicht

## <a name="monitor-resource-use"></a>Überwachen der Ressourcenverwendung
Zwei Ansichten können Sie Ressourcenverwendung für eine SQL-Datenbank gegenüber der Dienstebene überwachen können:

- [Sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>Sys.dm_db_resource_stats
Die [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) -Ansicht können Sie in jeder SQL-Datenbank. Die **sys.dm_db_resource_stats** -Ansicht zeigt aktuelle Ressource mit Daten der Dienstebene. Durchschnittliche Prozentsätze für CPU, Daten i/o Protokollschreibvorgänge und Speicher werden alle 15 Sekunden und 1 Stunde gewartet.

Da diese Ansicht eine genauere Betrachtung Ressourcen bereitstellt, verwenden Sie **sys.dm_db_resource_stats** zum Stand Analyse oder zur Problembehandlung. Diese Abfrage zeigt z. B. die durchschnittliche und maximale Ressourcenverwendung für die aktuelle Datenbank in der letzten Stunde:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Andere Abfragen finden Sie in den Beispielen in [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>resource_stats

[Resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) auf der **master** -Datenbank verfügt über weitere Informationen, mit denen Sie die Leistung Ihrer SQL-Datenbank auf die bestimmte Tier und Leistung überwachen kann. Die Daten werden alle 5 Minuten und ca. 35 Tage beibehalten. Diese Ansicht eignet sich für eine längerfristige Verlaufsanalyse der Verwendung von Ressourcen in der SQL-Datenbank.

Der folgende Graph zeigt die CPU-Ressourcenverwendung für eine Premium-Datenbank mit P2 Leistung für jede Stunde in der Woche. In diesem Diagramm beginnt an einem Montag zeigt 5 Arbeitstage und zeigt ein Wochenende, wenn die Anwendung weniger aufgefüllt.

![SQL Datenbank Ressourcenverwendung](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Die Daten dieser Datenbank derzeit verfügt eine maximale CPU-Auslastung nur 50 Prozent CPU-Nutzung relativ Leistungsstufe P2 (mittags am Dienstag). Wenn CPU der bestimmende Faktor Anwendungsprofil Ressource ist, können Sie entscheiden, dass P2 die Leistung ist zu garantieren, dass die Arbeitslast immer passt. Wenn Sie eine Anwendung mit der Zeit erwarten, ist eine gute Idee, einen zusätzliche Ressource Puffer, damit die Anwendung jemals Leistungsstufe Grenzwert erreichen. Wenn Sie die Leistung erhöhen, unterstützen Sie Kunden sichtbar Fehler zu vermeiden, die auftreten können, wenn eine Datenbank nicht ausreichend Strom zu Anfragen, insbesondere Wartezeit Sensitive Environments. Ein Beispiel ist eine Datenbank, die eine Anwendung unterstützt, die basierend auf den Ergebnissen der Datenbankaufrufe Webseiten zeichnet.

Beachten Sie, dass andere Anwendung dieselbe Währungseinheit unterschiedlich interpretieren können. Z. B. wenn eine Anwendung Lohndaten täglich verarbeiten und demselben Diagramm, empfiehlt diese Art von "Stapelverarbeitung" Modell gut Ebene Leistung P1. Leistungsstufe P1 hat 100 DTUs verglichen mit 200 DTUs Ebene Leistung P2. Leistungsstufe P1 bietet halbe Leistung P2-Leistungsfähigkeit. Folglich entspricht 50 Prozent der CPU-Verwendung in P2 100 Prozent CPU-Auslastung P1. Wenn die Anwendung keine Timeouts, möglicherweise keine Rolle nimmt ein Auftrag 2 Stunden oder 2,5 Stunden, wenn heute erledigt. Eine Anwendung in dieser Kategorie können wahrscheinlich eine Leistungsstufe P1. Sie können die Tatsache nutzen gibt es Zeiträume während des Tages Ressourcenverwendung niedriger wird, damit alle "große Spitze" eines der Täler später übergreifen könnte. Leistungsstufe P1 möglicherweise als die Aufträge pro Tag beendet können für diese Art von Anwendung und sparen.

Azure SQL-Datenbank macht verbraucht Ressourceninformationen für jeden aktiven Datenbank an **resource_stats** der **master** -Datenbank auf jedem Server. Die Daten in der Tabelle ist für 5 Minuten zusammengefasst. Mit Dienstebenen Basic, Standard und Premium nehmen die Daten in der Tabelle angezeigt werden, damit diese Daten für Analyse als nahe-Echtzeit-Analysen von mehr als 5 Minuten. Abfrage der **resource_stats** Ansicht die Geschichte der Datenbank anzeigen und überprüfen, ob die Reservierung gewählte übermittelt die Leistung möchten bei Bedarf.

>[AZURE.NOTE] Sie müssen mit **der Masterdatenbank logischen SQL Datenbankserver **resource_stats** in den folgenden Beispielen Abfrage** verbunden.

Dieses Beispiel zeigt, wie die Daten in dieser Ansicht verfügbar gemacht werden:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Anzeigen der resource_stats](./media/sql-database-performance-guidance/sys_resource_stats.png)

Das nächste Beispiel zeigt verschiedene Arten, mit denen Sie die Katalogansicht **resource_stats** Informationen zur Verwendung von Ressourcen in der SQL-Datenbank:

1. Betrachten der letzten Woche Ressource für Datenbank-userdb1 verwenden, können Sie diese Abfrage ausführen:

        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;

2. Um Ihre Arbeitslast die Leistungsstufe passt zu bewerten, müssen Sie jeden Aspekt der ressourcenmetriken Drilldown: CPU, Lesevorgänge schreibt, Anzahl der Arbeitskräfte und Anzahl der Sessions. Hier ist eine überarbeitete Abfrage **resource_stats** die durchschnittlichen und maximalen Werte dieser Ressource Metriken gemeldet:

        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

3. Mit diesen Informationen über die durchschnittlichen und maximalen Werte jeder Ressource Metrik können Sie beurteilen, wie gut Ihre Arbeitslast die Leistungsstufe passt gewählte. Durchschnittswerte aus **resource_stats** geben in der Regel eine solide gegen die Zielgröße. Es ist Ihre primäre Messung Stick. Beispielsweise verwenden Sie möglicherweise die Dienstebene Standard mit S2 Leistung. Der Durchschnitt verwendet Prozentsätze CPU- und e/a-Lesevorgänge Schreibvorgänge sind unter 40 Prozent, ist die durchschnittliche Anzahl der Arbeitnehmer unter 50 und die durchschnittliche Anzahl der Sessions ist 200. Die Arbeitslast möglicherweise Leistungsstufe S1 passen. Es ist leicht zu erkennen, ob die Datenbank in das Arbeits- und Sitzung entspricht. Um festzustellen, ob eine Datenbank einen geringeren Leistung in Bezug auf CPU, passt liest und schreibt, die DTU Anzahl der unteren Leistungsstufe DTU Anzahl der aktuellen Leistungsstufe und Multiplizieren Sie das Ergebnis mit 100:

    * *S1 DTU / S2 DTU* 100 = 20 / 50* 100 = 40 **

    Das Ergebnis ist der relativen Leistungsunterschiede zwischen zwei Leistungsmerkmale in Prozent. Die Ressourcenverwendung diesen Betrag überschreitet, kann Ihre Arbeitslast in der geringeren Leistung passt. Allerdings müssen Sie alle Wertebereiche Ressource verwendet, und bestimmen, Prozentsatz, wie oft Ihre Arbeitslast in der geringeren Leistung passen. Die folgende Abfrage gibt den prozentualen pro ressourcendimension basierend auf den Schwellenwert von 40 Prozent, die wir in diesem Beispiel berechnet:

        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Basierend auf der Datenbank-SLO (SLO), können Sie entscheiden, ob Ihre Arbeitslast in der geringeren Leistung entspricht. Wenn Ihre Arbeitslast SLO 99,9 Prozent und die vorherige Abfrage Werte größer als 99,9 % für alle drei Ressourcendimensionen gibt, passt Ihre Arbeitslast wahrscheinlich in der geringeren Leistung.

    Betrachten der prozentualen Ihnen außerdem Einblick in die höhere Leistungsfähigkeit zu Ihrem SLO verschieben muss. Userdb1 zeigt z. B. die folgende CPU-Auslastung für die Woche:

  	| Durchschnittliche CPU-Prozentsatz | Maximale CPU-Prozentsatz |
  	|---|---|
  	| 24,5 | 100,00 |

    Durchschnittliche CPU ist etwa zu einem Viertel der Grenze der Leistungsfähigkeit, die in die Leistungsfähigkeit der Datenbank passen. Aber der maximale Wert zeigt an, dass die Datenbank die Leistungsstufe erreicht. Müssen Sie die höhere Leistungsfähigkeit verschieben? Erläutert, wie oft die Auslastung erreicht 100 Prozent, und vergleichen Sie ihn mit der Arbeitslast SLO werden müssen.

        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent’
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Wenn diese Abfrage einen Wert zurückgibt weniger als 99,9 % aller drei Ressourcendimensionen entweder verschieben auf die nächsthöhere Leistung oder Anwendung optimieren Techniken reduzieren die Belastung der SQL-Datenbank.

4. In dieser Übung ferner Ihre voraussichtliche Auslastung erhöhen in der Zukunft.

## <a name="tune-your-application"></a>Optimieren der Anwendung

In herkömmlichen lokalen SQL Server trennt Anfangskapazität Planungsprozess häufig aus eine Anwendung in der Produktion ausgeführt. Hardware- und Lizenzen werden zuerst erworben und Performance-Optimierung erfolgt später. Bei Verwendung von Azure SQL-Datenbank ist eine gute Idee, der Ausführung einer Anwendung und Optimierung verknüpfen. Mit dem Modell für bedarfsorientierte Kapazität Zahlen können Sie Ihre Anwendung mit minimalen Ressourcen benötigt, statt Bereitstellung überproportional vieler Hardware basierend auf Prognosen Wachstumspläne für eine Anwendung, die häufig falsch einstellen. Kunden können festlegen, dass eine Anwendung nicht und stattdessen übermäßige Hardwareressourcen. Dieser Ansatz wäre sinnvoll eine wichtige Anwendung ausgelastet Zeitraum ändern möchten. Aber Optimieren einer Anwendung kann Ressourcen und niedrigere monatliche Rechnungen beim Minimieren die Dienstebenen in Azure SQL-Datenbank verwenden.

### <a name="application-characteristics"></a>Eigenschaften

Obwohl Dienstebenen Azure SQL-Datenbank zur Verbesserung der Leistungsstabilität und Vorhersagbarkeit für eine Anwendung dienen können einige bewährte Methoden der Anwendung besser Ressourcen an einen nutzen helfen. Zwar vielen erhebliche Leistungssteigerungen durch wechseln zu einer höheren Leistung oder Service-Tier Anwendungsbeispiele zusätzliche Abstimmung besseren Service profitieren. Zur Verbesserung der Leistung sollten Sie zusätzliche Optimierung für Programme, die diese Merkmale aufweisen:

- **Programme, deren Leistung wegen "weitschweifender"**. Ausführliche Anträge stellen übermäßige Datenzugriffsoperationen die Netzwerklatenz. Sie müssen diesen Anwendungstypen Anzahl Datenzugriffsoperationen zur SQL-Datenbank zu ändern. Sie können z. B. Anwendungsleistung verbessern, indem wie Batchverarbeitung Ad-hoc-Abfragen oder Abfragen in gespeicherten Prozeduren verschieben. Weitere Informationen finden Sie unter [Batchabfragen](#batch-queries).
- **Datenbanken mit einer Arbeitslast, die durch einen vollständigen einzelnen Computer unterstützt werden kann**. Datenbanken, die die Ressourcen der höchsten Ebene der Premium-Performance profitieren können Arbeitslast skalieren. Weitere Informationen finden Sie unter [Cross - datenbanksharding](#cross-database-sharding) und [funktionalen Partitionierung](#functional-partitioning).
- **Anträge, die suboptimale Fragen**. Clientanwendungen können insbesondere die Datenzugriffsschicht, die schlecht Abfragen optimiert haben keine höhere Performance profitieren. Dies schließt Abfragen, die keine WHERE-Klausel, fehlenden Indizes oder veraltete Statistiken. Diese Programme profitieren von Abfragen optimieren Techniken. Weitere Informationen finden Sie unter [fehlende Indizes](#missing-indexes) und [Abfrage optimieren und Hinweise](#query-tuning-and-hinting).
- **Anträge, die suboptimale Daten haben, Zugriff auf Entwurf**. Anträge, die inhärente Data Access Parallelitätsprobleme z. B. Deadlocks können eine höhere Leistung nicht profitieren. Verringern Sie Roundtrips Azure SQL-Datenbank durch Zwischenspeichern von Daten auf dem Client mit der Azure Caching-Dienst oder einer anderen Technologie Zwischenspeichern. Siehe [Anwendung Tier Zwischenspeichern](#application-tier-caching).

## <a name="tuning-techniques"></a>Optimieren von Verfahren
In diesem Abschnitt betrachten wir einige Techniken Tune Azure SQL-Datenbank um die beste Leistung für Ihre Anwendung und auf der niedrigsten Ebene der Leistung führen. Einige dieser Techniken herkömmlichen SQL Server-Optimierung best Practices entsprechen, aber andere sind Azure SQL-Datenbank. In einigen Fällen können Sie überprüfen die belegten Ressourcen für eine Datenbank zu Bereichen weiter optimieren und ausbauen von herkömmlichen SQL Server-Techniken in Azure SQL-Datenbank arbeiten.

### <a name="azure-portal-tools"></a>Azure-Portal-tools
Finden Sie zwei Tools im Azure-Portal, das Ihnen helfen können analysieren und Beheben von Leistungsproblemen SQL-Datenbank:

- [Einen Einblick Abfrage-Leistung](sql-database-query-performance.md)
- [SQL Datenbank Advisor](sql-database-advisor.md)

Azure-Portal enthält weitere Informationen zu diesen Tools und deren Verwendung. Effizient diagnostizieren und beheben Sie Probleme sollten zunächst die Tools im Azure-Portal. Wir empfehlen, manuelle Abstimmung Ansätze, die wir besprechen Sie fehlende Indizes und Optimieren von Abfragen in besonderen Fällen zu verwenden.

### <a name="missing-indexes"></a>Fehlende Indizes
Ein häufiges Problem in OLTP-Datenbank-Performance bezieht sich auf den physischen Datenbankentwurf. Häufig werden Datenbankschemas entwickelt und ohne Ebene (oder laden Datenmengen) ausgeliefert. Die Leistung der Abfrageplan möglicherweise werden auf kleinen leider unter Produktion Datenmengen erheblich beeinträchtigen. Die häufigste Quelle dieses Problems fehlt geeignete Indizes zu filtern oder andere Einschränkungen in einer Abfrage. Fehlende Indizes Manifeste als Tabelle häufig bei eine Indexsuche ausreichend konnte scannen.

In diesem Beispiel verwendet der ausgewählten Abfrageplan einen Scan beim Seek ausreichen:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Ein Abfrageplan mit fehlenden Indizes](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL-Datenbank helfen suchen und korrigieren allgemeine fehlen Conditions index Ihnen. DMVs Azure SQL-Datenbank integriert betrachten Abfrage Kompilierungen in denen Index reduziert die Kosten zum Ausführen einer Abfrage würde. Während der Abfrage verfolgt SQL Datenbank wie oft jeden Abfrageplan wird ausgeführt und geschätzte Lücke zwischen der ausgeführten Abfrage-Plan und dem vorstellen, war dieser Index verfolgt. Diese DMVs können schnell erraten welche Änderungen physischen Datenbankentwurf Arbeitslast Kosten für eine Datenbank und der tatsächlichen Auslastung verbessern können.

Diese Abfrage können Sie mögliche fehlende Indizes auswerten:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

In diesem Beispiel führte die Abfrage Vorschlag:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Nachdem es erstellt wurde, nimmt diese einer SELECT-Anweisung einen anderen Plan verwendet ein anstelle eines Scans und effizienter Ausführen des Plans:

![Ein Abfrageplan korrigierte Indizes](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

Die wichtigste Erkenntnis ist die e/a-Kapazität eines freigegebenen Commodity Systems als dedizierter Server-Computer. Gibt es eine Prämie Minimierung der unnötige/a, um das System in der DTU jeder Leistungsstufe Dienstebenen Azure SQL-Datenbank nutzen. Entsprechenden physischen Datenbankentwurf Optionen können deutlich verbessern die Wartezeit für einzelne Abfragen Durchsatz gleichzeitige Anfragen pro Skalierungseinheit verbessern und senken Sie die Kosten für die Abfrage erforderlich. Weitere Informationen zu fehlenden Index DMVs finden Sie unter [dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Optimieren von Abfragen und Hinweise
Der Abfrageoptimierer in Azure SQL-Datenbank ist ähnlich der herkömmlichen SQL Server-Abfrageoptimierer. Die meisten der best Practices für Abfragen optimieren und Verstehen der Argumentation Modell für den Abfrageoptimierer auch Einschränkungen auf Azure SQL-Datenbank. Wenn Sie Abfragen in Azure SQL-Datenbank einstellen, erhalten Sie den zusätzlichen Vorteil der Reduzierung aggregate Ressource erfordert. Die Anwendung möglicherweise kostengünstiger als eine ungestimmte Entsprechung ausgeführt werden, da auf einer niedrigeren Leistung ausgeführt werden kann.

Ein Beispiel, in SQL Server häufig und das gilt auch für Azure SQL-Datenbank, ist wie der Abfrageoptimierer "überprüft" Parameter. Während der Kompilierung wertet der Abfrageoptimierer den aktuellen Wert des Parameters zu bestimmen, ob es einen optimalen Abfrage-Plan erzeugen. Obwohl diese Strategie oft zu Abfrageplan, die schneller als ein Plan ohne bekannte Parameterwerte kompiliert führen, es arbeitet unvollständig sowohl in SQL Server und Azure SQL-Datenbank. Manchmal Parameters ist nicht Schnupfen, und manchmal Parameters Schnupfen generierten ist für den vollständigen Satz von Parameterwerten in einer Arbeitslast suboptimale Microsoft schließt Abfragehinweise (Richtlinien), sodass Absicht gezielter angeben und das Standardverhalten des Parameter-sniffing überschreiben. Häufig können Hinweise verwenden, Sie Anfragen beheben, in denen das Standardverhalten für SQL Server oder Azure SQL-Datenbank für einen bestimmten Debitor Arbeitslast unvollständig ist.

Das nächste Beispiel zeigt, wie der Abfrageprozessor einen Plan generieren, die nicht optimalen Leistung und Ressourcenbedarf. Dieses Beispiel zeigt auch, verwenden Sie einen Abfragehinweis Abfrage ausführen und Ressourcen Vorschriften für die SQL-Datenbank zu verringern, können:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

Dieser Setup-Code erstellt eine Tabelle, die Verteilung geneigt ist. Optimale Abfrageplan unterscheidet sich abhängig von der Parameter ausgewählt wird. Leider Kompilieren nicht Cacheverhalten Plan immer die Abfrage basierend auf der gebräuchlichsten Parameterwert. So kann für einen nicht optimalen Plan zwischengespeichert und für viele Werte auch verwendet, wenn ein anderen Plan Plan besser durchschnittlich möglicherweise. Der Abfrageplan zwei gespeicherte Prozeduren erstellt, die identisch, außer dass eine besondere Abfragehinweis.

**Beispiel, Teil 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Beispiel, Teil 2**

(Wir empfehlen, dass die Ergebnisse in der resultierenden Telemetriedaten sind, warten mindestens 10 Minuten vor der Teil 2 des Beispiels.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Jeder Teil dieses Beispiel versucht, eine parametrisierte Insert-Anweisung 1000 Mal ausführen (um eine ausreichende Last als Test-DataSet verwenden generieren). Wenn sie gespeicherte Prozeduren ausgeführt wird, überprüft der Abfrageprozessor Parameterwert, der an die Prozedur übergeben wird, während die erste Kompilierung (Parameter "sniffing"). Der Prozessor resultierenden Plan zwischengespeichert und für spätere Aufrufe verwendet, auch wenn der Parameterwert unterscheidet. Eine optimale Planung möglicherweise nicht in allen Fällen verwendet werden. Manchmal müssen Sie den Optimierer einen Plan wählen, besser für den durchschnittlichen Fall als Sonderfall bei der ersten Kompilieren der Abfrage führen. In diesem Beispiel wird der ursprüngliche Plan einen "Scan"-Plan, der liest alle Zeilen, um jeden Wert suchen, den Parameter entspricht:

![Fragen Sie mit einem Scan-Plan Optimieren von ab](./media/sql-database-performance-guidance/query_tuning_1.png)

Da wir die Prozedur ausgeführt, mit dem Wert 1, der resultierende Plan für den Wert 1 aber suboptimale für alle anderen Werte in der Tabelle. Wahrscheinlich das Ergebnis nicht, was Sie wollen, würden Sie jeden Plan auswählen, da der Plan mehr langsam und weitere Ressourcen.

Wenn der Testlauf mit `SET STATISTICS IO` soll `ON`, der logische Prüfung in diesem Beispiel erfolgt im Hintergrund. Sie können sehen, dass es 1.148 Lesevorgänge von diesem Plan (ist ineffizient, wenn der durchschnittliche Fall ist nur eine Zeile zurückgeben) durchgeführt:

![Fragen Sie mithilfe eines logischen Scans Optimieren von ab](./media/sql-database-performance-guidance/query_tuning_2.png)

Der zweite Teil des Beispiels verwendet einen Abfragehinweis anzuweisen, den Optimierer einen bestimmten Wert während der Kompilierung verwendet. In diesem Fall erzwingt den Abfrageprozessor den Wert ignoriert werden soll, die als Parameter übergeben wird und an `UNKNOWN`. Dies bezieht sich auf einen Wert mit der Häufigkeit der Tabelle (Neigung wird ignoriert). Die resultierende ist ein Seek-basierte Plan, der schneller und verwendet weniger Ressourcen durchschnittlich als Plan in Teil 1 dieses Beispiels:

![Optimieren von Abfragen einen Abfragehinweis mit](./media/sql-database-performance-guidance/query_tuning_3.png)

Sehen Sie den Effekt in der **resource_stats** Tabelle (es gibt eine Verzögerung von der Ausführung des Tests und wenn die Daten die Tabelle aufgefüllt). Für dieses Beispiel während des Zeitfensters 22:25:00 Teil 1 und Teil 2 22:35:00 ausgeführt. Beachten Sie, dass das frühere Fenster mehr Ressourcen in dieser Zeit als höher (durch Verbesserung der Plan).

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Abfrage Ergebnisse optimieren](./media/sql-database-performance-guidance/query_tuning_4.png)

>[AZURE.NOTE] Obwohl in diesem Beispiel absichtlich klein ist, kann der Effekt suboptimale Parameter insbesondere bei größeren Datenbanken erheblich sein. Die Differenz in extremen Fällen kann schnell Fällen Sekunden bis Stunden langsam Fällen.

Überprüfen Sie **resource_stats** , um festzustellen, ob die Ressource für einen Test mehr oder weniger Ressourcen als anderen Tests verwendet. Beim Vergleichen von Daten trennen Sie das Timing der Tests so, dass sie nicht im selben Fenster 5 Minuten an **resource_stats** . Das Ziel dieser Übung ist zum Minimieren des verwendeten Ressourcen und nicht zu Spitzenzeiten Ressourcen. Im Allgemeinen reduziert die Optimierung eines Codeabschnitts Wartezeit auch Ressourcenverbrauch. Stellen Sie sicher, dass die Änderungen an einer Anwendung notwendig sind und negativ auf die Kundenzufriedenheit für jemanden beeinflussen nicht Benutzer Abfragehinweise in der Anwendung.

Hat eine Arbeitslast eine Gruppe wiederholter Abfragen ist häufig es sinnvoll und Optionen ausgewählten Plan überprüfen, da minimale Ressourcen Größeneinheit zum Hosten der Datenbank erforderlichen Laufwerke. Nach einer Validierung, gelegentlich nochmals Pläne können Sie sicherstellen, dass sie nicht beschädigt sind. Sie erhalten weitere Informationen zu [Abfragehinweisen (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Cross-datenbanksharding
Da Standardhardware Azure SQL-Datenbank ausgeführt wird, werden die Grenzen für eine einzelne Datenbank geringer als bei einer herkömmlichen lokalen SQL Server-Installation. Einige Kunden Hilfe Sharding Techniken Datenbanken Datenbank verteilt, wenn Vorgänge innerhalb der Grenzen einer einzelnen Datenbank in Azure SQL-Datenbank passen. Die meisten Benutzer Sharding Techniken in Azure SQL-Datenbank ihre Daten in einer einzigen Dimension in Datenbanken aufgeteilt. Bei dieser Vorgehensweise müssen Sie verstehen, OLTP-Anwendung häufig Transaktionen auszuführen, die nur eine Zeile oder eine kleine Gruppe von Zeilen im Schema anwenden.

>[AZURE.NOTE] SQL-Datenbank enthält jetzt eine Bibliothek Sharding bei. Weitere Informationen finden Sie unter [Bibliothek Kundenübersicht elastische Datenbank](sql-database-elastic-database-client-library.md).

Beispielsweise weist eine Datenbank, Kundenname, Auftrag und Auftragsdetails (wie herkömmliche Beispieldatenbank Northwind, die mit SQL Server), konnte Sie diese Daten mehrere Datenbanken aufgeteilt durch Kunden und die zugehörige Bestellung Bestelldetailinformationen gruppieren. Sie können sicherstellen, dass die Kundendaten in einer einzelnen Datenbank bleibt. Die Anwendung würde Kunden auf Datenbanken effektiv Verteilung der Last auf mehrere Datenbanken aufgeteilt. Sharding Kunden können nicht nur die maximale Datenbankgröße vermeiden jedoch Azure SQL-Datenbank kann auch erheblich größer als die Grenzen der anderen Leistungsmerkmale sind als die DTU jeder einzelnen Datenbank passt Arbeitslasten verarbeiten.

Datenbanksharding aggregate Ressourcenkapazität für eine Lösung reduziert, es ist zwar dringend Unterstützung sehr große Projektmappen Datenbanken verteilt sind. Jede Datenbank kann eine unterschiedliche Performance auf sehr große Unterstützung "geltende" Datenbanken mit hohen Ressourcenbedarf ausgeführt.

### <a name="functional-partitioning"></a>Funktionale Partitionierung
SQL Server-Benutzer kombinieren häufig viele Funktionen in einer einzelnen Datenbank. Beispielsweise verfügt eine Anwendung Logik für die Lagerverwaltung für einen Speicher, die Datenbank möglicherweise zugeordnete Inventory tracking Bestellungen, gespeicherte Prozeduren und indizierte oder materialisierte Ansichten, die Ende des Monats Berichte verwalten. Dieses Verfahren erleichtert die Verwaltung der Datenbank für Vorgänge wie Backup, aber auch müssen Sie der Hardware Spitzenbelastungen über alle Funktionen einer Anwendung zu behandeln.

Bei Verwendung eine Scale-Out-Architektur in Azure SQL-Datenbank ist eine gute Idee, verschiedene Funktionen einer Anwendung in verschiedenen Datenbanken aufgeteilt. Mit dieser Technik wird jede Anwendung einzeln skaliert. Wie eine Anwendung ausgelastet wird und erhöht die Belastung der Datenbank können Administrator unabhängige Ebenen für jede Funktion in der Anwendung. An der Grenze mit dieser Architektur kann eine Anwendung mehr als ein einzige Ware Computer verarbeiten kann, da die Last auf mehrere Computer verteilt wird.

### <a name="batch-queries"></a>Batchabfragen
Anwendung, die mit umfangreicher Daten zuzugreifen, ist ad-hoc-Abfragen häufig eine beträchtliche Zeit Netzwerkkommunikation zwischen der Anwendungsebene und der Datenbankebene Azure SQL ausgegeben. Auch wenn die Anwendung und Azure SQL-Datenbank im selben Rechenzentrum sind möglicherweise die Netzwerklatenz zwischen den beiden durch eine große Anzahl von Daten Zugriffsoperationen vergrößert werden. Netzwerk zu Reisen runde für die Datenzugriffsoperationen, verwenden die Option entweder batch-ad-hoc-Abfragen oder sie gespeicherte Prozeduren erstellen. Wenn Stapel ad-hoc-Abfragen können Sie mehrere Abfragen als eine große Charge in einer einzigen Azure SQL-Datenbank senden. Kompilieren von ad-hoc-Abfragen in einer gespeicherten Prozedur zu konnte das gleiche Ergebnis erreichen, sofern Sie batch. Mithilfe einer gespeicherten Prozedur profitieren auch Sie erhöhen die Wahrscheinlichkeit für das Zwischenspeichern von Abfrageplänen in Azure SQL-Datenbank die gespeicherte Prozedur erneut verwenden können.

Einige Programme sind schreiben. Manchmal können Sie die gesamte e/a-Last einer Datenbank reduzieren, indem wie schreibt batch zusammengefasst werden. Häufig ist dies einfach anstelle von Autocommit-Transaktionen in gespeicherten Prozeduren und ad-hoc-Batches explizite Transaktionen. Eine Bewertung Techniken, die Sie verwenden können, finden Sie unter [Batching Techniken zur Anwendung in Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Experimentieren Sie mit eigenen Modell für die Batchverarbeitung finden. Müssen Sie wissen, dass ein Modell möglicherweise geringfügig Transaktionskonsistenz gewährleistet. Die Rechte Arbeitslast, die Ressourcenverwendung minimiert müssen die richtige Kombination von Konsistenz und Leistung Kompromisse.

### <a name="application-tier-caching"></a>Zwischenspeichern der Anwendungsebene
Ergeben haben Lesevorgänge Arbeitslasten. Ebenen Zwischenspeichern kann die Datenbank entlasten und könnte möglicherweise herabsetzen Performance erforderlich, um eine Datenbank mithilfe von Azure SQL-Datenbank unterstützen. Mit [Azure Redis Cache](https://azure.microsoft.com/services/cache/)haben eine Arbeitslast Lesevorgänge Sie können die Daten einmal lesen (oder einmal pro Anwendungsebenencomputer je nach Konfiguration), und die Daten außerhalb der SQL-Datenbank speichern. Dies ist eine Möglichkeit zu Datenbank laden (CPU- und e/a Read) besteht jedoch auf Transaktionskonsistenz möglicherweise aus dem Cache gelesenen Daten mit den Daten in der Datenbank. In vielen gewisse Inkonsistenz ist akzeptabel, gilt das nicht für alle Arbeitslasten. Sie sollten alle Anforderungen verstehen, vor der Implementierung einer Zwischenspeicherungsstrategie Anwendungsebene.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen über Service-Tiers hinweg finden Sie unter [SQL-Datenbank und Leistung](sql-database-service-tiers.md)
- Weitere Informationen zu elastische datenbankpools finden Sie unter [Was ist ein Datenbankpool Azure elastische?](sql-database-elastic-pool.md)
- Informationen zu Leistung und elastische datenbankpools finden Sie [einen Datenbankpool elastische berücksichtigen](sql-database-elastic-pool-guidance.md)
