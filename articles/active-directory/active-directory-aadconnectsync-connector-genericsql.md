<properties
   pageTitle="Generischer SQL Connector | Microsoft Azure"
   description="Dieser Artikel beschreibt die allgemeine Microsoft SQL-Connector konfigurieren."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-technical-reference"></a>Allgemeine technische SQL-Connector

Dieser Artikel beschreibt den generischen SQL-Connector. Der Artikel gilt für die folgenden Produkte:

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Hotfix 4.1.3671.0 oder später [KB3092178](https://support.microsoft.com/kb/3092178)verwenden.

MIM2016 und FIM2010R2 ist der Connector als Download im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495)verfügbar.

Dieser Connector in Aktion finden Sie in Artikel [Generischen SQL-Connector schrittweise](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) .

## <a name="overview-of-the-generic-sql-connector"></a>Übersicht über den generischen SQL-Connector

Generischen SQL-Connector können Sie in einem Datenbanksystem Synchronisierungsdienst integrieren, die ODBC-Verbindung bietet.  

Aus einer grundlegenden Perspektive die folgenden Funktionen der aktuellen Version der Connector unterstützt:

Funktion | Unterstützung
--- | ---
Verbundene Datenquelle | Der Connector wird mit allen 64-Bit-ODBC-Treiber unterstützt. Es wurde mit der folgenden getestet: <li>Microsoft SQL Server und SQL Azure</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 und 11 g</li><li>MySQL 5.x</li>
Szenarien   | <li>Objekt Lifecycle Management</li><li>Kennwort-Management</li>
Vorgänge | <li>Vollständiger Import und Deltaimport, Export</li><li>Fügen Sie für den Export: hinzu, löschen Sie, aktualisieren und ersetzen</li><li>Kennwort, Kennwort ändern</li>
Schema | <li>Dynamische Erkennung von Objekten und Attributen</li>

### <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie den Connector verwenden, sorgen Sie Folgendes auf dem Synchronisierungsserver:

- Microsoft .NET 4.5.2 Framework oder höher
- 64-Bit-ODBC-Clienttreiber

### <a name="permissions-in-connected-data-source"></a>Berechtigungen in verbundenen Datenquelle
Zum Erstellen oder eines der unterstützten im generischen SQL Connector ausführen, benötigen Sie Folgendes:

- db_datareader
- db_datawriter

### <a name="ports-and-protocols"></a>Ports und Protokolle
Für ODBC-Treiber arbeiten erforderlichen Ports Dokumentation des Datenbankanbieters.

## <a name="create-a-new-connector"></a>Erstellen eines neuen Connectors
Erstellen Sie einen generischen SQL-Connector versehen Sie **Synchronisierungsdienst** , **Management Agent** und **Erstellen**. Wählen Sie den Connector **generischen SQL (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Konnektivität
Der Connector verwendet eine ODBC DSN-Datei für die Konnektivität. Erstellen Sie die DSN-Datei mit **ODBC-Datenquellen** im Startmenü unter **Verwaltung**. Erstellen Sie in dem Verwaltungstool einen **Datei-DSN** mit dem Anschluss bereitgestellt werden kann.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Die Konnektivität wird erstmals beim Erstellen eines neuen generischen SQL-Connectors. Sie müssen zunächst die folgenden Informationen:

- DSN-Dateipfad
- Authentifizierung
    - Benutzername
    - Kennwort

Die Datenbank sollte diese Authentifizierungsmethoden unterstützt:

- **Windows-Authentifizierung**: die Authentifizierung Datenbank verwendet die Windows-Anmeldeinformationen den Benutzer überprüft. Das Benutzername/Kennwort angegeben zur Authentifizierung mit der Datenbank. Dieses Konto benötigt Berechtigungen für die Datenbank.
- **SQL-Authentifizierung**: die Authentifizierung Datenbank verwendet das Benutzername/Kennwort einen Bildschirm Verbindung definiert zu der Datenbank herstellen. Wenn Sie User Name/Passwort in der DSN-Datei speichern, haben die Anmeldeinformationen auf dem Bildschirm Verbindung Vorrang.
- **Azure SQL-Datenbank-Authentifizierung**: Weitere Informationen finden Sie unter [Verbinden mit SQL Datenbank durch Verwendung von Azure Active Directory-Authentifizierung](..\sql-database\sql-database-aad-authentication.md).

**DN ist**: Bei dieser Option wird der DN auch als Ankerattribut verwendet. Es kann für eine einfache Implementierung verwendet werden, sondern hat auch die folgende Einschränkung:

-   Connector unterstützt nur einen Objekttyp. Daher können Attribute nur dasselbe Objekt verweisen.

**Exporttyp: Objekt ersetzen**: beim Export nur einige Attribute ändern das gesamte Objekt mit allen Attributen wird exportiert und das vorhandene Objekt ersetzt.

### <a name="schema-1-detect-object-types"></a>Schema 1 (erkennen Objekttypen)
Auf dieser Seite werden Sie konfigurieren, wie der Connector die unterschiedlichen Objekttypen in der Datenbank gefunden wird.

Jeder Objekttyp als eine Partition und konfiguriert auf **Partitionen konfigurieren und Hierarchien**.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Objekttyp Methode**: der Connector unterstützt diese Erkennungsmethoden Objekt geben.

- **Festwert**: bieten eine durch Kommas getrennte Liste die Liste der Objekttypen. Beispiel: `User,Group,Department`.  
![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **Tabelle/Ansicht/gespeicherte Prozedur**: Geben Sie den Namen der Tabelle, Ansicht, gespeicherte Prozedur und den Namen der Spalte, die die Liste der Objekttypen enthält. Verwenden Sie eine gespeicherte Prozedur auch geben Parameter dafür im Format **[Name]: [Richtung]: [Wert]**. Bieten Sie jeden Parameter in einer separaten Zeile (STRG + EINGABETASTE eine neue Zeile zu verwenden).  
![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **SQL-Abfrage**: mit dieser Option können Sie eine SQL-Abfrage bereitzustellen, die beispielsweise eine einzelne Spalte mit Objekttypen gibt `SELECT [Column Name] FROM TABLENAME`. Die zurückgegebene Spalte muss vom Typ String (Varchar).

### <a name="schema-2-detect-attribute-types"></a>Schema 2 (Attributtypen erkennen)
Auf dieser Seite werden Sie konfigurieren, wie die Namen und Typen werden erkannt werden. Die Konfigurationsoptionen sind für jeden Objekttyp, der auf der vorherigen Seite ermittelt aufgeführt.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Attributtyp Methode**: der Connector unterstützt diese Attribut Typ Erkennungsmethoden mit jedem erkannten Objekttyp im Schema 1 Bildschirm.

- **Tabelle/Ansicht/gespeicherte Prozedur**: Geben Sie den Namen der Tabelle, Ansicht, gespeicherten Prozedur, die mit die Attributnamen gefunden werden sollte. Verwenden Sie eine gespeicherte Prozedur auch geben Parameter dafür im Format **[Name]: [Richtung]: [Wert]**. Bieten Sie jeden Parameter in einer separaten Zeile (STRG + EINGABETASTE eine neue Zeile zu verwenden). Um die Attributnamen in einem mehrwertigen Attribut erkennen, bieten Sie eine kommagetrennte Liste von Tabellen oder Ansichten. Mehrwertige Szenarien nicht unterstützt wenn übergeordnete und untergeordnete Tabelle dieselben Spaltennamen haben.
- **SQL-Abfrage**: mit dieser Option können Sie eine SQL-Abfrage angeben, die eine einzelne Spalte mit Namen, beispielsweise gibt `SELECT [Column Name] FROM TABLENAME`. Die zurückgegebene Spalte muss vom Typ String (Varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Schema 3 (Anker definieren und DN)
Auf dieser Seite können Sie Anker und DN-Attribut für jedes erkannte Objekttyp konfigurieren. Sie können mehrere zu den Anker eindeutige auswählen.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- Mehrwertige und booleschen Attribute sind nicht aufgelistet.
- Dasselbe Attribut kann nicht für DN und Anker verwenden, wenn **DN ist** auf der Seite Verbindung ausgewählt ist.
- Wenn **DN ist** auf der Seite Verbindung aktiviert ist, ist diese Seite DN-Attribut erforderlich. Dieses Attribut wird auch als Ankerattribut verwendet.
![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Schema 4 (definieren Attribut Type Reference und Richtung)
Auf dieser Seite können Sie den Attributtyp wie Integer, Binary, Boolean oder Richtung für jedes Attribut konfigurieren. Mehrwertige Attribute einschließlich aller Attribute Seite **Schema 2** aufgeführt.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **Datentyp**: Attributtyp solche bekannt das Synchronisierungsmodul zugeordnet. Standardmäßig wird dieselbe Art verwenden, wie im SQL-Schema entdeckt jedoch DateTime und Verweis nicht leicht. Müssen Sie **DateTime** oder **Verweis**angeben.
- **Richtung**: Festlegen der Attribut-Richtung zum Importieren, exportieren oder ImportExport. ImportExport ist die Standardeinstellung.
![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Hinweise:

- Ist ein Attributtyp nicht nachweisbar durch den Connector verwendet den Datentyp String.
- **Verschachtelte Tabellen** kann mehreren Tabellen angesehen werden. Oracle werden die Zeilen der geschachtelten Tabelle in keiner bestimmten Reihenfolge gespeichert. Beim Abrufen der geschachtelten Tabelle in PL/SQL-Variablen sind die Zeilen aufeinander folgenden Indizes beginnend mit 1 angegeben. Das gibt Ihnen arrayähnlichen Zugriff auf einzelne Zeilen.
- **VARRYS** werden im Connector nicht unterstützt.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Schema 5 (Partition für Attribute definieren)
Auf dieser Seite konfigurieren Sie für alle Attribute der Partition (Objekttyp) ein Attribut verweisen.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Verwenden Sie **DN ist**, müssen Sie denselben Objekttyp wie Verwenden von beziehen. Sie können nicht einen anderen Objekttyp verweisen.

### <a name="global-parameters"></a>Globale Parameter
Die Seite Globalparameter dient zum Delta Import Datums-/Uhrzeitformat und Kennwortmethode konfigurieren.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

Die generischen SQL-Connector unterstützt die folgenden Methoden für Delta Import:

- **Trigger**: finden Sie unter [generieren Delta Ansichten mithilfe von Triggern](https://technet.microsoft.com/library/cc708665.aspx).
- **Wasserzeichen**: eine generische Methode mit jeder Datenbank verwendet werden kann. Wasserzeichen-Abfrage ist voreingestellt des Datenbankanbieters. Wasserzeichen-Spalte muss in jeder Tabelle/Ansicht vorhanden sein. Diese Spalte muss fügt verfolgen und Updates an Tabellen als und seine abhängigen (mehrwertige oder) Tabellen. Die Systemuhren der Synchronisierungsdienst und der Datenbankserver müssen synchronisiert werden. Wenn dies nicht der Fall ist, einige Einträge in der deltaimport können weggelassen werden.  
Einschränkung:
    - Wasserzeichen-Strategie ist keine für gelöschte Unterstützungsobjekte.
- **Snapshot**: (funktioniert nur mit Microsoft SQL Server) [mit Snapshots generieren Delta-Ansichten](https://technet.microsoft.com/library/cc720640.aspx)
- **Change Tracking**: (funktioniert nur mit Microsoft SQL Server) [zu ändern](https://msdn.microsoft.com/library/bb933875.aspx)  
Nachteile:
    - Anker und DN-Attribut müssen Teil des Primärschlüssels für das ausgewählte Objekt in der Tabelle sein.
    - SQL-Abfrage wird beim Import und Export mit Change Tracking nicht unterstützt.

**Zusätzliche Parameter**: Festlegen der Zeitzone des Datenbank-Servers, der angibt, wo sich der Datenbankserver befindet. Dieser Wert dient zur Unterstützung von Formaten Datum & Uhrzeit Attribute.

Der Connector speichert Datum und Uhrzeit Datum immer im UTC-Format. Um richtig konvertiert das Datum und Uhrzeit, der Zeitzone des Datenbankservers und der Formats muss angegeben werden. Das Format sollte in .net Format angegeben werden.

Beim Export muss jedes Mal Datumsattribut Connector im UTC-Format angegeben werden.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Passwort-Konfiguration**: der Connector bietet Kennwort Synchronisierung Funktionen und unterstützt und Kennwort ändern.

Der Connector bietet zwei Methoden zur Synchronisierung von Kennwörtern unterstützt:

- **Gespeicherte Prozedur**: Diese Methode erfordert zwei gespeicherte Prozeduren festlegen und unterstützen Kennwort. Geben Sie alle Parameter hinzufügen und ändern Sie des Vorgangs Kennwort **Kennwort SP festlegen** und **Ändern Kennwort SP** Parameter bzw. nach unten wird.
![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **Kennwort-Erweiterung**: Diese Methode erfordert Kennwort-Erweiterung DLL (Sie müssen zu dem Namen der DLL, die die [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) -Schnittstelle implementiert). Kennwort-Erweiterungsassembly muss Erweiterung Ordner befinden, damit der Connector die DLL zur Laufzeit laden kann.
![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Sie müssen die Kennwort-Management auf **Erweiterung konfigurieren** aktivieren.
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurieren von Partitionen und Hierarchien
Wählen Sie auf der Seite Partitionen und Hierarchien alle Objekttypen. Jeder Objekttyp wird eine eigene Partition.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Sie können auch auf die **Konnektivität** oder **Globalparameter** definierten Werte überschreiben.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Anker konfigurieren
Diese Seite ist schreibgeschützt, da Anker bereits definiert wurde. Das ausgewählte Ankerattribut werden immer mit dem Objekttyp um sicherzustellen, dass sie eindeutig über Objekttypen angehängt.

![Anker](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Konfigurieren Sie führen Schrittparameter
Diese Schritte werden auf Ausführungsprofile für den Connector konfiguriert. Diese Konfigurationen sind das eigentliche importieren und exportieren.

### <a name="full-and-delta-import"></a>Vollständige und Deltaimport
Generischen SQL-Connector unterstützt vollständige und Delta Import dieser Methoden:

- Tabelle
- Ansicht
- Gespeicherte Prozedur
- SQL-Abfrage

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabelle/Sicht**  
Mehrwertige Attribute für ein Objekt importieren, müssen Sie den Namen Tabellenansicht Kommas in **Namen von mehrwertigen Tabellenansichten** entsprechenden Joins in der **Join-Bedingung** mit der übergeordneten Tabelle vorsehen.

Beispiel: Sie möchten das Employee-Objekt und seine mehrwertige Attribute importieren. Es gibt zwei Tabellen, Mitarbeiter (Haupttabelle) und Abteilung (mehrwertig).
Folgendermaßen Sie vor:

- Typ **Mitarbeiter** in **Tabelle/Ansicht/SP**.
- Geben Sie in **Name des mehrwertigen Tabellenansichten**Abteilung.
- Geben Sie die Join-Bedingung zwischen Mitarbeiter und Abteilung **Join-Bedingung**, z. B. `Employee.DEPTID=Department.DepartmentID`.
![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Gespeicherte Prozeduren**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- Sie haben viele Daten empfiehlt es Paginierung mit gespeicherten Prozeduren implementiert.
- Die gespeicherte Prozedur Paginierung unterstützen müssen Sie starten und endIndex angeben. Siehe: [große Datenmengen effizient Paging](https://msdn.microsoft.com/library/bb445504.aspx).
- @StartIndexund @EndIndex zur Ausführungszeit mit entsprechenden Seitengröße konfiguriert **Konfigurieren** Seite ersetzt. Z. B. wenn der Connector erste Seite und die Seitengröße Ruft Wert 500 in solchen @StartIndex 1 und @EndIndex 500. Bei Anschluss Folgeseiten und ändern diese Werte erhöhen die @StartIndex & @EndIndex Wert.
- Um parametrisierte gespeicherte Prozedur auszuführen, geben Sie die Parameter in `[Name]:[Direction]:[Value]` Format. Geben Sie jeden Parameter in einer separaten Zeile (mit STRG + EINGABETASTE eine neue Zeile zu).
- Generischer SQL-Connector unterstützt auch Importvorgang Verbindungsserver in Microsoft SQL Server. Wenn Informationen aus einer Tabelle in verknüpfte Server abgerufen werden sollen, muss im Format Tabelle bereitgestellt:`[ServerName].[Database].[Schema].[TableName]`
- Generischen SQL-Connector unterstützt nur die Objekte, die ähnlichen Struktur (sowohl alias Name und Datentyp) und das Schema Erkennung auszuführen. Wenn das ausgewählte Objekt aus Schema und Informationen zur Schritt unterscheidet, kann SQL Connector nicht Unterstützung dieses Szenarios.

**SQL-Abfrage**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- Mehrere Resultsets Abfragen nicht unterstützt.
- SQL-Abfrage die Paginierung unterstützt und bieten und endIndex als Variable Paginierung unterstützen.

### <a name="delta-import"></a>Deltaimport
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Delta Importkonfiguration erfordert einige weitere Konfiguration mit vollständigen Import verglichen.

- Bei Auswahl die Trigger oder Snapshot Ansatz für Delta Überarbeitungen Geben Sie Tabelle oder Snapshot Datenbank im **Tabelle oder Snapshot-Datenbankname** .
- Außerdem müssen Sie Join-Bedingung zwischen Tabelle und übergeordnete Tabelle z. bereitzustellen`Employee.ID=History.EmployeeID`
- Um die Transaktion aus der Tabelle in der übergeordneten Tabelle verfolgen, erforderlich den Namen der Spalte, der den Vorgang Informationen (hinzufügen, aktualisieren und löschen).
- Wählt Wasserzeichen Delta Überarbeitungen Geben Sie den Namen der Spalte mit den Vorgangsinformationen im **Wasser Mark Spaltennamen**.
- Die Spalte **Type-Attribut Änderung** ist für den Änderungstyp erforderlich. Diese Spalte wird eine Änderung in der primären Tabelle oder mehrere Werte, einen Änderungstyp an Delta. Diese Spalte kann Modify_Attribute Änderungstyp Attributebene oder ein hinzufügen, ändern oder Löschen für eine Änderung auf Objektebene ändern. Wenn es etwas anderes als Standardwert hinzufügen, definieren ändern oder löschen, können Sie diese Werte mit dieser Option.

### <a name="export"></a>Exportieren
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Generischen SQL-Connector unterstützt mit vier unterstützten Methoden wie exportieren:

- Tabelle
- Ansicht
- Gespeicherte Prozedur
- SQL-Abfrage

**Tabelle/Sicht**  
Wenn die Option Tabelle/Sicht generiert der Connector die entsprechenden Abfragen dazu exportieren.

**Gespeicherte Prozeduren**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Wenn die Option gespeicherte Prozedur erfordert Export drei verschiedene gespeicherte Prozeduren Insert/Update/Delete-Operationen.

- **SP-Name hinzufügen**: This SP führt jedes Objekt kommt mit Anschluss für das Einfügen in der jeweiligen Tabelle.
- **SP-Name aktualisieren**: This SP führt jedes Objekt kommt mit Anschluss für das Update in der jeweiligen Tabelle.
- **SP-Name löschen**: This SP führt jedes Objekt kommt mit Anschluss zum Löschen in der jeweiligen Tabelle.
- Attribut aus dem Schema als Parameterwert für die gespeicherte Prozedur verwendet. Beispielsweise `EmployeeName: INPUT: @EmployeeName` (EmployeeName Schema Connector aktiviert ist und der Connector ersetzt den entsprechenden Wert dabei Export)
- Um parametrisierte gespeicherte Prozedur auszuführen, geben Sie Parameter in `[Name]:[Direction]:[Value]` Format. Geben Sie jeden Parameter in einer separaten Zeile (mit STRG + EINGABETASTE eine neue Zeile zu).

**SQL-Abfrage**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Bei Auswahl die Option SQL Query ist der Export dreier verschiedene Abfragen Insert/Update/Delete-Operationen erforderlich.

- **Abfrage einfügen**: Diese Abfrage ausgeführt wird, kommt ein Objekt zum Connector zum Einfügen in der jeweiligen Tabelle.
- **Abfrage aktualisieren**: Diese Abfrage ausgeführt wird, kommt ein Objekt mit Anschluss für das Update in der jeweiligen Tabelle.
- **Abfrage löschen**: Diese Abfrage ausgeführt wird, kommt ein Objekt zum Connector in der jeweiligen Tabelle zum Löschen.
- Attribut z. B. als Parameterwert für die Abfrage verwendete Schema ausgewählt`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Problembehandlung

-   Informationen zum Aktivieren der Protokollierung den Connector beheben finden Sie unter [How to Enable Tracing ETW-Anschlüsse](http://go.microsoft.com/fwlink/?LinkId=335731).
