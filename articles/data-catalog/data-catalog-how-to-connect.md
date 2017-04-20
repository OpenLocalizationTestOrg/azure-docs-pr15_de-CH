<properties
   pageTitle="Herstellen einer Verbindung zu Datenquellen | Microsoft Azure"
   description="Gewusst wie-Artikel hervorheben Verbindung zu Datenquellen mit Azure Data Catalog entdeckt."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="09/15/2016"
   ms.author="maroche"/>


# <a name="how-to-connect-to-data-sources"></a>Herstellen einer Verbindung zu Datenquellen

## <a name="introduction"></a>Einführung
**Microsoft Azure Data Catalog** ist eine vollständig verwaltete Cloud-Dienst, der als ein System der Registrierung und Entdeckung für Enterprise-Datenquellen fungiert. Also darum **Azure Data Catalog** Menschen entdecken, verstehen und Verwenden von Datenquellen und helfen Unternehmen mehr Nutzen aus vorhandenen Daten. Ein wichtiger Aspekt dieses Szenarios mithilfe der Daten – Benutzer erkennt eine Datenquelle und ihren Zweck verstehen, ist der nächste Schritt Verbindung mit der Datenquelle die Daten verwenden.

## <a name="data-source-locations"></a>Daten-Quellspeicherort
Während der Registrierung Datenquelle empfängt **Azure Data Catalog** Metadaten der Datenquelle. Diese Metadaten enthält Details zu den Quellspeicherort. Die Details des Speicherorts variieren von Datenquelle Datenquelle, jedoch enthält immer die Verbindung erforderliche Informationen. Der Speicherort für eine SQL Server-Tabelle enthält z. B. Servername, Datenbankname, Schemanamen und Tabellennamen während der Speicherort für SQL Server Reporting Services-Bericht den Servernamen und den Pfad zum Bericht enthält. Andere Arten von Datenquellen haben Standorte, die die Struktur und die Funktionen des Quellsystems widerspiegeln.

## <a name="integrated-client-tools"></a>Integrierte Client-tools
Ist die einfachste Möglichkeit, eine Verbindung mit der "im..." Menü in **Azure Data Catalog** -Portal. Dieses Menü zeigt eine Liste der Optionen für die Verbindung mit der ausgewählten Daten Anlage.
Mit der Kachelansicht Standard wird dieses Menü auf jeder Kachel.

 ![Öffnen einer SQL Server-Tabelle in Excel Daten Anlage Kachel](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Verwendung die Listenansicht steht im Menü in der Suchleiste am oberen Fensterrand Portal.

 ![SQL Server Reporting Services-Bericht öffnen im Berichts-Manager über die Suchleiste](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Unterstützten Clientanwendungen
Bei Verwendung der "Open in..." Menü für Datenquellen in Azure Data Catalog-Portal die richtige Clientanwendung muss auf dem Clientcomputer installiert.

| Anwendung öffnen | Erweiterung der Datei / Protokoll | Unterstützte Anwendungsversionen |
| --- | --- | --- |
| Excel | ODC | Excel 2010 oder höher |
| Excel (Top 1000) | ODC | Excel 2010 oder höher |
| Power-Abfrage | XLSX | Excel 2016 oder Excel 2010 oder Excel 2013 Power Query für Excel-add-in installiert
| Power BI Desktop | .pbix | Power BI Desktop Juli 2016 |
| SQL Server Data Tools | Vsweb: / / | Visual Studio 2013 Update 4 oder höher mit SQL Server installiert |
| Berichts-Manager | http:// | [Browseranforderungen für SQL Server Reporting Services](https://technet.microsoft.com/en-us/library/ms156511.aspx) finden Sie unter |

## <a name="your-data-your-tools"></a>Die Daten der tools
Im Menü verfügbaren Optionen hängen vom Typ der ausgewählten Daten Anlage. Natürlich nicht alle Mittel berücksichtigt der "im..." Menü, aber es ist problemlos Client Werkzeug Datenquelle herstellen. Wenn eine Datenressource in **Azure Data Catalog** -Portal aktiviert ist, wird der vollständige Speicherort im Eigenschaftenbereich angezeigt.

 ![Verbindungsinformationen für eine SQL Server-Tabelle](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

Informationen Verbindung unterscheidet sich von den Datenquellentyp an Datenquellentyp, aber die Informationen im Portal erhalten Sie alles mit den Daten in jedem Clienttool. Benutzer können die Verbindungsdetails für Datenquellen, die sie mit **Azure Data Catalog**, um die Daten in ihre Auswahl-Tool ermittelt haben.

## <a name="connecting-and-data-source-permissions"></a>Herstellen und Daten-Quelle-Berechtigungen
Obwohl **Azure Data Catalog** Datenquellen sichtbar macht, ist Zugriff auf die Daten von Data Source Besitzer oder Administrator gesteuert. Entdecken eine Datenquelle in **Azure Data Catalog** gibt Benutzer Berechtigungen Zugriff auf die Datenquelle selbst.

Um Benutzern erleichtern, entdecken Sie eine Datenquelle jedoch keinen Zugriff auf die Daten, können Benutzer in Access Request-Eigenschaft beim Bereitstellen eine Datenquelle hinzufügen. Informationen, einschließlich Links zu Prozess oder Ansprechpartner für Data Source Zugang – präsentiert neben die Speicherort-Informationen im Portal.

 ![Verbindungsinformationen Anforderung Zugriff Anweisungen](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

##<a name="summary"></a>Zusammenfassung
Registrieren einer Datenquelle **Azure Data Catalog** macht Daten strukturellen und beschreibenden Metadaten aus der Datenquelle in den Katalogdienst kopiert erkennbar. Sobald eine Datenquelle, erkannt und registriert wurde, Benutzer eine Verbindung mit der Datenquelle von **Azure Data Catalog** -Portal "im..." " Menü oder ihre Daten verwenden Wahl.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit Azure Data Catalog](data-catalog-get-started.md) -Lernprogramm schrittweise Informationen zum Herstellen einer Verbindung mit Datenquellen.
