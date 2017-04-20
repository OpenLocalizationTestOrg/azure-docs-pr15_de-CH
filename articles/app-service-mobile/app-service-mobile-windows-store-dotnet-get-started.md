<properties
    pageTitle="Erstellen Sie eine universelle Windows Plattform (UWP), die auf Mobile Apps | Microsoft Azure"
    description="Dieses Lernprogramm Einstieg Azure mobile app-Back-Ends für universelle Windows-Plattform (UWP)-Anwendungsentwicklung in C#, Visual Basic oder JavaScript."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-windows-app"></a>Erstellen einer Windows-Anwendung

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht die universelle Windows-Plattform (UWP) app Cloud-Back-End-Dienst hinzufügen. Weitere Informationen finden Sie unter [Was sind Mobile Apps](app-service-mobile-value-prop.md). Es folgen Screenshots aus abgeschlossenen app:

![Desktop-app abgeschlossen](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Auf dem Desktop ausgeführt. 

![Abgeschlossene Phone-app](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Auf einem Telefon

Dieses Lernprogramm ist eine Voraussetzung für andere Mobile App Lernprogramme für UWP-apps. 

##<a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Haben Sie ein Konto, können Sie eine Azure testen und bis zu 10 freie mobile apps, die Sie auch nach der Testphase nutzen können. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* [Visual Studio Community 2015] oder höher.

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie für ein Azure-Konto, gehen Sie [versuchen App](https://tryappservice.azure.com/?appServiceName=mobile)Service. Dort sofort können eine kurzlebige Starter-app in App Service – keine Kreditkarte und keine Zusagen.

##<a name="create-a-new-azure-mobile-app-backend"></a>Erstellen Sie ein neues Azure Mobile App backend

Folgen Sie erstellen Sie ein neues Mobile App backend

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Sie haben jetzt eine Azure Mobile App-Back-End bereitgestellt, die von mobile Clientanwendungen verwendet werden kann. Anschließend downloadet Serverprojekt für eine einfache "Aufgabenliste" Back-End- und Azure veröffentlichen.

## <a name="configure-the-server-project"></a>Konfigurieren Sie das Serverprojekt

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-client-project"></a>Herunterladen und Ausführen des Projekts client

Nachdem Sie Ihre Mobile App-Back-End konfiguriert haben, eine neue Clientanwendung erstellen oder ändern eine vorhandene Anwendung Azure Verbindung. In diesem Abschnitt Laden Sie ein Vorlagenprojekt UWP Anwendung, die die Mobile Anwendung Back-End-Verbindung angepasst.

1. Klicken Sie im **Schnellstart** -Blade für Mobile App Backend auf **Erstellen einer neuen Applikation** > **herunter**, und extrahieren Sie die komprimierten Projektdateien auf dem lokalen Computer.

    ![Downloaden Sie Windows Schnellstart-Projekt](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

3. (Optional) Der gleichen Projektmappe wie das Serverprojekt UWP-app-Projekt hinzugefügt. Dadurch einfacher zu debuggen und Testen der Anwendung und dem Back-End in der gleichen Visual Studio-Projektmappe tun möchten. Um eine UWP-app-Projekt der Lösung hinzuzufügen, müssen Sie Visual Studio 2015 oder höher verwenden.

4. Drücken Sie mit der Anwendung UWP als Startprojekt F5 zum Bereitstellen und die Anwendung ausführen.

5. Geben Sie in der app aussagekräftigen Text wie *das Lernprogramm abgeschlossen*in das Textfeld **eine TodoItem einfügen** , und klicken Sie auf **Speichern**.

    ![Vollständige Windows Schnellstart-desktop](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Sendet eine POST-Anforderung an das neue mobile app-Backend in Azure gehostet wird.

6. (Optional) Beenden Sie die Anwendung und starten Sie es auf einem anderen Gerät oder Emulator für Mobilgeräte.

    ![Windows Schnellstart vollständige Telefonnummer](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Beachten Sie, dass Daten aus dem vorherigen Schritt von Azure geladen werden, nachdem der UWP App. 

##<a name="next-steps"></a>Nächste Schritte

* [Authentifizierung für Ihre Anwendung hinzufügen](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Enthält Informationen zum Authentifizieren von Benutzern der Anwendung mit Identitätsanbieter.

* [Push-Benachrichtigung für Ihre Anwendung hinzufügen](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Informationen Sie zum Push Notifications für Ihre Anwendung unterstützen und Mobile App Backend um Azure Notification Hubs Pushbenachrichtigungen senden konfigurieren hinzufügen.

* [Offline-Synchronisierung für Ihre Anwendung aktivieren](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Informationen Sie zum offline-Unterstützung mithilfe einer Mobile App-Back-End hinzufügen. Offline synchronisieren können Endbenutzer eine mobile Anwendung interagieren&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;selbst wenn keine Verbindung zum Netzwerk.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203
