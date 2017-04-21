

Registrieren Sie die Anwendung für Push-Benachrichtigung über Apple Push Notification Service (APN) müssen Sie ein neues pushzertifikat App-ID und provisioning-Profil für das Projekt auf der Apple Developer Portal erstellen. App-ID enthält Konfigurationsinformationen, die Ihre Anwendung zum Senden und Empfangen von Pushbenachrichtigungen aktivieren. Diese Einstellungen umfassen das Push Notification Zertifikat mit Apple Push Notification Service (APN) beim Senden und Empfangen von Pushbenachrichtigungen Authentifizierung erforderlich. Weitere Informationen zu diesen Konzepten finden Sie unter der offiziellen [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) -Dokumentation.


####<a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Zertifikat signieren Datei für pushzertifikat generieren

Diese Schritte führen Sie durch die Zertifikatssignatur Anforderung erstellen. Hiermit wird ein pushzertifikat mit APN zu generieren.

1. Auf Ihrem Mac ausführen Schlüsselbund. Sie können **die Dienstprogramme** oder **den Ordner auf der Startrampe** geöffnet werden.

2. **Schlüsselbund**, erweitern Sie **Zertifikat-Assistenten**klicken **Anfordern eines Zertifikats von einer Zertifizierungsstelle...**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)

3. Wählen Sie Ihre **E-Mail-Adresse** und **Allgemeiner Name** muss **auf der Festplatte gespeichert** ist, und klicken Sie auf **Weiter**. Lassen Sie das Feld **E-Mail CA** leer, nicht erforderlich ist.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)

4. Geben Sie einen Namen für die Datei CSR Certificate Signing Request () **Speichern**, wählen Sie den Speicherort in **,**und klicken Sie auf **Speichern**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)

    Dies speichert die CSR-Datei an der ausgewählten Position. Der Standardspeicherort ist der Desktop. Beachten Sie den Speicherort für diese Datei ausgewählt.


####<a name="register-your-app-for-push-notifications"></a>Registrieren Sie Ihre Anwendung für Pushbenachrichtigungen

Erstellen Sie eine neue explizite App-ID für Ihre Anwendung mit Apple und auch für Pushbenachrichtigungen konfigurieren.  

1. [IOS Provisioning Portal](http://go.microsoft.com/fwlink/p/?LinkId=272456) Apple Developer Center navigieren melden Sie sich mit Ihrer Apple ID, **Bezeichner**, und klicken Sie auf **App-IDs**und auf klicken Sie auf die **+** eine neue Applikation registrieren anmelden.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)

2. Die folgenden drei Felder für Ihre neue Anwendung und anschließend auf **Weiter**:

    * **Name**: Geben Sie im Feld **Name** im Abschnitt **Beschreibung ID** einen beschreibenden Namen für Ihre Anwendung.

    * **Paket-ID**: Abschnitt **Expliziten App-ID** Geben Sie ein **Bündel Bezeichner** `<Organization Identifier>.<Product Name>` [App Distribution Handbuch](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8)genannt. Dieser muss übereinstimmen, was auch im Projekt XCode, Xamarin und Cordova für Ihre Anwendung verwendet wird.

    * **Pushbenachrichtigungen**: Kontrollkästchen im Abschnitt **Anwendungsdienste** **Pushbenachrichtigungen** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)

3.  Auf dem Bildschirm App-ID bestätigen überprüfen Sie die Einstellung, und nachdem Sie überprüft haben auf sie **Absenden**

4.  Eingereichte neue App-ID sehen Sie im Registrierungsbildschirm **abgeschlossen** . Klicken Sie auf **Fertig**.

5. Suchen Sie im Developer Center unter App IDs app-ID, die Sie gerade erstellt und auf einer Zeile. App-ID Zeile auf wird die app-Details angezeigt. Klicken Sie auf die Schaltfläche **Bearbeiten** im unteren Bereich.

6. Einen Bildlauf zum unteren Rand des Bildschirms, und klicken Sie auf **Zertifikat erstellen** im Abschnitt **Entwicklung Push SSL-Zertifikat**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)

    Der Assistent "IOS Zertifikat hinzufügen" wird angezeigt.

    > [AZURE.NOTE] In diesem Lernprogramm verwendet ein Entwicklungszertifikat. Dabei wird verwendet, wenn eine produktionszertifikat registrieren. Nur sicherstellen Sie, dass Sie beim Senden von Benachrichtigungen dasselbe Zertifikat verwenden.

7. Klicken Sie auf **Aus Datei**, suchen Sie den Speicherort der Kundenservicemitarbeiter für die pushzertifikat. Klicken Sie auf **generieren**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)

8. Nachdem das Zertifikat vom Portal erstellt wurde, klicken Sie **herunterladen** .

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)

    Diese downloads Signaturzertifikat und auf Ihrem Computer im Ordner Downloads gespeichert.

    ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)

    > [AZURE.NOTE] Standardmäßig heißt die Datei Entwicklungszertifikat **aps_development.cer**.

9. Doppelklicken Sie auf gedownloadete Push Zertifikat **aps_development.cer**. Installiert das neue Zertifikat im Schlüsselbund, wie unten dargestellt:

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)

    > [AZURE.NOTE] Namen in dem Zertifikat möglicherweise anders, aber werden sie vorangestellt **Entwicklung Apple iOS Push Services:**.

10. Klicken Sie im Schlüsselbund auf das neue pushzertifikat, das nur im Bereich **Zertifikate** erstellt. Klicken Sie auf **Exportieren**, benennen Sie die Datei, wählen Sie **P12** und klicken Sie auf **Speichern**.

    Beachten Sie den Dateinamen und den Speicherort des Zertifikats Push exportierte P12. Es verwendet APN Authentifizierung aktivieren auf der Azure-Verwaltungsportal hochladen.



####<a name="create-a-provisioning-profile-for-the-app"></a>Provisioning-Profil für die Anwendung erstellen

1. Wählen Sie in <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS Provisioning Portal</a> **Bereitstellen Profile**, wählen Sie **Alle**und dann auf die **+** , um ein neues Profil zu erstellen. Dies startet den Assistenten **Hinzufügen iOS Provisiong Profil**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)

2. Wählen Sie **iOS-Apps** **Entwicklung** als Provisiong Profil, und klicken Sie auf **Weiter**.


3. Als nächstes wählen Sie die app-ID nur aus der Dropdownliste **App-ID** erstellt und klicken Sie auf **Weiter**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)


4. Wählen Sie im Bildschirm **Wählen Sie Zertifikate aus** das Entwicklungszertifikat zum Signieren von Code aus und auf **Weiter**. Dies ist ein Signaturzertifikat nicht das soeben erstellten pushzertifikat.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)


5. Als nächstes wählen Sie die **Geräte** für Tests und klicken Sie auf **Weiter**

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)


6. Abschließend wählen Sie einen Namen für das Profil in **Profilname**, klicken Sie auf **generieren**.

    ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)
