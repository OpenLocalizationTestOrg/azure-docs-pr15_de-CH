#### <a name="prerequisites"></a>Erforderliche Komponenten
- Ein Azure-Konto; Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
- [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) -Konto 

Autorisieren Sie bevor Ihr Konto Dynamics in eine Logik-app verwenden Logik app Ihr CRM Online-Konto herstellen. Diese möglich problemlos in Ihre Anwendung Logik der Azure-Portal. 

Autorisieren Sie Ihre Logik app Verbindung zu Ihr CRM Online-Konto mit den folgenden Schritten:

1. Erstellen Sie eine Logik-app. Im Designer Logik Apps **zeigen Microsoft verwalteten APIs** in der Dropdown-Liste auswählen und dann "Dynamik" in das Suchfeld eingeben. Wählen Sie den Trigger oder Aktionen:  
  ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Wenn zuvor Verbindung zu Dynamics erstellt haben, werden Sie aufgefordert, die Dynamics-Anmeldeinformationen anmelden:  
  ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Wählen Sie **Anmelden**, und geben Sie Ihren Benutzernamen und Ihr Kennwort. Wählen Sie **Anmelden**. 

    Diese Anmeldeinformationen werden zum Autorisieren Logik-app herstellen und die Daten in Ihrem Konto Dynamics. 
4. Beachten Sie, dass die Verbindung hergestellt wurde. Jetzt gehen Sie die Schritte in Ihrer Anwendung Logik:  
  ![](./media/connectors-create-api-crmonline/dynamics-properties.png)
