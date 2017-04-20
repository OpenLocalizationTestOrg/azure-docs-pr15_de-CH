<properties
    pageTitle="Erste Schritte mit Azure Suche in NodeJS | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Erstellen einer Suchanwendung auf einer gehosteten Cloud-Suchdienst mit NodeJS als Programmiersprache Azure durchgehen."
    services="search"
    documentationCenter=""
    authors="EvanBoyle"
    manager="pablocas"
    editor="v-lincan"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.date="07/14/2016"
    ms.author="evboyle"/>

# <a name="get-started-with-azure-search-in-nodejs"></a>Erste Schritte mit Azure Suchen in NodeJS
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Informationen Sie zum Erstellen einer benutzerdefinierten NodeJS Suche Anwendung, Azure nach seiner Suchfunktionalität verwendet. In diesem Lernprogramm verwendet [Azure Search Service REST-API](https://msdn.microsoft.com/library/dn798935.aspx) -Objekte und Operationen in dieser Übung erstellen.

Verwendet [NodeJS](https://nodejs.org) und NPM, [erhabene Text 3](http://www.sublimetext.com/3)und Windows PowerShell auf Windows 8.1 entwickeln und Testen Sie den Code.

Zum Ausführen dieses Beispiels benötigen einen Azure-Suchdienst Sie zum [Azure-Portal](https://portal.azure.com)anmelden. Eine schrittweise Anleitung finden Sie unter [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) .

## <a name="about-the-data"></a>Datenschutz

Diese Anwendung verwendet Daten aus der [USA geologischen Dienste (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), auf den Zustand von Rhode Island Dataset verkleinern gefiltert. Wir verwenden diese Daten zu einer Suchmaschine, die Wahrzeichen wie Krankenhäuser und Schulen, geologischen wie Flüsse, Seen und Gipfel zurückgibt.

Das **DataIndexer** -Programm in dieser Anwendung erstellt und lädt mit einem [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) Konstrukt gefilterte USGS Dataset aus einer öffentlichen Azure SQL-Datenbank abrufen. Anmeldeinformationen und Verbindung mit online-Datenquelle Informationen im Programmcode. Es ist keine weitere Konfiguration erforderlich.

> [AZURE.NOTE] Wir wenden einen Filter auf dieses Dataset unter maximal 10.000 Dokument frei Tarif bleiben. Verwenden Sie die standard-Stufe gilt diese Beschränkung nicht. Details für jede Tarif Kapazität finden Sie unter [Search Service Grenzen](search-limits-quotas-capacity.md).


<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Suchen Sie der Dienstname und api-Schlüssel Azure Suchdienst

Nach dem Erstellen des Dienstes im Portal die URL zurück oder `api-key`. Verbindung zum Suchdienst benötigen Sie die URL und `api-key` Aufruf authentifizieren.

1. [Azure-Portal](https://portal.azure.com)anmelden.
2. Klicken Sie in der Navigationsleiste auf **Search-Dienst** alle Azure Search Services für Ihr Abonnement bereitgestellt.
3. Wählen Sie den Dienst, den Sie verwenden möchten.
4. Auf dem Dashboard Service sehen Sie Kacheln für wichtige Informationen sowie das Schlüsselsymbol für den Zugriff auf die Administratorschlüssel.

    ![][3]

5. Kopieren Sie URL des Admin-Schlüssel und einer Abfrageschlüssel Sie benötigen drei später, wenn Sie die Datei config.js hinzufügen.

## <a name="download-the-sample-files"></a>Die Beispieldateien downloaden

Verwenden Sie eine der folgenden Methoden zum Herunterladen des Beispiels.

1. Gehen Sie zu [AzureSearchNodeJSIndexerDemo](https://github.com/AzureSearch/AzureSearchNodeJSIndexerDemo).
2. Auf **ZIP herunterladen**, speichern Sie die ZIP-Datei und extrahieren Sie alle darin enthaltenen Dateien zu.

Alle nachfolgenden Datei geändert und zur Anweisung erfolgen für Dateien in diesem Ordner.


## <a name="update-the-configjs-with-your-search-service-url-and-api-key"></a>Aktualisieren der config.js. mit Dienst-URL suchen und API-Schlüssel

Mit URL und api-Schlüssel, der zuvor kopierte URL Admin-Schlüssel und Abfrage-Schlüssel in Konfigurationsdatei angeben.

Administratorschlüssel gewähren Sie Vollzugriff auf Dienstvorgänge, einschließlich erstellen oder Löschen eines Indexes und Dokumente. Im Gegensatz dazu sind Abfrage Schlüssel für schreibgeschützte Vorgänge meist von Azure Search mit Clientanwendungen.

In diesem Beispiel sind Abfrageschlüssel um die best Practice in Clientanwendungen Abfrage Taste beitragen.

Der folgende Screenshot zeigt **config.js** öffnen in einem Text-Editor mit Posten abgegrenzt, so dass Sie, wo sehen können Sie die Datei mit den Werten aktualisieren, die für den Suchdienst.

![][5]


## <a name="host-a-runtime-environment-for-the-sample"></a>Host ein Runtime-Umgebung für das Beispiel

Das Beispiel erfordert einen Installation mit global Npm HTTP-Server.

Verwenden Sie ein PowerShell-Fenster für die folgenden Befehle.

1. Navigieren Sie zu dem Ordner mit der Datei **package.json** .
2. Typ `npm install`.
2. Typ `npm install -g http-server`.

## <a name="build-the-index-and-run-the-application"></a>Index erstellen und Ausführen der Anwendungdes

1. Typ `npm run indexDocuments`.
2. Typ `npm run build`.
3. Typ `npm run start_server`.
4. Direkte Browser an`http://localhost:8080/index.html`

## <a name="search-on-usgs-data"></a>USGS Daten suchen

USGS DataSet enthält Datensätze, die zum Zustand Rhode Island relevant sind. Wenn Sie leere Suchfeld **Suchen** klicken, erhalten Sie die Top 50 Einträge Standard.

Einen Suchbegriff wird der Suchmaschine etwas auf eingeben. Versuchen Sie, einen regionalen Namen. "Roger Williams" war Gouverneur von Rhode Island. Zahlreiche Parks, Gebäude und Schulen werden benannt.

![][9]

Versuchen Sie außerdem einen dieser Begriffe:

- Pawtucket
- Pembroke
- Gans + cape


## <a name="next-steps"></a>Nächste Schritte

Dies ist das erste Azure Search Lernprogramm basierend auf NodeJS und das Dataset USGS. Im Laufe der Zeit erweitern wir dieses Lernprogramm, um zusätzliche Features zeigen, die Sie in Ihre benutzerdefinierten Projektmappen verwenden möchten.

Haben Sie bereits einige Hintergrundinformationen in Azure Suche, können dieses Beispiel als Ausgangspunkt Sie dank Suggesters (Typ vor oder AutoVervollständigen Abfragen), Filter und facettierten Navigation. Sie können auch auf der Suchergebnisseite verbessern, Anzahl und Batchverarbeitung Dokumente, sodass Benutzer durch die Ergebnisse navigieren können.

Neu bei Azure suchen? Wir empfehlen versuchen, andere Lernprogramme entwickeln Kenntnisse erstellen können. Besuchen Sie unsere [Dokumentation](https://azure.microsoft.com/documentation/services/search/) finden weitere Ressourcen. Sie können auch Links in unsere [Video und Tutorial Liste](search-video-demo-tutorial-list.md) Informationen anzeigen.

<!--Image references-->
[1]: ./media/search-get-started-nodejs/create-search-portal-1.PNG
[2]: ./media/search-get-started-nodejs/create-search-portal-2.PNG
[3]: ./media/search-get-started-nodejs/create-search-portal-3.PNG
[5]: ./media/search-get-started-nodejs/AzSearch-NodeJS-configjs.png
[9]: ./media/search-get-started-nodejs/rogerwilliamsschool.png
