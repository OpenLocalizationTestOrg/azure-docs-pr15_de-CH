<properties 
    pageTitle="Implementieren von facettierten Navigation in Azure Search | Microsoft Azure | Navigationskategorien" 
    description="Anwendung in Azure Suche einen Suchdienst Cloud gehostet auf Microsoft Azure integriert fügen Sie facettierten Navigation hinzu." 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="heidist"/>

#<a name="how-to-implement-faceted-navigation-in-azure-search"></a>Implementieren von facettierten Navigation in Azure suchen

Facettierte Navigation ist ein Filtermechanismus, der selbst Drilldown-Navigation in Anwendung suchen bereitstellt. Der Begriff 'facettennavigation' nicht vertraut ist, ist fast ein zuvor verwendet haben. Wie das folgende Beispiel zeigt, ist facettierten Navigation lediglich Kategorien Ergebnisse filtern.

## <a name="what-it-looks-like"></a>So sieht es aus

 ![][1]
  
Facets können Sie was Sie suchen, während gleichzeitig sichergestellt wird, dass Sie nicht NULL. Als Entwickler können mit Facetten besonders Suchkriterien für Ihre Suche Corpus Navigation verfügbar machen. Im Online-Handel Applikationen basiert facettierten Navigation häufig über Marken Departments (Kinder Schuhe), Größe, Preis, Verbreitung und Bewertung. 

Facettierte Navigation Technologieübergreifende suchen unterscheidet und kann sehr komplex sein. In Azure Search facettierten Navigation Zeitpunkt Abfrage basiert attributierte Felder oben in Ihrem Schema angegeben. In Abfragen, die die Anwendung erstellt übermitteln eine Abfrage *facettenabfrageparameter* um verfügbaren Facet Filterwerte für dieses Dokument Resultset zu erhalten. Legen Sie tatsächlich das Dokument Ergebnis verkürzt, die Anwendung muss eine `$filter` Ausdruck.

Anwendung Entwicklung ist Code schreiben, der Abfragen erstellt den Großteil der Arbeit. Viele der facettierten Navigation möchten Anwendungsverhaltens erfolgt vom Dienst Unterstützung für Bereiche einrichten und zählt Facet Ergebnisse abrufen. Der Service umfasst auch sinnvolle Standardwerte, mit denen Sie vermeiden unhandlich Navigationsstrukturen. 

Dieser Artikel enthält folgende Abschnitte:

- [Gewusst wie: Erstellen](#howtobuildit)
- [Erstellen der Darstellungsschicht](#presentationlayer)
- [Index erstellen](#buildindex)
- [Daten überprüfen](#checkdata)
- [Erstellen der Abfrage](#buildquery)
- [Tipps zur Steuerung der facettierten navigation](#tips)
- [Facettierte Navigation basierend auf Wertebereiche](#rangefacets)
- [Facettierte Navigation basierend auf GeoPoints](#geofacets)
- [Probieren Sie es aus](#tryitout)

##<a name="why-use-it"></a>Warum verwenden
Die effektivsten suchapplikationen haben mehrere interaktionsmodelle neben suchen. Facettierte Navigation ist eine alternative Eingabestelle für die Suche eine praktische Alternative zu komplexe Suche Ausdrücke handschriftlich eingeben.

##<a name="know-the-fundamentals"></a>Die Grundlagen kennen

Neue Entwicklung suchen hingegen ist der beste Weg facettierten Navigation vorstellen Möglichkeiten selbst Suche anzeigt. Es ist ein Drilldown-Umgebung, basierend auf vordefinierten Filter zur Eingrenzung schnell Suchergebnisse durch Point-and-Click-Aktionen. 

**Interaktionsmodell**

Suchfunktionalität facettierten Navigation ist iterativ, wir verstehen als eine Folge von Abfragen, die in Reaktion auf Benutzeraktionen entfalten starten.

Ausgangspunkt ist eine Anwendungsseite, die facettierte Navigation in der Regel am Rand platziert. Facettierte Navigation ist häufig eine Struktur mit Kontrollkästchen für jeden Wert oder klickbaren Text. 

1.  Eine Abfrage nach Azure gesendet gibt die facettierten Navigationsstruktur über eine oder mehrere facettenabfrageparameter. Beispielsweise gehören die Abfrage `facet=Rating`, mit einer `:values` oder `:sort` Option, um die Präsentation zu optimieren.
2.  Die Darstellungsschicht rendert eine Suchseite, die facettierte Navigation mit Facetten in der Anforderung angegeben.
3.  Aufgrund einer facettierten Navigation, die Bewertung der Benutzer "4" an, dass nur Produkte mit 4 oder höher angezeigt werden soll. 
4.  Die Anwendung sendet als Antwort enthält eine Abfrage mit`$filter=Rating ge 4` 
5.  Die Darstellungsschicht aktualisiert die Seite, zeigen eine reduzierte Resultset enthält nur die Elemente, die die neuen Kriterien erfüllen (in diesem Fall Produkte 4 bewertet und).

Ein Facet Abfrageparameter ist nicht zu verwechseln mit Abfrage. Er wird nie als Auswahlkriterium in einer Abfrage verwendet. Stattdessen betrachten Sie facettenabfrageparameter als Eingaben für die Navigationsstruktur, die in der Antwort zurückkommt. Für jeden Facetten Abfrageparameter bereitgestellten wertet Azure Search Teilergebnisse für jeden Wert des Aspekts wie viele Dokumente sind.

Beachten Sie die `$filter` in Schritt 4. Dies ist ein wichtiger Aspekt der facettierten Navigation. Obwohl Facetten und Filter in der API unabhängig sind, benötigen Sie beide Erlebnis möchten. 

**Entwurfsmuster**

Im Anwendungscode ist das Muster facettenabfrageparameter mithilfe die facettierten Navigationsstruktur mit facettenergebnisse sowie eine $filter Ausdruck das Click-Ereignis. Stellen Sie sich die `$filter` an der Darstellungsschicht Ausdruck wie der Code hinter der tatsächlichen Verkürzen der Suchergebnisse zurückgegeben. Ein Facet Farben angegeben, auf die Farbe Rot über erfolgt eine `$filter` Ausdruck nur die Elemente, die Farbe Rot. 

**Abfrage-Grundlagen in Azure Suche**

Azure-Suche wird eine Anforderung durch eine oder mehrere Abfrageparameter angegeben (finden Sie eine Beschreibung der einzelnen [Dokumente durchsuchen](http://msdn.microsoft.com/library/azure/dn798927.aspx) ). Ohne die Abfrageparameter sind erforderlich, jedoch muss mindestens damit eine Abfrage gültig ist.

Genauigkeit im Allgemeinen verstanden, irrelevant Treffer Filtern erfolgt durch eine oder beide dieser Ausdrücke:

- **Suche =**<br/>
Der Wert dieses Parameters ist den Suchbegriff. Möglicherweise ein Stück Text oder komplexe Suchbegriff, der mehrere Ausdrücke und Operatoren enthält. Auf dem Server wird ein Suchbegriff für Volltextsuche, durchsuchbaren Felder im Index nach übereinstimmenden Begriffe Zurückgeben von Ergebnissen in der Reihenfolge Abfragen verwendet. Setzen Sie `search` null Abfrage Ausführung ist über den gesamten Index (d. h. `search=*`). In diesem Fall andere Elemente von der Abfrage wie eine `$filter` oder Bewertungsprofil, primären Faktoren Dokumente zurückgegeben `($filter`) und in welcher Reihenfolge (`scoringProfile` oder `$orderb`y).

- **$filter =**<br/>
Ein Filter stellt einen leistungsstarken Mechanismus zum Beschränken der Größe der Suchergebnisse basierend auf den Werten eines bestimmten Dokuments. Ein `$filter` zuerst ausgewertet, gefolgt von facettierung Logik, die die verfügbaren Werte und entsprechende Anzahl für jeden Wert generiert

Komplexe Suchausdrücken verringert die Leistung der Abfrage. Nutzen Sie möglichst wohlformulierte Filterausdrücke Genauigkeit und Leistung von Abfragen verbessern.

Vergleichen Sie einen komplexe Suche Ausdruck enthält einen Filterausdruck zum besseren Verständnis wie Filter präziser hinzugefügt:

- `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`

- `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Obwohl beide Abfragen gültig sind, ist die zweite überlegen, wenn Sie nicht aus mit in Seattle suchen. Die erste Abfrage verwendet diese Suchbegriffe erwähnten oder String-Felder wie Name, Beschreibung und einem anderen Feld mit durchsuchbaren Daten nicht erwähnt. Die zweite Abfrage sucht genaue Übereinstimmung auf strukturierte Daten und zu viel genauer.

In Clientanwendungen, die facettennavigation enthalten, sollten Sie sicher sein, dass jede Benutzeraktion über eine facettierte Navigationsstruktur einschränkende Suchergebnisse erzielt durch einen Filterausdruck beigefügt.

<a name="howtobuildit"></a>
##<a name="how-to-build-it"></a>Gewusst wie: Erstellen

Facettierte Navigation in Azure Suche erfolgt im Anwendungscode vordefinierte Elemente im Schema verwendet, die die Anforderung erstellt.

Vordefinierte Suchergebnisse Index ist die `Facetable [true|false]` Indexattribut ausgewählte Felder aktivieren oder deaktivieren ihre Verwendung in einem facettierten Navigationsstruktur festgelegt. Ohne `"Facetable" = true`, ein Feld kann in Facet Navigation verwendet werden.

Zeitpunkt der Abfrage erstellt der Anwendungscode eine Anforderung mit `facet=[string]`, eine Anforderungsparameter, Feld Facetten durch bereitstellt. Eine Abfrage können mehrere Aspekte wie `&facet=color&facet=category&facet=rating`, jeweils getrennt durch ein kaufmännisches und-Zeichen (&).

Anwendungscode muss auch erstellen eine `$filter` Ausdruck zur Ereignisbehandlung auf facettierten Navigation. Ein `$filter` verringert die Suchergebnisse der Aspektwert als Filterkriterien verwenden.

Azure Suche liefert Suchergebnisse pro vom Benutzer zusammen mit Updates facettierten Navigationsstruktur eingegebenen Begriffe. In Azure Search facettierten Navigation ist eine Ebene, Facetten Werte und zählt, wie viele Ergebnisse jeweils gefunden werden.

Die Darstellungsschicht im Code stellt die Benutzeroberfläche. Es sollte die Bestandteile der facettierten Navigation wie die Bezeichnung, Werte, Kontrollkästchen und die Anzahl der Liste. Die REST-API von Azure Suche ist plattformunabhängig, sodass alle Sprache und Plattform. Das wichtigste ist UI-Elemente enthalten, die inkrementelle Aktualisierung mit aktualisierten Benutzeroberfläche unterstützen zusätzliche Facetten ausgewählt ist. 

In den folgenden Abschnitten nehmen näher betrachten wie jedes Teil wir mit der Darstellungsschicht.

<a name="presentationlayer"></a>
##<a name="build-the-presentation-layer"></a>Erstellen der Darstellungsschicht

Arbeiten von der Darstellungsschicht können Sie Anforderungen, die sonst übersehen werden können und verstehen aufdecken, welche Funktionen Suchvorgänge sind.

Seite Web- oder Anwendungsserver in facettierten Navigation facettierten Navigationsstruktur zeigt, Benutzereingaben auf der Seite erkennt und fügt die geänderten Elemente. 

Für Web Applications AJAX häufig in der Darstellungsschicht verwendet, da Sie inkrementelle Änderung aktualisieren können. Sie können ASP.NET MVC oder andere visualisierungsplattform, die eine Azure-Suchdienst über HTTP herstellen kann. In diesem Artikel - **Katalog AdventureWorks** -beispielanwendung ist eine ASP.NET MVC-Anwendung.

Im folgenden Beispiel der **index.cshtml** -Datei der Anwendung entnommen erstellt eine dynamische HTML-Struktur mit facettierten Navigation in der Seite mit den Suchergebnissen. Im Beispiel facettierten Navigation in der Suchergebnisseite integriert und wird angezeigt, nachdem der Benutzer einen Suchbegriff übermittelt hat.

Beachten Sie, dass jede Facette eine Bezeichnung (Farben, Kategorien, Preise) eine Bindung zu einem facettierten Feld (Farbe, CategoryName ListPrice) und ein `.count` Parameter verwendet, um die Anzahl der Einträge für dieses Facet Ergebnis zurückzugeben.

  ![][2]
 

> [AZURE.TIP] Beim Entwerfen der Suchergebnisseite müssen Sie einen Mechanismus zum Löschen des Facets hinzufügen. Verwenden Sie Kontrollkästchen können Benutzer leicht die Filter löschen intuit. Für andere Layouts benötigen Sie möglicherweise eine Breadcrumb-Muster oder anderen kreativ. Anwendung Beispiel AdventureWorks Catalog können Sie z. B. den Titel AdventureWorks Katalog Suchseite zurücksetzen klicken.

<a name="buildindex"></a>
##<a name="build-the-index"></a>Index erstellen

Facettierung auf Basis von Feld im Index über dieses Indexattribut aktiviert: `"Facetable": true`.  
Sind alle Feldtypen, möglicherweise in der facettierten Navigation mit der `Facetable` standardmäßig. Diese Feldtypen gehören `Edm.String`, `Edm.DateTimeOffset`, und die numerischen Feldtypen (alle Feldtypen sind im Wesentlichen Facetable außer `Edm.GeographyPoint`, die nicht in facettennavigation verwendet werden). 

Beim Erstellen eines Indexes empfiehlt facettierten Navigation explizit facettierung für Felder deaktivieren, die nicht als ein Facet verwendet werden soll.  Insbesondere Zeichenfolgenfelder Singleton-Werte, z. B. eine ID oder Produkt sollte festgelegt werden `"Facetable": false` zu ihrer Verwendung zufälligen (und ineffektiv) eine facettierte Navigation.

Folgendes ist das Schema für AdventureWorks Katalog Beispiel-app (getrimmt einige Attribute Gesamtgröße reduzieren):

 ![][3]
 
Beachten Sie, dass `Facetable` ist deaktiviert für Zeichenfolgenfelder Facetten oder eine ID-Namen verwendet werden sollte. Facettierung deaktivieren sie dabei nicht hilft, die Größe des Indexes klein und im Allgemeinen verbessert die Leistung.

> [AZURE.TIP] Es wird empfohlen enthalten Sie die vollständige Indexattribute für jedes Feld. Obwohl `Facetable` ist standardmäßig für fast alle Felder absichtlich festlegen jedes Attribut können Sie die Auswirkungen jedes Schema Entscheidung denken. 

<a name="checkdata"></a>
##<a name="check-for-data-quality"></a>Qualität der Daten überprüfen 

Beim Entwickeln einer datenzentrischen Anwendung ist Vorbereiten der Daten häufig eine größere Teile des Auftrags. Suche sind keine Ausnahme. Die Qualität der Daten wirkt sich direkt auf facettierten Navigationsstruktur, ob materialisiert, erwarten sie sowie dessen Effektivität bei Filter erstellen, die das Ergebnis zu reduzieren.

Suchkorpus entsteht in Azure Suche von Dokumenten, die einen Index auffüllen. Dokumente liefern die Werte, mit denen Aspekte zu berechnen. Möchten Sie Facetten nach Marke oder Preis, sollte jedes Dokument Werte für *Name* und *Darfen* , die gültig, konsistent und produktiv wie eine Filteroption.

Einige Erinnerungen was für Scrubben sind:

- Fragen Sie für jedes Feld die gewünschten Facetten durch sich, ob Werte enthalten, die als Filter im selbst suchen. Die Werte sind kurzen beschreibenden und ausreichend charakteristischen löschen Auswahl zwischen verschiedenen Optionen.
- Rechtschreibfehler oder nahezu übereinstimmende Werte. Wenn Farbe und Feldwerte Facet Orange und Ornage (Fehler), basierend auf dem Feld Farbe Facets würden beide abholen.
- Gemischter Groß-Text kann zudem facettierten Navigation mit Orange und Orange als zwei unterschiedliche Werte anrichten. 
- Single und Plurale Versionen denselben Wert können für jede separate Facets führen.

Wie Sie sich vorstellen können, ist Sorgfalt bei der Vorbereitung der Daten ein wichtiger Aspekt der effektiven facettierten Navigation.

<a name="buildquery"></a>
##<a name="build-the-query"></a>Erstellen der Abfrage

Der Code, den Sie zum Erstellen von Abfragen schreiben sollten alle Teile einer gültigen Abfrage einschließlich Suchausdrücke, Facets, filtern, bewerten Profile – alles verwendet eine Anforderung formulieren angeben. In diesem Abschnitt werden wir untersuchen, Facetten in einer Abfrage anpassen und wie Filter mit zu einem reduzierten Resultset verwendet werden.

Ein Beispiel ist häufig ein guter Ausgangspunkt. Im folgenden Beispiel, aus der Datei **CatalogSearch.cs** erstellt eine Anforderung, die Facet Navigation basierend auf Farbe, Kategorie und Preis erstellt. 

Beachten Sie, dass Facetten in diese Anwendung sind. Die Suchfunktion in AdventureWorks Katalog dient facettierten Navigation und Filter. Dies spiegelt sich in herausragender Platzierung der facettierten Navigation auf der Seite. Beispiel-Anwendung enthält URI-Parameter für Facetten (Farbe, Kategorie, Preise) als Eigenschaften für die Suchmethode (wie in diesem Beispiel).

  ![][4]
 
Abfrageparameter Facet auf ein Feld und je nach Datentyp, kann weitere parametrisiert werden durch Komma getrennte Liste mit `count:<integer>`, `sort:<>`, `intervals:<integer>`, und `values:<list>`. Einrichten von Bereichen wird eine Werteliste für numerische Daten unterstützt. Einzelheiten finden Sie unter [Dokumente durchsuchen (Azure Such-API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Mit Facetten sollte Ihre Anwendung formulierte Anforderung auch Filter Candidate Dokumente basierend auf der Auswahl Wert Facet einzuschränken erstellen. Für LMN Veranstaltungsservice facettierten Navigation bietet Hinweise zu Fragen wie "welche Farben, Hersteller und Arten von Fahrrädern stehen", während des Filterns von Fragen wie "welche genaue Fahrräder sind rot, Mountainbikes in diesem Preis" Antworten.

Klickt ein Benutzer auf "Rot" Red Produkte angezeigt werden sollen, zählen die nächste Abfrage die Anwendung sendet `$filter=Color eq ‘Red’`.

## <a name="best-practices-for-faceted-navigation"></a>Best Practices für die facettierte navigation

Die folgende Liste enthält einige best Practices.

- **Genauigkeit**<br/>
Verwenden Sie Filter. Wenn Sie auf könnte nur die allein aufgrund Suchausdrücke ein Dokument haben, das genaue Aspektwert in ihre Felder nicht zurückgegeben werden. 

- **Zielfelder**<br/>
Im facettierten Drilldown soll in der Regel nur Dokumente enthalten, die den Wert des Aspekts in einem bestimmten Feld (facettiert) nicht über alle durchsuchbaren Felder. Hinzufügen eines Filters verstärkt das Zielfeld durch Weiterleitung des Dienstes nur im Feld facettierten für einen übereinstimmenden Wert zu suchen.

- **Index-Effizienz**<br/>
Wenn Ihre ausschließlich facettierten Navigation Anwendung (d. h. kein Suchfeld), markieren Sie das Feld als `searchable=false`, `facetable=true` zu einer kompakteren Index. Tritt außerdem nur auf ganze Facetwerte keine Silbentrennung oder Bauteile, die einen Wert mehreren Indizierung Indizieren.

- **Leistung**<br/>
Filter Candidate Dokumente Suche eingrenzen und Positionierung ausschließen. Haben Sie eine Reihe von Dokumenten erhalten mit Bedacht Facet Drilldown häufig bedeutend bessere Leistung Sie.


<a name="tips"></a> 
##<a name="tips-on-how-to-control-faceted-navigation"></a>Tipps zur Steuerung der facettierten navigation

Im folgenden ist Tipp mit auf bestimmte Probleme.

**Etiketten für jedes Feld in der Facet Navigation hinzufügen**

Etiketten sind in der Regel in HTML oder Form (in diesem Beispiel**index.cshtml** ) definiert. Es ist keine API Azure Facet Navigation Etiketten suchen oder jede andere Art von Metadaten.

**Definieren Sie, welche Felder als Facetten verwendet werden können**

Beachten Sie, dass das Schema des Indexes bestimmt, welche Felder als eine Facette verwendet werden. Angenommen, ein Facetable ist, gibt die Abfrage die Facetten von Feldern. Das Feld mit dem facettierung werden bietet unterhalb der Bezeichnung angezeigt. 

Unter jedem Etikett angezeigt werden aus dem Index abgerufen. Beispielsweise wenn *facettenfeld ist,*werden Werte für zusätzliche Filter die Werte für dieses Feld (rote, schwarze und usw.).

Für numerische und DateTime-Werte nur Sie können explizit Werte im facettenfeld (z. B. `facet=Rating,values:1|2|3|4|5`). Eine Werteliste ist für diese Feldtypen vereinfachen die Trennung von facettenergebnisse in zusammenhängenden Bereiche (entweder numerische Werte oder Zeiträume Bereiche) zulässig. 

**Trimmen Sie facettenergebnisse**

Facettenergebnisse sind Dokumente in den Suchergebnissen, die eine Facette entsprechen. Im folgenden Beispiel können in den Suchergebnissen für *Cloud-computing*-254 Elemente *internen Spezifikation* als Inhaltstyp. Elemente sind nicht gegenseitig. Wenn ein Element die Kriterien beider Filter entspricht, wird er in jedem gezählt. Dies ist möglich, wenn facettierung auf `Collection(Edm.String)` Felder, die häufig zum Dokument Tags implementieren.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Im Allgemeinen findet facettenergebnisse sind dauerhaft zu groß, sollten Sie weitere Filter hinzufügen, wie in den vorherigen Abschnitten zu Ihren Benutzern mehrere Optionen zum Einschränken der Suche.

**Elemente im Navigationsbereich Facet einschränken**

Für jedes Feld facettierten in der Navigationsstruktur ist standardmäßig maximal 10 Werte. Diese Standardeinstellung ist sinnvoll für Navigationsstrukturen, da die Werteliste auf eine verwaltbare Größe hält. Sie können das Überschreiben durch Zuweisen eines Werts zu zählen.

- `&facet=city,count:5`Gibt an, dass nur die ersten 5 Städte bewertete Ergebnisse oben als Ergebnis Facet zurückgegeben werden. Suchbegriff "Flughafen" und 32 Spiele gewährt, wenn die Abfrage gibt `&facet=city,count:5`, facettenergebnisse werden nur die ersten fünf eindeutigen Orte die Dokumente in den Suchergebnissen enthalten.

Beachten Sie den Unterschied zwischen facettenergebnisse und Suchergebnisse. Suchergebnisse werden alle Dokumente, die der Abfrage entsprechen. Facet Ergebnisse für jeden facettenwert entspricht. Im Beispiel werden Suchergebnisse Städtenamen enthalten, die nicht in der Liste Klassifikation Facet (in unserem Beispiel 5). Ergebnisse, die durch die facettierte Navigation werden für den Benutzer sichtbar, wenn er Facetten löscht oder andere Facets neben Ort wählt herausgefiltert werden. 

> [AZURE.NOTE] Über `count` wird kann mehrere verwirrend sein. Die folgende Tabelle enthält eine kurze Zusammenfassung der Verwendung des Begriffs in Azure Such-API, Beispielcode und Dokumentation. 

- `@colorFacet.count`<br/>
Präsentation Code sollte Count-Parameter Facette verwendet, um die Anzahl der facettenergebnisse angezeigt werden. In facettenergebnisse gibt Count die Anzahl der Dokumente, die auf die Facetten Begriff oder entsprechen.

- `&facet=City,count:12`<br/>
In einer Abfrage Facets können Count auf einen Wert festgelegt werden.  Der Standardwert ist 10 legen Sie oben oder unten. Festlegen von `count:12` wird oben 12 Facet Ergebnisse Dokumentanzahl übereinstimmt.

- "`@odata.count`"<br/>
In der Abfrageantwort gibt dieser Wert die Anzahl der übereinstimmenden Elemente in den Suchergebnissen. Im Durchschnitt ist größer als die Summe aller Facetten Ergebnisse kombiniert aufgrund der Elemente, die mit den Suchbegriff übereinstimmen, aber haben keine Facet Wert entspricht.


**Ebenen in der facettierten navigation** 

Wie bereits erwähnt, ist keine direkte Unterstützung zum Verschachteln Facetten in einer Hierarchie. Standardmäßig unterstützt facettierten Navigation nur einen Filter. Allerdings existieren Workarounds. Facet hierarchische Struktur in Codieren einer `Collection(Edm.String)` mit einem Eintrag pro Hierarchie zeigen. Gegenstand dieses Artikels ist diese Lösung implementieren Sammlungen [OData Beispiel](http://msdn.microsoft.com/library/ff478141.aspx)erfahren. 

**Überprüfen von Feldern**

Wenn Sie die Liste der Facets nicht vertrauenswürdigen Benutzereingaben dynamisch erstellen, sollten Sie entweder überprüfen, ob der facettierten Felder gültig sind oder die Namen beim Erstellen von URLs mit Escapezeichen `Uri.EscapeDataString()` in .NET oder das Äquivalent in der Plattform Ihrer Wahl.

**Anzahl der Facetten-Ergebnisse**

Wenn eine facettierte Abfrage einen Filter hinzufügen möchten Facet Anweisung beibehalten (z. B. `facet=Rating&$filter=Rating ge 4`). Technisch gesehen Facet = Bewertung ist nicht erforderlich, aber dabei gibt die Anzahl der Facetten-Werte für die Bewertung 4 und höher. Z. B. wenn ein Benutzer auf "4" und die Abfrage einen Filter für größer oder gleich "4 enthält", zählt kehren für jede Bewertung 4 und höher.  

**Sharding folgen auf Facetten**

Unter Umständen können Sie feststellen, dass Facet zählt die Resultsets nicht übereinstimmen (siehe [facettierten Navigation in Azure Search (Forumsbeitrag)](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Anzahl der Facetten können Sharding Architektur ungenau. Jeder Suchindex hat mehrere Splitter und jeweils die Top N-Facets Dokumentanzahl, die dann in einem einzigen Ergebnis kombiniert Berichte. Haben einige Splitter viele übereinstimmende Werte, während andere weniger möglicherweise einige Facetwerte fehlen oder unter gezählt in den Ergebnissen.

Obwohl dies jederzeit ändern kann, sollten Sie dies heute, können Sie umgehen sie von künstlich überhöhte Anzahl die:<number> eine sehr große Zahl vollständige Berichte aus jeder Splitter erzwingen. Wenn der Wert von Count: größer als oder gleich der Anzahl der eindeutigen Werte in dem Feld werden die genauen Ergebnisse garantiert. Wenn jedoch Dokument zählen eine Leistungseinbußen sehr hoch, verwendet daher diese Option überlegt.

<a name="rangefacets"></a>
##<a name="facet-navigation-based-on-a-range-values"></a>Basierend auf einer Bereichswerte Facet navigation

Facettierung für Bereiche ist allgemeine Suche Anwendung erforderlich. Bereiche werden für numerische Daten und DateTime-Werte unterstützt. Erfahren Sie mehr über jeden Ansatz [Dokumente durchsuchen (Azure Such-API)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Azure Suche vereinfacht Bereich erstellen, indem Sie zwei Ansätze zum Berechnen eines Bereichs. Für beide Ansätze erstellt Azure suchen die entsprechenden Bereiche gewährt Ihnen bereitgestellten Eingaben. Beispielsweise wenn Bereichswerte 10 angeben | 20 | 30 es erstellt automatisch Bereiche 0 10 10 bis 20, 20-30. Beispiel-Anwendung entfernt alle Intervalle, die leer sind. 

**Methode 1: Verwenden Sie den Parameter Intervall**<br/>
Um Preis Facetten in Schritten von 10 $ festzulegen, würden Sie Folgendes angeben:`&facet=price,interval:10`


**Methode 2: Verwenden einer Werteliste**<br/>
Für numerische Daten können Sie eine Liste der Werte.  Betrachten Sie Facetten Bereich ListPrice wie folgt gerendert:

  ![][5]

In der **CatalogSearch.cs** eine Werteliste wird angegeben:

    facet=listPrice,values:10|25|100|500|1000|2500

Jeder Bereich baut 0 als Ausgangspunkt, einen Wert aus der Liste als Endpunkt und getrimmt an vorherigen diskrete Intervallen erstellen. Azure Suche wird als Teil des facettierten Navigation dazu. Sie müssen keinen Code für jedes Intervall Strukturierung schreiben.

### <a name="build-a-filter-for-facet-ranges"></a>Erstellen eines Filters für Facet Bereiche ###

Filtern basierend auf einem vom Benutzer ausgewählten Dokumente können Sie die `"ge"` und `"lt"` Filtern Operatoren in einem Ausdruck zwei Teilen, die die Endpunkte des Bereichs definiert. Z. B. wenn der Benutzer den Bereich 10-25 Filter wäre `$filter=listPrice ge 10 and listPrice lt 25`.

In diesem Beispiel wird mit der Filterausdruck **Servicelevel** und **PriceTo** Parameter Endpunkte festgelegt. **BuildFilter** Methode in **CatalogSearch.cs** enthält den Filterausdruck, der Dokumente in einem Bereich gibt.

  ![][6]

<a name="geofacets"></a> 
##<a name="filtered-navigation-based-on-geopoints"></a>Gefilterte Navigation basierend auf GeoPoints

Die gemeinsamen an filtert, helfen Ihnen einen Shop Restaurant oder Ziel der Nähe in das aktuelle Verzeichnis. Während diese Art von Filter facettierten Navigation aussieht, ist eigentlich nur ein Filter. Wir erwähnen hier für diejenigen Implementierung Beratung, insbesondere Entwurfsproblem speziell suchen.

Es gibt zwei Geodaten Funktionen in Azure Search, **geo.distance** und **geo.intersects**.

- **Geo.distance** -Funktion gibt die Entfernung in Kilometern zwischen zwei Punkten, einem Feld und einer eine Konstante übergeben als Teil des Filters. 

- Die **geo.intersects** -Funktion gibt true, wenn ein bestimmter Punkt innerhalb eines Polygons angegebenen ist, wobei der Punkt ein und das Vieleck als Konstante Liste von Koordinaten, die als Teil des Filters angegeben.

Filter-Beispiele finden Sie in der [Syntax OData (Azure suchen)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>
##<a name="try-it-out"></a>Probieren Sie es aus

Azure Suche Adventure Works Demo auf Codeplex enthält Beispiele in diesem Artikel verwiesen wird. Arbeiten mit Suchergebnissen sehen Sie die URL geändert Abfrage erstellen. Diese Anwendung zufällig Facets URI angefügt einzeln auswählen.

1.  Konfigurieren Sie Beispiel-Anwendung die Dienst-URL und API-Schlüssel. 

    Beachten Sie das Schema, das in der Datei Program.cs des Projekts CatalogIndexer definiert ist. Es gibt Facetable Felder für Farbe, Listenpreis, Größe, Gewicht, Kategoriename und ModelName.  Nur wenige (Farbe, Listenpreis, Kategoriename) sind eigentlich facettierten Navigation implementiert.

3.  Führen Sie die Anwendung. 

    Zunächst wird nur das Suchfeld angezeigt. Sie können klicken Suche sofort alle Ergebnisse oder einen Suchbegriff eingeben.

    ![][7]
 
4.  Geben Sie einen Suchbegriff wie Fahrrad, und klicken Sie auf **Suchen**. Die Abfrage wird schnell ausgeführt.
 
    Facettierte Navigationsstruktur wird auch mit den Suchergebnissen zurückgegeben.  In der URL gibt Facets für Farben, Kategorien und Preise null. In der Suchergebnisseite enthält facettierten Navigationsstruktur zählt jede Facette Ergebnis.

     ![][8]
 
5.  Klicken Sie auf Farbe, Kategorie und Preis. Facets sind bei der ersten Suche null, aber wie sie Werte, werden die Suchergebnisse Elemente nicht mehr entsprechen abgeschnitten. Beachten Sie, dass der URI ändert Ihrer Abfrage nimmt.

    ![][9]
 
6.  Facettierte Abfrage deaktivieren, damit andere Abfrage Verhaltensweisen versuchen können, klicken Sie auf **AdventureWorks Katalog** am oberen Rand der Seite.

    ![][10]
 
<a name="nextstep"></a>
##<a name="next-step"></a>Nächstes

Testen Sie Ihr Wissen können Sie ein facettenfeld *ModelName*hinzufügen. Der Index ist bereits für dieses Facet festgelegt, sodass keine Änderungen auf den Index erforderlich sind. Aber Sie ändern den HTML-Code ein neues Facet für Modelle, und Abfrage-Konstruktor facettenfeld hinzufügen müssen.

Weitere Einblicke in Entwurfsprinzipien für facettierte Navigation empfehlen wir die folgenden Links:

- [Entwerfen der facettierten suchen](http://www.uie.com/articles/faceted_search/)
- [Entwurfsmuster: Facettierten Navigation](http://alistapart.com/article/design-patterns-faceted-navigation)

Sie können auch [Azure Search Deep Dive](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410)ansehen. 45:25 ist es eine Demo zum Implementieren von Facets.

<!--Anchors-->
[How to build it]: #howtobuildit
[Build the presentation layer]: #presentationlayer
[Build the index]: #buildindex
[Check for data quality]: #checkdata
[Build the query]: #buildquery
[Tips on how to control faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/Facet-1-slide.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

 