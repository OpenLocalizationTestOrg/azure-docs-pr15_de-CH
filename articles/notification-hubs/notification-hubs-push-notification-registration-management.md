<properties
    pageTitle="Registrierung-Management"
    description="Dieses Thema erläutert die Geräte mit Benachrichtigung registrieren, um Push-Benachrichtigung zu erhalten."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="registration-management"></a>Registrierung-management

##<a name="overview"></a>Übersicht

Dieses Thema erläutert die Geräte mit Benachrichtigung registrieren, um Push-Benachrichtigung zu erhalten. Thema beschreibt die Registrierung auf hoher Ebene und führt zwei Grundprinzipien für Geräte registrieren: vom Gerät direkt an den benachrichtigungshub registriert und über eine Backend Anwendung registrieren. 


##<a name="what-is-device-registration"></a>Was ist die geräteregistrierung

Registrierung des Geräts mit einer Benachrichtigung erfolgt über eine **Registrierung** oder **Installation**.

#### <a name="registrations"></a>Registrierung
Registrierung ein Tags und möglicherweise eine Vorlage Plattform Notification Service (PNS)-Handle für ein Gerät zugeordnet. Das PNS-Handle möglicherweise ein ChannelURI gerätetoken oder GCM-Identifikationsnummer. Tags dienen zum Weiterleiten von Benachrichtigungen mit den richtigen Gerät behandelt. Weitere Informationen finden Sie unter [Routing und Tag](notification-hubs-tags-segment-push-message.md). Vorlagen dienen zum pro Registrierung Transformation implementieren. Weitere Informationen finden Sie unter [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Installationen
Eine Installation ist eine Anmeldung, die eine Sammlung von Push bezogenen Eigenschaften. Es ist die neueste und beste Ansatz zum Registrieren Ihrer Geräte. Jedoch wird nicht von clientseitigen .NET SDK ([Notification Hub SDK für Back-End-Vorgänge](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) noch unterstützt.  Dies bedeutet, dass bei der Anmeldung aus dem Client-Gerät Sie würde [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx) -Ansatz verwenden, um Installationen unterstützt. Wenn Sie einen Back-End-Dienst verwenden, sollten Sie [Notification Hub SDK für Back-End-Vorgänge](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)verwenden können.

Es folgen einige wichtige Vorteile Installationen:

* Erstellen oder Aktualisieren einer Installation ist vollständig Idempotent. Sie können es ohne Rücksicht auf doppelte Registrierung wiederholen.
* Installation-Modell erleichtert die einzelnen Push - für bestimmte Geräte führen. Ein System-Tag **"$InstallationId: [InstallationId]"** automatisch mit jeder Installation Grundlage der Erfassung. So können Sie dieses Tag auf einem bestimmten Gerät ohne zusätzliche Programmierung des Empfängers aufrufen.
* Mit Installationen kann auch teilweise Anmeldung Updates ausführen. Partielle Aktualisierung einer Installation wird mit PATCH-Methode mithilfe des [JSON-Patch Standard](https://tools.ietf.org/html/rfc6902)angefordert. Dies ist besonders nützlich, wenn auf die Registrierung aktualisieren. Sie haben nicht die gesamte Registrierung und senden alle vorherigen Tags wieder.

Eine Installation darf die folgenden Eigenschaften. Eine vollständige Liste der Installation Eigenschaften finden Sie unter [Erstellen oder überschreiben eine Installation mit REST-API](https://msdn.microsoft.com/library/azure/mt621153.aspx) oder die [Eigenschaften](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) der.

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }

 

Es ist wichtig zu beachten, dass die Registrierung und Installationen standardmäßig nicht ablaufen.

Registrierung und Installationen muss ein gültiges PNS-Handle für jede Gerätekanal. Da Handles PNS nur in einer Clientanwendung auf dem Gerät abgerufen werden können, ist ein Muster direkt auf dem Gerät mit der Clientanwendung registrieren. Auf der anderen Seite erfordern Sicherheitsaspekte und Geschäftslogik Zusammenhang mit Tags Gerät Eintragung in der app Backend-Verwaltung. 

#### <a name="templates"></a>Vorlagen

[Vorlagen](notification-hubs-templates-cross-platform-push-messages.md), die Geräteinstallation auch halten alle Vorlagen in eine JSON Gerät gehörenden (siehe Beispiel oben). Die Namen können Ziel unterschiedliche Vorlagen für dieses Gerät.

Beachten Sie, dass jeden Vorlagennamen Vorlage Text und einen optionalen Satz von Tags zugeordnet ist. Darüber hinaus kann jede Plattform zusätzliche Eigenschaften haben. (Verwendung von WNS) Windows Store und Windows Phone 8 (mit MPNS) kann ein zusätzlicher Satz von Headern Teil der Vorlage. Bei APN können Sie entweder eine Konstante oder einen Vorlagenausdruck Ablauf-Eigenschaft festlegen. Eine vollständige Liste der Installation finden Eigenschaften, [Erstellen oder eine Installation mit überschreiben](https://msdn.microsoft.com/library/azure/mt621153.aspx) .

#### <a name="secondary-tiles-for-windows-store-apps"></a>Sekundäre Kacheln für Windows Store-Apps

Windows Store-Clientanwendungen entspricht Senden von Benachrichtigungen, sekundäre Kacheln als primäre zu senden. Dies ist auch bei Installationen unterstützt. Beachten Sie, dass sekundäre Kacheln unterschiedliche ChannelUri der SDK auf Ihre Clientanwendung transparent behandelt.

SecondaryTiles Wörterbuch verwendet die gleichen TileId, die zum Erstellen des SecondaryTiles-Objekts in Windows Store-Apps verwendet wird.
ChannelUris der sekundäre Kacheln können mit primären ChannelUri jederzeit ändern. Um die Anlagen im Notification Hub aktualisiert zu halten, muss das Gerät mit den aktuellen ChannelUris der sekundären Kacheln aktualisiert werden.


##<a name="registration-management-from-the-device"></a>Registrierungsverwaltung vom Gerät

Verwaltung geräteregistrierung von Clientanwendungen ist die Back-End-nur Benachrichtigungen senden. Clientanwendungen halten von PNS behandelt und Tags registrieren. Die folgende Abbildung veranschaulicht dieses Muster.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Das Gerät zunächst ruft PNS-Handle aus PNS und direkt mit dem benachrichtigungshub registriert. Wenn die Registrierung abgeschlossen ist, kann Anwendung Back-End-Registrierung auf benachrichtigen. Weitere Informationen zu Benachrichtigungen finden Sie unter [Routing und Tag](notification-hubs-tags-segment-push-message.md).
Hinweis in diesem Fall verwendet nur Zugriffsrechte für die benachrichtigungshubs vom Gerät empfangen. Weitere Informationen finden Sie unter [Sicherheit](notification-hubs-push-notification-security.md).

Die einfachste Methode ist die Registrierung vom Gerät hat einige Nachteile.
Der erste Nachteil ist, dass eine Clientanwendung nur die Tags aktualisieren kann, wenn die Anwendung aktiv ist. Beispielsweise verfügt ein Benutzer zwei Geräte, die registrieren Kategorien, Sportteams, wenn das erste Gerät für einen zusätzlichen Tag (z. B. Seahawks) registriert, erhalten das zweite Gerät keine Benachrichtigung über die Seahawks bis die Anwendung auf dem zweiten Gerät ein zweites Mal ausgeführt wird. Im Allgemeinen wird Tags von mehreren Geräten betroffen sind, Tags aus der Back-End-Management eine wünschenswerte Option.
Der zweite Nachteil Registrierung Management von der Clientanwendung wird verlangt, da apps geknackt werden können, sichern die Registrierung bestimmter Tags besonders vorsichtig, wie im Abschnitt "Tag-Ebene Sicherheit".



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Beispielcode mit einer Benachrichtigung von einem Gerät eine Installation registrieren 

Derzeit wird dies nur unterstützt mit [Notification Hubs REST-API](https://msdn.microsoft.com/library/mt621153.aspx).

Sie können auch mit [JSON Patch - Standard](https://tools.ietf.org/html/rfc6902) für die Installation aktualisiert PATCH-Methode.

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }

   

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Beispielcode mit einer Benachrichtigung von einem Gerät eine Registrierung registrieren


Diese Methoden erstellen oder aktualisieren eine Registrierung für das Gerät, auf dem sie aufgerufen werden. Dies bedeutet, dass um den Griff oder die Tags aktualisieren die gesamte Registrierung überschrieben werden muss. Beachten Sie, dass Registrierung vorübergehend, damit immer einen zuverlässigen Speicher mit aktuellen Tags haben soll, die ein bestimmtes Gerät benötigt.


    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }
     
    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Management von Backend-Registrierung

Registrierung von Backend Verwaltung erfordert zusätzlichen Code. Die app vom Gerät muss aktualisierte PNS behandeln, Backend bei jedem Starten der Anwendung (mit Tags und Vorlagen) und Backend muss dieses Handle am benachrichtigungshub aktualisieren angeben. Die folgende Abbildung veranschaulicht dieses Design.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Registrierung von Backend verwalten Vorteile die Möglichkeit, Tags Registrierung ändern, selbst wenn die entsprechende Anwendung auf dem Gerät inaktiv ist und die Clientanwendung authentifiziert, bevor er die Registrierung einen Tag hinzufügen.


#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Beispielcode mit einer Benachrichtigung von Backend-eine Installation registrieren

Das Client-Gerät noch Ruft die PNS-Handle und relevante Eigenschaften wie vor und ruft eine benutzerdefinierte API im Back-End, die die Registrierung durchführen und Tags usw. autorisieren können. Backend kann [Notification Hub SDK für Back-End-Operationen](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)nutzen.

Sie können auch mit [JSON Patch - Standard](https://tools.ietf.org/html/rfc6902) für die Installation aktualisiert PATCH-Methode.
 

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Beispielcode mit einer Benachrichtigung von einem Gerät eine Identifikationsnummer registrieren

Ihre app-Back-End können Sie grundlegende CRUDS Vorgänge für Registrierung ausführen. Zum Beispiel:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");
            
    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


Backend muss zwischen Registrierung Parallelität. Service Bus bietet vollständige Parallelität für die registrierungsverwaltung. Auf HTTP-Ebene wird dies mit der ETag auf Registrierung Management implementiert. Dieses Feature wird transparent von Microsoft SDKs verwendet, die eine Ausnahme aus, wenn ein Update Parallelität Gründen abgelehnt wird. App Backend ist verantwortlich für die Ausnahmebehandlung und Update ggf. wiederholen.