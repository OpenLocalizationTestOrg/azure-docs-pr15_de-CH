<properties
   pageTitle="PowerShell Connector | Microsoft Azure"
   description="Dieser Artikel beschreibt die Microsoft Windows PowerShell-Connector konfigurieren."
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

# <a name="windows-powershell-connector-technical-reference"></a>Technische Referenz zu Windows PowerShell Connector
Dieser Artikel beschreibt die Windows PowerShell-Connector. Der Artikel gilt für die folgenden Produkte:

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Hotfix 4.1.3671.0 oder später [KB3092178](https://support.microsoft.com/kb/3092178)verwenden.

MIM2016 und FIM2010R2 ist der Connector als Download im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495)verfügbar.

## <a name="overview-of-the-powershell-connector"></a>Übersicht über PowerShell-Connector
PowerShell-Connector können Sie den Synchronisierungsdienst integrieren mit externen Systemen, die Windows PowerShell-basierte APIs bieten. Der Connector stellt eine Brücke zwischen dem erweiterbaren Konnektivität Aufruf-basierten Management Agents 2 (ECMA2) Framework und Windows PowerShell. Weitere Informationen über ECMA-Framework finden Sie unter [Erweiterbare Konnektivität 2.2 Management Agent Verweis](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie den Connector verwenden, sorgen Sie Folgendes auf dem Synchronisierungsserver:

- Microsoft .NET 4.5.2 Framework oder höher
- Windows PowerShell 2.0, 3.0 oder 4.0

Die Ausführungsrichtlinie Synchronisierungsdienst Server muss konfiguriert werden Connector Windows PowerShell-Skripts ausführen. Wenn der Connector ausgeführt werden digital signiert, konfigurieren die Ausführungsrichtlinie durch Ausführen dieses Befehls:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Erstellen eines neuen Connectors
Zum Erstellen eines Windows PowerShell-Connectors in der Synchronisierungsdienst müssen Sie eine Reihe von Windows PowerShell-Skripts angeben, die vom Synchronisierungsdienst angeforderten Schritte ausführen. Die Datenquelle herstellen und die benötigten Funktionen die Skripts, die Sie implementieren müssen abhängig. Dieser Abschnitt stellt jedes Skripts implementiert werden kann und wenn sie erforderlich sind.

Windows PowerShell-Connector dient die Skripts in der Synchronisierungsdienst-Datenbank zu speichern. Es ist möglich, Skripts auszuführen, die im Dateisystem gespeichert werden, ist es einfacher, jedes Skript direkt in die Connectorkonfiguration Text einfügen.

Erstellen Sie einen Connector PowerShell versehen Sie **Synchronisierungsdienst** , **Management Agent** und **Erstellen**. Wählen Sie den Connector **PowerShell (Microsoft)** .

![Connector erstellen](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Konnektivität
Geben Sie Parameter für die Verbindung mit einem remote-System. Diese Werte sind sicher Synchronisierungsdienst gespeichert und Ihre Windows PowerShell-Skripts zur Verfügung gestellt, wenn der Connector ausgeführt wird.

![Konnektivität](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Sie können die folgenden Parameter für die Verbindung konfigurieren:

**Konnektivität**

Parameter | Standardwert | Zweck
--- | --- | ---
Server | <Blank> | Name des Servers, der den Anschluss an.
Domäne | <Blank> | Domäne der Anmeldeinformationen für die Verwendung zu speichern, wenn der Connector ausgeführt wird.
Benutzer | <Blank> | Benutzername der Anmeldeinformationen für die Verwendung zu speichern, wenn der Connector ausgeführt wird.
Kennwort | <Blank> | Kennwort der Anmeldeinformationen für die Verwendung zu speichern, wenn der Connector ausgeführt wird.
Identitätswechsel Connector | False | Bei true wird der Synchronisierungsdienst Windows PowerShell-Skripts im Kontext der Anmeldeinformationen ausgeführt. Nach Möglichkeit wird empfohlen, der **$Credentials** für jede übergebene Parameter ist Skript anstelle Identitätswechsel. Weitere Informationen über zusätzliche Berechtigungen, die erforderlich sind, um diese Option verwenden, finden Sie unter [Zusätzliche Konfigurationsschritte für den Identitätswechsel](#additional-configuration-for-impersonation).
Beim Identitätswechsel geladen Sie Benutzerprofil | False | Weist Windows das Benutzerprofil des Verbinders Anmeldeinformationen beim Identitätswechsel geladen. Wenn imitierte Benutzer ein servergespeichertes Profil hat, wird der Connector das servergespeicherte Profil nicht geladen. Weitere Informationen über zusätzliche Berechtigungen, die dieser Parameter erforderlich sind, finden Sie unter [Zusätzliche Konfigurationsschritte für den Identitätswechsel](#additional-configuration-for-impersonation).
Anmeldetyp vorgenommen | Keine | Anmeldetyp beim Identitätswechsel. Weitere Informationen finden Sie in [DwLogonType] [ dw] Dokumentation.
Signierte Skripts | False | Wenn true, überprüft der Windows PowerShell-Connector jedes Skript hat eine gültige digitale Signatur. False, sicherstellen Sie, dass Windows PowerShell-Ausführungsrichtlinie Synchronisierungsdienst Server RemoteSigned oder eingeschränkt.

**Common-Modul**  
Der Connector ermöglicht eine freigegebene Windows PowerShell-Modul in der Konfiguration gespeichert. Wenn der Connector ein Skript ausgeführt wird, wird das Windows PowerShell-Modul auf, damit von Skript importiert werden kann.

Importieren, exportieren und Synchronisierung von Kennwörtern Skripts wird das allgemeine Modul den Connector MAData Ordner extrahiert. Das allgemeine Modul wird für Schema, Validierung, Hierarchie und Partition Ermittlungsskripts Ordner TEMP. In beiden Fällen heißt extrahierte allgemeine Modul Skript nach gemeinsamen Modulname Skript festlegen.

Um ein Modul mit dem Namen FIMPowerShellConnectorModule.psm1 aus dem Ordner MAData zu laden, verwenden Sie Folgendes:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Um ein Modul mit dem Namen FIMPowerShellConnectorModule.psm1 aus dem Ordner TEMP zu laden, verwenden Sie Folgendes:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Parametervalidierungsfunktionen**  
Prüfskripts ist ein optionaler Windows PowerShell-Skript, das verwendet werden kann, um sicherzustellen, dass Anschluss vom Administrator bereitgestellten Parameter gültig sind. Validierung sind, Anmeldeinformationen Konnektivität Parameter häufige Verwendungen des Prüfskripts. Prüfskripts wird aufgerufen, nachdem die folgenden Registerkarten und Dialogfelder werden geändert:

- Konnektivität
- Globale Parameter
- Konfiguration der Partition

Prüfskripts empfängt der Connector die folgenden Parameter:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | Registerkarte oder Dialogfeld, das die validierungsanforderung ausgelöst.
ConfigParameters | [KeyedCollection] [ keyk] [Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.

Prüfskripts sollte ein einzelnes ParameterValidationResult Objekt in der Pipeline zurückgeben.

**Schema-Erkennung**  
Das Ermittlungsskript Schema ist obligatorisch. Dieses Skript gibt die Objekttypen und Attribute-Attribut Integritätsregeln, die beim Konfigurieren der Attributflussregeln Synchronisierungsdienst verwendet. Schema Ermittlungsskript wird während der Erstellung von Sendeconnectors und füllt den Connector Schema. Es wird auch von Schema aktualisieren Aktion im Synchronisation-Manager verwendet.

Das Ermittlungsskript Schema erhält folgende Parameter vom Anschluss:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameters | [KeyedCollection] [ keyk] [Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.

Das Skript muss ein einzelnes [Schema] zurückgeben[ schema] Objekt in der Pipeline. Das Schemaobjekt besteht [] [ schemaT] Objekte, die Objekttypen darstellen (z. B.: Benutzer und Gruppen). Objekt enthält eine Auflistung der [SchemaAttribute] [ schemaA] Objekte, die die Attribute darstellen (z. B.: Vorname, Nachname und Adresse) des Typs.

**Zusätzliche Parameter**  
Neben den standardmäßigen Konfigurationen definieren Sie zusätzliche benutzerdefinierte Einstellungen, die spezifische Instanz des Connectors. Diese Parameter können den Connector Partition angegeben oder Schritt ausführen und entsprechende Windows PowerShell-Skripts zugegriffen. Benutzerdefinierte Konfigurationen der Synchronisierungsdienst Datenbank in nur-Text-Format gespeichert werden oder möglicherweise verschlüsselt werden. Die Synchronisierung automatisch verschlüsselt und entschlüsselt sichere Konfigurationen bei Bedarf.

Um benutzerdefinierte Einstellungen anzugeben, trennen Sie den Namen der einzelnen Parameter durch ein Komma (,).

Zugriff auf benutzerdefinierte Einstellungen aus einem Skript müssen Sie den Namen mit einem Unterstrich suffix ( \_ ) und Parameter (Global, Partition oder RunStep). Zugriff auf globale Dateinamenparameter z. B. diesen Codeausschnitt:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Funktionen
Die Registerkarte Capabilities Management Agent-Designer definiert das Verhalten und die Funktionalität des Connectors. Auf dieser Registerkarte ausgewählten Optionen können geändert werden, wenn der Connector erstellt wurde. Diese Tabelle enthält die Funktion Einstellungen.

![Funktionen](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

Funktion | Beschreibung |
--- | --- |
[Distinguished Name der Formatvorlage][dnstyle] | Wenn der Connector definierte Namen unterstützt und so was Stil angibt.
[Exporttyp][exportT] | Bestimmt den Typ von Objekten, die das Skript exportieren angezeigt werden. <li>AttributeReplace – enthält den vollständigen Satz von Werten für ein mehrwertiges Attribut ändert das Attribut.</li><li>AttributeUpdate – enthält nur Deltas mit einem mehrwertigen Attribut ändert das Attribut.</li><li>MultivaluedReferenceAttributeUpdate - enthält einen vollständigen Satz von Werten für mehrwertige Attribute ohne Verweis und nur Deltas für mehrwertige Attribute.</li><li>ObjectReplace – enthält alle Attribute eines Objekts, wenn jedes Attribut ändern</li>
[Datennormalisierung][DataNorm] | Weist der Synchronisierungsdienst Anchor Attribute normalisieren, bevor sie die Skripts bereitgestellt werden.
[Objekt-Bestätigung][oconf] | Ausstehende importieren Verhalten konfiguriert der Synchronisierungsdienst. <li>Normal-Standardverhalten, die alle exportierte Änderungen importieren bestätigt werden erwartet</li><li>NoDeleteConfirmation – Gibt beim Löschen eines Objekts es keine ausstehenden Import generiert.</li><li>NoAddAndDeleteConfirmation – wird ein Objekt erstellt oder gelöscht, es keine ausstehenden Import generiert.</li>
DN als Anker verwenden | Der Distinguished Name der Formatvorlage auf LDAP festgelegt ist, ist das Ankerattribut für den Connectorspace auch den distinguished Name.
Gleichzeitige Vorgänge mehrere Anschlüsse | Wenn diese Option aktiviert ist, können mehrere Windows PowerShell-Connectors gleichzeitig ausführen.
Partitionen | Wenn aktiviert, unterstützt der Connector mehrere Partitionen und Partition Discovery.
Hierarchie | Wenn aktiviert, unterstützt der Connector eine hierarchische Struktur für LDAP-Stil.
Import aktivieren | Wenn aktiviert, importiert der Connector über Skripts importieren.
Deltaimport aktivieren | Wenn aktiviert, kann der Connector Deltas von Skripts importieren anfordern.
Export aktivieren | Wenn aktiviert, werden der Connector Daten über Skripts exportieren exportiert.
Vollständigen Export aktivieren | Wenn aktiviert, unterstützt Skripts exportieren exportieren den gesamten Connectorspace. Um diese Option verwenden, müssen exportieren können auch überprüft werden.
Keine Referenzwerte In ersten Export erfolgreich | Wenn aktiviert, werden die Attribute in einem zweiten exportieren exportiert.
Umbenennung aktivieren | Wenn aktiviert, können die definierte Namen geändert werden.
Löschen hinzufügen, ersetzen | Wenn aktiviert, fügen Sie löschen-als Ersatz für einzelne Vorgänge exportiert werden.
Kennwortvorgänge aktivieren | Wenn aktiviert, werden Synchronisationsskripten Kennwort.
Exportkennwort im ersten Durchlauf aktivieren | Wenn aktiviert, werden Kennwörter während der Bereitstellung beim Erstellen des Objekts exportiert.

### <a name="global-parameters"></a>Globale Parameter
Globalparameter auf den Management Agent-Designer können Sie die Windows PowerShell-Skripts konfigurieren, die vom Connector ausgeführt. Sie können auch globale Werte für benutzerdefinierte Einstellungen auf der Registerkarte Verbindung definiert.

**Partition Discovery**  
Eine Partition ist ein separaten Namespace in einem gemeinsamen Schema. In Active Directory ist jede Domäne beispielsweise eine Partition innerhalb einer Gesamtstruktur. Eine Partition ist die logische Gruppierung für den Import und export. Import und Export besitzen Partition wie ein Kontext und alle Vorgänge in diesem Kontext. Partitionen eine Hierarchie in LDAP darstellen sollen. Der distinguished Name der Partition wird importieren verwendet, um sicherzustellen, dass alle zurückgegebenen Objekte innerhalb des Bereichs einer Partition. Der definierte Partitionsname wird auch während der Bereitstellung aus dem Metaverse im Connectorspace verwendet, bestimmen die Partition, der während des Exports ein Objekt zugeordnet werden soll.

Das Ermittlungsskript Partition erhält folgende Parameter vom Anschluss:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameters  | [KeyedCollection][keyk][Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.

Das Skript muss entweder eine einzelne [Partition] zurück[ part] Objekt oder eine Liste [T] Partition Objekte an die Pipeline.

**Hierarchie Discovery**  
Das Ermittlungsskript Hierarchie wird nur verwendet, wenn Distinguished Name der Formatvorlage LDAP ist. Das Skript wird zum können Sie durchsuchen und eine Reihe von Containern gilt im oder außerhalb des Gültigkeitsbereichs für den Import und export. Das Skript soll nur eine Liste der Knoten, die direkte untergeordnete Elemente des Stammknotens für das Skript angegeben.

Das Ermittlungsskript Hierarchie erhält folgende Parameter vom Anschluss:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.
ParentNode | [HierarchyNode][hn] | Der Stammknoten der Hierarchie, unter der das Skript direkt untergeordnete Elemente zurückgeben soll.

Das Skript muss entweder ein einzelnes untergeordnetes HierarchyNode Objekt oder eine Liste [T] mit untergeordneten HierarchyNode Objekte an die Pipeline zurückgeben.

#### <a name="import"></a>Importieren
Connectors, die Importvorgänge unterstützen müssen drei Skripts.

**Import starten**  
Begin-Skript läuft am Anfang eines Schrittes Import ausführen. In diesem Schritt können Sie eine Verbindung mit dem Quellsystem und vorbereitenden Schritte vor dem Importieren von Daten aus verbundenen System führen.

Begin-Skript empfängt den Connector die folgenden Parameter:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Informiert das Skript über den Typ der Import ausführen (Delta- oder vollständige) Partition, Hierarchie, Wasserzeichen und erwartete Größe.
Typen | [Schema][schema] | Schema im Connectorspace importiert wird.

Das Skript muss eine einzelne [OpenImportConnectionResults] zurückgeben[ oicres] Objekt in der Pipeline zum Beispiel:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Daten importieren**  
Das Datenskript wird vom Connector aufgerufen, bis das Skript zeigt an, dass keine weiteren Daten importieren. Windows PowerShell-Connector hat Seitengröße 9.999 Objekte. Wenn Ihr Skript über 9.999 Objekte für den Import zurückgibt, müssen Sie Paging unterstützt. Der Connector macht eine benutzerdefinierte Eigenschaft, einen Shop ein Wasserzeichen verwenden können, damit jedes Mal, wenn das Datenskript aufgerufen wird, wird das Skript importieren von Objekten an der Stelle fortgesetzt.

Das Datenskript empfängt die folgenden Parameter vom Anschluss:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.
GetImportEntriesRunStep | [ImportRunStep][irs] | Enthält das Wasserzeichen (Eintrag), das während verwendet ausgelagerten Einfuhren und Delta importiert.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Informiert das Skript über den Typ der Import ausführen (Delta- oder vollständige) Partition, Hierarchie, Wasserzeichen und erwartete Größe.
Typen | [Schema][schema] | Schema im Connectorspace importiert wird.

Das Datenskript muss eine Liste schreiben [[CSEntryChange][csec]] Objekt in der Pipeline. Diese Auflistung besteht aus CSEntryChange Attribute, die jedes importierte Objekt darstellen. Ausführung eines vollständigen Import müssen dieser Auflistung umfassende CSEntryChange-Objekten, die alle Attribute für jedes Objekt. Während ein Delta Import sollte CSEntryChange Objekt entweder Attribut Level Deltas für jedes Objekt importieren oder eine Darstellung der Objekte, die (Replace Mode) geändert haben.

**Ende importieren**  
Am Ende der Import ausgeführt wird Ende Import ausgeführt werden. Dieses Skript sollte alle Bereinigungsaufgaben erforderlich (z. B. Schließen Verbindungen zu Systemen) und auf Fehler auszuführen.

Skript importieren beenden erhält die folgenden Parameter vom Anschluss:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Informiert das Skript über den Typ der Import ausführen (Delta- oder vollständige) Partition, Hierarchie, Wasserzeichen und erwartete Größe.
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | Informiert das Skript über den Grund der Import beendet wurde.

Das Skript muss eine einzelne [CloseImportConnectionResults] zurückgeben[ cicres] Objekt in der Pipeline zum Beispiel:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Exportieren
Identisch mit der Import-Architektur des Connectors müssen Connectors, die exportieren unterstützt drei Skripts.

**Export starten**  
Begin ist Export am Anfang eines Schrittes Export ausführen ausgeführt werden. In diesem Schritt können Sie eine Verbindung mit dem Quellsystem und alle vorbereitenden Schritte vor dem Exportieren von Daten in verbundenen System durchführen.

Das Begin-Skript exportieren erhält folgende Parameter vom Anschluss:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Informiert das Skript über den Typ der Export ausführen (Delta- oder vollständige) Partition, Hierarchie und erwartete Größe.
Typen | [Schema][schema] | Schema im Connectorspace exportiert wird.

Das Skript sollte nicht Ausgabe an die Pipeline zurückgeben.

**Exportieren von Daten**  
Der Synchronisierungsdienst Ruft Daten exportieren Skript so oft wie erforderlich zu alle ausstehenden Exporte. Hat im Connectorspace Weitere ausstehenden Exporte Seitengröße der Connector kann Daten Exportskript mehrmals und möglicherweise mehrmals werden für das gleiche Objekt aufgerufen.

Daten Exportskript erhält folgende Parameter vom Anschluss:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.
CSEntries | IList[CSEntryChange][csec] | Liste der im Connectorspace-Objekte mit ausstehenden Exporte während dieser Phase verarbeitet werden.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Informiert das Skript über den Typ der Export ausführen (Delta- oder vollständige) Partition, Hierarchie und erwartete Größe.
Typen | [Schema][schema] | Schema im Connectorspace exportiert wird.

Exportieren Datenskript muss eine [PutExportEntriesResults] zurückgeben[ peeres] Objekt in der Pipeline. Dieses Objekt muss kein Fehler oder eine Änderung das Ankerattribut Informationen für jeden exportierten Connector einschließen. Beispielsweise ein PutExportEntriesResults-Objekt in der Pipeline zurück:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Ende exportieren**  
Nach Abschluss des Exports ausgeführt, das auszuführende Skript Ende exportieren Dieses Skript sollte alle Bereinigungsaufgaben erforderlich (z. B. Schließen Verbindungen zu Systemen) und auf Fehler auszuführen.

Exportskript Ende erhält folgende Parameter vom Anschluss:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Informiert das Skript über den Typ der Export ausführen (Delta- oder vollständige) Partition, Hierarchie und erwartete Größe.
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | Informiert das Skript über den Grund der Export beendet wurde.

Das Skript sollte nicht Ausgabe an die Pipeline zurückgeben.

#### <a name="password-synchronization"></a>Synchronisierung von Kennwörtern
Windows PowerShell-Connectors können als Ziel für Änderungen/Kennwörtern verwendet werden.

Das Skript Kennwort erhält folgende Parameter vom Anschluss:

Name | Datentyp | Beschreibung
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][Zeichenfolge [ConfigParameter][cp]] | Tabelle der Konfigurationsparameter für den Connector.
Anmeldeinformationen | [PSCredential][pscred] | Vom Administrator auf der Registerkarte Verbindung eingegebenen Anmeldeinformationen enthält.
Partition | [Partition][part] | Anwendungsverzeichnispartitionen, die in der CSEntry ist.
CSEntry | [CSEntry][cse] | Connectorspace-Eintrag für das Objekt erhalten ein Kennwort ändern oder zurücksetzen.
Vorgangstyp | Zeichenfolge | Gibt an, ob der Vorgang zurücksetzen (**SetPassword**) oder ändern (**Kennwort ändern**).
PasswordOptions | [PasswordOptions][pwdopt] | Flags, die das gewünschte Kennwort zurücksetzen Verhalten. Dieser Parameter ist nur verfügbar, wenn OperationType **SetPassword**.
OldPassword | Zeichenfolge | Altes Kennwort für das Ändern des Objekts enthält. Dieser Parameter ist nur verfügbar, wenn OperationType **ChangePassword**.
NewPassword | Zeichenfolge | Enthält das Objekt neue Kennwort, das das Skript festgelegt werden soll.

Kennwort Skript soll nicht an die Windows PowerShell-Pipeline Ergebnissen. Bei einem im Skript Kennwort Fehler sollte das Skript eine der folgenden Ausnahmen Synchronisierungsdienst über das Problem informiert auslösen:

- [PasswordPolicyViolationException] [ pwdex1] -wird ausgelöst, wenn das Kennwort die Kennwortrichtlinie nicht im verbundenen System erfüllt.
- [PasswordIllFormedException] [ pwdex2] -wird ausgelöst, wenn das Kennwort nicht verbundenen System akzeptabel ist.
- [PasswordExtension] [ pwdex3] – alle Fehler im Skript Kennwort ausgelöst.

## <a name="sample-connectors"></a>Beispiel-Anschlüsse
Eine vollständige Übersicht der verfügbaren Beispiel Connectors finden Sie unter [Windows PowerShell Connector Beispiel Connector Auflistung][samp].

## <a name="other-notes"></a>Weitere Hinweise

### <a name="additional-configuration-for-impersonation"></a>Zusätzliche Konfiguration für den Identitätswechsel
Gewähren Sie Benutzer, die die folgenden Berechtigungen auf dem Server Synchronisierungsdienst Identität:

Lesezugriff auf folgenden Registrierungsschlüssel:

- HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
- HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

Ermitteln Sie die Sicherheits-ID (SID) des Dienstkontos Synchronisierungsdienst führen Sie folgende PowerShell Befehle:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Lesezugriff auf folgenden Dateisystemordnern:

- %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Namen der Windows PowerShell-Connector für den Platzhalter {ConnectorName}.

## <a name="troubleshooting"></a>Problembehandlung

-   Informationen zum Aktivieren der Protokollierung den Connector beheben finden Sie unter [How to Enable Tracing ETW-Anschlüsse](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
