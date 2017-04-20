<properties
    pageTitle="Kopieren eine SQL Azure-Datenbank mithilfe des Azure-Portals | Microsoft Azure"
    description="Erstellen Sie eine Kopie einer SQL Azure-Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>



# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals

> [AZURE.SELECTOR]
- [Übersicht](sql-database-copy.md)
- [Azure-portal](sql-database-copy-portal.md)
- [PowerShell](sql-database-copy-powershell.md)
- [T-SQL](sql-database-copy-transact-sql.md)

Die folgenden Schritte zeigen, wie einer SQL [Azure-Portal](https://portal.azure.com) auf dem gleichen Server oder einen anderen Server kopieren.

Zum Kopieren einer SQL-Datenbank benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie Azure-Abonnement lediglich **Testversion** am oberen Rand dieser Seite klicken Sie und dann wieder zu diesem Artikel.
- Eine SQL-Datenbank kopieren. Wenn Sie keinen SQL-Datenbank erstellen einen der Schritte in diesem Artikel: [Erstellen Sie Ihre erste Azure SQL-Datenbank](sql-database-get-started.md).


## <a name="copy-your-sql-database"></a>Kopieren der SQL-Datenbank

Öffnen der Seite SQL-Datenbank für die Datenbank, die Sie kopieren möchten:

1.  Zum [Azure-Portal](https://portal.azure.com).
2.  Klicken Sie auf **Weitere Dienste** > **SQL-Datenbanken**, und klicken Sie dann auf die gewünschte Datenbank.
3.  Klicken Sie auf der Seite SQL-Datenbank **Kopieren**:

    ![SQL-Datenbank](./media/sql-database-copy-portal/sql-database-copy.png)

1.  Auf der Seite **Kopieren** wird ein Standardname für die Datenbank bereitgestellt. Geben Sie einen anderen Namen, wenn Sie möchten (alle Datenbanken auf einem Server müssen eindeutige Namen haben).
2.  Wählen Sie einen **Zielserver**. Der Zielserver ist, wo die Datenbankkopie erstellt wird. Sie können die Datenbank auf demselben Server oder einen anderen Server kopieren. Sie können einen Server erstellen oder einen vorhandenen Server auswählen. 
3.  Nach Anwahl **Zielserver** **elastische Datenbankpool**und der **Preisstufe** aktiviert wird. Wenn der Server einen Pool, können Sie die Datenbank hinein kopieren.
3.  Klicken Sie auf **OK** , um den Kopiervorgang zu starten.

    ![SQL-Datenbank](./media/sql-database-copy-portal/copy-page.png)


## <a name="monitor-the-progress-of-the-copy-operation"></a>Überwachen Sie den Fortschritt des Kopiervorgangs

- Nach dem Starten der Kopie, klicken Sie auf Portal Benachrichtigung für Details.

    ![Benachrichtigung][3]
 
    ![Monitor][4]


## <a name="verify-the-database-is-live-on-the-server"></a>Überprüfen Sie die Datenbank auf dem server

- Klicken Sie auf **Weitere Dienste** > **SQL-Datenbanken** und überprüfen Sie, ob die Datenbank **Online**ist.


## <a name="resolve-logins"></a>Benutzernamen auflösen

Um Benutzernamen zu beheben, nachdem der Kopiervorgang abgeschlossen ist, finden Sie unter [Benutzernamen auflösen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="next-steps"></a>Nächste Schritte

- Finden Sie einen Überblick über das Kopieren einer Azure SQL-Datenbank [eine SQL Azure-Datenbank kopieren](sql-database-copy.md) .
- Anzeigen Sie kopieren eine Datenbank mit PowerShell [Kopie einer Azure SQL-Datenbank mithilfe von PowerShell](sql-database-copy-powershell.md)
- Anzeigen Sie kopieren eine Datenbank mithilfe von Transact-SQL- [Kopie einer Azure SQL-Datenbank mithilfe von T-SQL](sql-database-copy-transact-sql.md)
- Finden Sie unter [Verwalten von Azure SQL-datenbanksicherheit nach Wiederherstellung](sql-database-geo-replication-security-config.md) Informationen zum Verwalten von Benutzern und Benutzernamen beim Kopieren einer Datenbank auf einem anderen logischen Server.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Verwalten von Benutzernamen](sql-database-manage-logins.md)
- [Verbinden mit SQL-Datenbank mit SQL Server Management Studio, und führen Sie eine T-SQL-Abfrage](sql-database-connect-query-ssms.md)
- [Die Datenbank in ein BACPAC exportieren](sql-database-export.md)
- [Business Continuity – Überblick](sql-database-business-continuity.md)
- [Dokumentation zur SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)




<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png

