<properties 
    pageTitle="Wie führen Sie live-streaming über das Azure-Portal mit lokalen | Microsoft Azure" 
    description="Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen eines Kanals für eine Pass-Through-Bereitstellung konfiguriert ist." 
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
    ms.topic="get-started-article"
    ms.date="10/24/2016" 
    ms.author="juliako"/>


#<a name="how-to-perform-live-streaming-with-on-premise-encoders-using-the-azure-portal"></a>Live-streaming über das Azure-Portal mit lokalen durchführen

> [AZURE.SELECTOR]
- [Portal]( media-services-portal-live-passthrough-get-started.md)
- [.NET]( media-services-dotnet-live-encode-with-onpremises-encoders.md)
- [REST]( https://msdn.microsoft.com/library/azure/dn783458.aspx)

Dieses Lernprogramm führt Sie durch die Schritte auf der Azure-Portal einen **Kanal** erstellen, die für eine Pass-Through-Bereitstellung konfiguriert ist. 

##<a name="prerequisites"></a>Erforderliche Komponenten

Die folgenden müssen das Lernprogramm:

- Ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 
- Media Services-Konto. Zum Erstellen eines Kontos Media Services finden Sie unter [Erstellen eines Kontos Media Services](media-services-portal-create-account.md).
- Eine Webcam. Z. B. [Telestream Wirecast Encoder](http://www.telestream.net/wirecast/overview.htm).

Es wird dringend empfohlen, die folgenden Artikel lesen:

- [Azure Media Services RTMP und Live Encoder](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
- [Übersicht über Live gekocht mit Azure Media Services](media-services-manage-channels-overview.md)
- [Live-streaming mit lokalen, die Multi-Bitrate Streams erstellen](media-services-live-streaming-with-onprem-encoders.md)


##<a id="scenario"></a>Häufig für live-streaming

Die folgenden Schritte beschreiben die Aufgaben beim Erstellen von live-Streaming-Anwendung, die Kanäle konfiguriert sind für Pass-Through-Bereitstellung verwenden. Dieses Lernprogramm veranschaulicht eine Pass-Through-Kanal und live-Events.

1. Eine Videokamera an einen Computer anschließen Starten und Konfigurieren von lokalen live Encoder, der einen Multi-Bitrate RTMP oder fragmentiert MP4-Stream ausgegeben. Weitere Informationen finden Sie unter [Azure Media Services RTMP-Unterstützung und Live-Encoder](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Dieser Schritt kann auch nach den Kanal erstellen durchgeführt werden.

1. Erstellen Sie und starten Sie einen Pass-Through-Kanal.
1. Abrufen der Kanal Aufnahme URL. 

    Die Erfassung URL live Encoder dient den Stream an den Kanal senden.
1. Channel-Vorschau-URL abzurufen. 

    Verwenden Sie diese URL, um sicherzustellen, dass der Kanal richtig live-Streams empfangen.

3. Erstellen einer live Veranstaltungsprogramm. 

    Verwendung das Azure-Portal erstellt ein live-Ereignis erstellen auch eine Anlage. 
      
    >[AZURE.NOTE]Müssen Sie mindestens eine reservierte Einheit auf Streaming-Endpunkt zum Streaming von Inhalten soll streaming.
1. Starten Sie/Programm jetzt streaming und Archivierung.
2. Optional kann live Encoder eine Ankündigung signalisiert. Die Ankündigung wird in den Ausgabestream.
1. Beenden Sie das Veranstaltungsprogramm streaming und Archivierung das Ereignis beenden möchten.
1. Löschen Sie das Veranstaltungsprogramm und optional löschen Sie die Anlage.     

>[AZURE.IMPORTANT] Lesen Sie über Konzepte und Aspekte im Zusammenhang mit live-streaming mit lokalen Encoder Pass [Livestreaming mit lokalen, die Multi-Bitrate Streams erstellen](media-services-live-streaming-with-onprem-encoders.md) .

##<a name="to-view-notifications-and-errors"></a>Benachrichtigung und Fehler anzeigen

Wenn Sie Benachrichtigungen und Fehlern Azure-Portal anzeigen möchten, klicken Sie auf das Symbol.

![Benachrichtigung](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

##<a name="configure-streaming-endpoints"></a>Streaming-Endpunkte konfigurieren 

Media Services bietet dynamische Verpackung zu Ihrem Multi-Bitrate MP4s in folgenden streaming-Formate kann: MPEG DASH HLS, Smooth Streaming oder HDS ohne diese Streaming-Formate erneut packen. Dynamische Verpackung müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen mit Media Services erstellt und die entsprechende Antwort auf Anfragen von einem Client fungiert.

Um dynamische Verpackung nutzen zu können, müssen Sie mindestens eine Streaming-Einheit für den Streaming-Endpunkt aus der Lieferung des Inhalts werden soll.  

Um die Anzahl der reservierte Einheiten streaming zu erstellen, führen Sie folgende Schritte aus:

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com/).
1. Klicken Sie im Fenster **Eigenschaften** auf **Streaming-Endpunkte**. 

2. Klicken Sie auf die Standardeinstellungen für streaming-Endpunkt. 

    **STREAMING ENDPUNKTDATEN STANDARDMÄßIG** angezeigt.

3. Ziehen Sie zum Angeben der Streaming-Einheiten Schieberegler **Streaming-Einheiten** .

    ![Streaming-Einheiten](./media/media-services-portal-passthrough-get-started/media-services-streaming-units.png)

4. Klicken Sie auf **Speichern** , um die Änderung zu speichern.

    >[AZURE.NOTE]Die Zuordnung neuer Einheiten kann bis zu 20 Minuten dauern.
    
##<a name="create-and-start-pass-through-channels-and-events"></a>Erstellen und Starten von Pass-Through-Kanäle und Ereignisse

Ein Kanal ist mit Ereignissen/Programme, mit denen Sie das Veröffentlichen und Speichern von Segmenten in live-Streams steuern. Kanäle verwalten Ereignisse. 
    
Sie können die Anzahl der Stunden, den aufgezeichneten Inhalt für das Programm das **Fenster Archiv** Länge beibehalten möchten. Dieser Wert kann auf maximal 25 Stunden ab 5 Minuten festgelegt. Archiv-Fensterlänge schreibt auch Kunden maximal von der aktuellen Position live zurück suchen kann. Ereignisse können über den angegebenen Zeitraum ausgeführt, jedoch hinter der Fensterlänge liegt Inhalt kontinuierlich verworfen. Der Wert dieser Eigenschaft bestimmt auch, wie lange der Client Manifeste werden.

Jedes Ereignis ist mit einer Anlage. Um das Ereignis zu veröffentlichen, müssen Sie einen Locator OnDemand für die zugeordnete Anlage erstellen. Diese Locator ermöglicht eine Streaming-URL erstellen, die für Clients bereitgestellt werden können.

Ein Kanal unterstützt bis zu drei gleichzeitig ausgeführte Ereignisse Erstellen mehrerer Archive dem eingehenden Stream. Dadurch können Sie veröffentlichen und Archivierung von Bestandteilen eines Ereignisses. Beispielsweise ist die Anforderung eines Programms von 6 Stunden archiviert aber nur 10 Minuten. Dazu müssen Sie zwei Programme gleichzeitig erstellen. Ein Programm zum Archivieren von 6 Stunden des Ereignisses festgelegt, aber die Anwendung veröffentlicht. Das Programm für 10 Minuten archivieren und dieses Programm veröffentlicht.

Vorhandene live Ereignisse sollten Sie nicht wieder verwenden. Stattdessen erstellen und Starten eines neuen Ereignisses für jedes Ereignis.

Starten Sie das Ereignis, wenn Sie streaming und Archivierung bereit sind. Beenden Sie das Programm jederzeit beenden streaming und Archivierung das Ereignis. 

Archivierten Inhalt löschen beenden Sie und löschen Sie das Ereignis und anschließend die zugeordnete Anlage. Anlage kann nicht gelöscht werden, wird durch ein Ereignis. das Ereignis muss zuerst gelöscht werden. 

Auch nach dem Beenden und das Ereignis löschen wäre die Benutzer Ihre archivierten Content als Video on Demand für Streaming als Anlage nicht zu löschen.

Soll den archivierten Inhalt beibehalten, aber nicht verfügbar für streaming, löschen Sie streaming locator

###<a name="to-use-the-portal-to-create-a-channel"></a>Das Portal mit Erstellen eines Kanals 

Dieser Abschnitt zeigt, wie mit der Option **Schnell erstellen** einer Pass-Through-Kanal.

Weitere Informationen zu Pass-Through-Kanäle finden Sie [Live-streaming mit lokalen, die Multi-Bitrate Streams erstellen](media-services-live-streaming-with-onprem-encoders.md).

1. Wählen Sie in [Azure-Portal](https://portal.azure.com/)Azure Media Services-Konto.
2. Klicken Sie im Fenster **Eigenschaften** auf **Live-streaming**. 

    ![Erste Schritte](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
    
    **Live-streaming** -Fenster wird angezeigt.

3. Klicken Sie auf erstellen einen Pass-Through-Kanal mit den RTMP **Schnellerfassungsformular** Protokoll aufnehmen.

    Das Fenster **Neuen Kanal erstellen** .
4. Benennen Sie dem neuen Kanal, und klicken Sie auf **Erstellen**. 

    Dies erstellt einen Pass-Through-Kanal mit den RTMP Protokoll aufnehmen.

##<a name="create-events"></a>Erstellen von Ereignissen

1. Wählen Sie einen Kanal, der Sie ein Ereignis hinzufügen möchten.
2. Drücken Sie **Live-Event** .

![Ereignis](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)


##<a name="get-ingest-urls"></a>Get Aufnahme URLs

Nachdem der Kanal erstellt wurde, erhalten Sie Aufnahme URLs, die für dem live-Encoder bereitgestellt wird. Der Encoder verwendet diese URLs einen Livestream eingeben.

![Erstellt](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

##<a name="watch-the-event"></a>Sehen Sie sich das Ereignis

Um das Ereignis zu sehen, klicken Sie auf **Überwachung** im Azure-Portal oder kopieren Sie Streaming-URL und Player Ihrer Wahl. 
 
![Erstellt](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Live-Ereignis automatisch konvertiert, abrufbare Inhalte beendet.

##<a name="clean-up"></a>Bereinigen

Weitere Informationen zu Pass-Through-Kanäle finden Sie [Live-streaming mit lokalen, die Multi-Bitrate Streams erstellen](media-services-live-streaming-with-onprem-encoders.md).

- Ein Kanal kann beendet werden, nur, wenn alle Ereignisse/Programme auf dem Kanal wurde beendet.  Nach des Kanals beenden wird es keine Kosten. Wenn Sie erneut starten müssen, haben sie dieselbe URL aufnehmen, damit Sie den Encoder neu konfigurieren müssen.
- Ein Kanal kann gelöscht werden, nur, wenn alle aktiven Ereignisse im Kanal gelöscht wurden.

##<a name="view-archived-content"></a>Archivierter Inhalt

Auch nach dem Beenden und das Ereignis löschen wäre die Benutzer Ihre archivierten Content als Video on Demand für Streaming als Anlage nicht zu löschen. Anlage kann nicht gelöscht werden, wird durch ein Ereignis. das Ereignis muss zuerst gelöscht werden. 

Verwalten Ihre **Einstellung** und **Anlagenbuchhaltung**.

![Anlagen](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

##<a name="next-step"></a>Nächstes

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
