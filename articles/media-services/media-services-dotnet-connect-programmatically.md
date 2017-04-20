<properties 
    pageTitle="Herstellen einer Verbindung mit Media Services-Konto mit .NET" 
    description="In diesem Thema veranschaulicht, wie Media Services Einzelhandelsabonnements .NET herstellen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Herstellen einer Verbindung mit Media Services-Konto mit Media Services SDK für .NET

> [AZURE.SELECTOR]
- [REST](media-services-rest-connect-programmatically.md)
- [.NET](media-services-dotnet-connect-programmatically.md)


So erhalten Sie eine programmgesteuerte Verbindung zu Microsoft Azure Media Services bei der Programmierung mit Media Services SDK für .NET beschrieben.


## <a name="connecting-to-media-services"></a>Herstellen einer Verbindung mit Media Services

Programmgesteuerte Verbindung mit Media Services muss zuvor ein Azure-Konto eingerichtet, Media-Dienste auf diesem Konto konfiguriert, und legen Sie ein Visual Studio-Projekt für die Entwicklung mit Media Services SDK für .NET. Weitere Informationen finden Sie unter Setup für die Entwicklung mit Media Services SDK für .NET.

Am Ende des Media Services Account Setup erhalten Sie folgende erforderliche Verbindung. Diese Media Services programmgesteuerte Verbindung zu verwenden.

- Media Services Kontonamen.

- Der Media Services Account-Schlüssel.

Um diese Werte zu finden, zum Azure Management Portal wählen Sie Ihre Media-Dienstkonto und klicken Sie auf das Symbol "**Schlüssel verwalten**" auf der Unterseite des Fensters Portal. Klicken auf das Symbol neben jedem Textfeld kopiert den Wert in die Zwischenablage.


## <a name="creating-a-cloudmediacontext-instance"></a>Erstellen einer CloudMediaContext-Instanz

Starten von Media Services programmieren müssen Sie **CloudMediaContext** -Instanz erstellen, die den Server darstellt. Das **CloudMediaContext** enthält Verweise auf Projekte, Ressourcen, Dateien, Richtlinien und Locators einschließlich Sammlungen.

>[AZURE.NOTE] Die **CloudMediaContext** -Klasse ist nicht threadsicher. Erstellen Sie eine neue CloudMediaContext pro Thread oder Operationen.


CloudMediaContext hat fünf Konstruktorüberladungen. Es wird empfohlen, Konstruktoren verwenden, die **MediaServicesCredentials** als Parameter akzeptieren. Weitere Informationen finden Sie unter **Wiederverwenden Control Service Zugriffstoken** , das folgt. 

Im folgende Beispiel verwendet den öffentlichen CloudMediaContext(MediaServicesCredentials credentials)-Konstruktor:

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Access Control Service Token wiederverwenden

Dieser Abschnitt veranschaulicht die Access Control Service Token mithilfe CloudMediaContext Konstruktoren, die MediaServicesCredentials als Parameter wieder.


[Azure Active Directory Access Control](https://msdn.microsoft.com/library/hh147631.aspx) (auch bekannt als Access Control Service oder ACS) ist ein Cloud-basierten Dienst, der leicht zu authentifizieren und Autorisieren von Benutzern Zugriff auf ihre ASP.NET-Webanwendungen bereitstellt. Microsoft Azure Media Services steuert den Zugriff auf die Dienste jedoch OAuth-Protokoll, das ACS-Token erfordert. Media Services empfängt eine autorisierungsserver ACS-Tokens.

Bei der Entwicklung mit Media Services SDK können Sie Token nicht behandelt, da das SDK code Manager für Sie. Führt jedoch zu unnötigen token Anfragen SDK ACS-Tokens vollständig verwalten lassen. Anfordern von Tokens Zeit und Client und Server verbraucht. Der ACS-Server Steuerung auch Anfragen ist zu hoch. Beträgt 30 Anforderungen pro Sekunde, Näheres [ACS Dienst eingeschränkt](https://msdn.microsoft.com/library/gg185909.aspx) .

Media Services SDK Version 3.0.0.0 ab, können Sie die ACS-Token verwenden. Die **CloudMediaContext** Konstruktoren, die **MediaServicesCredentials** als Parameter ACS-Tokens zwischen mehreren Kontexten freigeben. MediaServicesCredentials-Klasse kapselt die Anmeldeinformationen Media Services. Wenn ein ACS-Token verfügbar ist und die Ablaufzeit bezeichnet, können Erstellen einer neuen MediaServicesCredentials-Instanz mit dem Token zu CloudMediaContext-Konstruktor übergeben. Beachten Sie, dass Media Services SDK Token automatisch aktualisiert, wenn sie ablaufen. Zweierlei Wiederverwenden von ACS-Tokens, wie in den folgenden Beispielen gezeigt.

- Sie können das **MediaServicesCredentials** -Objekt im Arbeitsspeicher (z. B. in einer statischen Klassenvariablen) zwischenspeichern. Übergeben Sie das zwischengespeicherte Objekt dann an den CloudMediaContext-Konstruktor. Das MediaServicesCredentials-Objekt enthält einen ACS-Token, der wiederverwendet werden kann, wenn es noch gültig ist. Wenn das Token nicht gültig ist, wird von Media Services SDK mit den MediaServicesCredentials-Konstruktor angegebenen Anmeldeinformationen aktualisiert werden.

    Beachten Sie, dass das Objekt **MediaServicesCredentials** ein gültiges Token wird nach der RefreshToken aufgerufen wird. Das **CloudMediaContext** Ruft die **RefreshToken** -Methode im Konstruktor. Falls Sie ein externes Speichergerät token Werte speichern, müssen Sie überprüfen, ob der TokenExpiration-Wert gültig ist, bevor das token speichern. Wenn er nicht gültig ist, rufen Sie RefreshToken vor dem Zwischenspeichern.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- Sie können auch die Zeichenfolge Zugriffstoken und TokenExpiration Werte Zwischenspeichern. Werte können später verwendet werden, ein neues MediaServicesCredentials-Objekt mit zwischengespeicherten token erstellt.  Dies ist besonders für Szenarien, in denen das Token sicher zwischen mehreren Prozessen oder Computern gemeinsam genutzt werden kann.

    Die folgenden Codeausschnitte rufen die Methoden SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage und UpdateTokenDataInExternalStorageIfNeeded, die nicht in diesem Beispiel definiert. Sie können diese Methoden zum Speichern, abrufen und Aktualisieren von token Daten in einem externen definieren. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    Verwenden der gespeicherten Tokenwerte MediaServicesCredentials erstellen.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Aktualisieren der token Kopie bei Token von Media Services SDK aktualisiert wurde. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Haben Sie mehrere Medien Dienstkonten (z. B. für Load sharing Zwecke oder Geo-Verteilung) können Sie MediaServicesCredentials Objekte mithilfe der System.Collections.Concurrent.ConcurrentDictionary-Auflistung (die Auflistung ConcurrentDictionary stellt eine threadsichere Auflistung von Schlüssel-Wert-Paare, die von mehreren Threads gleichzeitig zugegriffen werden kann) zwischenspeichern. GetOrAdd-Methode können Sie die zwischengespeicherten Anmeldeinformationen erhalten. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Herstellen einer Verbindung mit Media Services-Konto der Region Nord-China

Wenn Ihr Konto im Norden China befindet, verwenden Sie den folgenden Konstruktor:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Zum Beispiel:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Verbindung Steuerungswerte Konfiguration

Es wird dringend empfohlen, Verbindung Werte, besonders sensible wie Kontoname und Kennwort Konfiguration speichern. Außerdem wird empfohlen, vertrauliche Daten zu verschlüsseln. Sie können die gesamte Konfigurationsdatei verwenden das Windows Encrypting File System (EFS) verschlüsseln. EFS auf eine Datei, mit der rechten Maustaste der Datei, wählen **Eigenschaften**, und aktivieren Sie die Verschlüsselung in **der Registerkarte Einstellungen** . Oder erstellen Sie eine benutzerdefinierte Lösung für ausgewählte Teile einer Konfigurationsdatei mithilfe der geschützten Konfiguration verschlüsseln. Finden Sie unter [Encrypting Configuration Information Using Protected Configuration](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Die folgende Datei App.config enthält der erforderlichen Werte. Die Werte in der <appSettings> sind die erforderlichen Werte von Media Services Account Setup-Prozess hat.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


Um Verbindung von Konfiguration abzurufen, kann die **ConfigurationManager** -Klasse verwenden und weisen Sie die Werte für Felder im Code:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
