<properties
    pageTitle="Schnellstart-Handbuch: Computer lernen empfohlene API | Microsoft Azure"
    description="Azure maschinelles lernen Recommendations - Quick Start-Handbuch"
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
    ms.date="08/22/2016"
    ms.author="luisca"/>

# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Schnellstart-Handbuch für kognitive Services empfohlene API

Dieses Dokument beschreibt, wie zu integrierten der Dienst oder die Anwendung der [Empfohlenen API](http://go.microsoft.com/fwlink/?LinkId=759710).
Weitere Informationen über die empfohlene API und andere kognitive Dienste finden [hier](http://go.microsoft.com/fwlink/?LinkId=759709). In diesem Handbuch finden Sie auch die [Empfehlung-API-Referenz](http://go.microsoft.com/fwlink/?LinkId=759348) nützlich.


<a name="Overview"></a>
## <a name="general-overview"></a>Übersicht

Dieses Dokument ist eine schrittweise Anleitung. Unser Ziel ist, gehen Sie durch die Schritte zum Trainieren eines Modells und auf Ressourcen, mit denen Sie das Modell Ihrer produktiven Umgebung verwendet wird.

In dieser Übung dauert ca. 30 Minuten.

Um [Empfohlene API](http://go.microsoft.com/fwlink/?LinkId=759710)zu verwenden, müssen Sie die folgenden Schritte ausführen:

1. Erstellen eines Modells - Modell ist ein Container für die Daten und Katalogdaten empfehlungsmodell.
1. Katalogdaten importieren - Katalog enthält Metadateninformationen über die.
1. 2 Arten (oder beides) können Verwendungsdaten Import - Daten hochgeladen werden:
  -  Hochladen einer Datei, die Daten enthält.
  -  Anschaffung Ereignisse senden.
  Normalerweise laden Sie eine Verwendung um eine anfängliche empfehlungsmodell (bootstrap) und bis System Übernahme Datenformat mit genügend Daten erfasst.
1. Empfehlung Modell – Dies ist ein asynchroner Vorgang empfehlungssystem übernimmt alle Daten und erstellt eine Empfehlung. Dieser Vorgang dauert einige Minuten oder mehrere Stunden dauern, je nach Größe der Daten und die Build-Konfigurationsparameter. Beim Auslösen des Builds erhalten Sie Build-ID. Verwenden Sie diese überprüfen, wenn der Buildprozess beendet wurde, vor Recommendations verwenden.
1. Verwenden Sie Empfehlung - Get Vorschläge für ein bestimmtes Element oder eine Liste von Elementen.

<a name="GetStarted"></a>
### <a name="lets-get-started"></a>Fangen wir an!

Sie beginnen eine empfohlene Modell. Dann werden wir Sie zur Verwendung durch das Modell macht Vorschläge auf einer e-Commerce-Site generierten Ergebnisse führen.

<a name="Ex1Task1"></a>
#### <a name="task-1---signing-up-for-the-recommendations-api"></a>Aufgabe 1 - Signatur für die empfohlene API ####

In dieser Aufgabe werden empfohlene API Service abonnieren und empfohlene Modell erstellen.

1. Zum **Azure-Portal** [http://portal.azure.com/](http://portal.azure.com/) und mit der Azure-Konto anmelden.

1.  Klicken Sie auf **+ Neuer**.

1. Aktivieren Sie die Option **Intelligenz** .

1. Wählen Sie das Produkt **Kognitive Services-APIs** .
Dieses Produkt ermöglicht Ihnen ein Abonnement für kognitive Dienste APIs (Gesicht Textanalyse, Computer Vision usw.). Heute konzentrieren wir uns auf die empfohlene API.

1. Geben Sie den **Kontonamen** auf Zielseite kognitive Services-API für Ihr Abonnement Recommendations. (Für die Instanz: "MyRecommendations"). Diese Namen müssen keine Leerzeichen.

1. **API-Typ**wählen Sie **Recommendations**.

1. **Preisstufe**können Sie einen Plan auswählen. Sie können Ebenen **frei** für 10.000 Transaktionen pro Monat auswählen. Einen freien Plan ist ist eine gute Möglichkeit, versucht das System gestartet Einmal zur Produktion, empfehlen wir prüfen Ihre Anfrage Volume und entsprechend dem Plan ändern. (Hinweis: können Sie jeweils nur ein kostenlos Abonnement)

1. Wählen Sie eine **Ressourcengruppe**oder erstellen Sie eine neue zu, wenn Sie nicht bereits eine haben.

1. Sie können andere Elemente im Dialogfeld erstellen ändern. Es sollte klargestellt werden, der Ressourcenanbieter heute nur USA Rechenzentren unterstützt.

1. Wenn Sie Auswahl getroffen, klicken Sie auf **Erstellen**.

1. Warten Sie einige Minuten für die Ressource bereitgestellt werden.
Nach der Bereitstellung wird Abschnitt **Schlüssel** Blade- **Einstellungen** zu gelangen, in dem Sie einen primären und sekundären Schlüssel die API bereitgestellt.  Kopieren Sie den Primärschlüssel Bedarf werden beim ersten Modell erstellen.

<a name="Ex1Task2"></a>
#### <a name="task-2---did-you-bring-your-data"></a>Aufgabe 2: brachte Sie Ihre Daten? ####

Die empfohlene API Lernen aus Ihren Katalog und Ihre Bereitstellung geeignete Vorschläge. Also müssen Sie Feeds mit gültigen Daten über Ihre Produkte (Wir bezeichnen dies als **eine Katalogdatei** ) und Transaktionen für interessante Muster des Verbrauchs finden (Wir bezeichnen diese **Verwendung**).

1. Normalerweise würden Sie die Transaktionsdatenbank für diese Informationen Abfragen.
Falls Sie diese praktische haben, haben wir einige Beispieldaten basierend auf Microsoft Store Daten bereitgestellt.

 Sie können die Daten [hier](http://aka.ms/RecoSampleData)herunterladen. Kopieren Sie und Entpacken Sie die MsStoreData.Zip in einen Ordner auf Ihrem lokalen Computer.

 > **Hinweis:** Beispielcode, den Sie herunterladen und Ausführen in Aufgabe 3 hat Daten bereits in-eingebettet, damit diese Aufgabe optional ist.  Andererseits dieser Aufgabe können Sie zu realistischeren Datensätze können Sie Eingaben in die empfohlene API besser verstehen.

1.  Nun werfen wir einen Blick auf die Katalogdatei. Navigieren Sie zum Speicherort, in dem die Daten kopiert.
 Die Katalogdatei im **Editor**zu öffnen.

 Sie werden feststellen, dass die Katalogdatei einfach. Hat das folgende format`<itemid>,<item name>,<product category>`

 >  AAA-04294 OfficeLangPack 2013 32/64 E34 Online DwnLd, Office <br>
 > AAA-04303 OfficeLangPack 2013 32/64 Online DwnLd ET, Office  <br>
 > C9F 00168, KRUSELL Kiruna kippen Abdeckung für Nokia Lumia 635 - Camel, Zubehör

 Wir sollten darauf hinweisen, dass eine Katalogdatei reicher beispielsweise können Metadaten (Wir bezeichnen diesen *Artikel Features*) Produkte. Abschnitt [Katalogformat](http://go.microsoft.com/fwlink/?LinkID=760716) -API-Referenz für Weitere Informationen sollte auf das Format der angezeigt werden.

1. Wir dasselbe mit Daten. Sie werden feststellen, dass die Verwendung des Formats ist `<User Id>,<Item Id>,<Time Stamp>,<Event>`.

  > 00037FFEA61FCA16, 288186200, 2015/08/04T11:02:52, Einkauf 0003BFFDD4C2148C 297833400, 2015/08/04T11:02:50, Einkauf 0003BFFDD4C2118D 297833300, 2015/08/04T11:02:40, Einkauf 00030000D16C4237 297833300, 2015/08/04T11:02:37, Einkauf 0003BFFDD4C20B63 297833400, 2015/08/04T11:02:12, Einkauf 00037FFEC8567FB8 297833400, 2015/08/04T11:02:04, Bestellung

Beachten Sie, dass die ersten drei Elemente erforderlich sind. Der Ereignistyp ist optional. [Verwendung Format](http://go.microsoft.com/fwlink/?LinkID=760712) Weitere Informationen zu diesem Thema finden.

 > **Wie viele Daten benötigen Sie?**
 <p>
Nun, hängt es wirklich Daten selbst. Das System lernt, wenn Benutzer verschiedene Artikel kaufen. Für einige Builds wie FBT ist es wichtig zu wissen, welche Elemente im gleichen Transaktionen erworben werden. (Wir bezeichnen diese *gemeinsam Vorkommen*). Eine gute Faustregel soll mindestens 20 Transaktionen werden die meisten Elemente hätten Sie 10.000 Elemente im Katalog, wir empfehlen mindestens 20 Mal, Anzahl oder 200.000 Transaktionen haben.
Wieder ist dies eine Faustregel. Sie müssen Ihre Daten experimentieren.
</p>

<a name="Ex1Task3"></a>
####Aufgabe 3: Erstellen eines Modells recommendations####

Sie haben ein Konto und Daten erstellen Ihre erste Modell.

In dieser Aufgabe verwenden Sie Beispiel-Anwendung das erste Modell erstellen.

1. Zunächst sollten Sie [Empfohlene API Reference](http://go.microsoft.com/fwlink/?LinkId=759348)berücksichtigen.

1. [Beispiel-Anwendung](http://go.microsoft.com/fwlink/?LinkID=759344) in einem lokalen Ordner herunterladen.

1. Öffnen Sie in Visual Studio **C#** im Ordner **RecommendationsSample.sln** -Lösung.

1. Öffnen Sie die Datei **SampleApp.cs** . Beachten Sie, dass in der Datei die folgenden Schritte durch:
 + Modell erstellen
 + Hinzufügen einer Katalogdatei
 + Eine Verwendung hinzufügen
 + Auslösen eines Builds für das Modell
 + Abrufen Sie eine Empfehlung für ein Paar von Elementen
<p></p>

1. Ersetzen Sie den Wert für das **AccountKey** -Feld mit dem Schlüssel aus Aufgabe 1.

1. Die Lösung durchgehen und sehen, wie ein Modell erstellt wird.

1. Tauschen Sie die Katalog und Nutzung heruntergeladenen Dateien nur um ein neues Modell für Microsoft Store oder Empfehlungen erstellen. Sie müssen ändern sowie der Modellname und die Elemente für die empfohlenen anfordern.

1. Beim Erstellen des Modells Beachten Sie die **Modell-ID** Bedarf wird beim Anfordern von Recommendations in Ihrem Unternehmen.

>  Erfahren Sie mehr über Typen und die Qualität der Builds erstellen [hier](cognitive-services-recommendations-buildtypes.md).

<a name="Ex1Task4"></a>
### <a name="putting-your-model-in-production"></a>Setzen Ihr Modell in der Produktion! ###

Da Sie wissen, wie ein Modell erstellen und Vorschläge verwenden, besteht der nächste Schritt es auf Ihrer Website mobile Anwendung oder Ihr CRM oder ERP-System integrieren.
Natürlich würde jeder dieser Implementierung unterscheiden. Da die empfohlene API als einen Webdienst angefordert werden, sollten Sie problemlos aus anderen Umgebungen aufrufen können.

**Wichtig:**  Wenn Sie Vorschläge von öffentlichen Client (zum Beispiel Ihre e-Commerce-Website) anzeigen möchten, sollten Sie einen Proxyserver Ratschläge zu erstellen. Dies ist wichtig, damit der API-Schlüssel nicht auf externe Entitäten (möglicherweise nicht vertrauenswürdig) verfügbar gemacht wird.

Hier sind einige Ideen für empfohlene Verwendungsmöglichkeiten Speicherorte:

### <a name="product-page"></a>Produktseite

**Empfohlene Artikel**
<p>Wenn das Modell auf Daten trainiert wurde, können Ihre Kunden *Produkte sind wahrscheinlich an Personen, die das Quellelement erworben haben*.</p>
<p>Wenn das Modell in geschult wurde, klicken Sie auf Daten ermöglicht Ihren Kunden, *Produkte, die von Benutzern besucht, die das Quellelement besucht haben, dürften*. Dieser Typ von Modell kann ähnliche Elemente zurückgeben.</p>

**Zusammen Recommendations häufig gekauft**
<p>Ein häufig gekauft zusammen Build geschult werden konnte, so können Sie Elemente wahrscheinlich mit diesen Artikel erworben werden.</p>

### <a name="check-out-page"></a>Seite

**Empfohlene Artikel**
<p>Empfehlung, das Modell als wäre Eingabe eine Liste von Elementen. So konnte Sie alle Artikel im Warenkorb als Eingabe Vorschläge zu übergeben.
In diesem Fall bietet das Modell empfohlen, die angesichts der Artikel im Warenkorb.
</p>

### <a name="landing-page"></a>Zielseite

**Benutzer-Empfehlung**
<p>
Vorschläge können Modell als Eingabe die Benutzer-Id.  Dadurch wird die Transaktionshistorie Benutzer verwenden, um personalisierte Empfehlung angegebenen Benutzer bereitzustellen.
</p>

Checken Sie [Artikel Recommendations Dokumentation erhalten](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>
### <a name="whats-next"></a>Was kommt als nächstes?
Herzlichen Glückwunsch bei es dies viel! Um erfahren, dass Sie die vollständige [API Reference Recommendations](http://go.microsoft.com/fwlink/?LinkId=759348) Fragen finden, kontaktieren Sie unsmlapi@microsoft.com
