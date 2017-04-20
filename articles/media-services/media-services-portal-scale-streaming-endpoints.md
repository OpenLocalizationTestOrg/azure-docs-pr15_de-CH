<properties
    pageTitle=" Skalierung streaming Endpunkte mit Azure-Portal | Microsoft Azure"
    description="Dieses Lernprogramm führt Sie durch die Schritte der Skalierung streaming Endpunkte mit Azure-Portal."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Skalieren Sie streaming Endpunkte mit Azure-portal

##<a name="overview"></a>Übersicht

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 

Arbeit mit Azure Media Services, die eines der häufigsten Szenarios adaptive Bitrate streaming Video an die Clients übermittelt. Media Services unterstützt die folgende adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern).

Media Services bietet dynamische Verpackung der adaptive Bitrate codiert MP4 Inhalt streaming-Formate von Media Services (MPEG DASH HLS Smooth Streaming, HDS) just-in-Time, ohne vorkonfigurierte Versionen dieser streaming-Formaten speichern zu kann.

Um dynamische Verpackung nutzen zu können, müssen Sie Folgendes tun:

- Codieren Sie die Aufsteckkarte (Quelle) Datei in adaptive Bitrate MP4-Dateien (Codierung Schritte werden später in diesem Lernprogramm gezeigt).  
- Erstellen Sie mindestens eine Streaming-Einheit für die *streaming-Endpunkt* aus der Lieferung des Inhalts werden soll. Schritte anzeigen zum Ändern der Anzahl der Streaming-Einheiten

Dynamische Verpackung müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen mit Media Services erstellen und die entsprechende Antwort auf Anfragen von einem Client verwendet.

Darüber hinaus können Sie die Kapazität der Endpunkt Streaming Service mit wachsenden Bedarf an Netzwerkbandbreite durch Anpassen von Streaming-Einheiten steuern. Es wird empfohlen, eine oder mehrere Maßeinheiten für Applikationen in Produktion zugewiesen werden. Streaming-Einheiten bieten sowohl dedizierte Egress-Kapazität, die in Schritten von 200 Mbit/s und weitere Funktionen die Funktionen erworben werden können umfasst: [dynamische Verpackung](media-services-dynamic-packaging-overview.md)CDN-Integration und erweiterte Konfiguration. Weitere Informationen finden Sie unter [streaming Endpunkte mit Azure-Portal verwalten](media-services-portal-manage-streaming-endpoints.md).

## <a name="scale-streaming-endpoints"></a>Streaming-Endpunkte skalieren

Um die Anzahl der reservierte Einheiten streaming zu erstellen, führen Sie folgende Schritte aus:

1. Wählen Sie in [Azure-Portal](https://portal.azure.com/)Azure Media Services-Konto.
2. Wählen Sie im Fenster **Settings** **Streaming Endpunkte**.
3. Klicken Sie auf den Streaming-Endpunkt zu skalieren. 
4. Verschieben Sie den Regler an die Anzahl der Streaming-Einheiten
 
![Streaming-Endpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

Folgendes gilt:

- Die Zuweisung neuer streaming Einheiten dauert etwa 20 Minuten. 
- Derzeit können wird von jeder positive Wert von streaming-Einheiten auf keine deaktivieren auf-Anforderung-streaming bis zu einer Stunde.
- Höchste Anzahl von Einheiten für 24 Stunden wird bei der Berechnung der Kosten verwendet. Informationen über Preise finden Sie in der [Media-Preisdetails](http://go.microsoft.com/fwlink/?LinkId=275107).

##<a name="next-steps"></a>Nächste Schritte

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


