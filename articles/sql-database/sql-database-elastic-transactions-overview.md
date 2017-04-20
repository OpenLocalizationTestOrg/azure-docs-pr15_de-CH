<properties
   pageTitle="Verteilte Transaktionen über Cloud-Datenbanken"
   description="Übersicht über elastische Datenbanktransaktionen mit SQL Azure-Datenbank"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jhubbard"
   editor="torsteng"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="sql-database"
   ms.date="05/27/2016"
   ms.author="torsteng"/>

# <a name="distributed-transactions-across-cloud-databases"></a>Verteilte Transaktionen über Cloud-Datenbanken

Elastische Datenbanktransaktionen für Azure SQL Datenbank (SQL) können Sie Transaktionen ausführen, die mehrere Datenbanken in der SQL-Datenbank umfassen. Elastische Datenbanktransaktionen für SQL-Datenbank stehen für .NET Applications mit ADO .NET und Programmierung vertraut mit [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) -Klassen integriert. Die Bibliothek finden Sie unter [.NET Framework 4.6.1 (Web Installer)](https://www.microsoft.com/download/details.aspx?id=49981).

Lokal benötigt ein Szenario normalerweise Microsoft Distributed Transaction Coordinator (MSDTC) ausgeführt. Da MSDTC nicht für Anwendung in Azure-Plattform als Dienst verfügbar ist, wurde die Möglichkeit zur Koordinierung verteilter Transaktionen jetzt direkt in SQL integriert. Applikationen können mit einer SQL-Datenbank verteilte Transaktionen starten und die Datenbank transparent koordiniert verteilte Transaktion wie in der folgenden Abbildung dargestellt. 

  ![Verteilte Transaktionen mit Azure SQL-Datenbank mit elastische Datenbanktransaktionen ][1]

## <a name="common-scenarios"></a>Häufige Szenarien

Elastische Datenbanktransaktionen für SQL DB aktivieren Applications in mehrere SQL-Datenbanken gespeicherten Daten atomar ändern. Die Vorschau konzentriert sich auf clientseitige Entwicklung in C# und .NET. Eine serverseitige Erfahrung mit T-SQL ist für einen späteren Zeitpunkt geplant.  
Elastische Datenbanktransaktionen richtet die folgenden Szenarien:

* Mit mehreren Datenbanken Anwendung in Azure: in diesem Szenario vertikal Datenpartitionierung über mehrere Datenbanken in der SQL-Datenbank, verschiedene Arten von Daten in verschiedenen Datenbanken befinden. Einige Vorgänge erfordern Änderungen an Daten, die in zwei oder mehr Datenbanken gespeichert. Die Anwendung verwendet elastische Datenbanktransaktionen Koordinieren der ändert sich in Datenbanken und Unteilbarkeit sicherzustellen.

* Sharding ergeben in Azure: in diesem Szenario verwendet die Datenebene [elastische Datenbank-Clientbibliothek](sql-database-elastic-database-client-library.md) oder selbst Sharding horizontal partitionieren die Daten in vielen Datenbanken in der SQL-Datenbank. Ein herausragender Anwendungsfall muss Sharding Multi-Tenant-Anwendung bei Änderung Mieter umfassen atomare Änderungen durchgeführt. Stellen Sie sich beispielsweise eine Übertragung von einem Mandanten in einen anderen beiden, die auf verschiedene Datenbanken. Zweite ist abgestimmte Sharding Kapazitätsbedarf für große Mieter aufzunehmen, die wiederum in der Regel bedeutet, dass einige atomare Vorgänge zum gleichen Mieter Datenbanken erstreckt. Dritte ist atomar Updates auf Daten verweisen, die über Datenbanken hinweg repliziert werden. Atomare Operationen durchgeführte, dementsprechend können jetzt über mehrere Datenbanken in der Vorschau koordiniert werden.
Elastische Datenbanktransaktionen verwenden Zweiphasencommit Atomizität in Datenbanken sicherzustellen. Für Transaktionen, die weniger als 100 Datenbanken innerhalb einer einzelnen Transaktion geeignet ist. Diese Grenzwerte werden nicht erzwungen, jedoch sollte man Leistung und Erfolgsraten elastische Datenbanktransaktionen beeinträchtigt, wenn diese Grenzwerte überschritten.


## <a name="installation-and-migration"></a>Installation und migration

Funktionen für elastische Datenbanktransaktionen in SQL DB werden durch Updates Proxybibliothek System.Data.dll und System.Transactions.dll bereitgestellt. Die DLLs sicherzustellen, Zweiphasencommit gegebenenfalls Ablauf verwendet wird. Starten der Anwendungsentwicklung elastische Datenbanktransaktionen mit installieren Sie [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) oder höher. Unter einer früheren Version von .NET Framework Transaktionen nicht zu einer verteilten Transaktion heraufgestuft und eine Ausnahme ausgelöst.

Nach der Installation können Sie die verteilte Transaktion APIs in System.Transactions mit SQL DB verwenden. Haben Sie vorhandene MSDTC Applikationen diese APIs einfach neu zu erstellen Ihre vorhandenen Applikationen für .NET 4.6 nach der Installation der 4.6.1 Framework. Wenn Ihre .NET 4.6-Projekte verwenden sie automatisch die aktualisierten DLLs die neue Frameworkversion und verteilte Transaktion zusammen mit SQL DB API-Aufrufe jetzt erfolgreich.

Beachten Sie, dass elastische Datenbanktransaktionen Installation von MSDTC nicht benötigen. Stattdessen werden elastische Datenbanktransaktionen und im SQL-DB direkt verwaltet. Dies vereinfacht erheblich Cloudszenarien, da Bereitstellung MSDTC nicht verteilte Transaktionen mit SQL DB verwenden. Abschnitt 4 erläutert ausführlich elastische Datenbanktransaktionen und erforderliche .NET Framework zusammen mit Ihrem Cloudanwendungen in Azure bereitstellen.

## <a name="development-experience"></a>Erfahrung in der Entwicklung

### <a name="multi-database-applications"></a>Mit mehreren Datenbanken

Der folgende Beispielcode verwendet die Programmierung vertraut mit .NET System.Transactions. Die TransactionScope-Klasse stellt eine Ambiente-Transaktion in .NET. (Eine "Ambiente-Transaktion" gehört, die im aktuellen Thread befindet.) Alle Verbindungen innerhalb der TransactionScope teilnehmen Transaktion. Wenn verschiedene Datenbanken teilnehmen, wird die Transaktion automatisch zu einer verteilten Transaktion erhöht. Das Ergebnis der Transaktion erfolgt durch Festlegen des Bereichs an ein Commit abgeschlossen.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Sharding Datenbanken
 
Elastische Datenbanktransaktionen für SQL DB unterstützen auch die Koordination verteilter Transaktionen, in denen Sie mit OpenConnectionForKey-Methode der Clientbibliothek elastische Datenbank Verbindungen für einen skalierten, Tier. Betrachten Sie müssen Transaktionskonsistenz Suchen über verschiedene Werte für verschiedene Sharding gewährleistet. Verbindung zum Hosten unterschiedlichen Sharding Schlüsselwerte Splitter sind mit OpenConnectionForKey vermittelt. Im Allgemeinen können Verbindungen zu anderen Splitter sein transaktionale Garantien sicherstellen eine verteilte Transaktion erfordert. Das folgende Codebeispiel veranschaulicht diesen Ansatz. Es wird davon ausgegangen, dass eine Variable namens Shardmap mit der Clientbibliothek elastische Datenbank eine shardzuordnung darstellen:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }
        
        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>Installation von .NET für Azure Cloud Services

Azure bietet mehrere Angebote auf Host. Ein Vergleich der verschiedenen Angebote ist in [Azure App Service, Cloud-Diensten und virtuellen Maschinen Vergleich](../app-service-web/choose-web-site-cloud-service-vm.md). Wenn das Gastbetriebssystem des Angebots kleiner als .NET 4.6.1 elastische Transaktionen erforderlich ist, müssen Sie das Gastbetriebssystem 4.6.1 aktualisieren. 

Azure App Services sind Upgrades Gastbetriebssystem derzeit nicht unterstützt. Azure Virtual Machines einfach melden Sie die VM und führen Sie das Installationsprogramm für die neuesten .NET Framework. Für Azure Cloud Services müssen Sie die Installation einer neueren Version von .NET in Startaufgaben der Bereitstellung. Die Konzepte und Schritte sind in [.NET eine Cloud Service-Rolle installieren](../cloud-services/cloud-services-dotnet-install-dotnet.md)dokumentiert.  

Beachten Sie, dass das Installationsprogramm für .NET 4.6.1 bootstrapping bei Azure Cloud Services als das Installationsprogramm für .NET 4.6 temporären Speicherplatz erfordern. Um eine erfolgreiche Installation sicherzustellen, müssen Sie temporären Speicher für Ihren Azure-Cloud-Dienst in der Datei ServiceDefinition.csdef in Abschnitt LocalResources und der Umgebung die Startaufgabe erhöhen wie im folgenden Beispiel gezeigt:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>
    
## <a name="transactions-across-multiple-servers"></a>Transaktionen über mehrere Server

Elastische Datenbanktransaktionen werden auf verschiedene logische Server in Azure SQL-Datenbank unterstützt. Wenn Transaktionen logischen Server Grenzen, müssen der beteiligten Server eine gegenseitige kommunikationsbeziehung eingegeben werden. Sobald die Kommunikation Beziehung, kann elastische Transaktionen mit Datenbanken auf dem Server eine Datenbank in zwei Servern teilnehmen. Eine kommunikationsbeziehung muss mit Transaktionen in mehrere logische Server ein Paar von logischen Servern werden.

Mithilfe der folgenden PowerShell-Cmdlets serverübergreifende kommunikationsbeziehungen elastische Datenbanktransaktionen verwalten:

* **Neu AzureRmSqlServerCommunicationLink**: dieses Cmdlet verwenden, um eine neue Kommunikation zwischen zwei logischen Servern in Azure SQL-Datenbank erstellen. Die Beziehung ist symmetrisch, dass beide Server mit dem Server initiieren können.
* **Get-AzureRmSqlServerCommunicationLink**: Verwenden Sie dieses Cmdlet bestehenden Kommunikation und deren Eigenschaften abrufen.
* **Entfernen AzureRmSqlServerCommunicationLink**: dieses Cmdlet verwenden, um eine vorhandene kommunikationsbeziehung entfernen. 


## <a name="monitoring-transaction-status"></a>Überwachen der Buchungsstatus

Verwenden Sie dynamische Verwaltungsansichten (DMVs) in SQL DB Monitorstatus und Fortschritt der laufenden elastische Datenbanktransaktionen. Alle Transaktionen bezogene DMVs gelten für verteilte Transaktionen in der SQL-Datenbank. Sie finden hier die entsprechende Liste von DMVs: [verknüpfte dynamische Verwaltungsansichten Transaktion und Funktionen (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).
 
Diese DMVs sind besonders hilfreich:

* **sys.dm\_Tran\_active\_Transaktionen**: Listet aktive Transaktionen und deren Status. Die UOW (Unit Of Work) Spalte kann verschiedenen untergeordneten Transaktionen zu identifizieren, die die gleichen verteilten Transaktion gehören. Alle Transaktionen innerhalb einer verteilten Transaktion ausführen derselben UOW-Wert. Die [DMV Dokumentation](https://msdn.microsoft.com/library/ms174302.aspx) anzeigen
* **sys.dm\_Tran\_Datenbank\_Transaktionen**: zusätzliche Informationen wie Platzierung der Buchung im Protokoll. Die [DMV Dokumentation](https://msdn.microsoft.com/library/ms186957.aspx) anzeigen
* **sys.dm\_Tran\_Sperren**: enthält Informationen zu sperren, die derzeit laufende Transaktionen reserviert sind. Die [DMV Dokumentation](https://msdn.microsoft.com/library/ms190345.aspx) anzeigen

## <a name="limitations"></a>Grenzen 

Die folgenden Nachteile gelten gegenwärtig elastische Transaktionen in der SQL-Datenbank:

* Nur Transaktionen in Datenbanken in der SQL-Datenbank werden unterstützt. Andere [X / Open XA](https://en.wikipedia.org/wiki/X/Open_XA) Ressourcen und Datenbanken außerhalb SQL DB elastische Datenbanktransaktionen beteiligt. Dies bedeutet, dass elastische Datenbanktransaktionen vor SQL Server und Azure SQL-Datenbanken erstreckt nicht möglich. Für verteilte Transaktionen auf dem Gelände weiterhin MSDTC. 
* Nur Client koordiniert Transaktionen eine werden unterstützt. Serverseitige Unterstützung für T-SQL, wie BEGIN DISTRIBUTED TRANSACTION ist geplant, aber noch nicht verfügbar. 
* Nur Datenbanken auf Azure SQL DB V12 werden unterstützt.
* Transaktionen über WCF-Dienste werden nicht unterstützt. Beispielsweise haben Sie eine WCF-Dienstmethode, die eine Transaktion ausgeführt wird. Einschließen des Aufrufs innerhalb eines Transaktionsbereichs schlägt als [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Verwenden Sie noch elastische Datenbankfunktionen für Ihre Azure? Sehen Sie sich unsere [Wegweiser für die Dokumentation](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). Bei Fragen wenden Sie sich an uns [SQL Datenbank Forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) und Wünsche, fügen sie das [Bewertungsportal SQL-Datenbank](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



