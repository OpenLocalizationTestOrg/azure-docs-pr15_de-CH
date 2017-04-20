<properties
    pageTitle="Lernprogramm für SQL: Erstellen einer SQL-Datenbank | Microsoft Azure"
    description="Informationen Sie zum Einrichten einer logischen SQL-Datenbankserver, Server Firewallregel SQL-Datenbank und Beispieldaten. Außerdem Informationen Sie zum Verbinden mit Clienttools, Konfigurieren von Benutzern und eine Firewallregel Datenbank einrichten."
    keywords="Lernprogramm für SQL, erstellen Sie eine SQL­Datenbank"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/07/2016"
    ms.author="carlrab"/>


# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>Lernprogramm für SQL: mithilfe eine SQL-Datenbank in Minuten Azure-Portal

> [AZURE.SELECTOR]
- [Azure-portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

In diesem Lernprogramm erfahren Sie, wie das Azure-Portal zu verwenden:

- Erstellen einer Azure SQL-Datenbank mit Beispieldaten.
- Erstellen Sie eine Firewall auf Serverebene Regel für eine einzelne IP-Adresse oder einen Bereich von IP-Adressen.

Sie können diese Tasks mithilfe von [C#](sql-database-get-started-csharp.md) oder [PowerShell](sql-database-get-started-powershell.md)ausführen.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>Erstellen Sie Ihre erste Azure SQL-Datenbank 

1. Wenn Sie zurzeit nicht verbunden sind, verbinden Sie mit [Azure-Portal](http://portal.azure.com).
2. Klicken Sie auf **neu**, und suchen Sie **SQL-Datenbank**auf **Daten + Speicher**.

    ![Neue Sql-Datenbank 1](./media/sql-database-get-started/sql-database-new-database-1.png)

3. Klicken Sie auf **SQL-Datenbank** -Blade SQL Datenbank öffnen. Der Inhalt auf diese hängt von der Anzahl von Abonnements und die vorhandenen Objekte (z. B. vorhandene Server).

    ![Neue Sql-Datenbank 2](./media/sql-database-get-started/sql-database-new-database-2.png)

4. Geben Sie einen Namen für die erste Datenbank - wie "Meine Datenbank" im Feld **Datenbankname** . Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen angegeben haben.

    ![Neue Sql-Datenbank 3](./media/sql-database-get-started/sql-database-new-database-3.png)

5. Wenn Sie mehrere Abonnements verfügen, wählen Sie ein Abonnement.
6. **Ressourcengruppe**klicken Sie auf **neu erstellen** , und geben Sie einen Namen für die erste Ressourcengruppe - wie "Meine Ressourcen-Gruppe". Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen angegeben haben.

    ![Neue Sql-Datenbank 4](./media/sql-database-get-started/sql-database-new-database-4.png)

7. Klicken Sie unter **Quelle auswählen**auf **Beispiel** , und klicken Sie unter **Stichprobe** **AdventureWorksLT [V12]**.

    ![Neue Sql-Datenbank 5](./media/sql-database-get-started/sql-database-new-database-5.png)

8. **Server**klicken Sie auf **Configure Settings erforderlich**.

    ![Neue Sql-Datenbank 6](./media/sql-database-get-started/sql-database-new-database-6.png)

9. Klicken Sie auf das Server-Blade **erstellen einen neuen Server**. Eine SQL Azure-Datenbank wird in ein Serverobjekt erstellt einen neuen Server oder einem vorhandenen Server.

    ![Neue Sql-Datenbank 7](./media/sql-database-get-started/sql-database-new-database-7.png)

10. Überprüfen Sie das **neue Server** -Blade die Informationen haben, die Sie für den neuen Server bereitstellen müssen.

    ![Neue Sql-Datenbank 8](./media/sql-database-get-started/sql-database-new-database-8.png)

11. Geben Sie im Feld **Servername** einen Namen für den ersten Server - wie "my-neuen-Server-Objekt". Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen angegeben haben.

    ![Neue Sql-Datenbank 9](./media/sql-database-get-started/sql-database-new-database-9.png)
 
12. Geben Sie unter **Server administratoranmeldung**einen Benutzernamen für das Administratorkonto für diesen Server - wie "Mein Admin-Konto". Dieser Benutzername wird der principal-Benutzername genannt. Ein grünes Häkchen zeigt an, dass Sie einen gültigen Namen angegeben haben.

    ![Neue Sql-Datenbank 10](./media/sql-database-get-started/sql-database-new-database-10.png)

13. Unter **Kennwort** und **Kennwort bestätigen**, geben Sie ein Kennwort für den Server principal Anmeldekonto - wie "p@ssw0rd1". Ein grünes Häkchen zeigt an, dass Sie ein gültiges Kennwort angegeben haben.

    ![Neue Sql-Datenbank 11](./media/sql-database-get-started/sql-database-new-database-11.png)
 
14. Wählen Sie unter **Speicherort**einen Rechenzentrums zu Ihrem Standort - wie "Australien Ost".

    ![Neue Sql-Datenbank 12](./media/sql-database-get-started/sql-database-new-database-12.png)

15. Unter ** Erstellen V12 Server (neueste), beachten Sie, dass nur eine aktuelle Version der SQL Azure-Server erstellen.

    ![Neue Sql-Datenbank 13](./media/sql-database-get-started/sql-database-new-database-13.png)

16. Beachten Sie, dass das Kontrollkästchen **Zulassen Azure Services Zugriff auf Server** standardmäßig aktiviert ist und hier nicht geändert werden. Dies ist eine erweiterte Option. Sie können diese Einstellung in der Server-Firewall für das Serverobjekt ändern, obwohl für die meisten Szenarios dies nicht erforderlich ist.

    ![Neue Sql-Datenbank 14](./media/sql-database-get-started/sql-database-new-database-14.png)

17. Überprüfen Sie auf dem neuen Serverblade Ihre Auswahl und klicken Sie dann auf diese neuen Server für die neue Datenbank auswählen **Wählen** .

    ![Neue Sql-Datenbank 15](./media/sql-database-get-started/sql-database-new-database-15.png)

18. Blade SQL Datenbank unter **Preisstufe**klicken Sie **S2 Standard** und dann auf **grundlegende** günstigsten Tarif für die erste Datenbank wählen. Den Tarif können Sie später jederzeit ändern.

    ![Neue Sql-Datenbank 16](./media/sql-database-get-started/sql-database-new-database-16.png)

19. Überprüfen Sie auf der SQL-Datenbank Ihre Auswahl und klicken Sie auf **Erstellen** Ihrer ersten Server und Datenbank erstellen. Die von Ihnen angegebenen Werte werden überprüft und Bereitstellung beginnt.

    ![Neue Sql-Datenbank 17](./media/sql-database-get-started/sql-database-new-database-17.png)

20. Klicken Sie auf Portal Symbolleiste auf **Benachrichtigung** Elemente zum Überprüfen des Status der Bereitstellung.

    ![Neue Sql-Datenbank 18](./media/sql-database-get-started/sql-database-new-database-18.png)

>[AZURE.IMPORTANT]Bereitstellung abgeschlossen ist, werden die neuen SQL Azure-Server und Datenbank in Azure erstellt. Sie dürfen eine Verbindung zu Ihrem neuen Server oder eine Datenbank SQL Server-Tools verwenden, bis Sie eine Firewallregel Server um Verbindungen von Azure SQL-Datenbank-Firewall erstellen.

[AZURE.INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Nächste Schritte
Sie haben dieses Lernprogramm SQL abgeschlossen und eine Datenbank mit Beispieldaten erstellt, können Sie mithilfe Ihrer bevorzugten Tools durchsuchen.

- Wenn Sie mit dem Transact-SQL und SQL Server Management Studio (SSMS) vertraut sind, [eine SQL-Datenbank mit SSMS Abfrage](sql-database-connect-query-ssms.md)zu verbinden.

- Wenn Excel kennen, erfahren, wie Sie [mit einer SQL Azure mit Excel](sql-database-connect-excel.md).

- Wenn Sie codieren möchten, wählen Sie die Programmiersprache Bibliotheken [für SQL-Datenbank und SQL Server-Verbindung](sql-database-libraries.md).

- Zu Ihrem lokalen SQL Server-Datenbanken in Azure, finden Sie weitere [Migrieren einer SQL-Datenbank](sql-database-cloud-migrate.md) .

- Einige Daten in eine neue Tabelle in einer CSV-Datei laden, mit BCP-Befehlszeilenprogramm, finden Sie unter [Laden von Daten in SQL-Datenbank aus einer CSV-Datei mit BCP](sql-database-load-from-csv-with-bcp.md).

- Azure SQL Database Security herumprobieren, finden Sie unter [Erste Schritte mit Sicherheit](sql-database-get-started-security.md)


## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Was ist SQL-Datenbank?](sql-database-technical-overview.md)
