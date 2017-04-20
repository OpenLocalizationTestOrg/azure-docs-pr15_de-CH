
Standardmäßig kann eine Mobile Anwendung Back-End-APIs anonym aufgerufen werden. Anschließend müssen Sie nur authentifizierte Clients Zugriff.  

+ **Node.js Backend (Portal)** :  
    
    Mobile App **Einstellungen**auf **Einfache Tabellen** und wählen Sie die Tabelle. Klicken Sie auf **Berechtigungen ändern**, **Authentifizierter Zugriff** für alle Berechtigungen, klicken Sie auf **Speichern**. 

+ **.NET Backend (C#)**:  

    Navigieren Sie in Project Server zu **Domänencontrollern** > **TodoItemController.cs**. Hinzufügen der `[Authorize]` -Attribut auf die Klasse **TodoItemController** wie folgt. Zum Einschränken des Zugriffs auf bestimmte Methoden: Dieses Attribut kann auch auf die Methoden der Klasse angewendet werden. Veröffentlichen Sie Project Server.


        [Authorize]
        public class TodoItemController : TableController<TodoItem>

+ **Node.js Backend (über Node.js-Code)** :  
    
    Um die Authentifizierung für den Zugriff auf die Tabelle Serverskripts Node.js fügen Sie folgende Zeile hinzu:


        table.access = 'authenticated';

    Weitere Informationen finden Sie in [wie: Authentifizierung für den Zugriff auf Tabellen](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Das Codeprojekt Schnellstart von der Website herunterladen, finden Sie [wie: Herunterladen Node.js Backend-Schnellstart Codeprojekt Git mit](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).

