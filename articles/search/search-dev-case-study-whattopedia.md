<properties 
    pageTitle="Azure Suche Developer-Fallstudie: Wie WhatToPedia ein Infomedia Portal auf Microsoft Azure erstellt | Microsoft Azure | Gehostete Cloud-Suchdienst" 
    description="Informationen Sie zum Erstellen einer Informationen Portal und Meta-Suchmaschine Azure Suche, einen Suchdienst Cloud gehostet für Entwickler." 
    services="search, sql-database,  storage, web-sites" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="jhubbard"/>

<tags 
    ms.service="search" 
    ms.devlang="NA" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="search" 
    ms.date="08/29/2016" 
    ms.author="heidist"/>

# <a name="azure-search-developer-case-study"></a>Azure Suche Developer-Fallstudie

## <a name="how-whattopediacomhttpwhattopediacom-built-an-infomedia-portal-on-microsoft-azure"></a>Wie [WhatToPedia.com](http://whattopedia.com/) ein Infomedia Portal auf Microsoft Azure erstellt

 ![][6]  &nbsp;&nbsp;&nbsp;  <font size="9">Die Idee</font> 


Unsere soll Informationsportal erstellen Käufer mit durch eine hohe Relevanz, Bereich Suchen Erfahrung, ähnlich wie Portale Übereinstimmung Touristen, Hotels, Restaurants und Unterhaltung in Neuland Reisen verbinden. 

Das Portal, das wir stellen, eine außergewöhnlich hohe Qualität Suchfunktionalität Einzelhändler Daten in einem Markt liefern, bietet Kunden suchen, Speicher und andere Ausstattung Einzelhändler. Wir werden die Suchmaschine mit einem ursprünglichen Dataset Startwert, aber tiefer Wert im Laufe der Zeit mit Hilfe der Einzelhändler Abonnenten über ihr Unternehmen Informationen erstellt werden. Aktionen neue Waren verwendeten Marken, interne Specialty Services-– alle sind Beispiele für Daten, die unsere Website Wert hinzufügt. Diese Daten selbst gemeldet und integriert Corpus Suche nach Einzelhändler als Abonnent anmeldet. Werbung und Abonnementmodell erhalten die Einnahmen für unser neues Unternehmen.

Suche werden Interaktionsmodell vorherrschende Benutzer auf eine reine Cloud. Zwecken und niedrige Kosten werden fast alles, von Portalfunktionalität zur Versionskontrolle, über einen online-Dienst. Über eine Suchmaschine, die Funktionen bereitstellt, müssen wir, standardmäßig eine Suchmaschine kann schnell erstellt, ohne zum Erstellen und Verwalten einer Suchmaschine selbst.

## <a name="what-we-built"></a>Wir erstellten

WhatToPedia ist ein Infomedia Startunternehmen. Wir erstellten [WhatToPedia.com](http://whattopedia.com/) – - in Test-Markt in Nordeuropa Go live Datum 2. Februar 2015. Unsere Kundenbasis ist primär-stationären Geschäfte, die eine kostengünstige Online-Präsenz, die einfach zu verwalten.

Unsere Aufgabe ist, gewinnen Kunden durch eine hervorragende Online-Umgebung steigert Ergebnisse nach Stadt oder Umgebung, Marken Namen oder speichern. Käufer anzuziehen hat einen Welleneffekt motivieren Einzelhändler unsere Portalwebsite abonnieren. Abonnements sind kostenpflichtig, zu einem erschwinglichen Preis.

 ![][7] 

Nach der Anmeldung für ein Abonnement übernimmt ein Einzelhändler ihre vorhandenen Profils (ursprünglich von uns von Daten), dazu Werbeaktionen, vorgestellte Marken oder Ankündigungen aktualisieren. Interne Funktionen wie Sprachen, Währungen akzeptiert Steuerfreier Einkauf kann die Ausstattung suchen Kunden besser gewinnen selbst gemeldet.

## <a name="who-we-are"></a>Wer wir sind

Mein Name ist Thomas Segato (Microsoft Consulting) und Jesper Boelling, leitender Entwickler bei WhatToPedia der Lösung werden gearbeitet. 

WhatToPedia ist ein Start Test marketing Portal Neugeschäfts in Schwedens, wo 60.000 Einzelhändler-stationären KMU (kleine und mittelständische Unternehmen) sind. Da wir wissen, dass eine Person in Europa mehrere Sprachen und mehrere Währungen, erstellen wir Lösungen, die mehrsprachige Käufer aufnehmen. Wir benötigten und gefunden, eine Suchmaschine, die mehrsprachigen Anforderungen in Azure Suche unterstützt.

Azure Suche war ein Spiel-Wechsler für unser Projekt. Vor der Verfügbarkeit der Azure suchen wir viel Energie durch Schleifen erstellen eigene Suchmaschine ausgegeben. Mit Azure Search Onlinedienst unsere Lösung die größten technische und administrative Hürde entfernt, bedeutete die schneller, und eine stabilere Umgebung.  

## <a name="how-we-did-it"></a>Wie haben wir es

Unsere Vision ist eine vollständige Infrastruktur basierend auf Cloud-Dienste. Microsoft wurde als strategische Plattform gewählt, weil es der Anbieter konnte die erforderlichen angebotenen Services (für Zusammenarbeit und Entwicklung), Skalierung nach Bedarf und erschwinglichen Preisen.
 
### <a name="high-level-components"></a>Allgemeine Komponenten

Es erstellt ein Unternehmen nicht nur eine Website. Die gesamte unterstützen erforderlich eine Vielzahl von Tools und Applikationen. Wir angenommen, Visual Studio und Visual Studio Team Services Development Team Foundation Service (TFS) Online Versionskontrolle und Scrum Management Office 365 für Kommunikation und Zusammenarbeit und natürlich Microsoft Azure für Site-Vorgänge und Speicher. Mit Visual Studio bereitgestellten IDE direkte Bereitstellung in Azure, Integration in TFS Online Bereitstellen zusätzlicher Produktivität.

Das Diagramm zeigt die allgemeinen Komponenten WhatToPedia-Infrastruktur verwendet.

   ![][8]

### <a name="how-we-use-microsoft-azure"></a>Verwendung Microsoft Azure

Grünen Felder im obigen Diagramm betrachten, sehen Sie, dass WhatToPedia-Lösung basiert auf:

- [Azure Suche](https://azure.microsoft.com/services/search/)
- [Azure Websites mit MVC 4](https://azure.microsoft.com/services/websites/)
- [Azure Webaufträge für geplante tasks](../app-service-web/websites-webjobs-resources.md)
- [SQL Azure-Datenbank](https://azure.microsoft.com/services/sql-database/)
- [Azure BLOB-Speicher](https://azure.microsoft.com/services/storage/)
- [SendGrid e-Mail-Zustellung](https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/)

Zentrale Lösung ist und suchen. Der Datenfluss vom Händler Anbieter für den Endkunden ist unten dargestellt:

  ![][9]

Primärer Datenspeicher ist der Händler und Buchhaltungsdaten in Azure SQL-Datenbank. Dabei die ursprüngliche Dataset sowie Einzelhändler Daten mit der Zeit hinzugefügt. Wir verwenden eine Azure Webauftrags Updates aus SQL-Datenbank suchen Korpus in Azure Suche buchen.

### <a name="presentation-layer"></a>Darstellungsschicht

Das Portal ist eine Azure-Website in MVC 4 und [Twitter Bootstrap](http://en.wikipedia.org/wiki/Bootstrap_%28front-end_framework%29)implementiert. Es bietet eine äußerst saubere HTML als formularbasierte Entwicklung ASP.NET MVC ausgewählt. Um apps für mehrere Geräte erstellen und verwalten mehrere mobile Plattformen, wurde Twitter Bootstrap gewählt, um alle Geräte und Plattformen unterstützen.

### <a name="authentication-permissions-and-sensitive-data"></a>Authentifizierung, Berechtigungen und vertrauliche Daten

Kunden Durchsuchen der Website anonym. So gibt es keine Anmeldung an Käufer und speichern wir Consumer Daten. 

Einzelhändler sind anders. Hier speichern wir öffentliche Profilinformationen Abrechnungsinformationen und Medien-Content auf der Website verfügbar gemacht werden sollen. Alle Händler, die Website abonniert, erhalten ein Benutzername zur Authentifizierung des Benutzers vor Updates auf dem Abonnenten Profil verwendet.  Sicher alle Abonnentendaten in Azure SQL-Datenbank und Azure BLOB-Speicher gespeichert.
Wir haben basierend auf .NET formularbasierte Authentifizierungsmodell. Wir haben dies Einfachheit; Wir benötigen nicht, Rollen, UI-Unterstützung und andere überflüssige Features von anderen Ansätzen. 

Um sicherzustellen, dass Einzelhändler nur die Daten angezeigt, zu der sie gehört, erstellt Händler-ID für jeden Einzelhändler, die anschließend für alle dient Lese- und Schreibvorgänge mit Einzelhändler Daten. Bei diesem Ansatz fanden wir, dass wir nicht Einzelhändlern Datenbankberechtigungen erteilen. Alle Einzelhändler interagieren mit dem System eine einzelne Datenbankrolle mit die Händler-ID als unsere Daten Isolation Technik.

Da unser Unternehmen geht können downstream Effekte (Kundenstamm Einzelhändler Anreiz und Abonnieren), wir verarbeiten von Einkäufen über das Web Linie zeichnen. So finden Sie nicht Warenkorb auf unserer Website, die unseren Anforderungen zu vereinfachen. 

Eine weitere Vereinfachung wir beschäftigt war Abrechnung und Konten zu zahlenden operativen überlassen. Durch routing Debitorenzahlungsinformationen direkt an einen Drittanbieter ([SveaWebPay](http://www.sveawebpay.se/)) wir Risiken der Speicherung und dem Schutz sensibler Daten in unseren Datenspeicher zugeordnet. 

### <a name="search-engine"></a>Suchmaschine

Der Kern unserer Lösung ist die Grundlage der Azure-Suchdienst. Zunächst wir eine benutzerdefinierte Suchmaschine erstellt aber dabei uns die Komplexität bewusst war sehr hoch und aufgefordert, die uns andere Alternativen. 

Grundlegende Funktionen, die für uns am wichtigsten waren:

- Filter
- Facettierte navigation
- Steigerung der Ergebnisse
- Paging AJAX

Internet-Suche haben wir das folgende Video wir Azure Suche versuchen Inspirierte: [Deep Dive auf der TechEd Europe](http://channel9.msdn.com/events/TechEd/Europe/2014/DBI-B410) 

Nach Videos, waren wir wir sahen Grundlage Prototyp erstellen. Da wir bereits ein Datenmodell in MVC, wurde der Prototyp einfach Daten durchsucht werden Begriffe, und wir haben bereits Vorgaben für wie wir wollten, Facetten, Sortieren und Filtern der Daten. 

Dies ist wie wir den Prototyp erstellt.

**Konfigurieren von Azure-Suchdienst**

1. Anmeldung bei Azure-Verwaltungsportal und unsere Abonnement den Suchdienst hinzugefügt. Die freigegebene Version (mit unserem Abonnement kostenlos) verwendet.
2. Erstellen eines Indexes. Für den Prototyp verwendet Portal UI die Felder definieren und erstellen die Punktzahl Profile. Unsere Bewertungsprofil basiert auf Positionsdaten: Land | Stadt | Adresse (siehe: Punktzahl Profile).
3. Kopieren Sie die URL und Admin API-Schlüssel zu unserem Konfigurationsdateien. Dieser Schlüssel ist auf der Suche-Seite des Portals und dient zum Dienst authentifizieren.
    
**Entwickeln eines Auftrags Search Indexer – Windows-Konsole**

1. Gelesen Sie alle Händler aus Datenbank.
2. Aufrufen der API Suche Azure hochladen Fachhändler nacheinander (siehe: http://msdn.microsoft.com/library/azure/dn798930.aspx).
3. Festlegen einer Eigenschaft in Datenbank Händler für inkrementelle Indizierung indiziert. Wir haben dies durch Hinzufügen eines Felds "Indexer", in dem den Indexstatus eines Profils (indizierte oder nicht) gespeichert. 

Siehe Anhang für den Codeausschnitt, der Indexer-Projekt erstellt.

**Entwickeln einer Suche Web-Portal – MVC**

1. Azure-Suchdienst Suche alle Dokumente zu aufrufen (siehe: http://msdn.microsoft.com/library/azure/dn798927.aspx)
2. Nach der Suche Service-Antwort (mit json.net http://james.newtonking.com/json) extrahieren
   - Ergebnisse
   - Facetten
   - Ergebnis zählt
   - Entwickeln einer Benutzeroberfläche Suchergebnisse, Facets und zählt (wir bereits diese).

Dies ist der Code zu den Ergebnissen von Azure Suche verwendeten:

    string requestUrl = 
    string.Format("https://{0}.search.windows.net/indexes/profiles/docs?searchMode=all&$count=true&search={1}&facet=city,count:20&facet=category&$top=10&$skip={2}&api-version=2014-07-31-Preview{3}", Config.SearchServiceName, EscapeODataString(q), skip, filter);
      var response = client.GetAsync(requestUrl).Result; //  + Uri.EscapeDataString("hennes")
      response.EnsureSuccessStatusCode();
     dynamic json = JsonConvert.DeserializeObject(response.Content.ReadAsStringAsync().Result);

**Bis Position**

Die wichtigste Anforderung im Prototyp überprüfen enthalten wahrscheinlich ein Suchschlüsselwort Speicherort der Abfrage hinzufügen. Es ist für unser Portal, das Benutzer einen Ortsnamen in die Suche ein, Abfrage Fachhändler in der Stadt höher Beschreibung Ort Schlüsselwort unter Händlern bewerten würde. Für diese Anforderung verwendet ein Bewertungsprofil Ort andere Felder höher einstufen.

**Unterstützung mehrerer Sprachen**

Wir mussten Suchergebnisse im richtigen Sprachen anzuzeigen und eine Option für die gleichen Ergebnisse in verschiedenen Sprachen suchen. Die beiden Seiten dieses Problems sind: 

- Suchen nach Wörtern in mehreren Sprachen
- Suchergebnisse in der richtigen Sprache anzeigen

Wir gelöst Präsentation Teil, indem Sie ein Dokument für jede Sprache lokalisierten Text und eine Eigenschaft mit der Sprache. Wenn ein Benutzer einen Suchbegriff eingibt wir Benutzer `$filter` Ausdrücke der Sprache den Benutzer Filtern ausgewählt wurde.

Jedes Dokument verfügt über verborgene Eigenschaft namens "Orte" des Auflistungstyps erstellt. Diese Eigenschaft speichert Ortsnamen in allen Sprachen der Benutzer in mehreren Sprachen suchen.

###<a name="data-storage"></a>Datenspeicher

Alle Daten (Profile, Abonnements und Accounting) ist in SQL-Datenbank gespeichert. Alle Mediendateien werden in Azure BLOB-Speicher, einschließlich Bilder und Videos seitens der Einzelhändler gespeichert. Separate BLOB-Speicher isoliert Effekte hochladen; Dateien nicht mit der Website gemischte müssen wir nicht die Site neu erstellen, wenn wir Dateien hinzufügen.

Ein wichtiger Vorteil unserer Speicherentwurf ist, dass mehrere Entwickler einen einzelne Entwicklungsspeicher freigeben können. Eine Voraussetzung für das WhatToPedia-Projekt wurde eine innerhalb von 15 Minuten, einschließlich Händler Daten, Bilder und Videos erstellen. Immer die neuesten Daten von TFS Online ein SQL-Skript und den Importvorgang ausführen, kann eine vollständige Umgebung in kürzester Zeit überhaupt Stand sein. Diese Vorgehensweise verbessert auch den staging-Prozess.

###<a name="webjobs"></a>Webaufträge

Wir verwenden Azure Webaufträge zum Aktualisieren von Daten in den Index. Erstellen eines Auftrags Search Indexer, war Indizierung Teil sehr einfach unsere Lösung integrieren. Die einzige codeänderung wir war das Indexer-Projekt aufnehmen hinzufügen ein `Indexed` in unser Datenmodell Indexstatus an. Wenn ein neues Profil hinzugefügt oder aktualisiert, wird die `Indexed` Feld auf False festgelegt ist. Dasselbe gilt, wenn der Einzelhändler seine Profildaten über das Portal geändert.  

Der Auftrag sieht für alle Zeilen mit `Indexed` auf False festgelegt. Wenn die Zeile gefunden wird, wird der Beleg gebucht Azure Suche und die `Indexed` Feld true. Wir mussten planen hinzufügen und Daten aktualisieren, da der Azure-Suchdienst tatsächlich dies übernimmt. Wenn Sie ein Dokument, das bereits vorhanden ist hinzufügen, geschieht der Dienst automatisch eine Aktualisierung.

Alle Aufträge des Web Console Applications entstanden, die in Azure Websites als ZIP-Dateien hochgeladen, entpackt und dann eingeplant werden können.

Der Auftrag soll alle 5 Minuten als geplante Webtask auszuführen. Der Dienst ungefähr drei 3.000 Dokumente hochladen Minuten war in unsere berechnet. 

> [AZURE.NOTE] Es ist ein Prototyp Indexer-Feature, das vor kurzem in Azure Search eingeführt wurde. Dieses Feature wurde zu spät für uns in der ersten Version verwenden, aber es scheint lösen das gleiche Problem haben wir unsere Indexer, der Datenladevorgänge automatisieren.


###<a name="backup-strategy"></a>Backup-Strategie

Es entwickelt eine mehrstufige backup-Strategie von Szenarien aus schwerwiegenden Fehler auf Recovery einer einzelnen Transaktion wiederherstellen. Zu schützende Ressourcen gehören drei Arten von Daten (Website, Abonnentendaten und Mediendateien). 

Erstens bekanntlich Quellcode Website in TFS Online halten, fällt die Website, wir es neu konfigurieren können TFS veröffentlichen. 

Abonnentendaten in Azure SQL-Datenbank ist die empfindlichste Anlage. Wir wieder dies mit der integrierten Funktion (siehe [Azure SQL-Datenbank sichern und Wiederherstellen](http://msdn.microsoft.com/library/azure/jj650016.aspx)). Der Sicherungszeitplan ist vollständige Sicherung der Datenbank einmal wöchentlich sichern differenzieller Datenbanken täglich und Transaktionsprotokollen 5 Minuten.  Die Größe der Daten ist diese Lösung für unsere sofortigen und geplanten Datenmengen.

Drittens wird in Azure BLOB-Speicher Bild-und Videodateien gespeichert. Wir prüfen noch, ultimativen Sicherungsplan für diese Daten Cloudberry Explorer für Azure als mögliche Lösung erwägen. Jetzt verwenden wir einen Webauftrag Bilder und Videos an einem anderen Speicherort kopieren.

##<a name="what-we-learned"></a>Gelernte

Da wir bereits Daten, war einfach zu Proof of Concept. Stunden hatten wir Prototypen mit Leistungsindikatoren paging Rank Profile und Suchergebnisse. Die Suchergebnisse so genau waren, wir beschlossen, einige Kunden präsentiert Filter entfernen. 

Die größte Überraschung für uns war wie schnell wir Azure Search lernen, und wie viel wir zurück. Haben wir Proof of Concept Stunden (siehe Anmerkung unten) 3 Codezeilen in der front-End-Anwendung (plus eine neue Webauftrags) 500 Codezeilen ersetzen und bessere Ergebnisse erzielen. 

Unser Code implementiert zuvor Paging, Zahlen und andere Verhaltensweisen, die zu durchsuchen sind. Azure Suche enthalten die Ergebnisse wieder erhalten Treffer Facets paging von Daten zählt – alles benötigt, und haben uns angeben. Sie enthalten auch steigern und integrierte facettierten Navigation, die wir in unserer ursprünglichen Lösung.

Die größte Herausforderung bei der Implementierung ist eine Vorabversion und Informationen und Erfahrungen war. Sobald wir ein paar Punkte verbunden, fanden wir, dass Azure-Suchdienst mit einfach durch die REST-API und JSON-Datenformat. Wir könnten rufen Rahmen direkt offen Quelle Plugins wie JQuery-JSON.Net und können wir schnell experimentieren und debugging Tools wie Fiddler. 

> [AZURE.NOTE] Abgesehen von Daten vorbereitet geholfen, die uns den Prototyp bereits verstanden wie Suche Technologie funktioniert, macht uns produktiver und mehr integrierten Funktionen zu schätzen. Ggf. Hochfahren auf Suche Abfrage facettierten Navigation, Filter usw. Prototypen länger erwarten. 

###<a name="controlling-facets-in-the-search-presentation-page"></a>Aspekte der Suchseite Präsentation steuern

Unsere Erkenntnisse während der Prüfung des Konzepts gehörte Aspekte sorgfältig im Voraus planen. Nach dem Laden einer große Datenmenge in der Projektmappe, sahen wir, dass des Volumens der Facetten zu hoch, um den Benutzern zu präsentieren. 

Wir lösen der Zählparameter Facet einschränken. Der Count-Parameter legt eine Obergrenze für die Anzahl der Facetten an den Benutzer zurückgegeben. Ein Link, der eine des Count-Parameters Diskussion finden Sie [hier](search-faceted-navigation.md).

###<a name="webjobs-for-scheduling-tasks"></a>Webaufträge Tasks planen

Azure Suche nicht nur Überraschung für uns. Wir entdeckten Webaufträge unsere Datenladevorgänge Azure Suche automatisieren überlegen unserer vorherigen Ansatz war die Verwendung einer dedizierten VM geplante Tasks zur Aktualisierung des Suchindexes mit Windows-Taskplaner brachte. Webaufträge war einfacher konfigurieren und Debuggen und viel billiger als dedizierte VM bezahlen.

###<a name="azure-blob-storage-explorer-for-updating-images"></a>Azure BLOB-Speicher-Explorer zum Aktualisieren von Abbildern

Wir fanden, dass [Azure BLOB-Speicher-Explorer](https://azurestorageexplorer.codeplex.com/) (verfügbar auf Codeplex) sehr hilfreich bei der Verwaltung von Bild- und Videodateien Updates auf der Website verwenden. Wir können sie als Entwicklertool Bilder und Videos, die Teil unserer Hauptsite manuell aktualisieren. Wir fanden es flexibler ändert das Portal bereitstellen und vollständige Testdurchläufe beseitigt, wenn wir ein Bild aktualisieren. 

##<a name="a-few-final-words"></a>Letzte Wort

Stellte große WhatToPedia möchten uns ihre Geschichte!  

Wir hoffen, dass Sie diese Fallstudie nützlich. Wenn Sie Azure Suche weiter, empfehle ich einige Ressourcen Sie beschleunigen:

- [MSDN-Forum für Azure suchen](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch)
- [StackOverflow hat auch ein tag](http://stackoverflow.com/questions/tagged/azure-search)
- [Dokumentationsseite auf Azure.com](https://azure.microsoft.com/documentation/services/search/)
- [Azure Dokumentation auf MSDN](http://msdn.microsoft.com/library/azure/dn798933.aspx)


##<a name="appendix-search-indexer-webjob"></a>Anhang: Search Indexer Webauftrag

Der folgende Code erstellt den Indexer gemäß Abschnitt auf den Prototyp.

        static void Main(string[] args)
        {
            int success = 0;
            int errors = 0;

            Log.Write("Starting job","", System.Diagnostics.TraceLevel.Info);

            var serviceName = ConfigurationManager.AppSettings["SearchServiceName"];
            var serviceKey = ConfigurationManager.AppSettings["SearchServiceKey"];

            HttpClient client = new HttpClient();
            client.DefaultRequestHeaders.Add("api-key", serviceKey);

            var db = new DB(Config.ConectionString);

            var recreateIndex = false;
            Boolean.TryParse(ConfigurationManager.AppSettings["RecreateIndex"], out recreateIndex);

            if(recreateIndex)
            {
                Log.Write("Recreating index and set all to no index", "", System.Diagnostics.TraceLevel.Info);
                db.SetAllToNotIndexed();
                RecreateIndex(serviceName, client);
            }            
            
            var profiles = db.Profiles.Where(p=>!p.Indexed).ToList();

            Log.Write(string.Format("Indexing {0} profiles",profiles.Count),"", System.Diagnostics.TraceLevel.Info);

            var cities = db.Cities.ToList();
            var categories = db.Tags.Where(p=>p.ParentId==null).ToList();            

            foreach (var profile in profiles)
            {
                Log.Write(string.Format("Indexing profile {0}", profile.Name),"",profile.ProfileId,0,System.Diagnostics.TraceLevel.Verbose);

                try
                {
                    var city = cities.Where(p => p.CityId == profile.CityId);
                    var category = categories.Where(p => p.TagId == profile.CategoryId);

                    var cityse = city.Where(p => p.Lang == "se").FirstOrDefault();
                    var cityen = city.Where(p => p.Lang == "en").FirstOrDefault();
                    var categoryse = category.Where(p => p.Lang == "se").FirstOrDefault();
                    var categoryen = category.Where(p => p.Lang == "en").FirstOrDefault();

                    var citysename = cityse == null ? "" : cityse.Name;
                    var cityenname = cityen == null ? "" : cityen.Name;
                    var categorysename = categoryse == null ? "" : categoryse.Name;
                    var categoryenname = categoryen == null ? "" : categoryen.Name;

                    var tags = db.GetTagsFromProfile(profile.ProfileId);

                    var batch = new
                    {
                        value = new[] 
                    { 
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_en",
                            profileid = profile.ProfileId.ToString(),
                            city = cityenname,
                            category = categoryenname,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "en",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        },
                        new 
                        { 
                            id = profile.ProfileId.ToString()+"_se",
                            profileid = profile.ProfileId.ToString(),
                            city = citysename,
                            category = categorysename,
                            address = profile.Adress1,
                            email = profile.Email,
                            name = profile.Name,
                            lang = "se",
                            brands = profile.Brands,
                            descen=profile.DescEn,
                            descse=profile.DescSe,
                            orgnumber=profile.OrgNumber,
                            phone=profile.Phone,
                            zip=profile.Zip,
                            cities = city.Select(p=>p.Name).ToArray(),
                            categories = category.Select(p=>p.Name).ToArray(),
                            cityid = profile.CityId.ToString(),
                            tags=tags.ToArray()
                        }
                    },
                    };

                    var response = client.PostAsync("https://" + serviceName + ".search.windows.net/indexes/profiles/docs/index?api-version=2014-10-20-Preview", new StringContent(JsonConvert.SerializeObject(batch), Encoding.UTF8, "application/json")).Result;
                    response.EnsureSuccessStatusCode();

                    db.Entry(profile).State = System.Data.Entity.EntityState.Modified;
                    profile.Indexed = true;
                    db.SaveChanges();
                    success++;
                }
                catch(Exception ex)
                {
                    Log.Write("Error indexing profile", ex.Message, profile.ProfileId, 0, System.Diagnostics.TraceLevel.Verbose);
                    errors++;
                }
            }
            if(errors > 0)
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Error);
            }
            else
            {
                Log.Write(string.Format("Job ended success ({0}), errors ({1})", success, errors), "", System.Diagnostics.TraceLevel.Info);
            }
            
        }

        static void RecreateIndex( string ServiceName, HttpClient client)
        {
            var index = new
            {
                name = "profiles",
                fields = new[] 
                { 
                    new { name = "id",              type = "Edm.String",         key = true,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "profileid",       type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "cityid",          type = "Edm.String",         key = false,  searchable = false, filterable = false, sortable = false, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "city",            type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = true,  facetable = true, retrievable = true,  suggestions = true  },
                    new { name = "category",        type = "Edm.String",         key = false, searchable = true,  filterable = true, sortable = false, facetable = true, retrievable = true,  suggestions = false  },
                    new { name = "address",         type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "email",           type = "Edm.String",         key = false, searchable = true,  filterable = false, sortable = true, facetable = false, retrievable = true,  suggestions = false },
                    new { name = "name",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true, suggestions = true },
                    new { name = "lang",            type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = false,  facetable = false,  retrievable = true, suggestions = false },
                    new { name = "brands",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descen",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "descse",          type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "orgnumber",       type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "phone",           type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "zip",             type = "Edm.String",         key = false, searchable = true,  filterable = true,  sortable = true,  facetable = false,  retrievable = true,  suggestions = false },
                    new { name = "cities",          type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                   new { name = "categories",      type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false },
                    new { name = "tags",            type = "Collection(Edm.String)",         key = false, searchable = true,  filterable = false,  sortable = false,  facetable = false,  retrievable = false, suggestions = false }
                    
                }
            };

            var url = "https://" + ServiceName + ".search.windows.net/indexes/?api-version=2014-10-20-Preview";

            var deleteUrl = "https://" + ServiceName + ".search.windows.net/indexes/profiles?api-version=2014-10-20-Preview";

            try
            {
                var deleteResponseIndex = client.DeleteAsync(deleteUrl).Result;
                deleteResponseIndex.EnsureSuccessStatusCode();
            }
            catch (Exception ex)
            {

            }

            var responseIndex = client.PostAsync(url, new StringContent(JsonConvert.SerializeObject(index), Encoding.UTF8, "application/json")).Result;
            responseIndex.EnsureSuccessStatusCode();            
          


<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Subheading 4]: #subheading-4
[Subheading 5]: #subheading-5
[Next steps]: #next-steps

<!--Image references-->
[6]: ./media/search-dev-case-study-whattopedia/lightbulb.png
[7]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Search-Bageri.png
[8]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Stack.png
[9]: ./media/search-dev-case-study-whattopedia/WhatToPedia-Archicture.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: ../virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
 
