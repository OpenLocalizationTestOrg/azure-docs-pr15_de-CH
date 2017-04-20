Einen Trigger hinzugefügt haben, interessant die dazu mit den Daten, die vom Trigger generiert wird. Führen Sie diese Schritte zum Hinzufügen einer Aktion **SFTP - Ordner extrahieren** . Dadurch wird der Inhalt einer Datei extrahieren Sie definierte Bedingung erfüllt sind. 

Konfigurieren der Aktion müssen die folgenden Informationen. Sie werden feststellen, dass sie benutzerfreundliche Daten vom Trigger als Eingabe für einige der Eigenschaften für die neue Datei:

|SFTP - Extrakt Folder-Eigenschaft|Beschreibung|
|---|---|
|Quelldateipfad-Archiv|Dies ist der Pfad für die Datei wird extrahiert. Wählen eines der Token in einem früheren oder SFTP-Server den Pfad zu suchen.|
|Zielordnerpfad|Dies ist der Pfad, in dem die extrahierten Dateien gespeichert werden. Wählen Sie eines der Token in einem früheren als der Zielpfad oder SFTP-Server durchsuchen, und wählen Sie einen Pfad.|
|Überschreiben?|Gibt an, ob eine Datei mit demselben Namen wie die extrahierte Datei im Ordner Zielpfad gefunden wird, wenn die vorhandene Datei überschrieben werden soll.|

Fangen die Aktion um die Dateien zu extrahieren, wenn die definierte Bedingung *True*ergibt. 

1. Wählen Sie **eine Aktion hinzufügen**.        
![SFTP Aktion Bedingung Bild 6](./media/connectors-create-api-sftp/condition-6.png)   
- Wählen Sie die Aktion **SFTP - Ordner extrahieren**      
![SFTP Aktion Bedingung Bild 7](./media/connectors-create-api-sftp/condition-7.png)   
- **Pfad der Archivdatei Quelle** auswählen              
![SFTP Aktion Bedingung Bild 9](./media/connectors-create-api-sftp/condition-9.png)   
- Wählen Sie das **Pfad** -Token. Dies bedeutet, dass verwendet den Pfad der Datei, die der Trigger gefunden als den Quellpfad der Archive.           
![SFTP Aktion Bedingung Bild 10](./media/connectors-create-api-sftp/condition-10.png)   
- Wählen Sie **Ordner Zielpfad**           
![SFTP Aktion Bedingung Bild 11](./media/connectors-create-api-sftp/condition-11.png)   
- Wählen Sie das **Pfad** -Token. Dies bedeutet, dass Sie verwendet den Pfad der Datei, die der Trigger gefunden als Zielpfad für die extrahierten Dateien.   
- Geben Sie *\ExtractedFile* in das Steuerelement **Zielordnerpfad** . Hierzu nach Datei Pfad Token im Ordner Pfad Zielsteuerelement.         
![SFTP Aktion Bedingung Bild 12](./media/connectors-create-api-sftp/condition-12.png)   
- Geben Sie *True* in der **Überschreiben?* Steuerelement vorhandene Dateien überschrieben werden sollen, wenn sie denselben Namen wie die extrahierten Dateien haben.      
![SFTP Aktion Bedingung Bild 13](./media/connectors-create-api-sftp/condition-13.png)   
- Speichern des Workflows  
