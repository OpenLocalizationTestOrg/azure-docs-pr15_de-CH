### <a name="prerequisites"></a>Erforderliche Komponenten
- Ein Azure-Konto; Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
- Ein [Azure SQL-Datenbank](../articles/sql-database/sql-database-get-started.md) mit der Verbindungsinformationen, einschließlich Servernamen, Datenbanknamen und Benutzername/Kennwort. Diese Informationen sind in der SQL-Datenbank-Verbindungszeichenfolge enthalten:
  
    Server = Tcp:*Yoursqlservername*. database.windows.net,1433;Initial Catalog =*Yourqldbname*; Persist Security Info = False; Benutzer-ID = {Ihr_Benutzername}; Kennwort = {Your_password}; MultipleActiveResultSets = False; Verschlüsseln = True; TrustServerCertificate = False; Connection Timeout = 30;

    Weitere Informationen zur [Azure SQL-Datenbanken](https://azure.microsoft.com/services/sql-database).

> [AZURE.NOTE] Beim Erstellen einer Azure SQL-Datenbank können Sie auch die Beispieldatenbanken enthaltene SQL erstellen. 



Verbinden Sie bevor Ihre Azure SQL-Datenbank in eine Anwendung Logik verwenden die SQL-Datenbank. Diese möglich problemlos in Ihre Anwendung Logik der Azure-Portal.  

Verbinden Sie mit der Azure SQL-Datenbank mit den folgenden Schritten:  

1. Erstellen Sie eine Logik-app. Fügen Sie im Designer Logik Apps Trigger hinzu, und fügen Sie eine Aktion. Wählen Sie in der Dropdown-Liste **Anzeigen von Microsoft verwalteten APIs** , und geben Sie "Sql" im Suchfeld. Wählen Sie eine der Aktionen aus:  

    ![SQL Azure Verbindungsvorgangs erstellen](./media/connectors-create-api-sqlazure/sql-actions.png)

2. Wenn zuvor Verbindung zu SQL-Datenbank erstellt haben, werden Sie aufgefordert, die Details der Verbindung:  

    ![SQL Azure Verbindungsvorgangs erstellen](./media/connectors-create-api-sqlazure/connection-details.png) 

3. Geben Sie die SQL-Datenbank. Eigenschaften mit einem Sternchen sind erforderlich.

    | Eigenschaft | Details |
|---|---|
| Verbindung über Gateway | Lassen Sie dieses deaktiviert. Hiermit wird eine lokale SQL Server-Verbindung. |
| Verbindungsname * | Geben Sie einen beliebigen Namen für die Verbindung. | 
| SQL Server Name * | Geben Sie den Servernamen ein. Das ist *servername.database.windows.net*. Der Servername ist im SQL-Eigenschaften im Azure-Portal angezeigt und auch in der Verbindungszeichenfolge angezeigt. | 
| Name der SQL-Datenbank * | Geben Sie den Namen der SQL-Datenbank hat. Dies ist im SQL-Eigenschaften in der Verbindungszeichenfolge aufgeführt: Initial Catalog =*Yoursqldbname*. | 
| Benutzername * | Geben Sie beim Erstellen der SQL-Datenbank erstellten Benutzernamen. Dies ist im SQL-Eigenschaften im Azure-Portal aufgeführt. | 
| Kennwort * | Geben Sie das Kennwort, das Sie beim Erstellen der SQL-Datenbank erstellt. | 

    Diese Anmeldeinformationen zum Autorisieren der Logik app herstellen und Ihre SQL-Daten. Danach suchen die Verbindungsdetails ähnlich der folgenden:  

    ![SQL Azure Verbindungsvorgangs erstellen](./media/connectors-create-api-sqlazure/sample-connection.png) 

4. Wählen Sie **Erstellen**. 

5. Beachten Sie, dass die Verbindung hergestellt wurde. Jetzt gehen Sie die Schritte in Ihrer Anwendung Logik: 

    ![SQL Azure Verbindungsvorgangs erstellen](./media/connectors-create-api-sqlazure/table.png)