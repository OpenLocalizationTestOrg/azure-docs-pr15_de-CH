### <a name="prerequisites"></a>Erforderliche Komponenten
- Twilio Konto
- Eine verifizierten Twilio Telefonnummer, die SMS empfangen kann
- Eine verifizierten Twilio Telefonnummer, die SMS senden können

>[AZURE.NOTE] Wenn Sie ein Testkonto Twilio verwenden, können Sie nur Telefonnummern **überprüft** SMS.  

Vor der Verwendung von Ihrem Konto Twilio Logik App müssen Sie die Anwendung Logik für die Verbindung zu Ihrem Konto Twilio autorisieren. Glücklicherweise können Sie problemlos in der Logik app Azure-Portal dazu. 

Hier sind die Schritte Logik-app für die Verbindung zu Ihrem Konto Twilio autorisieren:

1. Um eine Verbindung zu Twilio, Logik app Designer erstellen, **Anzeigen von Microsoft verwalteten APIs** in der Dropdown-Liste Wählen Geben Sie *Twilio* in das Suchfeld ein. Wählen Sie den oder die Aktion, die Sie mögen verwenden:  
  ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Wenn Sie Verbindungskabel Twilio vor erstellt haben, erhalten Sie aufgefordert Ihre Twilio Anmeldeinformationen. Diese Anmeldeinformationen verwendet, um Ihre Logik app Verbindung zu autorisieren und Zugriff auf Ihr Konto Twilio Daten:  
  ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. Sie benötigen **Twilio Konto-Id** und **Zugriffstoken Twilio** aus dem Dashboard in Twilio, so melden Sie sich bei Ihrem Twilio Konto jetzt diese beiden Informationswerte greifen:  
  ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio und Logik apps verwenden unterschiedliche Namen auf diese beiden Informationen identifizieren. Sieht aus wie Sie Logik apps Dialogfeld zuordnen müssen:![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Klicken Sie auf **Verbindung erstellen** :  
  ![](./media/connectors-create-api-twilio/twilio-4.png)
6. Beachten Sie die Verbindung eingerichtet wurde und Sie können die Schritte in Ihrer Anwendung Logik fortsetzen:  
  ![](./media/connectors-create-api-twilio/twilio-5.png)