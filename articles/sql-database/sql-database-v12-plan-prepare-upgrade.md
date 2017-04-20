<properties
    pageTitle="Planen Sie die Aktualisierung auf SQL Datenbank V12 | Microsoft Azure"
    description="Beschreibt die Vorbereitung und Grenzen bei der Aktualisierung auf Version V12 von Azure SQL-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genemi"/>


# <a name="plan-and-prepare-to-upgrade-to-sql-database-v12"></a>Planen Sie und bereiten Sie der SQL-Datenbank V12 aktualisieren vor


Dieses Thema beschreibt die Planung und Vorbereitung zur Aktualisierung der SQL Azure-Datenbanken Version V11 auf V12 ausgeführt werden müssen.


Ein neues [Azure-Portal](https://portal.azure.com/) ist die Aktualisierung auf V12 unterstützen.


In der folgenden Tabelle werden andere Hilfethemen für V12 aufgeführt.


| Titel und link | Beschreibung des Inhalts |
| :--- | :--- |
| [Neuigkeiten in SQL Datenbank V12](sql-database-v12-whats-new.md) | Beschreibt die Details wie V12 vollständige Parität mit Microsoft SQL Server Azure SQL-Datenbank näher bringt. |
| [Erstellen Sie eine Datenbank in SQL Datenbank V12](sql-database-get-started.md) | Beschreibt die Erstellung einer neuen SQL Azure-Datenbank Version V12. Verschiedene Optionen über nur eine leere Datenbank beschrieben. |


## <a name="plan-ahead"></a>Planen


In den folgenden Unterabschnitten beschrieben Lern- und Entscheidungsprozesse, die Sie berücksichtigen müssen, bevor ein zum Aktualisieren der Datenbank SQL Azure auf V12 Aktionen.

### <a name="version-clarification"></a>Klarstellung der Version


Dieses Dokument bezieht sich auf die Aktualisierung von Version V11 auf V12 Microsoft Azure SQL-Datenbank. Formeller Versionsnummern liegen die folgenden beiden Werte von Transact-SQL-Anweisung gemeldet **auswählen @@version; ** :


- 12.0.2000.8 *(oder etwas höher V12)*
- 11.0.9228.18 *(V11)*
 - Manchmal wurde V11 auch SAWA v2 genannt.

### <a name="service-tier-planning"></a>Service-Tier-Planung


Beginnend mit V12 unterstützt Azure SQL-Datenbank die Dienstebenen Basic, Standard und Premium benannt. Web- und Service-Tier wird auf V12 nicht unterstützt. Daher soll die SQL Azure-Datenbank aktualisieren, die derzeit auf der Web- oder Business Service-Stufe ist, müssen Sie entscheiden, was seine Service-Tier sollte.


Ausführliche Informationen über die Dienstebenen Basic, Standard und Premium finden Sie unter:

- [SQL Datenbank Dienstebenen](sql-database-service-tiers.md)
- [Aktualisieren Sie SQL-Datenbank Online-Business Datenbanken auf neuen Dienstebenen](sql-database-upgrade-server-portal.md)



### <a name="review-the-geo-replication-configuration"></a>Überprüfen Sie die Konfiguration des Geo-Replikation


Wenn die SQL Azure-Datenbank für Geo-Replikation konfiguriert ist, sollten dokumentieren die aktuelle Konfiguration und Geo-Replikation vor der Vorbereitung zur Aktualisierung Aktionen beendet. Nach Abschluss der Aktualisierung müssen Sie Ihre Datenbank Geo-Replikation neu konfigurieren.


Die Strategie ist die Quelle beibehalten und eine Kopie der Datenbank testen.


## <a name="preparation-actions"></a>Aktionen zur Vorbereitung


Nach Abschluss der Planung können Sie Aktionsschritte in den folgenden Abschnitten zur Vorbereitung der Endphase Upgrade ausführen.


Detaillierte Beschreibung der Endphase Upgrade stehen in den Hilfethemen, die oben in diesem Hilfethema verknüpft sind.


### <a name="service-tier-actions"></a>Service-Tier-Aktionen


Der Tarif Web- und Service wird auf V12 nicht unterstützt.


Die V11 Azure SQL-Datenbank bietet einer Web- oder Business, der Aktualisierung Ihrer Datenbank zu einer unterstützten wechseln. Die Aktualisierung empfiehlt eine Ebene, die die Arbeitslast Geschichte der Datenbank entspricht. Allerdings können Sie unterstützte Ebene aus.


Reduzieren die Schritte während der Aktualisierung Ihrer Datenbank V11 vom Web und Geschäftsebene wechseln, vor der Aktualisierung. Dazu können Sie das neue [Azure-Portal](https://portal.azure.com/)verwenden.


Wenn Sie nicht sicher sind, welche Dienstebene zu wechseln sind, möglicherweise S2 auf der Standard-Stufe eine vernünftige erste Wahl. Weniger Ebenen enthalten weniger Ressourcen als Web- und Tier hatte.


### <a name="suspend-geo-replication-during-upgrade"></a>Geo-Replikation während Aktualisierung anhalten


Die Aktualisierung V12 kann nicht ausgeführt werden, wenn Geo-Replikation der Datenbank aktiv ist. Sie müssen zuerst die Datenbank Geo-Replikation Beenden neu konfigurieren.


Nach Abschluss der Aktualisierung können Sie die Datenbank erneut Geo-Replikation konfigurieren.


### <a name="open-ports-on-an-azure-vm-for-client-connectivity"></a>Offene Ports auf Azure-VM für Clientverbindungen


Wenn Ihr Clientprogramm SQL Datenbank V12 verbindet, während der Client auf eine Azure Virtual Machine (VM) ausgeführt wird, müssen Sie folgende Portbereiche auf dem virtuellen Computer öffnen:

- 11000 11999
- 14000 14999


Klicken Sie [hier](sql-database-develop-direct-route-ports-adonet-v12.md) für Informationen über die Ports für die SQL-Datenbank V12. Die Ports werden von Leistungssteigerungen in SQL Datenbank V12 benötigt.


##<a id="limitations"></a>Während und nach der Aktualisierung auf V12 Grenzen


### <a name="portals-for-v12"></a>Portale für V12


Es gibt drei Portale für Azure und jeweils unterschiedliche Funktionen zur SQL-Datenbank V12.


- [http://Portal.Azure.com/](https://portal.azure.com/)<br/>Azure-Portal neu und befindet sich noch in der vorschaustatus. Dieses Portal ist noch nicht zur vollständigen allgemeine Verfügbarkeit. Dieses Portal:
 - Verwalten der V12-Server und Datenbank.
 - Kann die Datenbank V11 auf V12 aktualisieren.


- [http://Manage.windowsazure.com/](http://manage.windowsazure.com/)<br/>Diese Azure-Verwaltungsportal kann schließlich auslaufen. Dieses Portal:
 - Verwalten der V12-Server und Datenbank.
 - Kann die V11 V12 Datenbank *nicht* aktualisieren.


- (http://*IhrServername*. database.windows.net)<br/>Azure SQL-Datenbank-Verwaltungsportal:
 - Kann*nicht* V12 verwalten.


Wir empfehlen Ihnen die Verbindung zu Ihrem SQL Azure-Datenbanken mit Visual Studio 2015 (VS2015). VS2015 können für folgende Aufgaben verwendet:


- Eine Transact-SQL-Anweisung ausführen.
- Um ein Schema zu entwerfen.
- Entwickeln Sie eine Datenbank online oder offline.


Oder stattdessen kostenlos herunterladen [Visual Studio Community 2015](https://www.visualstudio.com/vs/community/)eine voll funktionsfähige Version von VS2015 ist.


In älteren klassischen Azure-Portal, auf der Datenbankseite können Sie **in Visual Studio geöffnet** VS2015 auf Ihrem Computer für die Verbindung mit Ihrem SQL Azure-Datenbank starten klicken.


Eine andere Alternative können SQL Server Management Studio (SSMS) 2014 mit [CU6](http://support.microsoft.com/kb/3031047/) Sie Azure SQL-Datenbank herstellen. Weitere Informationen sind in diesem Blog:<br/>[Tooling Client Updates für Azure SQL-Datenbank](https://azure.microsoft.com/blog/2014/12/22/client-tooling-updates-for-azure-sql-database/).


> [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="limitation-during-upgrade-to-v12"></a>Einschränkung *bei* Aktualisierung V12


V11 Datenbank bleibt während der Aktualisierung auf V12 für den Datenzugriff zur Verfügung. Es gibt noch ein paar Nachteile zu berücksichtigen.


| Einschränkung | Beschreibung |
| :--- | :--- |
| Dauer der Aktualisierung | Die Dauer der Aktualisierung hängt von Größe, Edition und mehrere Datenbanken auf dem Server. Der Aktualisierungsprozess kann Stunden Tage Server besonders für Server, die Datenbanken ausführen:<br/><br/>*Größer als 50 GB oder<br/> * Ein Premium-Service-Ebene<br/><br/>Erstellung neuer Datenbanken auf dem Server während der Aktualisierung kann die Upgrade-Dauer erhöhen. |
| Keine Geo-Replikation | Geo-Replikation wird nicht auf einem Server V12 unterstützt, die derzeit eine Aktualisierung von V11 beteiligt ist. |
| Datenbank ist in der Endphase der Aktualisierung V12 kurzfristig nicht verfügbar | Zum Server V11 gehörenden Datenbanken bleiben während der Aktualisierung verfügbar. Jedoch steht die Verbindung zum Server und Datenbanken kurz in der Endphase die Umstellung von V11 bereit V12 Wenn beginnt.<br/><br/>Die Option Zeitraum liegen zwischen 40 Sekunden auf 5 Minuten. Für die meisten Server umschalten soll innerhalb von 90 Sekunden abgeschlossen. Erhöht für Server, die eine große Anzahl von Datenbanken oder wenn Datenbanken stark schreiben Arbeitslasten, umschalten. |


### <a name="limitation-after-upgrade-to-v12"></a>Einschränkung *nach dem* upgrade auf V12


| Einschränkung | Beschreibung |
| :--- | :--- |
| V11 kann nicht zurückgesetzt werden. | Nach einem Upgrade direkte kann nicht das Ergebnis zurückgesetzt oder rückgängig gemacht werden. |
| Web- oder Business-Ebene | Nachdem das Upgrade beginnt der Server für die neue Datenbank V12 können nicht erkennt oder Web- oder Business Service-Tier akzeptiert. |



### <a name="export-and-import-after-upgrade-to-v12"></a>Exportieren und Importieren von *nach dem* Upgrade auf V12


Sie exportieren oder Importieren einer V12 mithilfe der [Azure-Portal](https://portal.azure.com/). Oder exportieren oder importieren Sie mithilfe der folgenden Tools:


- SQL Server Management Studio (SSMS)
- Visual Studio 2015
- Data-Tier Application Framework (DacFx)


Um die Tools verwenden, Sie haben oder installieren Sie die neuesten Updates, um sicherzustellen, dass sie die neuen V12-Funktionen unterstützen:


- [Kumulative Update 6 für SQL Server Management Studio 2014](http://support2.microsoft.com/kb/3031047)
- [Februar 2015 Update für SQL Server-Datenbank in Visual Studio 2013 Werkzeuge](https://msdn.microsoft.com/data/hh297027)
- [Februar 2015 Data-Tier Application Framework (DacFx) für SQL Azure-Datenbank V12](http://www.microsoft.com/download/details.aspx?id=45886)


> [AZURE.NOTE] Die vorherigen Tool Links wurden am oder nach 2 März 2015 aktualisiert. Wir empfehlen diesen neueren Tools verwenden.


#### <a name="automated-export"></a>Automatisierte exportieren


Automatisierte exportieren weiterhin als Vorschau verfügbar.  Mit Automated exportieren sind keine Client-Tool Updates erforderlich.  Möchten Sie die Datei und importieren auf einen lokalen Server, werden die oben genannten Werkzeuge Updates erfolgreich importieren benötigt.


### <a name="restore-to-v12-of-a-deleted-v11-database"></a>V12 einer gelöschten V11 Datenbank wiederherstellen

Das folgende Szenario erklärt, dass eine gelöschte V11 Azure SQL-Datenbank auf einem Server V12 Azure SQL-Datenbank wiederhergestellt werden kann.

1. Nehmen Sie einen V11 Azure SQL-Datenbankserver. <br/> Auf dem Server müssen Sie eine Datenbank veraltet Web Business Service-Ebene.
2. Die Datenbank löschen.
3. Aktualisieren Sie den Server auf V12.
4. Anschließend wird die Datenbank auf dem Server wiederherstellen. <br/> Die Datenbank wird dadurch V12 S0 auf der Dienstebene Standard aktualisiert.
5. Sie können alle unterstützten Service-Tier Datenbank wechseln, ist S0 nicht Ihren Wünschen.


### <a name="powershell-cmdlets"></a>PowerShell-cmdlets


PowerShell-Cmdlets sind zum Starten, beenden oder Überwachen einer Aktualisierung zu Azure SQL-Datenbank V12 V11 oder andere Pre-V12-Version verfügbar.

- [Upgrade auf SQL Datenbank V12 mit PowerShell](sql-database-upgrade-server-powershell.md)

Referenzdokumentation über PowerShell-Cmdlets finden Sie unter:


- [AzureRMSqlServerUpgrade abrufen](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [AzureRMSqlServerUpgrade starten](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [AzureRMSqlServerUpgrade beenden](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Stop-Cmdlet bedeutet, um nicht anhalten. Gibt es keine Möglichkeit, eine Aktualisierung wieder beginnend fortsetzen. Stop-Cmdlet bereinigt und gibt alle Ressourcen frei.


## <a name="failure-resolution"></a>Fehler-Auflösung


Schlägt das Upgrade für alle Grund bleibt die Datenbank V11 aktiv und als normal.


## <a name="related-links"></a>Verwandte links


- Microsoft Azure [Vorschaufunktionen](https://azure.microsoft.com/services/preview/)


<!--Anchors-->
[Subheading 1]: #subheading-1
