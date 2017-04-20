<properties 
    pageTitle="Sitzungsstatus bei Azure Redis Cache in Azure App Service" 
    description="Erfahren Sie, wie mit dem Cachedienst Azure ASP.NET Session State Zwischenspeichern unterstützen." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="none"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="06/27/2016" 
    ms.author="riande"/>


# <a name="session-state-with-azure-redis-cache-in-azure-app-service"></a>Sitzungsstatus bei Azure Redis Cache in Azure App Service


Dieses Thema erläutert die Azure Redis Cache-Dienst für den Sitzungszustand verwenden.

Wenn Ihrer Anwendung ASP.NET den Sitzungszustand verwendet, müssen Sie eine externe Sitzungszustandsanbieter (Redis Cache Service oder eines SQL Server-Anbieters) konfigurieren. Wenn Sie den Sitzungszustand verwenden und einen externen Provider nicht verwenden, können Sie eine Instanz Ihrer Anwendung stehen. Redis Cache Service ist am schnellsten und einfachsten zu aktivieren.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>Cache erstellen
Folgen Sie [dieser Anleitung](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) Cache erstellen.

##<a id="configureproject"></a>Ihrer Anwendung RedisSessionStateProvider NuGet-Paket hinzufügen
Installieren Sie die NuGet `RedisSessionStateProvider` Paket.  Verwenden Sie den folgenden Befehl von der Paket-Manager-Konsole installieren (**Extras** > **NuGet Paket-Manager** > **Paket-Manager Konsole**):

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
Installieren von **Tools** > **NuGet Paket-Manager** > **NugGet Pakete Lösung verwalten**, Suchen nach `RedisSessionStateProvider`.

Weitere Informationen finden Sie unter [NuGet RedisSessionStateProvider Seite](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) und [Cache-Client konfigurieren](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet).

##<a id="configurewebconfig"></a>Ändern der Datei Web.Config
NuGet-Paket fügt neben Assemblyverweise Cache, Stub-Einträge in der Datei *web.config* . 

1. Öffnen Sie *web.config* und Suchen der **SessionState** Element.

1. Geben Sie die Werte für die `host`, `accessKey`, `port` (SSL-Anschluss sollte 6380), und `SSL` , `true`. Diese Werte können aus dem [Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=529715) Blade für die Cacheinstanz abgerufen werden. Weitere Informationen finden Sie unter [verbinden im Cache](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache). Beachten Sie, dass nicht-SSL-Anschluss für neue Zwischenspeicher standardmäßig deaktiviert ist. Weitere Informationen zu nicht-SSL-Anschluss finden Sie im Abschnitt [Zugriff auf Ports](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) im Thema [Cache in Azure Redis Cache konfigurieren](https://msdn.microsoft.com/library/azure/dn793612.aspx) . Das folgende Markup zeigt an, wie der Datei *web.config* speziell ändert, *Port*, *Host*AccessKey*, und *Ssl *.

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a>Verwenden Sie das Sitzungsobjekt in Code
Der letzte Schritt soll mit dem Sitzungsobjekt in ASP.NET Code. Objekte werden den Sitzungszustand mithilfe der **Session.Add** -Methode hinzufügen. Diese Methode verwendet die Schlüssel-Wert-Paare Elemente im Sitzungscache Zustand speichern.

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

Der folgende Code ruft diesen Wert aus dem Sitzungszustand.

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Redis Cache Cacheobjekte verwenden Sie in Ihrer Anwendung. Weitere Informationen finden Sie unter [MVC Film app Azure Redis Cache in 15 Minuten](https://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/).
Weitere Informationen zur Verwendung von ASP.NET Sitzungszustand finden Sie unter [ASP.NET Session State Overview][].

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

  *[Rick](https://twitter.com/RickAndMSFT) Anderson*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [ASP.NET Session State Overview]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 
