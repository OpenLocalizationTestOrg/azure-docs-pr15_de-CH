<properties
    pageTitle="Neuigkeiten von Azure suchen | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Azure Search ist ein Suchdienst gehostete Cloud vollständig verwaltet. Featureübersicht Weitere."
    services="search"
    manager="jhubbard"
    authors="ashmaka"
    documentationCenter=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="what-is-azure-search"></a>Was ist Azure Suche?

Azure Suche ist eine Cloud Suche als Service-Lösung, die delegiert Server und Infrastruktur-Management an Microsoft mit fertige-Dienst, den mit Daten füllen und Suchen Ihrer Web oder mobilen Anwendung hinzufügen können. Azure Suche können Sie Ihre Anwendung mit einem einfachen [REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx) oder [.NET SDK](search-howto-dotnet-sdk.md) ohne Suche Infrastrukturmanagement ein Experte Suche problemlos stabile Suchfunktionen hinzu.

## <a name="give-your-users-a-powerful-search-experience"></a>Geben Sie Ihren Anwendern eine leistungsstarke Umgebung

**Leistungsfähige Abfragen** können mithilfe der [einfachen Abfragesyntax](https://msdn.microsoft.com/library/azure/dn798920.aspx), mit logischen Operatoren Operatoren Ausdruck Suffixoperatoren, Rangfolge Operatoren formuliert werden. Darüber hinaus können die [Abfragesyntax Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) fuzzy-Suche, Suche Begriff angehoben und reguläre Ausdrücke. Azure unterstützt auch benutzerdefinierte lexikalische Analyzer um Ihre Anwendung mit phonetischen übereinstimmende komplexe Suchabfragen und reguläre Ausdrücke ermöglichen.

**Sprache** sind [für 56 Sprachen enthalten](https://msdn.microsoft.com/library/azure/dn879793.aspx). Mit Lucene Analyzer und Microsoft-Analyzer (raffiniert Jahre und Bing Verarbeitung natürlicher Sprache) können Azure Suche Text im Suchfeld Intelligent sprachspezifischen Linguistics einschließlich Zeitformen, Geschlecht, unregelmäßige Pluralform Nomen (z. B. ' Maus' und 'Maus'), Word de zusammensetzen, Worttrennung (für Sprachen ohne Leerzeichen) behandelt die Anwendung analysieren.

**Suchvorschläge** kann für Befehlszeilen suchen und Typeahead-Abfragen aktiviert werden. Geben Sie als Benutzer [aktuelle Dokumente im Index vorgeschlagen werden](https://msdn.microsoft.com/library/azure/dn798936.aspx) teilsuche Eingabe.

**Treffern** [ermöglicht](https://msdn.microsoft.com/library/azure/dn798927.aspx) Benutzern den Ausschnitt von Text in jedes Ergebnis, die Ergebnisse für die Abfrage enthält. Sie können wählen, welche Felder hervorgehobenen Ausschnitte zurück.

**Facettierte Navigation** wird einfach die Suchergebnisseite Azure Suche hinzugefügt. Mit [nur einem einzelnen Abfrageparameter](https://msdn.microsoft.com/library/azure/dn798927.aspx), liefert Azure Suche alle notwendigen Informationen zum Erstellen einer facettierten Suchfunktionalität in Ihre app Benutzeroberflächenelementen Benutzer Detailinformationen und Suchergebnisse (z.B. Filter Katalogelemente Preisklasse oder Marke) filtern.

**Geo-räumliche** Support können Sie Intelligent verarbeiten, Filtern und Anzeigen von Standorten. Azure Suche können Benutzer Daten in der Nähe eines Suchergebnisses an einem bestimmten Speicherort oder auf eine bestimmte Region basieren. In diesem Video wird erläutert, wie sie: [Channel 9: Azure Search und geografische Daten](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data).

**Filter** können problemlos facettierten Navigation in der Benutzeroberfläche Ihrer Anwendung integrieren, verbessern Abfrage Formulierung verwendet und Filter basierend auf Benutzer oder Entwickler-angegebenen Kriterien. Erstellen Sie leistungsfähige Filter mit der [OData-Syntax](https://msdn.microsoft.com/library/azure/dn798921.aspx).

## <a name="empower-your-developers-with-an-easy-to-use-service"></a>Ermöglichen Sie Entwickler mit einem einfachen Dienst

**Hohe Verfügbarkeit** gewährleistet eine äußerst zuverlässige Suche Service. Wenn skaliert, [Azure Suche bietet eine SLA 99,9 %](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Als End-to-End-Lösung, Azure Suche **vollständig verwaltet** muss absolut keine Infrastruktur-Management. Ihr Dienst kann problemlos an Ihre Bedürfnisse angepasst werden, durch Skalierung in zwei Dimensionen Speicherplatz Dokument höher Abfragen und/oder behandeln.

**Datenintegration** mithilfe von [Indexern](https://msdn.microsoft.com/library/azure/dn946891.aspx) können Azure Search suchen automatisch Azure SQL-Datenbank, Azure DocumentDB und [Azure BLOB-Speicher](search-howto-indexing-azure-blob-storage.md) zum Synchronisieren der Suchindex Inhalte mit primären Datenspeicher.

**Dokumentieren Sie Knacken** ist verfügbar (in Vorschau) [zu lesen und zu indizieren gängigen Dateiformate](search-howto-indexing-azure-blob-storage.md) sowie Microsoft Office PDF- und HTML-Dokumente.

**Suche Datenverkehr Analytics** sind [leicht gesammelt und analysiert](search-traffic-analytics.md) , Erkenntnisse, was Benutzer in das Suchfeld eingeben.

**Einfache Bewertung** ist ein wichtiger Vorteil von Azure suchen. [Bewertung der Profile](https://msdn.microsoft.com/library/azure/dn798928.aspx) werden die Organisationen Modell Relevanz als Funktion der Werte in den Dokumenten selbst verwendet. Beispielsweise könnten neuer Produkte oder Rabatt Produkte höher in den Suchergebnissen erscheinen. Sie können auch erstellen scoring Profile mit Tags für personalisierte Bewertung basierend auf Kunde Suchoptionen nachverfolgt und separat gespeichert haben.

**Sortierung** für mehrere Felder über das IndexSchema angeboten und dann zur Abfragezeit mit einzelnen Suchparameter umgeschaltet.

**Paging** und Drosselung Suchergebnisse ist [einfach mit dem Abstimmen](search-pagination-page-layout.md) , die Suchergebnisse Azure Suche bietet.  

**Search Explorer** können Sie Problem Abfragen aller Ihrer Indizes rechts von Ihr Konto Azure-Portal einfach Abfragen testen und verfeinern Punktzahl Profile.

## <a name="how-it-works"></a>Funktionsweise

### <a name="1-provision-service"></a>1. Bereitstellung service
Sie können ein Azure-Suchdienst mithilfe der [Azure-Portal](https://portal.azure.com/) oder [Azure Resource Management API](https://msdn.microsoft.com/library/azure/dn832684.aspx)Hochfahren.

Je nach den Suchdienst Konfiguration verwenden Sie freien Tier Service mit anderen Teilnehmern Azure Search freigegeben wird, oder ein [Tier gezahlt](https://azure.microsoft.com/pricing/details/search/) , die Ressourcen vom Dienst widmet. Bei der Bereitstellung, wählen Sie auch den Bereich des Rechenzentrums, der den Dienst hostet.

Je nach der Ebene des Dienstes Sie wählen Ihren Dienst in zwei Dimensionen skaliert werden können: 1) Hinzufügen von Replikaten zu Ihrer Kapazität stark Abfragen und 2) Partitionen, um Speicherplatz für weitere Dokumente hinzufügen. Dokument Datenspeicher- und Durchsatz separat behandelt, können Sie den Suchdienst an Ihre spezifischen Bedürfnisse anpassen.

### <a name="2-create-index"></a>2 Index erstellen
Bevor Sie Azure Suchdienst Ihre Inhalte hochladen können, müssen Sie zuerst ein Azure-Suchindex definieren. Ein Index ist wie eine Datenbanktabelle, die Ihre Daten und Suchabfragen annehmen kann. Sie definieren das IndexSchema Struktur der Dokumente zuordnen, wie Felder in einer Datenbank suchen möchten.

Das Schema dieser Indizes kann entweder im Azure-Portal oder programmgesteuert [mit .NET SDK](search-howto-dotnet-sdk.md) oder [REST-API](https://msdn.microsoft.com/library/azure/dn798941.aspx)erstellt werden. Definiertes Index können Sie dann Daten in Azure-Suchdienst hochladen, wo er später indiziert ist.

### <a name="3-index-data"></a>3. Daten
Nachdem Sie die Felder und der Index definiert haben, können Sie die Inhalte in den Index hochladen. Eine Push- oder Pull-Modell können zum Hochladen von Daten auf den Index.

Pull-Modell wird über Indexer bereitgestellt, die für kann, auf Anforderung oder geplante Updates konfiguriert werden (siehe [Indizierungsvorgänge Azure Search Service REST API ()](https://msdn.microsoft.com/library/azure/dn946891.aspx)), können Sie problemlos Daten und Änderungen aus Azure DocumentDB, Azure SQL-Datenbank, Azure BLOB-Speicher oder SQL Server gehosteten Azure-VM aufzunehmen.

Push-Modell wird über SDK oder REST-APIs zum Versand aktualisierte Dokumente zu einem Index bereitgestellt. Sie können Daten aus jedem Dataset im JSON-Format drücken. Siehe [Hinzufügen, aktualisieren oder Löschen von Dokumenten](https://msdn.microsoft.com/library/azure/dn798930.aspx) oder [wie .NET SDK)](search-howto-dotnet-sdk.md) Anleitung zum Laden von Daten.

### <a name="4-search"></a>4. Suchen
Nachdem Sie Ihre Azure-Suchindex aufgefüllt haben, können Sie [Suchabfragen Problem](https://msdn.microsoft.com/library/azure/dn798927.aspx) jetzt Ihr Dienstendpunkt REST-API oder .NET SDK mit einfachen HTTP-Anfragen.

## <a name="try-it-now-for-free"></a>Probieren Sie es (kostenlos!)
Sie können Azure heute versuchen Suche! Wenn Sie bereits ein Azure-Konto verfügen, können Sie [Bereitstellung einen Dienst in der freien](search-create-service-portal.md).

Haben Sie ein Azure-Konto können Sie eine kostenlose, 60-minütigen Sitzung ohne, erforderlich. Zum [Versuchen Azure App Service](http://go.microsoft.com/fwlink/p/?LinkId=618214) und wählen Sie "Web App". Dann wählen Sie "ASP.NET + Azure Search" beginnen.
