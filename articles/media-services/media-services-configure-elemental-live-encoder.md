<properties 
    pageTitle="Konfigurieren den elementaren Live Encoder eine einzelne Bitrate live senden | Microsoft Azure" 
    description="Dieses Thema zeigt konfigurieren elementaren Live Encoder Stream einzelne Bitrate AMS Kanäle senden, die für die Codierung von live aktiviert sind." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="cenkdin;anilmur;juliako"/>

#<a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>Verwenden der elementaren Live Encoder an einen einzigen Bitrate Livestream

> [AZURE.SELECTOR]
- [Elementare Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Dieses Thema zeigt konfigurieren [Elementaren Live](http://www.elementaltechnologies.com/products/elemental-live) Encoder Stream einzelne Bitrate AMS Kanäle senden, die für die Codierung von live aktiviert sind.  Weitere Informationen finden Sie unter [Arbeiten mit Channels, die zum Ausführen mit Azure Media Services Livecodierungsmodus aktiviert sind](media-services-manage-live-encoder-enabled-channels.md).

In diesem Lernprogramm wird gezeigt, wie Azure Media Services (AMS) mit Azure Media Services Explorer (AMSE) verwaltet wird. Dieses Tool kann nur auf Windows-PC ausgeführt. Mac oder Linux hingegen mit der Azure-Portal [Kanäle](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) und [Programme](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)erstellt.

##<a name="prerequisites"></a>Erforderliche Komponenten

- Sie müssen Kenntnisse über elementare Live Internet live Ereignisse erstellen.
- [Erstellen Sie ein Konto Azure Media Services](media-services-portal-create-account.md)
- Stellen Sie sicher, einen Streaming-Endpunkt mit mindestens eine Streaming-Einheit zugeordnet ist. Weitere Informationen finden Sie unter [Streaming Endpunkte in ein Media Services-Konto verwalten](media-services-portal-manage-streaming-endpoints.md).
- Installieren Sie die neueste Version des Tools [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Starten Sie das Tool und die AMS-Konto verbinden.

##<a name="tips"></a>Tipps

- Verwenden Sie nach Möglichkeit eine verdrahtete Verbindung.
- Ist eine Faustregel Bandbreitenbedarf bei streaming Bitraten doppelklicken. Obwohl dies nicht zwingend erforderlich ist, hilft der Auswirkungen Netzwerkstau.
- Schließen Sie Encoder mit Software basiert, alle nicht benötigten Programme.

## <a name="elemental-live-with-rtp-ingest"></a>Elementare Live mit RTP aufnehmen

Dieser Abschnitt veranschaulicht die elementaren Live Encoder konfigurieren, der einen einzelnen Bitrate Livestream über RTP gesendet.  Weitere Informationen finden Sie unter [Stream über RTP MPEG-TS](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Erstellen eines Kanals

1.  Navigieren Sie im Tool AMSE zur Registerkarte **Live** und rechts im Bereich Channel auf. Wählen Sie **Channel erstellen...** Menü.

![Elementare](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Geben Sie einen Channelnamen Beschreibungsfeld ist optional. Channeleinstellungen wählen Sie **Standard** für die Option Livecodierungsmodus mit dem Protokoll Eingabe RTP **(MPEG-TS)**. Lassen Sie alle anderen Einstellungen wie ein.


Sicherstellen Sie, dass **nun den neuen Kanal starten** aktiviert ist.

3. Klicken Sie auf **Channel**.
![Elementare](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

>[AZURE.NOTE] Der Kanal kann 20 Minuten dauern.

Beim Starten des Kanals können Sie [den Encoder konfigurieren](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

>[AZURE.IMPORTANT] Beachten Sie, dass die Abrechnung beginnt Kanal in einen bereiten Zustand wechselt. Weitere Informationen finden Sie unter [Status des Kanals](media-services-manage-live-encoder-enabled-channels.md#states).

###<a id=configure_elemental_rtp></a>Konfigurieren des elementaren Live-Encoders 

In diesem Lernprogramm werden die folgenden Ausgabeoptionen verwendet. Der Rest dieses Abschnitts werden die Konfigurationsschritte ausführlicher beschrieben. 

**Video**:
 
- Codec: h. 264 
- Profil: Hoch (Ebene 4.0) 
- Bitrate: 5000 Kbit/s 
- Schlüsselbild: 2 Sekunden (60 Sekunden) 
- Bildrate: 30
 
**Audio**:

- Codec: AAC (LC) 
- Bitrate: 192 Kbit/s 
- Abtastrate: 44,1 kHz


####<a name="configuration-steps"></a>Konfigurationsschritte

1. Auf die Weboberfläche **Elementaren Live** navigieren und Encoder für streaming- **UDP-TS** einrichten. 

2. Nach der Erstellung eines neuen Ereignisses der Ausgabegruppen Scrollen Sie und fügen Sie der Ausgabegruppe **UDP-TS hinzu** . 

3. Erstellen Sie eine neue Ausgabe durch **neue** auswählen und dann auf **Ausgabe hinzufügen**.  
    
    ![Elementare](./media/media-services-elemental-live-encoder/media-services-elemental13.png)
    
    >[AZURE.NOTE] Es wird empfohlen, elementare Ereignis Timecode auf "Uhr" können bei einem Stream schließen Encoder festgelegt hat.

4. Die Ausgabe erstellt wurde, klicken Sie auf **Stream**. Die Ausgabeoptionen können jetzt konfiguriert werden. 
5. Die "Stream 1" die soeben erstellte Scrollen Sie, klicken Sie auf die Registerkarte **Video** auf der linken Seite und erweitern Sie den Abschnitt **Advanced** Settings. 

    ![Elementare](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Zwar elementares Live eine Vielzahl von verfügbaren anpassen, sollten Folgendes Einstieg in AMS streaming. 
    
    - Auflösung: 1280 x 720 
    - Bildrate: 30 
    - GOP-Größe: 60 frames 
    - Interlace-Modus: Progressive 
    - Bitrate: 5000000 Bit/s (Dies kann basierend auf einer eingeschränkten Netzwerk angepasst werden) 
    

    ![Elementare](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

6. Der Kanal input URL abrufen.
    
    Navigieren an das AMSE-Tool und der Abschlussstatus Kanal aktivieren. Sobald der Zustand von **Starten** **ausgeführt**geändert hat, erhalten Sie die eingegebene URL.
      
    Ausführung des Kanals klicken Sie mit der rechten Maustaste den Kanalnamen, navigieren Sie auf Hover über **Eingabe-URL in Zwischenablage kopieren** und dann **Primäre Eingabe URL**.  
    
    ![Elementare](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
    
1. Fügen Sie diese Informationen im Feld **Primärziel** der elementaren. Alle anderen Einstellungen bleiben die Standardeinstellung.
    
    ![Elementare](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Wiederholen Sie diese Schritte mit der zweiten Eingabe URL für zusätzliche Redundanz durch Erstellen einer separaten Registerkarte "Ausgabe" für Streaming-UDP-TS.
    
7. Klicken Sie auf **Erstellen** (wenn ein neues Ereignis erstellt wurde) oder **Aktualisieren** (ein bereits vorhandenes Ereignis bearbeiten) und dann den Encoder starten. 

>[AZURE.IMPORTANT]Bevor Sie auf die Weboberfläche elementaren Live **Starten** klicken, sicherstellen **müssen** Sie, dass der Kanal bereit. 
>Auch dürfen Sie keine den Kanal ohne ein Ereignis > 15 Minuten länger betriebsbereit bleibt.

Wechseln Sie Stream für 30 Sekunden ausgeführt wurde, auf die Wiedergabe von AMSE Tool und Test.  

###<a name="test-playback"></a>Testwiedergabe
  
1. Navigieren Sie zu AMSE-Tool und rechts auf Kanal getestet werden. Im Menü **Wiedergabe der Vorschau** Mauszeiger und **Azure Media Player**auswählen.  

    ![Elementare](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Wenn der Stream im Player angezeigt wird, wurde dann Encoder ordnungsgemäß konfiguriert AMS herstellen. 

Wenn eine Fehlermeldung der Kanal zurückgesetzt werden müssen und Encoder-Einstellungen. Finden Sie im Thema [zur Problembehandlung](media-services-troubleshooting-live-streaming.md) Anleitung.   

###<a name="create-a-program"></a>Erstellen Sie ein Programm

1. Nach Bestätigung Kanal Wiedergabe erstellen. Registerkarte **Live** im Tool AMSE rechts im Programmbereich auf, und wählen Sie **Neu erstellen**.  

    ![Elementare](./media/media-services-elemental-live-encoder/media-services-elemental9.png)

2. Nennen Sie die Anwendung, und passen Sie ggf. die **Archivgröße Fenster** (standardmäßig auf 4 Stunden). Sie können anzugeben, oder behalten Sie die Standardeinstellung.  
3. Aktivieren Sie das **Programm jetzt starten** .
4. Klicken Sie auf **Programm**.  
  
    Hinweis: Die Erstellung schneller als Kanal erstellen.    
 
5. Sobald die Anwendung ausgeführt wird, bestätigen Sie Wiedergabe Rechtsklick Programms Navigieren zur **Wiedergabe der Programme** und **Azure Media Player**auswählen.  
6. Sobald bestätigt, rechts klicken Sie auf das Programm erneut und wählen Sie **den Ausgabe-URL in Zwischenablage kopieren** oder rufen Sie diese Informationen im Menü die Option **Programm Informationen ab** . 

Der Stream ist jetzt in einen Player eingebettet oder auf eine Zielgruppe für die live-Anzeige verteilt werden.  

## <a name="troubleshooting"></a>Problembehandlung

Finden Sie im Thema [zur Problembehandlung](media-services-troubleshooting-live-streaming.md) Anleitung. 


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
