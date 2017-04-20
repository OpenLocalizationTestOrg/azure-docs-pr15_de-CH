<properties
   pageTitle="DataSource Verbindungen | Microsoft Azure"
   description="Quellcode-Datenkabel für Datenmodelle in Azure Analysis Services beschrieben."
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

# <a name="datasource-connections"></a>DataSource-Verbindungen

Datenmodelle in Azure Analysis Services erfordern unterschiedliche Datenanbieter beim Verbinden mit bestimmten Datenquellen. In einigen Fällen möglicherweise tabellarische Modellen Herstellen einer Verbindung mit Datenquellen über systemeigene Anbieter wie SQL Server Native Client (SQLNCLI11) einen Fehler zurück.

. Wenn im Arbeitsspeicher oder direkte Abfrage Datenmodell, mit einer Cloud-Datenquelle wie Azure SQL-Datenbank verbunden verwenden Sie systemeigene Anbieter als SQLOLEDB, möglicherweise Fehlermeldung angezeigt: **"Provider"SQLNCLI11.1"nicht registriert"**.

Oder haben Sie ein DirectQuery Modell mit lokalen Datenquellen systemeigene Anbieter verwenden möglicherweise Fehlermeldung angezeigt: "Fehler beim Erstellen von OLE DB-Rowset **. Falsche Syntax bei 'LIMIT' "**.

## <a name="data-source-providers"></a>Datenquellenanbieter

Die folgenden Datasource-Anbieter im Speicher unterstützt direkte Abfrage Datenmodelle beim Verbinden mit lokalen oder cloud-Datenquellen:

|               | **Datenquelle**                     | **Im Speicher**                            |  **Abfrage**                                           |
|---------------------------|-------------------------------|---------------------------------------------|---------------------------------------------|
| **Cloud**                     | Azure SQL Datawarehouse      | .NET Framework-Datenanbieter für SQL Server | .NET Framework-Datenanbieter für SQL Server |
|                           | SQL Azure-Datenbank            | .NET Framework-Datenanbieter für SQL Server | .NET Framework-Datenanbieter für SQL Server |
| **Vor Ort** (über Gateway) | SQL Server                    | SQL Server Native Client 11.0               | .NET Framework-Datenanbieter für SQL Server |
|                           |  SQL Server                             | Microsoft OLE DB Provider für SQL Server    |   .NET Framework-Datenanbieter für SQL Server                                          |
|                           |  SQL Server                             | .NET Framework-Datenanbieter für SQL Server |  .NET Framework-Datenanbieter für SQL Server                                           |
|                           | Oracle                        | Microsoft OLE DB Provider für Oracle        | Oracle-Datenanbieter für .NET               |
|                           |  Oracle                             | Oracle-Datenanbieter für .NET               | Oracle-Datenanbieter für .NET                                            |
|                           | Teradata                      | OLE DB Provider für Teradata                | Teradata Datenanbieter für .NET             |
|                           |  Teradata                             | Teradata Datenanbieter für .NET             |  Teradata Datenanbieter für .NET                                            |
|                           | Analytics Plattform-System | .NET Framework-Datenanbieter für SQL Server | .NET Framework-Datenanbieter für SQL Server |


> [AZURE.NOTE] Sicherstellen Sie, dass beim lokalen Gateway mit 64-Bit-Anbieter installiert werden.

Beim Migrieren einer lokalen SQL Server Analysis Services-Tabellenmodell zu Azure Analysis Services kann der Anbieter geändert werden.

**Ein Datenquellenanbieter angeben**

1. In SSDT > **Tabellarischen Modell-Explorer** > **Datenquellen**eine Verbindung mit der Datenquelle klicken und dann auf **Datenquelle bearbeiten**.

2. **Verbindung bearbeiten**klicken Sie auf **Erweitert** , um erweiterte Eigenschaften-Fenster zu öffnen.

3. **Legen Sie erweiterte**Eigenschaften > **Provider**, und wählen Sie dann den entsprechenden Anbieter.

## <a name="impersonation"></a>Identitätswechsel
In einigen Fällen kann es an einen anderen Identitätswechselkonto erforderlich. Identitätswechselkonto kann in SSDT oder SSMS angegeben.

Bei lokalen Datenquellen:

- Wenn SQL-Authentifizierung sollte Identitätswechsel Dienstkonto.
- Wenn Windows-Authentifizierung verwenden festlegen Sie Windows Benutzerkennwort. Für SQL Server wird Windows-Authentifizierung mit einem bestimmten Identitätswechselkonto für Daten im Speicher Modelle unterstützt.

Für Cloud-Datenquellen:

- Wenn SQL-Authentifizierung sollte Identitätswechsel Dienstkonto.


## <a name="next-steps"></a>Nächste Schritte

Sollten Sie lokale Datenquellen haben, das [lokale Gateway](analysis-services-gateway.md)installieren. Weitere Informationen zum Verwalten des Servers im SSDT oder SSMS finden Sie unter [Server verwalten](analysis-services-manage.md).
