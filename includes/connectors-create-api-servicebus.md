### <a name="prerequisites"></a>Erforderliche Komponenten

Sie müssen ein Konto [Service Bus](https://azure.microsoft.com/services/service-bus/) .  

Vor der Verwendung von Azure Service Bus-Konto in eine Logik-app müssen Sie Logik app Verbindung zu Ihrem Dienstkonto Bus autorisieren. Glücklicherweise können Sie problemlos Ihre App Logik der Azure-Portal dazu.  

Hier sind die Schritte Logik-app für die Verbindung zu Ihrem Konto Service Bus autorisieren:  

1. Zum Erstellen einer Verbindung mit Service Bus im Designer app Logik wählen Sie **Anzeigen von Microsoft verwalteten APIs** in der Dropdown-Liste aus Geben Sie im Suchfeld **Servicebus** . Wählen Sie den oder die Aktion, die Sie verwenden möchten.  
    ![Service Bus Verbindung Bild 1](./media/connectors-create-api-servicebus/servicebus-1.png)  

2. Wenn Sie Verbindungskabel Service Bus vor erstellt haben, werden Sie aufgefordert, Ihre Anmeldeinformationen Service Bus. Diese Anmeldeinformationen zum Autorisieren Ihrer Anwendung Logik und Zugriff auf Ihre Kontodaten des Service Bus. Der Connector Service Bus benötigt die Verbindungszeichenfolge für den Service Bus-Namespace. Außerdem erfordert Berechtigungen **Verwalten** . Gut zu wissen, ob die Verbindungszeichenfolge für den Namespace oder eine bestimmte Entität enthält die `EntityPath` Parameter. Andernfalls ist es nicht die richtige Verbindungszeichenfolge für Logik app.  
    ![Service Bus-Verbindungszeichenfolge](./media/connectors-create-api-servicebus/connectionstring.png)

1. Nach die Verbindungszeichenfolge für den Namespace Erhalt können Sie sie für die Verbindung API-Logik-Apps.  
    ![Service Bus Verbindung Bild 2](./media/connectors-create-api-servicebus/servicebus-2.png)  

3. Beachten Sie die Verbindung eingerichtet wurde und Sie können die Schritte in Ihrer Anwendung Logik fortsetzen.  
    ![Service Bus Verbindung Bild 3](./media/connectors-create-api-servicebus/servicebus-3.png)   
