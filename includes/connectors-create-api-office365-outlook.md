#### <a name="prerequisites"></a>Erforderliche Komponenten
- Ein Azure-Konto; Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
- Ein [Office 365](https://office365.com) -Konto  

Autorisieren Sie bevor Ihr Office 365-Konto in eine Logik-app verwenden Logik app Ihr Office 365-Konto herstellen. Diese möglich problemlos in Ihre Anwendung Logik der Azure-Portal.  

Autorisieren Sie Ihre app Logik für die Verbindung zu Ihrem Office 365-Konto mit den folgenden Schritten:

1. Erstellen Sie eine Logik-app. Im Designer Logik Apps **zeigen Microsoft verwalteten APIs** in der Dropdown-Liste Wählen Sie aus, und geben Sie "Office 365" in das Suchfeld. Wählen Sie den Trigger oder Aktionen:  
    ![Office 365 Verbindungsvorgangs erstellen](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  

2. Wenn zuvor Verbindung zu Office 365 erstellt haben, werden Sie aufgefordert, Ihre Office 365-Anmeldeinformationen anmelden:  
    ![Office 365 Verbindungsvorgangs erstellen](./media/connectors-create-api-office365-outlook/office365-signin.png)  

3. Wählen Sie **Anmelden**, und geben Sie Ihren Benutzernamen und Ihr Kennwort. Wählen Sie **Anmelden**:  
    ![Office 365 Verbindungsvorgangs erstellen](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)

    Diese Anmeldeinformationen werden zum Autorisieren Ihrer Anwendung Logik zum Verbinden mit und Ihr Office 365-Konto. 

4. Beachten Sie, dass die Verbindung hergestellt wurde. Jetzt gehen Sie die Schritte in Ihrer Anwendung Logik:   
    ![Office 365 Verbindungsvorgangs erstellen](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  
  