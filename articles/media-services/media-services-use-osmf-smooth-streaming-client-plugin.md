<properties 
    pageTitle="Reibungslose Streaming-Plugin für Open Source Media Framework" 
    description="Informationen Sie zum Azure Media Services Smooth Streaming-Plugin für Adobe Open Source Media Framework verwenden." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="how-to-use-the-microsoft-smooth-streaming-plugin-for-the-adobe-open-source-media-framework"></a>Wie Microsoft Smooth Streaming-Plugin für Adobe Open Source Media Framework

##<a name="overview"></a>Übersicht


Microsoft Smooth Streaming-Plugin für Open Source Media Framework 2.0 (SS OSMF) erweitert die Standardfunktionen von OSMF und fügt Microsoft Smooth Streaming-Wiedergabe für neue und vorhandene OSMF. Das Plug-in fügt auch Wiedergabefunktionen Smooth Streaming auf Strobe Media Wiedergabe (SMP).

SS OSMF enthält zwei Versionen von Plugin:

- Statische Smooth Streaming-Plugin für OSMF (.swc)

- Dynamische Smooth Streaming-Plugin für OSMF (. SWF)

Dieses Dokument geht davon aus, dass der Leser eine allgemeine Kenntnisse von OSMF und OSMF-Plug-ins. Weitere Informationen zu OSMF finden Sie die Dokumentation auf der [Website OSMF](http://osmf.org/).

###<a name="smooth-streaming-plugin-for-osmf-20"></a>Streaming-Plugin für OSMF 2.0 Glätten

Das Plug-in unterstützt laden und bei Bedarf Smooth Streaming-Inhalte mit den folgenden Features:

- Bei Bedarf Smooth Streaming abspielen (Play, Pause, suchen, beenden)
- Live Smooth Streaming abspielen (Play)
- Live DVR-Funktionen (Pause, Seek, DVR Wiedergabe Go-to-Live)
- Unterstützung für video-Codecs - 264
- Unterstützung für Audio-Codecs - AAC
- Mehrere Sprache mit integrierten OSMF-APIs
- Max. Wiedergabe Qualität Auswahl mit integrierten OSMF-APIs
- Untertitel mit OSMF Untertitel Plugin Beiwagen
- Adobe&reg; Flash&reg; Player 11.4 oder höher.
- Diese Version unterstützt nur OSMF 2.0.

## <a name="supported-features-and-known-issues"></a>Unterstützte Funktionen und bekannte Probleme

Eine vollständige Liste der unterstützten Funktionen nicht unterstützte Funktionen und bekannte Probleme finden Sie in [diesem Dokument](http://download.microsoft.com/download/3/1/B/31B63D97-574E-4A8D-BF8D-170744181724/Smooth_Streaming_Plugin_for_OSMF.pdf).


## <a name="loading-the-plugin"></a>Laden des Plug-Ins
OSMF Plug-Ins laden Sie statisch (zur Kompilierungszeit) oder dynamisch (zur Laufzeit). Smooth Streaming-Plugin für OSMF-Download enthält sowohl statische als auch dynamische Versionen.

- Statik: statisch Laden ist eine statische Bibliothek (SWC) erforderlich. Statische Plugins werden als Verweis auf Projekte und Zusammenführen in der endgültigen Ausgabedatei der Kompilierzeit hinzugefügt.

- Dynamisches Laden: zum dynamischen Laden von vorkompilierte (SWF) Datei erforderlich ist. Dynamische Plugins in die Laufzeit geladen und nicht in die Projektausgabe einbezogen. (Kompilierte Ausgabe) HTTP- und Protokolle können dynamische Plugins geladen werden.

Weitere Informationen zu statischen und dynamischen Laden finden Sie unter der amtlichen [OSMF Plugin-Seite](http://osmf.org/dev/osmf/OtherPDFs/osmf_plugin_dev_guide.pdf).

###<a name="ss-for-osmf-static-loading"></a>SS Statik OSMF
Der folgende Codeausschnitt veranschaulicht die Plugin SS OSMF statisch geladen und grundlegende Wiedergabe mit OSMF MediaFactory-Klasse. Vor der SS für OSMF Code, stellen Sie sicher, der Projektverweis "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" statische Plugin enthält.

```
package 
{
    
    import com.microsoft.azure.media.AdaptiveStreamingPluginInfo;
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    
    
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        

        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;

            initMediaPlayer();

        }
    
        private function initMediaPlayer():void
        {
        
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);
            _mediaPlayerSprite.scaleMode = ScaleMode.NONE;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            
            pluginResource = new PluginInfoResource(new AdaptiveStreamingPluginInfo( )); 
            _mediaFactory.loadPlugin( pluginResource ); 
        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.
            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.
        loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}
```


###<a name="ss-for-osmf-dynamic-loading"></a>SS OSMF dynamisches Laden

Der folgende Codeausschnitt veranschaulicht Plugin SS OSMF dynamisch geladen und spielen ein video mit OSMF MediaFactory-Klasse. Kopieren Sie vor der SS für OSMF Code "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" dynamische Plug-in zum Projektordner Datei Protokoll geladen oder unter einem Webserver HTTP-Auslastung kopieren. Es muss keine "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swc" in den Projektverweisen enthalten.

 
Paket {
    
    import flash.display.*;
    import org.osmf.media.*;
    import org.osmf.containers.MediaContainer;
    import org.osmf.events.MediaErrorEvent;
    import org.osmf.events.MediaFactoryEvent;
    import org.osmf.events.MediaPlayerStateChangeEvent;
    import org.osmf.layout.*;
    import flash.events.Event;
    import flash.system.Capabilities;

    
    //Sets the size of the SWF
    
    [SWF(width="1024", height="768", backgroundColor='#405050', frameRate="25")]
    public class TestPlayer extends Sprite
    {        
        public var _container:MediaContainer;
        public var _mediaFactory:DefaultMediaFactory;
        private var _mediaPlayerSprite:MediaPlayerSprite;
        
        
        public function TestPlayer( )
        {
            stage.quality = StageQuality.HIGH;
            initMediaPlayer();
        }
        
        private function initMediaPlayer():void
        {
            
            // Create the container (sprite) for managing display and layout
            _mediaPlayerSprite = new MediaPlayerSprite();    
            _mediaPlayerSprite.addEventListener(MediaErrorEvent.MEDIA_ERROR, onPlayerFailed);
            _mediaPlayerSprite.addEventListener(MediaPlayerStateChangeEvent.MEDIA_PLAYER_STATE_CHANGE, onPlayerStateChange);

            //Adds the container to the stage
            addChild(_mediaPlayerSprite);
            
            // Create a mediafactory instance
            _mediaFactory = new DefaultMediaFactory();
            
            // Add the listeners for PLUGIN_LOADING
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD,onPluginLoaded);
            _mediaFactory.addEventListener(MediaFactoryEvent.PLUGIN_LOAD_ERROR, onPluginLoadFailed );
            
            // Load the plugin class 
            loadAdaptiveStreamingPlugin( );  
            
        }
        
        private function loadAdaptiveStreamingPlugin( ):void
        {
            var pluginResource:MediaResourceBase;
            var adaptiveStreamingPluginUrl:String;

            // Your dynamic plugin web server needs to host a valid crossdomain.xml file to allow loading plugins.

            adaptiveStreamingPluginUrl = "http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf";
            pluginResource = new URLResource(adaptiveStreamingPluginUrl);
            _mediaFactory.loadPlugin( pluginResource ); 

        }
        
        private function onPluginLoaded( event:MediaFactoryEvent ):void
        {
            // The plugin is loaded successfully.

            // Your web server needs to host a valid crossdomain.xml file to allow plugin to download Smooth Streaming files.

    loadMediaSource("http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest")
        }
        
        private function onPluginLoadFailed( event:MediaFactoryEvent ):void
        {
            // The plugin is failed to load ...
        }
        
        
        private function onPlayerStateChange(event:MediaPlayerStateChangeEvent) : void
        {
            var state:String;
            
            state =  event.state;
            
            switch (state)
            {
                case MediaPlayerState.LOADING: 
                    
                    // A new source is started to load.
                    
                    break;
                
                case  MediaPlayerState.READY :   
                    // Add code to deal with Player Ready when it is hit the first load after a source is loaded. 
                    
                    break;
                
                case MediaPlayerState.BUFFERING :
                    
                    break;
                
                case  MediaPlayerState.PAUSED :
                    break;      
                // other states ...          
            }
        }
        
        private function onPlayerFailed(event:MediaErrorEvent) : void
        {
            // Media Player is failed .           
        }
        
        private function loadMediaSource(sourceURL : String):void 
        {
            // Take an URL of SmoothStreamingSource's manifest and add it to the page.
            
            var resource:URLResource= new URLResource( sourceURL );
            
            var element:MediaElement = _mediaFactory.createMediaElement( resource );
            _mediaPlayerSprite.scaleMode = ScaleMode.LETTERBOX;
            _mediaPlayerSprite.width = stage.stageWidth;
            _mediaPlayerSprite.height = stage.stageHeight;
            // Add the media element
            _mediaPlayerSprite.media = element;
        }     
        
    }
}

##<a name="strobe-media--playback-with-the-ss-odmf-dynamic-plugin"></a>Medienwiedergabe mit dem dynamischen Plugin SS ODMF Stroboskop

Smooth Streaming für dynamische OSMF-Plugin ist kompatibel mit [Stroboskop Media Wiedergabe (SMP)](http://osmf.org/strobe_mediaplayback.html). OSMF Plugin SS können SMP Smooth Streaming Wiedergabe hinzu. Kopieren Sie hierzu "MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf" unter einem Webserver HTTP Laden mithilfe der folgenden Schritte:

1.  Durchsuchen Sie die [Einrichtungsseite Stroboskop Medienwiedergabe](http://osmf.org/dev/2.0gm/setup.html). 
2.  Legen Sie das Src Smooth Streaming-Quelle (z.B. http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest) 
3.  Ändern Sie Konfiguration, und klicken Sie auf Vorschau und Update.
 
    **Hinweis** Content Webservers erforderlich gültige crossdomain.xml. 
4.  Kopieren Sie und fügen Sie den Code einer einfachen HTML-Seite verwenden einen Texteditor, wie im folgenden Beispiel:



        <html>
        <body>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest &autoPlay=true"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars=" src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true">
        </embed>
        </object>
        </body>
        </html>



5. Der Einbettungscode glatte OSMF Streaming-Plug-In hinzufügen und speichern.

        <html>
        <object width="920" height="640"> 
        <param name="movie" value="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf"></param>
        <param name="flashvars" value="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10"></param>
        <param name="allowFullScreen" value="true"></param>
        <param name="allowscriptaccess" value="always"></param>
        <param name="wmode" value="direct"></param>
        <embed src="http://osmf.org/dev/2.0gm/StrobeMediaPlayback.swf" 
            type="application/x-shockwave-flash" 
            allowscriptaccess="always" 
            allowfullscreen="true" 
            wmode="direct" 
            width="920" 
            height="640" 
            flashvars="src=http://devplatem.vo.msecnd.net/Sintel/Sintel_H264.ism/manifest&autoPlay=true&plugin_AdaptiveStreamingPlugin=http://yourdomain/MSAdaptiveStreamingPlugin-v1.0.3-osmf2.0.swf&AdaptiveStreamingPlugin_retryLive=true&AdaptiveStreamingPlugin_retryInterval=10">
        </embed>
        </object>
        </html>


6.  Die HTML-Seite speichern und auf einem Webserver veröffentlichen. Navigieren Sie zu der veröffentlichten Webseite mit Ihrem bevorzugten Flash&reg; Player aktiviert Internetbrowser (Internet Explorer, Chrome, Firefox, usw.).
7.  Smooth Streaming-Inhalte in Adobe&reg; Flash&reg; Player.

Weitere Informationen zu allgemeinen OSMF Entwicklung finden Sie unter der amtlichen [OSMF der Seite Development](http://osmf.org/resources.html).

##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Siehe auch

[Microsoft Adaptive Streaming-Plugin für OSMF aktualisieren](https://azure.microsoft.com/blog/2014/10/27/microsoft-adaptive-streaming-plugin-for-osmf-update/) 
