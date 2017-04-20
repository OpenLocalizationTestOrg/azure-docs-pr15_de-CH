<properties 
    pageTitle="Verzeichnisintegration zwischen Azure mehrstufige Authentifizierung und Active Directory"
    description="Dies ist die Seite Azure mehrstufige Authentifizierung, die beschreibt, wie Azure mehrstufige Authentifizierungsserver in Active Directory integrieren, damit die Verzeichnisse zu synchronisieren."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Verzeichnisintegration zwischen Azure MFA-Server und Active Directory

Abschnitt Directory können Sie zum Konfigurieren des Servers für die Integration von Active Directory oder einem anderen LDAP-Verzeichnis.  Dadurch werden konfigurieren Attribute entspricht das Verzeichnisschema und automatische Synchronisierung der Benutzer einrichten.

## <a name="settings"></a>Einstellungen
Standardmäßig sieht Azure mehrstufige Authentifizierungsserver importieren oder Benutzer aus Active Directory zu synchronisieren.  Die Registerkarte können Sie das Standardverhalten überschreiben und zum Binden an ein anderes LDAP-Verzeichnis, ADAM-Verzeichnis oder bestimmte Active Directory-Domänencontroller.  Darüber hinaus für die Verwendung der LDAP-Authentifizierung Proxy LDAP oder LDAP-Bindung als RADIUS-Ziel Vorauthentifizierung für IIS-Authentifizierung oder die Authentifizierung für User Portal.  Die folgende Tabelle beschreibt die einzelnen Einstellungen.

![Einstellungen](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Funktion | Beschreibung |
| ------- | ----------- |
| Verwenden Sie Active Directory | Die Option Active Directory mithilfe von Active Directory für Import und Synchronisierung.  Dies ist die Standardeinstellung. <br>Hinweis: Der Computer muss werden zu einer Domäne hinzugefügt und müssen Sie mit einem Domänenkonto für die Integration von Active Directory ordnungsgemäß angemeldet werden. |
| Vertrauenswürdige Domänen enthalten | Überprüfen der aktuellen Domäne, einer anderen Domäne in der Gesamtstruktur und Domänen das vertrauenswürdigen Domänen enthalten Kontrollkästchen, damit den Agent Verbindungsversuch mit Domänen vertrauen beteiligt eine Gesamtstruktur-Vertrauensstellung.  Wenn nicht importieren oder Benutzer aus vertrauenswürdigen Domänen synchronisieren, deaktivieren Sie das Kontrollkästchen zur Leistungssteigerung.  Die Standardeinstellung ist aktiviert. |
| LDAP-Konfiguration verwenden | Die Option LDAP verwenden die LDAP-Einstellungen für Import und Synchronisierung angegeben. Hinweis: Wenn LDAP verwenden aktiviert ist, wird die Benutzeroberfläche Verweise aus Active Directory auf LDAP. |
| Schaltfläche Bearbeiten | Die Schaltfläche Bearbeiten ermöglicht der aktuellen LDAP-Konfiguration geändert. |
| Attribut Bereich Abfragen | Gibt an, ob Bereichsabfragen Attribut verwendet werden soll.  Attribut Bereichsabfragen ermöglichen effiziente Verzeichnissuche Datensätze basierend auf den Einträgen in einem anderen Datensatz Attribut qualifiziert.  Azure mehrstufige Authentifizierungsserver verwendet Attribut Bereichsabfragen effizient Benutzer Abfragen, die Mitglied einer Sicherheitsgruppe sind.   <br>Hinweis: Es gibt Fälle, Attribut Bereichsabfragen werden unterstützt, aber nicht verwendet werden.  Beispielsweise haben Active Directory Probleme bei Attributabfragen Bereich eine Sicherheitsgruppe Elemente aus mehreren Domänen enthält.  In diesem Fall sollte das Kontrollkästchen deaktiviert sein. |

Die folgende Tabelle beschreibt die LDAP-Konfigurationsdateien.

| Funktion | Beschreibung |
| ------- | ----------- |
| Server | Geben Sie den Hostnamen oder die IP-Adresse des Servers mit dem LDAP-Verzeichnis.  Ein Sicherungsserver kann auch durch ein Semikolon getrennt angegeben werden. <br>Hinweis: Beim Binden SSL ist, muss ein vollständig qualifizierten Hostnamen im Allgemeinen. |
| Basis-DN | Geben Sie den DN des Basisverzeichnisses Objekt aus dem alle Verzeichnisabfragen startet.  Z. B. dc = Abc, dc = com. |
| Bindungstyp - Abfragen | Wählen Sie den entsprechenden Bindung zur Verwendung beim Binden an das LDAP-Verzeichnis durchsuchen.  Dies ist für Einfuhren, Synchronisierung und Benutzername Auflösung verwendet. <br><br>  Anonym - eine anonyme Bindung durchgeführt wird.  Bind-DN und Verbindungskennwort wird nicht verwendet.  Dies funktioniert nur, wenn das LDAP-Verzeichnis anonyme Bindung und Berechtigungen ermöglichen das Abfragen der entsprechenden Einträge und Attribute.  <br><br> Einfach - Bind-DN und binden Kennwort werden als Klartext zum Binden an das LDAP-Verzeichnis weitergegeben.  Dies sollte nur verwendet werden für Testzwecke, stellen Sie sicher, dass der Server erreichbar und Bind-Konto den Zugriff.  Es wird empfohlen, stattdessen SSL verwendet werden, nachdem das entsprechende Zertifikat installiert wurde.  <br><br> SSL - Bind-DN und das Kennwort gebunden werden mit SSL zum Binden an das LDAP-Verzeichnis verschlüsselt.  Dazu muss ein Zertifikat werden lokal installiert, das LDAP-Verzeichnis vertraut.  <br><br> Windows - Bindung Benutzername und Kennwort gebunden wird das sichere Verbinden mit einem Active Directory-Domänencontroller oder ADAM-Verzeichnis verwendet.  Wenn binden Benutzername leer ist, wird das angemeldete Benutzerkonto Binden verwendet. |
| Typ - Authentifizierungen binden | Wählen Sie die entsprechende Bindung für die Verwendung beim LDAP-Bind-Authentifizierung.  Siehe Beschreibung unter Art der Bind - Abfragen geben binden.  Sie können beispielsweise für anonyme Bindung an SSL-Bindung verwendet, um sichere LDAP Bind Authentifizierungen für Abfragen verwendet werden. |
| Bind-DN oder Bind-Benutzername | Geben Sie den DN des Benutzerdatensatzes für das Konto bei der Bindung an das LDAP-Verzeichnis verwenden.<br><br>Bind-DN wird nur verwendet, wenn binden einfacher oder SSL ist.  <br><br>Geben Sie den Benutzernamen des Windows-Kontos verwendet beim Binden an das LDAP-Verzeichnis, wenn binden Windows ist.  Wenn leer, wird das angemeldete Benutzerkonto Binden verwendet. |
| Binden Sie Kennwort | Geben Sie das Kennwort Bind für die Bind-DN oder Benutzername für die Bindung an das LDAP-Verzeichnis.  Um das Kennwort für die kombinierte Authentifizierung Serverdienst AdSync, Synchronisierung muss aktiviert sein, und der Dienst auf dem lokalen Computer ausgeführt werden.  Das Kennwort wird unter dem Konto als der kombinierte Authentifizierung AdSync Serverdienst ausgeführt wird, in Windows gespeicherte Benutzernamen und Kennwörter gespeichert werden.  Das Konto, das als Benutzeroberfläche mehrstufige Authentifizierung Server ausgeführt wird und das Konto, dem als kombinierte Auth-Serverdienst ausgeführt wird, wird auch das Kennwort gespeichert.  <br><br> Hinweis: Da nur des lokalen Servers Windows gespeicherte Benutzernamen und Kennwörter gespeichert werden, dabei müssen auf jedem Server mehrstufige Authentifizierung erfolgen, die an. |
| Abfrage-Größenlimit | Geben Sie das Limit für die maximale Anzahl der Benutzer, die eine Suche im Verzeichnis zurückgegeben wird.  Dieser Grenzwert sollte die Konfiguration auf dem LDAP-Verzeichnis übereinstimmen.  Suche große, Paging nicht unterstützt wird, versucht Import und Synchronisierung, Benutzer in Gruppen abrufen.  Wenn die Maximalgröße hier ist größer als der Grenzwert für das LDAP-Verzeichnis konfiguriert angegeben, können einige Benutzer übersehen werden. |
| Schaltfläche "testen" | Klicken Sie auf Test, um die Bindung mit dem LDAP-Server testen.  <br><br> Hinweis: Die Option LDAP verwenden muss nicht ausgewählt werden, um die Bindung zu testen.  Dies ermöglicht die Bindung vor der LDAP-Konfiguration getestet werden. |

## <a name="filters"></a>Filter
Filter können Sie Kriterien zum Qualifizieren von Datensätzen bei einer Suche im Verzeichnis.  Durch Festlegen des Filters können Sie die Objekte Bereich synchronisieren möchten.  

![Filter](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure mehrstufige Authentifizierung hat die folgenden 3 Optionen.

- **Container-Filter** - Filterkriterien zum Container Datensätze beim Durchführen einer Verzeichnissuche kennzeichnen.  Für Active Directory und ADAM (| () objectClass=organizationalUnit)(objectClass=container)) wird im Allgemeinen verwendet.  Für anderen LDAP-Verzeichnissen sollten je nach das Verzeichnisschema Filterkriterien, die jede Art von Containerobjekt kennzeichnet verwendet werden.  <br>Hinweis: Wenn leer, ((objectClass=organizationalUnit)(objectClass=container)) wird standardmäßig verwendet.

- **Sicherheitsgruppenfilter** - Filterkriterien zu Sicherheit Gruppieren von Datensätzen bei einer Suche im Verzeichnis verwendet.  Für Active Directory und ADAM (&(objectCategory=group) (GroupType:1.2.840.113556.1.4.804:=-2147483648)) wird im Allgemeinen verwendet.  Für anderen LDAP-Verzeichnissen sollten je nach das Verzeichnisschema Filterkriterien, die jede Art von Sicherheitsgruppenobjekt kennzeichnet verwendet werden.  <br>Hinweis: Wenn leer (&(objectCategory=group) (GroupType:1.2.840.113556.1.4.804:=-2147483648)) wird standardmäßig verwendet.

- **Benutzerfilter** - Filterkriterien verwendet, beim Durchführen einer Verzeichnissuche Benutzerdatensätze zu qualifizieren.  Für Active Directory und ADAM (& (objectClass=user)(objectCategory=person)) wird im Allgemeinen verwendet.  Für anderen LDAP-Verzeichnissen (ObjectClass = InetOrgPerson) oder ähnliches je nach das Verzeichnisschema verwendet werden soll. <br>Hinweis: Leer (& (objectCategory=person)(objectClass=user)) wird standardmäßig verwendet.

## <a name="attributes"></a>Attribute
Attribute können für ein bestimmtes Verzeichnis gegebenenfalls angepasst werden.  Dadurch können Sie benutzerdefinierte Attribute hinzufügen, und optimieren die Synchronisierung Sie müssen Attribute.  Der Wert für jedes Attributfeld sollte den Namen des Attributs im Verzeichnisschema definiert.  Verwenden Sie in der folgenden Tabelle Weitere Informationen.

![Attribute](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Funktion | Beschreibung |
| ------- | ----------- |
| Eindeutiger Bezeichner | Geben Sie Attribut das Attribut, das als eindeutiger Bezeichner Container Sicherheitsgruppe und Benutzerdatensätze dient.  In Active Directory ist dies normalerweise ObjectGUID.  In anderen LDAP-Implementierung kann es EntryUUID oder ähnliches sein.  Der Standardwert ist "objectGUID". |
| ... Schaltflächen (select-Attribut) | Jedes Attributfeld ist eine Schaltfläche "... neben das Attribut auswählen aus ermöglicht ein Attribut aus einer Liste ausgewählt werden". <br><br>Wählen Sie Attribute.<br><br>Hinweis: Attribute können manuell eingegeben und kein Attribut in der Attributliste übereinstimmen. |
| Eindeutiger Bezeichnertyp | Wählen Sie den Typ des Attributs Eindeutiger Bezeichner.  In Active Directory ist das ObjectGUID Attribut vom Typ GUID.  In anderen LDAP-Implementierung kann es ASCII-Byte-Array oder String sein.  Der Standardwert ist GUID. <br><br>Hinweis: Es ist wichtig, dies korrekt Synchronisationsobjekte wird durch ihre eindeutige Kennung und den eindeutigen Bezeichner verwendet, um das Objekt direkt in das Verzeichnis zu suchen.  Durch Festlegen dieser Zeichenfolge beim Verzeichnis den Wert speichert ein Byte-Array von ASCII-Zeichen Synchronisierung Funktion verhindern. |
| DN | Geben Sie Attribut das Attribut, das den distinguished Name für jeden Datensatz enthält.  In Active Directory ist dies normalerweise "distinguishedName".  In anderen LDAP-Implementierung kann es EntryDN oder ähnliches sein.  Der Standardwert ist "distinguishedName". <br><br>Hinweis: Ein Attribut mit nur den definierten Namen vorhanden ist, kann das Adspath Attribut verwendet werden.  Die "LDAP: / /<server>/" Teil des Pfades automatisch lassen den distinguished Name des Objekts entfernt. |
| Containername | Geben Sie Attribut das Attribut mit dem Namen in einem Container.  Der Wert dieses Attributs werden in die Containerhierarchie beim Importieren aus Active Directory oder Synchronisierung Elemente hinzufügen.  Der Standardwert ist Name. <br><br>Hinweis: Wenn verschiedene Container verschiedene Attribute für ihre Namen verwenden, können mehrere Container Namenattribute in angegeben werden durch Semikolons getrennt.  Das erste Container Namensattribut für ein Containerobjekt verwendet den Namen angezeigt. |
| Name der Sicherheitsgruppe | Geben Sie Attribut das Attribut mit dem Namen im Datensatz für eine Sicherheit.  Der Wert dieses Attributs wird beim Importieren von Active Directory in der Liste Gruppe angezeigt oder Synchronisierung Elemente hinzufügen.  Der Standardwert ist Name. |
| Benutzer | Die folgenden Attribute werden zum Suchen, anzeigen, importieren und Synchronisieren von Benutzerinformationen aus dem Verzeichnis. |
| Benutzername | Geben Sie Attribut das Attribut, das den Benutzernamen in einem Benutzerdatensatz enthält.  Der Wert dieses Attributs wird als mehrstufige Authentifizierung Server-Benutzername verwendet.  Ein zweites Attribut kann als Sicherung für die erste angegeben werden.  Das zweite Attribut wird nur verwendet werden, wenn das erste Attribut einen Wert für den Benutzer nicht enthalten.  Die Standardeinstellungen sind UserPrincipalName und sAMAccountName. |
| Vorname | Geben Sie Attribut das Attribut, das den ersten Eintrag enthält.  Der Standardwert ist "givenName". |
| Nachname | Geben Sie Attribut das Attribut, das den Nachnamen in einem Benutzerdatensatz enthält.  Der Standardwert ist sn. |
| E-Mail-Adresse | Geben Sie Attribut das Attribut, das die e-Mail-Adresse in einem Benutzerdatensatz enthält.  E-Mail-Adresse wird zu senden Willkommen e-Mails für den Benutzer verwendet.  Der Standardwert ist Mail. |
| Benutzergruppe | Geben Sie Attribut das Attribut, das der Benutzer in einem Benutzerdatensatz enthält.  Benutzergruppe kann Benutzer im Agent und Berichte im Verwaltungsportal für mehrstufige Authentifizierung Server Filtern verwendet werden. |
| Beschreibung | Geben Sie Attribut das Attribut, das die Beschreibung in einem Benutzerdatensatz enthält.  Beschreibung wird nur für die Suche verwendet.  Der Standardwert ist Beschreibung. |
| Aufruf von Sprache | Geben Sie Attribut das Attribut, das den Kurznamen der Sprache für Anrufe des Benutzers enthält. |
| SMS-Sprache | Geben Sie Attribut das Attribut, das den Kurznamen der Sprache für SMS-Nachrichten für den Benutzer enthält. |
| Telefon app Sprache | Geben Sie das Attributname des Attributs mit dem kurzen Namen der Sprache für Telefon app Textnachrichten für den Benutzer. |
| EID token Sprache | Geben Sie das Attributname des Attributs mit dem kurzen Namen der Sprache für EID Tokentext Nachrichten für den Benutzer. |
| Handys | Die folgenden Attribute zum Importieren oder Benutzertelefonnummern synchronisieren.  Wenn ein Attributname für Telefonnummer angegeben ist, ein wird nicht beim Importieren aus Active Directory oder Synchronisierung Elemente hinzufügen. |
| Unternehmen | Geben Sie Attribut das Attribut, das die geschäftliche Telefonnummer in einem Benutzerdatensatz enthält.  Der Standardwert ist TelephoneNumber. |
| Startseite | Geben Sie Attribut das Attribut, das die Telefonnummer in einem Benutzerdatensatz enthält.  Der Standardwert ist HomePhone. |
| Pager | Geben Sie Attribut das Attribut, das die Pagernummer in einem Benutzerdatensatz enthält.  Der Standardwert ist Pager. |
| Mobile | Geben Sie Attribut das Attribut, das die Mobiltelefonnummer in einem Benutzerdatensatz enthält.  Der Standardwert ist Mobil. |
| Fax | Geben Sie Attribut das Attribut, das die Faxnummer in einem Benutzerdatensatz enthält.  Der Standardwert ist FacsimileTelephoneNumber. |
| IP-Telefon | Geben Sie Attribut das Attribut, das die IP-Rufnummer in einem Benutzerdatensatz enthält.  Der Standardwert ist "ipPhone". |
| Benutzerdefinierte | Geben Sie Attribut das Attribut, das eine benutzerdefinierte Telefonnummer enthält |
|  | einen Benutzerdatensatz.  Der Standardwert ist leer. |
| Erweiterung | Geben Sie Attribut das Attribut, das die Durchwahl in einem Benutzerdatensatz enthält.  Der Wert im Feld Erweiterung wird als Erweiterung nur die primäre Rufnummer verwendet.  Der Standardwert ist leer. <br><br>Hinweis: Wenn das Extension-Attribut nicht angegeben ist, Extensions Telefon Attribut enthalten kann.  Die Erweiterung sollte so analysiert werden mit einem "X" beginnen.  Beispielsweise führt 555-123-4567 x890 555-123-4567 wie die Telefonnummer an 890 als Erweiterung. |
| Standardeinstellungen wiederherstellen | Klicken Sie auf die Schaltfläche Wiederherstellen, um alle Eigenschaften auf ihren Standardwert zurück.  Die Standardeinstellungen sollten ordnungsgemäß mit dem normalen Active Directory bzw. ADAM Schema. |

Um Attribute zu bearbeiten, klicken Sie auf der Registerkarte Attribute die Bearbeitungsschaltfläche.  Dies öffnet ein Fenster, das Sie die Attribute bearbeiten können.

![Attribute bearbeiten](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Synchronisierung
Synchronisierung wird die kombinierte Azure-Datenbank mit Benutzern in Active Directory oder einem anderen Lightweight Directory Access Protocol (LDAP) Lightweight Directory Access Protocol-Verzeichnis synchronisiert.  Der Prozess ähnelt dem Benutzer manuell aus Active Directory importieren jedoch für Active Directory-Benutzer und sicherheitsänderungen regelmäßig abfragt.  Ferner zum Deaktivieren oder Entfernen von Benutzern von Container oder Sicherheit entfernt und Entfernen von Benutzern aus Active Directory gelöscht.

Mehrstufige Authentifizierung ADSync Service ist ein Windowsdienst, der den periodischen Abruf von Active Directory ausführt.  Dies ist nicht zu verwechseln mit Azure AD synchronisieren oder Azure AD verbinden.  Mehrstufige Authentifizierung ADSync ist zwar eine ähnliche Codebasis Azure mehrstufige Authentifizierungsserver.  Es wird in einem angehaltenen Zustand installiert und gestartet, indem die mehrstufige Authentifizierung Serverdienst konfiguriert ausgeführt.  Haben Sie eine mehrstufige Authentifizierung Server-Konfiguration mit mehreren Servern kann mehrstufige Authentifizierung ADSync nur auf einem Server ausgeführt werden.

Mehrstufige Authentifizierung ADSync Dienst verwendet die DirSync LDAP-Server-Erweiterung von Microsoft bereitgestellten effizient Änderungen abgefragt.  Diese Aufrufer Dirsync-Steuerelement muss "Verzeichnis Änderungen" rechts und DS-Get Replikationsereignisse erweiterte Steuerelement rechts.  Standardmäßig werden die Konten Administrator und LocalSystem auf Domänencontrollern diese Rechte zugewiesen.  Mehrstufige Authentifizierung AdSync Service wird standardmäßig als lokales System ausführen.  Daher ist es am einfachsten, den Dienst auf einem Domänencontroller ausführen.  Eine vollständige Synchronisierung so konfigurieren kann der Dienst als Konto mit weniger Berechtigungen ausgeführt.  Weniger effizient ist dies erfordert weniger Rechte.

Wenn konfiguriert LDAP und das LDAP-Verzeichnis unterstützt das Dirsync-Steuerelement, Abfragen für Benutzer und sicherheitsänderungen funktionieren ebenso wie mit Active Directory.  Wenn das LDAP-Verzeichnis das Dirsync-Steuerelement nicht unterstützt, wird eine vollständige Synchronisierung jeden Zyklus ausgeführt.

![Synchronisierung](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Verwenden Sie in der folgenden Tabelle Weitere Informationen über jede einzelne Einstellung auf der Registerkarte Synchronisierung.

| Funktion | Beschreibung |
| ------- | ----------- |
| Aktivieren der Synchronisierung mit Active Directory | Wenn aktiviert, wird der Serverdienst mehrstufige Authentifizierung ändert Active Directory regelmäßig abgefragt gestartet. <br><br>Hinweis: mindestens ein Element der Synchronisierung hinzugefügt werden und synchronisieren jetzt erfolgen muss vor der mehrstufige Authentifizierung Server Dienst startet Verarbeitung ändert. |
| Synchronisieren alle | Geben Sie mehrstufige Authentifizierung Serverdienst wartet zwischen abrufen und Verarbeiten von Änderungen Intervall an. <br><br> Hinweis: Das angegebene Intervall ist die Zeit zwischen jedem Zyklus.  Die Zeit für die Verarbeitung ändert das Intervall überschreitet, wird der Dienst erneut abrufen. |
| Benutzer in Active Directory nicht entfernen | Wenn aktiviert, mehrstufige Authentifizierung Serverdienst Active Directory Gelöschte Benutzer Tombstones verarbeitet und verwandte mehrstufige Authentifizierung Server entfernen. |
| Führen Sie immer eine vollständige Synchronisierung | Wenn aktiviert, wird der kombinierten Authentifizierung Serverdienst immer eine vollständige Synchronisierung durchführen.  Wenn deaktiviert, führt mehrstufige Authentifizierung Serverdienst eine inkrementelle Synchronisierung durch Abfragen nur Benutzer, die geändert wurden.  Der Standardwert ist deaktiviert. <br><br> Hinweis: Bei, eine inkrementelle Synchronisierung erfolgt nur Verzeichnis unterstützt das Dirsync-Steuerelement und das Konto, an das Verzeichnis binden die entsprechenden Berechtigungen inkrementelle DirSync-Abfragen ausführen.  Wenn das Konto nicht berechtigt ist, mehrere Domänen an der Synchronisierung beteiligt durchführen eine vollständigen Synchronisierung wird empfohlen. |
| Erfordert Genehmigung mehr als X Benutzer Administrator deaktiviert oder entfernt werden | Synchronisierung Elemente können deaktivieren oder entfernen Sie Benutzer nicht mehr Mitglied des Artikels Container oder Sicherheitsgruppe konfiguriert werden.  Sicherheit kann Administrator genehmigt erforderlich sein, wenn die Anzahl der Benutzer zu deaktivieren oder entfernen Sie einen Schwellenwert überschreitet.  Wenn aktiviert, werden Genehmigung für den angegebenen Schwellenwert.  Der Standardwert ist 5, und der Bereich ist 1 bis 999. <br><br> Genehmigung wird vom ersten senden eine e-Mail-Benachrichtigung an Administratoren erleichtert. Die Benachrichtigung gibt Anleitung zur Überprüfung und Genehmigung deaktivieren und Entfernen von Benutzern.  Wenn die kombinierte Authentifizierung Server-Benutzeroberfläche gestartet wird, fordert er zur Genehmigung. |

Schaltfläche " **Jetzt synchronisieren** " können Sie eine vollständige Synchronisierung für die Synchronisation angegebenen Elemente ausführen.  Eine vollständige Synchronisierung ist erforderlich, wenn die Synchronisierung Elemente hinzugefügt, geändert, entfernt oder neu angeordnet werden.  Er ist auch erforderlich, bevor mehrstufige Authentifizierung AdSync Service werden da Ausgangspunkt wird von dem des Dienstes ändert abrufen.  Wenn Synchronisationsobjekte Änderungen vorgenommen wurden und keine vollständige Synchronisierung ausgeführt wurde, werden Sie jetzt synchronisieren beim Navigieren zu einem anderen Abschnitt oder die Benutzeroberfläche schließen aufgefordert.

Die Schaltfläche **Entfernen** kann der Administrator eine oder mehrere Synchronisationsobjekte aus Artikel mehrstufige Authentifizierung Server löschen.

>[AZURE.WARNING]Entfernt einen Artikeldatensatz Synchronisierung kann nicht wiederhergestellt werden. Sie müssen den Artikeldatensatz Synchronisation erneut versehentlich gelöscht.

Die Synchronisierung Vorlage(n) Synchronisation wurden mehrstufige Authentifizierung Server entfernt.  Mehrstufige Authentifizierung Serverdienst verarbeitet keine Synchronisierung Elemente.

Die Schaltflächen nach oben und nach unten können der Administrator die Reihenfolge der Synchronisierung Elemente ändern.  Die Reihenfolge ist wichtig, da derselbe Benutzer mehrere Synchronisierung Elemente (Container und eine Sicherheitsgruppe) angehören kann.  Einstellungen für den Benutzer während der Synchronisation kommen aus dem ersten Synchronisierung Element in der Liste, die der Benutzer zugeordnet ist.  Daher sollten die Synchronisationsobjekte Priorität zugeordnet werden.

>[AZURE.TIP]Eine vollständige Synchronisierung durchgeführt nach Synchronisierung Elemente entfernen.  Eine vollständige Synchronisierung durchgeführt Synchronisationsobjekte Bestellung.  Klicken Sie auf die Schaltfläche synchronisieren, um eine vollständige Synchronisierung durchführen.

## <a name="multi-factor-auth-servers"></a>Mehrstufige Authentifizierung Server
Backup RADIUS-Proxy LDAP-Proxy oder für IIS-Authentifizierung kann zusätzliche mehrstufige Authentifizierung Server eingerichtet werden. Die Synchronisierungskonfiguration werden alle Agents verteilt werden. Aber möglicherweise nur eines dieser Agents der kombinierten Authentifizierung Server ausgeführt. Auf dieser Registerkarte können Sie mehrstufige Authentifizierung Server auswählen, die für die Synchronisierung aktiviert werden soll.

![Multi-Factor-Authentifizierung Server](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
