<properties
    pageTitle="Azure CDN Übersicht | Microsoft Azure"
    description="Erfahren Sie, was Azure Content Delivery Network (CDN) ist und wie Sie es von hoher Bandbreite Inhalten durch Zwischenspeichern blobs und statische Inhalte."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/30/2016"
    ms.author="casoper"/>

# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Übersicht über Azure Content Delivery Network (CDN)

> [AZURE.NOTE] Dieses Dokument beschreibt, was Azure Content Delivery Network (CDN), wie er funktioniert und die Features des jeweiligen Produkts Azure CDN.  Diese Informationen überspringen und direkt zu einer Schulung wie CDN erstellen, finden Sie unter [Verwendung von Azure CDN](cdn-create-new-endpoint.md).  Eine Liste der aktuellen CDN Knotenpositionen anzeigen, finden Sie unter [Azure CDN POP-Standorten](cdn-pop-locations.md).

Azure Content Delivery Network (CDN) speichert Statische Webinhalte an strategisch maximalen Durchsatz für die Bereitstellung von Inhalten für Benutzer.  Das CDN bietet Entwicklern eine globale Lösung für Inhalte durch Zwischenspeichern des Inhalts an physischen Knoten weltweit. 

CDN mit Cache-Website bietet folgende Vorteile:

- Bessere Performance und auftreten für Benutzer besonders Anwendung, müssen mehrere Roundtrips Inhalt geladen.
- Große besser skalieren behandeln unmittelbaren bei hohen Belastung wie zu Beginn der Veranstaltung ein Produkt.
- Verteilen von Benutzeranfragen und Inhalte von Edge-Servern wird weniger Datenverkehr Ursprung gesendet.


## <a name="how-it-works"></a>Funktionsweise

![CDN-Übersicht](./media/cdn-overview/cdn-overview.png)

1. Benutzer (Alice) fordert eine Datei (auch als Vermögenswert) mithilfe einer URL ein spezieller Domänenname, z. B. `<endpointname>.azureedge.net`.  DNS leitet die Anforderung an die leistungsstärksten Point of Presence (POP) Position.  Normalerweise ist dies POP Benutzer geografisch am nächsten liegt.

2. Wenn Edgeserver im POP die Datei im Cache keinen, fordert der Edge-Server die Datei vom Ursprung.  Der Ursprung kann ein Azure Web App, Azure-Clouddienst, Azure Storage-Konto oder öffentlich zugängliche Webserver sein.

3. Der Ursprung gibt Edge Server, einschließlich der optionalen HTTP-Header der Datei Time-to-Live (TTL) beschreiben.

4. Der Edge-Server speichert die Datei und gibt die Datei an den ursprünglichen Requestor (Alice).  Die Datei verbleibt auf dem Edgeserver die Gültigkeitsdauer abgelaufen ist.  Wenn der Ursprung eine Gültigkeitsdauer angegeben haben, ist die Standard-TTL sieben Tage.

5. Zusätzliche Benutzer verlangen dann dieselbe Datei mit dem gleichen URL und können auch zum gleichen POP weitergeleitet.

6. Wenn die Gültigkeitsdauer für die Datei abgelaufen ist, wird der Dateiname der Edge-Server aus dem Cache zurückgegeben.  Dadurch Reaktionsschnelligkeit benutzerfreundlich.


## <a name="azure-cdn-features"></a>Azure CDN-Funktionen

Es sind drei Azure CDN: **Azure CDN Standard von Akamai** **Azure CDN Standard von Verizon**und **Azure CDN Premium von Verizon**.  In der folgenden Tabelle werden die mit den einzelnen Produkten.

|       | Standard Akamai | Standard Verizon | Premium Verizon |
|-------|-----------------|------------------|-----------------|
| Einfache Integration in Azure services wie [Speicher](cdn-create-a-storage-account-with-cdn.md), [Cloud-Services](cdn-cloud-service-with-cdn.md) [Web Apps](../app-service-web/cdn-websites-with-cdn.md)und [Media-Dienste](../media-services/media-services-portal-manage-streaming-endpoints.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;**|
| Management über [REST API](https://msdn.microsoft.com/library/mt634456.aspx) [.NET,](./cdn-app-dev-net.md) [Node.js](./cdn-app-dev-node.md)oder [PowerShell](./cdn-manage-powershell.md). | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| HTTPS-Unterstützung | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Lastenausgleich | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [DDOS](https://www.us-cert.gov/ncas/tips/ST04-015) -Schutz | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Dual-Stack IPv4/IPv6 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Benutzerdefinierte-Unterstützung](cdn-map-content-to-custom-domain.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Abfrage-Zeichenfolge Cache](cdn-query-string.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Geo-Filterung](cdn-restrict-access-by-country.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Schnell löschen](cdn-purge-endpoint.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Anlage vorab laden](cdn-preload-endpoint.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Core-Analysen](cdn-analyze-usage-patterns.md) |  | **& #x 2713;** | **& #x 2713;** |
| [HTTP-2-Unterstützung](https://msdn.microsoft.com/library/mt762901.aspx) | **& #x 2713;**  |  |  |
| [Erweiterte HTTP-Berichte](cdn-advanced-http-reports.md) | | | **& #x 2713;** |
| [Echtzeit-Statistiken](cdn-real-time-stats.md) | | | **& #x 2713;** |
| [Echtzeit-Alarme](cdn-real-time-alerts.md) | | | **& #x 2713;** |
| [Anpassbare, regelbasierten Inhaltsübermittlung engine](cdn-rules-engine.md) | | | **& #x 2713;** |
| Cache-Header Einstellungen ( [Regelmodul](cdn-rules-engine.md))  | | | **& #x 2713;** |
| URL-Umleitung/schreiben (mit [Regelmodul](cdn-rules-engine.md)) | | | **& #x 2713;** |
| Mobiles Geräteregeln (mit [Regelmodul](cdn-rules-engine.md))  | | | **& #x 2713;** |

>[AZURE.TIP] Ist ein Feature in Azure CDN sehen möchte?  [Geben Sie uns Feedback](https://feedback.azure.com/forums/169397-cdn)! 

## <a name="next-steps"></a>Nächste Schritte

Zunächst mit CDN finden Sie unter [Verwendung von Azure CDN](./cdn-create-new-endpoint.md).

Bestandskunde CDN hingegen können Sie jetzt die CDN-Endpunkte über [Microsoft Azure-Portal](https://portal.azure.com) oder mit [PowerShell](cdn-manage-powershell.md)verwalten.

Überprüfen Sie CDN in Aktion, die- [video einer Sitzung Build 2016](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Informationen Sie zum Azure CDN mit [.NET](./cdn-app-dev-net.md) oder [Node.js](./cdn-app-dev-node.md)automatisieren.

Informationen zu Preisen, finden Sie unter [Preise CDN](https://azure.microsoft.com/pricing/details/cdn/).
