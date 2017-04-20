<properties
   pageTitle="Azure Data Catalog unterstützten Datenquellen | Microsoft Azure"
   description="Spezifikation der derzeit unterstützten Datenquellen."
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="jstrauss"
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

# <a name="azure-data-catalog-supported-data-sources"></a>Azure Data Catalog unterstützten Datenquellen

Benutzer von Azure Data Catalog können Metadaten über eine öffentliche API Mausklick veröffentlichen-einmal Registration tool oder web-Portal Informationen direkt in den Katalog Daten manuell eingeben. Das folgende Raster fasst alle Quellen für jeden Katalog heute und publishing-Funktionen unterstützt.  Auch sind die externen Tools, die erfahrungsgemäß Portal "Öffnen in" jede Quelle starten kann. Das zweite Raster im Artikel hat technische Spezifikation der einzelnen Quellen Datenverbindungseigenschaften.


## <a name="list-of-supported-data-sources"></a>Liste der unterstützten Datenquellen

<table>

    <tr>
       <td><b>Datenquellenobjekt</b></td>
       <td><b>API</b></td>
       <td><b>Manuelle Eingabe</b></td>
       <td><b>Registrierungstool</b></td>
       <td><b>Öffnen In Tools</b></td>
       <td><b>Notizen</b></td>
    </tr>

    <tr>
      <td>Azure Data Lake Verzeichnis</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure Data Lake Datei</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Azure-Speicher-Blob</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Verzeichnis der Azure-Speicher</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Tabelle der Azure-Speicher</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>

    <tr>
      <td>BIETET Verzeichnis</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>BIETET Datei</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Struktur der Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Struktur anzeigen</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>MySQL-Ansicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle-Datenbanktabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Oracle-Datenbank anzeigen</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Andere (generische Ressource)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Data Warehouse-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Data Warehouse anzeigen</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Dimension</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services messen</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Analysis Services-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel PowerBI</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server Reporting Services-Bericht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Browser</font></td>
      <td><font size=2>Nur Server im einheitlichen Modus. SharePoint-Modus wird nicht unterstützt.</font></td>
    </tr>

    <tr>
      <td>SQL Server-Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SQL Server-Ansicht</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, PowerBI, SQL Server Data Tools</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata Tabelle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Teradata anzeigen</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP Hana anzeigen</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>PowerBI</font></td>
      <td><font size=2>Berechnung und analytische Ansichten; Attribut-Ansichten werden nicht unterstützt.</font></td>
    </tr>

    <tr>
      <td>DB2-Tabelle</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>DB2 anzeigen</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>Datei</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-Verzeichnis</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>FTP-Datei</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-Bericht</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-Endpunkt</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>HTTP-Datei</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData-Entität</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>OData-Funktion</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>PostgreSQL-Tabelle</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>PostgreSQL anzeigen</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SAP Hana anzeigen</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td> Salesforce-Objekt</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

    <tr>
      <td>SharePoint-Liste </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>

</table>

Benötigen Sie Unterstützung für zusätzliche fordern Sie eine Funktion mit [Azure Data Catalog-Forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


<br>
<br>
## <a name="data-source-reference-specification"></a>Datenquellenangabe Verweis
> [AZURE.NOTE] Spalte "DSL Structure" in der folgenden Tabelle nur Listen Verbindungseigenschaften für Eigenschaftensammlung "Adresse", die von Azure Data Catalog verwendet werden (d. h. Eigenschaftensammlung "Adresse" andere Eigenschaften der Datenquelle Azure Data Catalog beibehalten, aber nicht verwendet möglich.)
<table>
    <tr>
       <td><b>Typ</b></td>
       <td><b>Asset-Typ</b></td>
       <td><b>Objekttypen</b></td>
       <td><b>DSL-Struktur<b></td>
    </tr>
    <tr>
      <td>Azure See Datenspeicher</td>
      <td>Container</td>
      <td>Daten-See</td>
      <td>
        <font size=2>Protokoll: Webhdfs <br>Authentifizierung: {basic Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure See Datenspeicher</td>
      <td>Tabelle</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2>Protokoll: Webhdfs <br>Authentifizierung: {basic Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure-Speicher</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2>Protokoll: Azure-Blobs <br>Authentifizierung: {Azure-Zugriffstaste} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Domäne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Konto <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Container</font>
      </td>
    </tr>
    <tr>
      <td>Azure-Speicher</td>
      <td>Tabelle</td>
      <td>BLOB-Verzeichnis</td>
      <td>
        <font size=2>Protokoll: Azure-Blobs <br>Authentifizierung: {Azure-Zugriffstaste} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Domäne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Konto <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Container <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Name</font>
      </td>
    </tr>
    <tr>
      <td>Azure-Speicher</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2>Protokoll: Azure-Tabellen <br>Authentifizierung: {Azure-Zugriffstaste} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Domäne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Konto</font>
      </td>
    </tr>
    <tr>
      <td>Azure-Speicher</td>
      <td>Tabelle</td>
      <td>Tabelle</td>
      <td>
        <font size=2>Protokoll: Azure-Tabellen <br>Authentifizierung: {Azure-Zugriffstaste} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Domäne <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Konto <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Name</font>
      </td>
    </tr>
    <tr>
      <td>COSMOS</td>
      <td>Container</td>
      <td>Virtuelle Cluster</td>
      <td>
        <font size=2>Protokoll: Cosmos <br>Authentifizierung: {basic Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>COSMOS</td>
      <td>Tabelle</td>
      <td>Stream Stream Satz anzeigen</td>
      <td>
        <font size=2>Protokoll: Cosmos <br>Authentifizierung: {basic Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Container</td>
      <td>Standort</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {none, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>DataZen</td>
      <td>Bericht</td>
      <td>Bericht Dashboard</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {none, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: db2 <br>Authentifizierung: {basic Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2>Protokoll: db2 <br>Authentifizierung: {basic Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema</font>
      </td>
    </tr>
    <tr>
      <td>Dateisystem</td>
      <td>Tabelle</td>
      <td>Datei</td>
      <td>
        <font size=2>Protokoll: Datei <br>Authentifizierung: {none, basic, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Pfad</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tabelle</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2>Protokoll: ftp <br>Authentifizierung: {none, basic, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop verteiltes Dateisystem</td>
      <td>Container</td>
      <td>Cluster</td>
      <td>
        <font size=2>Protokoll: Webhdfs <br>Authentifizierung: {basic Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop verteiltes Dateisystem</td>
      <td>Tabelle</td>
      <td>Verzeichnis, Datei</td>
      <td>
        <font size=2>Protokoll: Webhdfs <br>Authentifizierung: {basic Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Struktur</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Struktur <br>Authentifizierung: {Hdinsight, basic Benutzername keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>ConnectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ServerProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Struktur</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2>Protokoll: Struktur <br>Authentifizierung: {Hdinsight, basic Benutzername keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>ConnectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ServerProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Container</td>
      <td>Standort</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {none, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Bericht</td>
      <td>Bericht Dashboard</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {none, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Http</td>
      <td>Tabelle</td>
      <td>Endpunkt, Datei</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {none, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Mysql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2>Protokoll: Mysql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Container</td>
      <td>Entitätencontainer</td>
      <td>
        <font size=2>Protokoll: Odata <br>Authentifizierung: {none, basic, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tabelle</td>
      <td>Entitätenmenge, Funktion</td>
      <td>
        <font size=2>Protokoll: Odata <br>Authentifizierung: {none, basic, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Ressource</font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Datenbank</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Oracle <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>Oracle-Datenbank</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2>Protokoll: Oracle <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Postgresql <br>Authentifizierung: {basic Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2>Protokoll: Postgresql <br>Authentifizierung: {basic Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Container</td>
      <td>Standort</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {none, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Bericht</td>
      <td>Bericht Dashboard</td>
      <td>
        <font size=2>Protokoll: http <br>Authentifizierung: {none, Basic, Windows, Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Power-Abfrage</td>
      <td>Tabelle</td>
      <td>Daten Mashup</td>
      <td>
        <font size=2>Protokoll: Power-Abfrage <br>Authentifizierung: {Oauth} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Tabelle</td>
      <td>Objekt</td>
      <td>
        <font size=2>Protokoll: Salesforce-com <br>Authentifizierung: {basic Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Klasse <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objektname</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2>Protokoll: Sap-Hana-Sql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </td>
    </tr>
    <tr>
      <td>SAP Hana</td>
      <td>Tabelle</td>
      <td>Ansicht</td>
      <td>
        <font size=2>Protokoll: Sap-Hana-Sql <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Tabelle</td>
      <td>Liste</td>
      <td>
        <font size=2>Protokoll: Sharepoint-Liste <br>Authentifizierung: {basic Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>SQL Datawarehouse</td>
      <td>Befehl</td>
      <td>Gespeicherte Prozedur</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Datawarehouse</td>
      <td>TableValuedFunction</td>
      <td>Tabellenfunktion</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Datawarehouse</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>SQL Datawarehouse</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Befehl</td>
      <td>Gespeicherte Prozedur</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Tabellenfunktion</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2>Protokoll: Tds <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-mehrdimensionale</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Standardauthentifizierung, die anonyme keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-mehrdimensionale</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Standardauthentifizierung, die anonyme keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-mehrdimensionale</td>
      <td>Maßnahme</td>
      <td>Maßnahme</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Standardauthentifizierung, die anonyme keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-mehrdimensionale</td>
      <td>Tabelle</td>
      <td>Dimension</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Standardauthentifizierung, die anonyme keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {Dimension}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabellarisch</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Standardauthentifizierung, die anonyme keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabellarisch</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Standardauthentifizierung, die anonyme keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabellarisch</td>
      <td>Maßnahme</td>
      <td>Maßnahme</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Standardauthentifizierung, die anonyme keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabellarisch</td>
      <td>Tabelle</td>
      <td>Tabelle</td>
      <td>
        <font size=2>Protokoll: Analysis Services <br>Authentifizierung: {Windows, Standardauthentifizierung, die anonyme keine} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekttyp: {Table}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2>Protokoll: reporting Services <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Version: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Bericht</td>
      <td>Bericht</td>
      <td>
        <font size=2>Protokoll: reporting Services <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Pfad <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Version: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Container</td>
      <td>Datenbank</td>
      <td>
        <font size=2>Protokoll: Teradata <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Tabelle</td>
      <td>Tabelle, Ansicht</td>
      <td>
        <font size=2>Protokoll: Teradata <br>Authentifizierung: {Protokoll, Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datenbank <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server-Master Data Services</td>
      <td>Container</td>
      <td>Modell</td>
      <td>
        <font size="2">Protokoll: Mssql Mds <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Version</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server-Master Data Services</td>
      <td>Tabelle</td>
      <td>Entität</td>
      <td>
        <font size="2">Protokoll: Mssql Mds <br>Authentifizierung: {Windows} <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Modell <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Version <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Entität</font>
      </td>
    </tr>
    <tr>
      <td>Andere (nicht genannten)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>Protokoll: generische Ressourcen <br>Adresse: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Posten-Nr</font>
      </td>
    </tr>
</table>
