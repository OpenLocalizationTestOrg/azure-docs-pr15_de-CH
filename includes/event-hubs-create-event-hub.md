## <a name="create-an-event-hub"></a>Erstellen Sie einen Ereignis-Hub

1. [Azure-Portal][]anmelden und auf **neu** oben links auf dem Bildschirm.

2. **Daten + Analytics**klicken Sie auf **Ereignis Hubs**.

    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)

3. Geben Sie einen Namespacenamen Blatt **Erstellen-Namespace** . Das System überprüft sofort, wenn der Name verfügbar ist.

    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)

4. Wenn Namespacenamen verfügbar ist, wählen Sie Tarif (Basic oder Standard). Wählen Sie außerdem ein Azure-Abonnement Ressourcengruppe und Speicherort für die Ressource zu erstellen. 

2. Klicken Sie auf **Erstellen** , um den Namespace zu erstellen.

6. Klicken Sie in der Liste Ereignis Hubs Namespace Namespace neu erstellt.      

    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)

7. Klicken Sie auf **Ereignis Hubs**Blatt Namespace.

    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)

8. Klicken Sie am oberen Rand der Blade **Event Hub hinzufügen**.

    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)

3. Geben Sie einen Namen für Ihr Ereignis-Hub und dann auf **Erstellen**.

    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)

4. Klicken Sie in der Liste Ereignis Hubs neu erstellte Ereignis Hub. 

    ![](./media/event-hubs-create-event-hub/create-event-hub6.png)

5. Im Namespace Blade (nicht das spezifische Ereignis Hub Blade) auf **Shared-Richtlinien**und klicken Sie dann auf **RootManageSharedAccessKey**.

    ![](./media/event-hubs-create-event-hub/create-event-hub7.png)

5. Klicken Sie auf Kopieren, um die **RootManageSharedAccessKey** -Verbindungszeichenfolge in die Zwischenablage kopieren. Speichern Sie diese Verbindungszeichenfolge später im Lernprogramm.

    ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Event-Hub wird jetzt erstellt und haben die Verbindungszeichenfolgen müssen Sie senden und Empfangen von Ereignissen.

[Azure-portal]: https://portal.azure.com/