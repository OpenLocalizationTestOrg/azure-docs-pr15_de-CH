<properties 
    pageTitle="Wie führen Sie live-streaming mit Azure Media Services Multi-Bitrate Streams mit Azure-Portal erstellen | Microsoft Azure" 
    description="Dieses Lernprogramm führt die Schritte zum Erstellen eines Kanals, der einen einzigen Bitrate Livestream empfängt und Multi-Bitrate Datenstrom Azure-Portal codiert." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
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


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-the-azure-portal"></a>Wie führen Sie live-streaming mit Azure Media Services Multi-Bitrate Streams mit Azure-Portal erstellen

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST-API](https://msdn.microsoft.com/library/azure/dn783458.aspx)

Dieses Lernprogramm führt Sie schrittweise ein **Kanal** , der Stream Multi-Bitrate codiert und erhält einen einzelnen Bitrate Livestream erstellen.

>[AZURE.NOTE]Weitere grundlegende Informationen zu Kanäle live Codierung aktiviert sind, finden Sie in der [Live-streaming mit Azure Media Services Multi-Bitrate Streams erstellen](media-services-manage-live-encoder-enabled-channels.md).

##<a name="common-live-streaming-scenario"></a>Häufig für Live-Streaming

Im folgenden werden allgemeine Schritte zum Erstellen der Anwendung für live-streaming.

>[AZURE.NOTE] Derzeit ist die maximale empfohlene Dauer ein live-Ereignis 8 Stunden. Wenden Sie sich an Amslived unter Microsoft.com, benötigen Sie einen Kanal für längere Zeit ausgeführt.

1. Eine Videokamera an einen Computer anschließen Starten und Konfigurieren von lokalen live Encoder, der eine einzelne Bitrate in eines der folgenden Protokolle Ausgabestream kann: RTMP, Smooth Streaming und RTP (MPEG-TS). Weitere Informationen finden Sie unter [Azure Media Services RTMP-Unterstützung und Live-Encoder](http://go.microsoft.com/fwlink/?LinkId=532824).
    
    Dieser Schritt kann auch nach den Kanal erstellen durchgeführt werden.

1. Erstellen und Starten eines Kanals. 

1. Abrufen der Kanal Aufnahme URL. 

    Die Erfassung URL live Encoder dient den Stream an den Kanal senden.
1. Channel-Vorschau-URL abzurufen. 

    Verwenden Sie diese URL, um sicherzustellen, dass der Kanal richtig live-Streams empfangen.

3. Erstellen Sie Ereignis/Program (die auch eine Anlage erstellen). 
1. Veröffentlichen des Ereignisses (die einen OnDemand-Locator für die zugeordnete Anlage erstellen).  

    Müssen Sie mindestens eine reservierte Einheit auf Streaming-Endpunkt zum Streaming von Inhalten soll streaming.
1. Starten Sie das Ereignis, wenn Sie streaming und Archivierung bereit sind.
2. Optional kann live Encoder eine Ankündigung signalisiert. Die Ankündigung wird in den Ausgabestream.
1. Beenden Sie das Ereignis streaming und Archivierung das Ereignis beenden möchten.
1. Ereignis löschen (und optional die Anlage löschen).   

##<a name="in-this-tutorial"></a>In diesem Lernprogramm

In diesem Lernprogramm wird das Azure-Portal die folgenden Aufgaben ausführen: 

2.  Konfigurieren Sie Streaming-Endpunkte.
3.  Erstellen eines Kanals live codieren aktiviert ist.
1.  Ruft die URL Aufnahme um Live-Encoder liefern. Diese URL verwendet live Encoder Stream in den Kanal aufnehmen. .
1.  Erstellen einer Veranstaltungsprogramm (und Anlage)
1.  Veröffentlichen der Anlage und streaming URLs abrufen  
1.  Wiedergeben des Inhalts 
2.  Bereinigen

##<a name="prerequisites"></a>Erforderliche Komponenten

Die folgenden müssen für dieses Lernprogramm erforderlich.

- Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Wenn Sie ein Konto haben, können Sie ein kostenloses Testabo in wenigen Minuten erstellen. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
- Media Services-Konto. Zum Erstellen eines Kontos Media Services finden Sie unter [Konto erstellen](media-services-portal-create-account.md).
- Eine Webcam und Encoder, der einen einzelne Bitrate Livestream senden kann.

##<a name="configure-streaming-endpoints"></a>Streaming-Endpunkte konfigurieren 

Media Services bietet dynamische Verpackung zu Ihrem Multi-Bitrate MP4s in folgenden streaming-Formate kann: MPEG DASH HLS, Smooth Streaming oder HDS ohne diese Streaming-Formate erneut verpacken. Dynamische Verpackung müssen nur speichern und die Dateien in einzelne Speicherformat bezahlen mit Media Services erstellen und die entsprechende Antwort auf Anfragen von einem Client verwendet.

Um dynamische Verpackung nutzen zu können, müssen Sie mindestens eine Streaming-Einheit für den Streaming-Endpunkt aus der Lieferung des Inhalts werden soll.  

Um die Anzahl der reservierte Einheiten streaming zu erstellen, führen Sie folgende Schritte aus:

1. [Azure-Portal](https://portal.azure.com/) anmelden und Ihr Konto AMS wählen.
1. Klicken Sie im Fenster **Eigenschaften** auf **Streaming-Endpunkte**. 

2. Klicken Sie auf die Standardeinstellungen für streaming-Endpunkt. 

    **STREAMING ENDPUNKTDATEN STANDARDMÄßIG** angezeigt.

3. Ziehen Sie zum Angeben der Streaming-Einheiten Schieberegler **Streaming-Einheiten** .

    ![Streaming-Einheiten](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-streaming-units.png)

4. Klicken Sie auf **Speichern** , um die Änderung zu speichern.

    >[AZURE.NOTE]Die Zuordnung neuer Einheiten kann bis zu 20 Minuten dauern.

##<a name="create-a-channel"></a>Erstellen eines KANALS

1. Wählen Sie in [Azure-Portal](https://portal.azure.com/)Media Services, und klicken Sie auf Ihren Kontonamen Media Services.
2. Wählen Sie **Live-Streaming**.
3. Wählen Sie **benutzerdefinierte**. Diese Option können Sie einen Kanal erstellen, der für die Codierung von live aktiviert ist.

    ![Erstellen eines Kanals](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
    
4. Klicken Sie auf **Settings**.
    
    1.  Wählen Sie den **Livecodierungsmodus** Kanal. Dieser Typ gibt an, dass Sie einen Kanal erstellen, die für die Codierung von live aktiviert ist. Die eingehende also einzelne Bitrate Stream an den Kanal gesendet und in eine angegebene live Encoder Einstellungen Multi-Bitrate codiert. Weitere Informationen finden Sie in der [Live-streaming mit Azure Media Services Multi-Bitrate Streams erstellen](media-services-manage-live-encoder-enabled-channels.md). Klicken Sie auf OK.
    2. Geben Sie einen Kanal an.
    3. Klicken Sie am unteren Bildschirmrand
    
5. Wählen Sie die Registerkarte **Erfassung** .

    1. Auf dieser Seite können Sie streaming-Protokoll auswählen. Für den Kanaltyp **Livecodierungsmodus** sind gültige Protokolloptionen:
        
        - Einzelne Bitrate fragmentierte MP4 (Smooth Streaming)
        - Einzelne Bitrate RTMP
        - RTP (MPEG-TS): MPEG-2-Transportstream über RTP.
        
        Ausführliche Erläuterung zu jedem Protokoll finden Sie in der [Live-streaming mit Azure Media Services Multi-Bitrate Streams erstellen](media-services-manage-live-encoder-enabled-channels.md).
    
        Die Protocol-Option während der Kanal nicht ändern oder die zugeordneten Ereignisprogrammen ausgeführt werden. Wenn Sie unterschiedliche Protokolle benötigen, sollten Sie separate Kanäle für jedes Streaming-Protokoll erstellen.  

    2. Sie können IP-Einschränkung für die Erfassung verwenden. 
    
        Sie können die IP-Adressen definieren, die diesen Kanal ein Video aufnehmen. Zulässige IP-Adressen als eine einzelne IP-Adresse (z. B. "10.0.0.1"), einen IP-Adressbereich eine IP-Adresse und Subnetzmaske CIDR (z. B. "10.0.0.1/22") oder einen IP-Adressbereich eine IP-Adresse und eine punktierte Dezimalzahl Subnetzmaske angegeben werden (z.B. ' 10.0.0.1(255.255.252.0)').

        Wenn keine IP-Adressen angegeben und es keine Regeldefinition gibt wird keine IP-Adresse zulässig. IP-Adresse eine Regel erstellen und 0.0.0.0/0 festgelegt.

6. Übernehmen Sie auf der Registerkarte **Vorschau** IP-Einschränkung für die Vorschau.
7. Geben Sie auf die Registerkarte **Codierung** Codierung Voreinstellung. 

    Derzeit das einzige System Sie wählen Voreinstellung ist **Default 720p**. Um eine benutzerdefinierte Vorgabe festzulegen, öffnen Sie Microsoft-Support-Ticket. Geben Sie den Namen der Voreinstellung erstellt. 

>[AZURE.NOTE] Derzeit kann Channel-Start bis zu 30 Minuten dauern. Kanal zurücksetzen kann bis zu 5 Minuten dauern.

Nach dem Erstellen des Kanals können Sie auf den Kanal und **Einstellungen** , Ihre Konfigurationen Kanäle angezeigt. 

Weitere Informationen finden Sie in der [Live-streaming mit Azure Media Services Multi-Bitrate Streams erstellen](media-services-manage-live-encoder-enabled-channels.md).


##<a name="get-ingest-urls"></a>Get Aufnahme URLs

Nachdem der Kanal erstellt wurde, erhalten Sie Aufnahme URLs, die für dem live-Encoder bereitgestellt wird. Der Encoder verwendet diese URLs einen Livestream eingeben.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)


##<a name="create-and-manage-events"></a>Erstellen und Verwalten von Ereignissen

###<a name="overview"></a>Übersicht

Ein Kanal ist mit Ereignissen/Programme, mit denen Sie das Veröffentlichen und Speichern von Segmenten in live-Streams steuern. Kanäle verwalten Ereignisse-Programme. Kanal und Programm Beziehung ist ähnlich dem Kanal einen ständigen Inhalt und eine Programm einige terminiertes Ereignis auf diesem Kanal Gültigkeitsbereich Medium.

Sie können die Anzahl der Stunden, die aufgezeichnete Inhalte für das Ereignis die **Fenster Archiv** Länge beibehalten möchten. Dieser Wert kann auf maximal 25 Stunden ab 5 Minuten festgelegt. Archiv-Fensterlänge schreibt auch Kunden maximal von der aktuellen Position live zurück suchen kann. Ereignisse können über den angegebenen Zeitraum ausgeführt, jedoch hinter der Fensterlänge liegt Inhalt kontinuierlich verworfen. Der Wert dieser Eigenschaft bestimmt auch, wie lange der Client Manifeste werden.

Jedes Ereignis ist mit einer Anlage. Um das Ereignis zu veröffentlichen müssen Sie einen Locator OnDemand für die zugeordnete Anlage erstellen. Mit diesem Locator können Sie Streaming-URL erstellen, die Sie für die Clients bereitstellen können.

Ein Kanal unterstützt bis zu drei gleichzeitig ausgeführte Ereignisse Erstellen mehrerer Archive dem eingehenden Stream. Dadurch können Sie veröffentlichen und Archivierung von Bestandteilen eines Ereignisses. Beispielsweise ist die Anforderung 6 Stunden eines Ereignisses archivieren, sondern nur 10 Minuten. Dazu müssen Sie zwei gleichzeitig ausgeführter Ereignis. Ein Ereignis soll das Ereignis von 6 Stunden archiviert, aber die Anwendung veröffentlicht. Das Ereignis für 10 Minuten archivieren und dieses Programm veröffentlicht.

Vorhandene Programme für neue Ereignisse sollten Sie nicht wieder verwenden. Stattdessen erstellen und Starten eines neuen Programms für jedes Ereignis.

Starten einer Veranstaltungsprogramm jetzt streaming und Archivierung. Beenden Sie das Ereignis streaming und Archivierung das Ereignis beenden möchten. 

Archivierten Inhalt löschen beenden Sie und löschen Sie das Ereignis und anschließend die zugeordnete Anlage. Anlage kann nicht gelöscht werden, wird das Ereignis. das Ereignis muss zuerst gelöscht werden. 

Auch nach dem Beenden und das Ereignis löschen wäre die Benutzer Ihre archivierten Content als Video on Demand für Streaming als Anlage nicht zu löschen.

Soll den archivierten Inhalt beibehalten, aber nicht verfügbar für streaming, löschen Sie streaming locator

###<a name="createstartstop-events"></a>Erstellen/Start/Stopp-Ereignisse

Haben Sie den Stream in den Kanal können Sie Streaming-Ereignis Erstellen einer Anlage, Anwendung und Streaming-Locator beginnen. Dies wird den Stream archivieren und Zuschauer durch Streaming-Endpunkt verfügbar machen. 

Es gibt zwei Arten beginnen: 

1. Drücken Sie auf **Channel** **Live Ereignis** ein neues Ereignis hinzuzufügen.

    Geben Sie: Ereignisname, Ressourcenname archivfenster und Verschlüsselungsoption.
    
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
    
    Bleibt **Publish live dabei jetzt** überprüft wird Ereignis veröffentlichen URLs erstellt.
    
    **Starten**, drücken Sie, wenn Sie das Ereignis übertragen möchten.

    Drücken Sie nach des Ereignisses starten **Überwachen** den Inhalt wiedergegeben.

2. Alternativ können Sie eine Tastenkombination und drücken **Go Live** auf den **Kanal** . Dadurch wird eine Anlage Programm und Streaming-Locator erstellen.

    Das Ereignis namens **Default** und archivfenster 8 Stunden festgelegt ist.

Veröffentlichten Ereignisses auf **Live-Ereignis** sehen. 

Wenn auf **Off Air**wird alle live Ereignisse beendet. 


##<a name="watch-the-event"></a>Sehen Sie sich das Ereignis

Um das Ereignis zu sehen, klicken Sie auf **Überwachung** im Azure-Portal oder kopieren Sie Streaming-URL und Player Ihrer Wahl. 
 
![Erstellt](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Live-Veranstaltung konvertiert automatisch Ereignisse bei Bedarf Inhalt beendet.

##<a name="clean-up"></a>Bereinigen

Wenn Sie streaming Ereignisse geschehen, zuvor bereitgestellten Ressourcen bereinigen möchten gehen Sie folgende.

- Beenden Sie drücken den Stream vom Encoder.
- Beenden Sie den Kanal. Nach des Kanals beenden wird es keine Kosten. Wenn Sie erneut starten müssen, haben sie dieselbe URL aufnehmen, damit Sie den Encoder neu konfigurieren müssen.
- Sie können Ihren Streamingendpunkt, sofern keine Archiv für Ihre live Ereignis als Stream bei Bedarf weiterhin beenden. Wenn der Kanal beendet ist, wird es keine Kosten.
  
##<a name="view-archived-content"></a>Archivierter Inhalt

Auch nach dem Beenden und das Ereignis löschen wäre die Benutzer Ihre archivierten Content als Video on Demand für Streaming als Anlage nicht zu löschen. Anlage kann nicht gelöscht werden, wird durch ein Ereignis. das Ereignis muss zuerst gelöscht werden. 

Verwalten Ihre **Einstellung** und **Anlagenbuchhaltung**.

![Anlagen](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

##<a name="considerations"></a>Hinweise

- Derzeit ist die maximale empfohlene Dauer ein live-Ereignis 8 Stunden. Wenden Sie sich an Amslived unter Microsoft.com, benötigen Sie einen Kanal für längere Zeit ausgeführt.
- Müssen Sie mindestens eine reservierte Einheit auf Streaming-Endpunkt zum Streaming von Inhalten soll streaming.


##<a name="next-step"></a>Nächstes

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
