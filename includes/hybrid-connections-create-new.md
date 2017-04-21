
1. Melden Sie sich auf dem lokalen Computer (Dies ist das alte Portal) [Azure-Verwaltungsportal](http://manager.windowsazure.com) an.

2. Wählen Sie unten im Navigationsbereich **und neu** > **App Services** > **BizTalk-Dienst** > **Benutzerdefinierte**.

3. **BizTalk-Dienstname** und wählen Sie eine **Ausgabe**. 

    In diesem Lernprogramm wird **mobile1**verwendet. Sie müssen einen eindeutigen Namen für die neue BizTalk-Service bereitstellen.

4. Erstellte BizTalk Service Registerkarte **Hybrid-Verbindungen** , klicken Sie auf **Hinzufügen**.

    ![Hybridverbindung hinzufügen](./media/hybrid-connections-create-new/3.png)

    Dies erstellt eine neue hybridverbindung.

5. Geben Sie einen **Namen** und **Hostnamen** für die hybridverbindung, und legen Sie **Port** auf `1433`. 
  
    ![Hybridverbindung konfigurieren](./media/hybrid-connections-create-new/4.png)

    Der Hostname ist der Name des lokalen Servers. Die hybridverbindung Zugriff auf SQL Server auf Port 1433 konfigurieren. Bei Verwendung eine benannte Instanz von SQL Server verwenden Sie statischen Port bereits definiert.

6. Nachdem die neue Verbindung erstellt wird, den Status der neuen Verbindung zeigt **lokale setup unvollständig**.

7. Navigieren Sie zu Ihren mobilen Dienst klicken Sie auf **Konfigurieren**, scrollen Sie **Hybrid-Verbindungen** und auf **Hybrid-Verbindung hinzufügen**, dann wählen Sie die soeben erstellte hybridverbindung und klicken Sie auf **OK**.

    Dadurch wird Ihr Mobilfunkanbieter die neue hybridverbindung verwenden.

Als Nächstes müssen Sie das Hybrid-Verbindungs-Manager auf dem lokalen Computer installiert.