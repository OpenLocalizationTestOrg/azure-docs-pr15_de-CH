1. [Azure-Portal]anmelden.

2. Klicken Sie auf **+ neu** > **Web + Mobile** > **Mobile-Anwendung**, geben Sie einen Namen für Ihre Mobile App-Backend.

3. Wählen Sie für die **Ressourcengruppe**eine vorhandene Ressourcengruppe aus oder erstellen Sie ein neues (mit demselben Namen als app.) 
 
    Sie wählen einen anderen App Service-Plan oder eine neue erstellen. Weitere Informationen zu App Services Pläne und erstellen ein neues Plans in unterschiedliche Preise tier und in die gewünschte Position [Azure App Service-Pläne ausführliche Übersicht](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. Für **App-Serviceplan**ist Standardpläne (im [Standard-Tier](https://azure.microsoft.com/pricing/details/app-service/)) ausgewählt. Sie können auch einen anderen Plan oder [einen neuen erstellen](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan)auswählen. Die App Service-Plan Bestimmen der [Funktionen Kosten und compute-Ressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihrer Anwendung. 

    Nachdem Sie den Plan entscheiden, klicken Sie auf **Erstellen**. Backend Mobile-Anwendung wird erstellt. 
    
6. Blatt **Einstellungen** für neue Mobile App Backend-klicken Sie auf **Quick Start** > Ausführen app > **Verbinden einer Datenbank**. 

    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)

7. Das Blade **Datenquelle hinzufügen** klicken Sie **SQL** > **neue Datenbank erstellen**, geben Sie die Datenbank- **Namen**, einen Tarif wählen und dann auf **Server**.  Sie können diese neue Datenbank verwenden. Wenn bereits eine Datenbank in demselben Speicherort haben, können Sie stattdessen **vorhandene Datenbank verwenden**. Eine Datenbank an einem anderen Ort ist nicht höhere Latenz und Bandbreite empfohlen.
 
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)

8. In **neue Server** -Blade einen eindeutigen Namen im Feld **Servername** , Benutzername und ein Kennwort **Zulassen Azure**Dienste Zugriff auf Server, und klicken Sie auf **OK**. Die neue Datenbank wird erstellt.

9. Klicken Sie auf **Verbindungszeichenfolge**in Blade **Datenquelle hinzufügen** Geben Sie die Werte für Benutzername und Kennwort für die Datenbank, und klicken Sie auf **OK**. Warten der Datenbank erfolgreich zunächst bereitgestellt werden.

<!-- URLs. -->
[Azure-Portal]: https://portal.azure.com/
