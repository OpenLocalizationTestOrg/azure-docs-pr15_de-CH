#### <a name="prerequisites"></a>Erforderliche Komponenten
- Ein Azure-Konto; Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
- [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) -Konto 

Vor der Verwendung von OneDrive-Konto in eine Logik Anwendung autorisieren Sie app Logik für die Verbindung zu Ihrem Konto OneDrive.  Diese möglich problemlos in Ihre Anwendung Logik der Azure-Portal. 

Autorisieren Sie Ihre app Logik für die Verbindung zu Ihrem OneDrive-Konto mit den folgenden Schritten:

1. Erstellen Sie eine Logik-app. Im Designer Logik Apps **zeigen Microsoft verwalteten APIs** in der Dropdown-Liste auswählen und dann "Onedrive" in das Suchfeld eingeben. Wählen Sie den Trigger oder Aktionen:  
  ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Wenn Sie zuvor alle Verbindungsversuche mit OneDrive erstellt haben, werden Sie aufgefordert, Ihre OneDrive-Anmeldeinformationen anmelden:  
  ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Wählen Sie **Anmelden**, und geben Sie Ihren Benutzernamen und Ihr Kennwort. Wählen Sie **Anmelden**:  
  ![](./media/connectors-create-api-onedrive/onedrive-3.png)   

    Diese Anmeldeinformationen zum Autorisieren der Logik app herstellen und die Daten in Ihrem OneDrive-Konto. 
4. Wählen Sie **Ja** ermächtigen Logik app Ihr OneDrive Konto verwenden:  
  ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Beachten Sie, dass die Verbindung hergestellt wurde. Jetzt gehen Sie die Schritte in Ihrer Anwendung Logik:  
  ![](./media/connectors-create-api-onedrive/onedrive-5.png)
