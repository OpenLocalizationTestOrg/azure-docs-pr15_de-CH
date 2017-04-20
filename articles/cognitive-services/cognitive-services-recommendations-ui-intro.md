<properties
    pageTitle="Erstellen eines Modells mit UI Recommnendations | Microsoft Azure"
    description="Azure Machine Learning Recommendations - Erstellen eines Modells mit der empfohlenen Benutzeroberfläche"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="luisca"/>

# <a name="building-a-model-with-the-recommendations-ui"></a>Erstellen eines Modells mit der empfohlenen Benutzeroberfläche

Dieses Dokument ist eine schrittweise Anleitung. Ziel ist die erforderlichen Schritte zum Trainieren eines Modells mit der [Empfohlenen UI](https://recommendations-portal.azurewebsites.net/)durchlaufen.
Mit können Sie verstehen, ein Modell vor programmgesteuert erstellt. Diese Implementierungsinformationen auch der Benutzeroberfläche die eignet sich der Dienst starten.

In dieser Übung dauert etwa 30 Minuten.

<a name="Step1"></a>
## <a name="step-1---sign-in-to-the-recommendations-ui"></a>Schritt 1 - Zeichen in der Empfehlung Benutzeroberfläche ##

1. Wenn dies noch nicht getan haben, müssen Sie [Anmeldung](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) für ein neues Abonnement [-API Recommendations](https://www.microsoft.com/cognitive-services/en-us/recommendations-api) . Sie können für den Dienst auf **Azure** auf [http://portal.azure.com/](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/Recommendations/pricingtier/S1) und Zeichen mit Ihrem Azure-Konto anmelden. Detaillierte Informationen für die Anmeldung erhalten in *Aufgabe 1* [Erste Schritte](cognitive-services-recommendations-quick-start.md) 

1. Nachdem Sie einen **Schlüssel** für Ihr Abonnement empfohlene API erhalten haben, gehen Sie [Empfohlene Benutzeroberfläche](https://recommendations-portal.azurewebsites.net/). 

1. Das Portal eingeben den Schlüssel im Feld **Konto** melden Sie an und klicken Sie auf **die Anmeldeschaltfläche** .

    ![Empfohlene Benutzeroberfläche: Das Anmeldedialogfeld.][reco_signin]


<a name="Step2"></a>
## <a name="step-2---lets-gather-some-training-data"></a>Schritt 2 - erfassen wir einige Daten ##

Vor dem Erstellen eines Builds die Engine benötigt zwei Informationen: eine Katalogdatei und Verwendung Dateien. 

Die Katalogdatei enthält Informationen über die Elemente, die Sie Ihrem Kunden anbieten. Verwendung-Datei enthält Informationen wie diese Elemente verwendet werden oder die Transaktionen aus Ihrem Unternehmen.

Normalerweise würden Sie die Informationsspeicher-Datenbank für diese Informationen Abfragen. Heute haben wir einige Beispieldaten für Sie bereitgestellt, damit Sie lernen, wie der empfohlene API.

Sie können die Daten von [http://aka.ms/RecoSampleData](http://aka.ms/RecoSampleData). Kopieren Sie und Entpacken Sie die Datei **Books.Zip** in einen Ordner auf Ihrem lokalen Computer. Beispielsweise **c:\data**.

Detaillierte Informationen zum Schema der Katalog und Verwendung finden im Artikel [sammeln Daten in Ihrem Modell](cognitive-services-recommendations-collecting-data.md) .
 
In dieser Übung werden wir eine kleine Datei verwenden, sodass Sie sehr lange warten, Schulung haben. Ggf. eine realistischere Datei haben wir auch **MsStoreData.zip** mit platziert Beispiel Transaktionen im Microsoft Store am [selben Speicherort](http://aka.ms/RecoSampleData).

<a name="Step3"></a>
## <a name="step-3---create-a-project-and-upload-catalog-and-usage-data"></a>Schritt 3 - Erstellen eines Projekts und Katalog und Nutzung Daten ##

Nach der Anmeldung auf die [Empfohlene Benutzeroberfläche](https://recommendations-portal.azurewebsites.net/)auf der Seite Projekte angezeigt. Wenn zuvor keine Projekte erstellt haben, sollten sie hier sehen.
Ein Projekt (auch *ein Modell* in der API Reference) ist ein Container für Ihren Katalog und Verwendung. Sie können mehrere *builds* im Projekt erstellen. Wir gehen Sie schrittweise in den nächsten Schritten.

1. Zum Erstellen eines neuen Projekts geben Sie den Namen im Textfeld (etwa funktioniert "MyFirstModel") und klicken Sie auf **Projekt hinzufügen**.
 
    ![Empfohlene Benutzeroberfläche: Seite.][reco_projects]

1. Nachdem das Projekt erstellt wird, klicken Sie auf Bereich **Hinzufügen einer Katalogdatei** auf **Datei** . Hochladen des Katalogs in Schritt 2 haben. Gespeichert am *c:\data*müssen Sie zu diesem Ordner navigieren.

    ![Empfohlene Benutzeroberfläche: Hinzufügen von Daten zu einem Projekt.][reco_firstmodel]

1. Nachdem der Katalog geladen wird, klicken Sie auf Abschnitt **Verwendete Dateien hinzufügen** auf **Datei** . Fügen Sie die Datei usage_large.txt.

> **Wenn eine große Datei Daten habe?**
>
> Nur verwendete Dateien kleiner als 200 MB hochgeladen werden kann. Andererseits fasst System bis zu 2 GB Daten, sodass Sie ggf. mehrere Dateien hochladen können.
> Nicht unbedingt so viel Daten ein gutes Modell generieren, ist nicht nur die Größe der Daten, die wichtig, aber die Qualität der Daten. Es ist üblich, Daten anzeigen, wobei die Transaktionen sind nur wenige beliebte Artikel und es "wenig signal" für andere Elemente.

<a name="Step4"></a>
## <a name="step-4---lets-do-some-training"></a>Schritt 4 - fangen wir trainieren. ##

Jetzt Hochladen des Katalogs und die Nutzungsdaten können wir das Modul trainieren, dass Muster aus unseren Daten erfahren.

1.  Klicken Sie auf die Schaltfläche **Neuen Build** .

1.  Wählen Sie den Buildtyp **Recommendations** . Beachten Sie, dass wir ein Ranking erstellen und eine FBT (häufig zusammen gekauft) Buildtypen sowie.

    ![Empfohlene Benutzeroberfläche: Build Dialogfeld.][reco_build_dialog.png]


    Erstellen einer FBT können Sie Produkte identifizieren normalerweise gekauft/verbraucht in derselben Transaktion.
    Ranking Build zur Identifizierung Merkmale. 
    Gehen wir nicht sehr tief FBT oder Positionierung in diesem Workshop erstellt, aber wenn Sie möchten prüfen, [Typen und Modell Dokumentationsseite erstellen](cognitive-services-recommendations-buildtypes.md).

1. Klicken Sie auf die **Generator** -Schaltfläche. Der Buildprozess nimmt etwa fünf Minuten Bücher Daten verwendet werden. Es dauert bei größeren Datasets.

<a name="Step5"></a>
## <a name="step-5---lets-find-out-what-the-machine-learned"></a>Schritt 5 - entdecken, was der Computer gelernt! ##

Nachdem der Build abgeschlossen ist, sehen Sie einen neuen Build in der Liste der Builds. Dieser Build kann für Artikel und Benutzer Recommendations abgefragt werden.

1. Nachdem der Build abgeschlossen ist, klicken Sie auf **Bewertung**. Dadurch können Sie mit dem Modell und empfohlene Elemente angezeigt.

    ![Empfohlene Benutzeroberfläche: Bewertung Schaltfläche][reco_score_button]

1. Wählen Sie ein Element anzeigen, welche Elemente als Vorschläge für dieses Element zurückgegeben werden. Beachten Sie, dass stehen nicht genügend Transaktionen eine Empfehlung für einen bestimmten Artikel vorherzusagen, das System keine Vorschläge für dieses Element zurück.  Wenn aus irgendeinem Grund ein Element, die keine Empfehlung gibt verfügen, versuchen Sie bewerten andere Elemente.

<a name="Step6"></a>
## <a name="step-6---next-steps"></a>Schritt 6: nächste Schritte ##
Herzlichen Glückwunsch! Sie trainiert ein Modell und dann Vorschläge aus dem Modell.  Empfohlene Benutzeroberfläche ist ein nützliches Tool, das können Sie den Status Ihrer Projekte und builds. 

Jetzt haben Sie ein Modell sollten Sie alle Schritte programmgesteuert erlernen. Um lernen die API programmgesteuert aufrufen, laden wir Sie zu [Empfohlene API Reference](http://go.microsoft.com/fwlink/?LinkId=759348) [Recommendations Sample Application](http://go.microsoft.com/fwlink/?LinkID=759344)herunterladen.


[reco_signin]:../media/cognitive-services/reco_signin.PNG
[reco_projects]:../media/cognitive-services/reco_projects.PNG
[reco_firstmodel]:../media/cognitive-services/reco_firstmodel.png
[reco_build_dialog.png]:../media/cognitive-services/reco_build_dialog.png
[reco_score_button]:../media/cognitive-services/reco_score_button.png
