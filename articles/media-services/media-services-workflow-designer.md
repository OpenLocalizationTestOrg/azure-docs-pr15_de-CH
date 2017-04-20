<properties 
    pageTitle="Erweiterte Codierung Workflows mit Workflow-Designer erstellen | Microsoft Azure" 
    description="Enthält Informationen zum erweiterten Codierung Workflows mit Workflow-Designer erstellen." 
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
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;johndeu;anilmur"/>


#<a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Erstellen Sie erweiterte Codierung Workflows mit Workflow-Designer

##<a name="overview"></a>Übersicht

**Workflow Designer** ist ein Windows-Programm, mit dem Entwerfen und Erstellen von benutzerdefinierten Workflows mit **Media Encoder Premium-Workflow**Codierung.
Mithilfe der Power Workflow Designer Tools können Sie entwerfen und komplexe Workflows erstellen, die in **Media Encoder**ausgeführt werden.  

Workflows können Kunden Entscheidungsfindungslogik und Verzweigung auf Grundlage der Eingabequelldatei Eigenschaften. Sie können Workflows erstellen, überschreibbare Eigenschaften und dynamische Werte selbst die komplexesten Codierungsaufgaben leicht wiederholen und in der Cloud anpassen.

Beispielworkflows, die Sie erstellen können gehören:

- Überprüfen der Quellinhalt Auflösung und Codieren der gewünschten Ausgabe verfolgt nur Workflows basiert Entscheidung.  Dies ist Quellinhalt durch Eliminierung von ungenutzten Spuren Vergrößerung Quelle Inhalt versehentlich generiert werden würde.
- Titel, überlagert und stitching zusammen Inhalte unterstützen können mehrere Eingabedateien verwendet werden. 

Dieses Tool kann auch verwendet werden, unsere [Workflows veröffentlicht](media-services-workflow-designer.md#existing_workflows)ändern. 

>[AZURE.NOTE]Um eine Kopie des Workflow Designer Tools zu erhalten, wenden Sie sich an mepd@microsoft.com.


Erstellte Workflow-Datei als Anlage hochgeladen werden und für die Codierung von Mediendateien verwendet werden. Informationen zum Codieren mit **Media Encoder Premium-Workflow** mit **.NET**finden Sie unter [Erweiterte Verschlüsselung mit Media Encoder Premium-Workflow](media-services-encode-with-premium-workflow.md).

##<a id="existing_workflows"></a>Ändern von vorhandenen workflows

Standardmäßig [veröffentlicht Workflows](media-services-workflow-designer.md#existing_workflows) können mit dem Designer Tool geändert werden. Workflow-Dateien standardmäßig erhalten [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Der Ordner enthält auch die Beschreibung dieser Dateien.

In den folgenden Videos demonstrieren den Designer verwenden.

###<a name="day-1--getting-started"></a>Tag 1 – erste Schritte

Tag1 Video behandelt:

- Übersicht über Designer
- Basisworkflows – "Hello World"
- Erstellen von mehreren Ausgabedateien MP4 mit streaming Azure Media Services

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###<a name="day-2"></a>Tag2

Tag2 Video behandelt:

- Unterschiedliche Quellszenarien Datei – Behandeln von audio
- Workflows mit erweiterten
- Diagramm-Phasen

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###<a name="day-3"></a>Tag3

Tag3-Video behandelt:

- Scripting in Workflows-Blaupausen
- Mit der aktuellen Encoder
- Fragen & Antworten
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]


## <a name="next-step"></a>Nächstes

Überprüfen Sie Media Services Lernpfade.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


Sie benötigen Unterstützung oder Fragen zum Erstellen benutzerdefinierter Workflows in Workflow Designer Tools, senden e-Mail an mepd@microsoft.com.

##<a name="see-also"></a>Siehe auch

[Azure Premium Encoder Workflow Designer Schulungsvideos](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)
