<properties
   pageTitle="Überwachung in Azure SQL Datawarehouse | Microsoft Azure"
   description="Erste Schritte mit Azure SQL Data Warehouse Überwachung"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="09/24/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# <a name="auditing-in-azure-sql-data-warehouse"></a>Überwachung in Azure SQL Datawarehouse

> [AZURE.SELECTOR]
- [Überwachung](sql-data-warehouse-auditing-overview.md)
- [Erkennung](sql-data-warehouse-security-threat-detection.md)

Überwachung von SQL Data Warehouse können Sie Datensatz, die Ereignisse in der Datenbank zu Azure Storage-Konto anmelden. Überwachung können Sie die Einhaltung behördlicher Auflagen und Datenbankaktivität verstehen Einblick in Diskrepanzen und Anomalien, die Unternehmen hinweisen oder Verstößen Sicherheit. SQL Data Warehouse Überwachung integriert auch Microsoft Power BI Drilldown-reporting und Analyse.

Überwachungstools aktivieren und Einhaltung von Compliance-Standards erleichtern jedoch nicht garantieren. Weitere Informationen zu Azure Programmen, die Einhaltung von Standards unterstützen finden Sie im <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Vertrauensstellungscenter Azure</a>.

+ [Datenbank-Auditing-Grundlagen]
+ [Einrichten der Überwachung für die Datenbank]
+ [Analysieren von Audit-Protokollen und Berichten]

##<a id="subheading-1"></a>Azure SQL Data Warehouse-Datenbank Auditing-Grundlagen


Datenbank-auditing SQL Data Warehouse können Sie:

- **Erhalten** einen Audit-Trail ausgewählten. Kategorien der Datenbankaktionen überwacht werden können.
- **Bericht** über Datenbankaktivität. Vorkonfigurierte Berichte und einem Dashboard können Sie schnell und Ereignis beginnen.
- **Analysieren** von Berichten. Verdächtige Ereignisse und ungewöhnliche Trends finden.

Sie können die Überwachung für folgende Ereignis:

**Einfache SQL** und **Parametrisierte SQL** für die gesammelte Audit-logs gelten als  

- **Zugriff auf Daten**
- **Geändert (DDL)**
- **Ändert (DML)**
- **Konten, Rollen und Berechtigungen (DL)**
- Die **gespeicherte Prozedur**, **Anmeldung** und **Verwaltung**.

Für jede Ereigniskategorie Überwachung von **Erfolg** und **Fehler** Operationen werden separat konfiguriert.

Details über die Aktivitäten und Ereignisse überwacht finden Sie unter <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">Audit Log Übersicht (Doc-Datei-Download)</a>.

Überwachungsprotokolle werden in Ihrem Konto Azure-Speicher gespeichert. Sie können ein Audit Log Aufbewahrungszeitraum festlegen.

Überwachungsrichtlinien kann für eine bestimmte Datenbank oder als Standardrichtlinie Server definiert werden. Ein Server Standardüberwachungsrichtlinie gilt für alle Datenbanken auf einem Server die keine bestimmte übergeordnete Datenbank Überwachungsrichtlinie definiert.

Vor dem Festlegen auf überwachen, Überwachung Kontrollkästchen verwenden ["für kompatiblen Client"](sql-data-warehouse-auditing-downlevel-clients.md).


##<a id="subheading-2"></a>Einrichten der Überwachung für die Datenbank

1. Starten der <a href="https://portal.azure.com" target="_blank">Azure-Portal</a>.

2. Navigieren Sie zur Konfiguration Falz SQL Data Warehouse-Datenbank SQL Server, die Sie überwachen möchten. Klicken Sie auf **die Schaltfläche oben und Blade-Einstellung** und wählen Sie **Überwachung aus**.

    ![][1]

3. Deaktivieren Sie in der Überwachung Blade-Konfiguration zunächst das Kontrollkästchen **Erben Überwachung Settings from Server** . Dadurch werden die Einstellungen für eine bestimmte Datenbank.

    ![][2]

4. Aktivieren Sie als Nächstes klicken **auf** Überwachung.

    ![][3]

5. Wählen Sie auditing Konfiguration Blatt **SPEICHERDETAILS** Blatt Audit Logs Speicher geöffnet. Wählen Sie Protokolle Speicherort Azure Storage-Konto und die Aufbewahrungsdauer. **Tipp:** Verwenden Sie dieselbe Speicherkonto für alle überwachten Datenbanken vorkonfigurierte Berichtvorlagen optimal.

    ![][4]

6. Klicken Sie auf **OK** , um die Details-Konfiguration zu speichern.


7. Unter **Ereignis Protokollierung**auf ** **ERFOLGS-** und alle Ereignisse zu protokollieren** oder wählen Sie einzelne Kategorien aus.


8. Wenn Sie Auditing bei einer Datenbank konfigurieren, müssen Sie ändern die Verbindungszeichenfolge des Clients um sicherzustellen, dass Daten Überwachung ordnungsgemäß erfasst werden. Überprüfen Sie im Thema [Ändern Server FDQN in der Verbindungszeichenfolge](sql-data-warehouse-auditing-downlevel-clients.md) Downlevel-Clientverbindungen.

9. Klicken Sie auf **OK**.


##<a id="subheading-3">Analysieren von Audit-Protokollen und Berichten</a>

Überwachungsprotokolle werden in einer Auflistung von Tabellen Speicher mit **SQLDBAuditLogs** in Azure Speicherkonto aggregiert während der Installation gewählte. Sie können mit einem Tool wie <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure-Speicher-Explorer</a>anzeigen.

Vorkonfigurierte Dashboard Berichtsvorlage ist verfügbar als <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">downloadbare Excel-Kalkulationstabelle</a> schnell Daten analysieren. Um die Vorlage auf die Überwachungsprotokolle verwenden, benötigen Sie Excel 2013 und Power Abfrage, die Sie herunterladen können <a href="http://www.microsoft.com/download/details.aspx?id=39379">hier</a>.

Die Vorlage enthält fiktive Beispieldaten und Power Abfrage einrichten, Audit-Log direkt von Ihrem Konto Azure-Speicher importieren.

Nähere Informationen zum Arbeiten mit der Berichtsvorlage lesen Sie <a href="http://go.microsoft.com/fwlink/?LinkId=506731">How To (Doc Download)</a>.

![][5]


##<a id="subheading-4">Vorgehensweisen für die Verwendung in der Produktion</a>
Die Beschreibung in diesem Abschnitt bezieht sich auf Screenshots oben. <a href="https://portal.azure.com" target="_blank">Azure-Portal</a> oder <a href= "https://manage.windowsazure.com/" target="_bank">Klassische Azure-Verwaltungsportal</a> kann verwendet werden.


##<a id="subheading-5"></a>Speicher des Sitzungsschlüssels

In der Produktion liegen der Speicherschlüssel regelmäßig aktualisieren. Beim Aktualisieren der Schlüssel erneut speichern müssen. Der Prozess ist wie folgt:.


1. Überwachung (beschriebenen Abschnitt Überwachung einrichten) Konfiguration Blatt **Speicher Zugriffstaste** vom *primären* **zu *sekundären* **wechseln
![][4]
2. Gehen Sie Speicher Konfiguration Blade und **generiert** *Access-Primärschlüssel*.

3. Wechseln Sie zurück zur Überwachung Konfiguration Blade wechseln **Speicherschlüssel Zugriff** vom *sekundären* zum *primären* und drücken Sie **Speichern**.

4. Zurück Benutzeroberfläche und **regeneriert** die *Sekundäre Taste* (als Vorbereitung für den nächsten Schlüssel aktualisieren.

##<a id="subheading-6"></a>Automatisierung
Es gibt einige PowerShell-Cmdlets, die Sie zum Konfigurieren der Überwachung in Azure SQL-Datenbank verwenden können. Zugriff auf die Überwachung Cmdlets muss PowerShell in Azure-Ressourcen-Manager-Modus ausgeführt werden.

> [AZURE.NOTE] [Azure-Ressourcen-Manager](https://msdn.microsoft.com/library/dn654592.aspx) -Modul befindet sich in der Vorschau. Die gleichen Verwaltungsfunktionen Azure Modul bereitgestellt nicht.

Wenn Sie Azure-Ressourcen-Manager aktiviert ist, `Get-Command *AzureSql*` verfügbaren Cmdlets aufgelistet.


<!--Anchors-->
[Datenbank-Auditing-Grundlagen]: #subheading-1
[Einrichten der Überwachung für die Datenbank]: #subheading-2
[Analysieren von Audit-Protokollen und Berichten]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


<!--Link references-->
