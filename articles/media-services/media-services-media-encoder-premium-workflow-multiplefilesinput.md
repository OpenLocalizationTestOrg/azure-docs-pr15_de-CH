<properties
    pageTitle="Premium-Encoder mit mehreren Dateien und Eigenschaften | Microsoft Azure"
    description="Dieses Thema erläutert die SetRuntimeProperties mehrere Eingabedateien verwenden und benutzerdefinierte Daten Prozessor Media Media Encoder Premium-Workflow verwenden."
    services="media-services"
    documentationCenter=""
    authors="xpouyat"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"  
    ms.author="xpouyat;anilmur;juliako"/>

# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Die Verwendung mehrerer Dateien Eigenschaften mit Premium-Encoder

## <a name="overview"></a>Übersicht

Es gibt Szenarios, in denen Sie möglicherweise anpassen, Eigenschaften, Clip Liste XML-Inhalt oder mehrere Eingabedateien senden, wenn Sie eine Aufgabe mit Prozessor Media **Media Encoder Premium-Workflow** senden. Beispiele sind:

- Video und Text Wert (z. B. das aktuelle Datum) zur Laufzeit für jedes Eingabe Video Text überlagern.
- Anpassen der Clip Liste XML (eine oder mehrere Quelldateien mit oder ohne Zuschneiden usw. angeben).
- Überlagern ein Logobild video Eingabe während des Videos codiert ist.

Lassen Sie **Media Encoder Premium-Workflow** , wissen, dass Sie beim Erstellen der Aufgabe oder mehrere Eingabedateien senden einige Eigenschaften im Workflow ändern, müssen Sie eine Verbindungszeichenfolge verwenden enthält, **SetRuntimeProperties** oder **TranscodeSource**. Dieses Thema erläutert die Verwendung.


## <a name="configuration-string-syntax"></a>Syntax für Konfiguration

Die Konfigurationszeichenfolge in der Codierung Aufgabe festgelegt verwendet ein XML-Dokument, die folgendermaßen aussieht:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Die folgenden ist C#-Code, der die XML-Konfiguration aus einer Datei gelesen und übergibt sie an die Aufgabe in einem Projekt:

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## <a name="customizing-component-properties"></a>Anpassen der Eigenschaften  

### <a name="property-with-a-simple-value"></a>Mit einem einfachen Wert
In einigen Fällen ist eine Komponenteneigenschaft zusammen mit der Workflowdatei angepasst, die durch Media Encoder Premium-Workflow ausgeführt werden.

Angenommen Sie, Sie konstruiert einem Workflow Text Überlagern von Videos und Text (z. B. das aktuelle Datum) zur Laufzeit festgelegt werden soll. Hierzu können Sie SMS als neuen Wert für die Text-Eigenschaft der Komponente Overlay Codierung Aufgabe festgelegt werden. Dieser Mechanismus können Sie andere Eigenschaften einer Komponente im Workflow (wie Position oder Farbe der Überlagerung, die Bitrate der AVC-Encoder usw..) ändern.

**SetRuntimeProperties** wird verwendet, um eine Eigenschaft in der Workflow-Komponenten zu überschreiben.

Beispiel:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### <a name="property-with-an-xml-value"></a>Mit XML-Wert

Zum Festlegen einer Eigenschaft, die einen XML-Wert erwartet einkapseln `<![CDATA[ and ]]>`.

Beispiel:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Vergewissern Sie sich nicht in einem zurück nach `<![CDATA[`.


### <a name="propertypath-value"></a>PropertyPath-Wert

In den vorherigen Beispielen wurde die PropertyPath "/ Mediendateien Input-Dateiname" oder "/ InactiveTimeout" oder "ClipListXml".
Dies ist im Allgemeinen der Name der Komponente und der Name der Eigenschaft. Der Pfad haben mehr oder weniger Ebenen, wie "/ PrimarySourceFile" (da die Eigenschaft im Stammverzeichnis des Workflows ist) oder "/ Video Verarbeitung-Grafik überlagern/Deckkraft" (da die Überlagerung in einer Gruppe ist).    

Überprüfen Sie Pfad-und Eigenschaft Schaltfläche Maßnahmen, die unmittelbar neben jeder Eigenschaft. Sie können diese Aktion klicken und **Bearbeiten**auswählen. Dadurch wird der tatsächliche Name der Eigenschaft und unmittelbar darüber Namespace angezeigt.

![Aktion-bearbeiten](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Eigenschaft](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Mehrere Dateien

Jede Aufgabe, die dem **Media Encoder Premium-Workflow** senden erfordert zwei Anlagen:

- Die erste ist eine *Workflow-Anlage* , die eine Workflowdatei enthält. Sie können Workflow-Dateien mit dem [Workflow-Designer](media-services-workflow-designer.md)entwerfen.
- Der zweite ist ein *Medien* mit Media-Dateien, die Sie verschlüsseln möchten.

Wenn Sie mehrere Mediendateien Encoder **Media Encoder Premium-Workflow** senden, gelten die folgenden Nebenbedingungen:

- Alle Mediendateien muss diese *Medien*. Verwenden mehrerer Medien wird nicht unterstützt.
- Legen Sie die primäre Datei in dieses Medienobjekts (im Idealfall sieht die wichtigsten Videodatei, die der Encoder aufgefordert zu).
- Es ist notwendig, Daten mit dem **SetRuntimeProperties** oder **TranscodeSource** Element an den Prozessor übergeben.
  - **SetRuntimeProperties** wird die Filename-Eigenschaft oder eine andere Eigenschaft in den Workflow überschrieben.
  - **TranscodeSource** wird verwendet, um den Clip Liste XML-Inhalt angeben.

Anschlüsse im Workflow:

 - Verwenden Sie eine oder mehrere Komponenten Media Dateieingabe und Plan mit **SetRuntimeProperties** den Dateinamen angeben, dann schließen Sie die primäre Datei Komponente Pin werden. Stellen Sie sicher, dass keine zwischen der Primärdatei Media Datei Aussteuerungsanzeige Verbindung.
 - Möchten Sie XML-Clip-Liste und eine Medienquelle Komponente verwenden und beide miteinander verbinden.

![Keine primäre Quelldatei Dateieingabe Medien](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Keine der primären Datei Dateieingabe Media-Komponenten ist SetRuntimeProperties verwenden, um die Filename-Eigenschaft festgelegt.*

![Verbindung Clip aus XML-Quelle Clip](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Sie können Clip Liste XML Medienquelle verbinden und TranscodeSource.*


### <a name="clip-list-xml-customization"></a>Clip-Liste XML-Anpassung
Mit **TranscodeSource** in der Konfigurationszeichenfolge XML können Sie XML Liste Clip im Workflow zur Laufzeit angeben. Dies erfordert die Clip-Liste XML-Pin Medienquelle Komponente im Workflow verbunden werden.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

/PrimarySourceFile zur Verwendung dieser Eigenschaft Ausgabedateien benennen, indem Sie 'Ausdrücke' angeben möchten, sollten dann übergeben Clip Liste XML als eine Eigenschaft *nach der* Eigenschaft /primarySourceFile zu vermeiden, dass der Clipliste /primarySourceFile Einstellung überschrieben.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Mit zusätzlichen Rahmen genau:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## <a name="example"></a>Beispiel

Ein Beispiel, in dem Sie ein Logobild auf der video während das Video kodiert überlagern möchten In diesem Beispiel input Video heißt "MyInputVideo.mp4" und das Logo heißt "MyLogo.png". Führen Sie die folgenden Schritte aus:

- Erstellen einer Workflow-Anlage mit der Workflow-Datei (siehe folgende Beispiel).
- Erstellen einer Medienobjekts enthält zwei Dateien: MyInputVideo.mp4 als die primäre Datei und MyLogo.png.
- Senden Sie eine Aufgabe Media Encoder Premium-Workflow Media Prozessor mit dem oben eingegebenen, und geben Sie die folgende Verbindungszeichenfolge.

Konfiguration:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


Im obigen Beispiel wird der Name der Videodatei Dateieingabe Media-Komponente und die Eigenschaft PrimarySourceFile gesendet. Der Namen der Logodatei wird auf einem anderen Medium Dateieingabe gesendet, die die Grafik überlagern Komponente verbunden ist.

>[AZURE.NOTE]Video Dateiname wird der Eigenschaft PrimarySourceFile gesendet. Dafür ist die Verwendung dieser Eigenschaft im Workflow für das Erstellen der richtigen Ausgabedateiname Ausdrücke, z. B. mit.


### <a name="step-by-step-workflow-creation-that-overlays-a-logo-on-top-of-the-video"></a>Schrittweise Workflows erstellen, überlagert ein Logo auf Video     

Hier werden die Schritte zum Erstellen eines Workflows, die beiden Dateien als Eingabe akzeptiert: ein Video und ein Bild. Es wird das Bild auf das Video überlagern.

**Workflow-Designer** öffnen, und wählen Sie **Datei** > **Neuen Arbeitsbereich** > **Transcodieren Blaupause**.

Der neue Workflow zeigt drei Elemente:

- Primäre Datei
- Clipliste XML
- Ausgabe von Datei-Anlage  

![Neue Codierung Workflow](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Neue Codierung Workflow*


Um die Eingabe Mediendatei zu akzeptieren, beginnen Sie mit Media Datei Input-Komponente hinzufügen. Der Workflow eine Komponente hinzu, in das Suchfeld Repository suchen Sie und der Designer-Bereich ziehen Sie den gewünschten Eintrag.

Fügen Sie die Datei für das Entwerfen des Workflows verwendet werden. Dazu klicken Sie im Workflow-Designer auf den Hintergrund und die Primärdatei-Eigenschaft im rechten-Eigenschaft auf Suchen. Klicken Sie auf das Ordnersymbol und wählen Sie die entsprechende Datei.

![Primäre Datei](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Primäre Datei*


Geben Sie anschließend die Videodatei in der Komponente Media Dateieingabe.   

![Eingabequelle der Mediendatei](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Eingabequelle der Mediendatei*


Sobald dies geschehen ist, die Komponente Media Dateieingabe überprüfen Sie die Datei und füllen die Ausgabepins entsprechend überprüft die Datei.

Im nächste Schritt ist das Hinzufügen einer "Video Daten Typ Updater" um den Farbraum zu Rec.709 anzugeben. Hinzufügen einer "Video Format Converter", die auf Daten Layout/Layout festgelegt = konfigurierbare Ebenen. Dies wird den video-Stream in ein Format konvertieren, die als Quelle für die Überlagerung Komponente durchgeführt werden können.

![Video Data Type Updater und Dateiformat-Konverter](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Videodaten Typ Updater und Dateiformat-Konverter*

![Layouttyp = konfigurierbare Ebenen](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Layout ist konfigurierbare Planar*

Nächstes fügen Sie Video Overlay Komponente hinzu und (nicht komprimierte) video Pin Dateieingabe Media (nicht komprimierte) video Pin an.

Fügen Sie ein anderes Medium Dateieingabe (um die Logodatei laden), klicken Sie auf diese Komponente umbenennen "Media Datei Eingabe Logo", und wählen Sie ein Bild (beispielsweise eine PNG-Datei) in der. Nicht komprimierte Bild Pin PIN nicht komprimierte Bild der Überlagerung anschließen

![Overlay-Komponente und Bild Dateiquelle](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Overlay-Komponente und Bild Dateiquelle*


Wenn Sie die Position des Logos für das Video ändern möchten (beispielsweise möchten 10 Prozent von oben links das Video positionieren), deaktivieren Sie das Kontrollkästchen "Manuelle Eingabe". Dies ist möglich, da Sie eine Media Dateieingabe Logodatei Overlay-Komponente zu verwenden.

![Overlay-position](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Overlay-position*


Die Designeroberfläche zum Codieren des Videostream zu h. 264 fügen Sie AVC-Video Encoder und AAC Encoder Komponenten hinzu. Verbinden Sie die Stifte.
Einrichten der AAC-Encoder und Audio-Format/Konvertierungsvorgabe: 2.0 (L, R).

![Audio und Video Encoder](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Audio und Video Encoder*


Jetzt die **ISO Mpeg-4-Multiplexer** und **DateiAusgabe** Komponenten und verbinden Sie Pins wie.

![MP4-multiplexer und Ausgabe](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4-multiplexer und Ausgabe*


Sie müssen den Namen der Ausgabedatei fest. Klicken Sie auf die Komponente **Datei** und bearbeiten Sie den Ausdruck für die Datei:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Dateiname für Ausgabe](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Dateiname für Ausgabe*

Führen Sie Workflow lokal, um sicherzustellen, dass er ordnungsgemäß ausgeführt wird.

Anschließend können Sie sie in Azure Media Services ausführen.

Zuerst bereiten Vermögenswert in Azure Media Services mit zwei Dateien: die Videodatei und das Logo. Dies kann mithilfe von .NET oder REST-API. Sie können auch dazu Azure-Portal oder [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE).

Dieses Lernprogramm veranschaulicht die mit AMSE verwalten. Gibt es zwei Arten so eine Anlage hinzu:

- Erstellen Sie einen lokalen Ordner, Dateien Sie die zwei, und ziehen Sie den Ordner auf der Registerkarte **Anlage** .
- Die Videodatei als Anlage, Anlage anzeigen wechseln Sie zur Registerkarte Dateien und eine zusätzliche Datei (Logo) hochladen.

>[AZURE.NOTE]Legen Sie eine primäre Datei in der Anlage (die wichtigsten Videodatei) fest.

![Objektdateien in AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Objektdateien in AMSE*


Wählen Sie die Anlage und mit Premium-Encoder codieren. Hochladen des Workflows, und wählen Sie sie aus.

Klicken Sie auf die Schaltfläche, um Daten an den Prozessor übergeben, und fügen Sie folgenden XML-Code zum Festlegen der Common Language Runtime-Eigenschaften:

![Premium-Encoder in AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium-Encoder in AMSE*


Fügen Sie die folgenden XML-Daten an. Sie müssen den Namen der Videodatei für die Dateieingabe Medien und PrimarySourceFile angeben. Geben Sie den Namen der Datei für das Logo zu.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Verwenden Sie das .NET SDK zum Erstellen und ausführen, müssen diese XML-Daten wie der Verbindungszeichenfolge übergeben werden.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

Nach Abschluss des Auftrags zeigt die MP4-Datei in die Ausgabe Anlage die Überlagerung!

![Überlagerung für das video](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Überlagerung für das video*


Sie können den Beispielworkflow von [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/)herunterladen.


## <a name="see-also"></a>Siehe auch

- [Einführung in Premium-Codierung in Azure Media Services](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [Verwendung von Premium-Codierung in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

- [Codierung abrufbare Inhalte mit Azure Media Services](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Media Encoder Premium-Workflow Formate und codecs](media-services-premium-workflow-encoder-formats.md)

- [Workflow-Beispieldateien](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Azure Media Services-Explorertools](http://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
