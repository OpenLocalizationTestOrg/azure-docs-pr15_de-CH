
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## <a name="create-a-new-azure-sql-database"></a>Erstellen einer neuen SQL Azure-Datenbank

Gehen Sie im Azure-Portal eine neue Azure SQL-Datenbank auf einem neuen oder vorhandenen Azure SQL-Datenbank logischen erstellen.

1. Wenn Sie zurzeit nicht verbunden sind, verbinden Sie mit [Azure-Portal](http://portal.azure.com).
2. Klicken Sie auf **neu**und klicken Sie dann auf **SQL Datenbank neue**Geben Sie **SQL-Datenbank**.

     ![Neue Datenbank](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)

3. Klicken Sie auf **SQL-Datenbank (neue Datenbank)**.

     ![Neue Datenbank](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)

4. Klicken Sie auf **Erstellen** , um eine neue Datenbank in der SQL-Datenbankdienst erstellen.

     ![Neue Datenbank](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)

5. Geben Sie die Werte für die folgenden Server:

 - Name der Datenbank
 - Abonnement: Dies gilt nur, wenn mehrere Abonnements.
 - Ressourcengruppe: Wenn Sie gerade Ihre ersten Schritte sind, verwenden Sie die Ressourcengruppe des logischen Servers.
 - Quelle auswählen: Sie können eine leere Datenbank, Daten oder eine Azure-Datenbank-Sicherung. Eine lokalen SQL Server-Datenbank migrieren oder Laden von Daten mit BCP-Befehlszeilentool finden Sie unter den Links am Ende dieses Artikels.
 - Server: Einen neuen oder vorhandenen logischen Server.
 - Server administratoranmeldung
 - Kennwort
 - Tarif: Wenn Sie gerade Ihre ersten Schritte sind, verwenden Sie den Standardwert S0.
 - Sortierung: Dies gilt nur, wenn eine leere Datenbank ausgewählt wurde.

        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)

6.  Klicken Sie auf **Erstellen**. Im Infobereich angezeigt, Bereitstellung gestartet wurde.

     ![Neue Datenbank](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)

7. Warten Sie für die Bereitstellung abgeschlossen ist, bevor Sie mit dem nächsten Schritt fortfahren.

     ![Neue Datenbank](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)
