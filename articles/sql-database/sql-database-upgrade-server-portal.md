<properties
    pageTitle="Upgrade auf Azure SQL-Datenbank V12 Azure-Portal mit | Microsoft Azure"
    description="Erläutert die Azure SQL-Datenbank V12 upgrade einschließlich Web- und Datenbanken und ein V11 Server Datenbanken direkt in einem Pool elastische Datenbanken mit Azure-Portal migrieren."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/08/2016"
    ms.author="sstein"/>


# <a name="upgrade-to-azure-sql-database-v12-using-the-azure-portal"></a>Upgrade auf Azure SQL-Datenbank V12 mit Azure-Portal


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


SQL Datenbank V12 ist die neueste Version Aktualisieren vorhandener Server auf SQL Datenbank V12 empfiehlt.
SQL Datenbank V12 hat viele [Vorteile gegenüber der vorherigen Version](sql-database-v12-whats-new.md) einschließlich:

- Verbesserte Kompatibilität mit SQL Server.
- Verbesserte Leistung und neue Leistungsmerkmale.
- [Elastische datenbankpools](sql-database-elastic-pool.md).

Dieser Artikel beschreibt, wie Sie für das Aktualisieren von vorhandenen Datenbank V11 SQL Server und Datenbanken auf SQL-Datenbank V12.

Bei der Aktualisierung auf V12 aktualisieren Sie Web- und Datenbanken auf neue Service erfahren Sie, wie Web Datenbanken aktualisieren enthalten sind.

Zusätzlich möglich zu einem [Pool für elastische Datenbanken](sql-database-elastic-pool.md) migrieren kostengünstiger als die Aktualisierung einzelner Leistung (Preisgestaltung Ebenen) für einzelne Datenbanken. Pools vereinfachen auch Datenbank brauchen Sie nur die Leistung Einstellungen für den Pool nicht separat verwalten die Leistungsfähigkeit der einzelnen Datenbanken. Wenn Sie Datenbanken auf mehreren Servern sollten Sie in demselben Server verschieben und Inbetriebnahme eines Pools nutzen. Sie können [Datenbanken V11 Server direkt in Pools für elastische Datenbanken mithilfe von PowerShell automatisch migriert](sql-database-upgrade-server-powershell.md). Sie können auch das Portal um V11 Datenbanken in einem Pool zu migrieren, aber im Portal muss bereits einen V12 Server einen Pool erstellen. Routen werden in diesem Artikel zum Erstellen des Pools nach dem Serverupgrade bei [Datenbanken, die einen Pool profitieren,](sql-database-elastic-pool-guidance.md)bereitgestellt.

Beachten Sie, dass die Datenbanken online bleiben und weiterhin in den Aktualisierungsvorgang. Zum Zeitpunkt der tatsächlichen Umstellung auf die neue temporäre Verbindung zur Datenbank ablegen Leistungsfähigkeit kann kann eine sehr kleine Dauer, die in der Regel etwa 90 Sekunden passieren jedoch 5 Minuten. Wenn die Anwendung [für die Verbindung Beendigungen vorübergehender Fehler](sql-database-connectivity-issues.md) ist ausreichend Schutz Verbindungen am Ende der Aktualisierung unterbrochen.

SQL Datenbank V12 aktualisieren kann nicht rückgängig gemacht werden. Nach einer Aktualisierung kann der Server V11 wiederhergestellt werden.

Nach der Aktualisierung auf V12 werden [Service-Tier-Empfehlung](sql-database-service-tier-advisor.md) und [Leistungsaspekte elastischen Pool](sql-database-elastic-pool-guidance.md) nicht sofort verfügbar bis der Dienst Zeit zum Auswerten der Arbeitslasten auf dem neuen Server. V11 Server Empfehlung Geschichte gilt nicht für V12-Servern, nicht beibehalten.

## <a name="prepare-to-upgrade"></a>Vorbereiten der Aktualisierung

- **Alle Web- und Datenbanken**: Siehe untenstehenden oder finden Sie [Alle Web- und Datenbanken](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) [Überwachen und Verwalten einer elastischen Datenbank (PowerShell)](sql-database-elastic-pool-manage-powershell.md).
- **Überprüfen und Geo-Replikation**: Wenn Ihre Azure SQL-Datenbank für Geo-Replikation konfiguriert ist sollten Sie Dokumentieren der aktuellen Konfiguration und [Geo-Replikation beenden](sql-database-geo-replication-portal.md#remove-secondary-database). Nach die Aktualisierung neu Geo-Replikation konfigurieren Sie die Datenbank.
- **Öffnen Sie diese Ports, wenn Clients auf Azure-VM**: Wenn Ihr Clientprogramm SQL Datenbank V12 verbindet, während der Client auf eine Azure Virtual Machine (VM) ausgeführt wird, müssen Sie Anschlussbereiche 11000 11999 und 14000-14999 auf dem virtuellen Computer öffnen. Einzelheiten finden Sie [Ports für SQL Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).



## <a name="start-the-upgrade"></a>Starten des Upgrades

1. [Azure-Portal](https://portal.azure.com/) suchen auf den Server aktualisieren, indem Sie die Option **Durchsuchen**soll > **SQL Server**und Auswählen des v2. 0 Servers aktualisieren möchten.
2. Wählen Sie **Neueste SQL Datenbank**, und wählen Sie **diesen Server aktualisieren**.

      ![Server aktualisieren][1]

3. Aktualisieren einen Server auf das neueste SQL Datenbank ist dauerhaft und nicht rückgängig gemacht werden. Um die Aktualisierung zu bestätigen, geben Sie den Namen des Servers und klicken Sie auf **OK**.

## <a name="upgrade-all-web-and-business-databases"></a>Alle Web- und Datenbanken

Wenn Ihr Server Web oder von Datenbanken müssen Sie diese aktualisieren. Bei der Aktualisierung auf SQL Datenbank V12 aktualisieren Sie alle Web Datenbanken auf neue Service.    

Zur Unterstützung bei der Aktualisierung empfiehlt die SQL-Datenbank eine entsprechende Service-Tier und Performance-Niveau (Tarif) für jede Datenbank. Der Dienst empfiehlt bewährte Tier zum Ausführen der vorhandenen Datenbank Arbeitslast durch Analysieren von Verlaufsdaten Verwendung für die Datenbank.

3. **Diesen Server aktualisieren** Blatt wählen Sie jeder Datenbank überprüfen und wählen Sie zum Aktualisieren auf Datenebene empfohlene Preisgestaltung aus Sie immer verfügbaren Tarifen suchen und auswählen, die Ihre Umgebung am besten.


     ![Datenbanken][2]


7. Nach dem Klicken auf die empfohlene Stufe wird mit **Ihrem Tarif wählen** angezeigt, in dem Sie eine Ebene auswählen und klicken **Wählen Sie** auf dieser Ebene ändern. Wählen Sie eine neue Ebene für jede Datenbank Web oder Business.

    Zu beachten ist, dass die SQL-Datenbank nicht in eine bestimmte Tier oder Leistung Ebene gesperrt ist als an der Datenbank ändern können Sie leicht zwischen verfügbaren Dienstebenen und Leistung ändern. Tatsächlich Basic, Standard und Premium SQL Datenbanken pro Stunde berechnet, und Ihnen jede Datenbank 4 Mal innerhalb von 24 Stunden nach oben oder unten zu skalieren.

    ![Empfehlung][6]


Nachdem alle Datenbanken auf dem Server kommen können Sie mit dem Upgrade beginnen

## <a name="confirm-the-upgrade"></a>Bestätigen Sie die Aktualisierung

3. Wenn alle Datenbanken auf dem Server Upgrade berechtigt sind müssen Sie **Typ der Servername** überprüfen möchten, führen Sie die Aktualisierung, und klicken Sie auf **OK**.

    ![Überprüfen der Aktualisierung][3]


4. Die Aktualisierung wird gestartet und zeigt die in Bearbeitung befindlichen Benachrichtigung. Der Upgrade-Vorgang initiiert. Je nach der spezifischen Datenbanken dauert aktualisieren V12 einige Zeit. Während dieser Zeit alle Datenbanken auf dem Server bleiben online, aber Server und Datenbank-Management-Aktionen eingeschränkt werden.

    ![das Upgrade wird durchgeführt][4]

    Zum Zeitpunkt der tatsächlichen Umstellung auf die neue Leistung möglich Ebene vorübergehend die Verbindung zu der Datenbank ablegen lang sehr klein (in der Regel gemessen in Sekunden). Wenn eine Anwendung vorübergehend Fehlerbehandlung (Wiederholungslogik) für Verbindung Beendigungen ist ausreichenden Schutz Verbindungsverlust am Ende der Aktualisierung.

5. Nach Abschluss des Upgrade-Vorgangs wird das **Neueste Update** Blade **aktiviert**angezeigt.

    ![V12 aktiviert][5]  

## <a name="move-your-databases-into-an-elastic-database-pool"></a>Verschieben Sie die Datenbanken in einem Datenbankpool elastische

In [Azure-Portal](https://portal.azure.com/) V12-Server suchen Sie, und klicken Sie auf **Ressourcenpool hinzufügen**.

– oder –

Meldung der **hier empfohlenen elastischen datenbankpools für diesen Server**, klicken Sie auf, um einfach einen Pool erstellen, der Datenbanken Ihres Servers optimiert. Weitere Informationen anzeigen Sie [und Hinweise für einen Datenbankpool elastische](sql-database-elastic-pool-guidance.md)

![Pool zu einem Server hinzufügen][7]

Führen Sie die Schritte im Artikel [Erstellen einer elastischen Datenbankpool](sql-database-elastic-pool.md) zum Erstellen des Pools.

## <a name="monitor-databases-after-upgrading-to-sql-database-v12"></a>Überwachen Sie nach der Aktualisierung auf SQL Datenbank V12 Datenbanken

>[AZURE.IMPORTANT] Aktualisieren Sie auf die neueste Version von SQL Server Management Studio (SSMS) neue v12-Funktionen nutzen. [SQL Server Management Studio download] (https://msdn.microsoft.com/library/mt238290.aspx).

Nach der Aktualisierung Überwachen der Datenbank, um sicherzustellen, dass die gewünschte Leistung ausgeführt Applications und Belieben zu optimieren.

Neben der Überwachung einzelner Datenbanken können elastische datenbankpools [überwachen, verwalten und Größe einen elastischen Datenbankpool mit Azure-Portal](sql-database-elastic-pool-manage-portal.md) überwachen oder mit [PowerShell](sql-database-elastic-pool-manage-powershell.md).


**Ressourcendaten:** Basic, Standard und Premium-Datenbanken ist Ressourcendaten über die [sys.dm_ Db_ Resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) DMV in der Datenbank. Diese DMV bietet nahezu Echtzeit Verbrauch Ressourceninformationen 15 zweite Granularität für die letzte Stunde des Vorgangs. DTU Prozentsatz Verbrauch für ein Intervall wird als Höchstprozentsatz Verbrauch von CPU und EA Protokoll Dimensionen berechnet. Dies ist eine Abfrage zum Berechnen des durchschnittlichen DTU Prozentsatz Verbrauchs in der letzten Stunde:

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

Überwachung Zusatzinformationen:

- [Leitfaden zur Azure SQL-Datenbank-Leistung für einzelne Datenbanken](http://msdn.microsoft.com/library/azure/dn369873.aspx).
- [Preis- und Hinweise für einen Datenbankpool elastische](sql-database-elastic-pool-guidance.md).
- [Überwachung über dynamische Verwaltungsansichten Azure SQL-Datenbank](sql-database-monitoring-with-dmvs.md)




**Warnung:** Soll "Alarme" in Azure-Portal benachrichtigen DTU Verbrauch für eine aktualisierte Datenbank bestimmte hohe nähert. Datenbank-Alerts können im Azure-Portal für verschiedene Leistungsmetriken wie DTU, CPU, e/a- und Protokolldateien werden. Die Datenbank durchsuchen Sie, und wählen Sie **Warnregeln** **Einstellungen** -Blades.

Beispielsweise können Sie Einrichten einer e-Mail-Warnung "DTU Prozentsatz" übersteigt die durchschnittliche DTU Prozentsatz 75 % in den letzten 5 Minuten. Finden Sie weitere Informationen zum Konfigurieren von Benachrichtigungen [Benachrichtigungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) erhalten.





## <a name="next-steps"></a>Nächste Schritte

- [Suchen Recommendations pool und einen Pool erstellen](sql-database-elastic-pool-create-portal.md).
- [Die Dienstebene Tier und Leistung Ihrer Datenbank ändern](sql-database-scale-up.md).



## <a name="related-links"></a>Verwandte Links

- [Neuigkeiten in SQL Datenbank V12](sql-database-v12-whats-new.md)
- [Planen Sie und bereiten Sie der SQL-Datenbank V12 aktualisieren vor](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png
