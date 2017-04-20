<properties
   pageTitle="Problembehandlung bei Microsoft Power BI eingebettete Vorschau"
   description="Problembehandlung bei Microsoft Power BI eingebettete Vorschau"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Problembehandlung bei Microsoft Power BI eingebettete Vorschau
Dieser Artikel enthält Antworten für **Power BI eingebettete**Behandlung.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>SQL Server-Verbindungszeichenfolgen festlegen
Um eine SQL Server-Verbindung Zeichenfolge festzulegen, müssen Sie einem bestimmten Format. Es folgt eine Beispiel-Verbindungszeichenfolge für SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Erfahren Sie mehr über SQL Server-Verbindungszeichenfolgen finden Sie in folgenden Artikeln:

-   [SQL Server-Verbindungszeichenfolgen](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Festlegen von Anmeldeinformationen
In dem Fall Anmeldeinformationen für eine Entwicklungs- oder staging-Umgebung, wie Benutzername und Kennwort müssen Sie Anmeldeinformationen aktualisieren, die eine entsprechen.

## <a name="see-also"></a>Siehe auch
- [Erste Schritte mit](power-bi-embedded-get-started-sample.md)
- [Was ist Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)
