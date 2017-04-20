<properties 
    pageTitle="Media Services Versionsinformationen | Microsoft Azure" 
    description="Medien-Versionsinformationen" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="media" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

# <a name="azure-media-services-release-notes"></a>Versionsinformationen Azure Media Services

Diese Versionshinweise zusammenfassen ändert sich von früheren Versionen und bekannte Probleme.

>[AZURE.NOTE] Wir möchten gerne von unseren Kunden und Beheben von Problemen, die Sie betreffen. Stellen Sie in [Azure Media Services MSDN-Forum], um Probleme melden oder Fragen.


##<a id="issues"></a>Derzeit bekannte Probleme

### <a id="general_issues"></a>Media Services allgemeine Probleme

Problem|Beschreibung
---|---
Mehrere häufig verwendete HTTP-Header werden nicht in die REST-API bereitgestellt.|Media Services-Clientanwendungen mithilfe der REST-API entwickeln, finden Sie, einige HTTP-Header-Feldern (einschließlich CLIENT-Anfrage-ID-Kennung und RETURN-CLIENT-Anfrage-ID) werden nicht unterstützt. Der Header werden in einem zukünftigen Update hinzugefügt.
Prozent-Codierung ist nicht zulässig.|Media Services verwendet den Wert der Eigenschaft IAssetFile.Name beim Erstellen von URLs für das streaming von Inhalten (z. B. http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Aus diesem Grund ist die Prozent-Codierung nicht zulässig. Der Wert der **Name** -Eigenschaft keinen der folgenden [%-Codierung-reservierten Zeichen](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Es können nur auch einen "." für die Erweiterung.
Die ListBlobs-Methode, die Teil von Azure Storage SDK Version 3.x fehlschlägt.|Media Services generiert basierend auf der Version [2012-02-12](http://msdn.microsoft.com/library/azure/dn592123.aspx) SAS-URLs. Azure Storage SDK Liste Blobs in einem BLOB-Container verwenden, verwenden Sie die [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) Methode Teil Azure Storage SDK Version 2.x. Die ListBlobs-Methode, die Teil der Azure Storage SDK Version 3.x fehl.
Media Services Drosselungsmechanismus schränkt die Ressourcenverwendung für Programme, die den Dienst übermäßige anfordern. Der Dienst kann den Dienst nicht verfügbar (503) HTTP-Statuscode zurückgeben.|Weitere Informationen finden Sie in der Beschreibung der 503 HTTP-Statuscode in [Azure Media Services Fehlercodes](http://msdn.microsoft.com/library/azure/dn168949.aspx) Thema.
Beim Abfragen von Entitäten ist maximal 1000 Elemente gleichzeitig zurückgegeben werden, da Öffentliche REST v2 Abfrageergebnisse auf 1000 Ergebnisse begrenzt. | Müssen Sie **Überspringen** und **nehmen** (.NET) **oben** (REST) als in [diesem Beispiel .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) und [dabei REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)beschrieben. 
Einige Clients können über ein Problem wiederholen Tag im Manifest Smooth Streaming kommen.|Weitere Informationen finden Sie in [diesem](media-services-deliver-content-overview.md#known-issues) Abschnitt.
Azure Media Services .NET SDK-Objekte nicht serialisiert werden kann und daher funktionieren nicht mit Azure Caching.|Beim Serialisieren des Objekts SDK AssetCollection um Azure Caching hinzuzufügen, wird eine Ausnahme ausgelöst.
Codierung Aufträge schlagen fehl, wird die Meldungszeichenfolge "Phase: DownloadFile. Code: System.NullReferenceException ".|Der übliche Arbeitsablauf Codierung ist video zu übersetzenden Dateien auf eine Eingabe, und senden mindestens ein Kodierung Aufträge für diese Eingabe Anlage ohne weitere Änderung, die Anlage eingegeben. Jedoch wenn input Anlage (beispielsweise durch Hinzufügen/Löschen/Umbenennen von Dateien in der Anlage) ändern, fehlschlagen dann nachfolgende Aufträge DownloadFile Fehler. Die Lösung ist input Anlage löschen und erneut hochladen auf eine neue Anlage zu übersetzenden Dateien. 

##<a id="rest_version_history"></a>REST-API Versionsverlauf

Informationen über den Versionsverlauf Media Services REST-API finden Sie unter [Azure Media Services REST-API-Referenz].

##<a id="july_changes16"></a>Juli 2016 Version

###<a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Updates in die Manifestdatei (*. ISM) generiert codieren Aufgaben

Wenn eine Kodierung Aufgabe Media Encoder Standard oder Azure Media Encoder gesendet wird, generiert die Codierung Aufgabe [streaming Manifestdatei](media-services-deliver-content-overview.md) (* ISM) Datei in der Ausgabe des Vermögenswertes. Mit dem neuesten Service Release wurde die Syntax der Streaming-Manifestdatei aktualisiert.

>[AZURE.NOTE]Die Syntax der Streamingdatei Manifest (ISM) ist für die interne Verwendung reserviert und kann in zukünftigen Versionen geändert. Nicht ändern Sie oder bearbeiten Sie den Inhalt dieser Datei.

###<a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Ein neues Clientmanifest (*. ISMC) Datei wird in der Ausgabe Anlage Wenn eine Kodierung Aufgabe eine oder mehrere MP4-Dateien ausgegeben

Starten mit dem neuesten Service Release Abschluss eine Codierung Aufgabe, die eine generiert, weitere MP4-Dateien, die Ausgabe enthält Anlage auch eine Streamingdatei Client Manifest (*.ismc). Die Datei .ismc verbessert die Leistung von dynamischen Stream. 

>[AZURE.NOTE]Die Syntax der Clientdatei Manifest (.ismc) ist für die interne Verwendung reserviert und kann in zukünftigen Versionen geändert. Nicht ändern Sie oder bearbeiten Sie den Inhalt dieser Datei.

Weitere Informationen finden Sie in [diesem](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) Blog.

### <a name="known-issues"></a>Bekannte Probleme

Einige Kunden kommen Aross ein Problem wiederholen Tag im Manifest Smooth Streaming. Weitere Informationen finden Sie in [diesem](media-services-deliver-content-overview.md#known-issues) Abschnitt.

##<a id="apr_changes16"></a>April 2016 Version

### <a name="azure-media-analytics"></a>Azure Media Analytics

Azure Media Dienstleistungen Azure Media Analytics für leistungsstarke video Intelligence eingeführt. Ausführliche Informationen finden Sie in [Azure Media Services Analytics Overview](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (Vorschau)

Azure Media Services können nun Ihre HTTP-Live-Streaming (HLS) mit Apple FairPlay dynamisch zu verschlüsseln. AMS-Lizenz Zustelldienst können FairPlay Lizenzen an Clients übermittelt. Weitere Informationen finden Sie unter [verwenden Azure Media Services die HLS streamen Inhalt geschützt mit Apple FairPlay ](media-services-protect-hls-with-fairplay.md).
  
##<a id="feb_changes16"></a>Februar 2016 Version

Die neueste Version von Azure Media Services SDK für .NET (3.5.3) enthält Widevine zugehörige Fehler behoben. Das Problem war: AssetDeliveryPolicy konnte nicht für mehrere Elemente mit Widevine verschlüsselt wiederverwendet werden. Als Teil dieser Fehlerkorrektur wurde die folgende Eigenschaft SDK hinzugefügt: **WidevineBaseLicenseAcquisitionUrl**.
    
    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},
        
    };

##<a id="jan_changes_16"></a>Veröffentlichung Januar 2016

Codierung reservierte Einheiten umbenannt, um Verwechslungen mit Encoder zu vermeiden.

Basic, Standard und Premium codieren, reservierten Einheiten werden umbenannt, S1, S2 und S3 Einheiten bzw. reserviert.  Kunden verwenden grundlegende Codierung RUs sehen S1 als Bezeichnung in Azure-Portal und in der Stückliste bei Standard und Premium Etiketten S2 und S3 bzw. sehen. 

##<a id="dec_changes_15"></a>Version Dezember 2015

Azure SDK-Team veröffentlicht eine neue Version von [Azure SDK für PHP](http://github.com/Azure/azure-sdk-for-php) -Paket mit Updates und neue Funktionen für Microsoft Azure Media Services ab. Azure Media Services SDK für PHP nun unterstützt die neuesten [Inhalte](media-services-content-protection-overview.md) Schutzfunktionen: dynamische Verschlüsselung mit AES und DRM (PlayReady und Widevine) mit und ohne Token. Außerdem wird die Skalierung [Codierung Einheiten](media-services-dotnet-encoding-units.md)unterstützt.

Weitere Informationen finden Sie unter:

- [Microsoft Azure Media Services SDK für PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) -Blog.
- Die folgenden [Beispiele](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) helfen Ihnen schnell beginnen:
    - **vodworkflow_aes.PHP**: Dies ist eine PHP-Datei, die Verwendung von AES-128 dynamische Verschlüsselung und Schlüssel Übermittlungsdienst. Es basiert auf den in [diesem](media-services-protect-with-aes128.md) Artikel beschriebene Beispiel für .NET.
    - **vodworkflow_aes.PHP**: ist eine PHP-Datei, die wie Dynamic PlayReady-Verschlüsselung und Übermittlungsdienst Lizenz verwendet werden. Es basiert auf den in [diesem](media-services-protect-with-drm.md) Artikel beschriebene Beispiel für .NET.
    - **scale_encoding_units.PHP**: ist eine PHP-Datei, die wie Codierung reservierte Einheit anzeigt.


##<a id="nov_changes_15"></a>November 2015 Version

Azure Media Services bietet Google Widevine Lizenz Delivery Service in der Cloud. Einzelheiten Sie weitere zu [dieser Ankündigung Blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Siehe auch [in diesem Lernprogramm](media-services-protect-with-drm.md) und [GitHub Repository](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Beachten Sie, dass Widevine Lizenz Lieferung Dienstleistungen Azure Media Services in der Vorschau. Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

##<a id="oct_changes_15"></a>Veröffentlichung Oktober 2015

Azure Media Services (AMS) ist nun in folgenden Rechenzentren: Süden Brasiliens Indien West, South Indien und Indien zentralen. Sie verwenden Azure-Verwaltungsportal [Media Dienstkonten](media-services-portal-create-account.md) erstellen und verschiedene Aufgaben beschrieben [hier](https://azure.microsoft.com/documentation/services/media-services/). Live-Codierung ist jedoch in Rechenzentren nicht aktiviert. Darüber hinaus stehen nicht alle Arten von Codierung reservierte Einheiten in Rechenzentren.

- Brasilien Süd: Standard und Codierung reservierte Basiseinheiten sind nur verfügbar
- Indien West, South Indien und Indien Central: nur grundlegende Codierung reservierte Einheiten


##<a id="september_changes_15"></a>Version September 2015 

- AMS bietet Video-On-Demand (VOD) und Live-Streams mit Widevine modulares DRM-Technologie schützen. Sie können Delivery Servicepartner Widevine Lizenzen Bereitstellung: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [CastLabs](http://castlabs.com/company/partners/azure/). Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).

    [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (ab Version 3.5.1) oder REST-API können Sie Ihre AssetDeliveryConfiguration Widevine Verwendung konfigurieren.  

- AMS Unterstützung für Apple ProRes Videos. Sie können jetzt QuickTime Videos Quelldateien hochladen, die Apple ProRes oder andere Codecs verwenden. Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).

- Jetzt können Media Encoder Standard untergeordnete clipping und live Archiv extrahieren möchten. Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

- Die folgenden Filter wurden aktualisiert: 

    - Jetzt können Apple HTTP-Live-Streaming (HLS) Format nur Audio verwenden. Dieses Update können Sie reine Audiospur durch Angabe (nur Audio = False) in der URL.
    - Beim Definieren von Filtern für Ihre Anlagen Sie jetzt können mehrere kombinieren (3) Filter in einer URL.

    Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) Blog.

- AMS unterstützt jetzt HLS v4 I-Frames. I-Frame-Unterstützung optimiert schnelle vor- und Rücklauf Operationen. Standardmäßig enthalten alle HLS v4 Ausgaben I-Frame-Wiedergabeliste (Ext).
 
    Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) Blog.

##<a id="august_changes_15"></a>Veröffentlichung August 2015

- Azure Media Services SDK Java V0.8.0 und neue Beispiele sind jetzt verfügbar. Weitere Informationen finden Sie unter:

    - [Blogbeitrag](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
    - [Java-Beispiele repository](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- Azure Media Player-Update mit Multi-Audio-Unterstützung. Weitere Informationen finden Sie unter:
    - [Blogbeitrag](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

##<a id="july_changes_15"></a>Juli 2015 Version

- Ankündigung der allgemeinen Verfügbarkeit von Media Encoder Standard. Weitere Informationen finden Sie in [diesem Blogbeitrag](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).

    Media Encoder Standard verwendet in [diesem](http://go.microsoft.com/fwlink/?LinkId=618336) Abschnitt beschriebenen Vorgaben. Beachten Sie, dass beim codiert mit einer Voreinstellung für 4k **Premium** reserviert Einheitentyp abrufen soll. Weitere Informationen finden Sie unter [Skalierung Codierung](media-services-scale-media-processing-overview.md).
- Leben Sie in Echtzeit Untertitel Azure Media Services und Player. Weitere Informationen finden Sie in [diesem Blogbeitrag](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

###<a name="media-services-net-sdk-updates"></a>Mediendienste .NET SDK-Updates

Azure Media Services .NET SDK ist Version 3.4.0.0. Die folgende Funktionen wurde in dieser Version hinzugefügt:  

- Unterstützung für live-Archiv. Beachten Sie, dass eine Anlage herunterladen kann, die ein live-Archiv enthält.
- Unterstützung für dynamische Filter.
- Implementierte Funktionalität, die Benutzer zu Behälter beim Löschen der Anlage.
- Fehlerbehebungen im Zusammenhang mit Richtlinien Kanäle wiederholen.
- **Media Encoder Premium-Workflow**aktiviert.

##<a id="june_changes_15"></a>Veröffentlichung Juni 2015

###<a name="media-services-net-sdk-updates"></a>Mediendienste .NET SDK-Updates

Azure Media Services .NET SDK ist Version 3.3.0.0. Die folgende Funktionen wurde in dieser Version hinzugefügt:  

- Unterstützung für OpenId verbinden Discovery-Spezifikation
- Unterstützung für die Behandlung von Schlüsseln Rollover auf Anbieter Identität. 

Verwenden Sie einen Identitätsanbieter das Discoverydokument OpenID verbinden verfügbar macht (wie der folgenden Anbieter: Azure Active Directory Google, Salesforce), Azure Media Services zu Signaturschlüssel für Validierung JWT Token OpenID connect Discovery Spec angewiesen. 

Weitere Informationen finden Sie unter [Verwendung von Json Web Schlüssel OpenID verbinden Discovery Spec JWT token Authentifizierung in Azure Media Services arbeiten](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).


##<a id="may_changes_15"></a>Mai 2015 Version

Ankündigung der folgenden neuen Funktionen:

- [Eine Vorschau der Live-Kodierung mit Media Services](media-services-manage-live-encoder-enabled-channels.md)
- [Dynamische manifest](media-services-dynamic-manifest-overview.md)
- [Eine Vorschau der Azure Media Hyperlapse Media Prozessor](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

##<a id="april_changes_15"></a>April 2015 Version

 ###<a name="general-media-services-updates"></a>Allgemeine Media Services-Updates

- [Ankündigung von Azure MediaPlayer](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
- Mit Media Services REST 2.10 Kanäle konfiguriert RTMP-Protokoll aufnehmen werden erstellt mit primären und sekundären URLs aufnehmen. Weitere Informationen finden Sie unter [Channel Aufnahme Konfigurationen](media-services-live-streaming-with-onprem-encoders.md#channel_input)
- Azure Media Indexer aktualisiert
- Unterstützung für Spanisch
- Neue Konfiguration XML-format

Weitere Informationen finden Sie in [diesem Blog](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).
###<a name="media-services-net-sdk-updates"></a>Mediendienste .NET SDK-Updates

Azure Media Services .NET SDK ist Version 3.2.0.0.

Es folgen einige kundenorientierte Updates:

- **Bedeutende Änderung**: **TokenRestrictionTemplate.Issuer** und **TokenRestrictionTemplate.Audience** vom Typ String werden geändert.
- Erstellung von benutzerdefinierten Updates wiederholen Richtlinien.
- Fehlerbehebungen im Zusammenhang mit Dateien hochladen/herunterladen.
- Die **MediaServicesCredentials** -Klasse akzeptiert nun Zugriff auf primären und sekundären Steuerungsendpunkt authentifizieren.



##<a id="march_changes_15"></a>März 2015 Version

### <a name="general-media-services-updates"></a>Allgemeine Media Services-Updates

- Media Services bietet jetzt Azure CDN-Integration. **CdnEnabled** -Eigenschaft wurde zur Unterstützung der Integration **StreamingEndpoint**hinzugefügt.  **CdnEnabled** ab Version 2.9 REST-APIs verwendet werden (Weitere Informationen finden Sie unter [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)).  .NET SDK ab Version 3.1.0.2 **CdnEnabled** verwendet werden (Weitere Informationen finden Sie unter [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint(v=azure.10).aspx)).
- Ankündigung von **Media Encoder Premium-Workflow**. Weitere Informationen finden Sie unter [Einführung in Premium Codierung in Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).
 


##<a id="february_changes_15"></a>Februar 2015 Version

### <a name="general-media-services-updates"></a>Allgemeine Media Services-Updates

Media Services REST API ist Version 2.9. Ab dieser Version können Sie die Azure CDN-Integration mit streaming-Endpunkte. Weitere Informationen finden Sie unter [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

##<a id="january_changes_15"></a>Veröffentlichung Januar 2015

### <a name="general-media-services-updates"></a>Allgemeine Media Services-Updates

Ankündigung der allgemeinen Verfügbarkeit von Content Protection dynamische Verschlüsselung. Weitere Informationen finden Sie unter [Azure Media Services verbessert die allgemeine Verfügbarkeit der DRM-Technologie streaming Sicherheit](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

###<a name="media-services-net-sdk-updates"></a>Mediendienste .NET SDK-Updates

Azure Media Services .NET SDK ist Version 3.1.0.1.

Diese Version markiert den Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate-Standardkonstruktor als veraltet Der neue Konstruktor verwendet TokenType als Argument.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


##<a id="december_changes_14"></a>Version Dezember 2014

###<a name="general-media-services-updates"></a>Allgemeine Media Services-Updates

- Prozessor Azure Indexer Medien wurden einige Updates und neue Funktionen hinzugefügt. Weitere Informationen finden Sie unter [Azure Media Indexer 1.1.6.7-Versionsinformationen](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
- Eine neue REST-API, die Codierung Aktualisierung ermöglicht reserviert Einheiten hinzugefügt: [EncodingReservedUnitType mit](http://msdn.microsoft.com/library/azure/dn859236.aspx).
- Hinzugefügte CORS Unterstützung für wichtige Übermittlungsdienst.
- Leistungssteigerungen Autorisierungsoptionen Abfragen wurden durchgeführt.
- In China Rechenzentrum [Schlüssel Lieferung URL](http://msdn.microsoft.com/library/azure/ef4dfeeb-48ae-4596-ab28-44d6b36d8769#get_delivery_service_url) ist jetzt pro Kunde (wie in anderen Rechenzentren).
- Hinzugefügte HLS Auto Ziel Dauer. Bei live-streaming wird immer dynamisch HLS gepackt. Standardmäßig berechnet Media Services HLS Segment Verpackung Verhältnis (FragmentsPerSegment) entsprechend dem Keyframe Intervall (KeyFrameInterval), so genannte Group of Pictures, GOP, das vom Live Encoder empfangen wird. Weitere Informationen finden Sie unter [Arbeiten mit Azure Media Services Live-Streaming].
 
###<a name="media-services-net-sdk-updates"></a>Mediendienste .NET SDK-Updates

- [Azure Media Services.NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) ist Version 3.1.0.0.
- .Net SDK Abhängigkeit aktualisiert auf .NET Framework 4.5.
- Eine neue API, die Codierung reservierte Einheiten Aktualisierung ermöglicht hinzugefügt. Weitere Informationen finden Sie unter [reservierte Einheitentyp aktualisieren und Codierung RUs erhöhen .NET verwenden](http://msdn.microsoft.com/library/azure/jj129582.aspx).
- Zusätzliche JWT (JSON Web Token) Unterstützung für token-Authentifizierung. Weitere Informationen finden Sie unter [JWT-token Authentifizierung in Azure Media Services und dynamische Verschlüsselung](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
- Hinzugefügte relativen Versatz ExpirationDate zu "BeginDate" in der Vorlage PlayReady-Lizenz.


##<a id="november_changes_14"></a>November 2014 Release

- Media Services können jetzt live Smooth Streaming (FMP4)-Inhalte über eine SSL-Verbindung aufnehmen. Um Aufnahme über SSL unbedingt Erfassung URL HTTPS aktualisieren.  Weitere Informationen zum live-streaming, finden Sie unter [Arbeiten mit Azure Media Services Live-Streaming].
- Beachten Sie, dass derzeit eine RTMP Livestream über eine SSL-Verbindung aufnehmen kann nicht.
- Sie können auch die Inhalte über eine SSL-Verbindung übertragen. Dazu sicherzustellen Sie, dass Ihre streaming URLs mit HTTPS beginnen.
- Beachten Sie, dass Sie nur über SSL streamen können, wenn Streaming-Endpunkt aus dem Ihre Inhalte liefern nach dem 10. September 2014 erstellt wurde. Wenn streaming URLs auf streaming Endpunkte 10. September erstellt, enthält die URL "streaming.mediaservices.windows.net" (neues Format). Streaming URLs mit "origin.mediaservices.windows.net" (das alte Format) unterstützen SSL. Wenn der URL im alten Format und über SSL, [Erstellen Sie einen neuen Streaming-Endpunkt](media-services-portal-manage-streaming-endpoints.md)übertragen werden soll. Erstellt basierend auf den neuen Streaming-Endpunkt URLs zum Verwenden des Inhalts über SSL.

##<a id="october_changes_14"></a>Oktober 2014 Release

### <a id="new_encoder_release"></a>Media Services Encoder Release

Ankündigung der neuen Version von Media Services Azure Media Encoder. Mit Azure Media Encoder nur für Zahlen Ausgabe GBs aber ansonsten neue Encoder ist kompatibel mit früheren Encoder. Weitere Informationen [Media-Preisdetails]).

### <a id="oct_sdk"></a>Media Services .NET SDK 

Media Services SDK für .NET Extensions ist Version 2.0.0.3.

Media Services SDK für .NET ist Version 3.0.0.8.

Die folgenden wurden geändert:

- Umgestaltung in Wiederholung Richtlinie Klassen.
- HTTP-Anforderungsheader Benutzeragententext hinzugefügt.
- Nuget Wiederherstellung Buildschritt hinzufügen
- Festsetzung Szenario Tests Verwendung X509 Zertifikat vom Repository.
- Überprüfen von Einstellungen Beendigung Aktualisierung Channels und streaming.
 

### <a name="new-github-repository-to-host-media-services-samples"></a>Neue GitHub Repository Host Media Services-Beispiele

Beispiele befinden sich in [Azure Media Services Samples GitHub Repository](https://github.com/Azure/Azure-Media-Services-Samples).


##<a id="september_changes_14"></a>Version September 2014

Media Services REST Metadaten ist Version 2.7. Weitere Informationen über die neuesten weiteren Updates finden Sie in [Azure Media Services REST-API-Referenz].

Media Services SDK für .NET ist Version 3.0.0.7
 
### <a id="sept_14_breaking_changes"></a>Grundlegend geändert

* **Ursprung** wurde in [StreamingEndpoint]umbenannt.
* Eine Änderung im Standardverhalten bei Verwendung der **Azure-Verwaltungsportal** zu codieren MP4-Dateien veröffentlichen.

Zuvor, wenn mit der Azure-Verwaltungsportal an eine einzelne Datei MP4 video-Assets SAS-URL erstellt (SAS-URLs können Sie das Video von einer BLOB-Speicher herunterladen). Bei Verwendung der Azure-Verwaltungsportal zu codieren und veröffentlichen ein Einzeldatei-MP4-Videos-Assets zeigt die generierte URL derzeit auf ein streaming-Endpunkt Azure Media Services.  Diese Änderung betrifft nicht MP4-Videos, die direkt in Media Services hochgeladen und ohne Codierung von Azure Media Services veröffentlicht.

Derzeit haben Sie die folgenden beiden Optionen zur Lösung des Problems.

* Streaming-Einheiten aktivieren und Verwenden von dynamischen Verpackung MP4 Anlage eine reibungslose Streamingpräsentation zu streamen.

* Erstellen Sie eine SAS-Url der MP4 herunterladen (oder schrittweise Wiedergabe). Weitere Informationen zum Erstellen von SAS-Locator finden Sie unter [Inhalt liefern].


### <a id="sept_14_GA_changes"></a>Neue Funktionen/Szenarien Teil GA-release

* **Indexer Media Prozessor**. Weitere Informationen finden Sie unter [Mediendateien mit Azure Media Indexer die Indizierung].

* Die [StreamingEndpoint] Entität kann jetzt benutzerdefinierte Domänennamen (Host) hinzufügen.

    Für einen benutzerdefinierten Domänennamen als Namen für streaming Media Services-Endpunkt verwendet werden müssen Sie benutzerdefinierte Hostnamen Streaming-Endpunkt hinzufügen. Verwenden Sie Media Services REST APIs oder .NET SDK benutzerdefinierte Hostnamen hinzufügen.
    
    Folgendes gilt:
    
    * Sie müssen an den benutzerdefinierten Domänennamen.
    
    * Der Besitz des Domänennamens muss Azure Media Services überprüft. Überprüfen Sie die Domäne erstellen CName Karten <MediaServicesAccountId>.<parent domain> Verifydns. < Mediaservices-Dns-Zone >. 
    
    * Sie müssen anderen CName erstellen, benutzerdefinierte Hostnamen (z. B. sports.contoso.com) die Hostnamen der Media Services StreamingEndpont (z. B. amstest.streaming.mediaservices.windows.net) zugeordnet ist.


    Weitere Informationen finden Sie in der Eigenschaft **CustomHostNames** im Thema [StreamingEndpoint] .

### <a id="sept_14_preview_changes"></a>Neue Features oder Szenarien, die public Preview-Version gehören

* Live-Streaming-Vorschau. Weitere Informationen finden Sie unter [Arbeiten mit Azure Media Services Live-Streaming].

* Wichtige Übermittlungsdienst. Weitere Informationen finden Sie unter [verwenden AES 128 dynamische Verschlüsselung und Schlüssel Übermittlungsdienst].

* Dynamische AES-Verschlüsselung. Weitere Informationen finden Sie unter [verwenden AES 128 dynamische Verschlüsselung und Schlüssel Übermittlungsdienst].

* Zustelldienst PlayReady-Lizenz. Weitere Informationen finden Sie unter [Dynamic PlayReady-Verschlüsselung verwenden und Lizenz Übermittlungsdienst].

* Dynamische PlayReady-Verschlüsselung. Weitere Informationen finden Sie unter [Dynamic PlayReady-Verschlüsselung verwenden und Lizenz Übermittlungsdienst].

* Media Services PlayReady-Lizenz-Vorlage. Weitere Informationen finden Sie unter [Übersicht über Vorlagen von Media Services PlayReady Lizenz].

* Streaming-Speicher verschlüsselt Anlagen. Weitere Informationen finden Sie unter [Streaming-Speicher verschlüsselt Content].

##<a id="august_changes_14"></a>Veröffentlichung August 2014

Beim Kodieren eines Vermögenswertes wird nach Abschluss eines Codierungsauftrags Vermögenswert Ausgabe erzeugt. Bis diese Version erzeugt Azure Media Services Encoder Metadaten Ausgabe Anlagen. Ab diesem Release Encoder erzeugt auch Metadaten input Anlagen. Weitere Informationen finden Sie [Metadaten für Eingabe] und [Ausgabe Metadaten] .


##<a id="july_changes_14"></a>Juli 2014 Release

Die folgenden Updates wurden für Azure Media Services Manager und Encryptor:

* Nur Audio wiedergegeben wird, wenn Transmuxing ein live-Archiv Anlage HTTP-Live-Streaming – dies behoben wurde und nun sowohl Video als auch Audio wiedergegeben werden.

* Beim Packen einer Anlage HTTP-Streaming Live und AES 128-Bit-Verschlüsselung Umschlag eingesetzte Streams auf Geräte nicht wiedergegeben werden-dieser Fehler behoben und verpackte Stream spielt auf Geräte, die HTTP-Live-Streaming unterstützen.

##<a id="may_changes_14"></a>Mai 2014 Release

### <a id="may_14_changes"></a>Allgemeine Media Services-Updates

Jetzt können [Dynamische Verpackung] Stream HTTP-Live-Streaming (HLS) v3. Stream HLS v3, Ursprung Locator Pfad folgendermaßen hinzufügen: * .ism/manifest(format=m3u8-aapl-v3). Weitere Informationen finden Sie im [Blog des Nick Drouin].

Dynamische Verpackung jetzt auch unterstützt liefern HLS (v3 und v4) mit PlayReady verschlüsselt auf Smooth Streaming statisch mit PlayReady verschlüsselt. Informationen zum Smooth Streaming mit PlayReady verschlüsseln finden Sie unter [Schutz glatte Stream mit PlayReady].

### <a name="may_14_donnet_changes"></a>Mediendienste .NET SDK-Updates

Folgenden Verbesserungen gehören Media Services .NET SDK 3.0.0.5 eingeführt:

* Höhere Geschwindigkeit und Stabilität für Medien hochladen/herunterladen.

* Verbesserung der Wiederholung Logik und flüchtige Ausnahmebehandlung: 

    * Vorübergehender Fehler erkennen und Wiederholung Logik wurden Ausnahmen zurückzuführen sind Abfragen, speichern, hochladen oder Herunterladen von Dateien verbessert. 
    
    * Beim Abrufen von Ausnahmen (z. B. während einer token ACS-Anforderung) sehen Sie Fehler schneller jetzt fehlschlagen.

Weitere Informationen finden Sie unter [Wiederholen Logik in Media Services SDK für .NET].

##<a id="april_changes_14"></a>April 2014 Encoder Release

### <a name="april_14_enocer_changes"></a>Media Services Encoder Updates

* Unterstützung für AVI-Dateien mit dem Gras Valley EDIUS nichtlineare Editor erstellte das Video leicht ist Einnahme mit Gras Valley HQ/HQX Codec komprimiert. Weitere Informationen finden Sie unter [Gras Valley kündigt EDIUS 7 Streaming über die Cloud].

* Unterstützung für die Benennungskonvention für die Dateien vom Media Encoder angeben. Weitere Informationen finden Sie unter [Media Service Encoder-Ausgabedateinamen steuern].

* Unterstützung für video oder audio überlagern. Weitere Informationen finden Sie unter [Erstellen von Overlays].

* Unterstützung für mehrere video-Segmente zusammenfügen. Weitere Informationen finden Sie im [Video-Segmente zusammenfügen].

* Fehler: Bezug auf transcodieren MP4s, Audio mit MPEG-1 Audio Layer 3 (aka MP3) codiert wurde.


##<a id="jan_feb_changes_14"></a>Januar Februar 2014 Versionen

### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 und 3.0.0.3

Änderung in 3.0.0.1 und 3.0.0.2 umfassen:

* Zusammenhang mit der Verwendung von LINQ-Abfragen mit OrderBy behoben.

* Split Test Solutions in [GitHub] in Einheitenbasis und szenariobasierten Tests.

Weitere Informationen über die Änderungen finden Sie unter: [Azure Media Services .NET SDK 3.0.0.1 und 3.0.0.2 frei].

Die folgenden wurden 3.0.0.3 geändert:

* Azure-Speicher Abhängigkeiten mit Version 3.0.3.0 aktualisiert. 

* Abwärtskompatibilität behoben für 3.0. *.* frei. 


##<a id="december_changes_13"></a>Dezember 2013 Release

### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.0

>[AZURE.NOTE] 3.0.x.x-Versionen sind nicht abwärtskompatibel mit 2.4.x.x.

Die neueste Version von Media Services SDK ist jetzt 3.0.0.0. Sie können das aktuelle Paket von Nuget herunterladen oder Bits von [GitHub].

Media Services SDK Version 3.0.0.0 ab, können Sie die Token [Azure Active Directory Access Control Service (ACS)] verwenden. Weitere Informationen finden Sie im Abschnitt "Wiederverwenden Control Service Zugriffstoken" in [Verbindung mit Media Services mit Media Services SDK für .NET] Thema.

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services .NET SDK Extensions 2.0.0.0

Azure Media Services .NET SDK Extensions ist eine Reihe von Erweiterungsmethoden und Hilfsfunktionen, die Vereinfachung des Codes und Azure Media Services zu erleichtern. Sie erhalten die neuesten Bits von [Azure Media Services.NET SDK Extensions].

##<a id="november_changes_13"></a>November 2013 Release

### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK ändern

Ab dieser Version behandelt Media Services SDK für .NET vorübergehende Fehler, die auftreten können, wenn Media Services REST-API-Ebene aufrufen.

##<a id="august_changes_13"></a>August 2013 Release

### <a name="aug_13_powershell_changes"></a>Media Services PowerShell-Cmdlets in Azure Sdk-Tools enthalten

Die folgenden Media Services PowerShell-Cmdlets sind jetzt in [Azure-Sdk-Tools]enthalten.

* AzureMediaServices abrufen 

    Z. B. `Get-AzureMediaServicesAccount`.

* Neue AzureMediaServicesAccount 

    Z. B. `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.

* Neue AzureMediaServicesKey 

    Z. B. `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.

* AzureMediaServicesAccount entfernen 

    Z. B. `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

##<a id="june_changes_13"></a>Juni 2013 Release

### <a name="june_13_general_changes"></a>Azure Media Services ändern

In diesem Abschnitt erwähnten geändert werden Updates im Juni 2013 Media Services-Versionen enthalten.

* Möglichkeit, mehrere Speicherkonten Media Service-Konto verknüpfen. 

    StorageAccount
    
    Asset.StorageAccountName und Asset.StorageAccount

* Möglichkeit, Job.Priority zu aktualisieren. 

* Benachrichtigung verwandte Entitäten und Eigenschaften: 

    JobNotificationSubscription
    
    NotificationEndPoint
    
    Auftrag

* Asset.Uri 

* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Azure Media Services .NET SDK ändert

Folgende Änderungen enthalten im Juni 2013 Media Services SDK-Versionen. Aktuelle Media Services SDK ist auf GitHub verfügbar.

* Ab Version 2.3.0.0 Media Services SDK unterstützt mehrere Speichergruppen verknüpfen Konten ein Media Services-Konto. Dieses Feature unterstützt die folgenden APIs:
    
    Der IStorageAccount-Typ.
    
    Die Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts-Eigenschaft.
    
    Die StorageAccount-Eigenschaft.
    
    Die StorageAccountName-Eigenschaft.
    
    Weitere Informationen finden Sie unter [Verwalten von Services Medien mehrere Storage-Konten].

* Benachrichtigung bezogene APIs. Beginnend mit der Version 2.2.0.0 haben Azure Queue Storage Benachrichtigungen zuhören. Weitere Informationen finden Sie unter [Behandeln von Media Services Job Benachrichtigungen].
    
    Die Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions-Eigenschaft.
    
    Der Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint-Typ.
    
    Der Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription-Typ.
    
    Der Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection-Typ.
    
    Der Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType-Typ.
    
    Der Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState-Typ.

* Abhängigkeit der Azure-Speicher Client SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).

* Abhängigkeit von OData 5.5 (Microsoft.Data.OData.dll).


##<a id="december_changes_12"></a>Version Dezember 2012

### <a name="dec_12_dotnet_changes"></a>Azure Media Services .NET SDK ändert

* IntelliSense: Fehlenden Intellisense-Dokumentation für viele hinzugefügt

* Microsoft.Practices.TransientFaultHandling.Core: Behoben, hatte das SDK noch eine Abhängigkeit mit einer alten Version dieser Assembly. Das SDK verweist jetzt auf Version 5.1.1209.1 dieser Assembly.

Korrekturen für Probleme im November 2012 SDK:

* IAsset.Locators.Count: Diese Zahl ist jetzt korrekt auf neue IAsset Schnittstellen gemeldet nachdem alle Locators gelöscht wurden.

* IAssetFile.ContentFileSize: Dieser Wert wird nun ordnungsgemäß nach ein Upload von IAssetFile.Upload(filepath) festgelegt.

* IAssetFile.ContentFileSize: Diese Eigenschaft kann jetzt festgelegt werden, wenn eine Bilddatei zu erstellen. Es ist bereits schreibgeschützt.

* IAssetFile.Upload(filepath): Behoben, wurde diese Uploadmethode synchron folgenden Fehler beim Auslösen Hochladen mehrerer Dateien auf die Anlage. Fehler "Server konnte die Anforderung authentifiziert. Stellen Sie sicher, dass der Wert der Authorization-Header korrekt unterschreiben gebildet wird."

* IAssetFile.UploadAsync: Behoben, können nicht mehr als 5 Dateien gleichzeitig hochgeladen werden.

* IAssetFile.UploadProgressChanged: Dieses Ereignis ist jetzt vom SDK bereitgestellt.

* IAssetFile.DownloadAsync (String, BlobTransferClient, ILocator, CancellationToken): Überladung dieser Methode jetzt bereitgestellt.

* IAssetFile.DownloadAsync: Behoben, können nicht mehr als 5 Dateien gleichzeitig heruntergeladen werden.

* IAssetFile.Delete(): Behoben Aufruf von Delete kann, eine Ausnahme auslösen, wenn die IAssetFile keine Datei hochgeladen wurde.

* Aufträge: Behoben, Verketten einer "MP4 reibungslose Streams Vorgang" mit einer "PlayReady Aufgabe" mit einer Stellenvorlage nicht entstehen Aufgaben zu.

* EncryptionUtils.GetCertificateFromStore(): Diese Methode löst nicht mehr eine null-Verweisausnahme aufgrund ein Zertifikat basierend auf Konfigurationsprobleme Zertifikat suchen.

##<a id="november_changes_12"></a>November 2012 Release

In diesem Abschnitt erwähnten Änderung war im November 2012 (Version 2.0.0.0) enthaltene SDK. Diese Änderungen erfordern Code geschrieben für Juni 2012 Preview release SDK geschrieben oder geändert werden.

* Anlagen
    
    IAsset.Create(assetName) dient nur Anlage erstellen. IAsset.Create lädt Dateien nicht mehr als Teil des Methodenaufrufs. Verwenden Sie IAssetFile zum Hochladen.
    
    Die Methode IAsset.Publish und AssetState.Publish-Enumerationswert wurden aus dem SDK Dienste entfernt. Code, der auf diesen Wert muss neu geschrieben werden.

* FileInfo

    Diese Klasse wurde entfernt und durch IAssetFile ersetzt.

    IAssetFiles

    IAssetFile ersetzt FileInfo und weist ein anderes Verhalten. Instanziieren Sie die Verwendung IAssetFiles Objekts, gefolgt von Dateiuploads mit Media Services SDK oder Azure Storage SDK. Die folgenden IAssetFile.Upload Überladung können verwendet werden:

    * IAssetFile.Upload(filePath): Eine synchrone Methode, die den Thread blockiert wird nur empfohlen, wenn eine einzelne Datei hochladen.
    
    * IAssetFile.UploadAsync (FilePath, BlobTransferClient, Locator, CancellationToken): eine asynchrone Methode. Dies ist die bevorzugte Hochladen-Mechanismus. 

    Bekanntes Problem: mit dem CancellationToken bricht tatsächlich hochladen; jedoch möglich Abbruch Status der Aufgaben mehrere Zustände. Sie müssen richtig abfangen und Behandeln von Ausnahmen.

* Locators
    
    Die Ursprungs-Versionen wurden entfernt. SAS-spezifischen Kontext. Locators.CreateSasLocator (Anlage, AccessPolicy) werden veraltet oder durch GA entfernt markiert Siehe Abschnitt Locators unter neue Funktionen für geänderte Verhalten.


##<a id="june_changes_12"></a>Preview-Version von Juni 2012

Die folgende Funktionen wurde in der November-Version des SDK.

* Löschen von Entitäten

    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey Objekte auf Objektebene, d. h. IObject.Delete(), anstatt ein Löschen der Auflistung cloudMediaContext.ObjCollection.Delete(objInstance) wird nun gelöscht.

* Locators

    Locator müssen jetzt CreateLocator Methode erstellt werden und die LocatorType.SAS oder LocatorType.OnDemandOrigin Enumerationswerte als Argument für den bestimmten Locator erstellen möchten.

    Neue Eigenschaften wurden hinzugefügt Locators nutzbare URIs für Ihre Inhalte zu erleichtern. Diese Neugestaltung des Locators sollte zu mehr Flexibilität für zukünftige Erweiterbarkeit durch dritte einfache Bedienung für Media-Clientanwendungen.

* Asynchrone Unterstützung

    Asynchrone Unterstützung wurde auf alle Methoden hinzugefügt.


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN-Forum]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST-API-Referenz]: http://msdn.microsoft.com/library/azure/hh973617.aspx 
[Mediendienste Preisdetails]: http://azure.microsoft.com/pricing/details/media-services/
[Eingeben von Metadaten]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Ausgeben von Metadaten]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Bereitstellung von Content]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Mediendateien mit Azure Media Indexer die Indizierung]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Arbeiten mit Azure Media Services Live-Streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Mithilfe von AES-128 dynamische Verschlüsselung und Schlüssel Lieferservice]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Mithilfe von PlayReady dynamische Verschlüsselung und Lizenz Lieferservice]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Mediendienste PlayReady-Lizenz Vorlage Überblick]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Streaming-Speicher verschlüsselten Inhalt]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure Classic Portal]: https://manage.windowsazure.com
[Dynamische Verpackung]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin Blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Glatte Stream mit PlayReady schützen]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Wiederholungslogik in Media Services SDK für .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Gras Valley kündigt EDIUS 7 durch die Cloud]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Steuerung des Media Service Encoder Ausgabedateinamen]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Erstellen von Overlays]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Stitching Video-Segmente]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 und 3.0.0.2 frei]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Active Directory Azure Access Control Service (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Herstellen einer Verbindung mit Media Services mit Media Services SDK für .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services .NET SDK Extensions]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure-Sdk-tools]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Verwalten von Media Services Ressourcen mehrere Storage-Konten]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Medien Services Job-Benachrichtigung]: http://msdn.microsoft.com/library/azure/dn261241.aspx
 
