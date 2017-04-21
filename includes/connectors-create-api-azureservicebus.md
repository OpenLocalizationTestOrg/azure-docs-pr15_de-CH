Einen Trigger hinzugefügt haben, interessant die dazu mit den Daten, die vom Trigger generiert wird. Folgendermaßen Hinzufügen einer Aktion **SharePoint Online - Datei erstellen** . Diese Aktion erstellt eine Datei in SharePoint Online jedem neuen Artikel Trigger auslöst. 

Konfigurieren der Aktion müssen die folgenden Informationen. Sie werden feststellen, dass sie benutzerfreundliche Daten vom Trigger als Eingabe für einige der Eigenschaften für die neue Datei:

|Datei erstellen|Beschreibung|
|---|---|
|Website-URL|Dies ist die URL des SharePoint Online-Website die neue Datei erstellt werden soll. Wählen Sie die Site aus der Liste.|
|Pfad|Dies ist der Ordner (Website-URL) die neue Datei gespeichert. Suchen Sie und wählen Sie den Ordner.|
|Dateiname|Dies ist der Name der Datei wird erstellt.|
|Inhalt der Datei|Der Inhalt in die Datei geschrieben werden.|

1. Wählen Sie **+ Neuer Schritt** die Aktion hinzuzufügen.  
![](./media/connectors-create-api-sharepointonline/action-1.png)  
- Klicken Sie **Hinzufügen einer Aktion** . Dieses Öffnet das Suchfeld, wobei Sie für jede Aktion suchen können, möchten. Beispielsweise sind SharePoint-Aktionen an.    
![](./media/connectors-create-api-sharepointonline/action-2.png)    
- *Sharepoint* SharePoint Maßnahmen suchen eingeben.
- Wählen Sie die durchzuführende **SharePoint Online - Datei erstellen** .   **Hinweis**: Sie werden aufgefordert, autorisieren Ihrer Anwendung Logik auf der SharePoint-Konto zugreifen, wenn Sie nicht bereits getan haben.    
![](./media/connectors-create-api-sharepointonline/action-3.png)    
- Das Steuerelement **Erstellen** wird geöffnet.   
![](./media/connectors-create-api-sharepointonline/action-4.png)     
- Wählen Sie **Website-URL** , und navigieren Sie zu der Website, in dem Sie die Datei erstellen möchten.     
![](./media/connectors-create-api-sharepointonline/action-5.png)  
- Wählen Sie **Pfad** und suchen Sie nach dem Ordner die neue Datei gespeichert.  
![](./media/connectors-create-api-sharepointonline/action-6.png)  
- Wählen Sie das Steuerelement **Namen** und geben Sie den Namen der Datei, die Sie erstellen möchten. Feststellen Sie für den Dateinamen, dass Sie die Eigenschaften aus der Trigger können zuvor erstellten einfach aus der Liste auswählen.     
![](./media/connectors-create-api-sharepointonline/action-7.png)  
- Wählen Sie das Steuerelement **Inhalt** und geben Sie den Inhalt in die Datei geschrieben, die erstellt wird. Der Inhalt der Datei bemerken Sie, Eigenschaften vom Trigger verwenden, die Sie zuvor erstellt haben. Wählen Sie Eigenschaften aus der Liste aus. Alternativ können Sie **Inhalt** Text direkt in das Steuerelement eingeben. In diesem Beispiel kann ich einige Eigenschaften ausgewählt und Leerzeichen und ein Bindestrich zwischen jede Eigenschaft hinzugefügt.        
![](./media/connectors-create-api-sharepointonline/action-8.png)  
- Speichern des Workflows  
