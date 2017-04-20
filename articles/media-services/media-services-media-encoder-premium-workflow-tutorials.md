<properties 
    pageTitle="Avancierte Media Encoder Premium-Workflow-Lernprogramme" 
    description="Dieses Dokument enthält exemplarische Vorgehensweisen, die zeigen, wie erweiterte Aufgaben mit Media Encoder Premium-Workflow und komplexe Workflows mit Workflow-Designer erstellen." 
    services="media-services" 
    documentationCenter="" 
    authors="xstof" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"  
    ms.author="xstof;xpouyat;juliako"/>

#<a name="advanced-media-encoder-premium-workflow-tutorials"></a>Erweiterte Media Encoder Premium-Workflow-Lernprogramme

##<a name="overview"></a>Übersicht 

Dieses Dokument enthält exemplarische Vorgehensweisen, die zeigen, wie Sie Workflows mit **Workflow-Designer**anpassen. Tatsächliche Workflowdateien finden Sie [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

##<a name="toc"></a>INHALTSVERZEICHNIS

Folgende Themen werden behandelt:

- [In einer einzigen Bitrate MP4 MXF-Codierung](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
    - [Starten eines neuen Workflows](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new) 
    - [Verwenden der Dateieingabe Medien](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
    - [Überprüfen der Mediendatenströme](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
    - [Video Encoder für hinzufügen Generierung von MP4-Dateien](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
    - [Codierung von Audiodaten](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
    - [Audio und Video-Streams in einem MP4-Container Multiplexing](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
    - [MP4-Datei schreiben](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
    - [Erstellen eines Vermögenswertes Media Services aus der Ausgabedatei](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
    - [Workflow abgeschlossen lokal testen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
- [MXF in Multibitrate MP4s - Codierung dynamische Verpackung aktiviert](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
    - [Eine oder mehrere zusätzliche MP4 Ausgaben hinzufügen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
    - [Konfigurieren die Dateinamen für die Ausgabe](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
    - [Eine separate Audiospur hinzufügen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
    - [Hinzufügen der. ISM SMIL-Datei](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
- [Codierung MXF in Multibitrate MP4 - erweiterte Vorlage](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
    - [Übersicht über Workflow zu](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_overview)
    - [Namenskonventionen für Dateien](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
    - [Veröffentlichen der Komponenteneigenschaften der Workflow-Stamm](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
    - [Haben Ausgabedatei generiert veröffentlichten Eigenschaftswerte Namen auf](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
- [Multibitrate MP4-Ausgabe Miniaturansichten hinzufügen](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
    - [Übersicht über Workflow Miniaturansichten hinzufügen](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to_multibitrate_MP4_overview)
    - [JPG-Codierung hinzufügen](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
    - [Umgang mit Farbraum konvertieren](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
    - [Schreiben von Miniaturansichten](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
    - [Erkennen von Fehlern in einem workflow](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
    - [Workflow abgeschlossen](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
- [Zeitbasierte verkürzen Multibitrate MP4-Ausgabe](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
    - [Übersicht über Workflow hinzufügen mit](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
    - [Stream Trimmer verwenden](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
    - [Workflow abgeschlossen](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
- [Einführung in skriptgesteuerte Komponente](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
    - [Skripts in einem Workflow: Hello World](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
- [Frame-basierte Trimmen Multibitrate MP4-Ausgabe](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
    - [Hinzufügen mit Blueprints im Überblick](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
    - [Mithilfe der Clipliste XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
    - [Ändern der Liste Clips aus einer Komponente Skript](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
    - [Hinzufügen einer ClippingEnabled-Eigenschaft](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

##<a id="MXF_to_MP4"></a>In einer einzigen Bitrate MP4 MXF-Codierung
 
In dieser exemplarischen Vorgehensweise erstellen wir eine Bitrate. MP4-Datei mit AAC er codiert Ton ein. MXF-Eingabedatei. 

###<a id="MXF_to_MP4_start_new"></a>Starten eines neuen Workflows 

Öffnen Sie Workflow-Designer, und wählen Sie "Datei"-"Neuer Arbeitsbereich"-"Transcode Plan" 

Der neue Workflow zeigt 3 Elemente: 

- Primäre Datei
- Clipliste XML
- Ausgabe von Datei-Anlage  

![Neue Codierung Workflow](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Neue Codierung Workflow*

###<a id="MXF_to_MP4_with_file_input"></a>Verwenden der Dateieingabe Medien

Um unsere input Mediendatei akzeptieren, beginnt man mit Media Datei Input-Komponente hinzufügen. Der Workflow eine Komponente hinzu, in das Suchfeld Repository suchen Sie und der Designer-Bereich ziehen Sie den gewünschten Eintrag. Dafür Media Datei Input und der Dateiname Pin aus Media Datei Input primäre Quelldatei Komponente an.

![Dateieingabe angeschlossenes Medium](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Dateieingabe angeschlossenes Medium*

Bevor wir viel mehr tun können, müssen wir zuerst zum Workflow-Designer welche Beispieldatei angeben wir mit unserem Workflow entwerfen möchten. Dazu klicken Sie auf Hintergrund Designerbereich und die Primärdatei-Eigenschaft im rechten-Eigenschaft auf Suchen. Klicken Sie auf das Ordnersymbol und wählen Sie die gewünschte Datei zum Testen des Workflows mit. Sobald dies geschehen ist, die Komponente Media Dateieingabe überprüfen Sie die Datei und Füllen der Ausgabepins, um die Datei wiederzugeben, überprüft.

![Eingabe von gefüllten Mediendateien](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Eingabe von gefüllten Mediendateien*

Während mit was wir arbeiten, möchten sie nicht sagen, aber die codierte Ausgabe wohin sollten gibt. Ähnlich wie die primäre Quelldatei wurde konfiguriert, die Ausgabe Ordner Variable-Eigenschaft direkt darunter jetzt konfigurieren.

![Konfigurierte Eigenschaften für Eingabe und Ausgabe](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Konfigurierte Eigenschaften für Eingabe und Ausgabe*

###<a id="MXF_to_MP4_streams"></a>Überprüfen der Mediendatenströme

Häufig hat es gewünscht schaut wie Stream, Abläufe durch den Workflow. Klicken Sie zum Überprüfen von eines Streams an jedem Punkt im Workflow eine Ausgabe oder einem Eingabepin an der Komponenten. Versuchen Sie in diesem Fall nicht komprimierte Video Ausgang von Media Dateieingabe auf. Ein Dialogfeld wird geöffnet, mit der ausgehende Video überprüfen können.

![Überprüfen nicht komprimierte Video-Ausgang](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Überprüfen nicht komprimierte Video-Ausgang*

In diesem Fall teilt es uns beispielsweise, dass mit einem 1920 x 1080 mit 24 Frames pro Sekunde 4 geht: 2:2-Sampling Video fast 2 Minuten.

###<a id="MXF_to_MP4_file_generation"></a>Video Encoder für hinzufügen Generierung von MP4-Dateien

Beachten Sie, dass eine nicht komprimierte Video- und mehrere dekomprimiert Audio Ausgabepins stehen auf unserer Media-Datei; Um eingehende Video codieren, brauchen wir eine Kodierung Komponente - in diesem Fall generiert. MP4-Dateien.

Fügen Sie zum Codieren des Videostream zu h. 264 AVC-Video Encoder-Komponente auf die Designeroberfläche. Diese Komponente ein Dekomprimieren Videostream als Eingabe akzeptiert und einen komprimierten AVC-video-Stream auf seine Ausgabe liefert.

![Nicht verbundene AVC-Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Nicht verbundene AVC-Encoder*

Die Eigenschaften bestimmen wie die Codierung genau passiert. Werfen Sie einen Blick auf einige der wichtigeren Einstellungen:

- Ausgabe: Höhe und Breite Ausgabe: bestimmt die Auflösung des codierten Videos. In unserem Fall gehen wir mit 640 x 360
- Bildrate: Wenn auf Passthrough gesetzt es nur die Quellframerate übernehmen, kann dies jedoch überschreiben. Beachten Sie, dass die Framerate Umwandlung nicht Bewegung kompensiert wird.
- Profil und Ebene: die AVC-Profil und bestimmt. Um bequem Weitere Informationen zu den verschiedenen Ebenen und Profile, klicken Sie auf das Fragezeichensymbol AVC Video Encoder-Komponente und die Hilfeseite zeigt weitere Details zu den einzelnen Ebenen. Gehen Sie für unser Beispiel wir mit Main Ebene 3.2 (Standard).
- Bewerten Sie Modus und Bitrate (Kbit/s): in diesem Szenario wählen wir eine Konstante Bitrate (CBR) Ausgabe mit 1200 Kbit/s
- Videoformat: darum Sprachbenutzerschnittstelle (Video Verwendbarkeit Informationen), die im h. 264-Stream (Seiteninformationen Decoder zu der Anzeige verwendet, aber nicht unbedingt korrekt decodiert) geschrieben wird:
- NTSC (Standard für US und Japan mit 30 fps)
- PAL (typisch für Europa mit 25 fps)
- Modus der GOP-Größe: wir feste GOP-Größe für unsere Zwecke mit einem Schlüssel von 2 Sekunden mit GOPs geschlossen konfigurieren. Dadurch bietet Kompatibilität mit dynamischen Verpackung Azure Media Services.

Futtermittel unsere AVC-Encoder nicht komprimierte Video-Ausgang Media Datei Input Komponente an Anschließen der nicht komprimierten Videos Pin von AVC-encoder

![Verbundene AVC-Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Verbundene AVC Main encoder*

###<a id="MXF_to_MP4_audio"></a>Codierung von Audiodaten

An diesem Punkt haben wir Video kodiert, aber die ursprüngliche nicht komprimierte Audiodaten noch komprimiert werden. Für diese gehen wir mit AAC-Codierung von der Komponente AAC-Encoder (Dolby). Der Workflow hinzufügen.

![Nicht verbundene AVC-Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Nicht verbundene AAC-encoder*

Jetzt gibt es eine Inkompatibilität: Gibt es nur einen einzigen dekomprimiert audio Filtereingang vom Encoder AAC und höchstwahrscheinlich Media Datei Input zwei verschiedene nicht komprimierte Audiodaten verfügbar: linken Audiokanal und rechts. (Wenn Sie mit Surround-Sound, ist das 6 Kanäle.) Es ist so nicht möglich eine Verbindung direkt Audio Media Datei Eingabequelle in AAC-audio-Kodierung. AAC-Komponente erwartet eine so genannte "verschachtelt" Audiodaten: einem einzigen Datenstrom, die der linken und rechten Kanäle miteinander verzahnt. Wenn wir von unserer Quelldatei wissen sind die Audiospuren an welcher Position in der Quelle wir solche Interleaving Audiodaten mit Positionen der richtig zugewiesene Lautsprecher links und rechts generieren können.

Zunächst wird eine überlappenden Stream aus der erforderlichen audio-Quellkanäle generiert. Audio Stream Interleaver Komponente behandelt dies für uns. Workflow fügen Sie hinzu und verbinden Sie die Audioausgängen Dateieingabe Media hinein.

![Audiodaten Interleaver verbunden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Audiodaten Interleaver verbunden*

Jetzt haben wir eine überlappende Audiodaten angeben nicht wir noch, wo Sie die linken oder rechten Lautsprecher Positionen weisen. Wir können Lautsprecher Position Auftraggeber nutzen, um angeben.

![Die Auftraggeber eine Speaker Position hinzufügen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Die Auftraggeber eine Speaker Position hinzufügen*

Konfigurieren Sie Lautsprecher Position Auftraggeber für die Verwendung durch einen Encoder Voreinstellung "Benutzerdefiniert" und Channel-Voreinstellung "2.0 (L, R)" bezeichnet einen Stereo-Eingabestream. (Dadurch wird der linken und der rechten Lautsprecher Position Kanal 1 Kanal 2 zuweisen.)

Die Ausgabe der Auftraggeber Position Lautsprecher an die Eingabe des Encoders AAC anschließen Erzähle AAC-Encoder für ein "2.0 (L, R)" Kanal voreingestellt, damit es weiß mit Stereo-Audio als Eingabe.

###<a id="MXF_to_MP4_audio_and_fideo"></a>Audio und Video-Streams in einem MP4-Container Multiplexing

Angesichts unserer AVC codiertes video-Stream und unserer AAC Audiostream codiert, erfassen wir in ein. MP4-Container. Kombinieren Sie verschiedene Streams in einem einzelnen Prozess heißt "multiplexing" (oder "muxen"). In diesem Fall haben wir die Audio- und video-Streams in einem kohärenten interleaving. MP4-Paket. Die Komponente, die für Koordinaten ein. MP4-Container ist der ISO MPEG-4-Multiplexer aufgerufen. Der Entwurfsoberfläche fügen Sie hinzu und seine Eingaben AVC-Video-Encoder und AAC-Encoder an.

![Verbundene MPEG4 Multiplexer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Verbundene MPEG4 Multiplexer*

###<a id="MXF_to_MP4_writing_mp4"></a>MP4-Datei schreiben

Beim Schreiben einer Ausgabedatei ist die Ausgabe-Komponente verwendet. Wir können dies in die Ausgabe der ISO MPEG-4-Multiplexer, damit die Ausgabe geschrieben wird auf der Festplatte. Verbinden Sie hierzu Filterausgang Container (MPEG-4) mit Eingabepins Schreiben der Datei.

![Dateiausgabe verbunden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Dateiausgabe verbunden*

Die Eigenschaft der Dateiname verwendet wird bestimmt. Diese Eigenschaft mit einem bestimmten Wert hartcodiert sein, wird wahrscheinlich einen es durch einen Ausdruck festlegen möchten.

Der Workflow automatisch bestimmen die Ausgabe zu Name-Eigenschaft aus einer Datei, klicken Sie auf die Schaltfläche neben dem Dateinamen (neben dem Ordnersymbol). Wählen Sie aus dem Dropdown-Menü "Ausdruck". Der Ausdrucks-Editor wird angezeigt. Deaktivieren Sie zuerst den Inhalt des Editors.

![Leere Ausdrucks-Editor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Leere Ausdrucks-Editor*

Ausdrucks-Editor kann einen literalen Wert gemischt mit einer oder mehreren Variablen eingeben. Variablen beginnen mit einem Dollarzeichen. $-Taste drücken, wird der Editor eine Dropdownliste mit verfügbaren Variablen angezeigt. In unserem Fall verwenden wir aus Output Directory Variablen und Basis Eingabedatei Name:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Ausgefüllt, Ausdrucks-Editor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Ausgefüllt, Ausdrucks-Editor*

>[AZURE.NOTE]Um zu sehen, finden Sie eine Ausgabedatei des Projekts Codierung in Azure Geben Sie einen Wert im ausdruckseditor. 

Wenn Sie den Ausdruck auf ok bestätigen, wird zu diesem Zeitpunkt der Datei Eigenschaft löst schätzen im Eigenschaftenfenster angezeigt.

![Dateiausdruck ergibt Ausgabeverzeichnis](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Dateiausdruck ergibt Ausgabeverzeichnis*

###<a id="MXF_to_MP4_asset_from_output"></a>Erstellen eines Vermögenswertes Media Services aus der Ausgabedatei

Während wir eine MP4-Ausgabedatei geschrieben haben, müssen wir angeben, dass diese Datei Ausgabe-Anlage Media Services gehört durch das Ausführen dieses Workflows generieren. Zu diesem Zweck wird der Ausgabe Datei/Anlage Knoten auf der Leinwand Workflow verwendet. Alle eingehende Dateien in diesem Knoten machen Teil der resultierenden Azure Media Services Anlage.

Verbinden Sie DateiAusgabe Komponente Ausgabe Datei/Anlage Komponente beendet den Workflow.

![Workflow abgeschlossen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Workflow abgeschlossen*

###<a id="MXF_to_MP4_test"></a>Workflow abgeschlossen lokal testen

Drücken Sie zum Testen des Workflows lokal Wiedergabeschaltfläche in der Symbolleiste oben. Abschluss des Workflows ausführen überprüfen Sie im konfigurierten Ausgabeordner generierte Ausgabe. Die sehen der fertigen MP4-Ausgabedatei MXF Eingabequelle Datei codiert wurde.

##<a id="MXF_to_MP4_with_dyn_packaging"></a>Dynamische Verpackung Codierung MXF in MP4 - Multibitrate aktiviert

In dieser exemplarischen Vorgehensweise erstellen mehrere Bitrate MP4-Dateien wir mit codierten AAC audio aus. MXF-Eingabedatei.

Wenn eine Multi-Bitrate Anlage Ausgabe gewünscht wird in Kombination mit dynamischen Verpackung Funktionen von Azure Media Services mehrere GOP ausgerichtete MP4-Dateien jedes müssen einer anderen Bitrate und Auflösung generiert werden. Dazu bietet [Codierung MXF in einem einzigen Bitrate MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) exemplarischen Vorgehensweise einen guten Ausgangspunkt.

![Workflow starten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Workflow starten*

###<a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Eine oder mehrere zusätzliche MP4 Ausgaben hinzufügen

Jede Datei MP4 in unserer resultierenden Azure Media Services Anlage unterstützen einer anderen Bitrate und Auflösung. Fügen Sie eine oder mehrere Dateien von MP4-Ausgabe an den Workflow.

Um sicherzustellen, dass wir alle unsere video Encoder mit denselben Einstellungen erstellt haben, ist es am günstigsten, bereits vorhandenen AVC Video Encoder duplizieren und konfigurieren Sie eine andere Kombination aus Auflösung und Bitrate (Fügen Sie eine 960 x 540 mit 25 Frames pro Sekunde bei 2,5 Mbit/s). Zum Duplizieren des vorhandenen Encoders Kopie einfügen auf der Designeroberfläche.

Verbinden Sie nicht komprimierte Video-Ausgang des Media-Datei in unsere neue AVC-Komponente.

![Zweite AVC-Encoder verbunden](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Zweite AVC-Encoder verbunden*

Passen Sie die Konfiguration für unsere neue AVC-Encoder 960 x 540 2,5 Mbps Ausgabe jetzt an. (Verwenden der Ausgabe Eigenschaften "Width", "Ausgabe: Höhe" und "Bitrate (Kbit/s)".)

Erhalten wir wollen sich ergebender Vermögenswert mit dynamischen Verpackung Azure Media Services, Streaming-Endpunkt muss aus diesen MP4-Dateien genau in einer Weise ausgerichtet sind, Clients, das Wechseln zwischen verschiedenen Bitraten einer einzigen glatte kontinuierlichen Video- und Audioqualität erhalten HLS-fragmentierte MP4/Strich-Fragmente erzeugen. Damit dies geschieht, müssen wir sicherstellen, dass in den Eigenschaften des AVC Encoder GOP ("Group of Pictures") Größe für beide MP4-Dateien auf 2 Sekunden festgelegt durch:

- GOP-Größe-Modus auf feste GOP-Größe festlegen und
- Key Frame Intervall zwei Sekunden.
- auch stehen das Steuerelement GOP IDR geschlossen GOP alle GOP sicherstellen ohne Abhängigkeiten

Benennen Sie zu unserem Workflow zu verstehen, den ersten AVC Encoder "AVC-Video Encoder 640 x 360 1200 Kbit/s" und die zweite AVC "AVC-Video Encoder 960 x 540 2500 kbps".

Fügen Sie jetzt eine zweite ISO MPEG-4-Multiplexer und eine zweite Datei. Neue AVC-Encoder mit dem multiplexer und stellen Sie sicher, dass die Ausgabe in die Datei geleitet wird. Schließen Sie die AAC audio Encoder Ausgabe der neuen multiplexer Eingabe auch aus. Der Datei wiederum dann anschließbar Ausgabe Datei/Anlage Knoten Media Services Asset hinzufügen, die erstellt wird.

![Zweite Muxer und Datei-Ausgang](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Zweite Muxer und Datei-Ausgang*

Kompatibilität mit dynamic Packaging Azure Media Services GOP Anzahl und Dauer der multiplexer Chunk-Modus konfigurieren und GOPs pro Abschnitt auf 1 festgelegt. (Dies sollte der Standardwert.)

![Muxer-Chunk-Modus](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Muxer-Chunk-Modus*

Hinweis: Sie sollten diese für andere Kombinationen Bitrate und Auflösung wiederholen zu Anlage Ausgabe hinzugefügt haben.

###<a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Konfigurieren die Dateinamen für die Ausgabe

Wir haben mehr als eine Datei für die Ausgabe-Ressource hinzugefügt. Auf diese Weise sicherstellen, dass die Dateinamen aller Ausgabedateien voneinander unterscheiden und vielleicht sogar Anwenden einer Dateibenennungskonvention aus der Dateiname wird mit arbeiten müssen.

Dateibenennung Ausgabe gesteuert über Ausdrücke im Designer. Öffnen im Eigenschaftenbereich für eine Ausgabe-Komponenten und den Ausdrucks-Editor für die Eigenschaft. Unsere erste Ausgabedatei durch den folgenden Ausdruck konfiguriert wurde (Siehe Anleitung für die Migration von [MXF, einzelne Bitrate MP4-Ausgabe](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Unsere Dateiname zwei Variablen bestimmt ist: das Ausgabeverzeichnis schreiben und Basis Quelldateiname. Erstere werden als Eigenschaft im Workflow Stamm und letztere hängt von der eingehenden Datei. Beachten Sie, dass das Ausgabeverzeichnis zum lokalen Testen verwenden; Diese Eigenschaft wird durch das Workflowmodul überschrieben werden, wenn Workflow cloudbasierten Media-Prozessor in Azure Media Services ausgeführt wird.
Geben unseren Ausgabedateien eine konsistente Ausgabe benennen, ändern Sie die erste Datei benennen Ausdruck:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

und die zweite:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Führen Sie eine Zwischenprüfung ausführen, um sicherzustellen, dass beide MP4-Ausgabedateien ordnungsgemäß generiert werden.

###<a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Eine separate Audiospur hinzufügen

Wie wir später sehen, wenn wir eine ISM-Datei mit unseren MP4-Ausgabedateien zu generieren, auch benötigen reine MP4-Datei als die Audiospur für unsere adaptives streaming wir. Zum Erstellen dieser Datei (ISO MPEG-4-Multiplexer) Workflow zusätzliche Muxer hinzufügen und Verbinden von Filterausgang AAC Encoder mit der Eingabe für Spur 1.

![Audio Muxer hinzugefügt](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Audio Muxer hinzugefügt*

Erstellen Sie eine dritte Komponente ausgehenden Datenstrom aus der Muxer und Datei benennen Ausdruck als Ausgabe:
    
    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Audio Muxer Datei erstellen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Audio Muxer Datei erstellen*

###<a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Hinzufügen der. ISM SMIL-Datei

Für dynamische Verpackung mit MP4-Dateien (und nur Audio MP4) in unserem Media Services Anlage zusammenarbeiten, brauchen wir auch eine Manifestdatei (auch als "SMIL" Datei bezeichnet: Synchronized Multimedia Integration Language). Diese Datei zeigt Azure Media Services MP4 Dateien zur dynamischen Verpackung und diesen für audio-streaming berücksichtigt werden. Normale Manifestdatei für eine Gruppe von MP4 mit einer einzigen Audiodaten sieht folgendermaßen aus:
    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

Der ISM-Datei innerhalb einer Switch-Anweisung auf die einzelnen video MP4-Dateien und zusätzlich diese auch ein (oder mehrere) Audiodatei Verweise auf eine MP4, die nur die Audiodaten enthält.

Generieren der Manifestdatei für unsere Gruppe des MP4 erfolgt über eine Komponente namens "AMS Manifest Writer". Verwendung der Oberfläche ziehen und "Vollständig schreiben" Ausgabepins Datei drei AMS Manifest Writer Eingabe verbinden. Vergewissern Sie sich die Ausgabe des AMS Manifest Writers, Ausgabe Datei/Anlage verbinden.

Wie bei unseren anderen Datei Ausgabe Komponenten der ISM Ausgabe Dateiname mit einem Ausdruck konfigurieren:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Fertig Workflow ähnelt der folgenden:

![Fertige MXF Multibitrate MP4-Workflow](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Fertige MXF Multibitrate MP4-Workflow*

##<a id="MXF_to__multibitrate_MP4"></a>Codierung MXF in Multibitrate MP4 - erweiterte Vorlage

In der [vorherigen exemplarischen Vorgehensweise Workflow](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) haben wir gesehen, wie eine einzelne Eingabe MXF-Ressource zu Ausgabe Multi-Bitrate MP4-Dateien, eine reine MP4-Datei und eine Manifestdatei mit dynamischen Verpackung Azure Media Services konvertiert werden kann.

Diese exemplarische Vorgehensweise zeigt, wie einige der Aspekte können erweitert und als einfacher.

###<a id="MXF_to_multibitrate_MP4_overview"></a>Übersicht über Workflow zu

![Multibitrate MP4-Workflow zu](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Multibitrate MP4-Workflow zu*

###<a id="MXF_to__multibitrate_MP4_file_naming"></a>Namenskonventionen für Dateien

Im vorherigen Workflow einen einfachen Ausdruck als Grundlage für das Generieren von Ausgabedateinamen angegeben. Wir haben jedoch einige Kopien: alle der einzelnen Dateikomponenten dieser Ausdruck angegeben.

Unsere Datei Ausgabe-Komponente für die erste Datei wird z. B. mit dem folgenden Ausdruck konfiguriert:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Für die zweite Ausgabe von video haben wir einen Ausdruck wie:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Wäre es nicht weniger fehleranfällig und bequemer, wenn wir einige dieser Duplikate entfernen und konfigurierbarer stattdessen machen cleaner? Glücklicherweise können wir: der Designer Ausdruck in Kombination mit benutzerdefinierte Eigenschaften auf unseren Workflow-Stamm erstellen erhalten wir einen zusätzlichen Komfort.

Angenommen, wir Dateiname Konfiguration von Bitraten einzelner MP4-Dateien fahren. Diese Bitraten wollen wir an einer zentralen Stelle (im Stammverzeichnis unser Diagramm), sie konfigurieren zugegriffen werden können und Generierung des Dateinamens Laufwerk konfigurieren. Dazu zunächst die Bitrate Eigenschaft AVC Encoder in das Stammverzeichnis von unserem Workflow veröffentlichen, wird sowohl das Stammverzeichnis sowie ab AVC-Encoder auf. (Auch wenn an zwei verschiedenen Stellen angezeigt, es ist nur eine zugrunde liegende Wert.)

###<a id="MXF_to__multibitrate_MP4_publishing"></a>Veröffentlichen der Komponenteneigenschaften der Workflow-Stamm

Öffnen des ersten AVC-Encoders, Gehe zu Eigenschaft Bitrate (Kbit/s) und wählen Sie aus der Dropdown-Liste veröffentlichen.

![Veröffentlichen die Bitrate-Eigenschaft](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Veröffentlichen die Bitrate-Eigenschaft*

Konfigurieren Sie das Dialogfeld veröffentlichen Veröffentlichen in das Stammverzeichnis von unserem Diagramm Workflow veröffentlichten Namen "video1bitrate" und "Video 1 Bitrate" lesbaren Anzeigenamen. Konfigurieren einen benutzerdefinierten Namen "Bitraten Streaming" bezeichnet und veröffentlichen erreicht.

![Veröffentlichen die Bitrate-Eigenschaft](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Publishing Dialogfeld Bitrate-Eigenschaft*

Wiederholen Sie der Bitrate-Eigenschaft des zweiten AVC-Encoder, und nennen Sie sie "video2bitrate" mit dem Anzeigenamen "Video 2 Bitrate" in einer benutzerdefinierten Gruppe "Bitraten Streaming".

Wenn wir jetzt die Stamm Workfloweigenschaften untersuchen, sehen wir unsere benutzerdefinierte Gruppe mit zwei veröffentlichten Eigenschaften angezeigt. Beide spiegeln den Wert ihrer jeweiligen AVC Encoder Bitrate.

![Veröffentlichte Bitrate Props Workflow Stamm](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Wenn wir von Code oder ein Ausdruck auf diese Eigenschaften zugreifen möchten, können wir das wie folgt tun:

- aus einer Komponente direkt unterhalb des Stammverzeichnisses Inline: Node.getPropertyAsString('.. / video1bitrate', null)
- in einem Ausdruck: ${ROOT_video1bitrate}
 
Wir führen "Streaming Bitraten" Gruppe sowie unsere Audiospur-Bitrate auf veröffentlichen. In den Eigenschaften des AAC Bitrate Einstellung suchen und veröffentlichen aus der Dropdown-Liste daneben wählen. Das Stammobjekt des Diagramms mit Namen "audio1bitrate" Veröffentlichen Sie und an "Audio 1 Bitrate" innerhalb der benutzerdefinierten Gruppe "Bitraten Streaming".

![Dialogfeld für die Audiobitrate Veröffentlichung](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Dialogfeld für die Audiobitrate Veröffentlichung*

![Resultierende Video- und Eigenschaften Stamm](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Resultierende Video- und Eigenschaften Stamm*

Beachten Sie, dass auch neu konfiguriert Werte und ändert die Werte der jeweiligen Komponenten sie mit drei ändern (und aus veröffentlicht).

###<a id="MXF_to__multibitrate_MP4_output_files"></a>Haben Ausgabedatei generiert veröffentlichten Eigenschaftswerte Namen auf

Statt hartzucodieren können unsere generierten Dateinamen wir ändern unsere Dateinamen-Ausdruck aller Komponenten Ausgabe Bitrate Eigenschaften auf nur Stamm Graph veröffentlichte. Beginnend mit der ersten DateiAusgabe suchen Sie Dateieigenschaft und bearbeiten Sie den Ausdruck wie folgt:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Die verschiedenen Parameter in diesem Ausdruck zugegriffen und auf das Dollarzeichen auf der Tastatur im Fenster Ausdruck eingegeben. Eine der verfügbaren Parameter ist unsere video1bitrate wir zuvor veröffentlicht.

![Zugreifen auf Parameter innerhalb eines Ausdrucks](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Zugreifen auf Parameter innerhalb eines Ausdrucks*

Gehen Sie genauso für die Ausgabe für unseren zweiten Video:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

und für die DateiAusgabe nur-Audio-:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Wenn wir jetzt die Bitrate für video oder audio-Dateien ändern, der jeweiligen Encoder neu konfiguriert und die Bitrate basierende Namenskonvention automatisch berücksichtigt.

##<a id="thumbnails_to__multibitrate_MP4"></a>Multibitrate MP4-Ausgabe Miniaturansichten hinzufügen

Ein Workflow, der generiert [Multibitrate MP4-Ausgabe eine MXF Eingabe](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)ab, wird jetzt gesucht werden Miniaturansichten der Ausgabe hinzufügen.

###<a id="thumbnails_to__multibitrate_MP4_overview"></a>Übersicht über Workflow Miniaturansichten hinzufügen

![Multibitrate MP4 Workflow ab](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Multibitrate MP4 Workflow ab*

###<a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>JPG-Codierung hinzufügen

Das Herzstück unserer Miniaturansicht Generation werden in JPG-Dateien JPG Encoder-Komponente.

![JPG-Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG-Encoder*

Wir können nicht der Datei Media in JPG-Encoder jedoch direkt unsere dekomprimiert Videodatenstrom verbinden. Stattdessen erwartet Einzelbilder übergeben werden. Hierzu können wir durch die Video-Frame-Gate-Komponente.

![JPG-Encoder herstellen ein Frame Tor](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*JPG-Encoder herstellen ein Frame Tor*

Frame-Gate so vielen Sekunden oder Frames Videoframe übergeben können. Das Intervall und der offset mit dem in diesem Fall lässt sich die Eigenschaften.

![Frame-Gate Videoeigenschaften](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Frame-Gate Videoeigenschaften*

Erstellen einer Miniaturansicht minütlich durch Festlegen des Modus (Sekunden) und das Intervall auf 60.

###<a id="thumbnails_to__multibitrate_MP4_color_space"></a>Umgang mit Farbraum konvertieren

Während es logisch wäre, dass beide Stifte nicht komprimierte Video Frame Gate und der Datei Media jetzt verbunden werden können, erhalten wir eine Warnung, wenn wir das tun würden.

![Eingabefarben Fehlermeldung](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Eingabefarben Fehlermeldung*

Ist die Farbe Informationen in unserer ursprünglichen raw dekomprimiert Videostream dargestellt wird aus unserem MXF geht anders JPG-Encoder erwarten. Genauer gesagt, eine so genannte "Farbraum" "RGB" oder "Graustufen" soll fließen. Dies bedeutet, dass eingehende Videostream Video Frame Gate eine Konvertierung zu dem Farbraum zuerst angewendet muss.

Ziehen Sie den Workflow Farbe Speicherplatz Konverter - Intel und das Tor Frame an.

![Verbinden einer Farbe Raum-Konverter](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Verbinden einer Farbe Raum-Konverter*

Wählen Sie im Eigenschaftenfenster BGR 24 Eintrag aus der Liste aus.

###<a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Schreiben von Miniaturansichten

Sich von unserem MP4-Video Encoder JPG-Komponente mehr als eine Datei ausgegeben wird. Zur Bewältigung dieser Komponente Szene Suchdiensts JPG-Datei verwendet werden: Es werden eingehenden JPG-Miniaturansichten und Schreiben sie jeden Dateinamen durch verschiedene Suffix. (Die Anzahl in der Regel, die Anzahl der Sekunden-Einheiten im Stream die Miniaturansicht aus wurde.)


![Einführung in die Szene JPG-Datei Suchdiensts](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Einführung in die Szene JPG-Datei Suchdiensts*

Mit der Eigenschaft Ausgabepfad Ordner konfigurieren: ${ROOT_outputWriteDirectory} 

und das Präfix für Dateinamen mit: 

    ${ROOT_sourceFileBaseName}_thumb_

Das Präfix bestimmt, wie die Miniaturansichten Dateien benannt werden. Sie können eine Zahl der Ziehpunkt Position im Stream Prüfpunktklausel angefügt werden.


![Szene Suchdiensts JPG-Datei Eigenschaften](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Szene Suchdiensts JPG-Datei Eigenschaften*

Verbinden Sie die Szene JPG-Datei Suchdiensts Ausgabe Datei/Anlage Knoten.

###<a id="thumbnails_to__multibitrate_MP4_errors"></a>Erkennen von Fehlern in einem workflow

Die Eingabe des Konverters Space Farbe mit dem raw dekomprimiert Videoausgang anschließen Führen Sie einen lokalen Testlauf für den Workflow jetzt aus. Gibt es wahrscheinlich Workflow plötzlich beendet und mit einem roten Umriss auf die Komponente, die Fehler anzuzeigen:

![Farbe-Platz-Konverter-Fehler](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Farbe-Platz-Konverter-Fehler*

Klicken Sie auf das kleine rote "E" Symbol in der oberen rechten Ecke der Komponente Color Space Konverter Grund Codierung Versuch ist fehlgeschlagen.

![Dialogfeld Farbe Raum-Konverter](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Dialogfeld Farbe Raum-Konverter*

Es stellt sich heraus, wie Sie sehen können, hat der eingehenden Farbraum für Konverter Speicherplatz Farbe zu rec601 für unsere angeforderte Konvertierung YUV in RGB. Offenbar anzugeben nicht unsere Stream des rec601. (Rec 601 ist ein Standard für die Codierung interlaced Videosignale in digitale video-Form. Es gibt einen aktiven Bereich für 720 Luminanz und 360 Chrominanz Proben pro Zeile an. Codierung System Farbe gilt als YCbCr 4:2:2.)

Um dieses Problem zu beheben, werden Metadaten unseres an, denen geht mit rec601 Inhalt. Dazu verwenden wir eine Komponente Video Data Type Updater die setzen wir unsere reine und die Farbkomponente Leerzeichen konvertieren. Dieser Daten Typ Updater ermöglicht die manuelle Aktualisierung bestimmter Videodaten Eigenschaften. Konfigurieren einer Farbe Raum Standard "Rec 601" anzugeben. Dadurch Video Data Type Updater kennzeichnen den Stream mit "Rec 601" Farbraum wäre kein Farbraum definiert. (Wird keine vorhandenen Metadaten überschrieben, wenn die Überschreibung aktiviert wurde.)

![Farbe Raum Standard Data Type Updater aktualisieren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Farbe Raum Standard Data Type Updater aktualisieren*

###<a id="thumbnails_to__multibitrate_MP4_finish"></a>Workflow abgeschlossen

Da unser Workflow abgeschlossen ist, führen Sie einen weiteren Test ausführen zu übergeben.

![Fertige Workflow für Multi-mp4-Ausgabe mit Miniaturansichten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Fertige Workflow für Multi-mp4-Ausgabe mit Miniaturansichten*

##<a id="time_based_trim"></a>Zeitbasierte verkürzen Multibitrate MP4-Ausgabe

Ein Workflow, der generiert [Multibitrate MP4-Ausgabe eine MXF Eingabe](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)ab, werden wir jetzt Suchen in das Quellvideo basierend auf Zeitstempel zuschneiden.

###<a id="time_based_trim_start"></a>Übersicht über Workflow hinzufügen mit

![Starten Workflows mit hinzufügen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Starten Workflows mit hinzufügen*

###<a id="time_based_trim_use_stream_trimmer"></a>Stream Trimmer verwenden

Stream Trimmer Komponente ermöglicht den Anfang und das Ende eines Eingabestreams auf Anzeigedauerinformationen (Sekunden, Minuten,) verkürzt. Frame-basierte Trimmen unterstützt Trimmer nicht.

![Stream Trimmer](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Stream Trimmer*

Statt verknüpfen AVC-Encoder und Lautsprecher Position Auftraggeber Dateieingabe Medien direkt, setzen wir zwischen den Stream Trimmer. (Eine für das Videosignal und Interleaving Audiosignal.)

![Stream Trimmer zwischen speichern](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Stream Trimmer zwischen speichern*

Trimmer konfigurieren wir, dass wir nur Video und Audio zwischen 15 und 60 Sekunden im Video verarbeitet.

Öffnen Sie die Eigenschaften des Video-Stream Trimmer und konfigurieren Sie Startzeit (15) und Endzeit (60er Jahre) Eigenschaften. Stellen Sie sicher, dass sowohl unsere Audio- und Trimmer immer dieselbe Anfangs-und Endwerte konfiguriert sind, veröffentlichen wir in den Stamm des Workflows.

![Start Time-Eigenschaft aus Stream Trimmer veröffentlichen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Start Time-Eigenschaft aus Stream Trimmer veröffentlichen*

![Eigenschaftendialogfeld für Startzeit veröffentlichen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Eigenschaftendialogfeld für Startzeit veröffentlichen*

![Eigenschaftendialogfeld für Endzeit veröffentlichen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Eigenschaftendialogfeld für Endzeit veröffentlichen*


Wenn wir jetzt den Stamm des Workflow untersuchen, werden beide Eigenschaften konfigurierbar und übersichtlich angezeigt aus.

![Veröffentlichte Eigenschaften auf Stamm](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Veröffentlichte Eigenschaften auf Stamm*

Jetzt öffnen Sie die Eigenschaften Zuschneiden von Audiodaten Trimmer und konfigurieren Sie Start-und Endzeit mit einem Ausdruck auf die veröffentlichten Eigenschaften im Stammverzeichnis des Workflow.

Für die Startzeit audio Trimmen:

    ${ROOT_TrimmingStartTime}

und für die Endzeit:

    ${ROOT_TrimmingEndTime}

###<a id="time_based_trim_finish"></a>Workflow abgeschlossen

![Workflow abgeschlossen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Workflow abgeschlossen*


##<a id="scripting"></a>Einführung in skriptgesteuerte Komponente

Skriptgesteuerte Komponenten können Workflow Phasen Ausführung beliebiger Skripts ausführen. Es gibt vier verschiedene Skripts ausgeführt werden können mit bestimmten Eigenschaften und ihren Platz in der Workflow-Lebenszyklus:

- **commandScript**
- **realizeScript**
- **processInputScript**
- **lifeCycleScript**

Die Dokumentation der Komponente Skript geht ausführlicher für die oben. Im [folgenden Abschnitt](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)wird die Skriptkomponente **RealizeScript** Cliplist Xml dynamisch erstellt, wenn der Workflow startet verwendet. Dieses Skript wird während der Installation der Komponente aufgerufen, des Lebenszyklus nur einmal passiert.


###<a id="scripting_hello_world"></a>Skripts in einem Workflow: Hello World

Skript-Komponente auf die Designeroberfläche ziehen Sie und Umbenennen Sie (z. B. "SetClipListXML").

![Hinzufügen einer skriptbasierten Komponente](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Hinzufügen einer skriptbasierten Komponente*

Beim Überprüfen der Eigenschaften der Komponente geskriptet werden vier verschiedenen Skripttypen angezeigt, jeweils ein anderes Skript konfiguriert.

![Skriptgesteuerte Komponenteneigenschaften](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Skriptgesteuerte Komponenteneigenschaften*

Deaktivieren Sie die ProcessInputScript und öffnen Sie den Editor für die RealizeScript. Wir sind nun bereit Einstieg in Skripts festlegen.

Skripts sind in Groovy, einer dynamisch kompilierten Skriptsprache für die Java-Plattform geschrieben, die Kompatibilität mit Java behält. Tatsächlich ist die meisten Java Code gültig kreative.

Schreiben wir nun ein einfaches Hello World einzigartige Skript im Rahmen unserer RealizeScript. Geben Sie im Editor Folgendes ein:

    node.log("hello world");

Führen Sie jetzt einen lokalen Testlauf. Untersuchen Sie nach dieser Ausführung (über die Registerkarte System Skript Komponente) der Protokolle.

![Hello World Ausgabe](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Hello World Ausgabe*

Nennen wir die Methode Log, Node-Objekt bezieht sich auf unsere aktuellen "Knoten" oder die Komponente, die wir in Skripts sind. Jede Komponente hat die Möglichkeit, Protokollierung Ausgabedaten, über die Registerkarte System. In diesem Fall wir das Zeichenfolgenliteral "Hello World" ausgeben. Wichtig zu verstehen, dass dies debugging unbezahlbar, Einblick sein kann in das Skript tatsächlich ausgeführten mit.

Von können in die Skriptingumgebung wir Zugriff auf Eigenschaften für andere Komponenten. Versuchen Sie Folgendes:


    //inspect current node: 
    def nodepath = node.getNodePath(); 
    node.log("this node path: " + nodepath);
    
    //walking up to other nodes: 
    def parentnode = node.getParentNode(); 
    def parentnodepath = parentnode.getNodePath(); 
    node.log("parent node path: " + parentnodepath);
    
    //read properties from a node: 
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null ); 
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null); 
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

Unsere Protokollfenster zeigen uns Folgendes:

![Ausgabe auf Knotenpfade](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Ausgabe auf Knotenpfade*


##<a id="frame_based_trim"></a>Frame-basierte Trimmen Multibitrate MP4-Ausgabe

Ein Workflow, der generiert [Multibitrate MP4-Ausgabe eine MXF Eingabe](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)ab, werden wir jetzt Suchen in das Quellvideo basierend auf Frame zählt zuschneiden.

###<a id="frame_based_trim_start"></a>Hinzufügen mit Blueprints im Überblick

![Workflow hinzufügen mit](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Workflow hinzufügen mit*

###<a id="frame_based_trim_clip_list"></a>Mithilfe der Clipliste XML

In allen vorherigen Workflow Lernprogramme verwendet Media Datei Input-Komponente als unsere Videoquellen. Für dieses spezielle Szenario, verwenden wir die Komponente Listenquelle Clip stattdessen. Beachten Sie, dass dies nicht das bevorzugte Arbeitsweise. Quelle der Clip nur verwenden, wenn ein Grund dafür besteht (wie in den folgenden Fall, in dem wir machen mit Clip Liste verkürzen Funktionen).

Zum Wechseln vom Media Dateieingabe an die Quelle Clip ziehen Sie Komponente Listenquelle Clip auf die Entwurfsoberfläche und Clip Liste XML-Knoten von Workflowdesigner Clip Liste XML-Pin an. Dies sollte die Quelle mit Ausgabepins, Clips nach video Eingabe füllen. Jetzt verbinden die Pins nicht komprimierte Video- und Audiodaten dekomprimiert die Quelle der Clip zu der jeweiligen AVC-Encoder Audio Stream Interleaver. Entfernen der Datei Media.

![Dateieingabe Medien ersetzt durch die Quelle Clip](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Dateieingabe Medien ersetzt durch die Quelle Clip*

Komponente Listenquelle Clips akzeptiert als Eingabe ein "Clip Liste XML". Wenn die Quelldatei lokal testen ist dieser Clip Liste Xml automatisch für Sie.

![Automatisch Clip Liste XML-Eigenschaft](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Automatisch Clip Liste XML-Eigenschaft*

XML etwas genauer anschauen, sieht dies wie:

![Clip Liste Dialogfeld bearbeiten](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Clip Liste Dialogfeld bearbeiten*

Dies spiegelt jedoch nicht die Funktionen des XML-Clip-Liste. Eine Möglichkeit haben wir ist ein "Trim" Element unter sowohl die Video- und Quelle wie folgt hinzufügen:

![Der Clipliste hinzufügen trim element](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Der Clipliste hinzufügen trim element*

Wenn Sie Clip Liste Xml wie vor ändern und einen lokalen Testlauf, sehen Sie das Video korrekt zwischen 10 und 20 Sekunden im Video entfernt.

Gegen was bei lokalen Ausführung aber tun müssen diese gleichen Cliplist Xml nicht den gleichen Effekt, wenn in einem Workflow angewendet, die in Azure Media Services ausgeführt wird. Beginn Azure Premium Encoder wird Xml Cliplist jedes Mal erneut, die Datei basierend auf die Codierungsauftrags angegeben wurde. Dies bedeutet, dass Änderungen auf Xml leider überschrieben würde.

Gegen Cliplist Xml bereinigt wird beim Starten eines Codierungsauftrags können wir dynamisch nach Beginn der Workflow erneut generieren. Benutzerdefinierten Aktionen können durch ein "Skript-Komponente" nennt gefasst. Weitere Informationen finden Sie unter [Einführung in skriptgesteuerte Komponente](media-services-media-encoder-premium-workflow-tutorials.md#scripting).


Skript-Komponente auf die Designeroberfläche ziehen Sie und benennen Sie sie in "SetClipListXML".

![Hinzufügen einer skriptbasierten Komponente](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Hinzufügen einer skriptbasierten Komponente*

Beim Überprüfen der Eigenschaften der Komponente geskriptet werden vier verschiedenen Skripttypen angezeigt, jeweils ein anderes Skript konfiguriert.

![Skriptgesteuerte Komponenteneigenschaften](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Skriptgesteuerte Komponenteneigenschaften*


###<a id="frame_based_trim_modify_clip_list"></a>Ändern der Liste Clips aus einer Komponente Skript

Bevor wir Cliplist Xml neu schreiben können, das beim Start des Workflows generiert wird, müssen wir Cliplist XML-Eigenschaft und Inhalte zugreifen. Wir können dies wie folgt tun:

    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Liste der eingehenden Clips angemeldet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Liste der eingehenden Clips angemeldet*

Zuerst benötigen wir eine Möglichkeit, ab welchem Zeitpunkt bestimmen, bis zu welchem Zeitpunkt wir das Video zuschneiden. Dazu bequem weniger technische Benutzer des Workflows, veröffentlichen Sie zwei Eigenschaften Stammobjekt des Diagramms. Dazu klicken Sie mit der rechten Maustaste die Designeroberfläche und wählen Sie "Eigenschaft hinzufügen":

- Erste: "ClippingTimeStart" Typ: "TIMECODE"
- Zweite Eigenschaft: "ClippingTimeEnd" Typ: "TIMECODE"

![Eigenschaftendialogfeld für Clipping Startzeit hinzufügen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Eigenschaftendialogfeld für Clipping Startzeit hinzufügen*

![Clipping Zeit Props Workflow Stamm veröffentlicht](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Clipping Zeit Props Workflow Stamm veröffentlicht*

Konfigurieren Sie beide Eigenschaften auf einen geeigneten Wert:

![Konfigurieren der Clipping und die end-Eigenschaft](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Konfigurieren der Clipping und die end-Eigenschaft*

Jetzt können unsere Skript wir beide Eigenschaften wie folgt zugreifen:

    
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Fenster mit Start- und Clipping](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Fenster mit Start- und Clipping*

Wir analysieren die Timecode-Zeichenfolgen in ein einfacher, mit einem einfachen regulären Ausdruck verwenden:
    
    //parse the start timing: 
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1); 
    node.log("timecode start is: " + starttimecode); 
    def startframerate = startregresult.group(2); 
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend); 
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2); 
    node.log("framerate end is: " + endframerate);

![Fenster mit analysierten timecode](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Fenster mit analysierten timecode*

Mit diesen Informationen können wir jetzt Cliplist Xml entsprechend die Start- und Endzeiten für die gewünschten Frame genaue Abschneiden des Films.

![Skriptcode Elemente hinzufügen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Skriptcode Elemente hinzufügen*

Dies erfolgte durch normale Zeichenfolge-Bearbeitungsoperationen. Resultierenden Clips Liste Xml ist der ClipListXML-Eigenschaft im Workflow Stamm über die Methode "SetProperty" zurückgeschrieben. Im Protokollfenster nach einem anderen Test würde uns Folgendes anzeigen:

![Protokollierung der Ergebnisliste clip](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Protokollierung der Ergebnisliste clip*

Führen Sie einen Testlauf, um anzuzeigen, wie die Video- und Audiostreams abgeschnitten wurde. Können mehrere Testlauf mit unterschiedlichen Werten für die Zuschneiden-Punkte werden feststellen Sie, dass die nicht jedoch berücksichtigt werden. Der Grund dafür ist, dass Designer im Gegensatz zu Azure Runtime nicht Xml Cliplist jedem überschreibt. Das bedeutet, die nur beim allerersten und Punkt einstellen, die ansonsten unsere Schutzklausel transformiert Xml führen (Wenn (clipListXML.indexOf ("<trim>") ==-1)) wird verhindert, dass den Workflow Trimmen eines anderen Elements hinzufügen, wenn bereits vorhanden.

Um Workflow zu lokal testen hinzuzufügen, fügen wir Haushalt Code, der überprüft, wenn ein Trimmen Element bereits vorhanden ist. In diesem Fall können wir es entfernen, bevor Sie fortfahren, durch Ändern der XML-mit den neuen Werten. Anstatt nur Zeichenfolgen ist es wahrscheinlich sicherer dazu durch Analysieren von tatsächlichen XML-Objektmodell.

Bevor wir jedoch Code hinzufügen können, müssen wir zuerst importieren Aussagen am Anfang des Skripts hinzuzufügen:
    
    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;

Danach können wir den Reinigung Code hinzufügen:

    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already: 
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes: 
    elementsToDelete.each{ 
        e -> e.getParentNode().removeChild(e);
    }; 
    node.log("deleted any existing trim nodes");
    
    //serialize the modified clip list xml dom into a string: 
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result); 
    clipListXML = result.getWriter().toString();
    
Dieser Code wird nur über dem Punkt, an dem wir die Elemente Cliplist Xml hinzufügen.

Wir können jetzt ausführen und Workflow so viel wie wir wollen und die Änderungen immer ändern.    

###<a id="frame_based_trim_clippingenabled_prop"></a>Hinzufügen einer ClippingEnabled-Eigenschaft

Da nicht immer empfiehlt zu trimmen, wir abschließend Workflow hinzufügen eine bequeme boolesches Flag, der angibt, ob wir Zuschneiden / clipping aktivieren möchten.

Veröffentlichen Sie wie vor, eine neue Eigenschaft in das Stammverzeichnis des Workflow namens "ClippingEnabled" des Typs "BOOLEAN".

![Veröffentlicht eine Eigenschaft für das Clipping aktivieren](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Veröffentlicht eine Eigenschaft für das Clipping aktivieren*

Mit der unter einfache Schutzklausel wir ob verkürzen erforderlich ist und ob unsere Clipliste muss so geändert werden kann.

    //check if clipping is required: 
    def clippingrequired = node.getProperty("../ClippingEnabled"); 
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML); 
        node.log("no clipping required"); 
        return; 
    }


###<a id="code"></a>Vollständiges Codebeispiel

    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;
    
    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping: 
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);
    
    //for local testing: delete any pre-existing trim elements 
    //from the clip list xml by parsing the xml into a DOM:
    
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder(); 
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath(); 
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }
    
    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e -> 
        e.getParentNode().removeChild(e); 
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return; 
    }

    //add trim elements to cliplist xml 
    if ( clipListXML.indexOf("<trim>") == -1 ) 
    {
        //trim video 
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode + 
            " </outPoint>\n </trim> \n"); 
        //trim audio 
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" + 
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML ); 
        node.setProperty("../clipListXml",clipListXML); 
    }


##<a name="also-see"></a>Siehe auch 

[Einführung in Premium-Codierung in Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Wie Sie Premium Codierung in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[On-Demand-Inhalte mit Azure Media Service-Codierung](media-services-encode-asset.md#media_encoder_premium_workflow)

[Media Encoder Premium-Workflow-Formate und Codecs](media-services-premium-workflow-encoder-formats.md)

[Workflow-Beispieldateien](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services-Explorertools](http://aka.ms/amse)

##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
