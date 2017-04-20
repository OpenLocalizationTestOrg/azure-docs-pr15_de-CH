####<a name="configuring-the-ios-project-in-xamarin-studio"></a>IOS-Projekt konfigurieren in Xamarin Studio

1. Öffnen Sie im Xamarin.Studio **Info.plist**und aktualisiert die **Paket-ID** mit der Paket-ID, die zuvor mit der neuen App-ID erstellt

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-21.png)

2. **Hintergrund-Modi** scrollen und das **Hintergrund-Modus aktivieren** und das **Remote-Benachrichtigung** überprüfen. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-22.png)

3. Doppelklicken Sie das Projekt im Solution **Projektoptionen**öffnen.

4.  Wählen Sie **Paket signieren iOS** unter **Erstellen**, und die entsprechende **Identität** und **Provisioning Profil** nur für dieses Projekt eingerichteten. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-20.png)

    Dadurch das Projekt Code Signing das neue Profil verwendet. Offizielle Bereitstellung Dokumentation Xamarin Gerät finden Sie unter [Xamarin Gerät bereitstellen].

####<a name="configuring-the-ios-project-in-visual-studio"></a>Konfigurieren von iOS-Projekt in Visual Studio

1. Klicken Sie in Visual Studio das Projekt und dann auf **Eigenschaften**.

2. Die Eigenschaftenseiten klicken Sie **iOS-Anwendung** und aktualisieren Sie der **Bezeichner** die ID, die Sie zuvor erstellt haben.

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-23.png)

3. Wählen Sie auf der Registerkarte **iOS Paket signieren** die entsprechende **Identität** und **Provisioning Profil** nur für dieses Projekt eingerichteten. 

    ![](./media/app-service-mobile-xamarin-ios-configure-project/mobile-services-ios-push-24.png)

    Dadurch das Projekt Code Signing das neue Profil verwendet. Offizielle Bereitstellung Dokumentation Xamarin Gerät finden Sie unter [Xamarin Gerät bereitstellen].

4. Doppelklick zu öffnen, und aktivieren Sie unter Hintergrund Modi **RemoteNotifications** Info.plist. 



[Xamarin Gerät bereitstellen]: http://developer.xamarin.com/guides/ios/getting_started/installation/device_provisioning/