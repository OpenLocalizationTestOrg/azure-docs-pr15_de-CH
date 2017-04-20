<properties
    pageTitle="Beispiele für Lucene Azure suchen | Microsoft Azure-Suche"
    description="Lucene Abfragesyntax für fuzzy-Suche, Suche Begriff angehoben, Suche mit regulären Ausdrücken und Platzhaltersuche."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="Lucene query analyzer syntax"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="liamca"
/>

# <a name="lucene-query-syntax-examples-for-building-queries-in-azure-search"></a>Lucene Abfragebeispiele Syntax zum Erstellen von Abfragen in Azure Suche

Beim Erstellen von Abfragen für Azure Suche können Sie die [einfache Abfragesyntax](https://msdn.microsoft.com/library/azure/dn798920.aspx) oder alternative [Lucene Abfrage Parser in Azure suchen](https://msdn.microsoft.com/library/azure/mt589323.aspx). Der Parser Lucene Abfrage unterstützt komplexere Abfragekonstrukte wie Feld Bereich Abfragen fuzzy-Suche, Suche, Begriff steigern und Ihre Ausdruck suchen.

In diesem Artikel können Sie Beispiele durchgehen, die Abfragesyntax Lucene und Ergebnisse nebeneinander anzeigen. Beispiele ausführen eine vorinstallierte Suchindex [JSFiddle](https://jsfiddle.net/), ein Online-Code-Editor für HTML und Skript. 

Mit der rechten Maustaste auf die Abfrage Beispiel URLs JSFiddle in einem separaten Browserfenster geöffnet.

> [AZURE.NOTE] Die folgenden Beispiele nutzen ein Suchindexes Stellenangebote basierend auf Initiative [Stadt New York OpenData](https://nycopendata.socrata.com/) bereitgestellten Dataset aus. Diese Daten nicht aktuell bzw. unvollständig berücksichtigen. Der Index ist eine Sandbox-Dienst von Microsoft bereitgestellt. Ein Azure-Abonnement oder Azure Suche versuchen diese Abfragen ist nicht erforderlich.

## <a name="viewing-the-examples-in-this-article"></a>Die Beispiele anzeigen in diesem Artikel

Alle Beispiele in diesem Artikel angeben Lucene Abfrage Parser über**Abfragetyp** Suchparameter Verwendung der Parser Lucene Abfrage Code Geben Sie bei jeder Anforderung **Abfragetyp** .  Gültige Werte sind **einfache**|**voll**mit **einfachen** Standard und **vollständige** Lucene Abfrage Parser. Details über das Festlegen von Abfrageparametern finden Sie unter [Dokumente durchsuchen (Azure Search Service-REST-API)](https://msdn.microsoft.com/library/azure/dn798927.aspx) .

**Beispiel 1** – mit der rechten Maustaste die folgende Abfrage Ausschnitt in einem neuen Browser geöffnet, die JSFiddle lädt und führt die Abfrage aus:
- [& QueryType = vollständig & Suchen = *](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*)

Diese Abfrage gibt Dokumente aus Aufträgen Index (Sandbox-Dienst geladen)

Im neuen Browserfenster wird JavaScript Quell- und HTML-Ausgabe nebeneinander angezeigt. Das Skript verweist auf eine Abfrage von URLs wird in diesem Artikel bereitgestellt wird. Beispielsweise wird in **Beispiel 1**die zugrunde liegenden Abfrage wie folgt:

    http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26searchFields=business_title%26$select=business_title%26queryType=full%26search=*

Beachten Sie die Abfrage verwendet eine vorkonfigurierte Azure Search Nycjobs aufgerufen. Der Parameter **Suchfeldern** beschränkt die Suche auf nur im Titelfeld Business. Der **Abfragetyp** wird auf **voll**festgelegt Azure Suche Lucene Abfrage Parser für diese Abfrage.

### <a name="fielded-query-operation"></a>Fielded Abfrage

Sie können die Beispiele in diesem Artikel durch Angabe einer **Fieldname:searchterm** Konstruktion definieren fielded Abfrageoperation, wobei das Feld Wort und Suchbegriff ist auch ein einzelnes Wort oder einen Ausdruck optional mit booleschen Operatoren. Die folgenden: Beispiele

- Business_title:(senior NOT junior)
- Status: ("New York" und "New Jersey")

Achten Sie darauf, dass Sie mehrere Zeichenfolgen in Anführungszeichen setzen, wenn beide Zeichenfolgen als eine Einheit wie zwei unterschiedliche Städte in Feld Ort suchen ausgewertet werden sollen. Stellen Sie außerdem sicher, der Operator aktiviert, siehe mit und und.

In **Fieldname:searchterm** angegebene Feld muss ein Feld durchsucht werden. Verwendung Indexattribute in Felddefinitionen Einzelheiten finden Sie unter [Create Index (Azure Search Service-REST-API)](https://msdn.microsoft.com/library/azure/dn798941.aspx) .

## <a name="fuzzy-search"></a>Fuzzy-Suche

Eine fuzzy-Suche sucht entspricht in Begriffen, die ähnlich sein. Pro [Lucene Dokumentation](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)Unscharfe Suche [Damerau Levenshtein Abstand](https://en.wikipedia.org/wiki/Damerau%e2%80%93Levenshtein_distance)basieren.

Eine fuzzy-Suche verwenden Sie die Tilde "~" Symbol am Ende eines einzelnen Wortes mit optionalen Parameter einen Wert zwischen 0 und 2, bearbeiten Abstand angibt. Z. B. "blue ~" oder "blue ~ 1" zurück Blau, Blau und Kleben.

**Beispiel 2** - Maustaste folgende Abfrage Ausschnitt, es auszuprobieren. Diese Abfrage sucht nach Titel Senior Begriff Sie aber nicht junior Business:

- [& QueryType = vollständig & Suchen = Business_title:senior nicht junior](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:senior+NOT+junior)

## <a name="proximity-search"></a>Suche

NEAR-Suchen verwendet, um Begriffe zu suchen, die nahe beieinander in ein Dokument. Einfügen eine Tilde "~" Symbol am Ende eines Satzes gefolgt von der Anzahl der Wörter, die Nähe Berandung erstellen. Beispielsweise "Hotel Flughafen" 5 finden Begriffe Hotel und Flughafen 5 Wörter in einem Dokument.

**Beispiel 3** – mit der rechten Maustaste im folgenden Codeausschnitt Abfrage. Diese Abfrage sucht nach mit der Begriff zuordnen (wo er falsch geschrieben wurde):

- [& QueryType = vollständig & Suchen = Business_title:asosiate ~](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:asosiate~)

**Beispiel 4** – die Abfrage mit der rechten Maustaste. Suchen Sie mit dem Begriff "senior Analyst", um nicht mehr als ein Wort aufweist:

- [& QueryType = vollständig & Suchen = Business_title: "senior Analyst" ~ 1](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~1)

**Beispiel 5** – versuchen Sie es erneut Wörter zwischen "senior Analyst" entfernen.

- [& QueryType = vollständig & Suchen = Business_title: "senior Analyst" ~ 0](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:%22senior%20analyst%22~0)

## <a name="term-boosting"></a>Langfristige Steigerung

Steigerung der Begriff bezieht sich auf Einstufung eines Dokuments Wenn sie stärkere Begriff relativ Dokumente enthält, die nicht den Begriff enthalten. Dies unterscheidet sich von Bewertung Profile, bewertet Profile bestimmte Felder statt konkret steigern. Das folgende Beispiel veranschaulicht die Unterschiede.

Erwägen Sie, ein steigert Bewertungsprofil in einem bestimmten Bereich, wie **Genre** im Beispiel Musicstoreindex entspricht. Steigerung der Begriff kann zu weiteren bestimmte Suche über andere Begriffe verwendet werden. Beispielsweise "Rock ^ 2 elektronische" verbessern Dokumente, Suchbegriffe im Feld **Genre** höher als andere durchsuchbare Felder im Index enthalten. Außerdem werden Dokumente mit den Suchbegriff "Rock" höher als die anderen Suchbegriff "elektronische" durch den Begriff Boost Wert (2) eingestuft.

Einen Begriff erhöhen, verwenden Sie das Caretzeichen "^", Symbol mit Verstärkung (eine Zahl) am Ende des Ausdrucks kennen. Je höher der Boost Faktor, mehr wird gegenüber anderen Suchbegriffe werden. Standardmäßig ist der Boost Faktor 1. Obwohl Boost Faktor positiv sein muss, kann es kleiner als 1 (z. B. 0,2).

**Beispiel 6** – die Abfrage mit der rechten Maustaste. Mit dem Begriff "Computer Analyst" Wir sehen keine Ergebnisse mit Wörtern Computer und Analytiker noch Analyst Aufträge werden am Anfang der Ergebnisse suchen.

- [& QueryType = vollständig & Suchen = Business_title:computer Analyst](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

**Beispiel 7** – versuchen Sie es erneut, dieses Mal steigert mit dem Begriff Computer über Begriff Analyst führt, wenn beide Wörter vorhanden sind.

- [& QueryType = vollständig & Suchen = Business_title:computer ^ 2 Analyst](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26$select=business_title%26queryType=full%26search=business_title:computer%5e2%20analyst)

## <a name="regular-expression"></a>Reguläre Ausdrücke

Eine Suche mit regulären Ausdrücken sucht basierend auf dem Inhalt zwischen Schrägstriche "/", wie in der [RegExp-Klasse](http://lucene.apache.org/core/4_10_2/core/org/apache/lucene/util/automaton/RegExp.html)dokumentiert.

**Beispiel 8** - Abfrage mit der rechten Maustaste. Suchbegriff für Projekte mit der Senior oder Junior.

- `&queryType=full&$select=business_title&search=business_title:/(Sen|Jun)ior/`

Die URL für dieses Beispiel wird auf der Seite nicht ordnungsgemäß gerendert. Um dieses Problem zu umgehen kopieren Sie den folgenden URL und Browser-URL-Adresse einfügen:    `http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:/(Sen|Jun)ior/)`


## <a name="wildcard-search"></a>Platzhaltersuche

Verwenden Sie allgemein anerkannten Syntax für mehrere (\*) oder (?) Zeichen Platzhalterzeichen. Hinweis Lucene Abfrage Parser unterstützt die Verwendung dieser Symbole und ein einzelner Begriff nicht im Ausdruck.

**Beispiel 9** - Abfrage mit der rechten Maustaste. Suchen Sie nach Aufträgen, die das Präfix "Prog" mit Business Titel Programmieren und Programmierer darin enthalten.

- [& QueryType = Full & $select = Business_title & Search = Business_title:prog*](http://fiddle.jshell.net/liamca/gkvfLe6s/1/?index=nycjobs&apikey=252044BE3886FE4A8E3BAA4F595114BB&query=api-version=2015-02-28-Preview%26queryType=full%26$select=business_title%26search=business_title:prog*)

Sie können kein * oder? Symbol als erstes Zeichen einer Suche.


## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie, der Parser Lucene Abfrage im Code angeben. Die folgenden Links Erläutern Sie Suchabfragen für .NET und die REST-API. Links verwenden standardmäßig einfach anwenden Sie aus diesem Artikel **Abfragetyp**angeben müssen.

- [Fragen Sie mit .NET SDK Azure Suchindex ab](search-query-dotnet.md)
- [Fragen Sie über die REST-API Azure Suchindex ab](search-query-rest-api.md)


 