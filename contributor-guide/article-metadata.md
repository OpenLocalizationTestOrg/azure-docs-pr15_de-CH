

#<a name="metadata-for-azure-technical-articles"></a>Metadaten für Azure technische Artikel

Alle Azure technische Artikeln enthalten zwei Metadaten - Eigenschaften und ein Tag. Eigenschaftenabschnitt kann einige Website-Automatisierung und Sachen SEO während der Tags-Abschnitt viel Content-Berichterstattung ermöglicht. Beide Abschnitte sind erforderlich.

- [Syntax]
- [Verwendung]
- [Attribute und Werte für Eigenschaftenabschnitt]
- [Attribute und Werte zum Abschnitt tags]

##<a name="syntax"></a>Syntax

Eigenschaftenabschnitt verwendet die folgende Syntax:

    <properties
       pageTitle="Page title that displays in search results and the browser tab | Microsoft Azure"
       description="Article description that will be displayed on landing pages and in most search results"
       services="service-name"
       documentationCenter="dev-center-name"
       authors="GitHub-alias-of-only-one-author"
       manager="manager-alias"
       editor=""
       tags="optional"
       keywords="For use by SEO champs only. Separate terms with commas. Check with your SEO champ before you change content in this article containing these terms."/>

Abschnitt "Tags" verwendet die folgende Syntax:

    <tags
       ms.service="required"
       ms.devlang="may be required"
       ms.topic="article"
       ms.tgt_pltfrm="may be required"
       ms.workload="na"
       ms.date="mm/dd/yyyy"
       ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

##<a name="usage"></a>Verwendung

- Die Elementnamen und Attributnamen Groß-/Kleinschreibung.
- Die <properties> Abschnitt muss die erste Zeile der Datei.
- Lassen Sie eine Leerzeile nach jedem Metadatenabschnitt und der Seitentitel, um sicherzustellen, dass der Seitentitel während der Veröffentlichung korrekt in HTML konvertiert wird.

## <a name="attributes-and-values-for-the-properties-section"></a>Attribute und Werte für Eigenschaftenabschnitt

![](./media/article-metadata/checkmark-small.png)**Titel**: erforderlich. wichtige Seo. Der Text für dieses Attribut wird in der Registerkarte und der Titel in einem Suchergebnis. 55-60 Zeichen einschließlich Leerzeichen und einschließlich der Site-ID *| Microsoft Azure* (Typ: Pipe Platz Microsoft Azure Speicherplatz).  Der Titel sollte H1 unterscheidet.

![](./media/article-metadata/checkmark-small.png)**Beschreibung**: erforderlich. wichtig für SEO (Relevanz) und Website-Funktionalität. Die Beschreibung sollte mindestens 125 Zeichen lange auf 155 Zeichen einschließlich Leerzeichen. Beschreiben des Zwecks des Inhalts so Kunden wissen, ob eine Liste von Suchergebnissen auswählen. Der Wert ist:

- Dieser Text kann als Beschreibung oder abstrakte Absatz auf Google angezeigt.
- Dieser Text wird im [Artikel Indexergebnisse](https://azure.microsoft.com/documentation/articles/)angezeigt.

![](./media/article-metadata/checkmark-small.png)**Services**: für Artikel mit einem Dienst. Dieser Wert wird oft als "Servce Slug" bezeichnet. Listen Sie alle verfügbaren Dienste, die durch Kommas getrennt. Der erste Dienst, den Sie Liste Laufwerk Navigation Breadcrumbs für die Seite und der linken Navigationsleiste der Seite angezeigt wird.

Artikel, die einen Services-Wert und einen DocumentationCenter-Wert angeben, werden Dienste Wert Breadcrumb fahren. Weitere Werte, die Sie als Tags in den publizierten Artikel aufgeführt wird. Werte:

- Active directory
- Active Directory b2c
- Active-Directory-ds
- App-service\api
- API-management
- App-service
- App-servic\mobile
- App-service\web
- App-service\logic
- Gateway-Anwendung
- Anwendung Einblicke
- Automatisierung
- Azure-portal
- Azure-Ressourcen-manager
- Azure-stack
- Sicherung
- Stapelverarbeitung
- Best Practices
- BizTalk-Dienste
- Cache
- CDN
- Cloud-services
- Daten-Katalog
- Daten-factory
- Datenanalyse-See
- See Datenspeicher
- Devtest lab
- DNS
- documentdb
- expressroute
- Ereignis-hubs
- hdinsight
- IOT hub
- Schlüssel-Depot
- Lastenausgleich
- lernfähige
- Marketplace
- Media-services
- Mobile engagement
- Mobile Dienste
- Multi-factor-Authentifizierung
- Benachrichtigung hubs
- betriebliche Informationen
- Operations Management suite
- powerapps
- Recovery manager
- Redis-cache
- RemoteApp
- Verwaltung von Informationsrechten
- Planer
- Suche
- Security center
- Service-bus
- Service-fabric
- Recovery-Standort
- SQL-Datenbank
- SQL Data warehouse
- SQL reporting
- Speicher
- Speichern
- storsimple
- Stream analytics
- Traffic manager
- virtuelle Maschinen
- virtuelles Netzwerk
- Visual Studio-online
- VPN-gateway
- Websites

![](./media/article-metadata/checkmark-small.png)**DocumentationCenter**: Dev-orientierte Artikel am besten durch eine Entwicklungscenter wichtige erforderlich. Geben Sie die einzelnen Entwickler oder Sprache Artikel gilt an Eingestellten Wert fahren Navigation Breadcrumbs für die Seite. Artikel, die einen Services-Wert und einen DocumentationCenter-Wert angeben, werden Dienste Wert Breadcrumb fahren. Werte:

- **.NET**
- **nodejs**
- **Java**
- **PHP**
- **Python**
- **Ruby**
- **Mobile**: veraltet. Ersetzen Sie durch spezifische Plattform.
- **Ios**: Verifing dieser neue Wert
- **Android**: überprüfen den neuen Wert
- **Windows**: überprüfen den neuen Wert
- **Xamarin**: überprüfen den neuen Wert

![](./media/article-metadata/checkmark-small.png)**Autoren**: Wert erforderlich. GitHub-Konto für den primären Autor oder Artikel KMU auflisten Dieses Attribut Laufwerke der Autorenzeile publizierten Artikel. Listen Sie nur eine trotz der Pluralname des Attributs.

![](./media/article-metadata/checkmark-small.png)**Manager**: erforderlich, wenn Sie Microsoft Mitwirkender sind. Liste des e-Mail-Alias des Content publishing Manager für Bereich. Eine Community Contributor hingegen das Attribut jedoch leer lassen, damit wir es ausfüllen können.

![](./media/article-metadata/checkmark-small.png)**Editor**: nicht verwendet. Verwenden sie nicht für andere Zwecke.

![](./media/article-metadata/checkmark-small.png)**Tags**: Optional. Nur fügen Sie ein, wenn eine Verknüpfung unterhalb der Breadcrumb Artikel zur Indexseite Artikel (http://azure.microsoft.com/documentation/articles/) soll ungefilterten Liste der Artikel, die der zugelassenen Werte entsprechen. Diese Werte bieten eine Möglichkeit, Inhalte zu gruppieren, wenn Inhalte Gruppierung nicht dienstspezifische sollen. Diese Tags können auch bezeichnen, die die Technologie gibt Artikel gilt. Dieser Wert **nicht** unterstützt Freiform-Tags oder Hashtags. die Tags müssen auf der Website aktiviert werden. Sie können mehrere Tags Werte einen Artikel durch Kommas getrennt angeben. Die zugelassenen Werte sind:

  - Architektur
  - Azure-Ressourcen-manager
  - Azure-Service-management
  - Abrechnung
  - MySQL

![](./media/article-metadata/checkmark-small.png)**Schlüsselwörter**: Optional. Für die Verwendung von SEO-Champs. Getrennte Begriffe durch Kommas. **Überprüfen Sie mit Ihrem SEO ändern oder Löschen dieser Artikel mit diesen Begriffen.** Dieses Attribut zeichnet Schlüsselwörter, die Seo Champ hat und Verbesserung Suche Rang nachverfolgen. Die Schlüsselwörter werden nicht im veröffentlichten HTML gerendert. Validierung wird dieses Attribut nicht erforderlich.

## <a name="attributes-and-values-for-the-tags-section"></a>Attribute und Werte zum Abschnitt tags

![](./media/article-metadata/checkmark-small.png)**ms.Service**: erforderlich. Gibt die Azure Service, Tools oder Funktion, die der Artikel betrifft. Ein Wert pro Seite.

 Wenn eine Seite mehrere Dienste gilt, wählen Sie den Dienst auf den meisten direkt gilt; Beispielsweise muss ein Artikel, der eine Anwendung gehostet auf Websites verwendet, Service Bus Funktionen vorzuführen **Service Bus** Wert statt mit dem **Websites**. Wenn eine Seite mehrere Dienste gilt, wählen Sie **mehrere**. Wenn eine Seite keine Services verwendet wird (dies selten ist), wählen Sie **NA**.

 - **Active directory**
 - **Active Directory b2c**
 - **Active-Directory-ds**
 - **API-management**
 - **App-Service**: gilt nur für allgemeine interessanten App-Dienst
 - **App-Dienst-api**
 - **Service Anwendungslogik**
 - **App-Service-mobile**
 - **App-Dienst-web**
 - **Anwendung Einblicke**
 - **Gateway-Anwendung**
 - **Automatisierung**
 - **Azure-Ressourcen-manager**
 - **Azure-Sicherheit**
 - **Azure-stack**
 - **Sicherung**
 - **Stapelverarbeitung**
 - **Best Practices**
 - **BizTalk-Dienste**
 - **Abrechnung**
 - **Cache**
 - **CDN**
 - **Cloud-services**
 - **Daten-Katalog**
 - **See Datenspeicher**
 - **Datenanalyse-See**
 - **Devtest lab**
 - **expressroute**
 - **hdinsight**
 - **Internet der Dinge**
 - **IOT hub**
 - **Schlüssel-Depot**
 - **lernfähige**
 - **Markt**: Artikel über Azure Marketplace
 - **Media-services**
 - **Mobile engagement**
 - **Mobile Dienste**
 - **Multi-factor-Authentifizierung**
 - **mehrere**: Seite gilt Diensten
 - **Na**: Seite gilt nicht für Dienste (selten)
 - **Benachrichtigung hubs**
 - **betriebliche Informationen**
 - **powerapps**
 - **Recovery manager**
 - **Redis-cache**
 - **RemoteApp**
 - **Verwaltung von Informationsrechten**
 - **Planer**
 - **Security center**
 - **Service-bus**
 - **Service-fabric**
 - **Site Recovery**: früher Recovery-Services
 - **SQL-Datenbank**
 - **SQL Data warehouse**
 - **SQL reporting**
 - **Speicher**
 - **Speichern**: Artikel über Dienste über den Azure-Speicher
 - **storsimple**
 - **Traffic manager**
 - **virtuelle Maschinen**
 - **virtuelles Netzwerk**
 - **Visual Studio-online**
 - **VPN-gateway**
 - **Websites**

![](./media/article-metadata/checkmark-small.png)**ms.DevLang**: erforderlich. Gibt die Programmiersprache, der der Artikel betrifft. Einzelner Wert pro Seite.

 Wenn eine Seite zwei Programmiersprachen gilt, wählen Sie **mehrere**. Wenn eine Seite hauptsächlich konzeptionellen und seinen Inhalt im Allgemeinen mehrere Programmiersprachen gilt, wählen Sie **mehrere**. Wenn eine Seite nicht sich an Entwickler richtet und Programmierung Sprachversionen nicht relevant ist, wählen Sie **NA aus** **Rest-api** REST API-Referenzthemen zu verwenden.

 - **cpp**
 - **dotnet**
 - **Java**
 - **JavaScript**
 - **mehrere**: Seite gilt mehrere Programmiersprachen.
 - **Na**: der Entwickler nicht abzielt und ist nicht für alle Sprachen.
 - **nodejs**
 - **Ziel c**
 - **PHP**
 - **Python**
 - **Rest-api**
 - **Ruby**


![](./media/article-metadata/checkmark-small.png)**ms.Topic**: erforderlich. Geben Sie Einzelheiten zum Thema. Die meisten neue Seiten von Contributors werden Artikel oder Verweis.

 - **Artikel**: Thema, Lernprogramm, Leistungsmerkmale oder andere Referenzartikel

 - **Kampagne-Seite**: nur Azure.com.  Eine Seite, die speziell als Zielseite für externe Kampagnen und ist nicht Bestandteil des primären Standorts IA  Sollte nicht für Dokumentation Artikel oder regulären Doc Zielseiten verwendet werden.  Beispiele: azure.microsoft.com/develop/net/aspnet/; Azure.Microsoft.com/Develop/Mobile/IOS/

 - **Entwickler-Center-Homepage**: nur Azure.com.  Ein Entwickler z. B. center-Homepage/Entwicklung/net

 - **Get gestartet Artikel**: im Abschnitt Erste Schritte oder Übersicht Navigationsleiste für einen Dienst gehörenden Artikel zuweisen.

 - **Artikel Held**: ein Lernprogramm "Helden", die Einführung eines Dienstes oder Features, die entwickelt Besucher schnell mit dem Dienst gestartet und kostenlose Anmeldungen und MSDN Aktivierungen Laufwerke. Dieser Wert nur Artikel am oberen Rand der Zielseite Dokumentation für Ihren Dienst erscheinen.

 - **Homepage**: Homepage Top Level-Dokumentation. Wir haben nur zwei: azure.microsoft.com/documentation/ und msdn.microsoft.com/library/azure/

 - **Index-Seite**: zweiter Ebene Zielseiten für Sprachen, Dienste oder Funktionen programmieren. Diese Azure.com und die Bibliothek verteilt werden und dienen als Einstiegspunkte bewertete spezifischere Informationen. Beispiele: http://azure.microsoft.com/develop/mobile/resources-wp8/, http://msdn.microsoft.com/library/azure/jj673460.aspx, http://msdn.microsoft.com/library/azure/hh689864.aspx

 - **INFOGRAFIK Seite**: nur Azure.com. Eine Seite, die aus einem durchsuchbaren INFOGRAFIK oder Poster, z.B. http://azure.microsoft.com/documentation/infographics/windows-azure/

 - **Hinweis**: ein API-Referenzseite (einschließlich REST-API) oder PowerShell-Cmdlet-Referenzseite

 - **Service-Homepage**: nur Azure.com.  Ein Dokument Homepage, z. B. /documentation/services/virtual-machines /

 - **Site-Abschnitt-Homepage**: nur Azure.com. Eine "Homepage" für einen bestimmten Inhalt azure.com Beispiele: http://azure.microsoft.com/documentation/infographics/, http://azure.microsoft.com/documentation/scripts/, http://azure.microsoft.com/documentation/videos/home/, http://azure.microsoft.com/downloads/

 - **Video-Seite**: nur Azure.com.  Eine Seite, die aus Videos, z.B. http://azure.microsoft.com/documentation/videos/azure-webjobs-hosting-testing-net/

![](./media/article-metadata/checkmark-small.png)**ms.tgt_pltfrm**: erforderlich. Gibt die Zielplattform beispielsweise Windows, Linux, Windows Phone, iOS, Android oder speziellen Zwischenspeicher Plattformen. Ein Wert pro Seite. Dieser Wert wird für die meisten Themen außer Mobile und virtuelle Maschinen **NA** sein.

 - **Cache-Funktion**
 - **Cache-mehrfach**
 - **Cache-redis**
 - **Cache-service**
 - **Cache freigegeben**
 - **Command line interface**
 - **Ibiza**: Inhalt, Ibiza Portal verwendet. Verwenden Sie diese nur in die Funktion diskutiert Ibiza Portal und aktuelle Portal verfügbar ist.
 - **Mobile Android**: nur jetzt Azure.com
 - **Mobile html**: nur jetzt Azure.com
 - **Mobile Ios**: nur jetzt Azure.com
 - **Mobile Kindle**: nur jetzt Azure.com
 - **mehrere Mobile**
 - **Mobile-Nokia-X**: Azure.com jetzt nur
 - **Mobile Phonegap**: nur jetzt Azure.com
 - **Mobile Sencha**: nur jetzt Azure.com
 - **Windows Mobile**: Azure.com nur jetzt; Universal Windows
 - **Windows-Mobiltelefon**
 - **Mobile Windows store**
 - **Mobile Xamarin**: Azure.com nur jetzt; Xamarin alle Plattformen
 - **Mobile Xamarin Android**: nur jetzt Azure.com
 - **Mobile Xamarin Ios**: nur jetzt Azure.com
 - **mehrere**: Seite gilt für mehrere Plattformen
 - **Na**: ein Plattformbezeichner gilt nicht für diese Seite
 - **PowerShell**
 - **VM-linux**
 - **mehrere VM**
 - **VM-Fenster**
 - **VM Windows sharepoint**
 - **VM-Windows-Sql-server**
 - **VS-erste Schritte**: identifiziert die Seitengruppe VS Einstieg. Tag 1/12/14 hinzugefügt.
 - **VS-was-passiert**: bezeichnet den VS erste Schritte geschehen. Tag 1/12/14 hinzugefügt.

![](./media/article-metadata/checkmark-small.png)**ms.Workload**: erforderlich. Gibt die Azure Arbeitslast, der auf die Seite angewendet wird. Ein Wert pro Artikel.

**8/6/15 aktualisieren** Der Wert ms.workload wird durch eine Xls, nicht den Wert in der Datei .md zugeordnet. Ms.workload-Wert ist für die Validierung erforderlich, bis die Funktion aktualisiert werden kann. Diese Arbeit ist jetzt geplant.  Verwenden Sie **"Na"** als Wert für jetzt.

![](./media/article-metadata/checkmark-small.png)**ms.Date**: erforderlich. Gibt das Datum der letzten überprüft Artikel Relevanz, Genauigkeit, richtige Screenshots und funktionierende Links. Geben Sie das Datum im Format tt/mm/jjjj. Dieses Datum wird auch auf der veröffentlichten Artikel als Datum der letzten Aktualisierung.

![](./media/article-metadata/checkmark-small.png)**ms.Author**: erforderlich. Gibt die Autoren das Thema zugeordnet. Interne Berichte (z. B. frische) verwenden Sie diesen Wert rechts Autoren Artikel zuordnen. Um mehrere Werte angeben, sollten sie durch Semikola getrennt werden. Microsoft Aliase oder vollständige e-Mail-Adressen sind zulässig. Die Länge kann nicht mehr als 200 Zeichen sein.


###<a name="contributors-guide-links"></a>Mitwirkenden Guide Links

- [Übersichtsartikel](./../README.md)
- [Index der Artikel](./contributor-guide-index.md)


<!--Anchors-->
[Syntax]: #syntax
[Verwendung]: #usage
[Attribute und Werte für Eigenschaftenabschnitt]: #attributes-and-values-for-the-properties-section
[Attribute und Werte zum Abschnitt tags]: #attributes-and-values-for-the-tags-section
