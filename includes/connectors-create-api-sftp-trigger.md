Fügen Sie einen Trigger.

1. *Sftp* Designer apps Logik in das Suchfeld eingeben und wählen Sie dann den Trigger **SFTP - Wenn eine Datei hinzugefügt oder geändert wird**   
![SFTP auslösenden Bild 1](./media/connectors-create-api-sftp/trigger-1.png)  
- Öffnet das Steuerelement **bei eine Datei hinzugefügt oder geändert wird**  
![SFTP auslösenden Bild 2](./media/connectors-create-api-sftp/trigger-2.png)  
- Wählen Sie die **** auf der rechten Seite des Steuerelements befindet. Daraufhin wird das Ordner Datumsauswahl-Steuerelement  
![SFTP auslösenden Bild 3](./media/connectors-create-api-sftp/action-1.png)  
- Wählen Sie **SFTP** auf den Stammordner als Ordner für neue oder geänderte Dateien überwachen. Der Stammordner ist jetzt im **Ordner** -Steuerelement angezeigt.  
![SFTP auslösenden Bild 4](./media/connectors-create-api-sftp/action-2.png)   

An diesem Punkt wurde Ihre Anwendung Logik Trigger konfiguriert, die Ausführung der anderen Auslöser und Aktionen im Workflow beginnen, wenn eine Datei geändert oder SFTP-Ordners erstellt. 

>[AZURE.NOTE]Für eine Anwendung Logik funktioniert muss es mindestens einen Trigger und eine Aktion enthalten. Führen Sie die Schritte im nächsten Abschnitt, um eine Aktion hinzuzufügen.  