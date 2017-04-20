## <a name="connect-to-outlookcom"></a>Verbinden mit Outlook.com

### <a name="prerequisites"></a>Erforderliche Komponenten
- Ein Outlook.com-Konto

Vor der Verwendung von Outlook.com-Konto in eine Logik-app müssen Sie Logik app Verbindung zu Ihrem Outlook.com-Konto autorisieren. Glücklicherweise können Sie problemlos in der Logik app Azure-Portal dazu. 

Hier sind die Schritte ermächtigen Ihre app Logik für die Verbindung zu Ihrem Outlook.com-Konto:

1. Alle Logik apps müssen durch einen Auslöser gestartet werden, nachdem Sie Ihre Logik app Designer geöffnet und zeigt eine Liste der Trigger, die Verwendung Ihrer Anwendung Logik zu:

  ![](./media/connectors-create-api-outlook/office365-outlook-0.png)
2. Geben Sie im Suchfeld "Outlook". Beachten Sie, dass die Liste gefiltert ist, um alle Trigger mit "Outlook" im Namen aufzulisten:![](./media/connectors-create-api-outlook/office365-outlook-0-5.png)
3. Wählen Sie **Office 365 Outlook - auf neue e-Mail**.   
  Wenn Sie alle Verbindungsversuche mit Outlook bereits erstellt haben, erhalten Sie aufgefordert Ihre Outlook.com Anmeldeinformationen. Diese Anmeldeinformationen verwendet, um Ihre Logik app Verbindung zu autorisieren und Zugriff auf Ihr Konto Outlook.com Daten:![](./media/connectors-create-api-outlook/office365-outlook-1.png)
4. Ihre Anmeldeinformationen für Outlook, und melden Sie sich:![](./media/connectors-create-api-outlook/office365-outlook-2.png)  
  Das wars. Sie haben nun eine Verbindung mit Outlook erstellt. Diese Verbindung werden in andere Logik-app verwendet, die Sie erstellen.


