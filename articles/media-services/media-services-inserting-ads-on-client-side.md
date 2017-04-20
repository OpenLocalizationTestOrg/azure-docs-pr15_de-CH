<properties 
    pageTitle="Anzeigen auf der Clientseite einfügen | Microsoft Azure" 
    description="In diesem Thema wird gezeigt, wie Werbung auf der Clientseite eingefügt." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="inserting-ads-on-the-client-side"></a>Einfügen von anzeigen auf der Clientseite

Dieses Thema enthält Informationen über verschiedene anzeigen auf der Clientseite einfügen.

Informationen über geschlossene Untertitel und Ad-Unterstützung in Live-Streaming-Videos finden Sie unter [unterstützt Untertitel und Ad einfügen](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

>[AZURE.NOTE] Azure Media Player unterstützt derzeit nicht anzeigen.

##<a id="insert_ads_into_media"></a>Medien anzeigen einfügen

Azure Media Services bietet Unterstützung für Werbung über die Windows Media-Plattform: Player-Frameworks. Frameworks mit Active Directory-Player sind für Windows 8, Silverlight, Windows Phone 8 und iOS-Geräte verfügbar. Jede Player-Framework enthält Beispielcode, der wie eine Player-Anwendung implementiert wird. Es gibt drei verschiedene Arten von Medien: Liste einfügen anzeigen.

- **Linear** – Vollbild anzeigen, die wichtigste Video anhalten.
- **Nonlinear** -Overlay-Anzeigen der wichtigsten Videowiedergabe angezeigt, in der Regel ein Logo oder andere statische Bild im Player.
- **Companion** -anzeigen, die außerhalb des Players angezeigt werden.

Anzeigen können jederzeit das Hauptvideo Zeitleiste platziert werden. Sie müssen dem Player beim Ad spielen und welche anzeigen spielen mitteilen. Dies erfolgt mithilfe einer Reihe von standard-XML-basierte Dateien: Video Ad Service Vorlage (VAST), Digital Video mehrere Ad Wiedergabeliste (VMAP), Media abstrakte Sequenzierung Vorlage (MAST) und Digital Video Player Ad Interface Definition (VPAID). GROßE Dateien angeben, welche anzeigen. VMAP-Dateien festlegen, wann verschiedenen anzeigen und große XML enthalten. MAST Dateien können Sequenz anzeigen, die auch große XML enthalten. VPAID-Dateien definieren Sie eine Schnittstelle zwischen den video-Player, Anzeige und Ad Server.

Jedes Framework Player funktioniert anders und wird jeweils in einem eigenen Thema behandelt. In diesem Thema werden die grundlegenden Mechanismen anzeigen einfügen beschrieben. Video-Player Applikationen anfordern ein Ad-Server anzeigen. Der Ad-Server kann auf verschiedene Arten Antworten:

- Eine große Datei zurückgeben
- Zurückgeben einer VMAP-Datei (mit eingebetteten VAST)
- Eine Rückgabedatei MAST (mit eingebetteten VAST)
- Zurückgeben einer großen Datei mit VPAID anzeigen

 
###<a name="using-a-video-ad-service-template-vast-file"></a>Mithilfe einer Videoanzeige Service-Vorlagendatei (VAST)

Eine große Datei gibt an, welche Anzeige oder anzeigen. Die folgenden XML-Code ist ein Beispiel einer großen Datei für eine lineare Anzeige:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>
    
Lineare Anzeige anhand der **<Linear>** Element. Gibt die Dauer der Anzeige, Verfolgungsereignisse, klicken Sie auf Überwachung und eine Anzahl von **<MediaFile>** Elemente. Überwachungsereignissen werden angegeben, in der **<TrackingEvents>** Element und einen Ad Server auf verschiedene Ereignisse zu verfolgen, die beim Anzeigen der Anzeige auftreten. In diesem Fall starten Mittelpunkt abgeschlossen, und erweitern Sie Ereignisse verfolgt werden. Start-Ereignis tritt ein, wenn die Anzeige. Mittelpunkt-Ereignis tritt ein, wenn mindestens 50 % der Anzeige Zeitachse angezeigt wurde. Das complete-Ereignis tritt ein, wenn die Anzeige zu Ende ausgeführt wurde. Erweitern-Ereignis tritt auf, wenn den video-Player Vollbildmodus erweitern. Klicks mit angegebenen ein **<ClickThrough>** Element innerhalb einer **<VideoClicks>** Element und gibt einen URI zu einer Ressource an, wenn der Benutzer auf die Anzeige klickt. ClickTracking gemäß einer **<ClickTracking>** Element innerhalb der **<VideoClicks>** Element und gibt eine Tracking-Ressource für den Player anfordern, wenn der Benutzer auf die Anzeige klickt. Die **<MediaFile>** Elemente geben Informationen über eine bestimmte Codierung einer. Wenn mehr als eine **<MediaFile>** Element, der video-Player können die beste Codierung für die Plattform. 

Lineare anzeigen können in einer bestimmten Reihenfolge angezeigt werden. Hierzu fügen Sie zusätzliche <Ad> Elemente, die VAST, und geben Sie die Reihenfolge mit den Sequenz-Attribut. Das folgende Beispiel veranschaulicht dies:
    
    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>
    
Nichtlineare anzeigen gemäß einer <Creative> Element. Das folgende Beispiel zeigt eine <Creative> Element, das eine nicht lineare Anzeige beschreibt.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>

 
Die **<NonLinearAds>** Element darf eine oder mehrere **<NonLinear>** Elemente kann jeweils eine nichtlineare Anzeige beschreiben. Die **<NonLinear>** Element gibt die Ressource für die nichtlineare Anzeige an. Die Ressource kann eine **<StaticResouce>**, **<IFrameResource>**, oder **<HTMLResouce>**.**<StaticResource>** nicht-HTML-Ressource beschreibt und definiert ein CreativeType-Attribut, das angibt, wie die Ressource angezeigt wird:

Image-Gif Image/Jpeg, Image/Png-Ressource wird in einem HTML- **<img>** Tag.

Application/X-Javascript-Ressource wird in ein HTML-<**Script**>-Tag angezeigt.

Application/X-Shockwave-Flash-Ressource wird in Flash Player angezeigt.

**<IFrameResource>**Beschreibt eine HTML-Ressource, die in einem IFrame angezeigt werden können. **<HTMLResource>**Beschreibt einen HTML-Codeabschnitt, der in eine Webseite eingefügt werden kann. **<TrackingEvents>**Geben Sie Nachverfolgungsereignisse und URI, bei Eintreten des Ereignisses an. In diesem Beispiel werden die Ereignisse AcceptInvitation und reduzieren verfolgt. Weitere Informationen zu den **<NonLinearAds>** Element und seine untergeordneten Elemente finden Sie unter IAB.NET/VAST. Beachten Sie, dass die **<TrackingEvents>** Element befindet sich in der** <NonLinearAds> ** Element nicht **<NonLinear>** Element.

Companion-anzeigen in definiert eine <CompanionAds> Element. Die <CompanionAds> Element darf eine oder mehrere <Companion> Elemente. Jede <Companion> Element beschreibt eine Companion-Anzeige und enthalten einen <StaticResource>, <IFrameResource>, oder <HTMLResource> , wie eine nicht lineare Anzeige angegeben werden. Eine große Datei kann mehrere Companion anzeigen und die Player-Anwendung können die am besten geeignete Anzeige angezeigt. Weitere Informationen zu VAST finden Sie [UMFANGREICHE 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

###<a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Verwenden digitale Videos mehrere Ad-Wiedergabeliste (VMAP)

Eine VMAP-Datei können Sie angeben, Ad Seitenumbrüche auftreten, wie lange jedes ist, wie viele anzeigen in einer Pause angezeigt werden können und welche Anzeigentypen möglicherweise während einer Unterbrechung angezeigt. In einer VMAP-Beispieldatei, die eine einzelnen Anzeige Pause definiert Folgendes:
    
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>
     
VMAP Datei beginnt mit einem <VMAP> -Element mit mindestens einem <AdBreak> Elemente jedes definieren eine Unterbrechung Ad. Jeder Ad Pause gibt einen Typ des Seitenumbruchs, Break-ID und Zeitversatz. Das BreakType-Attribut gibt den Typ der Anzeige, die während der Unterbrechung wiedergegeben werden kann: lineare, nichtlineare, oder anzeigen. Karte anzeigen Anzeigen große Companion anzeigen. Mehrere Ad-Typ kann in einer Liste mit Kommas (ohne Leerzeichen) angegeben werden. Die BreakID ist eine optionale ID für die Anzeige. Die TimeOffset gibt an, wann die Anzeige angezeigt werden soll. Sie können eine der folgenden Arten angegeben werden:

1. Zeit – im Format hh: mm: oder hh:mm:ss.mmm .mmm Millisekunden ist. Der Wert dieses Attributs gibt die Zeit vom Anfang des Schnittfensters video an Ad Break.
1. Prozent – n % Format ist, wobei n der Prozentsatz der die Videozeitachse vor der Anzeige wiedergegeben
1. Anfang-Ende-gibt an, dass eine Anzeige angezeigt werden soll, vor oder nach das Video angezeigt wurde
1. Position – Gibt die Reihenfolge der Anzeige Pausen beim Ad Pausen unbekannt, wie live-streaming ist. Die Reihenfolge der jeder Pause Ad ist im Format #n angegeben n eine ganze Zahl 1 oder höher ist. 1 bedeutet Anzeige gespielt werden bei der ersten Gelegenheit 2 bedeutet die Anzeige usw. zu zweite gespielt werden.

Innerhalb des Elements <**AdBreak**> kann ein <**AdSource**> Element. <**AdSource**>-Element enthält die folgenden Attribute:

1. ID-gibt einen Bezeichner für Ad-Quelle
1. AllowMultipleAds – ein boolescher Wert, der angibt, ob mehrere anzeigen in der Pause Ad angezeigt werden können
1. FollowRedirects – leitet ein Optionaler boolescher Wert, der angibt, wenn der video-Player berücksichtigen sollten in einer Ad-Antwort

<**AdSource**>-Element bietet dem Player eine Anzeigenerfolgs Inline oder ein Verweis auf eine Ad-Antwort. Sie können eines der folgenden Elemente enthalten:

- <VASTAdData>Gibt an, dass eine große Anzeigenantwort innerhalb der VMAP Datei eingebettet ist
- <AdTagURI>ein URI, der eine Ad-Antwort von einem anderen System verweist
- <CustomAdData>-eine beliebige Zeichenfolge ohne große Antwort darstellt

In diesem Beispiel wird eine Inline-Ad-Antwort angegeben ist ein <VASTAdData> -Element, das eine große Ad-Antwort enthält. Weitere Informationen zu den anderen Elementen finden Sie unter [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

<**AdBreak**>-Element kann auch ein <**TrackingEvents**> Element enthalten. Das Element <**TrackingEvents**> können Sie verfolgen, am Anfang oder Ende einer Ad Pause oder während der Unterbrechung Ad, ob ein Fehler aufgetreten. <**TrackingEvents**>-Element enthält mindestens <**Tracking**> Elemente, die jeweils ein Ereignis Überwachung und Verfolgung URI angibt. Mögliche Nachverfolgungsereignisse sind:

1. BreakStart – verfolgt Anfang einer Ad-Unterbrechung
1. BreakEnd – verfolgen den Abschluss einer Ad-Unterbrechung
1. Fehler verfolgt ein Fehlers in der Ad-Pause

Das folgende Beispiel zeigt eine VMAP, der Überwachungsereignissen angibt

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Weitere Informationen zu <**TrackingEvents**>-Element und seine untergeordneten Elemente finden Sie unter http://iab.org/VMAP.pdf

###<a name="using-a-media-abstract-sequencing-template-mast-file"></a>Mithilfe einer Media abstrakter Sequenzierung Vorlagendatei (MAST)

MAST-Datei können Sie Auslöser angeben, die definieren, wann eine Anzeige angezeigt wird. Im folgenden finden eine MAST-Beispieldatei, die Trigger für eine Pre-Roll-Anzeige eine Mid-Roll-Anzeige und Nachlauf Ad enthält.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
    
        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>

 

MAST-Datei beginnt mit einem **<MAST>** Element, eine enthält **<triggers>** Element. Die <triggers> Element enthält ein oder mehrere **<trigger>** Elemente, wenn eine Anzeige wiedergegeben werden soll. 

Die **<trigger>** Element enthält ein **<startConditions>** Element angeben, wenn eine Anzeige Wiedergabe beginnen soll. Die **<startConditions>** Element enthält ein oder mehrere <condition> Elemente. Bei jeder <condition> ergibt True Trigger initiiert oder widerrufen, je nachdem, ob das <condition> enthalten ist ein **< StartConditions**> oder **<endConditions>** Element bzw.. Wenn mehrere <condition> Elemente vorhanden sind, werden sie eine implizite oder als jede Bedingung Auswertung True bewirkt den Trigger initiiert. <condition>Elemente können geschachtelt werden. Wenn untergeordnete <condition> Elemente sind voreingestellt, als eine implizite und behandelt alle müssen für den Trigger initiiert auf True ergeben. Die <condition> Element enthält die folgenden Attribute, die die Bedingung definieren: 

1. **Typ** – gibt den Typ der Bedingung, ein Ereignis oder Eigenschaft
1. **Name** – der Name der Eigenschaft oder des Ereignisses während der Auswertung verwendet werden
1. **Wert** der Wert eine Eigenschaft ausgewertet
1. **Operator** – der Vorgang bei der Auswertung verwenden: EQ (gleich), NEQ (ungleich), Art (größer), GEQ (größer oder gleich), LT (kleiner als), fahrenden Triebfahrzeugs (kleiner als oder gleich), Rest (modulo)

**<endConditions>**auch <condition> Elemente. Wird zurückgesetzt, wenn eine Bedingung Trigger als true ausgewertet wird. Die <trigger> -Element enthält außerdem ein <sources> -Element, das einem mehreren oder <source> Elemente. Die <source> Elemente definieren den URI Anzeige und Anzeige. In diesem Beispiel erhält ein URI eine große Antwort. 


    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>
 

###<a name="using-video-player-ad-interface-definition-vpaid"></a>Mithilfe von Video-Player-Anzeige Interface Definition (VPAID)

VPAID ist eine API für ausführbare Ad Einheiten einen video-Player zu aktivieren. Dadurch können interaktive Ad-Erlebnis. Der Benutzer kann die Anzeige interagieren und Ad kann Aktionen vom Viewer auf. Beispielsweise kann eine Anzeige Schaltflächen anzuzeigen, die der Benutzer Weitere Informationen oder eine längere Version der Anzeige angezeigt. Der video-Player muss die VPAID-API und ausführbare Ad müssen die API. Wenn ein Spieler Anfragen eine Anzeige von Ad-Server den Server eine große Antwort reagieren kann, die eine Anzeige VPAID enthält.

Eine ausführbare Anzeige wird Code erstellt, die ausgeführt werden müssen, in einer Common Language Runtime-Umgebung Adobe Flash™ oder JavaScript in einem Webbrowser ausgeführt werden kann. Ein Ad-Server gibt eine große Antwort mit VPAID Anzeige-Attribut der Wert der ApiFramework in der <MediaFile> Element darf "VPAID". Dieses Attribut gibt an, dass die enthaltene Anzeige eine ausführbare VPAID Anzeige. Das Type-Attribut muss auf den MIME-Typ der ausführbaren Datei "Application/X-Shockwave-Flash" oder "Application/X-Javascript" festgelegt. XML-Ausschnitt zeigt die <MediaFile> Element aus einer RIESIGEN Antwort mit einer ausführbaren VPAID Anzeige. 

    
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
 

Ausführbare Ad mit initialisiert die <AdParameters> Element innerhalb der <Linear> oder <NonLinear> Elemente in einer großen Antwort. Weitere Informationen zu den <AdParameters> Element finden Sie [UMFANGREICHE 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). Weitere Informationen über die VPAID-API finden Sie unter [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

##<a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Windows oder Windows Phone 8 Player mit Active Directory-Implementierung

Die Microsoft Media-Plattform: Player Framework für Windows 8 und Windows Phone 8 enthält eine Auflistung Anwendungsbeispiele, die zeigen, wie Sie eine video-Player-Anwendung mit Framework implementieren. Sie können das Player-Framework und den Beispielen [Player Framework für Windows 8 und Windows Phone 8](https://playerframework.codeplex.com)herunterladen.

Beim Öffnen der Projektmappe Microsoft.PlayerFramework.Xaml.Samples sehen Sie eine Reihe von Ordnern innerhalb des Projekts. Werbung-Ordner enthält Beispielcode für einen video-Player mit Ad-Unterstützung erstellen. In der Werbung ist Ordner eine Reihe von XAML-Cs Dateien wie anzeigen anders angezeigt. In der folgenden Liste werden beschrieben:

- AdPodPage.xaml veranschaulicht, wie eine Ad Pod.
- AdSchedulingPage.xaml veranschaulicht die Planung anzeigen.
- FreeWheelPage.xaml veranschaulicht, wie das Plug-in Freilauf anzeigen planen.
- MastPage.xaml zeigt anzeigen mit einer Datei MAST planen.
- ProgrammaticAdPage.xaml veranschaulicht, wie programmgesteuert anzeigen Video planen.
- ScheduleClipPage.xaml zeigt eine Anzeige ohne eine große Datei planen.
- VastLinearCompanionPage.xaml zeigt eine lineare einfügen und Companion-Anzeige.
- VastNonLinearPage.xaml wird gezeigt, wie eine nicht lineare Anzeige eingefügt.
- VmapPage.xaml veranschaulicht anzeigen mit einer VMAP-Datei.

Jedes dieser Beispiele verwendet die MediaPlayer-Klasse vom Player Framework definiert. Die meisten Beispiele verwenden Plugins, die Unterstützung für verschiedene Ad Antwortformate hinzufügen. Die ProgrammaticAdPage Probe interagiert programmgesteuert eine MediaPlayer-Instanz.

###<a name="adpodpage-sample"></a>AdPodPage-Beispiel

Dieses Beispiel verwendet die AdSchedulerPlugin, wenn eine Anzeige definieren. In diesem Beispiel soll eine Mid-Roll-Werbung nach 5 Sekunden wiedergegeben werden. Ad-Pod (eine Gruppe von anzeigen in Reihenfolge) wird in eine große Datei von einem Ad-Server angegeben. Angegebene URI große Datei der <RemoteAdSource> Element.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
    
        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>
    
                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>
    
                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

Weitere Informationen zu den AdSchedulerPlugin finden Sie in der [Werbung in der Player auf Windows 8 und Windows Phone 8](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

###<a name="adschedulingpage"></a>AdSchedulingPage

Dieses Beispiel verwendet auch die AdSchedulerPlugin. Sie weist drei anzeigen, einen Vorlauf Ad Mid-Roll-Anzeige und Nachlauf Ad. Der URI, der die VAST für jede Anzeige gemäß einer <RemoteAdSource> Element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>
    
                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


###<a name="freewheelpage"></a>FreeWheelPage

Dieses Beispiel verwendet FreeWheelPlugin Source-Attribut gibt an, die gibt einen URI an, der auf eine SmartXML-Datei verweist, die Ad-Content sowie Ad Planungsinformationen angibt.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="mastpage"></a>MastPage

Dieses Beispiel verwendet MastSchedulerPlugin, die MAST-Datei ermöglicht. Das Source-Attribut gibt den Speicherort der Datei MAST.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="programmaticadpage"></a>ProgrammaticAdPage

In diesem Beispiel interagiert programmgesteuert mit MediaPlayer. Die Datei ProgrammaticAdPage.xaml instanziiert MediaPlayer:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Die Datei ProgrammaticAdPage.xaml.cs erstellt eine AdHandlerPlugin fügt TimelineMarker an eine Anzeige angezeigt werden soll und fügt einen Ereignishandler für das MarkerReached-Ereignis einer RemoteAdSource angeben eines URI in eine große Datei lädt und spielt die Anzeige.
    
    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;
    
            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }
    
            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

###<a name="scheduleclippage"></a>ScheduleClipPage

In diesem Beispiel mithilfe der AdSchedulerPlugin eine Mid-Roll-Anzeige durch Angabe einer WMV-Datei, die die Anzeige enthält.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearcompanionpage"></a>VastLinearCompanionPage

Dieses Beispiel veranschaulicht die Verwendung der AdSchedulerPlugin eine lineare Mid-Roll-Anzeige eine Companion-Anzeige planen. Die <RemoteAdSource> -Element gibt den Speicherort der große Datei.
    
    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
    
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage

Dieses Beispiel verwendet AdSchedulerPlugin so planen Sie eine lineare und nichtlineare Ad. GROßE Dateispeicherort angegeben ist die <RemoteAdSource> Element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>
    
                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>
                            
                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

###<a name="vmappage"></a>VMAPPage

Diese Beispiele mithilfe der VmapSchedulerPlugin mit einer VMAP-Datei anzeigen. Angegebene URI in die Datei VMAP in das Quellattribut der der <VmapSchedulerPlugin> Element.
    
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

##<a name="implementing-an-ios-video-player-with-ad-support"></a>Implementieren eine iOS Video Player mit Ad-Unterstützung


Die Microsoft Media-Plattform: Player Framework für iOS enthält eine Auflistung Anwendungsbeispiele, die zeigen, wie Sie eine video-Player-Anwendung mit Framework implementieren. Sie können der Player und Beispiele von [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework)herunterladen. Github Seite enthält einen Link zum ein Wiki, das zusätzliche im Player-Framework Informationen und eine Einführung in die Player-Beispiel: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).


###<a name="scheduling-ads-with-vmap"></a>Planung mit VMAP

Im folgenden Codebeispiel wird das Anzeigen einer VMAP Datei planen.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest
    
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

###<a name="scheduling-ads-with-vast"></a>Planung mit VAST

Das folgende Beispiel zeigt eine späte Bindung große Anzeige planen.
    
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }
         
   Das folgende Beispiel zeigt eine frühe Bindung große Anzeige planen.
Beispiel: 4 planen eine frühe Bindung große Ad //Download der VAST-Datei Wenn (! [ framework.adResolver DownloadManifest: & manifest WithURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[selbst LogFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;
        
        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

Das folgende Beispiel zeigt, wie eine Anzeige mit grob Ausschneiden bearbeiten (RCE)

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

Im folgende Beispiel wird eine Ad Pod planen.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;
    
    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Im folgenden Beispiel wird veranschaulicht, wie nicht klebend Mid-Roll-Anzeige planen. Nicht klebend Ad spielt nur einmal unabhängig von jeder Suchvorgänge der Viewer führt.
    
    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    
    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Im folgenden Codebeispiel wird eine Kurznotiz Mid-Roll-Anzeige planen. Eine Kurznotiz wird jedem Anzeige angegebene Punkt auf die Videozeitachse erreicht.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


Das folgende Beispiel zeigt eine Anzeige Nachlauf planen.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Das folgende Beispiel zeigt eine Anzeige Vorlauf planen.
    
    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Das folgende Beispiel zeigt Mid-Roll-Overlay Ad planen.
    
    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

 
##<a name="see-also"></a>Siehe auch

[Video-Player Anwendungsentwicklung](media-services-develop-video-players.md)
