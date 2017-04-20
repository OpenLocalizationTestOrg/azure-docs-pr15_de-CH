<properties
   pageTitle="Generische LDAP-Anschluss | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie Microsoft generische LDAP-Connector konfigurieren."
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

# <a name="generic-ldap-connector-technical-reference"></a>Allgemeine technische LDAP-Anschluss
Dieser Artikel beschreibt die allgemeine LDAP-Anschluss. Der Artikel gilt für die folgenden Produkte:

- Microsoft Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Hotfix 4.1.3671.0 oder später [KB3092178](https://support.microsoft.com/kb/3092178)verwenden.

MIM2016 und FIM2010R2 ist der Connector als Download im [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495)verfügbar.

In Bezug auf die IETF-RFCs verwendet dieses Dokument das Format (RFC [RFC-Nummer] / [Kapitel im RFC-Dokument]), z. B. (RFC 4512/4.3).
Weitere Informationen finden unter http://tools.ietf.org/html/rfc4500 (Sie müssen die richtige RFC-Nummer 4500 ersetzen) haben.

## <a name="overview-of-the-generic-ldap-connector"></a>Übersicht über generische LDAP-Anschluss
Generische LDAP-Connector können Sie den Synchronisierungsdienst mit einem LDAP-v3-Server integrieren.

Bestimmte Vorgänge und Schemaelemente wie die deltaimport ausführen werden nicht in die IETF-RFCs angegeben. Diese Vorgänge werden nur LDAP-Verzeichnissen explizit angegeben.

Aus einer grundlegenden Perspektive die folgenden Funktionen der aktuellen Version der Connector unterstützt:

Funktion | Unterstützung
--- | --- |
Verbundene Datenquelle | Der Connector wird mit allen LDAP v3-Servern (RFC 4510 kompatibel) unterstützt. Es wurde mit der folgenden getestet: <li>Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Microsoft Active Directory globalen Katalog AD</li><li>389 Verzeichnisserver</li><li>Apache-Verzeichnisserver</li><li>IBM Tivoli DS</li><li>Isode Verzeichnis</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>DJ öffnen</li><li>DS öffnen</li><li>Offenes LDAP (openldap.org)</li><li>Oracle (früher Sun) Directory Server Enterprise Edition</li><li>RadiantOne virtuelles Verzeichnisserver (VDS)</li><li>Sun One Directory Server</li>**Wichtige Verzeichnisse nicht unterstützt:** <li>Microsoft Active Directory-Domänendienste (AD DS) [integrierte Active Directory Connector verwenden]</li><li>Oracle Internet Directory (OID)</li>
Szenarien   | <li>Objekt Lifecycle Management</li><li>Verwaltung</li><li>Kennwort-Management</li>
Vorgänge |Die folgenden Vorgänge werden auf alle LDAP-Verzeichnissen unterstützt: <li>Vollständigen Import</li><li>Exportieren</li>Angegebenen Verzeichnisse werden nur die folgenden Operationen unterstützt:<li>Deltaimport</li><li>Kennwort, Kennwort ändern</li>
Schema | <li>Schema wird aus dem LDAP-Schema (RFC3673 und RFC4512-4.2) erkannt.</li><li>Unterstützt Strukturklassen, Aux Klassen und ExtensibleObject-Objektklasse (RFC4512/4.3)</li>

### <a name="delta-import-and-password-management-support"></a>Delta Import und Kennwort Management-Unterstützung
Verzeichnisse für deltaimport und passwortmanagement unterstützt:

- Microsoft Active Directory Lightweight Directory Services (AD LDS)
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt das Kennwort
- Microsoft Active Directory globalen Katalog AD
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt das Kennwort
- 389 Verzeichnisserver
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt Set-Kennwort und Kennwort ändern
- Apache-Verzeichnisserver
    - Deltaimport unterstützt keine da dieses Verzeichnis nicht persistent Änderungsprotokoll
    - Unterstützt das Kennwort
- IBM Tivoli DS
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt Set-Kennwort und Kennwort ändern
- Isode Verzeichnis
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt Set-Kennwort und Kennwort ändern
- Novell eDirectory und NetIQ eDirectory
    - Hinzufügen, aktualisieren und Umbenennen unterstützt für deltaimport
    - Unterstützt keine Löschvorgänge für deltaimport
    - Unterstützt Set-Kennwort und Kennwort ändern
- DJ öffnen
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt Set-Kennwort und Kennwort ändern
- DS öffnen
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt Set-Kennwort und Kennwort ändern
- Offenes LDAP (openldap.org)
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt das Kennwort
    - Kennwort wird nicht unterstützt.
- Oracle (früher Sun) Directory Server Enterprise Edition
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt Set-Kennwort und Kennwort ändern
- RadiantOne virtuelles Verzeichnisserver (VDS)
    - Verwenden Sie müssen Version 7.1.1 oder höher
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt Set-Kennwort und Kennwort ändern
-  Sun One Directory Server
    - Unterstützt alle Vorgänge für deltaimport
    - Unterstützt Set-Kennwort und Kennwort ändern

### <a name="prerequisites"></a>Erforderliche Komponenten
Bevor Sie den Connector verwenden, sorgen Sie Folgendes auf dem Synchronisierungsserver:

- Microsoft .NET 4.5.2 Framework oder höher

### <a name="detecting-the-ldap-server"></a>Den LDAP-Server erkennen
Der Connector verwendet verschiedene Techniken zum Erkennen und Identifizieren des LDAP-Servers. Der Connector verwendet das Stamm-DSE Kreditor Name/Version und überprüft das Schema eindeutige Objekte und Attribute bekannt, dass bestimmte LDAP-Server. Diese Daten, wenn gefunden, zur Vorabfestlegung Konfigurationsoptionen im Anschluss.

### <a name="connected-data-source-permissions"></a>Verbundene Datenquelle Berechtigungen
Importieren und Exportieren Vorgänge für die Objekte im verbundenen Verzeichnis müssen das Connector-Konto über ausreichende Berechtigungen. Der Connector benötigt Schreibzugriff zu exportieren und zu importieren lesen. Konfiguration erfolgt innerhalb der Erfahrungen das Verzeichnis selbst.

### <a name="ports-and-protocols"></a>Ports und Protokolle
Der Connector verwendet die Portnummer in der Konfiguration für LDAP 389 und 636 für LDAPS werden angegeben.

Damit LDAPS erforderlich SSL 3.0 oder TLS. SSL 2.0 nicht unterstützt und kann nicht aktiviert werden.

### <a name="required-controls-and-features"></a>Erforderliche Steuerelemente und features
LDAP-Steuerelemente/Features muss auf dem LDAP-Server für die Verbindung ordnungsgemäß funktioniert:  
`1.3.6.1.4.1.4203.1.5.3`True/False-Filter

True/False-Filter wurde häufig gemeldet von LDAP-Verzeichnissen unterstützt und kann auf der **Seite Global** unter **Obligatorisch Features nicht gefunden**angezeigt. Hiermit wird in LDAP-Abfragen beispielsweise beim Importieren von mehreren Objekttypen **oder** Filter erstellen. Importieren von mehr als einem Objekttyp unterstützt LDAP-Server diese Funktion.

Verwenden Sie ein Verzeichnis ist eine eindeutige Kennung der Anker muss folgenden auch zur Verfügung (siehe Abschnitt [Konfigurieren Anker](#configure-anchors) in diesem Artikel Weitere Informationen):  
`1.3.6.1.4.1.4203.1.5.1`Alle Betriebsattribute

Wenn das Verzeichnis mehr Objekte enthält als was Aufrufen von Verzeichnis anpassen können, empfiehlt es Paging verwenden. Für die Auslagerung zu arbeiten, benötigen Sie eine der folgenden Optionen:

**Option 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Option 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Wenn beide Optionen in der Konfiguration des Connectors aktiviert sind, wird PagedResultsControl verwendet.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl mit USNChanged Delta Import-Methode dient nur gelöschte Objekte angezeigt werden.

Der Connector versucht, erkennen die Optionen, die auf dem Server vorhanden. Wenn die Optionen nicht erkannt werden können, ist eine Warnung in den Connectoreigenschaften auf der globalen Seite vorhanden. Nicht alle LDAP-Servern vorhanden alle Steuerelemente/Features unterstützen und auch wenn diese Warnung vorhanden ist, funktioniert des Connectors ohne Probleme.

### <a name="delta-import"></a>Deltaimport
Deltaimport ist nur verfügbar, wenn ein Support-Verzeichnis erkannt wurde. Derzeit werden folgenden Methoden verwendet:

- LDAP-Accesslog. Siehe [http://www.openldap.org/doc/admin24/overlays.html#Access protokollieren](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP-Changelog. Siehe [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- Zeitstempel. Der Connector verwendet für Novell-NetIQ eDirectory letzten zu erstellen und Aktualisieren von Objekten. Novell-NetIQ eDirectory bietet keine bedeutet eine Entsprechung Gelöschte Objekte abgerufen. Diese Option kann auch verwendet werden, wenn keine Delta Importmethode auf dem LDAP-Server aktiv ist. Diese Option kann nicht auf Objekte importieren gelöscht.
- USNChanged. Siehe: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Nicht unterstützt
Die folgenden LDAP-Funktionen werden nicht unterstützt:

- LDAP-verweisen zwischen Servern (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Erstellen eines neuen Connectors
Erstellen Sie einen generischen LDAP-Connector versehen Sie **Synchronisierungsdienst** , **Management Agent** und **Erstellen**. Wählen Sie den Connector **generische LDAP (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Konnektivität
Auf der Seite Verbindung müssen Sie Host, Anschluss und Bindung Informationen angeben. Je nach der Bindung ausgewählt, zusätzliche ist kann Informationen in den folgenden Abschnitten bereitgestellt.

![Konnektivität](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- Die Einstellung wird nur bei der ersten Verbindung mit dem Server verwendet, bei dem Schema.
- Bindung ist Anonymous dann weder Benutzername / Kennwort oder Zertifikat verwendet.
- Geben Sie für andere Bindungen Informationen entweder in Benutzername / Kennwort oder ein Zertifikat auswählen.
- Wenn Sie Kerberos zur Authentifizierung verwenden, bieten Sie auch Bereichs bzw. der Domäne des Benutzers.

Feld **Attribut Aliase** dient für Attribute im Schema mit RFC4522 Syntax definiert. Diese Attribute nicht während des Schema-Erkennung erkannt, und der Connector benötigt Hilfe diese Attribute. Beispielsweise sind folgende im Attribut Aliase identifizieren das Attribut UserCertificate als binäre Attribut eingegeben werden:

`userCertificate;binary`

So sieht beispielsweise wie diese Konfiguration aussehen könnte:

![Konnektivität](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Aktivieren Sie das Kontrollkästchen **operative Attribute im Schema enthalten** , auch Attribute vom Server erstellt. Diese enthalten Attribute wie das Objekt wurde als letzte Aktualisierung.

Wählen Sie **erweiterbare Include-Attribute im Schema** erweiterbare Objekte (RFC4512/4.3) und das Aktivieren dieser Option kann jedes Attribut auf alle Objekte aus Schema macht diese Option sehr groß, so dass nur das verbundene Verzeichnis diese Funktion empfohlen wird, die Option deaktiviert.

### <a name="global-parameters"></a>Globale Parameter
Auf der Seite Globalparameter konfigurieren Sie Delta Änderungsprotokoll und Zusatzfunktionen LDAP-DN. Die Seite wird automatisch Informationen von den LDAP-Server.

![Konnektivität](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

Im oberen Bereich werden Informationen vom Server, wie den Namen des Servers. Der Connector überprüft auch die obligatorische Steuerelemente im Stamm-DSE vorhanden. Wenn diese Steuerelemente nicht aufgeführt sind, wird eine Warnung angezeigt. Einige LDAP-Verzeichnissen nicht alle Funktionen im Stamm-DSE auflisten und es ist möglich, dass der Connector problemlos funktioniert, wenn eine Warnung vorhanden ist.

Die Kontrollkästchen **unterstützten Steuerelemente** Verhalten für bestimmte Vorgänge:

- Mit Struktur ausgewählt, eine Hierarchie gelöscht mit einem LDAP-Aufruf. Mit Struktur deaktiviert wird der Connector eine Löschung bei Bedarf.
- Mit ausgelagerter Ergebnisse ausgewählt wird der Connector ausgelagerten importieren mit der angegebenen Schritte ausführen.
- VLVControl und SortControl ist eine Alternative zur PagedResultsControl zum Lesen von Daten aus dem LDAP-Verzeichnis.
- Alle drei Optionen (PagedResultsControl, VLVControl und SortControl) deaktiviert sind importiert der Connector alle Objekte auf einmal Fehlschlagen eines umfangreichen Verzeichnisses ist.
- ShowDeletedControl wird nur verwendet, wenn das Delta Import USNChanged ist.

Das Änderungsprotokoll DN ist beispielsweise von Änderungsprotokoll Delta verwendet Namenskontext **Cn = Changelog**. Dieser Wert muss angegeben werden, Delta importieren können.

Folgendes ist eine Liste von DNS-Standardprotokoll ändern:

Verzeichnis | Delta-Protokoll
--- | ---
Microsoft AD LDS und AD-GC | Automatisch erkannt. USNChanged.
Apache-Verzeichnisserver | Nicht verfügbar.
Verzeichnis 389 | Änderungsprotokoll. Standardwert verwenden: **Cn = Changelog**
IBM Tivoli DS | Änderungsprotokoll. Standardwert verwenden: **Cn = Changelog**
Isode Verzeichnis | Änderungsprotokoll. Standardwert verwenden: **Cn = Changelog**
Novell-NetIQ eDirectory | Nicht verfügbar. Zeitstempel. Der Connector verwendet aktualisiert Datum und Uhrzeit abrufen hinzugefügt und Datensätze aktualisiert.
Öffnen von DJ-DS | Änderungsprotokoll.  Standardwert verwenden: **Cn = Changelog**
Offenes LDAP | Zugriffsprotokoll. Standardwert verwenden: **Cn = Accesslog**
Oracle DSEE | Änderungsprotokoll. Standardwert verwenden: **Cn = Changelog**
RadiantOne VDS | Das virtuelle Verzeichnis. Hängt das Verzeichnis VDS verbunden.
Sun One Directory Server | Änderungsprotokoll. Standardwert verwenden: **Cn = Changelog**

Kennwortattribut ist der Name des Attributs, der Connector verwenden soll, im Kennwort ändern das Kennwort festgelegt, und Kennwort Mengenoperationen.
Dieser Wert ist standardmäßig eine **UserPassword** aber kann geändert werden, wenn für ein bestimmtes LDAP-System benötigt.

In der Liste zusätzliche Partitionen kann Hinzufügen zusätzlichen Namespaces nicht automatisch erkannt. Diese Einstellung kann z. B. verwendet werden, wenn mehrere Server logischen Cluster bilden, die alle gleichzeitig importiert werden sollen. Wie Active Directory mehrere Domänen in einer Gesamtstruktur können, aber alle Domänen ein Schema gemeinsam, simuliert die gleiche zusätzliche Namespaces in dieses Feld eingeben. Jeder Namespace kann von verschiedenen Servern und auf der Seite Partitionen konfigurieren und Hierarchien weiter konfiguriert. STRG + EINGABETASTE eine neue Zeile zu verwenden.

### <a name="configure-provisioning-hierarchy"></a>Konfigurieren der Bereitstellung Hierarchie
Diese Seite wird zum Objekttyp, der bereitgestellt werden soll, z. B. OrganizationalUnit DN-Komponente, z. B. Organisationseinheit zuordnen.

![Bereitstellung der Hierarchie](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Bereitstellung Hierarchie konfigurieren, können Sie den Connector eine Struktur bei Bedarf automatisch erstellen konfigurieren. Beispielsweise wird ein Namespace dc = Contoso, dc = com und neues Objekt Cn Joe, Ou = = Seattle, c = US, dc = Contoso, dc = com bereitgestellt, und der Connector kann ein Objekt Typ für USA und OrganizationalUnit für Seattle erstellen, wenn diese nicht bereits im Verzeichnis vorhanden.

### <a name="configure-partitions-and-hierarchies"></a>Konfigurieren von Partitionen und Hierarchien
Wählen Sie auf der Seite Partitionen und Hierarchien alle Namespaces mit Objekten zu importieren und zu exportieren.

![Partitionen](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Für jeden Namespace kann auch Verbindung konfigurieren, die auf dem Bildschirm Verbindung angegebenen Werte überschreiben würde. Wenn diese Werte auf ihren Standardwert leer sind, werden die Informationen vom Bildschirm Konnektivität verwendet.

Es kann auch der Container und Organisationseinheiten des Connectors sollten importieren aus und exportieren zu wählen.

### <a name="configure-anchors"></a>Anker konfigurieren
Diese Seite muss immer einen vorkonfigurierten Wert und kann nicht geändert werden. Wenn der Hersteller identifiziert, kann Anker mit unveränderlich Attribut beispielsweise die GUID für ein Objekt gefüllt. Nicht erkannt oder bekanntermaßen nicht unveränderlichen Attribut, verwendet der Connector als Anker dn (distinguished Name).

![Anker](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

Folgendes ist eine Liste von LDAP-Servern und den Anker verwendet wird:

Verzeichnis | Ankerattribut
--- | ---
Microsoft AD LDS und AD-GC | objectGUID
389 Verzeichnisserver | DN
Apache-Verzeichnis | DN
IBM Tivoli DS | DN
Isode Verzeichnis | DN
Novell-NetIQ eDirectory | GUID
Öffnen von DJ-DS | DN
Offenes LDAP | DN
Oracle ODSEE | DN
RadiantOne VDS | DN
Sun One Directory Server | DN

## <a name="other-notes"></a>Weitere Hinweise
Dieser Abschnitt enthält Informationen, die spezifisch für diesen Connector oder aus anderen Gründen wichtig, Aspekte.

### <a name="delta-import"></a>Deltaimport
Delta Wasserzeichen in Open LDAP ist UTC-Datum/Zeit. Aus diesem Grund müssen die Systemuhren FIM-Synchronisierungsdienst und offenes LDAP synchronisiert werden. Andernfalls möglicherweise einige Einträge im Änderungsprotokoll Delta ausgelassen.

Für Novell eDirectory erkennt deltaimport keine Objekt gelöscht. Aus diesem Grund muss einen vollständigen Import regelmäßig, um alle gelöschten Objekte ausführen.

Für Verzeichnisse mit einem Zeitpunkt basiert Delta-Änderungsprotokoll wird empfohlen einen vollständigen Import regelmäßige Zeiten ausgeführt. Dieser Prozess ermöglicht Synchronisierungsmodul gefunden und Unterschiede zwischen den LDAP-Server und derzeit im Connectorspace.

## <a name="troubleshooting"></a>Problembehandlung

-   Informationen zum Aktivieren der Protokollierung den Connector beheben finden Sie unter [How to Enable Tracing ETW-Anschlüsse](http://go.microsoft.com/fwlink/?LinkId=335731).
