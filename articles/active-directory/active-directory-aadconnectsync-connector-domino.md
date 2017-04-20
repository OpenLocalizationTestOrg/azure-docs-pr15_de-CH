<properties
   pageTitle="Lotus Domino Connector | Microsoft Azure"
   description="Dieser Artikel beschreibt die Microsoft Lotus Domino Connector konfigurieren."
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

# <a name="lotus-domino-connector-technical-reference"></a>Lotus Domino Connector technische Referenz
Dieser Artikel beschreibt den Lotus Domino-Connector. Der Artikel gilt für die folgenden Produkte:

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Hotfix 4.1.3671.0 oder später [KB3092178](https://support.microsoft.com/kb/3092178)verwenden.

MIM2016 und FIM2010R2 ist der Connector als Download im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495)verfügbar.

## <a name="overview-of-the-lotus-domino-connector"></a>Übersicht über den Lotus Domino-Connector
Lotus Domino Connector können Sie den Synchronisierungsdienst IBM Lotus Domino-Server integriert.

Aus einer grundlegenden Perspektive die folgenden Funktionen der aktuellen Version der Connector unterstützt:

Funktion | Unterstützung
--- | ---
Verbundene Datenquelle | Server: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Client:<li>Lotus Notes 9.x</li>
Szenarien | <li>Objekt Lifecycle Management</li><li>Verwaltung</li><li>Kennwort-Management</li>
Vorgänge | <li>Vollständige und Deltaimport</li><li>Exportieren</li><li>Festlegen und Ändern von Kennwort Kennwort</li>
Schema | <li>Person (Roaming-Benutzer, Kontakt (Personen ohne Zertifikat))</li><li>Gruppe</li><li>Ressource (Raum Ressource Online-Besprechung)</li><li>Mail-Datenbank</li><li>Dynamische Erkennung von Attributen für unterstützte Objekte</li>

Lotus Domino-Connector verwendet den Lotus Notes-Client mit Lotus Domino-Server kommunizieren. Aufgrund dieser Abhängigkeit werden unterstützt Lotus Notes-Client auf dem Synchronisierungsserver installiert. Die Kommunikation zwischen dem Client und dem Server erfolgt über die Schnittstelle Lotus Notes .NET Interop (Interop.domino.dll). Diese Schnittstelle ermöglicht die Kommunikation zwischen.NET Plattform und Lotus Notes-Client und unterstützt den Zugriff auf Lotus Domino-Dokumente und Ansichten. Delta Import kann auch die systemeigene C++-Schnittstelle (je nach ausgewählten Delta Import) verwendet wird.

### <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie den Connector verwenden, sorgen Sie Folgendes auf dem Synchronisierungsserver:

- Microsoft .NET 4.5.2 Framework oder höher
- Lotus Notes-Client muss auf der Dirsync-Server installiert werden
- Lotus Domino Connector erfordert die Lotus Domino LDAP-Schema Standarddatenbank (schema.nsf) auf dem Domino-Verzeichnis-Server vorhanden sein. Wenn es nicht vorhanden ist, können Sie es ausführen oder Neustarten des LDAP-Dienstes auf dem Domino-Server installieren.

### <a name="connected-data-source-permissions"></a>Verbundene Datenquelle Berechtigungen
Um eines der unterstützten Lotus Domino Connector auszuführen, müssen Sie Mitglied der folgenden Gruppen sein:

- Administratoren Zugriff
- Administratoren
- Datenbank-Administratoren

Die folgende Tabelle enthält die Berechtigungen, die für jeden Vorgang erforderlich sind:

Vorgang | Zugriffsrechte
--- | ---
Importieren | <li>Öffentliche Dokumente lesen</li><li> Access-Administrator-Vollständig (Wenn Sie Mitglied der Gruppe Administratoren Vollzugriff haben, automatisch Ihnen effektiven Zugang zu ACL.)</li>
Exportieren und Passwort | Tatsächlicher Zugriff: <li>Dokumente erstellen</li><li>Dokumente löschen</li><li>Öffentliche Dokumente lesen</li><li>Öffentliche Dokumente schreiben</li><li>Replizieren oder Kopieren Dokumente</li>Exportvorgänge benötigen Sie auch die folgenden Funktionen: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>Benutzer</li><li>UserModifier</li>

### <a name="direct-operations-and-adminp"></a>Direkte Operationen und AdminP
Operationen gehen entweder direkt an das Domino-Verzeichnis oder durch den AdminP. Die folgenden Tabellen aller unterstützten Listenobjekte, Vorgänge und gegebenenfalls die zugehörigen Implementierungsmethode:

**Primäre Adressbuch**

Objekt | Erstellen | Update | Löschen
--- | --- | --- | ---
Person | AdminP | Direkte | AdminP
Gruppe | AdminP | Direkte | AdminP
MailInDB | Direkte | Direkte | Direkte
Ressource | AdminP | Direkte | AdminP

**Sekundäre Adressbuch**

Objekt | Erstellen | Update | Löschen
--- | --- | --- | ---
Person | N/A | Direkte | Direkte
Gruppe | Direkte | Direkte | Direkte
MailInDB | Direkte | Direkte | Direkte
Ressource | N/A | N/A | N/A

Beim Erstellen eine Ressource ist ein Notes-Dokument erstellt. Ebenso wird eine Ressource gelöscht wird, das Notes-Dokument gelöscht.

### <a name="ports-and-protocols"></a>Ports und Protokolle
Client für IBM Lotus Notes und Domino-Servern kommunizieren mit Notizen entfernte Prozedur aufrufen (NRPC), NRPC TCP verwenden soll. Die Standardportnummer 1352 ist, aber von den Domino-Administrator geändert werden.

### <a name="not-supported"></a>Nicht unterstützt
Folgende Vorgänge werden von der aktuellen Version von Lotus Domino Connector nicht unterstützt:

- Verschieben Sie Postfächer zwischen Servern.

## <a name="create-a-new-connector"></a>Erstellen eines neuen Connectors

### <a name="client-software-installation-and-configuration"></a>Client-Softwareinstallation und-Konfiguration
Lotus Notes werden auf dem Server **vor** installiert, die der Connector installiert ist.

Bei der Installation stellen Sie sicher, dass Sie einen **Einzelnen Benutzer installiert**werden. Standardmäßig **Mehrere Benutzer installieren** funktioniert nicht.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Installieren Sie auf der Seite Funktionen nur Lotus Notes Features und **Client einmalige Anmeldung**. Einmalige Anmeldung ist erforderlich für die Verbindung zum Domino-Server anmelden können.  
![Zur Installation 2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Hinweis:** Starten Sie Lotus Notes einmal mit einem Benutzer auf dem gleichen Server wie das Konto befindet wie der Connector verwendet. Stellen Sie außerdem sicher zu Lotus Notes-Client auf dem Server. Sie können gleichzeitig der Connector versucht, Verbindung zum Domino-Server ausgeführt werden.

### <a name="create-connector"></a>Connector erstellen
Erstellen Sie einen Connector für Lotus Domino versehen Sie **Synchronisierungsdienst** , **Management Agent** und **Erstellen**. Wählen Sie den Connector für **Lotus Domino (Microsoft)** .  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Wenn Ihre Version der Synchronisierungsdienst die Möglichkeit bietet, **Architektur**konfigurieren, stellen Sie sicher, dass der Connector auf den Standardwert festgelegt ist, im **Prozess**ausgeführt.

### <a name="connectivity"></a>Konnektivität
Auf der Seite Verbindung müssen den Lotus Domino-Servernamen angeben und geben Sie die Anmeldeinformationen.  
![Konnektivität](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Domino-Server-Eigenschaft unterstützt zwei Formaten für den Server:

- ServerName
- ServerName/Verzeichnisname

Das Format **ServerName/Verzeichnisname** ist das bevorzugte Format für dieses Attribut, da dadurch schnellere Reaktion, wenn der Connector den Domino-Server kontaktiert.

Die angegebenen Benutzer-ID ist in der Konfigurationsdatenbank der Synchronisierungsdienst gespeichert.

**Delta** Import müssen Sie diese Optionen:

- **Keine**. Der Connector Delta Einfuhren nicht.
- **Hinzufügen oder aktualisieren**. Der Connector ist deltaimport hinzufügen und Aktualisierungsvorgänge. Löschen benötigt einen **Vollständigen** Importvorgang. Dieser Vorgang wird .net Interop verwendet.
- **Hinzufügen, aktualisieren und löschen**. Der Connector ist deltaimport hinzufügen, aktualisieren und löschen. Dieser Vorgang verwendet die systemeigenen C++-Schnittstellen.

**Schema** -Optionen haben Sie folgenden Optionen:

- **Standard-Schema**. Der Connector erkennt das Schema aus dem Domino-Server. Dies ist die Standardoption.
- **DSML-Schema**. Nur verwendet, wenn der Domino-Server das Schema nicht verfügbar. Sie können mit dem Schema DSML-Datei erstellen und stattdessen importieren. Weitere Informationen zu DSML finden Sie auf der [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Durch Klicken auf Weiter werden die Konfigurationsparameter Benutzer-ID und das Kennwort überprüft.

### <a name="global-parameters"></a>Globale Parameter
Auf der Seite Globalparameter Konfigurieren der Zeitzone und importieren und exportieren "Vorgang".  
![Globale Parameter](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

Der **Domino Server-Zeitzone** Parameter definiert die Position des Domino-Servers.

Diese Konfigurationsoption muss **Delta import** Operationen unterstützen, da die Synchronisierung ermöglicht Dienst zwischen den letzten beiden Einfuhren bestimmen.

#### <a name="import-settings-method"></a>Importieren, Methode
Die **Vollständigen Import durchführen von** gibt es drei Optionen:

- Suche
- Anzeigen (empfohlen)

**Suche** in Domino Indizierung verwenden jedoch häufig Indizes werden nicht in Echtzeit aktualisiert, die vom Server zurückgegebenen Daten stimmt nicht immer. Bei einem System mit vielen diese Option normalerweise nicht funktionieren und False in einigen Fällen löscht bietet. **Suche** ist jedoch schneller **Anzeigen**.

**Ansicht** wird empfohlen, da es den korrekten Zustand der Daten bietet. Es ist etwas langsamer als suchen.

#### <a name="creation-of-virtual-contact-objects"></a>Erstellen von virtuellen Kontaktobjekte
Die **ermöglichen die Erstellung von \_Kontaktobjekt** gibt es drei Optionen:

- Keine
- Werte ohne Verweis
- Referenz-und ohne Verweis

Im Domino können Attribute viele unterschiedliche Formate auf andere Objekte enthalten. Varianten der Connector implementiert darstellen können \_Kontaktobjekte auch **Virtuelle Kontakte** (VC). Diese Objekte werden erstellt, sodass sie mit vorhandenen Metaverse-Objekten verknüpfen können oder als neue Objekte. Auf diese Weise können Verweise auf Elementattribute beibehalten.

Diese Einstellung aktivieren und der Inhalt ein Verweisattribut kein DN, ist ein \_Kontaktobjekt erstellt. Mitgliedsattribut einer Gruppe kann beispielsweise SMTP-Adressen enthalten. Es kann auch ShortName und andere Attribute in Attribute vorhanden. Wählen Sie für dieses Szenario **Nicht-Werte**. Diese Konfiguration ist die gebräuchlichste Einstellung für Domino-Implementierung.

Wenn Lotus Domino konfiguriert ist, müssen separate Adressbücher mit anderen distinguished Names, das dasselbe Objekt repräsentieren kann auch \_Kontaktobjekte für alle Referenzwerte, die in einem Adressbuch gefunden werden. Für dieses Szenario die Option **Referenz und nicht-Werte** .

Haben Sie mehrere Werte im Attribut **FullName** in Domino, möchten Sie auch die Erstellung virtueller Kontakte aktivieren, damit Verweise aufgelöst werden können. Beispielsweise kann dieses Attribut mehrere Werte nach einer Ehe oder Scheidung haben. Aktivieren Sie das Kontrollkästchen **aktivieren... FullName besitzt mehrere Werte** für dieses Szenario.

Auf die richtigen Attribute bei der \_Kontaktobjekte würde das MV-Objekt hinzugefügt werden.

Diese Objekte verfügen über VC =\_Kontakt ihre DN hinzugefügt.

#### <a name="import-settings-conflict-object"></a>Importieren, Konflikte Objekt
**Conflict-Objekt ausschließen**

In einer großen Domino-Implementierung kann mehrere Objekte gleichen DN durch Replikationsprobleme haben. In diesen Fällen sieht der Connector zwei Objekte mit unterschiedlichen UniversalIDs gleichen DN. Dieser Konflikt verursacht ein temporäres Objekt wird im Connectorspace erstellt. Der Connector kann Objekte ignorieren, die Domino Replikation Opfer ausgewählt wurden. Es wird empfohlen, dieses Kontrollkästchen aktiviert.

#### <a name="export-settings"></a>Export settings
Wenn die Option **AdminP aktualisiert verwenden** deaktiviert ist, Export Attribute, wie, einen direkten Aufruf und AdminP Prozess nicht verwenden. Verwenden Sie diese Option nur, wenn AdminP nicht konfiguriert wurde, um die referenzielle Integrität.

#### <a name="routing-information"></a>Routing-Informationen
In Domino ist es möglich, dass ein Verweisattribut Routinginformationen als Suffix der DN. Das Mitgliedsattribut einer Gruppe kann beispielsweise **CN=example/organization@ABC**. Das Suffix @ABC wird die routing-Informationen. Die Routinginformationen Domino dient das richtige Domino-System e-Mails an die ein System in einer anderen Organisation. Geben Sie im Feld Routing-Informationen innerhalb der Organisation im Bereich des Connectors routing Suffixe. Wenn einer dieser Werte in ein Verweisattribut als Suffix gefunden wird, wird die Routinginformationen aus der Referenz entfernt. Wenn einer dieser Werte angegeben, Routinggruppe Suffix auf einen Referenzwert zugeordnet werden kann ein \_Kontaktobjekt erstellt. Diese \_Kontakt Objekte werden mit ** RO=@ ** der DN eingefügt. Für diese \_Kontaktobjekten folgende Attribute werden zusätzlich können bei Bedarf auf ein tatsächliches Objekt verknüpfen: \_RoutingName, \_ContactName, \_DisplayName und UniversalID.

#### <a name="additional-address-books"></a>Zusätzliche Adressbücher
Haben Sie keine **Nebenstellen** installiert, mit den Namen der sekundären Adressbücher können diese Adressbücher manuell eingeben.

#### <a name="multivalued-transformation"></a>Mehrwertige Transformation
Viele Attribute in Lotus Domino sind mehrwertig. Die entsprechenden Metaverse-Attribute werden in der Regel einzelnen bewertet. Konfigurieren Sie den Import und Export Vorgang Option, ermöglichen den Connector die erforderliche Übersetzung die betroffenen Attribute zu.

**Exportieren**  
Die Exportoption Vorgang unterstützt zwei Modi:

- Element anfügen
- Element ersetzen

**Artikel ersetzen** – bei Auswahl dieser Option die Verbindung immer die aktuellen Werte des Attributs in Domino und ersetzt werden mit den angegebenen Werten. Die Werte können ein- oder mehrwertig.

Beispiel: Assistent Attribut eines Personenobjekts besitzt die folgenden Werte:

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Wenn ein neuer Assistent mit dem Namen **David Alexander** dieser Person-Objekt zugeordnet ist, ist das Ergebnis:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Element anfügen** – bei Auswahl dieser Option den Connector behält die vorhandenen Werten für das Attribut in Domino und neue Werte am Anfang der Liste.

Beispiel: Assistent Attribut eines Personenobjekts besitzt die folgenden Werte:

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Wenn ein neuer Assistent mit dem Namen **David Alexander** dieser Person-Objekt zugeordnet ist, ist das Ergebnis:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Importieren**  
Die Importoption Vorgang unterstützt zwei Modi:

- Standard
- Mehrwertige Einzelwert

**Standard** – Wenn Sie die Standardoption auswählen, werden alle Werte aller Attribute importiert.

Ein einwertiges Attribut **Multivalued einzelnen Wert** – bei Auswahl dieser Option ein mehrwertiges Attribut umgewandelt. Wenn mehrere Werte vorhanden ist, wird der Wert oben (dieser Wert ist in der Regel auch die neuesten) verwendet.

Beispiel: Assistent Attribut eines Personenobjekts besitzt die folgenden Werte:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Das neueste Update für dieses Attribut ist **David Alexander**. Da die Operation Importoption Multivalued einzelnen Wert festgelegt ist, importiert Connector nur **David Alexander** in Connectorspace.

Die Logik für mehrwertige Attribute in Attributen mit Einzelwerten konvertieren gilt nicht das Mitgliedsattribut der Gruppe und die Person Fullname-Attribut.

Es auch möglich, konfigurieren Sie importieren und Exportieren von Transformation Regeln für mehrwertige Attribute pro Attribut als Ausnahme globale Regel. Um diese Option zu konfigurieren, geben Sie [Objekttyp]. [Attributname] in den Textfeldern **Attribut Ausschlussliste importieren** und **Exportieren von Attribut Ausschlussliste** . Wenn Sie Person.Assistant eingeben und das globale Flag auf alle Werte importieren, wird nur der erste Wert für den Assistenten importiert.

#### <a name="certifiers"></a>Zertifizierer
Organisation/Organisationseinheit alle aufgelisteten vom Connector. Um Person Objekte in den primären Adressbuch exportieren zu können, ist eine Zulassungsstelle und das Kennwort erforderlich.

Wenn alle Zertifizierer dasselbe Kennwort verfügen, kann das **Kennwort für alle Certifers** verwendet werden. Geben Sie das Kennwort, und nur die Zulassungsstelle Datei angeben.

Wenn Sie nur importieren, haben Sie keine Zertifizierer angeben.

### <a name="configure-provisioning-hierarchy"></a>Konfigurieren der Bereitstellung Hierarchie
Wenn Sie Lotus Domino Connector konfigurieren, überspringen Sie diese Seite Der Connector für Lotus Domino unterstützt keine Hierarchie bereitstellen.  
![Bereitstellung der Hierarchie](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Konfigurieren von Partitionen und Hierarchien
Beim Konfigurieren von Partitionen und Hierarchien müssen Sie das primäre Adressbuch NAB=names.nsf auswählen. Neben der primären Adressbuch können Sie sekundäre Adressbücher auswählen, falls vorhanden.  
![Partitionen](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Wählen Sie Attribute
Beim Konfigurieren der Attribute müssen Sie alle Attribute, die mit dem Präfix auswählen ** \_MMS\_**. Diese Attribute sind bei der Bereitstellung neuer Objekte an Lotus Domino

![Attribute](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Objekt Lifecycle Management
Dieser Abschnitt enthält eine Übersicht über die verschiedenen Objekte in Domino.

### <a name="person-objects"></a>Person-Objekte
Das Person-Objekt repräsentiert Benutzer in Unternehmen und Organisationseinheiten. Neben der Standardattribute kann Domino Administrator benutzerdefinierte Attribute eines Person-Objekts hinzufügen. Eine Person-Objekt muss mindestens alle erforderlichen Attribute enthalten. Eine vollständige Liste der obligatorischen Attribute finden Sie unter [Lotus Notes-Eigenschaften](#lotus-notes-properties). Registrieren Sie eine Person-Objekt müssen die folgenden erforderlichen Komponenten erfüllt sein:

- Adressbuch (names.nsf) definiert und sollte die primäre Adressbuch.
- Sie müssen O-OU Certifier Id und das Kennwort ein bestimmtes Benutzers in der Organisation registrieren / Organisationseinheit.
- Sie müssen einen bestimmten Satz von Lotus Notes-Eigenschaften für ein Person-Objekt festlegen. Diese Eigenschaften werden für die Bereitstellung des Person-Objekts verwendet. Weitere Informationen finden Sie unter [Lotus Notes-Eigenschaften](#lotus-notes-properties) im Abschnitt weiter unten in diesem Dokument.
- Das ursprüngliche Kennwort für eine Person ist ein Attribut und ein Satz während der Bereitstellung.
- Das Person-Objekt muss einen der drei unterstützten Typen:
    1. Normale Benutzer mit e-Mail-Datei und eine Benutzer-ID-Datei
    2. Roaming-Benutzer (ein normaler Benutzer, die alle roaming Datenbankdateien enthält)
    3. Kontakte (Benutzer ohne Id-Datei)

(Außer Kontakte) können weiter in uns und internationale Benutzer gruppiert werden, gemäß den Wert der \_MMS\_IDRegType-Eigenschaft. Diese Personen den Notes-Client auf Lotus Domino-Server nutzen eine Notes-Id und ein Personendokument. Wenn sie Notes Mail verwenden, haben sie auch eine e-Mail-Datei. Der Benutzer muss registriert sein, um aktiv. Weitere Informationen finden Sie unter:

- [Notes-Benutzer einrichten](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [Registrierung](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [Verwalten von Benutzern](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [Benutzer umbenennen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Diese Vorgänge werden in Lotus Domino und in der Synchronisierungsdienst importiert.

### <a name="resources-and-rooms"></a>Räume und Ressourcen
Eine Ressource ist eine andere Art von einer Datenbank in Lotus Domino. Ressourcen können mit verschiedenen Arten von Geräten wie Projektoren werden. Untertypen von Connector für Lotus Domino unterstützt Ressourcen, die vom Resource Type-Attribut definiert sind:

Ressourcentyp | Ressourcentyp-Attribut
--- | ---
Raum | 1
Ressource (andere) | 2
Online-Besprechung | 3

Für die Ressource Objekttyp arbeiten ist Folgendes erforderlich:

- Resource Reservation-Datenbank sollte in verbundenen Domino-Servers vorhanden.
- Die Site ist bereits für die Ressource definiert.

Resource Reservation-Datenbank enthält drei Arten von Dokumenten:

- Website-Profil
- Ressource
- Reservierung

Weitere Informationen zum Einrichten von Resource Reservation-Datenbank finden Sie unter [Datenbank Ressource Reservierung](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Erstellen, aktualisieren und Löschen von Ressourcen**  
Create, Update und Delete-Operationen werden von Lotus Domino Connector Resource Reservation-Datenbank ausgeführt. Ressourcen werden als Dokumente im ACL (d. h. die primäre Adressbuch) erstellt. Weitere Informationen zum Bearbeiten und Löschen von Ressourcen finden Sie unter [Bearbeiten und Löschen von Dokumenten Ressource](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Importieren und Exportieren von Ressourcen**  
Ressourcen können importiert und exportiert vom Synchronisierungsdienst wie alle anderen Objekttypen. Wählen Sie den Objekttyp als Ressource während der Konfiguration. Für den erfolgreichen Export Betrieb haben Sie Details für Ressourcentyp, Konferenzdatenbank und Standortnamen.

### <a name="mail-in-databases"></a>Mail-Datenbanken
Eine E-Mail-Datenbank ist eine Datenbank, die e-Mails empfangen soll. Es ist ein Lotus Domino-Postfach, das nicht mit bestimmten Lotus Domino-Benutzerkonto verknüpft ist (d. h. es keinen eigenen Datei ID und Kennwort). Eine e-Mail-Datenbank hat eine eindeutige Benutzer-ID ("short Name") und eine eigene e-Mail-Adresse zugeordnet.

Besteht Bedarf für ein separates Postfach mit eigenen e-Mail-Adresse, die für andere Benutzer freigegeben werden kann (z. B. group@contoso.com), eine Mail-Datenbank erstellt. Der Zugriff auf dieses Postfach erfolgt über die Zugriffssteuerungsliste (ACL), enthält die Namen der Notes-Benutzer, die das Postfach zu öffnen.

Eine Liste der erforderlichen Attribute finden Sie im Abschnitt [Erforderliche Attribute](#mandatory-attributes) in diesem Artikel.

Wenn eine Datenbank eine e-Mail empfangen soll, wird ein Dokument Mail-Datenbank in Lotus Domino erstellt. Dieses Dokument muss im Domino-Verzeichnis aller Server vorhanden, die eine Kopie der Datenbank gespeichert. Eine ausführliche Beschreibung zum Erstellen eines Dokuments für die Mail-Datenbank finden Sie in der [Mail-Datenbank Dokumente erstellen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Vor dem Erstellen einer E-Mail In die Datenbank sollte bereits vorhanden sein (sollte wurde erstellt von Lotus Admin) auf dem Domino-Server.

### <a name="group-management"></a>Verwaltung
Eine ausführliche Übersicht über die Lotus Domino-Verwaltung erhalten in den folgenden Ressourcen:

- [Verwenden von Gruppen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [Erstellen einer Gruppe](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [Erstellen und Ändern von Gruppen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [Verwalten von Gruppen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [Gruppe umbenennen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Kennwort-Management
Für Lotus Domino registriert gibt es zwei Arten von Kennwörtern:

1. Benutzerkennwort (gespeichert in Benutzer.ID-Datei)
2. Internet / Kennwort

Der Connector für Lotus Domino unterstützt nur Operationen mit Kennwort.

Passwort-Management ausgeführt werden, sollten Sie Kennwort-Management für den Connector in der Management Agent-Designer aktivieren. Aktivieren Sie Kennwort-Management aktivieren Sie **Kennwort-Management** auf der **Erweiterung konfigurieren** .  
![Konfigurieren von Erweiterungen](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Die folgenden Vorgänge auf Internet-Kennwort Lotus Domino Connector-Unterstützung:

- Kennwort: Das Kennwort wird ein neues HTTP-Internet Benutzer in Domino. Standardmäßig ist das Konto entsperrt. Unlock-Flag wird auf die WMI-Schnittstelle das Synchronisierungsmodul verfügbar gemacht.
- Kennwort ändern: In diesem Szenario ein Benutzer das Kennwort ändern möchten oder wird aufgefordert, das Kennwort nach einer bestimmten Zeit ändern. Für diesen Vorgang zu setzen, sowohl das alte und das neue Kennwort sind obligatorisch. Nachdem geändert, wird das neue Kennwort in Lotus Domino aktualisiert.

Weitere Informationen finden Sie unter:

- [Verwenden die Sperrfunktion Internet](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [Internet-Kennwörter verwalten](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Informationen
Dieser Abschnitt listet Attribut Beschreibung und Attribute an den Connector für Lotus Domino.

### <a name="lotus-notes-properties"></a>Lotus Notes-Eigenschaften
Bei der Bereitstellung von Person-Objekten in das Lotus Domino-Verzeichnis müssen Objekte eine bestimmte Gruppe von Eigenschaften mit bestimmten Werten aufgefüllt. Diese Werte müssen nur für Vorgänge erstellen.

In der folgenden Tabelle sind die Eigenschaften aufgeführt und beschrieben werden.

Eigenschaft | Beschreibung
--- | ---
\_MMS_AltFullName | Die alternative vollständigen Namen des Benutzers.
\_MMS_AltFullNameLanguage | Die Sprache zum Angeben alternativen vollständigen Namens des Benutzers.
\_MMS_CertDaysToExpire | Die Anzahl der Tage ab dem aktuellen Datum vor dem Zertifikat abläuft. Wenn nicht angegeben, wird standardmäßig zwei Jahre ab dem aktuellen Datum.
\_MMS_Certifier | Eigenschaft, die die Organisationshierarchie die Zulassungsstelle enthält. Beispiel: OU = OrganizationUnit, O = Org, C = Land.
\_MMS_IDPath | Wenn die Eigenschaft leer ist, entsteht kein Benutzer ID-Datei lokal auf dem Server synchronisieren. Wenn die Eigenschaft einen Namen enthält, wird eine Benutzer-ID-Datei im Ordner Madata erstellt. Die Eigenschaft kann auch einen vollständigen Pfad enthalten.
\_MMS_IDRegType | Personen können Kontakte uns Benutzer und Benutzer klassifiziert werden. In der folgenden Tabelle sind die möglichen Werte aufgeführt: <li>0 - Kontakt</li><li>1 - USA Benutzer</li><li>2 - internationale</li>
\_MMS_IDStoreType | Erforderliche Eigenschaft für USA und internationale Benutzer. Die Eigenschaft enthält einen ganzzahligen Wert, der angibt, ob der Benutzer-ID als Anlage in das Notes-Adressbuch oder die Person e-Mail-Datei gespeichert ist. Ist die Benutzer-ID-Datei eine Anlage im Adressbuch, optional erstellt werden als mit \_MMS_IDPath. <li>Leer - Nachrichtenspeicher-ID-Datei im ID Vault keine ID-Datei (für Kontakte).</li><li> 1 - Anlage in das Notes-Adressbuch. Die \_MMS_Password-Eigenschaft muss festgelegt werden, für Benutzer ID Dateien anhängen</li><li>2 - speichern Sie ID in einer Person e-Mail-Datei. Die \_MMS_UseAdminP muss auf False festgelegt werden, die e-Mail-Datei während der Registrierung Person erstellt werden können. Die \_MMS_Password-Eigenschaft für Benutzerdateien Kennung festgelegt sein.</li>
\_MMS_MailQuotaSizeLimit | Die Anzahl der Megabyte für e-Mail-Datenbankdatei zulässig sind.
\_MMS_MailQuotaWarningThreshold | Die Anzahl der Megabyte für e-Mail-Datenbankdatei zulässig sind, bevor eine Warnung ausgegeben wird.
\_MMS_MailTemplateName | E-Mail-Vorlagendatei, die zum Erstellen der e-Mail-Datei. Wenn eine Vorlage angegeben wird, wird die e-Mail-Datei mit der angegebenen Vorlage erstellt. Wenn keine Vorlage angegeben ist, wird die Standard-Vorlagendatei zum Erstellen der Datei verwendet.
\_MMS_OU | Optionale Eigenschaft, die den Namen der Organisationseinheit die Zulassungsstelle. Diese Eigenschaft sollte für Kontakte.
\_MMS_Password | Erforderliche Eigenschaft für Benutzer. Die Eigenschaft enthält das Kennwort für die ID-Datei des Objekts.
\_MMS_UseAdminP | -Eigenschaft fest auf true, wenn die e-Mail-Datei vom auf dem Domino-Server AdminP-Prozess (asynchronous Exportvorgang) erstellt werden soll. Wenn-Eigenschaft auf False festgelegt ist, wird die e-Mail-Datei mit dem Domino-Benutzer (synchron in den Exportvorgang) erstellt.

Ein Benutzer mit einer zugeordneten ID-Datei die \_MMS_Password-Eigenschaft muss einen Wert enthalten. Für den e-Mail-Zugriff über den Lotus Notes-Client müssen laufen und MailFile Eigenschaften eines Benutzers einen Wert enthalten.

Für den Zugriff auf e-Mails über einen Webbrowser müssen die folgenden Werte enthalten:

- MailFile - erforderliche Eigenschaft mit dem Pfad auf dem Lotus Domino-Server, die e-Mail-Datei gespeichert ist.
- MailServer - erforderliche Eigenschaft mit dem Namen des Lotus Domino-Servers. Dieser Wert ist der Name, verwenden Sie die Lotus e-Mail-Datei auf dem Domino-Server.
- HTTPPassword - optionale Eigenschaft, die das Web-Passwort für das Objekt enthält.

Zugriff auf den Domino-Server ohne e-Mail muss die HTTPPassword-Eigenschaft einen Wert enthalten. MailFile-Eigenschaft und die MailServer-Eigenschaft können leer sein.

Mit \_MMS_ IDStoreType = 2 (Speicher-Id im e-Mail-Datei), das System der NotesRegistrationclass-Eigenschaft auf REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Erforderliche Attribute
Der Connector für Lotus Domino unterstützt in erster Linie diese Objekttypen (Dokumenttypen):

- Gruppe
- Mail-Datenbank
- Person
- Contact (Person keine Zertifizierer)
- Ressource

Dieser Abschnitt listet die Attribute für jedes unterstützte Objekt in einem Domino-Server exportieren.

Objekttyp | Erforderliche Attribute
--- | ---
Gruppe | <li>Listenname</li>
Main-Datenbank | <li>FullName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>
Person | <li>Nachname</li><li>MailFile</li><li>ShortName</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li>
Contact (Person keine Zertifizierer) | <li>\_MMS_IDRegType</li>
Ressource | <li>FullName</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Standort</li><li>DisplayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>

## <a name="common-issues-and-questions"></a>Häufige Probleme und Fragen

### <a name="schema-detection-does-not-work"></a>Schema-Erkennung funktioniert nicht
Um das Schema erkennen können, ist es erforderlich, die schema.nsf auf dem Domino-Server vorhanden ist. Diese Datei wird nur angezeigt, wenn LDAP auf dem Server installiert ist. Wenn das Schema nicht nachweisbar ist, überprüfen Sie Folgendes:

- Die Datei schema.nsf ist im Stammordner des Domino-Servers vorhanden
- Der Benutzer hat Berechtigungen für die Datei schema.nsf.
- Neustart des LDAP-Servers. **Lotus Domino-Konsole** öffnen und **Sagen LDAP-ReloadSchema** Befehl Schema neu laden.

### <a name="not-all-secondary-address-books-are-visible"></a>Nicht alle sekundären Adressbücher sind sichtbar
Domino Connector verwendet das Feature **Nebenstellen** sekundären Adressbüchern finden. Fehlen die sekundäre Adressbücher überprüfen Sie, ob [Nebenstellen](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) aktiviert und auf dem Domino-Server konfiguriert wurde.

### <a name="custom-attributes-in-domino"></a>Benutzerdefinierte Attribute in Domino
Gibt es verschiedene Arten in Domino, das Schema erweitern, wird es als benutzerdefiniertes Attribut vom Connector konsumierbare angezeigt.

**Methode 1: Lotus Domino-Schema erweitern**

1. Erstellen Sie eine Kopie der Domino Directory Vorlage {PUBNAMES. NTF} (Sie sollten nicht anpassen das Standardverzeichnis für IBM Lotus Domino Vorlage) folgende [Schritte](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) :
2. Öffnen Sie die Kopie des Domino Directory Vorlage {CONTOSO. NTF}-Vorlage, die im Domino-Designer erstellt wurde, gehen Sie folgendermaßen vor:
    - Klicken Sie auf freigegebene Elemente und erweitern Unterformulare
    - Doppelklicken Sie auf InheritableSchema Unterformular ${ObjectName} ({ObjectName} ist der Name der Standardklasse strukturellen beispielsweise: Person).
    - Name-Attribut zu Schema {MyPersonAtrribute} entspricht das Attribut hinzufügen möchten. Erstellen eines Feldes wählen Sie im Menü **Erstellen** , und wählen Sie dann im **Feld** .
    - Das hinzugefügte Feld legen Sie deren Eigenschaften Art, Stil, Größe, Schriftart und andere Parameter im Feld Eigenschaftenfenster auswählen.
    - Das-Attribut Standardwert, dasselbe wie der Name für das Attribut (z. B. wenn Attributname MyPersonAttribute, behalten Sie den Standardwert mit demselben Namen).
    - Speichern Sie ${ObjectName} InheritableSchema Unterformular mit aktualisierten Werte.
3. Ersetzen Sie die Domino-Verzeichnis Vorlage {PUBNAMES. NTF} mit der neuen benutzerdefinierten Vorlage {CONTOSO. NTF} folgende [Schritte](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Schließen Sie Domino Administrator und öffnen Sie der Domino-Konsole den LDAP-Dienst neu starten und das LDAP-Schema neu laden:
    - Fügen Sie im Domino-Konsole den Befehl unter **Domino** Befehlstext den LDAP-Dienst - [Neustart Aufgabe LDAP]( http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)gespeichert ein.
    - Um LDAP laden Schema Befehl sagen LDAP - LDAP-ReloadSchema erkennen
5. Person hinzufügen öffnen Domino Administrator und Registerkarte auswählen & & Gruppen hinzugefügte Attribut in Domino reflektiert wird.
6. Schema.nsf **Register** öffnen und hinzugefügte Attribut in der LDAP-Objektklasse DominoPerson reflektiert wird.

**Ansatz 2: Eine AuxClass mit benutzerdefinierten Attribut erstellen und Zuordnen der Objektklasse**

1. Erstellen Sie eine Kopie der Domino Directory Vorlage {PUBNAMES. NTF} folgende [Schritte](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (nicht das Standardverzeichnis für IBM Lotus Domino Vorlage anpassen):
2. Öffnen Sie die Kopie des Domino Directory Vorlage {CONTOSO. NTF}-Vorlage, die im Domino-Designer erstellt wurde.
3. Wählen Sie im linken Bereich Code freigegeben und Unterformulare.
4. Klicken Sie auf neues Unterformular
5. Gehen Sie die Eigenschaften für das neue Unterformular festlegen:
    - Neues Unterformular geöffnet ist, wählen Sie Design - Unterformular-Eigenschaften
    - Neben der Eigenschaft Name Geben Sie einen Namen für die zusätzlichen Objektklasse – z. B. TestSubform.
    - Behalten Sie die Optionen-Eigenschaft "Einschließen im Unterformular einfügen... Dialogfeld" aktiviert
    - Deaktivieren Sie die Optionen-Eigenschaft "Renderns über HTML in Notes."
    - Lassen Sie den anderen Eigenschaften gleich, und schließen Sie das Unterformular Eigenschaften.
    - Speichern Sie und schließen Sie das neue Unterformular.
6. Gehen Sie zum Hinzufügen eines zusätzlichen Objektklasse definieren:
    - Öffnen Sie das Unterformular, das Sie erstellt haben.
    - Wählen Sie erstellen - Feld.
    - Geben Sie neben Name auf der Registerkarte "Grundlagen" im Feld Namen, z.B.: {MyPersonTestAttribute}.
    - Das hinzugefügte Feld legen Sie deren Eigenschaften von Typ, Format, Größe, Schriftart und zugehörigen Eigenschaften auswählen.
    - Das-Attribut Standardwert, dasselbe wie der Name für das Attribut (z. B. wenn Attributname MyPersonTestAttribute, behalten Sie den Standardwert mit demselben Namen).
    - Speichern Sie das Unterformular mit aktualisierten Werte und gehen Sie folgendermaßen vor:
        - Wählen Sie im linken Bereich Code freigegeben und Unterformulare
        - Wählen Sie neue Unterformular, und Design - Eigenschaften.
        - Klicken Sie auf der dritten Registerkarte links, und wählen Sie **übertragen dieses Verbot Entwurf ändern**.
7. ${ObjectName} ExtensibleSchema Unterformular ({ObjectName} Name der strukturellen Standardklasse, beispielsweise Person ist) zu öffnen.
8. Fügen Sie Ressource ein und wählen Sie das Unterformular (die Sie, beispielsweise TestSubform erstellt) und speichern Sie das Unterformular ${ObjectName} ExtensibleSchema.
9. Ersetzen Sie die Domino-Verzeichnis Vorlage {PUBNAMES. NTF} mit der neuen benutzerdefinierten Vorlage {CONTOSO. NTF} folgende [Schritte](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Schließen Sie Domino Administrator und öffnen Sie der Domino-Konsole den LDAP-Dienst neu starten und das LDAP-Schema neu laden:
    - Fügen Sie im Domino-Konsole den Befehl unter **Domino** Befehlstext den LDAP-Dienst - [Neustart Aufgabe LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)gespeichert ein.
    - LDAP-Schema verwenden sagen LDAP-Befehl **Sagen LDAP-ReloadSchema**erneut laden.
11. Domino-Administrator öffnen, und wählen Sie Benutzer und Gruppen Registerkarte hinzugefügte Attribut wirkt sich Domino Person hinzufügen (unter andere Registerkarte).
12. Schema.nsf **Register** öffnen und hinzugefügte Attribut unter TestSubform LDAP-Objekt Erweiterungsklasse reflektiert wird.

**Methode 3: Die ExtensibleObject-Klasse das benutzerdefinierte Attribut hinzufügen**

1. {Schema.nsf} öffnen im Stammverzeichnis platziert
2. Klicken Sie LDAP-Objektklassen aus dem linken Menü **alle Schema** Dokumente und dann auf **Klasse hinzufügen** :
3. Geben Sie LDAP-Namen in Form von {ZzzExtensibleSchema} (Zzz Name der strukturellen Standardklasse, z. B. Person ist). Um das Schema für die Person Objektklasse zu erweitern, geben Sie z. B. LDAP an {PersonExtensibleSchema}.
4. Geben Sie übergeordnete Objekt Klasse an, das Schema erweitert werden soll. Zum Beispiel erweitert das Schema für die Person Objektklasse bereitstellen Sie Superior der Name {DominoPerson}:
5. Geben Sie eine zulässige OID, Object-Klasse entspricht.
6. Wählen Sie erweiterte/benutzerdefinierte Attribute erforderlich oder Optional Attributtypen Felder nach der Anforderung:
7. Klicken Sie nachdem die ExtensibleObjectClass erforderlichen Attribute hinzufügen auf **Speichern und schließen**.
8. Ein ExtensibleObjectClass wird für die entsprechenden Standard-Objektklasse mit erweiterten Attributen erstellt.

## <a name="troubleshooting"></a>Problembehandlung

-   Informationen zum Aktivieren der Protokollierung den Connector beheben finden Sie unter [How to Enable Tracing ETW-Anschlüsse](http://go.microsoft.com/fwlink/?LinkId=335731).
