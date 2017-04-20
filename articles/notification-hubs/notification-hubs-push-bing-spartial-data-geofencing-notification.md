<properties
    pageTitle="Geometrische eingezäunt Pushbenachrichtigungen Azure Notification Hubs mit Bing räumliche Daten | Microsoft Azure"
    description="In diesem Lernprogramm erfahren Sie, wie standortbasierte Pushbenachrichtigungen Azure Notification Hubs mit Bing räumliche Daten."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="Push-Benachrichtigung, Push-Benachrichtigung"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Geometrische eingezäunt Pushbenachrichtigungen Azure Notification Hubs mit Bing Geodaten
 
 > [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein aktives Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

In diesem Lernprogramm erfahren Sie, wie standortbasierte Pushbenachrichtigungen Azure Notification Hubs mit Bing Geodaten von Universal Windows-Plattform-Anwendung genutzt werden.

##<a name="prerequisites"></a>Erforderliche Komponenten
Zunächst müssen Sie sicherstellen, dass alle die Software und Dienstleistungen erforderlichen Komponenten verfügen:

* [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) oder höher ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) wird auch tun). 
* Neueste Version von [Azure SDK](https://azure.microsoft.com/downloads/). 
* [Entwicklungscenter für Bing Maps-Konto](https://www.bingmapsportal.com/) (Sie können kostenlos erstellen und Ihr Microsoft-Konto zuordnen). 

##<a name="getting-started"></a>Erste Schritte

Zunächst erstellen das Projekt. Starten Sie in Visual Studio ein neues Projekt vom Typ **Leere App (Universal Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Nach Abschluss die Erstellung müssen Sie den Kabelbaum für die Anwendung selbst. Nun richten Sie für geofencing-Infrastruktur. Denn wir Bing-Dienste für diese verwenden, ist ein öffentlicher REST API-Endpunkt, der bestimmten Stelle Frames Abfragen ermöglicht:

    http://spatial.virtualearth.net/REST/v1/data/
    
Sie müssen dafür die folgenden Parameter angeben:

* **Datenquellen-ID** und **Datenquellennamen** in Bing Maps-API Datenquellen enthalten verschiedene bucketed Metadaten, wie Standorte und Geschäftszeiten. Erfahren Sie mehr über die hier. 
* **Entitätsname** der Entität als Bezugspunkt für die Benachrichtigung verwenden möchten. 
* **Bing Maps-API-Schlüssel** – Dies ist der Schlüssel, die Sie beim Erstellen des Kontos Bing-Entwicklungscenter zuvor ermittelt.
 
Lassen Sie uns eine tief greifende zur Einrichtung für jeden der oben genannten Elemente.

##<a name="setting-up-the-data-source"></a>Einrichten der Datenquelle

Es ist in die Bing Maps-Entwicklungscenter möglich. Einfach klicken Sie in der oberen Navigationsleiste auf **Datenquellen** , und wählen Sie **Datenquellen verwalten**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Wenn Bing Maps-API vor nicht gearbeitet haben, wahrscheinlich werden es keine Datenquellen vorhanden, nur Upload-Daten an eine Datenquelle auf eine neue erstellen. Stellen Sie sicher, dass Sie alle erforderlichen Felder ausfüllen:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Sie werden vielleicht – wie die Datendatei ist und was Sie hochladen werden sollte? Für diese Prüfung können wir nur Pipe-basierte Sample, die einen Bereich von San Francisco Wasser frames:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
Diese darstellt Entität:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Einfach kopieren oben in eine neue Datei einfügen und als **NotificationHubsGeofence.pipe**speichern und Hochladen in Bing Developer Center.

>[AZURE.NOTE]Sie möglicherweise aufgefordert, einen neuen Schlüssel für den **Hauptschlüssel** anzugeben, mit dem **Abfrage-Schlüssel**ist. Einfach einen neuen Schlüssel über das Dashboard erstellen und Aktualisieren der Quellseite Upload Daten.

Nachdem Sie die Datei hochladen, müssen Sie sicherstellen, dass die Datenquelle veröffentlichen. 

**Datenquellen verwalten**, gehen Sie wie oben, finden Sie in der Liste Datenquelle, und klicken Sie in der Spalte **Aktionen** auf **Veröffentlichen** . In, erhalten die Datenquelle die Registerkarte **Veröffentlichen-Datenquellen** Sie:

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Wenn Sie auf **Bearbeiten**klicken, werden Sie auf einen Blick wo sehen wir eingeführt:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Zu diesem Zeitpunkt zeigt das Portal Sie nicht die Grenzen für den Geofence wir erstellt haben, müssen wir lediglich eine Bestätigung, dass die angegebenen Position in der richtigen Umgebung.

Jetzt haben Sie alle Vorschriften für die Datenquelle. Um Details auf Anfrage-URL für den API-Aufruf in Bing Maps Developer Center, auf **Daten** und wählen Sie **Informationen zur Datenquelle**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

**Abfrage-URL** sind wir hier. Dies ist der Endpunkt für den wir Abfragen, um zu überprüfen, ob das Gerät derzeit innerhalb eines Standorts oder nicht ausführen kann. Diese Überprüfung muss einfach führen Sie einen GET-Aufruf für die Abfrage-URL mit den folgenden Parametern angefügt:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Zeigen Sie so ein Ziel angegeben haben, die wir mit dem Gerät erhalten und Bing Maps führt automatisch die Berechnung, ob innerhalb der Geofence ist. Nach der Ausführung der Anforderung über einen Browser (oder Aufrollen) erhalten standard JSON-Antwort Sie:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Diese Antwort geschieht nur, wenn der Punkt innerhalb der festgelegten Grenzen handelt. Wenn dies nicht der Fall ist, erhalten Sie eine leere **Ergebnisse** Bucket:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##<a name="setting-up-the-uwp-application"></a>Einrichten der Anwendung UWP

Jetzt haben wir die Datenquelle jetzt können wir beginnen die UWP-Anwendung, die wir zuvor neu gestartet.

Zunächst müssen wir für unsere Anwendung Diensten ermöglichen. Doppelklicken Sie hierzu auf `Package.appxmanifest` Datei im **Projektmappen-Explorer**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Klicken Sie im Register Eigenschaften Paket nur geöffnet auf **Funktionen** und sicherstellen Sie, dass Sie **Speicherort**auswählen:

![](./media/notification-hubs-geofence/vs-package-location.png)

Position-Funktion deklariert, erstellen Sie einen neuen Ordner in der Projektmappe mit dem Namen `Core`, und fügen Sie eine neue Datei mit dem Namen `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

Die `LocationHelper` Klasse ist ziemlich einfach – es lediglich den Pfad durch das System-API zu ermöglichen:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Lesen Sie mehr zu den Speicherort im UWP-apps in [Dateien](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

Öffnen Sie zum Überprüfen, mit dem Speicherort funktioniert der Seite Code Ihre wichtigsten (`MainPage.xaml.cs`). Erstellen Sie einen neuen Ereignishandler für das `Loaded` Ereignis in der `MainPage` Konstruktor:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Die Implementierung der Ereignishandler lautet wie folgt:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Beachten Sie, dass den Handler als asynchrone da deklariert `GetCurrentLocation` "awaitable", und in einem Kontext asynchron ausgeführt werden. Da unter Umständen wir mit einer null am Ende könnte (z. B. Speicherort deaktiviert sind oder die Anwendung Berechtigungen an Zugriff verweigert), müssen wir auch sicherstellen, dass es ordnungsgemäß mit einer null behandelt wird.

Führen Sie die Anwendung. Stellen Sie sicher, dass Speicherort zugreifen:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Einmal sollte die Anwendung startet Sie die Koordinaten im **Ausgabefenster** können:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Jetzt wissen Sie sich Speicherort Übernahme Works-Test-Ereignishandler Loaded entfernen, da wir es nicht mehr verwenden.

Der nächste Schritt ist Speicherort erfassen. Gehen wir zurück zu den `LocationHelper` Klasse, und fügen Sie Ereignishandler für `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Die Implementierung wird die Koordinaten im Fenster **Ausgabe** angezeigt:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##<a name="setting-up-the-backend"></a>Einrichten der Back-End

Das [Back-End-Beispiel für .NET von GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers)herunterladen. Sobald der Download abgeschlossen ist, öffnen Sie die `NotifyUsers` Ordner – und die `NotifyUsers.sln` Datei.

Legen Sie die `AppBackend` Projekt als **Startprojekt** und starten.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Das Projekt ist bereits für Pushbenachrichtigungen an Zielgeräte senden konfiguriert, wir müssen nur zwei Dinge tun-swap die richtige Verbindung Zeichenfolge für den benachrichtigungshub und Grenze Kennung zum Senden der Benachrichtigung nur wenn der Benutzer in der Geofence hinzufügen.

So konfigurieren Sie die Verbindungszeichenfolge in der `Models` Ordner öffnen `Notifications.cs`. Die `NotificationHubClient.CreateClientFromConnectionString` Funktion enthält Informationen über die benachrichtigungshub, der in [Azure-Portal](https://portal.azure.com) (Einblick in das Blade **-Richtlinien** **Einstellungen**) abrufen. Speichern Sie die aktualisierte Konfigurationsdatei.

Nun müssen wir ein Modell dafür Bing Maps-API erstellen. Die einfachste Möglichkeit dazu ist mit der rechten Maustaste die `Models` Ordner **Hinzufügen** > **Klasse**. Nennen Sie sie `GeofenceBoundary.cs`. Danach kopieren JSON API-Antwort, die wir im ersten Abschnitt und in Visual Studio verwenden **Bearbeiten**erläutert > **Einfügen** > **JSON als Klassen einfügen**. 

Auf diese Weise stellen wir sicher, dass das Objekt genau wie deserialisiert werden sollte. Die Ergebnismenge Klasse sollte so aussehen:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Öffnen Sie dann `Controllers`  >  `NotificationsController.cs`. Wir müssen Post-Aufruf auf das Ziel Längen- und optimieren. Fügen zwei Zeichenfolgen zur Funktionssignatur – `latitude` und `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Erstellen Sie eine neue Klasse im Projekt aufgerufen `ApiHelper.cs` – verwenden wir diese Verbindung mit Bing überprüfen Grenze Schnittpunkte zeigen. Implementieren einer `IsPointWithinBounds` Funktion folgendermaßen:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Stellen Sie sicher, den API-Endpunkt mit dem Abfrage-URL ersetzt, die Sie zuvor von Bing-Entwicklungscenter erhalten (gilt auch für den API-Schlüssel). 

Sind die Ergebnisse der Abfrage, das bedeutet, dass der angegebene Punkt innerhalb der Grenzen des Geofence, wir zurückgeben `true`. Wenn keine Ergebnisse vorliegen, Bing sagt uns, dass der Punkt außerhalb des Rahmens suchen wir zurückgeben `false`.

In `NotificationsController.cs`, eine Prüfung vor der Switch-Anweisung erstellen:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Auf diese Weise die Benachrichtigung wird nur gesendet, wenn der Punkt innerhalb der Grenzen befindet.

##<a name="testing-push-notifications-in-the-uwp-app"></a>Push-Benachrichtigung in der UWP-Anwendung testen

Zurück an die Anwendung UWP, sollten wir jetzt Benachrichtigungen testen können. In der `LocationHelper` Klasse, erstellen Sie eine neue Funktion – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Tauschen Sie die `POST_URL` an der bereitgestellten Anwendung, die wir im vorherigen Abschnitt erstellt haben. Jetzt können lokal ausführen, aber Arbeit auf Öffentliche Version bereitstellen, muss mit einem externen Provider gehostet.

Nun stellen wir sicher, dass wir die UWP app für Pushbenachrichtigungen registrieren. Klicken Sie in Visual Studio auf **Projekt** > **Speichern** > **app mit dem Informationsspeicher zuordnen**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Wenn Sie Entwickler-Konto anmelden, stellen Sie sicher, wählen Sie eine vorhandene Anwendung oder eine neue zu erstellen und das Paket zuordnen. 

Besuchen Sie das Developer Center, und öffnen Sie die Anwendung, die Sie gerade erstellt haben. Klicken Sie auf **Dienste** > **Pushbenachrichtigungen** > **Live Services-Website**.

![](./media/notification-hubs-geofence/ms-live-services.png)

Beachten Sie auf der Website den **Geheimen Schlüssel** und die **Paket-SID**. Müssen Sie in Azure-Portal – den benachrichtigungshub öffnen, klicken Sie **auf** > **Notification Services** > **Windows (WNS)** und geben die Informationen in die erforderlichen Felder.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Klicken Sie auf **Speichern**.

Klicken Sie mit der rechten Maustaste auf **Verweise** im **Projektmappen-Explorer** , und wählen Sie **NuGet-Pakete verwalten**. Wir benötigen einen Verweis auf die **Microsoft Azure Service Bus verwaltete Bibliothek** hinzufügen – einfach `WindowsAzure.Messaging.Managed` und dem Projekt hinzugefügt.

![](./media/notification-hubs-geofence/vs-nuget.png)

Zu Testzwecken erstellen wir die `MainPage_Loaded` -Ereignishandler erneut und dieser Codeausschnitt hinzuzufügen:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Diese registriert app benachrichtigungshub. Sie sind bereit! 

In `LocationHelper`, in der `Geolocator_PositionChanged` können fügen Sie Ereignishandler ein Codeabschnitts Test, der die Position im Geofence erzwungen wird:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Da wir nicht die realen Koordinaten (der Rahmen derzeit möglicherweise nicht) übergeben und vordefinierte Werte verwenden, sehen wir eine Benachrichtigung auf Update angezeigt:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##<a name="whats-next"></a>Was kommt als nächstes?

Es gibt einige Schritte, die Sie zusätzlich sicherstellen, dass die Lösung Produktion folgen müssen.

Zunächst müssen Sie sicherstellen, dass Geofences dynamisch sind. Dies erfordert einige zusätzliche Arbeit mit Bing-API, um neue Grenzen innerhalb der vorhandenen Datenquelle hochladen können. Weitere Informationen zu diesem Thema finden Sie in der [Bing räumliche Data Services-API-Dokumentation](https://msdn.microsoft.com/library/ff701734.aspx) .

Zweitens sollten arbeiten um sicherzustellen, dass die Übermittlung an die richtigen Teilnehmer erfolgt, Sie sie über [tagging](notification-hubs-tags-segment-push-message.md)Ziel.

Oben gezeigte Lösung beschreibt ein Szenario, in dem möglicherweise eine Vielzahl von Zielplattformen, damit wir Geofencing-spezifische Funktionen nicht eingeschränkt. Andererseits die universelle Windows-Plattform erkennen [Geofences rechts von der-vordefinierten](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence)bietet.

Checken Sie weitere Einzelheiten zur Benachrichtigungshubs Funktionen unserer [dokumentationsportal](https://azure.microsoft.com/documentation/services/notification-hubs/).
