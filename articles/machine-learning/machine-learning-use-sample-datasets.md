<properties
    pageTitle="Verwenden der Stichprobenumfang Machine Learning Studio | Microsoft Azure"
    description="Beschreibung der Daten im Beispielmodelle ML Studio verwendet. Sie können diese Beispieldatensätzen für Experimente."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="use-the-sample-data-sets-in-azure-machine-learning-studio"></a>Verwenden Sie der Stichprobenumfang in Azure Machine Learning Studio

[top]: #machine-learning-sample-datasets

Beim Erstellen eines neuen Arbeitsbereichs in Azure Machine Learning sind eine Reihe von Beispieldatensätzen und Experimente standardmäßig enthalten. Viele dieser Beispieldatensätzen dienen die Beispielmodelle in [Azure Cortana Intelligence Katalog](http://gallery.cortanaintelligence.com/)und andere sind Beispiele für verschiedene Datentypen maschinelles lernen verwendet.

Einige dieser Datensätze sind in Azure BLOB-Speicher verfügbar. Für diese Datensätze enthält die Tabelle direkt. Können Sie diese Datensätze in den Experimenten mit den [Importierten Daten] [ import-data] Modul.

Die restlichen diese Beispieldatensätzen aufgeführt unter **Datasets gespeichert** Modul Palette links von der Leinwand Experiment beim Öffnen oder erstellen Sie einen neuen Test in ML.
Alle diese Datensätze können in Ihre eigenen experimentieren durch Ziehen der Leinwand experimentieren.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>DataSet-name</th>
  <th align=left>Datensatz Beschreibung</th>
</tr>

<tr>
  <td valign=top>Erwachsene Erhebung Einnahmen binäre Klassifizierung dataset</td>
  <td valign=top>
Eine Teilmenge der 1994 Erhebung Datenbank Berufstätige ab 16 angepasste GuV Index > 100.<p> </p><b>Verwendung:</b> klassifizieren Personen Demographie vorherzusehen, ob eine Person über 50 K pro Jahr verdient.<p> </p><b>Forschung:</b> Kohavi, R. Becker, B. (1996). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California, School of Information und Informatik </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>AirPort Codes Dataset</td>
  <td valign=top>
US Airport Codes.<p> </p>Dieses Dataset enthält eine Zeile für jeden US Flughafen die Airport ID und Name, Standort und Zustand.
  </td>
</tr>

<tr>
  <td valign=top>Automobile Daten (Raw)</td>
  <td valign=top>
Informationen über Autos von Marke und Modell, einschließlich Preis, Funktionen wie die Anzahl der Zylinder und MPG sowie eine Versicherung Risikowert.<p> </p>Der Risikowert zunächst automatisch Preis zugeordnet ist und bereinigt Risiken in einem Prozess stellen als symboling bezeichnet. Der Wert + 3 bedeutet, dass Auto riskant ist und-3 davon wahrscheinlich ziemlich sicher ist.<p> </p><b>Verwendung:</b> Features Regression oder Multivariate Klassifizierung der Risikowert Vorhersagen. <p> </p><b>Forschung:</b> Schlemmers, j.c. (1987). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California, School of Information und Informatik </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Fahrrad Vermietung UCI dataset</td>
  <td valign=top>
UCI-Verleih Dataset, die echten Daten aus Capital Bikeshare Unternehmen basiert, die ein Fahrrad Vermietung Netzwerk in Washington DC verwaltet.<p> </p>Das Dataset enthält eine Zeile für jede Stunde des Tages 2011 und 2012 insgesamt 17.379 Zeilen. Der Verleih stündlich reichen von 1 bis 977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Bill Gates RGB-Bild</td>
  <td valign=top>
Öffentlich zugängliche Bilddatei in CSV-Daten konvertiert.<p> </p>Der Code für das Bild konvertieren erfolgt Modell <strong>Quantisierung K-Means clustering mit Farbe</strong> auf.
  </td>
</tr>

<tr>
  <td valign=top>Blut Spende Daten</td>
  <td valign=top>
Eine Teilmenge der Daten aus der Datenbank Blut Spender von Bluttransfusion Service Center Hsin-Chu City, Taiwan.<p> </p>Spenderdaten einschließlich der Monate seit dem letzten Spende), und Häufigkeit oder die Gesamtzahl der Spenden, Zeit seit der letzten Spende und Blut gespendet.<p> </p><b>Verwendung:</b> über Klassifizierung abschätzen, ob der Spender im März 2007 Blutspenden, wobei 1 Spender die Zielperiode und 0 bedeutet nicht Spender soll. <p> </p><b>Forschung:</b> Yeh, IC, (2008). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California, School of Information und Informatik <p> </p>Yeh, I-Cheng Yang King-Jang und Ting, Tao-Ming "Knowledge Discovery RFM-Modell mit Bernoulli-Sequenz" Expert Systeme mit, 2008 <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Buchbesprechungen von Amazon</td>
  <td valign=top>
Überprüfung der Bücher bei Amazon amazon.com Website von Forschern der University of Pennsylvania (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">Gefühl</a>) entnommen. Finden Sie die Forschungsarbeit "Biographien, Bollywood, Ausleger Felder und Mixer: Domäne Anpassung Stimmung Klassifizierung" John Blitzer Mark Dredze und Fernando Pereira; Zuordnung von Computerlinguistik (ACL) 2007.<p> </p>Das ursprüngliche Dataset hat 975K Bewertungen mit 1, 2, 3, 4 oder 5. Die Berichte wurden in Englisch verfasst und den Zeitraum 1997-2007 sind. Dieses Dataset wurde ab 10 K Bewertungen aufgenommen.
  </td>
</tr>

<tr>
  <td valign=top>Brust Krebs Daten</td>
  <td valign=top>
Einer der drei im Zusammenhang mit Krebs Datasets Onkologie Institut, die häufig Computer lernen Literatur angezeigt. Diagnoseinformationen vom Labor mit 300 Gewebeproben kombiniert.<p> </p><b>Verwendung:</b> Krebs Klassifizierung basierend auf 9 Attribute, von denen einige linear und einige kategorischen. <p> </p><b>Forschung:</b> O.L Wohlberg, Wh, Straße, flugpionier und Mangasarian, (1995). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California, School of Information und Informatik </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Brust Krebs Features <td valign=top>
Das Dataset enthält Informationen für verdächtige Regionen 102K (Kandidaten) Röntgenbilder, von 117 Features beschrieben. Die Funktionen sind proprietär und ihre Bedeutung nicht durch die Dataset-Ersteller (Siemens Healthcare) angezeigt wird. 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Brust Krebs Info</td>
  <td valign=top>
Das Dataset enthält zusätzliche Informationen für jeden verdächtigen Bereich Bild. Jedes Beispiel enthält Informationen (z. B. Bezeichnung Patienten ID Koordinaten relativ zum gesamten Bild Patch) über die entsprechende Zeilennummer im Dataset Brust Krebs Features. Jeder Patient hat eine Reihe von Beispielen. Bei Krebs haben einige Beispiele für positive und einige negativ. Für Patienten Krebs haben, sind Beispiele negativ. Das Dataset enthält Beispiele 102K. Dataset gewichtet, 0,6 % der Punkte sind positiv, negativ werden. Das Dataset wurde von Siemens Healthcare zur Verfügung gestellt.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>CRM Appetency Etiketten freigegeben</td>
  <td valign=top>
Etiketten der KDD Cup 2009 Beziehung Vorhersage Kundenanfragen (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>CRM Abwanderung Etiketten freigegeben</td>
  <td valign=top>
Etiketten der KDD Cup 2009 Beziehung Vorhersage Kundenanfragen (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>CRM-Datensatz freigegeben</td>
  <td valign=top>
Diese Daten stammen aus der KDD Cup 2009 Beziehung Vorhersage Kundenanfragen (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>Das Dataset enthält 50K Kunden Französisch Telekommunikationsunternehmen Orange. Jeder Kunde hat 230 anonymisierte Features, 190, numerischen und 40 kategorischen sind. Die Funktionen sind sehr gering.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>CRM Upselling Etiketten freigegeben</td>
  <td valign=top>
Etiketten der KDD Cup 2009 Beziehung Vorhersage Kundenanfragen (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Energie-Effizienz Regression-Daten</td>
  <td valign=top>
Eine Auflistung von simulierten energieprofile, 12 verschiedenen Shapes. Die Gebäude unterscheiden zu 8 Features wie Scheiben Bereich der Scheiben Bereich Verteilung und Ausrichtung unterschieden.<p> </p><b>Verwendung:</b> Regression oder Klassifizierung Bewertung anhand eines echten Werten zwei Antworten Energieeffizienz Vorhersagen verwenden. Multi-Klasse Klassifizierung ist die antwortvariable als auf die nächste Ganzzahl. <p> </p><b>Forschung:</b> Xifara a & Tsanas a (2012). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California, School of Information und Informatik </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Flug verzögert Daten</td>
  <td valign=top>
Passagier Zeit Leistungsdaten sammeln der TranStats das US-Verkehrsministerium (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">Zeit</a>) entnommen.<p> </p>Das Dataset umfasst den Zeitraum April-Oktober 2013. Vor dem Hochladen auf Azure ML Studio wurde das Dataset wie folgt verarbeitet:<ul><li>Das Dataset wurde auf nur 70 größten Flughäfen der USA gefiltert</li><li>Stornierte Flüge wurden von mehr als 15 Minuten verzögert gekennzeichnet</li><li>Umgeleitete Flüge wurden herausgefiltert.</li><li>Die folgenden Spalten wurden: Jahr, Monat, DayofMonth, DayOfWeek, Netzbetreiber, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, abgebrochen</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Flugleistung (Raw)</td>
  <td valign=top>
Datensätze von Flugzeug Ankunfts- und USA vom Oktober 2011.<p> </p><b>Verwendung:</b> flugverspätungen Vorhersagen. <p> </p><b>Forschung:</b> vom US Department of Transportation <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Waldbrände Daten</td>
  <td valign=top>
Enthält Daten, Temperatur und Feuchtigkeit Indizes wie Geschwindigkeit, aus Nordost Portugal, kombiniert mit Feuer.<p> </p><b>Verwendung:</b> diese regressionsaufgabe ist ein schwer, wo soll Bereich gebrannten Waldbrandrisiko vorherzusagen. <p> </p><b>Forschung:</b> Cortez p & Morais, A. (2008). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California, School of Information und Informatik <p> </p>Cortez und Morais, 2007] S. Cortez und A. Morais. Ein Data Mining Ansatz für Vorhersagen Brände Meteorologische Daten. In J. Neves, M. F. Santos und J. Maurer EDS, neue Trends im KI Verfahren 13. EPIA 2007 - portugiesische Konferenz zu künstlicher Intelligenz, Dezember Guimarães, Portugal, s. 512-523, 2007. APPIA 13 ISBN 978-989-95618-0-9. Verfügbar unter: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Deutsch Kreditkarte UCI dataset</td>
  <td valign=top>
UCI-Statlog (Deutsch Kreditkarte) das Dataset (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Deutsch + Kreditdaten +</a>) mithilfe der Datei german.data.<p> </p>Das Dataset klassifiziert Personen durch einen Satz von Attributen als niedrig oder hoch Kreditrisiken beschrieben. Jedes Beispiel stellt eine Person dar. Gibt 20, numerischen und Kategorieliste, und eine binäre Bezeichnung (Wert für das Kreditrisiko). Hohe Risiken Habenposten haben Bezeichnung 2 = niedrige Risiken Habenposten haben Bezeichnung = 1. Ein niedriges Risiko wird als hoch sicherzustellen kostet 1, während ein hohes Risiko wird als niedrig sicherzustellen 5 kostet.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>IMDB Titel</td>
  <td valign=top>
Das Dataset enthält Informationen zu Filmen Twitter Tweets bewertet wurden: IMDB Film-ID Film und Genre, Produktion. Es gibt 17 KB Filme im Dataset. Das Dataset wurde in der "S. Dooms, T. De Pessemier und L. Martens. MovieTweetings: ein Film Dataset Bewertung von Twitter gesammelt werden. Workshop Crowdsourcing und menschliche Berechnung für Empfehlungssysteme, CrowdRec RecSys 2013."
  </td>
</tr>

<tr>
  <td valign=top>Iris zwei Daten</td>
  <td valign=top>
Dies ist vielleicht bekannteste Datenbank Musters Recognition Literatur. Die Datengruppe ist mit 50 jedes Blatts Maße aus drei Iris relativ gering.<p> </p><b>Verwendung:</b> Iris Typ aus den Vorhersagen.  <p> </p><b>Forschung:</b> Fisher, RA (1988). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California, School of Information und Informatik </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Tweets Film</td>
  <td valign=top>
Das Dataset ist eine erweiterte Version des Films Tweetings Dataset. Das Dataset enthält 170K Bewertungen für Filme aus strukturierten Tweets Twitter extrahiert. Jede Instanz stellt einen Tweet und ein Tupel: Mitgliedsname IMDB Film-ID Bewertung, Zeitstempel, Anzahl der Favoriten Tweet und Anzahl Retweets Tweet. Das Dataset wurde A. sagte, S. Dooms Loni b und d Tikk Recommender Systeme Herausforderung 2014 Verfügung.
  </td>
</tr>

<tr>
  <td valign=top>MPG Daten für verschiedene Autos</td>
  <td valign=top>
Dieses Dataset ist eine leicht abgeänderte Version des Datasets StatLib Bibliothek des Carnegie Mellon University. Das Dataset wurde in den 1983 American statistische Zuordnung verwendet.<p> </p>Die Daten sind Verbrauch für verschiedene Autos in Meilen pro Gallone und so die Anzahl der Zylinder, Hubraum, PS, Gewicht und Beschleunigung Informationen aufgeführt.<p> </p><b>Verwendung:</b> Kraftstoffverbrauch basierend auf 3 mehrwertigen diskrete und 5 kontinuierliche Attribute vorherzusagen. <p> </p><b>Forschung:</b> StatLib, Carnegie Mellon University (1993). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California, School of Information und Informatik </td>
</tr>

<tr>
  <td valign=top>Pima Indianern Diabetes binäre Klassifizierung dataset</td>
  <td valign=top>
Eine Teilmenge der Daten aus dem National Institute of Diabetes und Magen- und Nierenerkrankungen-Datenbank. Das Dataset wurde auf weibliche Patienten indischen Pima Erbe gefiltert. Die Daten umfassen medizinische Daten wie Glukose und insulinspiegel sowie Faktoren.<p> </p><b>Verwendung:</b> Vorhersagen, ob das Thema Diabetes (binäre Klassifizierung) hat. <p> </p><b>Forschung:</b> Sigillito V. (1990). UCI maschinelles lernen Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, Kalifornien: University of California, School of Information und Informatik </td>
</tr>

<tr>
  <td valign=top>Restaurantdaten</td>
  <td valign=top>
Ein Satz von Metadaten einschließlich Demographie und Voreinstellungen.<p> </p><b>Verwendung:</b> dieses Dataset in Kombination mit den anderen beiden Restaurants, und Testen Sie Recommender System verwenden. <p> </p><b>Forschung:</b> Bache, K. und Lichman, M. (2013). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California School of Information und Informatik.
  </td>
</tr>

<tr>
  <td valign=top>Restaurant Objektdaten</td>
  <td valign=top>
Ein Satz von Metadaten Restaurants und die lebensmitteltyp Restaurant Format und Speicherort.<p> </p><b>Verwendung:</b> dieses Dataset in Kombination mit den anderen beiden Restaurants, und Testen Sie Recommender System verwenden. <p> </p><b>Forschung:</b> Bache, K. und Lichman, M. (2013). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California School of Information und Informatik.
  </td>
</tr>

<tr>
  <td valign=top>Restaurant-Bewertung</td>
  <td valign=top>
Enthält Bewertungen von Benutzern Restaurants auf einer Skala von 0 auf 2.<p> </p><b>Verwendung:</b> dieses Dataset in Kombination mit den anderen beiden Restaurants, und Testen Sie Recommender System verwenden. <p> </p><b>Forschung:</b> Bache, K. und Lichman, M. (2013). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California School of Information und Informatik.
  </td>
</tr>

<tr>
  <td valign=top>Ausglühen mehreren Klassen Dataset Stahl</td>
  <td valign=top>
Dieses Dataset enthält eine Reihe von Datensätzen aus Glühen mit physischen Attribute (Breite, Stärke, Art (Spirale, Blatt) der resultierenden Stahl Typen.<p> </p><b>Verwendung:</b> Vorhersagen zwei numerische Klasse Attribute; Härte oder Stärke. Sie können auch Korrelationen zwischen Attributen analysieren.<p> </p>Stahlsorten folgen einen Reihe Standard festgelegten SAE und andere Organisationen. Sie suchen eine bestimmte "Klasse" (die Klassenvariable) und benötigten Werte erkennen. <p> </p><b>Forschung:</b> Pfund Sterling, D. & Buntine w (NA). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California, School of Information und Informatik <p> </p>Ein nützliches Handbuch Stahlsorten finden Sie hier: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Werden</td>
  <td valign=top>
Datensätze der hohe Gamma Partikel Bursts und Hintergrundgeräusche beide Monte-Carlo Prozess simuliert.<p> </p>Der Zweck der Simulation wurde zur Verbesserung der Genauigkeit der Boden der Cherenkov Gamma Teleskope mit statistischen Methoden das gewünschte Signal (Cherenkov-Strahlung Duschen) Hintergrundgeräusche (hadronische Schauer Kosmische in der oberen Atmosphäre initiiert) unterscheiden.<p> </p>Die Daten wurden bereits verarbeitet zu einem konischen Cluster mit der Achse Kamera zentriert ausgerichtet ist. Die Merkmale dieser Ellipse (oft als Hillas-Parameter) gehören die Parameter, die zur Diskriminierung verwendet werden können.<p> </p><b>Verwendung:</b> Vorhersagen, ob Abbild der Dusche Signal oder Hintergrundgeräusche darstellt.<p> </p><b>Hinweise:</b> einfache Genauigkeit ist seit Klassifizierung Hintergrund Ereignis als Signal als Klassifizierung Signalereignis als Hintergrund ist nicht für diese Daten. Vergleich der verschiedenen Klassifizierer sollte das ROC Diagramm verwendet werden. Die Wahrscheinlichkeit Hintergrund Ereignis wie Signal unter einer der folgenden Schwellenwerte akzeptieren: 0,01, 0,02, 0,05, 0,1 bzw. 0,2.<p> </p>Beachten Sie, dass Anzahl hintergrundereignisse (für hadronische Schauer h) unterschätzt, während echte Messungen h oder Rauschen Klasse die meisten Ereignisse darstellt. <p> </p><b>Forschung:</b> Bock, RK (1995). UCI-Maschine Repository <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>lernen. Irvine, Kalifornien: University of California School of Information </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Wetter Dataset</td>
  <td valign=top>
Stündliche terrestrischen wetterbeobachtungen aus NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">Seriendruckdaten aus 201304, 201310</a>).<p> </p>Daten werden Bemerkungen Airport Wetterstationen für den Zeitraum April-Oktober 2013 behandelt. Vor dem Hochladen auf Azure ML Studio wurde das Dataset wie folgt verarbeitet:<ul><li>Wetter Station-IDs wurden entsprechende Airport IDs zugeordnet.</li><li>Wetterstationen 70 größten Flughäfen zugeordnet wurden herausgefiltert.</li><li>Die Spalte wurde in separate Spalten für Jahr, Monat und Tag aufgeteilt.</li><li>Die folgenden Spalten wurden: AirportID, Jahr, Monat, Tag, Uhrzeit, Zeitzone, SkyCondition, Sichtbarkeit, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, Windstärke, Windrichtung, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Höhenmesser</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedia SP 500 Dataset</td>
  <td valign=top>
Daten werden von Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) auf Artikel des S & P 500 jedes, als XML-Daten abgeleitet.<p> </p>Vor dem Hochladen auf Azure ML Studio wurde das Dataset wie folgt verarbeitet:<ul><li>Extrahieren von Textinhalt für jeden Mandanten</li><li>Wiki-Formatierung entfernen</li><li>Nicht-alphanumerischen Zeichen entfernen</li><li>Konvertieren von Text in Kleinbuchstaben</li><li>Unternehmen Kategorien hinzugefügt wurden</li></ul><p> </p>Sich einige Unternehmen Artikel konnte nicht gefunden werden, also die Anzahl der Datensätze unter 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
Das Dataset enthält Daten und Angaben über ihre Reaktion auf direkte Mailings. Jede Zeile stellt einen Kunden dar. Das Dataset enthält 9 Benutzer demografische Daten und Verhalten und 3 Kennungsspalten (besuchen, Konvertierung und).  Ist eine binäre Spalte, die angibt, dass ein Kunde nach der Marketingkampagne besucht, Konvertierung gibt ein Kunde etwas gekauft und verbringen ist der Betrag, der ausgegeben wurde.  Das Dataset wurde von Kevin Hillstrom für MineThatData e-Mail-Analyse und Data Mining zur Verfügung gestellt.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Funktionen der Test Beispiele im Dataset RCV1 V2 Reuters News. Das Dataset enthält 781K Artikel zusammen mit deren IDs (erste Spalte des Dataset). Jeder Artikel zerlegt, Stopworded, und ergaben. Das Dataset wurde David bereitgestellt. D. Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Funktionen der Schulung Beispiele im Dataset RCV1 V2 Reuters News. Das Dataset enthält Artikel 23K sowie deren IDs (erste Spalte des Dataset). Jeder Artikel zerlegt, Stopworded, und ergaben. Das Dataset wurde David bereitgestellt. D. Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
DataSet der KDD Cup 1999 wissen und Data Mining Tools Wettbewerb (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>Dataset wurde heruntergeladen und in Azure BLOB-Speicher (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) gespeichert und Schulung sowohl Datasets testen. Trainings-Dataset hat etwa 126 KB Zeilen und 43 Spalten einschließlich der Etiketten 3 Spalten sind Teil der Beschriftung und 40 Spalten, die numerische Zeichenfolge/kategorische Features, stehen zum Trainieren des Modells. Die Testdaten hat ca. 22,5 K Beispiele mit 43 dieselben Spalten wie die Trainingsdaten testen.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1 v2.topics.qrels.csv</a></td>
  <td valign=top>
Thema Arbeitsaufträge für Newsbeiträge im Dataset RCV1 V2 Reuters News. Ein Artikel kann mehrere Themen zugeordnet. Das Format jeder Zeile ist "<topic name>  <document id> 1". Das Dataset enthält 2.6M Thema Aufgaben. Das Dataset wurde David bereitgestellt. D. Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Diese Daten stammen aus KDD Cup 2010 Student Performance Evaluation Challenge (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">Student Bewertung</a>). Daten sind die Algebra_2008_2009 Training (Stempel, J., Niculescu-Mizil A. Ritter, S. Gordon g. und Koedinger K.R (2010). Algebra I 2008-2009. Fordern Sie Daten aus KDD Cup 2010 erzieherischen Data Mining Herausforderung. Besuchen sie <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> oder <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>Dataset wurde heruntergeladen und in Azure BLOB-Speicher (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) gespeichert und Student Nachhilfe System-Dateien enthält. Bereitgestellten Funktionen enthalten Problem-ID und eine kurze Beschreibung, Studentenausweis, Timestamp und wie viele Versuche die Kursteilnehmer vor der Fehlerursache richtig gemacht. Wurde das ursprüngliche Dataset 8,9 M Datensätze, Dataset nach unten auf die ersten 100K Zeilen abgetastet. Das Dataset enthält 23 Tabulatoren getrennten Spalten verschiedener: Kategorieliste, numerischen und Timestamp.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
