<properties
    pageTitle="Das video Zuschneiden | Microsoft Azure"
    description="Dieser Artikel veranschaulicht das Media Encoder Standard Videos zuschneiden."
    services="media-services"
    documentationCenter=""
    authors="anilmur"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"  
    ms.author="anilmur;juliako;"/>

# <a name="crop-videos-with-media-encoder-standard"></a>Zuschneiden von Videos mit Media Encoder Standard

Media Encoder Standard (MES) können Sie Ihre Eingabe video zuschneiden. Zuschneiden ist der Prozess der Auswahl eines rechteckigen Fensters im video-Frame und Codierung nur die Pixel in diesem Fenster. Das folgende Diagramm veranschaulicht den Prozess.

![Schneiden Sie eine video](./media/media-services-crop-video/media-services-crop-video01.png)

Angenommen Sie, Sie als Eingabe eine Bildqualität, die eine Auflösung von 1920 x 1080 Pixel (Seitenverhältnis 16:9), sondern schwarze Balken (Säule Felder) links und rechts, damit nur ein 4:3 oder 1440 x 1080 Pixel enthält aktive Videosignale. Sie können MES mit dem zuzuschneiden oder zu bearbeiten, die schwarzen Balken und 1440 x 1080 Region codiert.

MWS Zuschneiden ist eine Phase vor der Verarbeitung der ursprünglichen Eingabe Video Zuschneiden Parameter in die Codierung Voreinstellung zuweisen. Codierung ist einem Breite-Höhe-Einstellungen gelten für die *vorverarbeitet* video und nicht mit dem ursprünglichen Video Wenn die Voreinstellung entwerfen, müssen Sie Folgendes: (a) Wählen Sie basierten auf der ursprünglichen Eingabe Videos Zuschneiden-Parametern und (b) die Einstellung basiert auf zugeschnittenes Bild codieren. Wenn Sie nicht übereinstimmen die Einstellungen zugeschnittenes Bild codieren, die Ausgabe nicht wie erwartet.

Im [folgenden](media-services-advanced-encoding-with-mes.md#encoding_with_dotnet) Thema wird mit eine Codierung erstellen und benutzerdefinierte Vorgaben für die Codierung Aufgabe angeben. 

## <a name="creating-a-custom-preset"></a>Erstellen eine benutzerdefinierte Vorgabe

Im Beispiel in der Abbildung dargestellt:

1. Ursprüngliche Eingabe ist 1920 x 1080
1. Ausgabe von 1440 x 1080 abgeschnitten, die im Fenster Eingabe zentriert werden muss
1. Dies bedeutet einen X-Offset (1920 – 1440) / 2 = 240 und eine Y-offset 0
1. Die Breite und Höhe des Rechtecks Zuschneiden sind 1440 1080, bzw.
1. In der Phase codieren die Fragen ist drei Schichten, Auflösung 1440 x 1080 960 x 720 und 480 x 360 bzw.

###<a name="json-preset"></a>JSON-Voreinstellung


    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


##<a name="restrictions-on-cropping"></a>Beschränkungen Zuschneiden

Das Zuschneiden Feature soll manuell. Sie müssten input Video in geeigneten Bearbeitungstool laden, mit der Sie wählen an, positionieren Sie den Cursor ermitteln Offsets für das Rechteck Zuschneiden Codierung Voreinstellung bestimmen, die für diese bestimmte Videos usw. optimiert. Dieses Feature soll nicht etwas aktivieren: automatische Erkennung und Entfernung von schwarzen Letterbox-Pillarbox Rahmen in Ihrem video Eingabe.

Folgende Integritätsregeln gelten für die Zuschneiden-Funktion. Wenn diese nicht erfüllt sind, kann codieren Task fehl oder unerwartete Ergebnisse.

1. Koordiniert und Größe des Rechtecks Zuschneiden input Video angepasst werden müssen
1. Wie bereits erwähnt, müssen Breite und Höhe in den Einstellungen codieren zugeschnittenes Bild
1. Zuschneiden gilt für Videos im Querformat erfasst (d. h. nicht für Videos von einem Smartphone statt vertikal oder im Hochformat)
1. Funktioniert am besten mit progressive-Video mit quadratischen Pixeln erfasst

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Nächstes
 
Anzeigen Sie Azure Media Services Lernpfade zu über hervorragende Funktionen AMS  

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
