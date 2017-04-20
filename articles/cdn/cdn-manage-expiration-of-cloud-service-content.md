<properties
 pageTitle="Ablauf von Azure Web Apps-Cloud Services, ASP.NET und IIS in Azure CDN verwalten | Microsoft Azure"
 description="Beschreibt, wie der Cloud Service in Azure CDN Inhaltsablauf verwalten"
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="09/19/2016"
 ms.author="casoper"/>

# <a name="how-to-manage-expiration-of-azure-web-appscloud-services-aspnet-or-iis-content-in-azure-cdn"></a>Ablauf von Azure Web Apps-Cloud Services, ASP.NET oder IIS in Azure CDN verwalten

> [AZURE.SELECTOR]
- [Azure Web Apps-Cloud Services, ASP.NET und IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure Blob-Speicherdienst](cdn-manage-expiration-of-blob-content.md)

Dateien von einem öffentlich zugänglichen Ursprung Webserver können in Azure CDN zwischengespeichert werden, bis zum Ablauf der Time-to-live (TTL).  Die TTL bestimmt die [ *Cache-Control* -Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in der HTTP-Antwort vom Ursprungsserver.  Dieser Artikel beschreibt, wie `Cache-Control` Header für Azure Web Apps, Azure Cloud Services, ASP.NET Applications und IIS-Websites die entsprechend konfiguriert sind.

>[AZURE.TIP] Sie können keine Gültigkeitsdauer für eine Datei festgelegt.  In diesem Fall wendet Azure CDN automatisch eine Standardgültigkeitsdauer von sieben Tagen.
>
>Weitere Informationen zur Funktionsweise von Azure CDN beschleunigen des Zugriffs auf Dateien und andere Ressourcen finden Sie unter [Übersicht über Azure CDN](./cdn-overview.md).

## <a name="setting-cache-control-headers-in-configuration"></a>Cache-Control-Header festlegen in der Konfiguration

Für statische Inhalte wie Bilder und Stylesheets steuern Sie die Aktualisierungsrate der **applicationHost.config** oder der Datei **web.config** für Ihre Webanwendung ändern.  Das **system.webServer\staticContent\clientCache** -Element in der Konfigurationsdatei Festlegen der `Cache-Control` Header für den Inhalt. Für **web.config**wirkt Konfigurationsinformationen alles im Ordner und alle Unterordner sich nicht auf Unterordner überschrieben.  Z. B. Voreinstellung ein TTL Stammverzeichnis alle statischen Inhalt zwischengespeichert 3 Tage können Unterordner, die mehr mit einem Cache von 6 Stunden Inhalte.  Für **applicationHost.config**Anträge auf der Website beeinträchtigt jedoch können in **web.config** -Dateien in den überschrieben werden.

Die folgenden XML-zeigt und Beispiel Einstellung **ClientCache** ein maximales Alter von 3 Tagen an:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

Angeben **UseMaxAge** Fügt eine `Cache-Control: max-age=<nnn>` Header der Antwort basierend auf dem Wert im **CacheControlMaxAge** -Attribut angegeben. Das Format der erstellten ist das **CacheControlMaxAge** Attribut <days>. <hours>:<min>:<sec>. Weitere Informationen zu dem **ClientCache** Knoten finden Sie unter [Clientcache <clientCache> ](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-in-code"></a>Cache-Control-Header festlegen im Code

Legen Sie für die ASP.NET Applications CDN Cacheverhalten programmgesteuert durch Festlegen der Eigenschaft **HttpResponse.Cache** . Weitere Informationen über die **HttpResponse.Cache** -Eigenschaft finden Sie unter [HttpResponse.Cache Eigenschaft](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) und [HttpCachePolicy-Klasse](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  

Wenn Sie programmgesteuert Anwendungsinhalt in ASP.NET cache, stellen Sie sicher, dass der Inhalt als zwischenspeicherbar gekennzeichnet, indem HttpCacheability fest auf *Öffentliche*möchten. Außerdem sicherstellen Sie, dass ein Cache Validierungssteuerelement festgelegt ist. Cache-Bestätigung kann eine letzte Änderung Zeitstempel durch Aufrufen von SetLastModified oder einen Etag-Wert durch Aufrufen von SetETag festlegen. Optional, Sie können auch eine Cache-Ablaufzeit durch Aufrufen von SetExpires oder weiter oben in diesem Dokument beschriebenen Standard Cache Heuristik abhängig.  

Cache für eine Stunde, fügen Sie beispielsweise Folgendes ein:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="next-steps"></a>Nächste Schritte

- [Details zu den **ClientCache** element](http://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
- [Lesen Sie die Dokumentation für die **HttpResponse.Cache** -Eigenschaft](http://msdn.microsoft.com/library/system.web.httpresponse.cache.aspx) 
- [Lesen Sie die Dokumentation zur **HttpCachePolicy-Klasse**](http://msdn.microsoft.com/library/system.web.httpcachepolicy.aspx).  
