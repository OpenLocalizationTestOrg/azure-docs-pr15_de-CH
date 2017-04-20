<properties 
    pageTitle="Wie bewerten Sie Profile in Azure suchen | Microsoft Azure | Gehostete Cloud-Suchdienst" 
    description="Optimieren Sie Suche durch Punkte Profile in Azure Suche eine gehostete Cloud-Suchdienst auf Microsoft Azure ranking." 
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

# <a name="how-to-use-scoring-profiles-in-azure-search"></a>Verwendung von scoring Profile in Azure suchen

Punktzahl Profile sind ein Feature von Microsoft Azure Search, die Berechnung von Ergebnissen Suche anpassen Platzierung der Elemente in einer Suchergebnisliste beeinflussen. Sie können sich vorstellen Bewertung Profile so Modell Relevanz durch Steigerung der Elemente, die vordefinierte Kriterien erfüllen. Angenommen Sie, dass die Anwendung ein Hotel Reservierung Site. Durch die Förderung der `location` Feld sucht, die einen Begriff wie Köln führen höhere Werte für Elemente enthalten, die Seattle in den `location` Feld. Beachten Sie, dass mehrere Bewertungsprofil oder none, können ist standardmäßig Bewertung für Ihre Anwendung.

Experimentieren mit scoring helfen, können Sie eine beispielanwendung herunterladen, die Punktzahl Profile verwendet, um die Rangfolge von Suchergebnissen zu ändern. Das Beispiel ist eine Konsolenanwendung – vielleicht nicht sehr realistisch für reale Anwendungsentwicklung – aber dennoch als Lernmittel. 

Beispielanwendung demonstriert Punktzahl Verhaltensweisen mit fiktiven Daten bezeichnet die `musicstoreindex`. Die Einfachheit der Beispiel-app erleichtert das Bewertungssystem Profile und Abfragen ändern und finden die sofortige Effekte Rangfolge, wenn das Programm ausgeführt wird.

<a id="sub-1"></a>
## <a name="prerequisites"></a>Erforderliche Komponenten

Beispiel-Anwendung wird mit Visual Studio 2013 in C# geschrieben. Versuchen Sie die kostenlose [Visual Studio 2013 Express Edition](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) haben Sie bereits eine Kopie der Visual Studio.

Sie benötigen ein Azure-Abonnement und einem Azure-Suchdienst für dieses Lernprogramm erforderlich. Hilfe beim Einrichten des Dienstes finden Sie unter [Search-Dienst im Portal erstellen](search-create-service-portal.md) .

[AZURE. ENTHALTEN [benötigen Sie ein Azure-Konto zum Bearbeiten dieses Lernprogramms:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## <a name="download-the-sample-application"></a>Beispielanwendung herunterladen

Zur [Azure Suche bewerten Profile Demo](https://azuresearchscoringprofiles.codeplex.com/) auf Codeplex in diesem Lernprogramm beschriebenen beispielanwendung herunterladen.

Klicken Sie auf die Registerkarte Quellcode eine Zip-Datei der Lösung zu **Laden** . 

 ![][12]

<a id="sub-3"></a>
## <a name="edit-appconfig"></a>App.config bearbeiten

1. Nach dem Extrahieren der Dateien öffnen Sie die Projektmappe in Visual Studio die Konfigurationsdatei bearbeiten.
1. Doppelklicken Sie im Projektmappen-Explorer auf **app.config**. Diese Datei legt den Dienstendpunkt und `api-key` zur Authentifizierung der Anforderung verwendet. Sie erhalten diese Werte aus dem Verwaltungsportal.
1. [Azure-Portal](https://portal.azure.com)anmelden.
1. Service-Dashboard Azure Suche wechseln
1. Um die Dienst-URL kopieren klicken Sie **Eigenschaften**
1. Klicken Sie auf den **Schlüssel** Kopieren der `api-key`.

Wenn Sie die URL hinzugefügt haben und `api-key` in app.config, um Einstellungen sollte so aussehen:

   ![][11]


<a id="sub-4"></a>
## <a name="explore-the-application"></a>Untersuchen der Anwendungdes

Sie sind nahezu bereit, und führen Sie die Anwendung, aber bevor Sie tun, die JSON-Dateien erstellen und den Index aufgefüllt.

**Schema.JSON** definiert den Index einschließlich Punktzahl Profile betont werden in dieser Demo. Beachten Sie, dass das Schema definiert alle Felder im Index nicht durchsuchbaren Felder wie verwendet `margin`, die in ein Bewertungsprofil können. Bewertung Profilsyntax dokumentiert [Bewertungsprofil ein Azure-Suchindex hinzufügen](http://msdn.microsoft.com/library/azure/dn798928.aspx).

**Data1-3.json** stellt die Daten über eine Handvoll Genres 246 Alben. Daten aus tatsächlichen Album- und Interpreteninformationen Informationen wie mit fiktiven `price` und `margin` Suchvorgänge veranschaulicht. Dateien zum Index entsprechen und Suchdienst Azure hochgeladen werden. Nach Daten hochgeladen und indiziert ist, können Sie Abfragen für diese ausgeben.

**"Program.cs"** führt folgende Vorgänge aus:

- Öffnet ein Konsolenfenster.

- Azure Suche die Dienst-URL herstellt und `api-key`.

- Löscht die `musicstoreindex` Wenn sie vorhanden ist.

- Erstellt ein neues `musicstoreindex` mithilfe der Datei schema.json.

- Mit Dateien füllt.

- Abfragen mit vier Abfragen. Beachten Sie, dass die Punktzahl Profile als Abfrageparameter angegeben werden. Alle Abfragen suchen "beste" für den gleichen Zeitraum. Die erste Abfrage zeigt standardmäßig bewerten. Die verbleibenden drei Abfragen verwenden ein Bewertungsprofil.

<a id="sub-5"></a>
## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendungdes

Regel Konnektivität oder Verweis Probleme erstellen Sie, und führen Sie die Anwendung, um sicherzustellen, dass keine Probleme zuerst arbeiten. Ein Konsolenanwendungsprojekt im Hintergrund öffnen sollte angezeigt werden. Alle vier Abfragen ausführen ohne Unterbrechung nacheinander. Auf vielen Systemen führt das gesamte Programm in weniger als 15 Sekunden. Enthält das Konsolenanwendungsprojekt Meldung "abgeschlossen. Drücken Sie die EINGABETASTE, um fortzufahren", das Programm wurde erfolgreich abgeschlossen. 

Abfrage führt einen Vergleich Markierung kopieren können die Abfrageergebnisse in der Konsole und eine Excel-Datei einfügen. 

Die folgende Abbildung zeigt die Ergebnisse der ersten drei Abfragen Side-by-Side. Alle Abfragen verwenden den gleichen Suchbegriff "beste" zahlreiche von Albumtiteln erscheint.

   ![][10]

Die erste Abfrage verwendet standardmäßig bewerten. Suchbegriff wird nur in Alben, und keine anderen Kriterien angegeben, sind mit "beste" Titel in der Reihenfolge zurückgegeben, in denen Search-Dienst gefunden. 

Die zweite Abfrage verwendet ein Bewertungsprofil, aber beachten Sie, dass das Profil keine Wirkung. Die Ergebnisse sind identisch mit denen der ersten Abfrage. Ist das Bewertungsprofil ein Feld (Genre) erhöht, der nicht für die Abfrage. Der Begriff "beste" in allen "Genre" "jedes Dokument existiert nicht. Wenn ein Bewertungsprofil wirkungslos ist, entsprechen die Ergebnisse standardmäßig bewerten.  

Die dritte Abfrage ist die erste Anzeichen Auswirkungen bewerten. Der Suchbegriff ist weiterhin "beste" wir dieselben Alben arbeiten, aber da das Bewertungsprofil bietet zusätzliche Kriterien, die verstärkt "Bewertung" und "zuletzt aktualisiert", einige Elemente sind angetrieben höher in der Liste.

Die nächste Abbildung zeigt die Abfrage vierte und letzte "Rand" erhöht. Feld "Rand" ist nicht durchsuchbar und nicht in den Suchergebnissen zurückgegeben werden. Der Randwert "" manuell hinzugefügt Tabelle zu belegen, die mit höheren Margen höher in der Liste der Suchergebnisse angezeigt. 

   ![][9]

Da Sie mit Punktzahl versuchen Sie verschiedene Abfragesyntax verwendet Profile oder umfangreichere Daten bewerten experimentiert. Links im nächsten Abschnitt informieren.

<a id="next-steps"></a>
## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr zum Bewerten von Profilen. Einzelheiten finden Sie unter [Bewertungsprofil ein Azure-Suchindex hinzufügen](http://msdn.microsoft.com/library/azure/dn798928.aspx) .

Erfahren Sie mehr über Syntax und Abfrage suchen. Einzelheiten finden Sie unter [Dokumente durchsuchen (Azure Suche REST-API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) .

Müssen zu erfahren mehr über Index erstellen? [In diesem Video](http://channel9.msdn.com/Shows/Cloud+Cover/Cloud-Cover-152-Azure-Search-with-Liam-Cavanagh) die Grundlagen verstehen.

<!--Anchors-->
[Prerequisites]: #sub-1
[Download the sample application]: #sub-2
[Edit app.config]: #sub-3
[Explore the application]: #sub-4
[Build and run the application]: #sub-5
[Next steps]: #next-steps

<!--Image references-->
[12]: ./media/search-get-started-scoring-profiles/AzureSearch_CodeplexDownload.PNG
[11]: ./media/search-get-started-scoring-profiles/AzureSearch_Scoring_AppConfig.PNG
[10]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX1.PNG
[9]: ./media/search-get-started-scoring-profiles/AzureSearch_XLSX2.PNG 