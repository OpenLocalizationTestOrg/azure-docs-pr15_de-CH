<properties
    pageTitle="Erste Schritte mit Mobile Apps mit Xamarin.Forms"
    description="Folgen Sie dieser Anleitung zum Einstieg in Azure Mobile Apps für Xamarin.Forms-Entwicklung"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-a-xamarinforms-app"></a>Erstellen einer Xamarin.Forms

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##<a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht die Cloud-Back-End-Dienst eine Xamarin.Forms app ein Azure Mobile App Back-End hinzufügen. Erstellen Sie eine neue Mobile App Backend und eine einfache _Aufgabenliste_ Xamarin.Forms Anwendung, die in Azure app-Daten gespeichert.

Dieses Lernprogramm ist eine Voraussetzung für alle anderen Apps Mobile Lernprogramme für Xamarin.Forms.

##<a name="prerequisites"></a>Erforderliche Komponenten

Um dieses Lernprogramm benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Wenn Sie kein Konto haben, melden Sie sich für eine Testversion von Azure und Abrufen von frei 10 Mobile Apps, die Sie auch nach der Testphase nutzen können. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio mit Xamarin. Weitere Informationen finden Sie in [Installation und Setup für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) . 

* Mac Xcode Version 7.0 oder höher und Xamarin Studio Community installiert. Siehe [Installation und Setup für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) und [Setup, Installation und Überprüfung für Mac-Benutzer](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](https://tryappservice.azure.com/?appServiceName=mobile)sofort eine kurzlebige Starter Mobile App in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="create-a-new-azure-mobile-app-backend"></a>Erstellen Sie ein neues Azure Mobile App backend

Folgen Sie erstellen Sie ein neues Mobile App backend

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


Sie haben jetzt eine Azure Mobile App-Back-End bereitgestellt, die von mobile Clientanwendungen verwendet werden kann. Anschließend downloadet Serverprojekt für eine einfache "Aufgabenliste" Back-End- und Azure veröffentlichen.

## <a name="configure-the-server-project"></a>Konfigurieren Sie das Serverprojekt

Schritte das Serverprojekt Node.js oder .NET Backend Verwendung konfigurieren.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##<a name="download-and-run-the-xamarinforms-solution"></a>Herunterladen und Ausführen der Projektmappe Xamarin.Forms

Hier stehen Ihnen einige Optionen. Die Lösung auf einem Mac herunterladen und in Xamarin Studio öffnen und die Lösung auf einem Windows-Computer herunterladen und öffnen Sie es in Visual Studio mit einem vernetzten Mac iOS-app erstellen. Finden Sie unter [Installation und Setup für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) benötigen Sie nähere Informationen zu Xamarin Setup-Szenarien.

Gehen:

 1. Öffnen Sie auf Ihrem Mac oder Windows Computer [Azure-Portal] im Browser-Fenster.
 2. Klicken Sie auf Blatt Einstellungen für Ihre Mobile **Erste Schritte** (Mobil) > **Xamarin.Forms**. Klicken Sie unter Schritt 3 **Erstellen einer neuen Anwendung** , wenn nicht bereits ausgewählt ist.  Klicken Sie anschließend auf die Schaltfläche **Download** .

    Diese downloads ein Projekt mit einer Clientanwendung, die mit der mobilen Anwendung verbunden ist. Speichern Sie die komprimierte Datei auf Ihrem lokalen Computer, und notieren Sie wo speichern.

 3. Extrahieren Sie das Projekt, das Sie heruntergeladen und in Xamarin Studio oder Visual Studio öffnen.

    ![][9]

    ![][8]


##<a name="optional-run-the-ios-project"></a>(Optional) Führen Sie das iOS-Projekt

Dieser Abschnitt ist für die Ausführung des Projekts Xamarin iOS für iOS-Geräte. Wenn Sie nicht mit iOS-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

####<a name="in-xamarin-studio"></a>In Xamarin

1. Maustaste auf iOS-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Klicken Sie im Menü **Ausführen** auf **Debuggen starten** , um das Projekt erstellen und Starten der Anwendung im Emulator iPhone.

####<a name="in-visual-studio"></a>In Visual Studio
1. Maustaste auf iOS-Projekt, und klicken Sie dann auf **als Startprojekt festlegen**.
2. Klicken Sie im Menü **Erstellen** auf **Configuration Manager**.
3. Wählen Sie im Dialogfeld **Konfigurations-Manager** die Kontrollkästchen **Erstellen** und **Bereitstellen** der iOS-Projekt.
4. Drücken Sie **F5** , um das Projekt erstellen und Starten der Anwendung im Emulator iPhone.

    >[AZURE.NOTE] Haben Probleme unterstützen ausführen NuGet-Paket-Manager und Update auf die neueste Version der Xamarin Pakete. Manchmal können die Schnellstart-Projekte hinter der neuesten aktualisiert.    

In der Anwendung geben Sie einen aussagekräftigen Text wie _Erfahren Xamarin_ der **+** Schaltfläche.

![][10]

Sendet eine POST-Anforderung an das neue mobile app Backend in Azure gehostet. Daten aus der Anforderung werden in der TodoItem-Tabelle eingefügt. In der Tabelle gespeicherten Elemente von mobile-app Backend zurückgegeben werden, und die Daten in der Liste angezeigt.

>[AZURE.NOTE]
> Sie finden den Code, der mobile Anwendung Backend in der Datei TodoItemManager.cs C# das portable Klassenbibliothek-Projekt Ihrer Lösung zugreift.

##<a name="optional-run-the-android-project"></a>(Optional) Android-Projekt ausführen

Dieser Abschnitt ist zum Ausführen von Xamarin Droid Projekt für Android. Wenn Sie keine Geräte arbeiten, können Sie diesen Abschnitt überspringen.

####<a name="in-xamarin-studio"></a>In Xamarin

1. Maustaste auf Android-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Klicken Sie im Menü **Ausführen** auf **Debuggen starten** , um das Projekt und starten Sie die Anwendung im Android-Emulator.

####<a name="in-visual-studio"></a>In Visual Studio
1. Maustaste auf Android (Droid)-Projekt, und klicken Sie dann auf **als Startprojekt festlegen**.
4. Klicken Sie im Menü **Erstellen** auf **Configuration Manager**.
5. Wählen Sie im Dialogfeld **Konfigurations-Manager** die Kontrollkästchen **Erstellen** und **Bereitstellen** des Projekts Android.
6. Drücken Sie **F5** , um das Projekt und starten Sie die Anwendung im Android-Emulator.

    >[AZURE.NOTE] Haben Probleme unterstützen ausführen NuGet-Paket-Manager und Update auf die neueste Version der Xamarin Pakete. Manchmal können die Schnellstart-Projekte hinter der neuesten aktualisiert.    


In der Anwendung geben Sie einen aussagekräftigen Text wie _Erfahren Xamarin_ der **+** Schaltfläche.

![][11]

Sendet eine POST-Anforderung an das neue mobile app Backend in Azure gehostet. Daten aus der Anforderung werden in der TodoItem-Tabelle eingefügt. In der Tabelle gespeicherten Elemente von mobile-app Backend zurückgegeben werden, und die Daten in der Liste angezeigt.

> [AZURE.NOTE]
> Sie finden den Code, der mobile Anwendung Backend in der Datei TodoItemManager.cs C# das portable Klassenbibliothek-Projekt Ihrer Lösung zugreift.


##<a name="optional-run-the-windows-project"></a>(Optional) Führen Sie das Windows-Projekt


Dieser Abschnitt ist für die Ausführung von Xamarin WinApp-Projekt für Windows-Geräte. Wenn Sie nicht mit Windows arbeiten, können Sie diesen Abschnitt überspringen.


####<a name="in-visual-studio"></a>In Visual Studio
1. Maustaste auf Windows-Projekte, und klicken Sie auf **als Startprojekt festlegen**.
4. Klicken Sie im Menü **Erstellen** auf **Configuration Manager**.
5. Wählen Sie im Dialogfeld **Konfigurations-Manager** die Kontrollkästchen **Erstellen** und **Bereitstellen** des Windows-Projekts, das Sie ausgewählt haben.
6. Drücken Sie **F5** , um das Projekt und die Anwendung in einem Windows-Emulator starten.

    >[AZURE.NOTE] Haben Probleme unterstützen ausführen NuGet-Paket-Manager und Update auf die neueste Version der Xamarin Pakete. Manchmal können die Schnellstart-Projekte hinter der neuesten aktualisiert.    


In der Anwendung geben Sie einen aussagekräftigen Text wie _Erfahren Xamarin_ der **+** Schaltfläche.

Sendet eine POST-Anforderung an das neue mobile app Backend in Azure gehostet. Daten aus der Anforderung werden in der TodoItem-Tabelle eingefügt. In der Tabelle gespeicherten Elemente von mobile-app Backend zurückgegeben werden, und die Daten in der Liste angezeigt.

![][12]

> [AZURE.NOTE]
> Sie finden den Code, der mobile Anwendung Backend in der Datei TodoItemManager.cs C# das portable Klassenbibliothek-Projekt Ihrer Lösung zugreift.

##<a name="next-steps"></a>Nächste Schritte

* [Authentifizierung für Ihre Anwendung hinzufügen](app-service-mobile-xamarin-forms-get-started-users.md)  
Enthält Informationen zum Authentifizieren von Benutzern der Anwendung mit Identitätsanbieter.

* [Push-Benachrichtigung für Ihre Anwendung hinzufügen](app-service-mobile-xamarin-forms-get-started-push.md)  
Informationen Sie zum Push Notifications für Ihre Anwendung unterstützen und Mobile App Backend um Azure Notification Hubs Pushbenachrichtigungen senden konfigurieren hinzufügen.

* [Offline-Synchronisierung für Ihre Anwendung aktivieren](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Informationen Sie zum offline-Unterstützung mithilfe einer Mobile App-Back-End hinzufügen. Offline synchronisieren können Endbenutzer eine mobile Anwendung interagieren&mdash;anzeigen, hinzufügen oder Ändern von Daten&mdash;selbst wenn keine Verbindung zum Netzwerk.

* [Verwendung den verwalteten Client für Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
Informationen Sie zum Arbeiten mit verwalteten Client SDK in Ihrer Anwendung Xamarin. 


<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure-Portal]: https://portal.azure.com/

