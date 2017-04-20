<properties 
    pageTitle="Einbetten von MPEG-DASH Adaptives Streaming-Video in HTML5 Anwendung mit DASH.js | Microsoft Azure" 
    description="In diesem Thema veranschaulicht, wie ein MPEG-DASH Adaptive Streaming Video in HTML5 Anwendung mit DASH.js eingebettet." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="embedding-a-mpeg-dash-adaptive-streaming-video-in-an-html5-application-with-dashjs"></a>Einbetten von MPEG-DASH Adaptives Streaming-Video in HTML5 Anwendung mit DASH.js

##<a name="overview"></a>Übersicht

MPEG-DASH ist ein ISO-Standard für adaptives streaming von Videoinhalten, bietet erhebliche Vorteile für die Ausgabe von hoher Qualität, adaptive Videostreaming übermitteln möchten. Mit MPEG-DASH werden Videodaten automatisch zur Definition einer niedrigeren gelöscht, wenn das Netzwerk überlastet wird. Dies verringert die Wahrscheinlichkeit des Viewers "unterbrochen" Video sehen, während der Player die nächsten Sekunden abspielen (aka Pufferung) heruntergeladen. Überlastung des Netzwerks verringert wird der video-Player wiederum eine höhere Qualität Stream zurück. Schneller Start Video führt die Möglichkeit, die erforderliche Bandbreite anpassen. Dies bedeutet, dass die ersten Sekunden in einem qualitätssegment Fast Download niedriger wiedergegeben werden und dann auf eine höhere Qualität Content einmal ausreichend gepuffert wurde.

Dash.js ist ein open-Source MPEG-DASH video-Player in JavaScript geschrieben. Ziel ist einen stabilen, plattformübergreifende Player bereitstellen, der frei in Anwendung wiederverwendet werden kann, die Videowiedergabe benötigen. Es bietet MPEG-DASH Wiedergabe in jedem Browser, der heute das W3C Media Quelle Extensions (MSE) unterstützt das Chrom, Microsoft Edge und IE11 (andere Browser haben ihre Absicht zur Unterstützung von MSE angegeben). Weitere Informationen zu DASH.js finden Sie unter Js Repository dash.js GitHub.


##<a name="creating-a-browser-based-streaming-video-player"></a>Erstellen einen Browser-basierte streaming video-player

Um eine einfache Webseite erstellen, die einen video-Player mit dem erwarteten zeigt Steuerelemente so eine wiedergeben, anhalten, Zurückspulen etc. werden sollen:

1. Erstellen Sie eine HTML-Seite
1. Video-Tag hinzufügen
1. Dash.js-Spieler hinzufügen
1. Initialisieren des Players
1. Einige CSS-Stil
1. Zeigen Sie die Ergebnisse in einem Browser, der MSE implementiert

Beim Initialisieren des Players kann in wenigen Zeilen JavaScript-Code erfolgen. Verwenden dash.js, ist es einfach zu MPEG-DASH Video in der Browser-basierte Anwendung einbetten.

##<a name="creating-the-html-page"></a>Die HTML-Seite erstellen

Der erste Schritt ist eine HTML-Standardseite, die das Element **video** speichern als basicPlayer.html, erstellen Sie wie im folgende Beispiel veranschaulicht:

    <!DOCTYPE html>
    <html>
      <head><title>Adaptive Streaming in HTML5</title></head>
      <body>
        <h1>Adaptive Streaming with HTML5</h1>
        <video id="videoplayer" controls></video>
      </body>
    </html>

##<a name="adding-the-dashjs-player"></a>DASH.js-Player hinzufügen

Dash.js Verweis Implementierung der Anwendung hinzufügen, müssen Sie die Datei dash.all.js aus der Version 1.0 von dash.js Projekt erfassen. Dies sollte in der JavaScript-Ordner der Anwendung gespeichert werden. Diese Datei ist eine benutzerfreundliche, der alle erforderlichen dash.js Code zusammen in einer einzigen Datei abruft. Haben Sie sich um das Repository dash.js finden Sie die einzelnen Dateien, Code und vieles mehr testen alle möchten ist mit dash.js und dash.all.js-Datei ist.

Head-Bereich eines basicPlayer.html dash.js Player Anwendung hinzufügen, fügen Sie ein Script-Tag hinzu:

    <!-- DASH-AVC/265 reference implementation -->
    < script src="js/dash.all.js"></script>


Als Nächstes erstellen Sie eine Funktion den Player beim Laden der Seite initialisiert. Fügen Sie das folgende Skript nach dem Laden von dash.all.js Zeile:

    <script>
    // setup the video element and attach it to the Dash player
    function setupVideo() {
      var url = "http://wams.edgesuite.net/media/MPTExpressionData02/BigBuckBunny_1080p24_IYUV_2ch.ism/manifest(format=mpd-time-csf)";
      var context = new Dash.di.DashContext();
      var player = new MediaPlayer(context);
                      player.startup();
                      player.attachView(document.querySelector("#videoplayer"));
                      player.attachSource(url);
    }
    </script>

Diese Funktion erstellt zunächst ein DashContext. Hiermit wird die Anwendung einer bestimmten Common Language Runtime-Umgebung konfigurieren. Vom technischen Standpunkt definiert Klassen Dependency Injection-Framework beim Erstellen der Anwendungdes verwendet werden soll. In den meisten Fällen verwenden Sie Dash.di.DashContext.

Instanziieren Sie die primäre Klasse dash.js Framework MediaPlayer Nächstes. Diese Klasse enthält die wichtigsten Methoden wie Wiedergabe anhalten verwaltet die Beziehung mit dem video und verwaltet auch die Interpretation der Datei Media Präsentation Beschreibung (MPD) beschreibt das Video wiedergegeben werden soll.

Die Funktion startup() der MediaPlayer-Klasse wird aufgerufen, um sicherzustellen, dass der Player video wiedergeben kann. Unter anderem sorgt diese Funktion erforderlichen Klassen (definiert durch den Kontext) geladen wurden. Wenn der Player bereit ist, können Sie mithilfe der Funktion attachView() video Element zuordnen. Dadurch können MediaPlayer zu injizieren Videodaten in das Element auch Wiedergabe nach Bedarf.

Damit bekannt ist, über das Video abgespielt werden voraussichtlich übergeben Sie die URL MPD-Datei an MediaPlayer. Die soeben erstellte setupVideo()-Funktion müssen ausgeführt werden, nachdem die Seite vollständig geladen wurde. Geben Sie dazu das Onload-Ereignis des Body-Elements. Ändern der <body> Element:

    <body onload="setupVideo()">

Legen Sie abschließend die Größe des video-Elements mit CSS. In einer Umgebung mit adaptive streaming ist dies besonders wichtig, da die Größe des wiedergegebenen Videos Wiedergabe passt sich ändernden Netzwerk ändern kann. Diese einfache Demo einfach erzwingen Sie das video Element zu 80 % des verfügbaren Browserfensters Head-Bereich der Seite die folgenden CSS hinzufügen:
    
    <style>
    video {
      width: 80%;
      height: 80%;
    }
    </style>

##<a name="playing-a-video"></a>Wiedergeben eines Videos

Zur Wiedergabe eines Videos in Ihrem Browser die Datei basicPlayback.html, und klicken Sie auf den video-Player angezeigt.


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Siehe auch

[Video-Player Anwendungsentwicklung](media-services-develop-video-players.md)

[GitHub dash.js repository](https://github.com/Dash-Industry-Forum/dash.js) 
