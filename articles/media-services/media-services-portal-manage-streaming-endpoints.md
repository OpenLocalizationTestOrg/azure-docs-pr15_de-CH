<properties 
    pageTitle="Streaming-Endpunkte mit Azure-Portal verwalten | Microsoft Azure" 
    description="In diesem Thema wird das streaming Endpunkte mit Azure-Portal verwalten." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    writer="juliako" 
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


#<a name="manage-streaming-endpoints-with-the-azure-portal"></a>Streaming-Endpunkte mit Azure-Portal verwalten

## <a name="overview"></a>Übersicht

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 

In Microsoft Azure Media Services stellt einen **Streaming-Endpunkt** einen streaming-Dienst, der Inhalte an eine Clientanwendung Player oder auf einem Content Delivery Network (CDN) für die weitere Verteilung liefern kann. Media Services bietet auch nahtlos Azure CDN. Ausgehenden Datenstrom aus StreamingEndpoint Dienst kann live-Streams oder ein Video bei Bedarf Anlage in Ihrem Media Services-Konto.

Darüber hinaus können Sie die Kapazität der Endpunkt Streaming Service mit wachsenden Bedarf an Netzwerkbandbreite durch Anpassen von Streaming-Einheiten steuern. Es wird empfohlen, eine oder mehrere Maßeinheiten für Applikationen in Produktion zugewiesen werden. Streaming-Einheiten bieten dedizierte Egress-Kapazität, die in Schritten von 200 Mbit/s erhältlich und zusätzliche Funktionen, einschließlich: [dynamische Verpackung](media-services-dynamic-packaging-overview.md)CDN-Integration und erweiterte Konfiguration.

>[AZURE.NOTE]Sie werden nur bei Streaming-Endpunkt im Ausführungszustand belastet.

Dieses Thema bietet eine Übersicht über die wichtigsten Funktionen von Streaming-Endpunkte bereitgestellt werden. Das Thema wird gezeigt, wie das Azure-Portal zu Streaming-Endpunkte. Informationen wie den Streaming-Endpunkt finden Sie unter [diesem](media-services-portal-scale-streaming-endpoints.md) Thema.

## <a name="start-managing-streaming-endpoints"></a>Verwalten Sie Streaming-Endpunkte

Starten der Verwaltung von Streaming-Endpunkte für Ihr Konto folgendermaßen Sie vor.

1. Wählen Sie in [Azure-Portal](https://portal.azure.com/)Azure Media Services-Konto.
2. Wählen Sie im Fenster **Settings** **Streaming Endpunkte**.

    ![Streaming-Endpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

##<a name="adddelete-a-streaming-endpoint"></a>Streaming-Endpunkt hinzufügen/löschen

Gehen Sie wie folgt vor, um hinzufügen/Streaming-Endpunkt mit der Azure-Portal löschen:

1. Streaming-Endpunkt klicken der **+ Endpunkt** am oberen Rand der Seite. 
2. Streaming-Endpunkt drücken Sie **Löschen** . 

    Die Standardeinstellungen für streaming-Endpunkt kann nicht gelöscht werden.
2. Klicken Sie **Start** Streaming-Endpunkt zu starten.

    ![Streaming-Endpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)

Standardmäßig können Sie bis zu zwei streaming Endpunkte aufweisen. Benötigen Sie mehr fordern, finden Sie unter [Kontingente und Einschränkungen](media-services-quotas-and-limitations.md).
    
##<a id="configure_streaming_endpoints"></a>Die Streaming-Endpunkt

Streaming-Endpunkt kann bei mindestens 1 Skalierungseinheit folgende Eigenschaften konfigurieren: 

- Zugriffskontrolle
- Cache-Steuerelement
- Cross-Site-Richtlinien

Ausführliche Informationen zu diesen Eigenschaften finden Sie unter [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx).

Streaming-Endpunkt können wie folgt:

1. Wählen Sie den Streaming-Endpunkt, den Sie konfigurieren möchten.
1. **Klicken Sie auf.**
  
Eine kurze Beschreibung der Felder folgt.

![Streaming-Endpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)
  
1. Maximale Cacherichtlinie: zum Cache für Anlagen bedient werden Streaming-Endpunkt zu konfigurieren. Wenn kein Wert festgelegt ist, wird der Standardwert verwendet. Die Standardwerte können auch direkt in Azure-Speicher definiert. Wenn Azure CDN für Streaming-Endpunkt aktiviert ist, sollte den Cache-Richtlinienwert nicht auf 600 Sekunden festgelegt werden.  

2. IP-Adressen zulässig: verwendet IP-Adressen an, die Verbindung zu veröffentlichten Streaming-Endpunkt zulässig sind. Keine angegebenen IP-Adressen wäre eine IP-Adresse herstellen. IP-Adressen als eine einzelne IP-Adresse (zum Beispiel "10.0.0.1"), einen IP-Adressbereich eine IP-Adresse und Subnetzmaske CIDR (z. B. "10.0.0.1/22") oder einen IP-Adressbereich, IP-Adresse und eine punktierte Dezimalzahl Subnetzmaske angegeben werden (z. B. "10.0.0.1(255.255.255.0)').

3. Konfiguration für Akamai Legalisation Header: Konfiguration Signatur Header Authentifizierungsanfrage Akamai-Server angegeben. Ablaufzeit ist in UTC.



##<a id="enable_cdn"></a>Azure CDN-Integration aktivieren

Sie können die Azure CDN-Integration für Streaming-Endpunkt aktivieren (es ist standardmäßig deaktiviert.)

Azure CDN-Integration auf True festlegen:

- Streaming-Endpunkt muss mindestens eine Streaming-Einheit. Wenn später Maßeinheiten auf 0 festgelegt werden soll, müssen Sie die CDN-Integration deaktivieren. Wird standardmäßig beim Erstellen einer neuen streaming Endpunkt eine streaming-Einheit automatisch festgelegt.

- Streaming-Endpunkt muss beendet werden. Nachdem das CDN aktiviert wird, können Sie Streaming-Endpunkt starten. 

Es dauert bis zu 90 Minuten Azure CDN Integration aktiviert.  Es dauert bis zu zwei Stunden ändert sich über alle die CDN-POPs aktiv sein.

CDN-Integration in der Azure-Datencentern aktiviert: US West uns Osten, Nordeuropa, Westeuropa, Japan West, Japan Osten, Südostasien und Ostasien.

Sobald er aktiviert ist, ruft der **Zugriffskontrolle** deaktiviert.

![Streaming-Endpunkt](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints5.png)

>[AZURE.IMPORTANT] Azure CDN Azure Media Services Integration ist in **Azure CDN von Verizon**implementiert.  **Azure CDN von Akamai** Azure Media Services verwenden möchten, müssen Sie [den Endpunkt manuell konfigurieren](../cdn/cdn-create-new-endpoint.md).  Weitere Informationen zu Azure CDN-Funktionen finden Sie unter [CDN Overview](../cdn/cdn-overview.md).

###<a name="additional-considerations"></a>Weitere Aspekte

- CDN für Streaming-Endpunkt aktiviert ist, können keine Clients Inhalt direkt vom Ursprung anfordern. Benötigen Sie Ihre Inhalte mit oder ohne CDN testen, können Sie einen anderen Streaming-Endpunkt erstellen, das CDN aktiviert ist.
- Die Streaming-Endpunkt Hostname bleibt nach der Aktivierung CDN. Sie müssen Änderungen an der Media Services Workflow nach dem CDN aktiviert ist. Ist Ihr Streaming-Endpunkt Hostname strasbourg.streaming.mediaservices.windows.net, nach dem CDN aktivieren, z. B. der genaue gleiche Hostnamen verwendet.
- Neue streaming Endpunkte können Sie einfach durch Erstellen eines neuen Endpunkts CDN aktivieren; für vorhandene streaming Endpunkte müssen Sie zuerst beenden den Endpunkt und aktivieren Sie dann das CDN.
 

Weitere Informationen finden Sie unter [Ankündigung Azure Media Services Integration in Azure CDN (Content Delivery Network)](http://azure.microsoft.com/blog/2015/03/17/announcing-azure-media-services-integration-with-azure-cdn-content-delivery-network/).


##<a name="next-steps"></a>Nächste Schritte

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
 
