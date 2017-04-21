Azure-speicherwarteschlangen können mithilfe von Visual Studio **Server-Explorer**.

![Server-Explorer Blobs][Image1]

1. Wählen Sie im Menü **Ansicht** auf **Server-Explorer**.
2. Im Server-Explorer Erweitern des **Azure** -Knotens für das Abonnement, **Storage** -Knoten und für das Speicherkonto angegebenen Dienst Azure-Speicher verbunden.
3. Wählen Sie den Knoten **Warteschlangen** , und wählen Sie im Kontextmenü **Warteschlange erstellen** .
4. Geben Sie einen Namen für den Container, und wählen Sie **OK**.   

Standardmäßig der neue Container ist privat, und geben Sie den speicherzugriffsschlüssel herunterladen Blobs in diesem Container. Wenn Sie die Dateien im Container veröffentlichen möchten, wählen Sie den Container im **Server-Explorer** , und drücken Sie `F4` **im Eigenschaftenfenster** angezeigt. **Öffentlicher Zugriff** auf **Blob**festgelegt. Jeder im Internet sehen Blobs in einem öffentlichen Container jedoch ändern oder löschen, nur, wenn die entsprechende Taste.


[Image1]: ./media/vs-create-blob-container-in-server-explorer/vs-storage-create-blob-containers-in-Server-Explorer.png