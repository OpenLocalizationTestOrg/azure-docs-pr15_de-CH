Hier ist **Service Bus - Wenn eine in einer Warteschlange Nachricht** Trigger verwenden, um eine Logik app Workflow initiiert, wenn ein neues Element an eine Service Bus-Warteschlange gesendet wird.  

>[AZURE.NOTE]Sie werden aufgefordert die Verbindungszeichenfolge Service Bus anmelden, wenn Sie eine Verbindung mit Service Bus nicht bereits erstellt haben.  

1. Geben Sie im Suchfeld die Logik apps Designer **Servicebus**. Wählen Sie den Trigger **Service Bus - Wenn eine in einer Warteschlange Nachricht** .  
![Service Bus auslösenden Bild 1](./media/connectors-create-api-servicebus/trigger-1.png)   
- **Wenn eine in einer Warteschlange Nachricht** -Dialogfeld wird angezeigt.  
![Service Bus auslösenden Bild 2](./media/connectors-create-api-servicebus/trigger-2.png)   
- Geben Sie den Namen der Warteschlange Service Bus Auslöser überwachen möchten.   
![Service Bus auslösenden Bild 3](./media/connectors-create-api-servicebus/trigger-3.png)   

An diesem Punkt wurde Ihre Anwendung Logik Trigger konfiguriert. Wenn ein neues Element in der Warteschlange gewählte eingeht, beginnt der Trigger Ausführen anderer Trigger und Aktionen im Workflow.    
