<properties
    pageTitle="Kopieren eine SQL Azure-Datenbank | Microsoft Azure"
    description="Erstellen Sie eine Kopie einer SQL Azure-Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="anosov1960"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/24/2016"
    ms.author="sstein; sashan"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database"></a>Kopieren einer SQL Azure-Datenbank

> [AZURE.SELECTOR]
- [Übersicht](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Die Azure [SQL-Datenbank automatisierte Backups](sql-database-automated-backups.md) können Sie um eine Kopie Ihrer SQL-Datenbank zu erstellen. Die Datenbankkopie verwendet die gleiche Technologie wie die geometrische Replikationsfunktion. Aber im Gegensatz zu Geo-Replikation beendet wird die Replikation Verknüpfung als Abschluss seeding Phase. Daher ist die Datenbank einen Snapshot der Quelldatenbank zum Zeitpunkt der kopieranforderung.  
Sie können die Datenbankkopie auf dem gleichen Server oder einem anderen Server erstellen. Tier und Leistung Dienstebene (Tarif) der Datenbankkopie entsprechen standardmäßig der Quelldatenbank. Bei der Verwendung der API können Sie unterschiedliche Leistungsniveau innerhalb derselben Dienstebene (Ausgabe) auswählen. Nachdem der Kopiervorgang abgeschlossen ist, wird die Kopie eine voll funktionsfähige unabhängige Datenbank. Sie können jetzt aktualisieren oder sollten sie eine Edition. Benutzernamen, Benutzer und Berechtigungen können unabhängig voneinander verwaltet werden.  

Beim Kopieren einer Datenbank auf dem gleichen logischen Server können denselben Benutzernamen auf beide Datenbanken verwendet werden. Das Sicherheitsprinzipal können Sie die Datenbank kopieren wird der Datenbankbesitzer (DBO) für die neue Datenbank. Alle Benutzer, deren Berechtigungen und ihre Sicherheits-IDs (SIDs) werden in die Datenbank kopieren kopiert.  

Beim Kopieren einer Datenbank auf einem anderen logischen Server wird das Sicherheitsprinzipal auf dem neuen Server Eigentümer der Datenbank der neuen Datenbank. Verwenden Sie für [Benutzer enthalten](sql-database-manage-logins.md) sichert Daten primäre und sekundäre Datenbanken immer die gleichen Anmeldeinformationen, nachdem der Kopiervorgang abgeschlossen ist Sie sofort mit den gleichen Anmeldeinformationen zugreifen können. Bei Verwendung von [Azure Active Directory](../active-directory/active-directory-whatis.md)können Sie vollständig für die Verwaltung von Anmeldeinformationen in der Kopie erforderlich. Beim Kopieren der Datenbank auf einen neuen Server funktioniert basierten Anmeldezugriff im Allgemeinen nicht jedoch da Benutzernamen auf dem neuen Server nicht vorhanden ist. Finden Sie unter [Verwalten von Azure SQL-datenbanksicherheit nach Wiederherstellung](sql-database-geo-replication-security-config.md) Informationen zum Verwalten von Benutzernamen beim Kopieren einer Datenbank auf einem anderen logischen Server. 

Zum Kopieren einer SQL-Datenbank benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie Azure-Abonnement lediglich **Testversion** am oberen Rand dieser Seite klicken Sie und dann wieder zu diesem Artikel.
- Eine SQL-Datenbank kopieren. Wenn Sie keinen SQL-Datenbank erstellen einen der Schritte in diesem Artikel: [Erstellen Sie Ihre erste Azure SQL-Datenbank](sql-database-get-started.md).

## <a name="next-steps"></a>Nächste Schritte

- Anzeigen Sie kopieren eine Datenbank mithilfe von Azure Portal [kopieren eine SQL Azure-Datenbank mithilfe des Azure-Portals](sql-database-copy-portal.md)
- Anzeigen Sie kopieren eine Datenbank mit PowerShell [Kopie einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md)
- Anzeigen Sie kopieren eine Datenbank mithilfe von Transact-SQL- [Kopie einer Azure SQL-Datenbank mithilfe von T-SQL](sql-database-copy-transact-sql.md)
- Finden Sie unter [Verwalten von Azure SQL-datenbanksicherheit nach Wiederherstellung](sql-database-geo-replication-security-config.md) Informationen zum Verwalten von Benutzern und Benutzernamen beim Kopieren einer Datenbank auf einem anderen logischen Server.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Verwalten von Benutzernamen](sql-database-manage-logins.md)
- [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine T-SQL-Abfrage](sql-database-connect-query-ssms.md)
- [Die Datenbank in ein BACPAC exportieren](sql-database-export.md)
- [Business Continuity – Überblick](sql-database-business-continuity.md)
- [Dokumentation zur SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)
