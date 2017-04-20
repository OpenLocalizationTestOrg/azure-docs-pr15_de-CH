### <a name="prerequisites"></a>Erforderliche Komponenten

- Ein [SMTP-](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) Konto  


Bevor Ihr SMTP-Konto in eine Logik-app verwenden können, müssen Sie Logik app Verbindung zum SMTP-Konto autorisieren. Glücklicherweise können Sie problemlos in der Logik app Azure-Portal dazu.  

Hier sind die Schritte ermächtigen Ihre app Logik für die Verbindung zu Ihrem SMTP-Konto:  
1. Um eine Verbindung zu SMTP Logik app Designer erstellen, **Anzeigen von Microsoft verwalteten APIs** in der Dropdown-Liste Wählen Geben Sie *SMTP* in das Suchfeld ein. Wählen Sie den oder die Aktion, die Sie mögen verwenden:  
![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Wenn Sie vor SMTP-Verbindung erstellt haben, abrufen Sie aufgefordert der SMTP-Anmeldeinformationen. Diese Anmeldeinformationen verwendet Ihre Anwendung Logik Verbindung autorisiert und Ihr SMTP-Konto-Daten zugreifen:  
![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Beachten Sie die Verbindung eingerichtet wurde und Sie können die Schritte in Ihrer Anwendung Logik fortsetzen:  
 ![](./media/connectors-create-api-smtp/smtp-3.png)  

