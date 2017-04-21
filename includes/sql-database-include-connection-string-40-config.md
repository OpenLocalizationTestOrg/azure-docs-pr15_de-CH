
<!--
includes/sql-database-include-connection-string-40-config.md

Latest Freshness check:  2015-09-04 , GeneMi.

## Connection string
-->


### <a name="example-config-file-for-connection-string-security"></a>Beispiel-Konfigurationsdatei für verbindungssicherheit


Falsche zu der Verbindungszeichenfolge als Literale in der C#-Code ist. Es ist besser, die Verbindungszeichenfolge in einer Konfigurationsdatei. Es können Sie jederzeit ohne Neukompilierung die Zeichenfolge bearbeiten.

Angenommen Ihre kompilierte C#-Programm heißt **ConsoleApplication1.exe**und diese .exe in befindet sich ein **Bin\debug\* * Verzeichnis.

In diesem Beispiel werden die meisten Teile der Verbindungszeichenfolge in einer Konfigurationsdatei mit dem Namen genau **ConsoleApplication1.exe.config**gespeichert. Diese Konfigurationsdatei muss auch befinden sich in **Bin\debug\**.

In der folgenden Konfigurationsdatei XML sehen Sie eine Verbindungszeichenfolge mit Namen **ConnectionString4NoUserIDNoPassword**. Der C#-Code sieht für diese Zeichenfolge.

Bearbeiten Sie die tatsächlichen Namen in die Platzhalter:

- {Your_serverName_here}
- {Your_databaseName_here}



        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
        
            <connectionStrings>
                <clear />
                <add name="ConnectionString4NoUserIDNoPassword"
                providerName="System.Data.ProviderName"
        
                connectionString=
                "Server=tcp:{your_serverName_here}.database.windows.net,1433;
                Database={your_databaseName_here};
                Connection Timeout=30;
                Encrypt=True;
                TrustServerCertificate=False;" />
            </connectionStrings>
        </configuration>



Abbildung haben wir zwei Parameter auslassen:

- Benutzer-ID = {dein_nutzername_hier};
- Kennwort = {Your_password_here};


Zählen sie, aber es ist besser, das Programm diese Werte von Tastatureingaben vom Benutzer erhalten haben. Es kommt.



<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
