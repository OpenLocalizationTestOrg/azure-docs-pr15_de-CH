<properties 
   pageTitle="Microsoft Produktvergleich überwachen | Microsoft Azure"
   description="Microsoft Operations Management Suite (OMS) ist die Microsoft-Cloud IT Management-Lösung, die Sie verwalten und schützen Ihre lokalen und cloud-Infrastruktur hilft.  In diesem Artikel identifiziert OMS andere Dienste und enthält Links zu ausführlichen Inhalt."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="microsoft-monitoring-product-comparison"></a>Microsoft Überwachung – Produktvergleich

Dieser Artikel enthält einen Vergleich zwischen System Center Operations Manager (SCOM) und Protokollanalyse in Operations Management Suite (OMS) Architektur, die Logik wie diese Ressourcen überwachen und wie sie die Daten analysieren sie sammeln.  Dies ist ein grundlegendes Verständnis der Unterschiede und Stärken geben.  

## <a name="basic-architecture"></a>Grundlegende Architektur
### <a name="system-center-operations-manager"></a>System Center Operations Manager

Alle SCOM-Komponenten werden im Rechenzentrum installiert.  Auf Windows- und Linux-Computern, die von SCOM verwalteten [Agents installiert](http://technet.microsoft.com/library/hh551142.aspx) .  Agents verbinden sich mit [Verwaltungsservern](https://technet.microsoft.com/library/hh301922.aspx) mit SCOM Datenbank und Data Warehouse.  Agenten benötigen Domänenauthentifizierung zum Verwaltungsserver herstellen.  Außerhalb einer vertrauenswürdigen Domäne können Zertifikatauthentifizierung ausführen oder zu einem [Gatewayserver](https://technet.microsoft.com/library/hh212823.aspx)verbinden.

SCOM erfordert zwei SQL-Datenbanken für operative Daten und anderen Datawarehouse und Analyse.  Ein [Berichtsserver](https://technet.microsoft.com/library/hh298611.aspx) wird SQL Reporting Services Bericht Daten aus dem Datawarehouse. 

SCOM kann mithilfe von managementpacks für Produkte wie [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708)und [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html)Cloudressourcen überwachen.  Diese managementpacks verwenden eine oder mehrere lokale Agents als Proxys für die Ermittlung von Cloudressourcen und ausgeführten Workflows zum Messen der Leistung und Verfügbarkeit.  Proxy-Agents werden auch [Netzwerkgeräten überwachen](https://technet.microsoft.com/library/hh212935.aspx) und andere externen Ressourcen verwendet.

Die Betriebskonsole ist eine Windows-Anwendung, die eine Verbindung mit einem Management-Server und den Administrator anzeigen und Analysieren der gesammelten Daten und SCOM-Umgebung konfigurieren.  Eine Web-basierte Konsole kann auf einem IIS-Server gehostet und analysiert Daten über einen Browser.

![SCOM-Architektur](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Protokollanalyse

Die meisten OMS-Komponenten sind in der Azure-Cloud bereitstellen und Verwalten mit minimalen Kosten und Verwaltungsaufwand.  Alle Daten der Protokollanalyse wird in OMS-Repository gespeichert.

Protokollanalyse können Daten aus einer von drei Quellen erfassen:

- Physische und virtuelle Maschinen mit Windows und [Microsoft Monitoring Agent (MMA)](https://technet.microsoft.com/library/mt484108.aspx) oder Linux und [Operations Management Suite Agent für Linux](https://technet.microsoft.com/library/mt622052.aspx).  Dieser Computer kann lokal oder virtuelle Computer in Azure oder anderen Cloud.
- Ein Azure Storage-Konto bei [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md) von Azure Worker-Rolle Webrolle oder virtuellen Computer gesammelten Daten.
- [Verbindung zu einer Verwaltungsgruppe SCOM](https://technet.microsoft.com/library/mt484104.aspx).  In dieser Konfiguration kommunizieren die Agenten mit SCOM Verwaltungsserver die Daten in SCOM-Datenbank bereitstellen, wird er dann OMS-Datenspeicher gesendet.
Administratoren Daten analysieren und konfigurieren Protokollanalyse OMS-Portal in Azure gehostet wird und über einen Webbrowser zugegriffen werden kann.  Mobiler apps für diese Daten sind für die standard-Plattformen verfügbar.

![Analytics Architektur](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Integration von SCOM und Protokollanalyse

SCOM Protokollanalyse als Datenquelle wird können Sie die Funktionen beider Produkte in einer hybriden Umgebung überwachen nutzen.  Sie können vorhandene SCOM-Agenten über die Betriebskonsole von OMS neben managementpacks von SCOM Ausführung verwaltet werden.  
Daten aus einer verbundenen SCOM Verwaltungsgruppe ist Protokollanalyse übermittelt mithilfe einer der folgenden Methoden:

- Ereignisse und Leistungsdaten werden vom Agent gesammelte und SCOM an.  Verwaltungsserver in SCOM liefern Daten zu Protokollanalyse.
- Einige Ereignisse wie IIS-Protokolle und Sicherheitsereignisse weiterhin direkt Protokollanalyse vom Agent übermittelt werden.
- Lösungsansätze bieten zusätzlichen Software auf den Agenten oder erfordern, dass Software installiert werden, um zusätzliche Daten zu sammeln.  Diese Daten werden in der Regel direkt an Protokollanalyse gesendet.
- Lösungsmöglichkeiten sammeln Daten direkt von SCOM-Verwaltungsservern, die nicht vom Agent stammt.  Beispielsweise sammelt die [Alert-Management-Lösung](https://technet.microsoft.com/library/mt484092.aspx) Alerts SCOM Nachdem sie erstellt wurden.

## <a name="monitoring-logic"></a>Überwachen der Logik
SCOM und Protokollanalyse mit ähnlichen Daten aus Agenten jedoch völlig wie sie definieren und implementieren die Logik für die Datensammlung und wie sie die Daten analysieren, die sie sammeln.

### <a name="operations-manager"></a>Operations Manager
Logik für SCOM Überwachung erfolgt in [managementpacks](https://technet.microsoft.com/library/hh457558.aspx) enthalten Logik zum Ermitteln von Komponenten zu überwachen, Messen der Komponenten und zum Sammeln von Daten zu analysieren.  Überwachungsdaten kann so einfach sein wie einen Ereignis oder ein Leistungsindikator gesammelt und können komplexen Logik in einem Skript.  Managementpacks, die vollständige Überwachung werden für eine Vielzahl von [Microsoft und Drittanbietern Applikationen](http://go.microsoft.com/fwlink/?LinkId=82105) neben Hardware-und Netzwerk.  Sie können für benutzerdefinierte Programme [erstellen eigene managementpacks](http://aka.ms/mpauthor) .

Managementpacks enthalten mehrere Workflows, dass jede einige unterschiedliche Überwachungsfunktion wie ein Leistungsindikator, überprüfen den Status eines Dienstes oder ein Skript ausführt.  Jeder Workflow läuft unabhängig und definiert seine eigenen Ergebnisse wie die, denen Datenbank zu schreiben wird und ob eine Warnung ausgelöst wird. 

Sie können Details wie die, die sie ausführen, wo sie als Fehler betrachtet, Schwellenwert Workflows und der Schweregrad der Warnung erzeugen sie überschreiben.  Zusätzliche Funktionen bieten eigene Workflows hinzufügen.

![Überschreibt](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Managementpacks in der Operations Manager-Datenbank installiert und automatisch vom Verwaltungsserver Agents verteilt werden.  Jeder Agent automatisch herunterladen von managementpacks und Laden Workflows für Programme, die sie installiert haben.  Vom Agent gesammelte Daten werden an den Verwaltungsserver zum Einfügen in SCOM Datenbank und Data Warehouse übermittelt.  Die Betriebskonsole können Sie anzeigen und Analysieren dieser Daten über benutzerdefinierte Ansichten, Dashboards und Berichte im Management Pack enthalten.

Die Verteilung von managementpacks wird im folgenden Diagramm dargestellt.

![Management Pack-Fluss](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Protokollanalyse
#### <a name="event-and-performance-collection"></a>Ereignis und Performance-Auflistung
Protokollanalyse erfasst Ereignisse und Leistungsindikatoren von agentsysteme wie Windows-Ereignisprotokoll, IIS-Protokolle und Syslog.  Sie können Kriterien für die Daten durch das Portal Protokollanalyse und erstellen Sie Abfragen analysieren Daten protokollieren.  Ein Satz von Kriterien ist als OMS Arbeitsbereich erstellen, können Sie zusätzliche Daten für bestimmte Applikationen definiert. 

![Definieren von Ereignisprotokollen Protokollanalyse](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

Während SCOM viele detaillierte Workflows, die in der Regel definieren Kriterien für Daten und die Aktion, die Reaktion erfolgen soll, hat Protokollanalyse allgemeine Kriterien für die Datensammlung.  Abfragen protokollieren und Solutions bieten mehr gezielte Kriterien analysieren und auf bestimmte Daten in der Cloud nach gesammelt wurden.

#### <a name="solutions"></a>Solutions
Solutions bieten zusätzlichen Logik für die Datensammlung und Analyse.  Wählen Sie Solutions OMS-Abonnement in den Lösungskatalog hinzu.

![Lösungskatalog](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Solutions ausführen in erster Linie in der Cloud Analyse von Ereignissen und Leistungsindikatoren im Repository OMS.  Sie können auch zusätzliche Daten erfasst werden definieren, die mit Abfragen protokollieren oder zusätzliche Benutzeroberfläche der Lösung im Dashboard OMS analysiert werden kann. 

Z. B. [Change Tracking-Lösung](https://technet.microsoft.com/library/mt484099.aspx) erkennt Neukonfiguration auf Systeme und schreibt Ereignisse OMS-Repository, die mit mehreren insofern analysiert und zusammengefasst werden können Änderung erkannt.  Sie können zusammengefasste Ansicht Protokoll Abfragen anzeigen, die detaillierten Daten der Projektmappe angezeigt.


Während Sie Lösungen können zu Ihrem Abonnement hinzufügen, müssen Sie nicht aktuell eigene Lösung erstellen.  Sie können Ereignisse und Leistungsindikatoren sammeln und benutzerdefinierte Ansichten basierend auf Protokoll Abfragen auswählen.

Im folgenden Diagramm wird der Überwachungslogik für Protokollanalyse zusammengefasst.

![Log Analytics Lösung Fluss](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Überwachung
### <a name="operations-manager"></a>Operations Manager
SCOM kann die verschiedenen Komponenten einer Anwendung modellieren und in Echtzeit Gesundheit vorsehen.  Dadurch können Sie Fehler anzeigen, die erkannt und mit der Zeit, sondern auch den tatsächlichen Zustand einer Anwendung oder eines Systems und seiner Komponenten jederzeit überprüfen.  Da die Zeiträume, die eine Anwendung verfügbar ist versteht, unterstützt das Modul Gesundheit in SCOM Service Level Agreements (SLA) analysieren und Berichten über die Verfügbarkeit einer Anwendung mit der Zeit.

Beispielsweise zeigt die Ansicht unten in Echtzeit Zustand der SQL-Datenbank-Engines von SCOM überwacht.  Der Zustand der einzelnen Datenbanken für eine Datenbank-Engines wird der unteren Hälfte der Ansicht angezeigt.

![Statusansicht](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Integritäts-Explorer für eine Datenbank-Engines mit den Monitoren unten, die verwendet werden, um dessen Gesamtzustand zu bestimmen.  Diese Bildschirme in SQL Management Pack definiert sind, und führen Sie für alle SQL-Datenbank-Engines von SCOM entdeckt.

![Integritäts-explorer](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Komponenten auf mehreren Systemen können kombiniert werden, um den Zustand einer verteilten Anwendung zu messen.  Dies kann besonders für Geschäftsanwendungen sein, die mehrere verteilte Komponenten enthalten.  Können ein Modell, das den Zustand der einzelnen Komponenten dieses Rollup in für die Anwendung.

Active Directory ist ein Beispiel für ein Management Pack, die ein Modell die verteilten Komponenten analysieren.  Das Beispieldiagramm unten zeigt den Zustand der gesamten Umgebung und die Beziehung zwischen Gesamtstrukturen, Domänen und Domänencontroller.  Jede dieser Komponenten enthält Unterkomponenten und mehrere Monitore ähnlich dem obigen Beispiel SQL.

![SCOM Diagrammansicht](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Protokollanalyse
OMS gehört eine gemeinsame Engine Modell Anwendung weder Gesundheitszustand in Echtzeit zu messen.  Individuelle Lösungen möglicherweise den Gesamtzustand von bestimmten Diensten Daten bewerten und können sie benutzerdefinierten Logik für den Agent für die Analyse in Echtzeit installieren.  Da Solutions in der Cloud mit OMS-Repository ausgeführt werden, bieten sie oft tiefergehende Analyse als normalerweise von managementpacks ausgeführt wird. 

Z. B. [AD und SQL Bewertung Solutions](https://technet.microsoft.com/library/mt484102.aspx) Daten analysieren und eine Bewertung für verschiedene Aspekte der Umgebung bereitstellen.  Sie enthält Verbesserungsvorschläge zur Verbesserung der Verfügbarkeit und Leistung der Umgebung möglich.

![Lösung anzeigen](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Datenanalyse
SCOM und Protokollanalyse bieten verschiedene Funktionen, um Daten zu analysieren.  SCOM weist Ansichten und Dashboards in der Betriebskonsole für die Analyse der aktuellen Daten in verschiedenen Formaten und Berichte für die Daten aus dem Datawarehouse in tabellarischer Form.  Protokollanalyse stellt ein vollständiges Protokoll Abfragesprache und Schnittstelle zum Analysieren von Daten in die OMS-Repository.  Wenn SCOM Protokollanalyse als Datenquelle verwendet wird, enthält das Repository Daten von SCOM Protokoll Analysetools zum Daten aus beiden Systemen verwendet werden können.

### <a name="operations-manager"></a>Operations Manager

#### <a name="views"></a>Ansichten
Ansichten in der Betriebskonsole können Sie zu verschiedenen Datentypen von SCOM in unterschiedlichen Formaten, normalerweise für Ereignisse, Alarme und Zustandsdaten tabellarische Liniendiagramme Leistungsdaten.  Ansichten führen Sie minimale Analyse oder Konsolidierung der Daten jedoch erlauben nach bestimmten Kriterien filtern. 

![Ansichten](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Managementpacks bieten in der Regel mehrere Ansichten unterstützen, der Anwendung oder dem System überwacht.  Dazu gehören Statusansichten für die verschiedenen Objekte, die das Management Pack erkennt Warnungsansichten für erkannte Probleme und Performance-Ansichten für Leistungsindikatoren.

Ansichten sind besonders geeignet für die Analyse der derzeitigen Umgebung einschließlich open Alerts und den Zustand der überwachten Systeme und Objekte.  Sie können detaillierte Ereignis oder Leistungsdaten unterstützen, um die Ursache einer bestimmten Warnung Drilldown. Ebenso können Sie die Leistung und der Zustand der verschiedenen Komponenten einer Anwendung zu deren aktuellen Status anzeigen.

#### <a name="dashboards"></a>Dashboards
Dashboards in der Betriebskonsole arbeiten hauptsächlich mit denselben Daten wie Ansichten aber besser angepasst werden können umfangreichere Visualisierung.  Eine Reihe von standard-Dashboards zur Verfügung, die einfach für Ihre eigenen Zwecke anpassen.  Auch können Sie PowerShell Widgets von PowerShell-Abfrage zurückgegebene Daten angezeigt werden können.

![Dashboard](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Entwickler haben die Möglichkeit, benutzerdefinierte Komponenten Dashboards hinzufügen, die sie in ihren managementpacks enthalten.  Dies können auf eine bestimmte Anwendung wie das Dashboard im SQL Management Pack unten hoch spezialisiert.  Das Dashboard kann auch als Vorlage für benutzerdefinierte Versionen verwendet werden.

![SQL-Dashboard](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Berichte
Berichte in SCOM Analysieren von Daten aus dem Datawarehouse in tabellarischer Form.  Sie können gedruckt und in verschiedenen Formaten wie PDF, CSV und Word automatische geliefert.  Berichte arbeiten mit Daten aus dem Datawarehouse sind besonders geeignet für langfristige Trendanalysen.

Managementpacks bieten in der Regel benutzerdefinierte Berichte für eine bestimmte Anwendung.  Sie können auch aus einer Bibliothek mit generischen Berichte auswählen, die Sie für Ihre eigene Anwendung oder ad-hoc-Analyse anpassen können.

Folgendes ist ein Beispiel Leistungsbericht mit Daten von Active Directory Management Pack.

![Bericht](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Protokollanalyse
Protokollanalyse hat eine [Abfragesprache](https://technet.microsoft.com/library/mt484120.aspx) , mit denen Sie Daten aus mehreren Programmen ohne Erstellen einer benutzerdefinierten Ansicht oder Analyse.  Da in der Cloud OMS implementiert, Leistung von Abfragen und Datenanalyse unterliegen hardwareeinschränkungen und Abfragen, Millionen von Datensätzen schnell analysieren können. 

Abfragen in Protokollanalyse sind auch die Grundlage für andere Funktionen.  Sie können Abfragen speichern, die Ergebnisse nach Excel exportieren oder es automatisch in regelmäßigen Abständen ausgeführt und eine Warnung generieren, wenn die Ergebnisse bestimmte Kriterien entsprechen.  

![Protokoll Abfrage Fluss](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Es folgt ein Beispiel einer Abfrage Protokollanalyse.  In diesem Beispiel alle Ereignisse mit "gestartet" im Namen zurückgegeben und gruppiert nach Ereignis-ID  Der Benutzer gibt einfach die Abfrage und Protokollanalyse dynamisch generiert die Benutzeroberfläche die Analyse.  Wählen Sie in der Liste werden die detaillierten Daten zurückgeben.

![Log-Abfrage](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Neben ad-hoc-Analyse können Abfragen in Protokollanalyse zur zukünftigen Verwendung gespeichert und auch [OMS Dashboard](http://technet.microsoft.com/library/mt484090.aspx) hinzugefügt, wie im folgenden Beispiel gezeigt.

![OMS-dashboard](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Nächste Schritte

- [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx)bereitstellen.
- Melden Sie sich für [Protokollanalyse](https://azure.microsoft.com/documentation/services/log-analytics).  
