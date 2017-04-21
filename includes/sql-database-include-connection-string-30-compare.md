
<!--
includes/sql-database-include-connection-string-30-compare.md

Latest Freshness check:  2015-09-03 , GeneMi.

## Connection string
-->


### <a name="compare-the-connection-string"></a>Vergleichen Sie die Verbindungszeichenfolge


Die folgende Tabelle vergleicht die Verbindungszeichenfolgen, die das C#-Programm Verbindung zu Ihrem lokalen SQL Server gegenüber der Azure SQL-Datenbank in der Cloud muss. Die Unterschiede sind fett dargestellt.


| Verbindungszeichenfolge für<br/>SQL Azure-Datenbank | Verbindungszeichenfolge für<br/>Microsoft SQL Server |
| :-- | :-- |
| Server =**Tcp:**{Your_serverName_here}**. database.windows.net,1433**;<br/>Benutzer-ID = {Your_loginName_here}**@{your_serverName_here}**;<br/>Kennwort = {Your_password_here};<br/>**Datenbank = {Your_databaseName_here};**<br/>**Connection Timeout = 30**;<br/>**Verschlüsselt = True**;<br/>**TrustServerCertificate = False**; | Server = {Your_serverName_here};<br/>Benutzer-ID = {Your_loginName_here};<br/>Kennwort = {Your_password_here}; |


Die **Datenbank =** ist optional für SQL Server jedoch für SQL-Datenbank erforderlich.


[.NET ADO SqlConnectionStringBuilder-Eigenschaften](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder_properties.aspx) - werden alle Parameter ausführlich erläutert.


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
