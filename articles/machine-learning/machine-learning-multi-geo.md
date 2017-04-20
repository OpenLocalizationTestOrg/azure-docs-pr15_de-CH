<properties
   pageTitle="Multi-Geo-Hilfe | Microsoft Azure"
   description="Erfahren Sie, wie Sie einen Webdienst in einer Azure-Region unterschiedliche aus südlichen zentralen USA (SCUS) veröffentlichen und einen Arbeitsbereich erstellen Azure-Region."
   services="machine-learning"
   documentationCenter=""
   authors="tedway"
   manager="jhubbard"
   editor="rmca14"
   tags=""/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="tedway; neerajkh"/>

# <a name="multi-geo-help-documentation"></a>Multi-Geo-Hilfe

Dieser Artikel beschreibt, wie Sie einen Arbeitsbereich erstellen und einen Webdienst in anderen Regionen Azure veröffentlichen.

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs

1. Klassischen Azure-Portal anmelden.

2.  Klicken Sie auf **+ neu** > **DATA SERVICES** > **MACHINE LEARNING** > **schnell erstellen**.  Wählen Sie unter **Position** eine andere Region wie **Südostasien**.
![Multi-Geo Hilfe Bild 1][1]
3. Wählen Sie den Arbeitsbereich und klicken Sie auf **Anmelden ML Studio**.
![Multi-Geo Hilfe Bild 2][2]

4. Sie haben nun einen Arbeitsbereich in einer anderen Region wie alle anderen Arbeitsbereich können. Zum Wechseln zwischen Ihren Arbeitsbereichen suchen Sie oben rechts auf dem Bildschirm. Klicken Sie auf die Dropdownliste, markieren Sie den Bereich und wählen Sie den Arbeitsbereich. Alles ist lokalen Arbeitsbereich Region; Beispielsweise werden alle Ihre Webdienste Arbeitsbereich erstellt im selben Bereich, denen im Arbeitsbereich befindet.
![Multi-Geo Hilfe Bild 3][3]

## <a name="open-an-experiment-from-gallery"></a>Versuch aus Galerie öffnen

Versuch aus Galerie öffnen, können Sie auch die Region auswählen, das Experiment kopieren möchten.

![Abbildung 4 Multi-Geo-Hilfe][4a]

## <a name="web-service-management"></a>Web-Service-management

Um programmgesteuert Webdienste wie Umschulung, verwalten verwenden Sie die regionsspezifische Adresse:
- https://asiasoutheast.Management.azureml.NET
- https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Hinweise

1.  Sie können nur zwischen Arbeitsbereiche, die auf den Bereich so gehören. Wenn Sie experimentieren kopieren Arbeitsbereiche in unterschiedlichen Regionen hinweg können Sie die [PowerShell](http://aka.ms/amlps) -Cmdlet [*Kopie AmlExperiment*](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) durchführen. Eine weitere Möglichkeit ist an Experiment in Galerie Modus öffnen im Arbeitsbereich aus der Region.
2.  Die Regionsauswahl wird nur Arbeitsbereiche für eine einzelne Region anzeigen. In Zukunft werden Sie eine vollständige Liste der Arbeitsbereiche sehen Sie auf alle Regionen gleichzeitig zugreifen.  
3.  Eine kostenlose Arbeitsbereich oder Gastzugriff (anonyme) Arbeitsbereich wird erstellt und im südlichen zentralen USA gehostet In Zukunft werden frei/Gastzugriff Arbeitsbereiche in der Region zu erstellen, die Sie auswählen können.  
4.  Webdienste Arbeitsbereich in Südostasien bereitgestellt werden auch in Südostasien gehostet werden. In Zukunft werden können die Flexibilität der Experimente in einer Region erstellen und Bereitstellen Webdienstendpunkte in verschiedenen Regionen generiert.  

## <a name="more-information"></a>Weitere Informationen

Frage im [Forum Azure maschinelles lernen](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/machine-learning-multi-geo/multi-geo_1.png
[2]: ./media/machine-learning-multi-geo/multi-geo_2.png
[3]: ./media/machine-learning-multi-geo/multi-geo_3.png
[4a]: ./media/machine-learning-multi-geo/multi-geo_4a.png
