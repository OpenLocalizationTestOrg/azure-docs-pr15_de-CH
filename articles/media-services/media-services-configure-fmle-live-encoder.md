<properties 
    pageTitle="Konfigurieren den FMLE Encoder eine einzelne Bitrate live senden | Microsoft Azure" 
    description="Dieses Thema zeigt konfigurieren Encoder Flash Media Live Encoder (FMLE) einen einzigen Bitrate Stream AMS Kanäle senden, die für die Codierung von live aktiviert sind." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="juliako;cenkdin;anilmur"/>

#<a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a>Verwenden Sie FMLE Encoder, um einen einzigen Bitrate Livestream senden

> [AZURE.SELECTOR]
- [FMLE](media-services-configure-fmle-live-encoder.md)
- [Elementare Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)

Dieses Thema zeigt konfigurieren Encoder [Flash Media Live Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) einen einzigen Bitrate Stream AMS Kanäle senden, die für die Codierung von live aktiviert sind. Weitere Informationen finden Sie unter [Arbeiten mit Channels, die zum Ausführen mit Azure Media Services Livecodierungsmodus aktiviert sind](media-services-manage-live-encoder-enabled-channels.md).

In diesem Lernprogramm wird gezeigt, wie Azure Media Services (AMS) mit Azure Media Services Explorer (AMSE) verwaltet wird. Dieses Tool kann nur auf Windows-PC ausgeführt. Mac oder Linux hingegen mit der Azure-Portal [Kanäle](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) und [Programme](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program)erstellt.

Beachten Sie, dass diese praktische Einführung beschreibt AAC verwenden. Aber unterstützt FMLE nicht AAC standardmäßig. Müssen Sie ein Plug-In für AAC-Codierung z. B. MainConcept erwerben: [AAC-Plugin](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

##<a name="prerequisites"></a>Erforderliche Komponenten

- [Erstellen Sie ein Konto Azure Media Services](media-services-portal-create-account.md)
- Stellen Sie sicher, einen Streaming-Endpunkt mit mindestens eine Streaming-Einheit zugeordnet ist. Weitere Informationen finden Sie unter [Streaming Endpunkte in ein Media Services-Konto verwalten](media-services-portal-manage-streaming-endpoints.md)
- Installieren Sie die neueste Version des Tools [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) .
- Starten Sie das Tool und die AMS-Konto verbinden.

##<a name="tips"></a>Tipps

- Verwenden Sie nach Möglichkeit eine verdrahtete Verbindung.
- Ist eine Faustregel Bandbreitenbedarf bei streaming Bitraten doppelklicken. Obwohl dies nicht zwingend erforderlich ist, hilft der Auswirkungen Netzwerkstau.
- Schließen Sie Encoder mit Software basiert, alle nicht benötigten Programme.

## <a name="create-a-channel"></a>Erstellen eines Kanals

1.  Navigieren Sie im Tool AMSE zur Registerkarte **Live** und rechts im Bereich Channel auf. Wählen Sie **Channel erstellen...** Menü.

![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Geben Sie einen Channelnamen Beschreibungsfeld ist optional. Einstellungen wählen Sie **Standard** für die Option Livecodierungsmodus mit dem Input-Protokoll **RTMP**. Lassen Sie alle anderen Einstellungen wie ein.


Sicherstellen Sie, dass **nun den neuen Kanal starten** aktiviert ist.

3. Klicken Sie auf **Channel**.
![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

>[AZURE.NOTE] Der Kanal kann 20 Minuten dauern.


Beim Starten des Kanals können Sie [den Encoder konfigurieren](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).

>[AZURE.IMPORTANT] Beachten Sie, dass die Abrechnung beginnt Kanal in einen bereiten Zustand wechselt. Weitere Informationen finden Sie unter [Status des Kanals](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_fmle_rtmp></a>FMLE Encoder konfigurieren

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


###<a name="configuration-steps"></a>Konfigurationsschritte

1. Navigieren Sie zu der Flash Media Live Encoder (FMLE) auf dem Computer verwendete Schnittstelle.

    Die Schnittstelle ist eine Hauptseite Einstellungen. Beachten Sie die folgenden Standardeinstellungen für streaming mit FMLE.
    
    - Format: H. 264-Framerate: 30,00 
    - Größe: 1280 x 720 
    - Bitrate: 5000 Kbit/s (basierend auf einer eingeschränkten Netzwerk einstellbar)  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

    Quellen mit Zeilensprung, bitte Häkchen die Option ""Zusammenfügen ""

2. Schraubenschlüssel-Symbol neben Format wählen, diese Berechtigungen:

    - Profil: Hauptmenü
    - Ebene: 4.0
    - Keyframe-Häufigkeit: 2 Sekunden 
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)

3. Legen Sie die folgende wichtige audio-Einstellung:
    
    - Format: AAC 
    - Abtastrate: 44100 Hz
    - Bitrate: 192 Kbit/s
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)

6. Abrufen des Kanals eingegebene URL um die FMLE **RTMP-Endpunkt**zuzuordnen.
    
    Navigieren an das AMSE-Tool und der Abschlussstatus Kanal aktivieren. Sobald der Zustand von **Starten** **ausgeführt**geändert hat, erhalten Sie die eingegebene URL.
      
    Ausführung des Kanals klicken Sie mit der rechten Maustaste den Kanalnamen, navigieren Sie auf Hover über **Eingabe-URL in Zwischenablage kopieren** und dann **Primäre Eingabe URL**.  
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)

7. Fügen Sie diese Informationen im Feld **FMS-URL** von der Ausgabe, und weisen Sie ein Stream. 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    Wiederholen Sie diese Schritte mit der zweiten Eingabe URL für zusätzliche Redundanz.
8. Wählen Sie **Verbinden**.

>[AZURE.IMPORTANT]Bevor Sie **Verbinden**klicken, sicherstellen **müssen** Sie, dass der Kanal bereit. 
>Auch Stellen Sie sicher, dass den Kanal ohne eine Eingabe feed > 15 Minuten länger betriebsbereit bleibt.

##<a name="test-playback"></a>Testwiedergabe
  
1. Navigieren Sie zu AMSE-Tool und rechts auf Kanal getestet werden. Im Menü **Wiedergabe der Vorschau** Mauszeiger und **Azure Media Player**auswählen.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Wenn der Stream im Player angezeigt wird, wurde dann Encoder ordnungsgemäß konfiguriert AMS herstellen. 

Wenn eine Fehlermeldung der Kanal zurückgesetzt werden müssen und Encoder-Einstellungen. Finden Sie im Thema [zur Problembehandlung](media-services-troubleshooting-live-streaming.md) Anleitung.  

##<a name="create-a-program"></a>Erstellen Sie ein Programm

1. Nach Bestätigung Kanal Wiedergabe erstellen. Registerkarte **Live** im Tool AMSE rechts im Programmbereich auf, und wählen Sie **Neu erstellen**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)

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
