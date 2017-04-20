<properties 
    pageTitle="Streaming von Windows Store-App-Lernprogramm Glätten | Microsoft Azure" 
    description="Erfahren Sie mit Azure Media Services eine C# Windows Store-Anwendung mit einer XML-MediaElement-Steuerelement Wiedergabe glatt Stream Inhalt erstellt." 
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



#<a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Erstellen einer Smooth Streaming-Windows Store-Anwendung

Die reibungslose Streaming Client SDK für Windows 8 können Entwickler Windows Store-Apps erstellen, die auf Anforderung und live Smooth Streaming-Inhalte wiedergeben können. Neben der grundlegenden Wiedergabe Smooth Streaming SDK stellt auch Funktionen wie Microsoft PlayReady-Schutz, Einschränkung, Live DVR Audioqualität stream wechseln, Statusupdates (z. B. Qualität ändert) und Fehlerereignisse überwacht und Inhalt. Informationen der unterstützten Funktionen finden Sie unter [Versionsinformationen](http://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes). Weitere Informationen finden Sie in der [Player-Framework für Windows 8](http://playerframework.codeplex.com/). 

Dieses Lernprogramm enthält vier Lektionen:

1. Erstellen einer grundlegenden glatte Streaming-Store-Anwendung
2. Hinzufügen eines Schiebereglers Media Status steuern
3. Smooth Streaming Streams auswählen
4. Smooth Streaming wählen

##<a name="prerequisites"></a>Erforderliche Komponenten

- Windows 8 32-Bit- oder 64-Bit. Sie erhalten [Windows 8 Enterprise-Evaluierungsversion](http://msdn.microsoft.com/evalcenter/jj554510.aspx) von MSDN.
- Visual Studio 2012 oder Visual Studio Express 2012 (oder höher). Die Testversion erhalten [hier](http://www.microsoft.com/visualstudio/11/downloads).
- [Microsoft Smooth Streaming Client SDK für Windows 8](http://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Homehttp://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home).


Codebeispiele für Entwickler MSDN (Code Gallery) kann bereits fertig gestellte Lösung für jede Lektion heruntergeladen werden: 

- [Lektion 1](http://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - eine einfache Windows 8 Smooth Streaming MediaPlayer 
- [Lektion 2](http://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) : eine einfache Windows 8 Smooth Streaming MediaPlayer mit einem Schieberegler steuern 
- [Lektion 3](http://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - ein Windows 8 Smooth Streaming MediaPlayer mit Streamauswahl  
- [Lektion 4](http://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - ein Windows 8 Smooth Streaming MediaPlayer mit Auswahl.

##<a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Lektion 1: Erstellen einer grundlegenden glatte Streaming-Store-Anwendung

In dieser Lektion erstellen eine Windows Store-Anwendung mit MediaElement-Steuerelement glatte Stream spielen Content Sie.  Die Anwendung sieht folgendermaßen aus:

![Beispiel für Smooth Streaming Windows Store][PlayerApplication]
 
Weitere Informationen zum Entwickeln von Windows Store-Anwendung finden Sie [hervorragende Apps für Windows 8 zu entwickeln](http://msdn.microsoft.com/windows/apps/br229512.aspx). Diese Lektion enthält folgende Verfahren:

1.  Erstellen Sie eine Windows Store-Projekt
2.  Entwerfen der Benutzeroberfläche (XAML)
3.  Die CodeBehind-Datei ändern
4.  Kompilieren und Testen der Anwendungdes

**Erstellen von Windows Store-Projekt**

1.  Visual Studio 2012 oder höher ausführen.
2.  Klicken Sie im Menü **Datei** auf **neu**und klicken Sie auf **Projekt**.
3.  Wählen Sie im Dialogfeld Neues Projekt geben Sie ein oder wählen Sie die folgenden Werte aus:

Name|Wert
---|---
Vorlagengruppe|Installiert/Vorlagen/Visual C# / Windows Store
Vorlage|Leere App (XAML)
Name|SSPlayer
Speicherort|C:\SSTutorials
Projektmappenname|SSPlayer
Projektmappenverzeichnis erstellen|(ausgewählt)

4.  Klicken Sie auf **OK**.

**Eine reibungslose Streaming Client SDK hinzufügen**

1.  Im Projektmappen-Explorer mit der rechten Maustaste **SSPlayer**und dann auf **Verweis hinzufügen**.
2.  Geben Sie ein oder wählen Sie die folgenden Werte aus:

Name|Wert
---|---
Referenzgruppe|Windows-Erweiterungen
Referenz|Wählen Sie Microsoft Smooth Streaming-Client SDK für Windows 8 und Microsoft Visual C++-Laufzeit
    
3.  Klicken Sie auf **OK**. 

Nach dem Hinzufügen der Verweise, werden wählen Sie die Zielplattform (X64 oder X86), Hinzufügen von verweisen nicht für beliebige CPU Plattformkonfiguration.  Im Projektmappen-Explorer sehen Sie gelbe Warnung Markierung für diese Verweise hinzugefügt.

**Player-Benutzeroberfläche entwerfen**

1.  Projektmappen-Explorer klicken Sie **MainPage.xaml** um in der Entwurfsansicht öffnen.
2.  Suchen Sie die ** &lt;Gitter&gt; ** und ** &lt;/Grid&gt; ** tags die XAML-Datei, und fügen Sie folgenden Code zwischen den beiden Tags:

        <Grid.RowDefinitions>
            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
        </Grid.RowDefinitions>
        
        <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="http://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
        </StackPanel>

        <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
        </StackPanel>

        <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
        </StackPanel>

    Wird das MediaElement-Steuerelement zur Medienwiedergabe verwendet. Das Schieberegler-Steuerelement mit dem Namen SliderProgress wird in der nächsten Lektion Media Status steuern verwendet.

3.  Drücken Sie **STRG + S** zum Speichern der Datei.

Das MediaElement-Steuerelement unterstützt Smooth Streaming Content Out-of-Box. Damit den Smooth Streaming-Support muss den Smooth Streaming-Datenstrom Handler Erweiterung und MIME-Typ registriert werden.  Um zu registrieren, verwenden Sie die MediaExtensionManager.RegisterByteStremHandler-Methode der Windows.Media-Namespace.

In diesem XAML-Datei werden einige Ereignishandler Steuerelemente zugeordnet.  Sie müssen diese Ereignishandler definieren.

**So ändern Sie die CodeBehind-Datei**

1.  Im Projektmappen-Explorer mit der rechten Maustaste **MainPage.xaml**und **Klicken**.
2.  Fügen Sie am Anfang der Datei die folgende Anweisung verwenden:

        using Windows.Media;

3.  Fügen Sie am Anfang der **MainPage** -Klasse den folgenden Datenmember:

        private MediaExtensionManager extensions = new MediaExtensionManager();

4.  Fügen Sie am Ende der **MainPage** -Konstruktor die beiden folgenden Zeilen hinzu:

        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
        
5.  Am Ende der **MainPage** -Klasse den folgenden Code:

        #region UI Button Click Events
        private void btnPlay_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Play();
            txtStatus.Text = "MediaElement is playing ...";
        }
        
        private void btnPause_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Pause();
            txtStatus.Text = "MediaElement is paused";
        }
        
        private void btnSetSource_Click(object sender, RoutedEventArgs e)
        {
            sliderProgress.Value = 0;
            mediaElement.Source = new Uri(txtMediaSource.Text);
        
            if (chkAutoPlay.IsChecked == true)
            {
                txtStatus.Text = "MediaElement is playing ...";
            }
            else
            {
                txtStatus.Text = "Click the Play button to play the media source.";
            }
        }
        
        private void btnStop_Click(object sender, RoutedEventArgs e)
        {
            mediaElement.Stop();
            txtStatus.Text = "MediaElement is stopped";
        }
        
        private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            txtStatus.Text = "Seek to position " + sliderProgress.Value;
            mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
        }
        #endregion

    Hier wird der SliderProgress_PointerPressed-Ereignishandler festgelegt.  Gibt weitere arbeiten, um es arbeiten, die in der nächsten Lektion dieses Lernprogramms abgedeckt werden.
6.  Drücken Sie **STRG + S** zum Speichern der Datei.

Der CodeBehind-Datei wird wie folgt aussehen:

![Codeansicht in Visual Studio der reibungslose Streaming Windows Store-Anwendung][CodeViewPic]

**Kompilieren und Testen der Anwendungdes**

1.  Klicken Sie im Menü **Erstellen** auf **Configuration Manager**.
2.  **Aktive Projektmappenplattform** Ihre Entwicklungsplattform entsprechend ändern.
3.  Drücken Sie **F6** , um das Projekt zu kompilieren. 
4.  Drücken Sie **F5** , um die Anwendung auszuführen.
5.  Am oberen Rand der Anwendung verwenden Sie den reibungslosen Streaming-URL oder eine andere eingeben. 
6.  Klicken Sie auf **Quelle**. **Automatische Wiedergabe** standardmäßig aktiviert ist, werden die Medien automatisch wiedergegeben.  Sie können das Medium **Wiedergeben**, **Anhalten** und **Beenden** Schaltflächen steuern.  Sie können den Datenträger mit dem vertikalen Schieberegler steuern.  Jedoch der horizontale Schieberegler zur Steuerung der Medienstatus noch nicht vollständig implementiert ist. 

Lesson1 wurde abgeschlossen.  In dieser Lektion verwenden Sie ein MediaElement-Steuerelement Wiedergabe Smooth Streaming-Inhalte.  In der nächsten Lektion fügen Sie einen Schieberegler, um den Fortschritt der Smooth Streaming-Inhalte zu steuern.


##<a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Lektion 2: Hinzufügen eines Schiebereglers Media Status steuern
In Lektion 1 haben Sie eine Windows Store-Anwendung mit MediaElement XAML Wiedergabe Smooth Streaming Media-Inhalte.  Er enthält einige grundlegende Medienfunktionen starten, beenden und anhalten.  In dieser Lektion fügen Sie der Anwendung ein Schieberegler-Steuerelement hinzufügen.

In diesem Lernprogramm verwenden wir einen Zeitgeber, Schiebereglerposition basierend auf der aktuellen Position des MediaElement-Steuerelements zu aktualisieren.  Der Schieberegler Start- und Zeit auch bei live-Inhalte aktualisiert werden müssen.  Dadurch kann besser in die adaptive Aktualisierung Ereignisquelle behandelt werden.

Medien sind Objekte, die Daten zu generieren.  Quelle Resolver einen URL oder Byte Stream und die entsprechenden Medienquelle für die Inhalte erstellt.  Der Konfliktlöser Quelle ist das Standardverfahren für die Applikationen Medien erstellen. 

Diese Lektion enthält folgende Verfahren:

1.  Registrieren Sie den Handler Smooth Streaming 
2.  Der Ereignishandler adaptive Quelle Manager Ebene hinzufügen
3.  Ebene Ereignishandler adaptive Quelle hinzufügen
4.  MediaElement-Ereignishandler hinzufügen
5.  Schieberegler zugehörigen Code hinzufügen
6.  Kompilieren und Testen der Anwendungdes

**Registrieren Sie den Handler Smooth Streaming-Datenstrom und der propertyset**

1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **MainPage.xaml**und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie am Anfang der Datei die folgende Anweisung verwenden:

        using Microsoft.Media.AdaptiveStreaming;

3.  Fügen Sie zu Beginn der MainPage-Klasse die folgenden Datenmember hinzu:

        private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
        private IAdaptiveSourceManager adaptiveSourceManager;
    
4.  Innerhalb des Konstruktors **MainPage** fügen Sie folgenden Code nach dem **dies. Initialisieren Components();** Linie und Registrierung Codezeilen in der vorherigen Lektion:
    
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
    
5.  Innerhalb des Konstruktors **MainPage** ändern zwei RegisterByteStreamHandler-Methoden zum Hinzufügen der her Parameter:

        // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
        // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
        // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
        extensions.RegisterByteStreamHandler(
            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
        propertySet);

6.  Drücken Sie **STRG + S** zum Speichern der Datei.

**Adaptive Quelle Manager Level-Ereignishandler hinzufügen**

1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **MainPage.xaml**und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie der **MainPage** -Klasse die folgenden Datenmember hinzu:

        private AdaptiveSource adaptiveSource = null;

3.  Fügen Sie am Ende der **MainPage** -Klasse den folgenden Ereignishandler hinzu:
    
        #region Adaptive Source Manager Level Events
        private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
        }
        #endregion Adaptive Source Manager Level Events

4.  Am Ende der **MainPage** -Konstruktor fügen Sie die folgende Zeile adaptive öffnen Ereignisquelle abonnieren:
    
    adaptiveSourceManager.AdaptiveSourceOpenedEvent += neue AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);

5.  Drücken Sie **STRG + S** zum Speichern der Datei.

**Adaptive Quelle auf Ereignishandler hinzufügen**

1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **MainPage.xaml**und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie der **MainPage** -Klasse die folgenden Datenmember hinzu:
    
        private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
        private Manifest manifestObject;
    
3.  Fügen Sie am Ende der **MainPage** -Klasse die folgenden Ereignishandler hinzu:

        #region Adaptive Source Level Events
        private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
        {
            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
        }
        
        private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
        {
            adaptiveSourceStatusUpdate = args;
        }
        
        private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
        {
            txtStatus.Text = "Error: " + args.HttpResponse;
        }
        #endregion Adaptive Source Level Events

4.  Am Ende der Methode **MediaElement AdaptiveSourceOpened** fügen Sie den folgenden Code, die Ereignisse zu abonnieren:
    
        adaptiveSource.ManifestReadyEvent +=
                    mediaElement_ManifestReady;
        adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 
            mediaElement_AdaptiveSourceStatusUpdated;
        adaptiveSource.AdaptiveSourceFailedEvent += 
            mediaElement_AdaptiveSourceFailed;
    
5.  Drücken Sie **STRG + S** zum Speichern der Datei.

Die gleichen Ereignisse sind auf Adaptive Quelle Manager, die für die Behandlung von Funktionen für alle Medienelemente in der Anwendung verwendet werden kann. Jede AdaptiveSource enthält seine eigenen Ereignisse und alle AdaptiveSource Ereignisse unter AdaptiveSourceManager kaskadiert.

**Medienelement Ereignishandler hinzufügen**

1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **MainPage.xaml**und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie am Ende der **MainPage** -Klasse die folgenden Ereignishandler hinzu:
    
        #region Media Element Event Handlers 
        private void MediaOpened(object sender, RoutedEventArgs e)
        {
            txtStatus.Text = "MediaElement opened";
        }
        
        private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
        {
            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
        }
        
        private void MediaEnded(object sender, RoutedEventArgs e)
        {
            txtStatus.Text ="MediaElement ended.";
        }
        #endregion Media Element Event Handlers

3.  Index die Ereignisse am Ende der **MainPage** -Konstruktor fügen Sie folgenden Code hinzu:
    
        mediaElement.MediaOpened += MediaOpened;
        mediaElement.MediaEnded += MediaEnded;
        mediaElement.MediaFailed += MediaFailed;

4.  Drücken Sie **STRG + S** zum Speichern der Datei.

**Regler hinzuzufügenden-Code**

1.  Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf **MainPage.xaml**und klicken Sie dann auf **Code anzeigen**.
2.  Fügen Sie am Anfang der Datei die folgende Anweisung verwenden:
    
        using Windows.UI.Core;

3.  Fügen Sie der **MainPage** -Klasse die folgenden Datenmember hinzu:
    
        public static CoreDispatcher _dispatcher;
        private DispatcherTimer sliderPositionUpdateDispatcher;
    
4.  Fügen Sie den folgenden Code am Ende der **MainPage** -Konstruktor:

        _dispatcher = Window.Current.Dispatcher;
        PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
        sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
    
5.  Fügen Sie am Ende der **MainPage** -Klasse den folgenden Code ein:
    
        #region sliderMediaPlayer
        private double SliderFrequency(TimeSpan timevalue)
        {
            long absvalue = 0;
            double stepfrequency = -1;
        
            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }
        
            TimeSpan totalDVRDuration = new TimeSpan(absvalue);
        
            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }
        
            return stepfrequency;
        }
        
        void updateSliderPositionoNTicks(object sender, object e)
        {
            sliderProgress.Value = mediaElement.Position.TotalSeconds;
        }
        
        public void setupTimer()
        {
            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
        }

        public void startTimer()
        {
            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderStartTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
        }
        
        // Slider start and end time must be updated in case of live content
        public async void setSliderEndTime(long startTime)
        {
            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
        }
        #endregion sliderMediaPlayer

    **Hinweis:** CoreDispatcher wird verwendet, um den UI-Thread nicht UI-Thread zu ändern. Bei Engpass Dispatcher-Thread können Entwickler mit Dispatcher bereitgestellten Benutzeroberflächenelement er beabsichtigt, aktualisieren.  Zum Beispiel:
    
        await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 
          timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
        double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 
          sliderProgress.Maximum = absvalue; }); 
        

6.  Fügen Sie am Ende der **MediaElement_AdaptiveSourceStatusUpdated** -Methode den folgenden Code ein:
    
        setSliderStartTime(args.StartTime);
        setSliderEndTime(args.EndTime);

7.  Am Ende der **MediaOpened** -Methode folgenden Code hinzu:
    
    sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan); sliderProgress.Width = mediaElement.Width; setupTimer();

8.  Drücken Sie **STRG + S** zum Speichern der Datei.

**Kompilieren und Testen der Anwendungdes**

1. Drücken Sie **F6** , um das Projekt zu kompilieren. 
2.  Drücken Sie **F5** , um die Anwendung auszuführen.
3.  Am oberen Rand der Anwendung verwenden Sie den reibungslosen Streaming-URL oder eine andere eingeben. 
4.  Klicken Sie auf **Quelle**. 
5.  Testen Sie den Schieberegler.

Lektion 2 wurde abgeschlossen.  In dieser Lektion haben Sie eine Schiebereglers Anwendung hinzugefügt. 

##<a name="lesson-3-select-smooth-streaming-streams"></a>Lektion 3: Wählen Sie Smooth Streaming-Streams
Smooth Streaming kann zum Streaming von Inhalten mit mehreren Sprachen Audiospuren, die die Zuschauer ausgewählt werden.  In dieser Lektion können Sie Betrachter Streams auswählen. Diese Lektion enthält folgende Verfahren:

1. Ändern Sie die XAML-Datei
2. Die Codedatei völlig ändern
3. Kompilieren und Testen der Anwendungdes


**So ändern Sie die XAML-Datei**

1. Im Projektmappen-Explorer mit der rechten Maustaste **MainPage.xaml**und dann auf **Ansicht-Designer**.
2. Suchen Sie &lt;Grid.RowDefinitions&gt;, und sie sieht die RowDefinitions ändern:

        <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
        </Grid.RowDefinitions>

3. In der &lt;Gitter&gt;&lt;/Grid&gt; Tags, fügen Sie folgenden Code ein Listbox-Steuerelement definieren, damit Benutzer die Liste der verfügbaren Streams und Streams auswählen können:

        <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
        </Grid>

4. Drücken Sie **STRG + S** zum Speichern.


**So ändern Sie die CodeBehind-Datei**

1. Im Projektmappen-Explorer mit der rechten Maustaste **MainPage.xaml**und **Klicken**.
2. Fügen Sie eine neue Klasse im Namespace SSPlayer: #region Klasse Stream
    
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
    
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
    
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
    
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream

3. Fügen Sie am Anfang der MainPage-Klasse Variablen die folgenden Definitionen:

        private List<Stream> availableStreams;
        private List<Stream> availableAudioStreams;
        private List<Stream> availableTextStreams;
        private List<Stream> availableVideoStreams;

4. Fügen Sie der MainPage-Klasse den folgenden Bereich:

        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
        
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
        
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
        
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
        
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
        
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
        
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
        
            // Select the frist video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
        
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
        
            // Select the frist audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
        
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
        
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection

5. Suchen Sie die Methode MediaElement_ManifestReady, fügen Sie den folgenden Code am Ende der Funktion:
    
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();

    MediaElement-Manifest bereit ist, der Code Ruft eine Liste der verfügbaren Streams ab, und füllt die Liste im Listenfeld Benutzeroberfläche.

6. Suchen Sie in der MainPage-Klasse die Benutzeroberfläche Schaltflächen klicken Sie auf Ereignisse, und fügen Sie die folgenden Funktionsdefinition:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Kompilieren und Testen der Anwendungdes**

1. Drücken Sie **F6** , um das Projekt zu kompilieren. 
2.  Drücken Sie **F5** , um die Anwendung auszuführen.
3.  Am oberen Rand der Anwendung verwenden Sie den reibungslosen Streaming-URL oder eine andere eingeben. 
4.  Klicken Sie auf **Quelle**. 
5.  Die Standardsprache ist Audio_eng. Versuchen Sie, zwischen Audio_eng und Audio_es. Jedes Mal, wählen Sie einen neuen Stream, klicken Sie die Schaltfläche Absenden.

Lektion 3 wurde abgeschlossen.  In dieser Lektion fügen Sie Funktionen zum Streams auswählen.

##<a name="lesson-4-select-smooth-streaming-tracks"></a>Lektion 4: Smooth Streaming-Tracks auswählen
Eine Präsentation Smooth Streaming kann mehrere Videodateien codiert mit verschiedenen Qualitätsstufen (Bitraten) und Auflösung enthalten. In dieser Lektion werden Benutzer Tracks auswählen können. Diese Lektion enthält folgende Verfahren:

1. Ändern Sie die XAML-Datei
2. Die Codedatei völlig ändern
3. Kompilieren und Testen der Anwendungdes

**So ändern Sie die XAML-Datei**

1. Im Projektmappen-Explorer mit der rechten Maustaste **MainPage.xaml**und dann auf **Ansicht-Designer**.
2. Suchen Sie die &lt;Gitter&gt; tag mit dem Namen **GridStreamAndBitrateSelection**, hängen Sie den folgenden Code am Ende des Tags:

        <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
        </StackPanel>

3. Drücken Sie **STRG + S** zum Speichern he


**So ändern Sie die CodeBehind-Datei**

1. Im Projektmappen-Explorer mit der rechten Maustaste **MainPage.xaml**und **Klicken**.
2. Fügen Sie eine neue Klasse in der SSPlayer-Namespace:
    
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
    
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
    
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
    
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
    
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track

3. Fügen Sie am Anfang der MainPage-Klasse Variablen die folgenden Definitionen:
    
        private List<Track> availableTracks;

4. Fügen Sie der MainPage-Klasse den folgenden Bereich:
    
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>

        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();

            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;

            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;

                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }

        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }

        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }

        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }

        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection

5. Suchen Sie die Methode MediaElement_ManifestReady, fügen Sie den folgenden Code am Ende der Funktion:

        getTracks(manifestObject);
        refreshAvailableTracksListBoxItemSource();

6. Suchen Sie in der MainPage-Klasse die Benutzeroberfläche Schaltflächen klicken Sie auf Ereignisse, und fügen Sie die folgenden Funktionsdefinition:

        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
        }

**Kompilieren und Testen der Anwendungdes**

1. Drücken Sie **F6** , um das Projekt zu kompilieren. 
2.  Drücken Sie **F5** , um die Anwendung auszuführen.
3.  Am oberen Rand der Anwendung verwenden Sie den reibungslosen Streaming-URL oder eine andere eingeben. 
4.  Klicken Sie auf **Quelle**. 
5.  Standardmäßig werden alle Spuren des Videostreams ausgewählt. Experimentieren Sie das Bit Rate ändert können den niedrigsten Bit wählen und wählen Sie die höchste Bitrate. Sie müssen nach jeder Änderung auf Absenden klicken.  Sie sehen die Videoqualität ändert.

Lektion 4 wurde abgeschlossen.  In dieser Lektion fügen Sie die Funktionen zum Titel auszuwählen.


##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="other-resources"></a>Weitere Ressourcen:
- [Wie eine glatte Streaming Windows 8 JavaScript-Anwendung mit erweiterten Funktionen erstellen](http://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
- [Reibungslose Streaming – technische Übersicht](http://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png
 
