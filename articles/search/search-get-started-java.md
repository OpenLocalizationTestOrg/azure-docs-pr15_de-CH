<properties
    pageTitle="Erste Schritte mit Azure Suche in Java | Microsoft Azure | Gehostete Cloud-Suchdienst"
    description="Wie eine gehostete Cloud Suche Anwendung in Azure Java als Programmiersprache verwenden."
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

# <a name="get-started-with-azure-search-in-java"></a>Erste Schritte mit Azure Suche in Java
> [AZURE.SELECTOR]
- [Portal](search-get-started-portal.md)
- [.NET](search-howto-dotnet-sdk.md)

Erfahren Sie, wie eine benutzerdefinierte Java-Suche-Anwendung erstellen, die Azure nach seiner Suchfunktionalität verwendet. In diesem Lernprogramm verwendet [Azure Search Service REST-API](https://msdn.microsoft.com/library/dn798935.aspx) -Objekte und Operationen in dieser Übung erstellen.

Zum Ausführen dieses Beispiels benötigen einen Azure-Suchdienst Sie zum [Azure-Portal](https://portal.azure.com)anmelden. Eine schrittweise Anleitung finden Sie unter [Erstellen einer Azure-Suchdienst im Portal](search-create-service-portal.md) .

So erstellen und Testen dieses Beispiel verwendet die folgende Software:

- [Eclipse-IDE für Java EE-Entwickler](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Achten Sie darauf, dass die EE-Version. Die Schritte erfordert ein Feature, das nur in dieser Ausgabe gefunden wird.

- [JDK-8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

- [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-the-data"></a>Datenschutz

Diese Anwendung verwendet Daten aus der [USA geologischen Dienste (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), auf den Zustand von Rhode Island Dataset verkleinern gefiltert. Wir verwenden diese Daten zu einer Suchmaschine, die Wahrzeichen wie Krankenhäuser und Schulen, geologischen wie Flüsse, Seen und Gipfel zurückgibt.

Das **SearchServlet.java** -Programm in dieser Anwendung erstellt und lädt mit einem [Indexer](https://msdn.microsoft.com/library/azure/dn798918.aspx) Konstrukt gefilterte USGS Dataset aus einer öffentlichen Azure SQL-Datenbank abrufen. Im Programmcode werden vordefinierte Anmelde- und Verbindungsinformationen zur online-Datenquelle bereitgestellt. Im Hinblick auf den Datenzugriff ist keine weitere Konfiguration erforderlich.

> [AZURE.NOTE] Wir wenden einen Filter auf dieses Dataset unter maximal 10.000 Dokument frei Tarif bleiben. Verwenden Sie die standard-Stufe diese Beschränkung gilt nicht, und ändern Sie diesen Code zur Verwendung eines größeren Datasets. Details für jede Tarif Kapazität finden Sie [Grenzen und Einschränkungen](search-limits-quotas-capacity.md).

## <a name="about-the-program-files"></a>Über die Programmdateien

Die folgende Liste beschreibt die Dateien, die für dieses Beispiel.

- Search.jsp: Die Benutzeroberfläche bereit.
- SearchServlet.java: Stellt Methoden (vergleichbar mit einem Controller in MVC)
- SearchServiceClient.java: Verarbeitet HTTP-Anfragen
- SearchServiceHelper.java: Eine Hilfsklasse, die statische Methoden enthält
- Document.Java: Stellt Daten
- config.Properties: Legt die Dienst-URL suchen und API-Schlüssel
- Pom.XML: Eine Maven-Beziehung

<a id="sub-2"></a>
## <a name="find-the-service-name-and-api-key-of-your-azure-search-service"></a>Suchen Sie der Dienstname und api-Schlüssel Azure Suchdienst

Alle REST API-Aufrufe in Azure Search verlangen, dass Sie die Dienst-URL und einen API-Schlüssel. 

1. [Azure-Portal](https://portal.azure.com)anmelden.
2. Klicken Sie in der Navigationsleiste auf **Search-Dienst** alle Azure Search Services für Ihr Abonnement bereitgestellt.
3. Wählen Sie den Dienst, den Sie verwenden möchten.
4. Auf dem Dashboard Service sehen Sie Kacheln für wichtige Informationen sowie das Schlüsselsymbol für den Zugriff auf die Administratorschlüssel.

    ![][3]

5. Kopieren Sie die URL und ein Administratorschlüssel. Sie benötigen später sie beim Hinzufügen der Datei **config.properties** .

## <a name="download-the-sample-files"></a>Die Beispieldateien downloaden

1. Rufen Sie [AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) auf Github.

2. Auf **ZIP herunterladen**, die ZIP-Datei auf Datenträger speichern und alle darin enthaltenen Dateien zu extrahieren. Extrahieren von Dateien in den Arbeitsbereich Java erleichtert das Projekt später zu berücksichtigen.

3. Die Beispieldateien sind schreibgeschützt. Maustaste auf Eigenschaften und deaktivieren Sie das Schreibschutzattribut.

Alle nachfolgenden Datei geändert und zur Anweisung erfolgen für Dateien in diesem Ordner.  

## <a name="import-project"></a>Projekt importieren

1. In Eclipse wählen Sie **Datei** > **Import** > **Allgemeine** > **Vorhandene Projekte in Arbeitsbereich**.

    ![][4]

2. **Stammverzeichnis auswählen**suchen Sie den Ordner mit Beispieldateien. Wählen Sie den Ordner des Projekts enthält. Das Projekt sollte als ein ausgewähltes Element in der Liste **Projekte** angezeigt.

    ![][12]

3. Klicken Sie auf **Fertig stellen**.

4. Verwenden Sie **Projekt-Explorer** zum Anzeigen und Bearbeiten von Dateien. Wenn es nicht bereits geöffnet ist, klicken Sie auf **Fenster** > **Ansicht anzeigen** > **Projekt-Explorer** oder die Verknüpfung zu öffnen.

## <a name="configure-the-service-url-and-api-key"></a>Konfigurieren Sie die Dienst-URL und API-Schlüssel

1. Doppelklicken Sie im **Projekt-Explorer**auf **config.properties** um den Servernamen und den API-Schlüssel enthält Konfigurationsinformationen zu bearbeiten.

2. Finden Sie die Schritte in diesem Artikel, wo Sie gefunden die Dienst-URL und den API-Schlüssel im [Azure-Portal](https://portal.azure.com), um die Werte, die Sie jetzt in **config.properties**eingeben.

3. Ersetzen Sie in **config.properties**mit der api für den Dienst "API-Schlüssel". Dienstname (die erste Komponente der URL http://servicename.search.windows.net) ersetzt anschließend "Dienstname" in derselben Datei.

    ![][5]

## <a name="configure-the-project-build-and-runtime-environments"></a>Konfigurieren der Projekt-, Build- und Runtime-Umgebung

1. In Eclipse im Projekt-Explorer mit der rechten Maustaste des Projekts > **Eigenschaften** > **Projekt Facets**.

2. Wählen Sie **dynamische Webmodul**, **Java**und **JavaScript**.

    ![][6]

3. Klicken Sie auf **Übernehmen**.

4. Wählen Sie **Fenster** > **Voreinstellungen** > **Server** > **Laufzeitumgebungen** > **hinzufügen.**.

5. Erweitern Sie Apache und wählen Sie die Version von Apache Tomcat-Server bereits installiert. In unserem System installiert wir Version 8.

    ![][7]

6. Geben Sie auf der nächsten Seite das Tomcat-Installationsverzeichnis. Auf einem Windows-Computer werden diese wahrscheinlich c:\Programme\Microsoft Files\Apache Software Foundation\Tomcat *Version*.

6. Klicken Sie auf **Fertig stellen**.

7. Wählen Sie **Fenster** > **Voreinstellungen** > **Java** > **JRE installiert** > **Hinzufügen**.

8. **JRE hinzuzufügen**wählen Sie **Standard-VM**.

10. Klicken Sie auf **Weiter**.

11. JRE Definition JRE home klicken Sie auf **Verzeichnis**.

12. Navigieren Sie zu **Programme** > **Java** , und JDK wählen Sie installiert. Es ist wichtig, den JDK JRE auswählen.

13. Wählen Sie JRE installiert **JDK**. Ihre sieht ähnlich wie im folgenden Screenshot.

    ![][9]

14. Optional Wählen Sie **Fenster** > **Web-Browser** > **Internet Explorer** , um die Anwendung in einem externen Browser-Fenster öffnen. Einen externen Browser ermöglicht Web Application optimiert.

    ![][8]

Die Konfigurationsaufgaben ist abgeschlossen. Als Nächstes Sie erstellen und Ausführen des Projekts.

## <a name="build-the-project"></a>Erstellen Sie das Projekt

1. Im Projekt-Explorer mit der rechten Maustaste des Namens des Projekts, und wählen Sie **Ausführen als** > **Maven erstellen...** , konfigurieren Sie das Projekt.

    ![][10]

8. Konfiguration bearbeiten Ziele geben Sie "Neuinstallation" und klicken Sie auf **Ausführen**.

Nachrichten werden im Konsolenfenster. Zeigt das Projekt ohne Fehler BUILDERFOLG sollte angezeigt werden.

## <a name="run-the-app"></a>Führen Sie die Anwendung

In diesem letzten Schritt wird die Anwendung in einem lokalen Server Runtime-Umgebung ausgeführt werden.

Wenn Sie eine Server-Runtime-Umgebung noch in Eclipse angegeben haben, müssen Sie zuerst tun.

1. Erweitern Sie im Projektexplorer **Inhaltsordner**.

5. Mit der rechten Maustaste **Search.jsp** > **als** > **auf dem Server ausgeführt**. Wählen Sie den Apache Tomcat-Server und dann auf **Ausführen**.

> [AZURE.TIP] Wenn Sie einen nicht standardmäßigen Arbeitsbereich für das Projekt verwendet, müssen Sie **Testlaufkonfiguration** auf den Projektspeicherort eines Serverfehlers Start zu ändern. Klicken Sie im Projekt-Explorer **Search.jsp** > **Ausführen als** > **Testlaufkonfigurationen**. Wählen Sie den Apache Tomcat-Server. Klicken Sie auf **Argumente**. Klicken Sie auf **Arbeitsbereich** oder **Dateisystem** , um den Ordner mit dem Projekt.

Wenn Sie die Anwendung ausführen, erhalten Sie ein Browserfenster über ein Suchfeld Begriffe eingeben.

Warten Sie ungefähr eine Minute, bevor Sie auf **Suchen** , geben Sie die Reaktionszeit erstellen und Laden des Indexes. Wenn Sie einen HTTP 404-Fehler erhalten, müssen Sie nur Versuch etwas warten.

## <a name="search-on-usgs-data"></a>USGS Daten suchen

USGS DataSet enthält Datensätze, die zum Zustand Rhode Island relevant sind. Wenn Sie leere Suchfeld **Suchen** klicken, erhalten Sie die Top 50 Einträge Standard.

Einen Suchbegriff wird der Suchmaschine etwas auf eingeben. Versuchen Sie, einen regionalen Namen. "Roger Williams" war Gouverneur von Rhode Island. Zahlreiche Parks, Gebäude und Schulen werden benannt.

![][11]

Versuchen Sie außerdem einen dieser Begriffe:

- Pawtucket
- Pembroke
- Gans + cape

## <a name="next-steps"></a>Nächste Schritte

Dies ist das erste Azure Search Lernprogramm basierend auf Java und Universelle Dataset. Im Laufe der Zeit erweitern wir dieses Lernprogramm, um zusätzliche Features zeigen, die Sie in Ihre benutzerdefinierten Projektmappen verwenden möchten.

Haben Sie bereits einige Hintergrundinformationen in Azure Suche, können dieses Beispiel als Sprungbrett für weitere Versuche Sie vielleicht [Suchseite](search-pagination-page-layout.md)erweitern oder [facettierten](search-faceted-navigation.md)zu implementieren. Sie können auch auf der Suchergebnisseite verbessern, Anzahl und Batchverarbeitung Dokumente, sodass Benutzer durch die Ergebnisse navigieren können.

Neu bei Azure suchen? Wir empfehlen versuchen, andere Lernprogramme entwickeln Kenntnisse erstellen können. Besuchen Sie unsere [Dokumentation](https://azure.microsoft.com/documentation/services/search/) finden weitere Ressourcen. Sie können auch Links in unsere [Video und Tutorial Liste](search-video-demo-tutorial-list.md) Informationen anzeigen.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
