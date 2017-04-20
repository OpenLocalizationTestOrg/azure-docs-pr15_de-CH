<properties
    pageTitle="Buildtypen und Qualität: empfohlene API | Microsoft Azure"
    description="Azure Machine Learning Recommendations – quick Start-Handbuch"
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
    ms.date="09/20/2016"
    ms.author="luisca"/>

#  <a name="build-types-and-model-quality"></a>Buildtypen und Qualität #

<a name="TypeofBuilds"></a>
## <a name="supported-build-types"></a>Unterstützte Buildtypen ##

Die empfohlene API unterstützt derzeit zwei Buildtypen: *Empfehlung* und *FBT*. Jede basiert auf verschiedene Algorithmen und jeder hat unterschiedliche Vorteile. Dieses Dokument beschreibt die einzelnen Builds und Techniken zum Vergleich der Qualität der Modelle generiert.

Wenn Sie nicht bereits getan haben, empfiehlt sich die [Schnellstartübersicht](cognitive-services-recommendations-quick-start.md)abzuschließen.

<a name="RecommendationBuild"></a>
### <a name="recommendation-build-type"></a>Empfehlung Buildtyp ###

Empfehlung Buildtyp verwendet Matrix faktorisierung für Vorschläge. [Latente Funktion](https://en.wikipedia.org/wiki/Latent_variable) Vektoren basierend auf Transaktionen beschreiben Sie jedes Element generiert und verwendet dann diese losen Vektoren Elemente vergleichen, die ähnlich sind.

Trainieren Sie das Modell basierend auf Verkäufen in Ihrem Geschäft und bieten eine Fluss 650 Telefon als Eingabe für das Modell wird das Modell eine Gruppe von Elementen zurück, die von Personen erworben werden, die voraussichtlich Fluss 650 Telefon kaufen in der Regel. Die Elemente möglicherweise nicht ergänzen. In diesem Beispiel kann das Modell denn wer Fluss 650 anderen Telefonen kann anderen Telefonen zurück.

Der Empfehlung Build hat zwei Funktionen, die es attraktiv:

*Kalte Element *unterstützt der Empfehlung Build ** Platzierung **

Elemente, die keinen signifikanten Auslastung werden kalt Elemente bezeichnet. Wenn Sie eine Lieferung von einem Telefon nie erhalten vor verkauft, ableiten nicht System beispielsweise Vorschläge für dieses Produkt allein. Dies bedeutet, dass das System Informationen über das Produkt selbst lernen sollen.

Wenn Sie kalt Element platzieren möchten, müssen Sie Funktionen Informationen für jeden Ihrer Artikel im Katalog. Es folgt die ersten Zeilen des Katalogs (Hinweis Schlüssel = Wert-Format für die Features) aussehen können.

>Geben Sie Pro2 6CX 00001, Fläche, Oberfläche, Hardware, Speicher = = 128 GB Speicher = 4G Hersteller = Microsoft

>73H-00013 Wake Xbox 360-Spiele, Typ = Software Language = Englisch Bewertung = Reifen

>WAH 0F05 Minecraft Xbox 360, Spiele, * Typ = Software Language = Spanisch Bewertung = Jugend

Sie müssen die folgenden Buildparameter festgelegt:

| Parameter erstellen         | Notizen
|------------------     |-----------
|*useFeaturesInModel*     | Auf **true**festgelegt.  Gibt an, ob Funktionen verwendet werden können, zu der empfehlungsmodell.
|*allowColdItemPlacement*   | Auf **true**festgelegt. Gibt an, ob die Empfehlung auch kalte Elemente über vergleichbare Funktion push sollten.
| *modelingFeatureList*   | Durch Kommas getrennte Liste der Komponentennamen im Build Empfehlung zu der Empfehlung verwendet werden. Beispielsweise "Sprache, Speicher" für das vorherige Beispiel.

**Der Empfehlung Build unterstützt Benutzer**

Empfehlung Build unterstützt [Benutzer](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd). Dies bedeutet, dass personalisierte Vorschläge für Benutzer anhand deren Transaktionsverlauf bereitstellen kann. Für Benutzer empfohlen können die Benutzer-ID oder der Geschichte der Transaktionen für den Benutzer bereitstellen.

Ein klassisches Beispiel möglicherweise Benutzer Vorschläge gelten soll bei der Anmeldung auf der Willkommensseite. Es können Sie Inhalt heraufstufen, die für den Benutzer gilt.

Sie möchten auch einen Buildtyp Recommendations anwenden, wenn der Benutzer ausgecheckt. An diesem Punkt haben die Liste der Elemente, die Benutzer kaufen und aktuelle Warenkorb Grundlage bieten.

#### <a name="recommendations-build-parameters"></a>Empfohlene erstellen Parameter

| Name  |   Beschreibung |    Typ <br>  Gültige Werte <br> (Standardwert)
|-------|-------------------|------------------
| *NumberOfModelIterations* |   Die Anzahl der Iterationen Modell führt spiegelt insgesamt berechnen und die Genauigkeit des Modells. Je höher die Zahl, die das Modell jedoch die Rechenzeit länger.  |   Ganze Zahl <br>  10 bis 50 <br>(40)
| *NumberOfModelDimensions* |   Die Anzahl der Dimensionen bezieht sich auf die Anzahl der Versuche des Modells Daten finden. Erhöhen die Anzahl der Dimensionen ermöglicht bessere Ergebnisse in kleinere Cluster optimieren. Jedoch verhindert zu viele Dimensionen Modell Korrelationen zwischen Elementen suchen. |  Ganze Zahl <br> 10 bis 40 <br>(20) |
| *ItemCutOffLowerBound* |  Definiert die Mindestanzahl der Verwendung Punkte, die ein Elements, die Teil des Modells berücksichtigt werden soll. |        Ganze Zahl <br> 2 oder mehr <br> (2) |
| *ItemCutOffUpperBound* |  Definiert die maximale Anzahl Punkte Verwendung, die ein Elements, die Teil des Modells berücksichtigt werden soll. |  Ganze Zahl <br>2 oder mehr<br> (2147483647) |
|*UserCutOffLowerBound* |   Definiert die Mindestanzahl von Transaktionen, die Benutzer durchgeführten Teil des Modells berücksichtigt werden muss. | Ganze Zahl <br> 2 oder mehr <br> (2)
| *UserCutOffUpperBound* |  Definiert die maximale Anzahl von Transaktionen, die Benutzer durchgeführten Teil des Modells berücksichtigt werden muss. | Ganze Zahl <br>2 oder mehr <br> (2147483647)|
| *UseFeaturesInModel* |    Gibt an, ob Funktionen verwendet werden können, zu der empfehlungsmodell. |     Boolescher Wert<br> Standard: True
|*ModelingFeatureList* |    Durch Kommas getrennte Liste der Komponentennamen im Build Empfehlung zu der Empfehlung verwendet werden. Wichtige Features hängt. |    Zeichenfolge bis zu 512 Zeichen
| *AllowColdItemPlacement* |    Gibt an, ob die Empfehlung auch kalte Elemente über vergleichbare Funktion push sollten. | Boolescher Wert <br> Standardwert: False
| *EnableFeatureCorrelation*    | Gibt an, ob Funktionen Begründung verwendet werden können. | Boolescher Wert <br> Standardwert: False
| *ReasoningFeatureList* |  Durch Kommas getrennte Liste der Komponentennamen für Schlussfolgerungen Sätze wie Empfehlung erläutern. Es hängt die Funktionen, die für Kunden wichtig sind. | Zeichenfolge bis zu 512 Zeichen
| *EnableU2I* | Aktivieren Sie personalisierte Vorschläge auch Benutzer (U2I) empfohlenen Artikel bezeichnet. | Boolescher Wert <br>Standard: True
|*EnableModelingInsights* | Definiert, ob offline-Auswertung für die Modellierung Erkenntnisse (Präzision und Vielfalt-Metriken) ausgeführt werden soll. Wenn auf True festgelegt, eine Teilmenge der Daten nicht verwendet werden für Schulung, da sie zum Testen des Modells reserviert werden muss. Weitere Informationen über [offline Produkttests](#OfflineEvaluation). | Boolescher Wert <br> Standardwert: False
| *SplitterStrategy* | Enable modeling Einblicke auf *true*festgelegt ist, ist wie Daten für Testzwecke geteilt werden soll.  | Zeichenfolge, *RandomSplitter* oder *LastEventSplitter* <br>Standard: RandomSplitter


<a name="FBTBuild"></a>
### <a name="fbt-build-type"></a>FBT-Buildtyp ###

Häufig gekaufte zusammen (FBT) Build enthält eine Analyse, die oft zählt, zwei oder drei verschiedene Produkte miteinander zusammen auftreten. Dann sortiert legt eine vergleichbare Funktion (**Co Vorkommen**, **Jaccard**, **heben**).

Stellen Sie sich **Jaccard** , und **heben Sie** zu normalisieren gemeinsam vorkommen.  Dies bedeutet, dass Elemente zurückgegeben, wenn sie bei mit Seed-Element.

In unserem Beispiel Fluss 650 Telefon wird Telefon X werden nur zurückgegeben, wenn in der gleichen Session wie der Fluss 650 Telefon Telefon X erworben wurde. Da dies unwahrscheinlich sein, erwarten wir Elemente ergänzen Fluss 650 zurückgegeben werden; beispielsweise eine Displayschutzfolie oder ein Netzteil Fluss 650

Derzeit werden zwei Elemente in der gleichen Sitzung erworben werden, treten in einer Transaktion mit den Benutzer-ID und den Zeitstempel angenommen.

FBT Builds unterstützen cold Elemente da definitionsgemäß zwei Elemente in derselben Transaktion erworben werden erwartet. Während FBT Builds Elemente (Dreiergruppen) zurückgeben können, unterstützen sie, da sie eine einzelne Element als Eingabe akzeptieren nicht personalisierte Recommendations.


#### <a name="fbt-build-parameters"></a>FBT Buildparameter

| Name  |   Beschreibung |       Typ  <br> Gültige Werte <br> (Standardwert)
|-------|---------------|-----------------------
| *FbtSupportThreshold* | Wie das Modell ist. Anzahl Co Elemente für die Modellierung berücksichtigt werden. |  Ganze Zahl <br> 3-50 <br> (6)
| *FbtMaxItemSetSize* | Die Anzahl der Elemente in einer häufig umschließt.| Ganze Zahl  <br> 2-3 <br> (2)
| *FbtMinimalScore* | Minimale Bewertung, die eine häufige Gruppe in die zurückgegebenen Ergebnisse enthalten sein soll. Je höher, desto besser. | Double <br> 0 und höher <br> (0)
| *FbtSimilarityFunction* | Definiert die Funktion Ähnlichkeit von Builds verwendet werden. **Heben Sie** bevorzugt Serendipity **Co Vorkommen** bevorzugt Vorhersagbarkeit und **Jaccard** ist ein Kompromiss. | Zeichenfolge  <br>  <i>schneemengen Aufzug, jaccard</i><br> Standardwert: <i>jaccard</i>

<a name="SelectBuild"></a>
## <a name="build-evaluation-and-selection"></a>Bewertung und Auswahl erstellen ##

Dieser Leitfaden kann Ihnen helfen festzustellen, ob verwenden Sie Vorschläge erstellen oder einen Build FBT bietet keine Antwort in Fällen, in denen Sie Sie verwenden können. Auch, wenn Sie wissen, dass Sie einen Buildtyp FBT verwenden möchten, empfiehlt trotzdem **Jaccard** oder vergleichbare Funktion **heben** .

Die beste Möglichkeit, zwischen zwei verschiedenen Builds werden in der realen Welt (Bewertung online) testen und Nachverfolgen der Umrechnungskurs für die verschiedenen Builds. Der Umrechnungskurs konnte auf Empfehlung Klicks gemessen, die tatsächliche Anzahl Einkäufe von empfohlenen dargestellt oder auch auf die tatsächlichen Beträge unterschiedliche Vorschläge gezeigt wurden. Sie können die Konvertierung Rate Metrik basierend auf Ihrem Ziel auswählen.

In einigen Fällen möchten Sie die offline Auswertung vor dem Speichern in der Produktion. Offline Bewertung kein Ersatz für Bewertung online ist, dient es als Metrik.

<a name="OfflineEvaluation"></a>
## <a name="offline-evaluation"></a>Offline-Auswertung  ##

Offline Bewertung soll Vorhersagen Genauigkeit (die Anzahl der Benutzer, die eine empfohlene Produkte kaufen) und der Vielfalt der Vorschläge (die Anzahl der Elemente, die empfohlen werden).
Als Teil der Bewertung Metriken Präzision und Vielfalt der findet ein Beispiel für Benutzer und Transaktionen für diese Benutzer in zwei Gruppen aufgeteilt: das Trainings-Dataset und das Test-Dataset.

> [AZURE.NOTE]Um offline Metriken zu verwenden, müssen Sie Zeitstempel in Ihre Daten.
> Daten müssen Verwendung ordnungsgemäß für Schulung und Datasets aufteilen.

> Außerdem kann offline Bewertung nicht für kleine verwendete Dateien Ergebnisse. Für die Bewertung gründlich, sollte mindestens 1.000 Verwendung Punkte in das Test-Dataset.

<a name="Precision"></a>
### <a name="precision-at-k"></a>Genauigkeit bei k ###
Die folgende Tabelle stellt die Ausgabe der Genauigkeit bei k offline Auswertung.

| K | 1 | 2 | 3 |   4 |     5
|---|---|---|---|---|---|
|Prozentsatz |   13,75 | 18.04   | 21 |  24.31 | 26.61
|Benutzer im test |    10.000 |    10.000 |    10.000 |    10.000 |    10.000
|Als Benutzer | 10.000 |    10.000 |    10.000 |    10.000 |    10.000
|Benutzer nicht berücksichtigt | 0 | 0 | 0 | 0 | 0

#### <a name="k"></a>K
In der obigen Tabelle darstellt *k* die Anzahl der Vorschläge dem Kunden. Die Tabelle lautet wie folgt: "Wenn während des Testzeitraums nur eine Empfehlung an den Kunden gezeigt wurde, nur 13,75 Benutzer würde erworbenen Empfehlung." Dies basiert auf der Annahme, dass das Modell mit Daten geschult wurde. Eine weitere Möglichkeit dazu ist die Genauigkeit 1 13,75.

Sie werden feststellen, wie an den Kunden mehr Elemente angezeigt werden, die Wahrscheinlichkeit, dass der Kunde Kauf empfohlen wird. Vorherigen Versuch verdoppelt die Wahrscheinlichkeit fast 26.61 Prozent bei 5 Elemente empfohlen werden.

#### <a name="percentage"></a>Prozentsatz
Der Prozentsatz der Benutzer mit mindestens einem empfohlenen *k interagiert* wird angezeigt. Der Prozentsatz wird berechnet, indem die Anzahl der Benutzer, die mit mindestens eine Empfehlung durch die Gesamtzahl der Benutzer als. Als weitere Benutzer angezeigt.

#### <a name="users-in-test"></a>Benutzer im test
Daten in dieser Zeile stellt die Gesamtzahl der Benutzer in das Test-Dataset.

#### <a name="users-considered"></a>Als Benutzer
Ein Benutzer wird nur berücksichtigt, wenn das System mindestens empfohlen *k* Elemente im Modell mit den Trainings-Dataset generiert.

#### <a name="users-not-considered"></a>Benutzer nicht berücksichtigt
Daten in dieser Zeile stellt Benutzern nicht berücksichtigt. Benutzer, die nicht mindestens erhalten *k* Komponenten.

Benutzer, die nicht als = Benutzer Test – Benutzer angesehen

<a name="Diversity"></a>
### <a name="diversity"></a>Vielfalt ###
Vielfalt Metriken messen Elementtypen empfohlen. Die folgende Tabelle stellt die Ausgabe der Vielfalt offline Auswertung.

|Quantil bucket |    0 bis 90|  90-99| 99-100
|------------------|--------|-------|---------
|Prozentsatz        | 34.258 | 55.127| 10.615


Gesamtanzahl Elemente empfohlen: 100.000

Eindeutige Elemente empfohlen: 954

#### <a name="percentile-buckets"></a>Quantil buckets
Jedem Bucket Quantil wird eine Spanne (minimale und maximale Werte, die zwischen 0 und 100) dargestellt. Artikel 100 sind die beliebtesten Produkte und nahe 0 Elemente werden am wenigsten häufig. Beispielsweise ist der Prozentwert für 99-100 %-Quantil Bucket 10.6 heißt es 10,6 Prozent empfohlenen nur die oberen davon beliebtesten Produkte zurückgegeben. Der Höchstwert ist exklusiv außer 100 Bucket minimalen Prozentwert ist inklusive
#### <a name="unique-items-recommended"></a>Eindeutige Elemente empfohlen
Die eindeutigen Elemente sollten Metrik zeigt die Anzahl der unterschiedliche Elemente, die für die Auswertung zurückgegeben wurden.
#### <a name="total-items-recommended"></a>Empfohlene Elemente (gesamt)
Die Gesamtzahl der Elemente empfohlen Metrik zeigt die Anzahl der Elemente empfohlen. Einige möglicherweise Duplikate.

<a name="ImplementingEvaluation"></a>
### <a name="offline-evaluation-metrics"></a>Offline-Auswertung Metriken ###
Präzision und Vielfalt offline Metriken können nützlich sein, wenn Sie die Builds auswählen. Zur Erstellungszeit an die jeweiligen FBT Empfehlung Buildparameter:

-   *EnableModelingInsights* Build-Parameter auf **true**festgelegt.
-   Optional Wählen Sie *SplitterStrategy* (entweder *RandomSplitter* oder *LastEventSplitter*).
*RandomSplitter* teilt Daten im Zug und Test auf bestimmte *RandomSplitterParameters* Test Prozent und zufälligen Startwert Werte.
*LastEventSplitter* teilt Daten im Zug und legt anhand der letzten Transaktion für jeden Benutzer.

Dadurch wird einen Build ausgelöst, der nur eine Teilmenge der Daten für Schulung und verwendet die restlichen Daten Bewertung Metriken berechnet.  Nach Abschluss des Builds müssen um die Ausgabe der Bewertung erhalten Sie die [Get Build Metriken API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f)übergibt den jeweiligen *ModelId* sowie *BuildId*aufrufen.

 Es folgt die JSON-Ausgabe für die Bewertung der Probe.


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }
