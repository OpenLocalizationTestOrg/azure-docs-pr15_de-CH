
1. Im Visual Studio-Projektmappen-Explorer mit der rechten Maustaste Windows Store-app-Projekt, klicken Sie auf **Speichern** > **App mit dem Speicher zuordnen**.

    ![Windows Store app zuordnen](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)

2. Im Assistenten **Weiter**, Ihr Microsoft-Konto melden Sie an, geben Sie einen Namen für Ihre Anwendung in **einer neuen Anwendungsname**, klicken auf **Reservieren**.

3. Nach Anwendung Registrierung erfolgreich erstellt wurde, wählen Sie den Namen der neuen app, klicken Sie auf **Weiter**und dann auf **zuordnen**. Dadurch wird das Anwendungsmanifest erforderliche Windows Store-Registrierungsinformationen hinzugefügt.

7. Wiederholen Sie die Schritte 1 und 3 für Windows Phone Store-app-Projekt mit der zuvor erstellten Registrierung für Windows Store-app.  

7. Navigieren Sie im [Windows-Entwicklungscenter](https://dev.windows.com/en-us/overview)mit Ihrem Microsoft-Konto anmelden, klicken Sie auf die neue Anwendung Eintragung in **Meine apps**und **Dienste** > **Pushbenachrichtigungen**.

8. Auf der Seite **Pushbenachrichtigungen** auf **Live Services Website** unter **Windows Push Notification Services (WNS) und Microsoft Azure Mobile Apps**und notieren Sie die Werte des **Pakets SID** und der *aktuelle* Wert **Anwendung**Schlüssel. 

    ![App-Einstellung im Developer center](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] Die geheimen Schlüssel und SID sind wichtige Anmeldeinformationen. Teilen Sie diese Werte mit weder mit Ihrer Anwendung verteilen.
