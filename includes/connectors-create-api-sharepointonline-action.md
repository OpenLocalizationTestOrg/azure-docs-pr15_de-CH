Einen Trigger hinzugefügt haben, interessant die dazu mit den Daten, die vom Trigger generiert wird. Gehen Sie die **SharePoint Online - Datei erstellen** -Aktion hinzuzufügen. Diese Aktion erstellt eine Datei in SharePoint Online jedem neuen Artikel Trigger auslöst. 

Konfigurieren der Aktion müssen die folgenden Informationen. Diese Informationen werden Sie feststellen, dass benutzerfreundliche Daten vom Trigger als Eingabe für einige der Eigenschaften für die neue Datei wird:

|Datei erstellen|Beschreibung|
|---|---|
|Website-URL|Dies ist die URL des SharePoint Online-Website die neue Datei erstellt werden soll. Wählen Sie die Site aus der Liste.|
|Pfad|Dies ist der Ordner (unter der Website-URL im vorherigen Schritt ausgewählt), wird die neue Datei gespeichert. Suchen Sie und wählen Sie den Ordner.|
|Dateiname|Dies ist der Name der Datei wird erstellt.|
|Inhalt der Datei|Der Inhalt in die Datei geschrieben werden.|

1. Wählen Sie **+ Neuer Schritt** die Aktion hinzuzufügen.  
![SharePoint online Aktion Bild 1](./media/connectors-create-api-sharepointonline/action-1.png)  
- Klicken Sie auf **eine Aktion hinzufügen** . Dieses Öffnet das Suchfeld, wobei Sie für jede Aktion suchen können, möchten. Beispielsweise sind SharePoint-Aktionen an.    
![SharePoint online Aktion Bild 2](./media/connectors-create-api-sharepointonline/action-2.png)    
- *Sharepoint* SharePoint Maßnahmen suchen eingeben.
- Wählen Sie die durchzuführende **SharePoint Online - Datei erstellen** .   **Hinweis**: Sie werden aufgefordert, autorisieren Ihrer Anwendung Logik auf der SharePoint-Konto zugreifen, wenn keine Verbindung mit SharePoint Online erstellt haben.    
![SharePoint online Aktion Bild 3](./media/connectors-create-api-sharepointonline/action-3.png)    
- Das Steuerelement **Erstellen** wird geöffnet.   
![SharePoint online Aktion Bild 4](./media/connectors-create-api-sharepointonline/action-4.png)     
- Wählen Sie **Website-URL** , und navigieren Sie zu der Website, in dem Sie die Datei erstellen möchten.     
![SharePoint online Aktion Bild 5](./media/connectors-create-api-sharepointonline/action-5.png)  
- Wählen Sie **Pfad** und suchen Sie nach dem Ordner die neue Datei gespeichert.  
![SharePoint online Aktion Bild 6](./media/connectors-create-api-sharepointonline/action-6.png)  
- Wählen Sie das Steuerelement **Namen** und geben Sie den Namen der Datei, die Sie erstellen möchten. Hier, Sie können den Namen entweder direkt eingeben oder können die Eigenschaften von zuvor erstellten Trigger. Hierzu wählen Sie Eigenschaften aus der Liste der **Ausgaben, wenn ein neues Element erstellt wird**. Diese Liste ist nur nach dem **Dateinamen** Steuerelement auswählen. In diesem Walkthough wird ID (die ID des neuen Listenelements) als den Namen der Datei von **SharePoint Online - Datei erstellen** -Aktion ausgewählt.    
![SharePoint online Aktion Bild 7](./media/connectors-create-api-sharepointonline/action-7.png)  
- Wählen Sie das Steuerelement **Inhalt** und geben Sie den Inhalt in die Datei geschrieben, die erstellt wird. Der Inhalt der Datei bemerken Sie, Eigenschaften vom Trigger verwenden, die Sie zuvor erstellt haben. Wählen Sie Eigenschaften aus der Liste aus. Alternativ können Sie **Inhalt** Text direkt in das Steuerelement eingeben. In diesem Beispiel kann ich einige Eigenschaften ausgewählt und Leerzeichen und ein Bindestrich zwischen jede Eigenschaft hinzugefügt.        
![SharePoint online Aktion Bild 8](./media/connectors-create-api-sharepointonline/action-8.png)  
- Speichern des Workflows  
- Herzlichen Glückwunsch, Sie haben eine voll funktionsfähige Logik-app, die ausgelöst wird, wenn eine SharePoint Online Liste ein neues Element hinzugefügt wird. Die Anwendung erstellt dann eine Datei mit einigen Eigenschaften von neuen Listenelements.  Jetzt testen sie durch Erstellen eines neuen Elements in der SharePoint-Liste. 
