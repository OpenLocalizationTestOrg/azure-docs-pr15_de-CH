<properties
   pageTitle="Verbinden mit SQL Azure Datawarehouse | Microsoft Azure"
   description="Wie Sie den Server und die Verbindungszeichenfolge Zeichenfolge für Ihre Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/26/2016"
   ms.author="sonyama;barbkess"/>

# <a name="connect-to-azure-sql-data-warehouse"></a>Verbinden Sie mit SQL Azure Datawarehouse

Dieser Artikel unterstützt Sie mit SQL Data Warehouse zum ersten Mal verbunden.

## <a name="find-your-server-name"></a>Suchen Sie den Namen des Servers

Der erste Schritt beim Herstellen einer Verbindung mit SQL Data Warehouse ist so finden Sie den Namen des Servers kennen.  Der Servername im folgenden Beispiel wird z. B. sample.database.windows.net. So finden Sie den vollständigen Servernamen:

1. Zum [Azure-Portal][].
2. Klicken Sie auf **SQL-Datenbanken** 
3. Klicken Sie auf die Datenbank, die, der Sie herstellen möchten.
4. Suchen Sie den vollständigen Servernamen ein.

    ![Vollständigen Servernamen][1]

## <a name="supported-drivers-and-connection-strings"></a>Unterstützte Treiber und Verbindungszeichenfolgen

Azure SQL Data Warehouse unterstützt [ADO.NET][], [ODBC][], [PHP][]und [JDBC][]. Klicken Sie auf der vorherigen Treiber auf die neueste Version und Dokumentation finden. Um die Verbindungszeichenfolge für die Treiber von Azure-Portal automatisch generieren, können Sie die **Datenbank-Verbindungszeichenfolgen anzeigen** aus dem vorherigen Beispiel klicken.  Beispiele für eine Verbindungszeichenfolge für jeden Treiber sieht folgen auch.

> [AZURE.NOTE] Erwägen Sie das Verbindungstimeout zu 300 Sekunden Ihre Verbindung zu kurze Zeit nicht verfügbar.

### <a name="adonet-connection-string-example"></a>Beispiel einer Verbindungszeichenfolge ADO.NET

```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>Beispiel für eine ODBC-Verbindungszeichenfolge

```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>Beispiel einer Verbindungszeichenfolge PHP

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Beispiel einer Verbindungszeichenfolge JDBC

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Verbindung

SQL Data Warehouse standardisiert Einstellungen in Verbindung und erstellen. Diese Einstellung können nicht überschrieben werden und umfassen:

| Datenbankeinstellung       | Wert                        |
| :--------------------- | :--------------------------- |
| [ANSI_NULLS][]         | AUF                           |
| [QUOTED_IDENTIFIERS][] | AUF                           |
| [DATEFORMAT][]         | mdy                          |
| [DATEFIRST][]          | 7                            |

## <a name="next-steps"></a>Nächste Schritte

Verbinden und Abfragen mit Visual Studio finden Sie unter [Abfrage mit Visual Studio][]. Weitere Informationen zu Authentifizierungsoptionen finden Sie unter [Authentifizierung in Azure SQL Data Warehouse][].

<!--Articles-->
[Abfrage mit Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Authentifizierung in SQL Azure Datawarehouse]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure-portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


