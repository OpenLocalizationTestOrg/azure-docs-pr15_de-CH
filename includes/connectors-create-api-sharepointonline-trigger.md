In diesem Beispiel werde ich zeigen, wie Sie **SharePoint Online - beim Erstellen ein neues Elements** Trigger einen Logik app Workflow initiieren, wenn in SharePoint Online Liste ein neues Element erstellt wird.

>[AZURE.NOTE]Sie werden aufgefordert erhalten Ihr SharePoint-Konto anmelden, wenn Sie nicht bereits eine *Verbindung* mit SharePoint Online erstellt haben.  

1. Geben Sie *Sharepoint* Designer apps Logik in das Suchfeld ein und wählen Sie die **SharePoint Online - Wenn ein neues Element erstellt wird** .  
![SharePoint online auslösenden Bild](./media/connectors-create-api-sharepointonline/trigger-1.png)  
- Das Steuerelement **beim Erstellen ein neues Elements** wird angezeigt.  
![SharePoint online auslösenden Bild 2](./media/connectors-create-api-sharepointonline/trigger-2.png)   
- Wählen Sie eine **Website-URL**. Dies ist der SharePoint online-Website für neue Elemente den Workflow auslösen überwachen möchten.  
![SharePoint online auslösenden Bild 3](./media/connectors-create-api-sharepointonline/trigger-3.png)   
- Wählen Sie einen **Namen**. Dies ist die Liste auf der SharePoint Online-Website, die Sie für neue Elemente überwachen möchten, die den Workflow auslösen.  
![SharePoint online auslösenden Bild 4](./media/connectors-create-api-sharepointonline/trigger-4.png)   

An diesem Punkt wurde Ihre Anwendung Logik Trigger konfiguriert, die Ausführung der anderen Auslöser und Aktionen im Workflow startet. Dies erfolgt jedes Mal entsteht ein neues Element in SharePoint Online Liste gewählte.  