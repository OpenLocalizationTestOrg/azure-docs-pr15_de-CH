<properties
    pageTitle="CDN Cachingrichtlinie Media Services Erweiterung"
    description="In diesem Thema Überblick CDN Cachingrichtlinie Media Services Erweiterung."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>CDN Cachingrichtlinie Media Services Erweiterung

Azure Media Services enthält HTTP Adaptives Streaming und progressiven Download. HTTP-basierte streaming ist hochgradig skalierbar mit Zwischenspeichern in Proxy CDN Ebenen sowie das clientseitige Zwischenspeichern. Streaming-Endpunkte enthält allgemeine Streaming-Funktionen und Konfiguration für HTTP-Cache-Header. Streaming-Endpunkte legt HTTP-Cache-Control: Max-Age und Expires-Header. Sie erhalten weitere Informationen für HTTP-Cacheheader [w3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

##<a name="default-caching-headers"></a>Standard-Caching-Header

Standardmäßig gelten streaming Endpunkte Cacheheader 3 Tage bei Bedarf Daten (Fragmente Medien/Chunks) und streamingendpunkte. Live-streaming, streaming Endpunkte wenden 3-tägige Cacheheader Daten (Fragmente Medien/Chunks) und 2 Sekunden zwischenspeichern Header streamingendpunkte Anfragen. Beim live-Programm bei Bedarf (live-Archiv) Schaltet gelten streaming Cacheheader bei Bedarf.

##<a name="azure-cdn-integration"></a>Azure CDN-integration

Azure Media Services bietet [integrierte CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) für streaming-Endpunkte. Cache-Control-Header gilt dasselbe wie streaming Endpunkte CDN streaming Endpunkte aktiviert. Azure CDN verwendet streaming-Endpunkt Cache Werte definieren die Lebensdauer intern zwischengespeicherten Objekte konfiguriert und verwendet diesen Wert auch die Lieferung Cache-Header festgelegt. Aktivierter CDN mit streaming Endpunkte empfiehlt es nicht kleinen Werte festlegen. Niedrige Werte wird die Leistung beeinträchtigen und reduzieren den Vorteil des CDN. Darf nicht kleiner als 600 Sekunden CDN streaming Endpunkte aktiviert Cache-Header festgelegt.

>[AZURE.IMPORTANT] Azure CDN Azure Media Services Integration ist in **Azure CDN von Verizon**implementiert.  **Azure CDN von Akamai** Azure Media Services verwenden möchten, müssen Sie [den Endpunkt manuell konfigurieren](cdn-create-new-endpoint.md).  Weitere Informationen zu Azure CDN-Funktionen finden Sie unter [CDN Overview](cdn-overview.md).

##<a name="configuring-cache-headers-with-azure-media-services"></a>Konfigurieren von Cache-Header mit Azure Media Services

Azure-Verwaltungsportal oder Azure Media Services APIs können Headerwerte Cache konfigurieren.

1. Konfigurieren Cacheheader mit Management-Portal finden Sie [Streaming-Endpunkte verwalten](../media-services/media-services-portal-manage-streaming-endpoints.md) Abschnitt der Streaming-Endpunkt.
2. Azure Media Services REST API [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK [StreamingEndpointCacheControl Eigenschaften](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Rangfolge für Cache-Konfiguration

1. Azure Media Services konfiguriert Wert setzt außer Kraft.
2. Keine manuelle Konfiguration, Standardwerte gilt.
3. 2 Sekunden Standardcache gilt Header Live-streaming streamingendpunkte unabhängig von Azure Media oder Azure Storage-Konfiguration und der Wert überschreiben nicht verfügbar.
