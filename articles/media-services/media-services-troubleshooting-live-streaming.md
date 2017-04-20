<properties 
    pageTitle="Fehlerbehebung für Livestreaming | Microsoft Azure" 
    description="Dieses Thema enthält Vorschläge zum live-Streaming-Problemen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016"  
    ms.author="juliako"/>

#<a name="troubleshooting-guide-for-live-streaming"></a>Fehlerbehebung für live-streaming

Dieses Thema enthält Vorschläge zum live-Streaming-Problemen.

## <a name="issues-related-to-on-premises-encoders"></a>Probleme mit lokalen Encoder 

Dieser Abschnitt enthält Vorschläge zur Problembehandlung bei Problemen im Zusammenhang mit lokalen Encoder, die einen einzelne Bitrate Stream AMS Kanäle senden, für die live-Codierung aktiviert, konfiguriert.

###<a name="problem-would-like-to-see-logs"></a>Problem: Wollen Protokolle 

- **Mögliches Problem**: kann nicht finden Encoder anmeldet, können Probleme beim Debuggen.
    
    - **Telestream Wirecast**: finden Sie in der Regel Protokolle unter C:\Users\{Benutzername} \AppData\Roaming\Wirecast\ 
    - **Elementare Live**: verbindet Protokollen auf dem Management-Portal finden. Klicken Sie auf **Statistiken**und **Protokollen**. Auf der Seite **Dateien** sehen Sie eine Liste der Protokolle für die LiveEvent Elemente. Wählen Sie die mit der aktuellen Sitzung. 
    - **Flash Media Live Encoder**: finden Sie das **Verzeichnis...** durch Navigieren zur Registerkarte **Codierung Protokoll** .
    
###<a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Problem: Es gibt keine Option zum progressiven Stream ausgeben

- **Mögliches Problem**: verwendete Encoder nicht automatisch zusammenfügen. 

    **Fehlerbehebung**: Deinterlacing Option Encoder Benutzeroberfläche suchen. Sobald Deinterlacing aktiviert ist, überprüfen Sie erneut für progressive Ausgabeoptionen. 
 
###<a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Problem: Versucht mehrere Encoder-Ausgabeoptionen und noch keine Verbindung herstellen. 

- **Mögliches Problem**: Azure Codierung Kanal wurde nicht richtig zurückgesetzt. 

    **Fehlerbehebung**: Sicherstellen der Encoder nicht mehr AMS drücken, beenden und Zurücksetzen des Kanals. Sobald wieder, versuchen Sie, die des Encoders bei. Wenn dies das Problem noch nicht behoben wird, können erstellen Sie einen neuen Kanal vollständig, manchmal Kanäle beschädigte nach mehreren fehlgeschlagenen versuchen.  

- **Mögliches Problem**: die GOP-Größe bzw. Keyframes sind nicht optimal. 

    **Fehlerbehebung**: empfohlen GOP-Größe oder Keyframe Intervall beträgt 2 Sekunden. Einige Encoder berechnet diese Einstellung in Bildern, während andere Sekunden. Beispiel: bei der Ausgabe von 30 fps GOP-Größe wäre 60 Frames entspricht 2 Sekunden.  
     
- **Mögliches Problem**: geschlossenen Ports blockiert den Stream. 

    **Fehlerbehebung**: beim streaming über RTMP überprüfen Firewall oder Proxy Settings bestätigen 1935 und 1936 ausgehende Ports geöffnet sind. RTP-streaming verwenden, sicher, dass ausgehender Port 2010 geöffnet ist. 


###<a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>Problem: Beim Konfigurieren des Encoders Stream mit RTP-Protokoll findet keine Hostnamen eingeben. 

- **Mögliches Problem**: viele RTP Encoder lassen keine Hostnamen und eine IP-Adresse müssen erworben werden.  

    **Fehlerbehebung**: zum Suchen der IP-Adresse öffnen Sie ein Eingabeaufforderungsfenster auf einem Computer. Setzen Sie hierzu in Windows ausführen Launcher (WIN + R) öffnen, und geben Sie "Cmd" zu öffnen.  

    Sobald das Eingabeaufforderungsfenster geöffnet ist, geben Sie "Ping [AMS Hostname]". 

    Der Hostname kann durch Auslassen der Portnummer aus dem Azure Aufnahme URL, wie im folgenden Beispiel abgeleitet werden: 

    RTP://Test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 / 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

###<a name="problem-unable-to-playback-the-published-stream"></a>Problem: Nicht Wiedergabe veröffentlichten Datenstroms.
 
- **Mögliches Problem**: Gibt es kein Streaming-Endpunkt ausgeführt oder keine Streaming-Einheiten (Maßeinheiten) zugeordnet ist. 

    **Fehlerbehebung**: Navigieren Sie zur Registerkarte "Streaming-Endpunkt" AMSE Tool und bestätigt wird, dass ein Streaming-Endpunkt mit Streaming-Einheit. 
    


>[AZURE.NOTE] Nachdem die Schritte zur Problembehandlung, die Sie weiterhin erfolgreich übertragen können, ein Ticket Support über das Azure-Portal.

##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
