<properties
   pageTitle="Was ist Azure Analysis Services | Microsoft Azure"
   description="Vollbildmodus von Analysis Services in Azure."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="owend"/>

# <a name="what-is-azure-analysis-services"></a>Was ist Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Basierend auf bewährten Analyse-Engine in Microsoft SQL Server Analysis Services ermöglicht Azure Analysis Services Enterprise-Daten in der Cloud modellieren.

> [AZURE.IMPORTANT] Azure Analysis Services ist in der **Vorschau**. Es gibt einige Dinge, die nur noch funktionieren. Werden Sie auf [Vorschau entsprechend](#preview-expectations) weiter unten in diesem Artikel. Und Sie zu unserem [Blog Azure Analysis Services](https://go.microsoft.com/fwlink/?linkid=830920) die neuesten Informationen.

## <a name="built-on-sql-server-analysis-services"></a>Basiert auf SQL Server Analysis Services
Azure Analysis Services ist kompatibel mit der gleichen SQL Server 2016 Analysis Services Enterprise Edition bereits kennen. Azure Analysis Services unterstützt tabellarische Modellen mit dem Kompatibilitätsgrad 1200. DirectQuery, Partitionen zeilenbasierter Sicherheit, bidirektionale Vertrauensstellungen und Übersetzung werden alle unterstützt.

## <a name="use-the-tools-you-already-know"></a>Verwenden Sie die Tools, die Sie bereits kennen
![BI-Entwicklertools](./media/analysis-services-overview/aas-overview-dev-tools.png)

Beim Erstellen von Datenmodellen für Azure Analysis Services verwenden die gleichen Tools für SQL Server Analysis Services. Erstellen und Bereitstellen von tabellarischen Modellen mit den neuesten Versionen von [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx) oder mithilfe von [Azure Powershell](../powershell-install-configure.md) und [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) Vorlagen in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connect-to-data-sources"></a>Verbinden mit Datenquellen
Datenmodelle auf Servern in Azure Data Sources lokal in Ihrer Organisation oder in der Cloud mit Unterstützung bereitgestellt. Daten aus beiden lokalen und cloud-Datenquellen für eine hybride BI-Lösung.

![Datenquellen](./media/analysis-services-overview/aas-overview-data-sources.png)

Da der Server in der Cloud ist, ist mit Cloud-Datenquellen nahtlose. Beim Verbinden mit lokalen Datenquellen gewährleistet den [lokalen Daten-Gateway](analysis-services-gateway.md) schnelle und sichere Verbindung mit dem Analysis Services-Server in der Cloud.  

 \*Einige Datenquellen werden in der Vorschau noch nicht unterstützt. Weitere finden Sie weiter unten in diesem Artikel [Vorschau erwartet](#preview-expectations) .

## <a name="explore-your-data-from-anywhere"></a>Datenanalyse überall
Verbinden von Servern von überall [Abrufen von Daten](analysis-services-connect.md) . Azure Analysis Services unterstützt die Verbindung von Power BI Desktop, Excel, benutzerdefinierte apps und browserbasierte Tools.

![Daten-Visualisierung](./media/analysis-services-overview/aas-overview-visualization.png)

 \*Power BI eingebettete ist noch nicht in der Vorschau unterstützt.

## <a name="secure"></a>Sichern

#### <a name="user-authentication"></a>Benutzerauthentifizierung
Benutzerauthentifizierung für Azure Analysis Services wird von [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md)behandelt. Beim Anmelden bei Azure Analysis Services-Datenbank verwenden Benutzer Identität einer Organisation mit Zugriff auf die Datenbank, auf die sie zugreifen möchten. Diese Benutzeridentitäten muss Mitglied der Standardwert Azure Active Directory für das Abonnement Azure Analysis Services-Server sich befindet. [Verzeichnisintegration](https://technet.microsoft.com/library/jj573653.aspx) zwischen AAD und lokale Active Directory ist eine hervorragende Möglichkeit, Ihre lokale Benutzer eine Azure Analysis Services-Datenbank zugreifen, aber nicht für alle Szenarios erforderlich.

Benutzer anmelden mit der Benutzerprinzipalname (UPN) ihr Konto und Kennwort. Beim Synchronisieren mit einem lokalen Active Directory ist UPN des Benutzers häufig ihre Organisationseinheit.

Berechtigungen für die Verwaltung der Serverressource Azure Analysis Services erfolgt durch Zuweisen von Benutzern zu Rollen innerhalb Ihrer Azure-Abonnement. Standardmäßig haben Abonnement Administratoren Berechtigungen für die Server-Ressource in Azure. Mithilfe von Azure-Ressourcen-Manager können weitere Benutzer hinzugefügt werden.

#### <a name="data-security"></a>Sicherheit
Azure Analysis Services nutzt Azure BLOB-Speicher Speicher und Metadaten für Analysis Services-Datenbanken bestehen. Dateien in BLOBs werden mit Azure Blob Server Seite Verschlüsselung (SSE) verschlüsselt. Direkte Abfrage Modus werden nur Metadaten gespeichert. die tatsächlichen Daten erfolgt aus der Datenquelle zur Abfragezeit.

#### <a name="on-premises-data-sources"></a>Lokale Datenquellen
Sicherer Zugriff auf Daten verbleiben lokal in Ihrer Organisation installieren und Konfigurieren einer [lokalen Daten-Gateway](analysis-services-gateway.md)erreicht werden können. Gateways bieten Zugriff auf Daten für direkte Abfrage und speicherresidenten Modus. Wenn ein Modell Azure Analysis Services mit einer lokalen Datenquelle herstellt, wird eine Abfrage mit verschlüsselten Anmeldeinformationen für die lokale Datenquelle erstellt. Gateway-Clouddienst analysiert die Abfrage und überträgt die Anforderung auf Azure Service Bus. Das lokale Gateway fragt Azure Service Bus für Anfragen. Das Gateway dann Ruft die Abfrage ab, entschlüsselt die Anmeldeinformationen und stellt eine Verbindung mit der Datenquelle ausgeführt. Die Ergebnisse werden aus der Datenquelle an das Gateway und dann auf Azure Analysis Services-Datenbank gesendet.

[Microsoft Online Services-Begriffe](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) und die [Microsoft Online Services-Datenschutzrichtlinie](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx)Azure Analysis Services unterliegen.
Erfahren Sie mehr über die Sicherheit von Azure finden Sie im [Microsoft-Sicherheitscenter](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="get-help"></a>Hilfe
Azure Analysis Services ist einfach einzurichten und zu verwalten. Sie finden alle Informationen zum Erstellen und Verwalten eines Servers benötigen. Erstellen eines Modells Daten auf Ihrem Server bereitstellen wird dasselbe für ein Datenmodell zu einem lokalen Server bereitgestellten erstellen. Gibt es eine umfangreiche Bibliothek von Grundlagen-, Verfahrens-, Lernprogramme und Referenzartikel [Analysis](https://msdn.microsoft.com/library/bb522607.aspx)Services auf MSDN.

Wir haben auch einige hilfreiche Videos [Azure Analysis](https://channel9.msdn.com/series/Azure-Analysis-Services)Services auf Kanal 9.

Dinge verändern. Sie erhalten stets die neueste Informationen zu [Azure Analysis Services-Blog](https://go.microsoft.com/fwlink/?linkid=830920).

## <a name="community"></a>Community
Analysis Services verfügt über eine umfassende Community von Benutzern. Gesprächsstoff [Azure Analysis Services](https://aka.ms/azureanalysisservicesforum)-Forum.

## <a name="feedback"></a>Feedback
Haben Sie Vorschläge oder Wünsche? Lassen Sie Ihre Kommentare in [Azure Analysis Services Feedback](https://aka.ms/azureanalysisservicesfeedback).

Haben Sie Vorschläge zur Dokumentation? Sie können mit Disqus am unteren Rand jeder Artikel Kommentare hinzufügen.

## <a name="preview-expectations"></a>Vorschau erwartet
Azure Analysis Services ist derzeit in der Vorschau. Es gibt einige Dinge, die, denen Sie beachten sollten.

##### <a name="server-modes"></a>Server-Modi
Azure Analysis Services unterstützt derzeit tabellarischen Modus für tabellarischen Modellen mit dem Kompatibilitätsgrad 1200. Multidimensional und Data Mining-Modus und PowerPivot für SharePoint-Modus werden nicht unterstützt.

##### <a name="data-sources"></a>Datenquellen
Vorschau werden die folgenden Datenquellen in 1200 Tabellenmodelle in Azure Analysis Services-Server bereitgestellt unterstützt.

| **Cloud** | **Vor Ort** |
|--------------|------------|
| SQL-Datenbank | SQL Server |
| SQL Datawarehouse | APS |
|      | Oracle |
|       | Teradata |


### <a name="data-source-providers"></a>Datenquellenanbieter
Datenmodelle in Azure Analysis Services erfordern unterschiedliche Datenanbieter Verbindung zu Datenquellen als Datenmodelle in SQL Server Analysis Services. Anbieter Anforderungen hängt Datenquelle wird in der Cloud oder vor Ort und Typ des Datenmodells. im Speicher oder direkte Abfrage. Weitere anzeigen Sie [Datasource Verbindungen](analysis-services-datasource.md)


### <a name="client-connections"></a>Clientverbindungen
Power BI eingebettete ist noch nicht in der Vorschau unterstützt.

Excel-Arbeitsmappen mit live eine Azure Analysis Services-Server gespeicherten auf OneDrive und SharePoint Online werden nicht unterstützt.



## <a name="next-steps"></a>Nächste Schritte
Jetzt über Azure Analysis Services wissen, ist es Zeit zu beginnen. Erfahren Sie, wie in Azure und [ein Tabellenmodell bereitstellen](analysis-services-deploy.md) , [Erstellen Sie einen Server](analysis-services-create-server.md) .
