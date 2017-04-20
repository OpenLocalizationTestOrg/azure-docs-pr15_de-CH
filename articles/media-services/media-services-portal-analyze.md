<properties
    pageTitle="Analysieren Sie Ihre Medien mit Azure-Portal | Microsoft Azure"
    description="Dieses Thema beschreibt, wie Ihre Medien mit Media Analytics Medienprozessoren (MPs) über das Azure-Portal."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="analyze-your-media-using-the-azure-portal"></a>Analysieren Sie Ihre Medien mit Azure-portal

> [AZURE.NOTE] Um dieses Lernprogramm benötigen Sie ein Azure-Konto. Weitere Informationen finden Sie unter [Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="overview"></a>Übersicht

Azure Media Services Analytics ist eine Sammlung von Sprach- und Vision Komponenten (Unternehmen, Kompatibilität, Sicherheit und globale), die Organisationen und Unternehmen leiten Sie umsetzbare Einsichten von ihren Videodateien erleichtern. Näheres Azure Media Services Analytics finden Sie [in diesem](media-services-analytics-overview.md) Thema. 

Dieses Thema beschreibt, wie Ihre Medien mit Media Analytics Medienprozessoren (MPs) über das Azure-Portal. Media Analytics MPs erzeugen MP4 oder JSON-Dateien. Wenn Media Prozessor eine MP4-Datei erzeugt, können Sie schrittweise die Datei herunterladen. Media-Prozessor eine JSON produziert, laden Sie die Datei aus dem Azure blobspeicher. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Wählen Sie eine Anlage, die Sie analysieren möchten 
 
1. Wählen Sie in [Azure-Portal](https://portal.azure.com/)Azure Media Services-Konto.
2. Wählen Sie im **Einstellungsfenster** **Anlagen**.  
.
    ![Analysieren von videos](./media/media-services-portal-analyze/media-services-portal-analyze001.png)

2. Wählen Sie die Anlage, die Sie analysieren und klicken Sie auf **Analysieren** möchten.
        
    ![Analysieren von videos](./media/media-services-portal-analyze/media-services-portal-analyze002.png)

3. Wählen Sie im **Prozess Medienobjekts mit Media Analytics** Prozessor. 

    Der Rest dieses Artikels erklärt, warum und wie jeder Prozessor. 
   
4. Drücken Sie **Erstellen** am Anfang eines Auftrags.

## <a name="azure-media-indexer"></a>Azure Media Indexer

**Azure Media Indexer** Media-Prozessor können Sie Mediendateien und Inhalte durchsucht werden, als auch geschlossene Untertitel-Spuren erstellen. Dieser Abschnitt bietet einige Informationen zu den Optionen für diese MP angegeben werden können.

![Analysieren von videos](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Sprache

Die natürliche Sprache in der Multimedia-Datei erkannt werden. Englisch oder Spanisch. 

### <a name="captions"></a>Untertitel

Sie können Caption Format, die aus dem Inhalt generiert werden. Ein Indizierung Auftrag erzeugen Untertiteldateien in folgenden Formaten:  

- **Samisch**
- **TTML**
- **WebVTT**

Beschriftung (CC) Dateien in diesen Formaten verwendet werden können, um Audio-und Videodateien Anhörung Behinderte zugänglich geschlossen.

### <a name="aib-file"></a>AIB-Datei

Wählen Sie diese Option Audio Index BLOB-Datei mit benutzerdefinierten SQL Server IFilter generieren möchten. Weitere Informationen finden Sie in [diesem](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) Blog.

### <a name="keywords"></a>Schlüsselwörter

Wählen Sie diese Option, eine XML-Datei Schlüsselwörter generieren möchten. Diese Datei enthält Schlüsselwörter, die Häufigkeit und Offsetinformationen aus Sprachinhalt extrahiert.

### <a name="job-name"></a>Auftragsname

Ein Anzeigename, der Sie den Auftrag identifizieren kann. [Dieser](media-services-portal-check-job-progress.md) Artikel beschreibt, wie Sie den Fortschritt eines Projekts überwachen können. 

### <a name="output-file"></a>Ausgabedatei

Ein Anzeigename, der Ausgabeinhalt identifizieren kann. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse

Azure Media Hyperlapse ist eine MP, der reibungslose Ablauf Zeit Videos aus Ego- oder Aktion Kamera Inhalt erstellt.  Weitere Informationen finden Sie in [diesem](media-services-hyperlapse-content.md) Thema. Dieser Abschnitt bietet einige Informationen zu den Optionen für diese MP angegeben werden können.

![Analysieren von videos](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Geschwindigkeit 

Geben Sie die Geschwindigkeit, input Video zu beschleunigen. Die Ausgabe ist eine Formatvariante stabilisiert und Zeit abgelaufen Eingabezeile video.

### <a name="job-name"></a>Auftragsname

Ein Anzeigename, der Sie den Auftrag identifizieren kann. [Dieser](media-services-portal-check-job-progress.md) Artikel beschreibt, wie Sie den Fortschritt eines Projekts überwachen können. 

### <a name="output-file"></a>Ausgabedatei

Ein Anzeigename, der Ausgabeinhalt identifizieren kann. 

## <a name="azure-media-face-detector"></a>Azure Media Gesichtserkennung

**Azure Media Gesicht Detektor** Media Prozessor (MP) können Sie zu zählen, Bewegung, Publikum und Reaktion über Gesichtsausdrücke selbst beurteilen. Dieser Service umfasst zwei Funktionen: 

- **Face-Erkennung**

    Face-Erkennung findet und verfolgt Gesichter in einem Video. Mehrere Flächen erkannt werden und wie sie mit der JSON-Datei zurückgegebenen Metadaten und Position bewegen, anschließend nachverfolgt werden. Während der Überwachung versucht es geben eine konsistente ID zur gleichen Fläche während Person Bildschirm bewegen wird auch wenn sie blockiert werden oder lassen Sie kurz den Frame.

    >[AZURE.NOTE]Dieser Dienst führt keine gesichtserkennung. Eine Person, die bewirkt, dass den Rahmen oder behindert wird, für lange erhalten eine neue ID Rückkehr.

- **Emotionen Erkennung**
    
    Emotionen Erkennung ist eine optionale Komponente nach Erkennung Media Prozessor, die Analyse mehrere emotionale Attribute aus den erkannt, einschließlich Glück, Trauer, Angst, Wut und zurückgibt. 

![Analysieren von videos](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Erkennungsmodus

Einer der folgenden Modi kann vom Prozessor verwendet werden:

- Face-Erkennung
- pro Fläche Emotion Erkennung
- Aggregate Emotion-Erkennung

### <a name="job-name"></a>Auftragsname

Ein Anzeigename, der Sie den Auftrag identifizieren kann. [Dieser](media-services-portal-check-job-progress.md) Artikel beschreibt, wie Sie den Fortschritt eines Projekts überwachen können. 

### <a name="output-file"></a>Ausgabedatei

Ein Anzeigename, der Ausgabeinhalt identifizieren kann. 

## <a name="azure-media-motion-detector"></a>Azure Media Bewegungsmelder

**Azure Media Bewegungsmelder** Media Prozessor (MP) können Sie Abschnitte der Zinsen in einem ansonsten lange und problemlosen Video effizient identifizieren. Bewegungsmelder kann auf Kamera verwendet werden, um Abschnitte des Videos zu identifizieren, an dem Bewegung stattfindet. Es generiert eine JSON-Datei enthält Metadaten mit Zeitstempel und den umgebenden Bereich, in dem das Ereignis aufgetreten ist.

Sicherheit-video-Feeds ausgerichtet, kann diese Technologie Bewegung in entsprechende Ereignisse und fälschlich wie Schatten und Beleuchtung kategorisieren. Dadurch können Sie für Sicherheitshinweise Kamera Feeds endlose irrelevante Ereignisse und Momente von Interesse extrahieren sehr langen überwachungsvideos aufführen.

![Analysieren von videos](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video-Miniaturen

Dieser Prozessor kann helfen, Übersichten für lange Videos erstellen automatisch das Quellvideo interessante Ausschnitte auswählen. Dies ist nützlich, wenn Sie einen schnellen Überblick auf ein langes Video bereitstellen möchten. Ausführliche Informationen und Beispiele finden Sie unter [Verwenden Azure Video Miniaturansichten Video Zusammenfassung erstellen](media-services-video-summarization.md)

![Analysieren von videos](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Auftragsname

Ein Anzeigename, der Sie den Auftrag identifizieren kann. [Dieser](media-services-portal-check-job-progress.md) Artikel beschreibt, wie Sie den Fortschritt eines Projekts überwachen können. 

### <a name="output-file"></a>Ausgabedatei

Ein Anzeigename, der Ausgabeinhalt identifizieren kann. 


##<a name="next-steps"></a>Nächste Schritte

Lernpfade Ansicht Media Services.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


