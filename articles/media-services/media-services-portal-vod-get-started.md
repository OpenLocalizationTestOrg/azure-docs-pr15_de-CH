<properties
    pageTitle=" Erste Schritte mit Inhalt bei Bedarf mit der Azure-Portal | Microsoft Azure"
    description="Dieses Lernprogramm führt Sie durch die Schritte implementiert einen grundlegenden Video-On-Demand (VoD) Inhalten mit Azure Media Services (AMS) Anwendung Azure-Portal."
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Erste Schritte mit Inhalt bei Bedarf mit der Azure-portal

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Dieses Lernprogramm führt Sie durch die Schritte implementiert einen grundlegenden Video-On-Demand (VoD) Inhalten mit Azure Media Services (AMS) Anwendung Azure-Portal.

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 

Dieses Lernprogramm beinhaltet die folgenden Aufgaben:

1.  Registrieren Sie Azure Media Services.
2.  Streaming-Endpunkt zu konfigurieren.
1.  Hochladen Sie eine Videodatei.
1.  Codieren Sie die Quelldatei in adaptive Bitrate MP4-Dateien.
1.  Veröffentlichen Sie Anlage und Get streaming und progressiven Download URLs.  
1.  Wiedergeben des Inhalts.


## <a name="create-an-azure-media-services-account"></a>Erstellen Sie ein Konto Azure Media Services

Die Schritte in diesem Abschnitt zeigen, wie ein AMS-Konto erstellen.

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com/).
2. Klicken Sie auf **+ Neuer** > **Web + Mobile** > **Media Services**.

    ![Media Services erstellen](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Geben Sie die gewünschten Werte, in **MEDIA SERVICES-Konto erstellen** .

    ![Media Services erstellen](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Geben Sie den Namen des neuen AMS **Kontonamen**. Ein Kontoname Media Services ist alle Kleinbuchstaben Zahlen oder Buchstaben ohne Leerzeichen und 3 bis 24 Zeichen lang ist.
    2. Wählen Sie im Abonnement unter verschiedenen Azure-Abonnements, denen Sie Zugriff haben.
    
    2. **Ressourcengruppe**wählen Sie die neue oder vorhandene Ressource.  Eine Ressourcengruppe ist eine Sammlung von Ressourcen, die Lebenszyklus, Berechtigungen und Richtlinien. Weitere Informationen [hier](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Speicherort**dient Auswählen der Region die Medien- und Metadaten für Ihr Media Services-Konto gespeichert. Dieser Bereich wird zum Verarbeiten und Streamen von Medien. Die verfügbaren Media Services Bereiche werden im Dropdown-Listenfeld angezeigt. 
    
    3. Wählen Sie **Konto**ein Speicherkonto BLOB-Speicher der Medieninhalte von der Media Services-Konto. Auswählen eines vorhandenen Speicherkontos in derselben geographischen Region als Media Services-Konto oder ein Speicherkonto erstellen. Ein neues Speicherkonto wird in derselben Region erstellt. Die Regeln für Namen Speicher entsprechen Media Services-Konten.

        Erfahren Sie mehr über Speicher [hier](storage-introduction.md).

    4. **Pin Dashboard** die Fortschritte der Bereitstellung Konto auswählen
    
7. Klicken Sie am unteren Rand des Formulars **Erstellen** .

    Sobald das Konto erfolgreich erstellt wurde, wird der Status **ausgeführt**. 

    ![Media Services settings](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Das AMS-Konto verwalten (z. B. Hochladen Videos Codieren von Ressourcen, Überwachung des Projektstatus) **Settings** Fenster.

## <a name="manage-keys"></a>Verwalten von Schlüsseln

Sie benötigen den Kontonamen und Primärschlüsselinformationen zum programmgesteuerten Zugriff auf das Media Services-Konto.

1. Wählen Sie Ihr Konto in Azure-Portal. 

    Das Fenster **Einstellungen** auf der rechten Seite. 

2. Wählen Sie im **Einstellungsfenster** **Schlüssel**. 

    Fenster **Verwalten Schlüssel** wird der Name und die primären und sekundären Schlüssel angezeigt. 
3. Drücken Sie die Schaltfläche Kopieren, um die Werte zu kopieren.
    
    ![Media Services-Schlüssel](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="configure-streaming-endpoints"></a>Streaming-Endpunkte konfigurieren

Arbeit mit Azure Media Services, die eines der häufigsten Szenarios adaptive Bitrate streaming Video an die Clients übermittelt. Media Services unterstützt die folgende adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern).

Media Services bietet dynamische Verpackung der adaptive Bitrate codiert MP4 Inhalt streaming-Formate von Media Services (MPEG DASH HLS Smooth Streaming, HDS) just-in-Time, ohne vorkonfigurierte Versionen dieser streaming-Formaten speichern zu kann.

Um dynamische Verpackung nutzen zu können, müssen Sie Folgendes tun:

- Codieren Sie die Aufsteckkarte (Quelle) Datei in adaptive Bitrate MP4-Dateien (Codierung Schritte werden später in diesem Lernprogramm gezeigt).  
- Erstellen Sie mindestens eine Streaming-Einheit für die *streaming-Endpunkt* aus der Lieferung des Inhalts werden soll. Schritte anzeigen zum Ändern der Anzahl der Streaming-Einheiten

Dynamische Verpackung müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen mit Media Services erstellt und die entsprechende Antwort auf Anfragen von einem Client fungiert.

Um die Anzahl der reservierte Einheiten streaming zu erstellen, führen Sie folgende Schritte aus:


1. Klicken Sie im Fenster **Eigenschaften** auf **Streaming-Endpunkte**. 

2. Klicken Sie auf die Standardeinstellungen für streaming-Endpunkt. 

    **STREAMING ENDPUNKTDATEN STANDARDMÄßIG** angezeigt.

3. Ziehen Sie zum Angeben der Streaming-Einheiten Schieberegler **Streaming-Einheiten** .

    ![Streaming-Einheiten](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klicken Sie auf **Speichern** , um die Änderung zu speichern.

    >[AZURE.NOTE]Die Zuordnung neuer Einheiten kann bis zu 20 Minuten dauern.

## <a name="upload-files"></a>Dateien hochladen

Stream Videos mithilfe von Azure Media Services müssen Quellvideos hochladen, in mehreren Bitraten codiert und veröffentlichen das Ergebnis. Zunächst wird in diesem Abschnitt behandelt. 

1. Klicken Sie im Fenster **Einstellung** auf **Ressourcen**.

    ![Dateien hochladen](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. Klicken Sie auf die Schaltfläche **Hochladen** .

    Das **Hochladen eines video-Assets** angezeigt.

    >[AZURE.NOTE] Es gibt keine Begrenzung der Größe.
    
4. Suchen Sie das gewünschte Video auf Ihrem Computer, wählen Sie aus, und drücken Sie OK.  

    Der Upload beginnt und den Fortschritt unter dem Dateinamen angezeigt.  

Sobald der Upload abgeschlossen ist, sehen Sie die neue Anlage im Fenster **Anlagen** aufgelistet. 

## <a name="encode-assets"></a>Codieren von Anlagen

Arbeit mit Azure Media Services, die eines der häufigsten Szenarios adaptives streaming Bitrate an die Clients übermittelt. Media Services unterstützt die folgende adaptive Bitrate streaming Technologie: http-Live-Streaming (HLS), Smooth Streaming MPEG DASH und HDS (für Adobe PrimeTime/Access nur Lizenznehmern). Um Videos für adaptive Bitrate streaming vorbereiten, müssen Sie das Quellvideo in Dateien Multi-Bitrate codiert. Verwenden Sie **Media Encoder Standard** Encoder Codieren von Videos.  

Media Services bietet auch dynamische Verpackung zu Ihrem Multi-Bitrate MP4s in folgenden streaming-Formate kann: MPEG DASH HLS, Smooth Streaming oder HDS ohne diese Streaming-Formate erneut packen. Dynamische Verpackung müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen mit Media Services erstellt und die entsprechende Antwort auf Anfragen von einem Client fungiert.

Um dynamische Verpackung nutzen zu können, müssen Sie Folgendes tun:

- Codieren Sie die Quelldatei in Multi-Bitrate MP4-Dateien (Codierung Schritte werden weiter unten in diesem Abschnitt gezeigt).
- Rufen Sie mindestens eine Streaming-Einheit für Streaming-Endpunkt aus dem Lieferung des Inhalts werden soll. Weitere Informationen finden Sie unter [streaming Endpunkte konfigurieren](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

### <a name="to-use-the-portal-to-encode"></a>Mit dem Portal zu codieren

Dieser Abschnitt beschreibt die Schritte, mit denen Sie Ihre Inhalte mit Media Encoder Standard codieren.

1.  Wählen Sie im **Einstellungsfenster** **Anlagen**.  
2.  Wählen Sie im Fenster **Anlagen** Anlage codieren möchten.
3.  Drücken Sie die **Codierung** .
4.  **Codieren einer Anlage** im Fenster "Media Encoder Standard" Prozessor und Auswählen einer Voreinstellung Beispielsweise wenn Sie input Video hat eine Auflösung von 1920 x 1080 Pixel kennen, dann können die "H264 mehrere Bitrate 1080p" voreingestellte. Weitere Informationen über Voreinstellungen finden Sie [diese](https://msdn.microsoft.com/library/azure/mt269960.aspx) Artikel – unbedingt Vorgaben auswählen, die für die Eingabe video am besten geeignet ist. Wenn Sie ein Video mit niedriger Auflösung (640 x 360), Sie sollten nicht die Standardeinstellung verwenden "H264 mehrere Bitrate 1080p" voreingestellte.
    
    Zur einfacheren Verwaltung können Sie den Namen der Anlage Ausgabe, und der Name des Auftrags.
        
    ![Codieren von Anlagen](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Drücken Sie **zu erstellen**.

### <a name="monitor-encoding-job-progress"></a>Codierung Projekt überwachen

Zum Überwachen des Codierungsauftrags **Settings** (am oberen Rand der Seite) auf und wählen Sie **Aufträge**.

![Aufträge](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Veröffentlichen

Um Ihre Benutzer mit einer URL angeben, das zum Streamen oder Ihre Inhalte herunterladen, müssen Sie "Ihrer Ressource veröffentlichen" einen Locator erstellen. Locators ermöglichen den Zugriff auf Dateien in der Anlage. Media Services unterstützt zwei Arten von Locators: 

- Streaming (OnDemandOrigin)-Locators für adaptives streaming (z. B. auf Streams MPEG DASH HLS oder Smooth Streaming) verwendet. Erstellen ein Streaming-Locators die Anlage muss eine ISM-Datei enthalten. 
- Progressiv (SAS)-Locators, zur Bereitstellung des Videos progressives Herunterladen.


Streaming-URL hat das folgende Format und Sie können Anlagen Smooth Streaming wiedergeben.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Erstellen Sie eine streaming-URL HLS anfügen (Format = m3u8 Aapl) an den URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Erstellen Sie ein MPEG-DASH streaming URL anhängen (Format = Mpd-Zeit-csf) an den URL.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


SAS-URL hat das folgende Format.

    {blob container name}/{asset name}/{file name}/{SAS signature}

>[AZURE.NOTE] Wenn Sie das Portal Locators vor März 2015 erstellt, wurden Locator mit einem zweijährigen Ablaufdatum erstellt.  

Aktualisieren Sie ein Ablaufdatum ein Locator verwenden Sie [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) oder [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) APIs. Wenn Sie das Ablaufdatum des SAS-Locator aktualisieren, wird der URL.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Mit dem Portal Vermögenswert veröffentlichen

Um das Portal zu Vermögenswert veröffentlichen, führen Sie folgende Schritte aus:

1. **Wählen Sie in** > **Elemente**.
1. Wählen Sie die Anlage, die Sie veröffentlichen möchten.
1. Klicken Sie auf die Schaltfläche **Veröffentlichen** .
1. Wählen Sie den Locator.
2. Drücken Sie **Hinzufügen**.

    ![Veröffentlichen](./media/media-services-portal-vod-get-started/media-services-publish1.png)

Die Liste der **URLs veröffentlicht**die URL hinzugefügt.

## <a name="play-content-from-the-portal"></a>Wiedergeben von Inhalten aus dem portal

Azure-Portal bietet einen Content-Player, mit denen Sie das Video zu testen.

Klicken Sie auf das gewünschte Video, und klicken Sie dann auf die Schaltfläche **Wiedergabe** .

![Veröffentlichen](./media/media-services-portal-vod-get-started/media-services-play.png)

Einige zu berücksichtigen:

- Stellen Sie sicher, dass das Video veröffentlicht wurde.
- **MediaPlayer** spielt den Standardwert streaming-Endpunkt. Wenn Sie von einem nicht standardmäßigen streaming-Endpunkt wiedergeben möchten, klicken Sie kopieren Sie die URL und einen anderen Player. Z. B. [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

##<a name="next-steps"></a>Nächste Schritte

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


