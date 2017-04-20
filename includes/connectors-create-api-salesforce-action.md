Eine Bedingung hinzugefügt haben, interessant die dazu mit den Daten, die vom Trigger generiert wird. Gehen Sie **Salesforce - Get Objekt** -Aktion hinzufügen. Diese Aktion erhalten Daten jedes Mal neu erstellt wird. Sie fügen außerdem eine zweite Aktion, die die Daten aus Salesforce - zu einem Objekt-Aktion eine e-Mail mit Office 365-Connector verwenden.  

Konfigurieren der Aktion müssen die folgenden Informationen. Sie werden feststellen, dass sie benutzerfreundliche Daten vom Trigger als Eingabe für einige der Eigenschaften für die neue Datei:

|Datei erstellen|Beschreibung|
|---|---|
|Objekttyp|Dies ist der Salesforce-Objekt, das Sie interessiert sind. Beispiele sind Lead Konto usw..|
|Objekt-ID|Dies ist einen Bezeichner für das Objekt.|


1. Klicken Sie **Hinzufügen einer Aktion** . Dieses Öffnet das Suchfeld, wobei Sie für jede Aktion suchen können, möchten. Beispielsweise sind Salesforce Aktionen an.      
![Salesforce Aktion Bild 1](./media/connectors-create-api-salesforce/action-1.png)  
- *Salesforce* Maßnahmen Salesforce suchen eingeben.
- Wählen Sie die durchzuführende **Salesforce - Objekt abrufen** .   **Hinweis**: Sie werden aufgefordert, autorisieren Ihre app Logik der Salesforce-Konto zugreifen, wenn Sie nicht bereits getan haben.    
![Salesforce Aktion Bild 2](./media/connectors-create-api-salesforce/action-2.png)    
- **Objekt** öffnen.  
- Wählen Sie den Objekttyp *führen* .
- Wählen Sie das **Objekt-ID** -Steuerelement.
- Wählen Sie **...** die Liste der Token zu erweitern, die als Eingabe für Aktionen verwendet werden kann.       
![Salesforce Aktion Bild 3](./media/connectors-create-api-salesforce/action-3.png)    
- **Kennung** Steuerelement wird geöffnet.   
![Salesforce Aktion Bild 4](./media/connectors-create-api-salesforce/action-4.png)     
- Beachten Sie, dass das Token Kennung nun in der Objekt-ID-Steuerelement ist, Objekt-Abruf mit ID suchen, die auf die potenziellen Kunden ID führen, die diese Anwendung Logik ausgelöst.  
![Salesforce Aktion Bild 5](./media/connectors-create-api-salesforce/action-5.png)  
- Speichern Sie Ihre Arbeit. Das ist es, Objekt-Abruf Ihrer Anwendung Logik hinzugefügt haben. Das Get-Steuerelement sollte wie folgt aussehen:    
![Salesforce Aktion Bild 6](./media/connectors-create-api-salesforce/action-6.png)  

Eine Aktion zu einem Lead hinzugefügt haben, sollten Sie mit den neu erstellten Leads führen. In einem Unternehmen möchten Sie per e-Mail an einer Verteilerliste benachrichtigen neu erstellt wurde. Wir verwenden Office 365 eine e-Mail mit Informationen aus dem neuen Lead Objekt in Salesforce.  

1. Wählen Sie **eine Aktion hinzufügen** , und geben Sie *e-Mails* im Suchsteuerelement. Dadurch werden die Aktionen, die mit senden und Empfangen von e-Mail.  
- Markieren Sie das Listenelement **Office 365 Outlook - eine e-Mail senden** . Wenn bereits eine *Verbindung* zu Ihrem Office 365-Konto erstellt haben, werden Sie aufgefordert, Ihre Office 365-Anmeldeinformationen erstellen sie jetzt eingeben. Wenn Sie fertig sind, öffnet das Steuerelement **eine e-Mail senden** .        
![Salesforce Aktion Bild 7](./media/connectors-create-api-salesforce/action-7.png)  
- Geben Sie die e-Mail, die e-Mail an **das Steuerelement** senden möchten.
-  Geben Sie im Steuerelement **Betreff** *erstellten neuen Lead* - und wählen Sie das *Unternehmen* Token. Feld *Unternehmen* neue Leads in Salesforce erstellt erscheint.  
-  Im Steuerelement **Text** können die Token vom neuen Lead-Objekt auswählen und Eingabe kann auch Text im Textkörper der e-Mail angezeigt werden soll. Hier ist ein Beispiel:  
![Salesforce Aktion Bild 8](./media/connectors-create-api-salesforce/action-8.png)   
- Speichern Sie den Workflow.  

Das wars. Ihre Anwendung Logik ist abgeschlossen.  

Nun können Sie Ihre Anwendung Logik testen: in Salesforce neu erstellt, die die Bedingung erfüllen Sie erstellt.  Wenn Sie diese exemplarische Vorgehensweise vollständig ausgeführt, erstellen Sie einfach einen Lead mit e-Mail-Adressen, die *amazon.com* es enthält. Nach wenigen Sekunden sollte Ihre Anwendung Logik ausgelöst und die Ergebnisse können wie folgt aussehen:  
![Salesforce Aktion Bild 9](./media/connectors-create-api-salesforce/action-9.png)  

