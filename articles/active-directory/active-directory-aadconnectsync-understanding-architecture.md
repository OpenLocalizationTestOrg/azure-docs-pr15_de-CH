<properties
   pageTitle="Azure AD Connect-Synchronisierung: die Architektur kennen | Microsoft Azure"
   description="Dieses Thema beschreibt die Architektur von Azure AD-Verbindung synchronisieren und verwendeten Begriffe erläutert."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/31/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-understanding-the-architecture"></a>Azure AD Connect-Synchronisierung: die Architektur kennen
Dieses Thema behandelt die grundlegende Architektur für Azure AD-Verbindung synchronisieren. In vielerlei Hinsicht ähnelt es Vorgänger MIIS 2003 ILM 2007 und FIM 2010. Azure AD Connect-Synchronisierung wird die Entwicklung dieser Technologie. Wenn Sie mit diesen früheren Verfahren vertraut sind, werden der Inhalt dieses Themas vertraut sowie. Wenn Sie die Synchronisation sind, ist dieses Thema für Sie. Es ist jedoch nicht erforderlich, die Einzelheiten dieses Themas erfolgreich in Anpassungen Azure AD verbinden synchronisieren (Synchronisierungsmodul in diesem Thema genannt).

## <a name="architecture"></a>Architektur
Das Synchronisierungsmodul erstellt eine integrierte Ansicht der Objekte, die in mehreren verbundenen Datenquellen gespeichert und verwaltet Identitätsinformationen in diesen Datenquellen. Diese integrierte Ansicht hängt von Identitätsinformationen verbundenen Datenquellen und Regeln, die bestimmen, wie diese Informationen abgerufen.

### <a name="connected-data-sources-and-connectors"></a>Verbundene Datenquellen und Anschlüsse
Das Synchronisierungsmodul verarbeitet Identitätsinformationen aus verschiedenen Daten-Repositories wie Active Directory oder SQL Server-Datenbank. Jedes Daten-Repository, das die Daten in einer Datenbank-ähnliches Format organisiert und standard Datenzugriff Methoden bereitstellt, wird Data Source Kandidaten für das Synchronisierungsmodul. Daten-Repositories, die vom Synchronisierungsmodul synchronisiert werden **Datenquellen** oder **verbundenen Verzeichnisse** (CD) bezeichnet.

Das Synchronisierungsmodul kapselt die Interaktion mit einer verbundenen Datenquelle innerhalb eines Moduls **Connector**genannt. Jeder verbundenen Datenquelle hat einen bestimmten Connector. Der Connector führt einen erforderlichen Vorgang in das Format die verbundenen Datenquelle versteht.

Connectors stellen API-Aufrufe Identität (Lesen und Schreiben) Informationsaustausch mit einer verbundenen Datenquelle. Es kann auch zum Hinzufügen eines benutzerdefinierten Verbinders mit erweiterbaren Konnektivität Framework. Die folgende Abbildung zeigt, wie eine Verbindung mit dem Synchronisierungsmodul verbundenen Datenquelle verbunden.

![Arch1](./media/active-directory-aadconnectsync-understanding-architecture/arch1.png)

Datenfluss in Richtung, aber es kann nicht fließen in beide Richtungen gleichzeitig. In anderen Worten ein Connector konfiguriert werden kann Daten aus der verbundenen Datenquelle Synchronisierungsmodul oder vom Synchronisierungsmodul an die verbundene Datenquelle übertragen, aber nur eine dieser Operationen kann jeweils für ein Objekt und Attribut. Die Richtung kann für verschiedene Objekte und andere Attribute unterscheiden.

Um einen Connector konfigurieren, geben Sie die Objekttypen, die Sie synchronisieren möchten. Angeben der Objekttypen definiert Objekte in die Synchronisierung einbezogen werden. Nächstes ist bekannt als eine Aufnahmeliste Attribut Attribute zum Synchronisieren auswählen. Diese Einstellung können jederzeit Änderungen Ihren Geschäftsregeln geändert werden. Bei Verwendung von Azure AD Connect-Installationsassistenten werden diese Einstellungen konfiguriert.

Zum Exportieren von Objekten mit einer verbundenen Datenquelle muss die Aufnahmeliste Attribut zumindest minimal Attribute zum Erstellen eines bestimmten Objekttyps in einer verbundenen Datenquelle enthalten. Beispielsweise muss das **sAMAccountName** -Attribut enthalten sein, in der Aufnahmeliste Attribut ein Benutzerobjekts in Active Directory exportiert, da alle Benutzerobjekte in Active Directory **sAMAccountName** -Attribut definiert sein müssen. Der Installations-Assistent wird erneut, diese Konfiguration.

Nutzt die verbundenen Datenquelle Strukturkomponenten Partitionen oder Container Objekte, können Sie die Bereiche in der verbundenen Datenquelle beschränken, die für eine bestimmte Lösung verwendet werden.

### <a name="internal-structure-of-the-sync-engine-namespace"></a>Interne Struktur Synchronisierung Engine namespace
Vollständige Synchronisierung Engine Namespace besteht aus zwei Namespaces, die Identitätsinformationen speichern. Zwei Namespaces sind:

- Connectorspace (CS)
- Metaverse (MV)

**Connectorspace** ist Stagingbereich, der Darstellung der ausgewiesenen Objekte aus einer verbundenen Datenquelle und in der Aufnahmeliste Attribut angegebenen Attribute enthält. Das Synchronisierungsmodul verwendet Connectorspace bestimmen, was in der verbundenen Datenquelle geändert hat und eingehende ändert. Das Synchronisierungsmodul verwendet auch Connectorspace zu ausgehenden Änderungen für den Export in der verbundenen Datenquelle. Das Synchronisierungsmodul verwaltet verschiedene Connectorspace als Stagingbereich für jeden Connector.

Das Synchronisierungsmodul mit Stagingbereich bleibt unabhängig von den verbundenen Datenquellen und Verfügbarkeit und Zugänglichkeit nicht betroffen. Daher können Sie mit den Daten im Stagingbereich Identitätsinformationen jederzeit bearbeiten. Das Synchronisierungsmodul kann nur die Änderungen in der verbundenen Datenquelle seit der letzten kommunikationssitzung beendet oder drücken die Änderungen Identitätsinformationen, die verbundene Datenquelle noch nicht erhalten hat, reduziert den Netzwerkverkehr zwischen dem Synchronisierungsmodul und der verbundenen Datenquelle anfordern.

Synchronisierungsmodul speichert außerdem Informationen über alle Objekte, die im Connectorspace Stufen. Beim Empfang neuer Daten ergibt Synchronisierungsmodul immer, ob die Daten bereits synchronisiert wurde.

**Metaverse** ist ein Speicherbereich, der aggregierten Identitätsinformationen aus mehreren miteinander verbundenen Datenquellen bietet eine globale integrierte Ansicht aller kombinierten Objekte enthält. Metaverse-Objekte werden auf die Identitätsinformationen erstellt, die aus den verbundenen Datenquellen und eine Reihe von Regeln, mit die Sie den Synchronisationsprozess anpassen können abgerufen werden.

Abbildung Connectornamespace Speicherplatz und Metaverse-Namespaces innerhalb des Moduls synchronisiert.

![Arch2](./media/active-directory-aadconnectsync-understanding-architecture/arch2.png)

## <a name="sync-engine-identity-objects"></a>Sync-Modul Identitätsobjekten
Die Objekte in dem Synchronisierungsmodul Repräsentation der beiden Objekte in der verbundenen Datenquelle oder die integrierte Ansicht, die Engine Synchronisieren dieser Objekte. Jedes Modul Synchronisierungsobjekt müssen einen global eindeutigen Bezeichner (GUID). GUIDs stellen Datenintegrität und express Beziehungen.

### <a name="connector-space-objects"></a>Connectorspace-Objekte
Wenn eine verbundene Datenquelle Synchronisierungsmodul kommuniziert Identitätsinformationen in der verbundenen Datenquelle liest und verwendet diese Informationen, um eine Darstellung des Identitätsobjekts im Connectorspace erstellen. Erstellen oder Objekte einzeln löschen. Allerdings können Sie alle Objekte in einem Connectorspace manuell löschen.

Alle Objekte im Connectorspace haben zwei Attribute:

- Ein global eindeutiger Bezeichner (GUID)
- Ein distinguished Name (DN)

Wenn die verbundenen Datenquelle das Objekt ein eindeutigen Attribut zuweist, können Objekte in den Connectorspace auch ein Ankerattributs. Das Ankerattribut identifiziert eindeutig ein Objekt in der verbundenen Datenquelle. Das Synchronisierungsmodul verwendet den Anker die entsprechende Darstellung dieses Objekts in der verbundenen Datenquelle gefunden. Synchronisierungsmodul wird vorausgesetzt, dass die Verankerung eines Objekts nicht über die Lebensdauer des Objekts ändert.

Viele Connectors verwenden einen bekannten eindeutigen Bezeichner zu einem Anker automatisch für jedes Objekt importiert werden. Active Directory Connector verwendet z. B. **ObjectGUID** -Attribut für einen Anker. Geben Sie bei verbundenen Datenquellen, die nicht definierten Bezeichner Anker Generation als Teil der Konfiguration des Connectors.

Fall basiert die Verankerung von einem oder mehr eindeutige Attribute eines Objekts geben weder sich ändert und so eindeutig kennzeichnet das Objekt im Connectorspace (z. B. eine Personalnummer oder eine Benutzer-ID).

Ein Connectorspace-Objekt kann eines der folgenden sein:

- Staging-Objekt
- Platzhalter

### <a name="staging-objects"></a>Staging-Objekte
Staging-Objekt stellt eine Instanz der angegebenen Objekttypen aus der verbundenen Datenquelle. Neben der GUID und der distinguished Name ist ein staging Objekt immer einen Wert, der den Objekttyp angibt.

Staging-Objekte immer importiert haben einen Wert für das Ankerattribut. Einen Wert für das Ankerattribut keinen Staging-Objekte, die vom Synchronisierungsmodul neu bereitgestellt worden und der verbundenen Datenquelle erstellt werden.

Staging-Objekte beinhalten außerdem aktuelle Werte Business Attribute und Informationen vom Synchronisierungsmodul musste, die Synchronisation durchzuführen. Informationen enthält Flags, die den Typ des Updates angeben, die für das staging Objekt bereitgestellt werden. Staging Objekt neue Identitätsinformationen aus der verbundenen Datenquelle erhalten hat, die noch nicht verarbeitet wurde, wird das Objekt als **ausstehende Import**gekennzeichnet. Verfügt ein staging-Objekt neue Identitätsinformationen, die noch nicht in der verbundenen Datenquelle exportiert, wird es als **ausstehende Export**gekennzeichnet.

Staging-Objekt kann als importieren oder ein Objekt exportieren. Das Synchronisierungsmodul erstellt Importobjekt mit Objektinformationen aus der verbundenen Datenquelle. Wenn Synchronisierungsmodul Informationen über das Vorhandensein eines neuen Objekts erhält, eines im Anschluss ausgewählten Objekttypen entspricht, erstellt er Importobjekt im Connectorspace als Darstellung des Objekts in der verbundenen Datenquelle.

Die folgende Abbildung zeigt ein Objekt importieren, das ein Objekt in der verbundenen Datenquelle darstellt.

![Arch3](./media/active-directory-aadconnectsync-understanding-architecture/arch3.png)

Das Synchronisierungsmodul erstellt ein exportobjekt mithilfe der Informationen im Metaverse. Exportieren von Objekten werden während der nächsten kommunikationssitzung in der verbundenen Datenquelle exportiert. Ausgehend von dem Synchronisierungsmodul existieren Exportieren von Objekten in der verbundenen Datenquelle noch nicht. Daher ist das Ankerattribut für ein Objekt exportieren nicht verfügbar. Nach Erhalt des Objekts vom Synchronisierungsmodul erstellt die verbundenen Datenquelle einen eindeutigen Wert für das Ankerattribut des Objekts.

Die folgende Abbildung zeigt, wie ein Objekt exportieren mit Identitätsinformationen im Metaverse erstellt wird.

![Arch4](./media/active-directory-aadconnectsync-understanding-architecture/arch4.png)

Das Synchronisierungsmodul wird bestätigt, dass den Export des Objekts das Objekt aus der verbundenen Datenquelle importieren. Exportieren von Objekten werden Objekte importieren, wenn Synchronisierungsmodul beim nächsten Import aus der verbundenen Datenquelle empfängt.

### <a name="placeholders"></a>Platzhalter
Das Synchronisierungsmodul verwendet einen flachen Namespace zum Speichern von Objekten. Einige verbundene Datenquellen wie Active Directory jedoch verwenden einen hierarchischen Namespace. Zum Transformieren von Daten aus einem hierarchischen Namespace in einen flachen Namespace verwendet Synchronisierungsmodul Platzhalter Hierarchie beibehalten.

Jeder Platzhalter stellt eine Komponente (z. B. einer Organisationseinheit) des hierarchischen Namens eines Objekts, das nicht in Synchronisierungsmodul importiert wurde jedoch mit den hierarchischen Namen erstellen. Diese Lücken durch Verweise auf Objekte, die keine Objekte im Connectorspace staging werden in der verbundenen Datenquelle erstellt.

Das Synchronisierungsmodul mithilfe Platzhalter referenzierten Objekte gespeichert, die noch nicht importiert. Z. B. Synchronisierung das Manager-Attribut für das *Abbie Stefan* und empfangenen Wert konfiguriert ist ein Objekt, das nicht noch wie importiert wurde *CN Lee Sperry, CN = = Benutzer, DC = Fabrikam, DC = com*, Informationen als Platzhalter im Connectorspace gespeichert. Das Managerobjekt später importiert, wird dem Platzhalterobjekt von staging Objekt überschrieben, das den Manager darstellt.

### <a name="metaverse-objects"></a>Metaverse-Objekte
Ein Metaverse-Objekt enthält die Aggregierte Ansicht, Synchronisierungsmodul staging Objekte im Connectorspace hat. Synchronisierungsmodul erstellt Metaverse-Objekten mithilfe der Objekte importieren. Mehrere Connectorspace-Objekte mit einem einzelnen Metaverse-Objekt verknüpft werden, aber ein Connectorspace-Objekt kann nicht mehr als ein Metaverse-Objekt verknüpft.

Metaverse-Objekte können manuell erstellt oder gelöscht werden. Das Synchronisierungsmodul wird automatisch Metaverse-Objekte, die keine Verknüpfung zu jeder Connectorspace-Objekt im Connectorspace gelöscht.

Um einen entsprechenden Objekttyp in Metaverse Objekte innerhalb einer verbundenen Datenquelle zuordnen, bietet Synchronisierungsmodul ein erweiterbares Schema mit einem vordefinierten Satz von Objekttypen und Attribute fest. Sie können neue Objekttypen und Attribute für Metaverse-Objekte erstellen. Attribute können ein- oder mehrwertig sein und die Attributtypen können Zeichenfolgen, Referenzen, Zahlen und boolesche Werte.

### <a name="relationships-between-staging-objects-and-metaverse-objects"></a>Beziehung zwischen Stagingbereich und Metaverse-Objekte
Datenfluss Namespace Engine Sync Link Beziehung zwischen Stagingbereich und Metaverse-Objekte ermöglicht. Eine Stagingdatenbank ein Metaverse-Objekt verknüpft ist ein **Objekt verknüpft** (oder **Connectorobjekt**) aufgerufen. Eine Stagingdatenbank Metaverse-Objekt nicht verknüpft ist ein **Objekt getrennt** (oder **Disconnector Objekt**) aufgerufen. Die Begriffe verbunden und getrennt lieber nicht zu verwechseln mit verantwortlich für das Importieren und Exportieren von Daten aus einem verbundenen Verzeichnis.

Platzhalter sind nie ein Metaverse-Objekt verknüpft.

Verknüpfte Objekt umfasst ein staging-Objekt und die verknüpfte Beziehung zu einem einzelnen Metaverse-Objekt. Verknüpfte Objekte zur Synchronisation von Attributwerten zwischen einem Connectorspace-Objekt und ein Metaverse-Objekt.

Wird ein staging-Objekt verknüpften Objekts während der Synchronisation, können Attribute zwischen dem staging Objekt und dem Metaverse-Objekt fließen. Attributfluss ist bidirektional und mit Attribut Import und Export Attributregeln konfiguriert ist.

Ein einzelnes Connectorspace-Objekt kann nur ein Metaverse-Objekt verknüpft. Jedoch kann Metaverse-Objekt mit mehreren Connectorspace-Objekte in derselben oder in anderen Räumen verknüpft werden, wie in der folgenden Abbildung dargestellt.

![Arch5](./media/active-directory-aadconnectsync-understanding-architecture/arch5.png)

Verknüpfte Beziehung zwischen der staging und metaverseobjekt persistent ist und entfernt werden kann, werden Regeln, die Sie angeben.

Eine getrennte Objekt ist staging-Objekt, das Metaverse-Objekt nicht verknüpft ist. Attribut nicht getrennte Objekts Werte verarbeitet weitere im Metaverse. Die Attributwerte des entsprechenden Objekts in der verbundenen Datenquelle werden nicht vom Synchronisierungsmodul aktualisiert.

Getrennte Objekte verwenden, können Sie Identitätsinformationen in Synchronisierungsmodul speichern und später bearbeiten. Staging-Objekt als getrennte Objekt im Connectorspace beibehalten hat viele Vorteile. Da das System die erforderliche Informationen für dieses Objekt bereits bereitgestellt wurde, ist nicht erforderlich, eine Darstellung des Objekts wieder beim nächsten Import aus der verbundenen Datenquelle erstellen. Auf diese Weise Synchronisierungsmodul hat immer eine vollständige Momentaufnahme der verbundenen Datenquelle auch wenn keine Verbindung mit der Datenquelle verbunden ist. Getrennte Objekte können in verknüpften Objekte und umgekehrt, je nach den Regeln konvertiert werden, die Sie angeben.

Ein Importobjekt wird als getrennte Objekt erstellt. Ein Objekt exportieren muss einem verknüpften Objekt sein. Die Systemlogik erzwingt diese Regel und löscht alle exportieren Objekt verknüpften Objekts.

## <a name="sync-engine-identity-management-process"></a>Sync-Modul Identity-Prozess
Identity Management-Prozess steuert, wie Identitätsinformationen zwischen anderen verbundenen Datenquellen aktualisiert werden. Identitätsmanagement tritt in drei Prozesse:

- Importieren
- Synchronisierung
- Exportieren

Während des Importvorgangs wertet Synchronisierungsmodul eingehende Identitätsinformationen aus einer verbundenen Datenquelle. Änderung erkannt, es neue Stagingdatenbank Objekte erstellt oder vorhandene staging Objekte im Connectorspace Synchronisierung aktualisiert.

Während der Synchronisation Synchronisierungsmodul aktualisiert das Metaverse Änderungen im Connectorspace aufgetreten sind und im Connectorspace Änderungen, die im Metaverse aufgetreten sind.

Während des Exportvorgangs legt Synchronisierungsmodul Änderungen auf Stagingservern Objekte bereitgestellt werden und als ausstehende Export gekennzeichnet sind.

Die folgende Abbildung zeigt, in der Prozesse als Identität für den Informationsfluss aus einer verbundenen Datenquelle auf einen anderen auftritt.

![Arch6](./media/active-directory-aadconnectsync-understanding-architecture/arch6.png)

### <a name="import-process"></a>Importieren
Während des Importvorgangs wertet Synchronisierungsmodul Updates Identitätsinformationen. Synchronisierungsmodul vergleicht Identitätsinformationen aus der verbundenen Datenquelle mit Identitätsinformationen zu einem Staging- und bestimmt, ob das staging Objekt Updates benötigt. Staging Objekt mit neuen Daten aktualisieren muss, wird das staging Objekt ausstehende importieren gekennzeichnet.

Durch staging Objekte im Connectorspace vor der Synchronisation verarbeiten Synchronisierungsmodul nur die Identitätsinformationen, die geändert wurde. Dieser Prozess bietet die folgenden Vorteile:

- **Effiziente Synchronisation**. Während der Synchronisierung verarbeitete Datenmenge wird minimiert.
- **Effiziente Resynchronisation**. Sie können ändern, wie Synchronisierungsmodul Identitätsinformationen verarbeitet, ohne das Synchronisierungsmodul an die Datenquelle wieder.
- **Möglichkeit einer Vorschau auf Synchronisierung**. Sie können die Synchronisierung überprüfen Ihre Annahmen über den Identity Management-Prozess anzeigen.

Für jedes Objekt im Anschluss angegeben versucht das Synchronisierungsmodul zunächst eine Darstellung des Objekts im Connectorspace des Connectors gefunden. Synchronisierungsmodul untersucht alle staging Objekte im Connectorspace und versucht, ein entsprechendes Stagingordners Objekt gefunden haben, eine entsprechende Ankerattribut. Hat keine vorhandenes Stagingordners Objekt entsprechende Ankerattribut sucht Synchronisierungsmodul ein entsprechendes Stagingordners-Objekt mit demselben Namen unterscheiden.

Wenn Synchronisierungsmodul staging Objekt findet, definierten Namen aber nicht Anker übereinstimmt, tritt das folgende spezielle Verhalten:

- Weist das Objekt im Connectorspace kein Anker, Synchronisierungsmodul entfernt dieses Objekt aus dem Connectorspace und markiert das metaverseobjekt, das es als **Bereitstellung auf der nächsten Synchronisierung erneut**verknüpft ist. Dann erstellt das neue Objekt importieren.
- Weist das Objekt im Connectorspace Anker, nimmt Synchronisierungsmodul, dass dieses Objekt umbenannt wurde oder das im verbundenen Verzeichnis gelöscht. Eine temporäre, neue DN für das Connectorspace-Objekt zugewiesen, damit eingehende Objekt bereitstellen können. Das alte Objekt wird **vorübergehend**, warten die Verbindung umbenennen oder Löschen des Problems importieren.

Wenn Synchronisierungsmodul staging Objekt findet, das in der Verbindung angegebene Objekt entspricht, bestimmt, welche Änderungen zu übernehmen. Z. B. Synchronisierungsmodul möglicherweise umbenennen oder löschen Sie das Objekt in der verbundenen Datenquelle und Attributwerte für das Objekt kann nur aktualisieren.

Staging-Objekte mit aktualisierten Daten werden als ausstehende importieren gekennzeichnet. Verschiedene Einfuhren offen stehen. Je nach Ergebnis des Importvorgangs hat ein staging-Objekt im Connectorspace eines der folgenden offenen Typen importieren:

- **Keine**. Keine Änderung auf alle Attribute des staging-Objekts stehen. Synchronisierungsmodul kennzeichnet dieses Typs nicht als ausstehende importieren.
- **Hinzufügen**. Staging-Objekt ist eine neue Importobjekt im Connectorspace. Synchronisierungsmodul kennzeichnet dieses Typs bis zur weiteren Verarbeitung im Metaverse importieren.
- **Update**. Synchronisierungsmodul findet ein entsprechendes Stagingordners Objekt im Connectorspace und kennzeichnet dieses Typs als ausstehende importieren, damit Updates auf die Attribute im Metaverse verarbeitet werden können. Die Updates umfassen Objekt umbenennen.
- **Löschen**. Synchronisierungsmodul findet ein entsprechendes Stagingordners Objekt im Connectorspace und kennzeichnet dieses Typs als ausstehende importieren, damit verknüpfte Objekt gelöscht werden kann.
- **Löschen/hinzufügen**. Synchronisierungsmodul findet ein entsprechendes Stagingordners Objekt im Connectorspace aber Objekttypen stimmen nicht überein. In diesem Fall ein löschen hinzufügen Änderung bereitgestellt. Ein Delete-add Änderung das Synchronisierungsmodul zeigt an, dass eine vollständige Resynchronisation dieses Objekts erfolgen muss, da unterschiedliche Regeln auf dieses Objekt anwenden, wenn der Objekttyp geändert.

Festlegen der ausstehenden Importstatus staging Objekt, ist deutlich verringert die Datenmenge, die während der Synchronisation verarbeitet, denn so können das System nur die Objekte verarbeiten, die Daten aktualisiert haben.

### <a name="synchronization-process"></a>Synchronisierung
Synchronisierung besteht aus zwei verwandten Prozesse:

- Eingehende Synchronisierung Inhalt Metaverse aktualisiert mit den Daten im Connectorspace.
- Ausgehende Synchronisation Wenn Inhalt im Connectorspace mit Daten im Metaverse aktualisiert wird.

Im Connectorspace bereitgestellten Informationen erstellt die eingehende Synchronisierung im Metaverse integrierte Ansicht der Daten, die in den verbundenen Datenquellen gespeichert ist. Staging alle Objekte oder nur die mit einer ausstehenden Informationen werden zusammengefasst, je nach die Regeln Konfiguration.

Ausgehende Synchronisierung Prozess Updates exportieren Objekte Wenn Metaverse-Objekte ändern.

Eingehende Synchronisierung erstellt die integrierte Ansicht im Metaverse Identitätsinformationen, die von den verbundenen Datenquellen erhalten. Synchronisierungsmodul verarbeiten Identitätsinformationen jederzeit mithilfe der aktuellen Identität, der aus der verbundenen Datenquelle.

**Eingehende Synchronisierung**

Eingehende Synchronisierung umfasst folgende Prozesse:

- **Bereitstellung** (sogenannte **Projektion** ist dabei von ausgehenden Synchronisierung Bereitstellung unterscheiden). Das Synchronisierungsmodul erstellt ein neues metaverseobjekt basierend auf einer staging-Objekt und verknüpft. Bereitstellung wird auf Objektebene.
- **Beitreten**. Das Synchronisierungsmodul verknüpft staging Objekt mit vorhandenen Metaverse-Objekt. Eine Verknüpfung ist ein Vorgang auf Objektebene.
- **Attributfluss importieren**. Synchronisierungsmodul aktualisiert die Attributwerte Attributfluss des Metaverse-Objekts aufgerufen. Import Attributfluss wird auf, die eine Verknüpfung zwischen einer staging und metaverseobjekt erfordert.

Ist der einzige Prozess, der Objekte im Metaverse erstellt. Nur Objekte importieren, die eigenständige Objekte sind unberührt. Synchronisierungsmodul erstellt während der Bereitstellung ein metaverseobjekt, das den Objekttyp des importierten Objekts entspricht und eine Verbindung zwischen beiden Objekten so Erstellen eines verknüpften Objekts.

Der Vorgang des Beitritts wird auch eine Verknüpfung zwischen Objekte importieren und ein Metaverse-Objekt. Die Differenz zwischen der Verknüpfung und Bereitstellung werden Join-Vorgang muss sich das Importobjekt hängen zu einem vorhandenen metaverseobjekt, in dem der Bereitstellungsprozess ein Metaverse-Objekt erstellt.

Synchronisierungsmodul versucht sich Importobjekt Metaverse-Objekt mithilfe von Kriterien, die in der Konfiguration der Synchronisierung angegeben.

Während der Bereitstellung und Join verknüpft Synchronisierungsmodul getrennte Objekt ein Metaverse-Objekt damit verknüpft. Nach Abschluss dieser Vorgänge auf Objektebene können Synchronisierungsmodul die Attributwerte des zugeordneten Metaverse-Objekt. Dieser Vorgang wird importieren Attributfluss bezeichnet.

Import Attributfluss tritt auf alle Objekte importieren, die neue Daten und Metaverse-Objekt verknüpft sind.

**Ausgehende Synchronisation**

Ausgehende Synchronisation Updates exportieren Objekte, wenn ein Metaverse-Objekt ändern, jedoch nicht gelöscht. Ausgehende Synchronisation soll beurteilen, ob die Änderung mit Metaverse-Objekten Updates staging Objekte in den Connector erforderlich. In einigen Fällen können die Änderung erforderlich, Staging Objekte in allen Connectors aktualisiert werden. Staging-Objekte, die geändert werden werden ausstehende exportieren, damit Objekte exportieren gekennzeichnet. Diese Objekte werden später übertragen der verbundenen Datenquelle beim Export exportieren.

Ausgehende Synchronisation hat drei Prozesse:

- **Bereitstellung**
- **Entfernung**
- **Attributfluss exportieren**

Bereitstellung und Entfernung sind beide Operationen auf Objektebene. Entfernung hängt von Bereitstellung, da nur Bereitstellung initiieren kann. Entfernung wird ausgelöst, wenn die Bereitstellung entfernt den Link zwischen einem metaverseobjekt und ein Objekt exportieren.

Bereitstellung wird immer ausgelöst, wenn Objekte im Metaverse Änderungen. Wenn Metaverse-Objekte geändert werden, kann Synchronisierungsmodul als Teil der Bereitstellungsprozess die folgenden Aufgaben ausführen:

- Erstellen Sie verknüpfte Objekte, ein Metaverse-Objekt mit einer neu erstellten Export-Objekt verknüpft ist.
- Umbenennen Sie verknüpfte Objekt.
- Entfernen Sie Links zwischen Metaverse-Objekt und staging-Objekte eine eigenständige erstellen.

Synchronisierungsmodul das Erstellen eines neuen Connectorobjekts Bereitstellung benötigt werden, ist das staging Objekt mit dem Metaverse-Objekt verknüpft ist immer ein Objekt exportieren, da das Objekt nicht in der verbundenen Datenquelle existiert.

Bereitstellung Synchronisierungsmodul das Trennen einer verknüpften-Objekt getrennte Objekt benötigt wird Entfernung ausgelöst. Deprovisioning Prozess löscht das Objekt.

Beim aufheben, wird ein Objekt exportieren physisch das Objekt löschen nicht. Das Objekt wird als **gelöscht**gekennzeichnet, sodass der Delete-Vorgang für das Objekt bereitgestellt wird.

Export Attributfluss tritt auch während der ausgehenden Synchronisierung ähnlich, dass eingehende Synchronisierung Attributfluss Import auftritt. Export erfolgt-Attribut nur zwischen Metaverse und exportieren, die verbunden sind.

### <a name="export-process"></a>Exportvorgang
Während des Exportvorgangs untersucht Synchronisierungsmodul alle exportieren-Objekte, die als ausstehende exportieren im Connectorspace gekennzeichnet und sendet Updates an der verbundenen Datenquelle.

Das Synchronisierungsmodul Ermitteln des Erfolgs der Export jedoch kann ausreichend feststellen, ob der Identity Management-Prozess abgeschlossen ist. Objekte in der verbundenen Datenquelle können immer durch andere Prozesse geändert werden. Da Synchronisierungsmodul keine permanente Verbindung mit der Datenquelle verbunden, reicht es nicht aus der verbundenen Datenquelle anhand einer erfolgreichen Export Benachrichtigung Annahmen über die Eigenschaften eines Objekts zu.

Ein Prozess in der verbundenen Datenquelle könnten z. B. die Attribute des Objekts wieder auf ihre ursprünglichen Werte ändern (d. h. die verbundenen Datenquelle konnte überschreiben die Werte unmittelbar, nachdem die Daten vom Synchronisierungsmodul übertragen und erfolgreich in der verbundenen Datenquelle).

Sync-Modul speichert Statusinformationen zu jedem Testserver Objekt importieren und exportieren. Wenn Werte der Attribute in der Aufnahmeliste Attribut angegebene seit dem letzten Export geändert haben, die Speicherung der import und export Status ermöglicht das Synchronisierungsmodul entsprechend reagieren. Synchronisierungsmodul verwendet den Importvorgang Attributwerte bestätigen, die in der verbundenen Datenquelle exportiert. Ein Vergleich zwischen importierten und exportierten Informationen kann wie in der folgenden Abbildung gezeigt Synchronisierungsmodul bestimmen, ob der Export erfolgreich war oder wiederholt werden.

![Arch7](./media/active-directory-aadconnectsync-understanding-architecture/arch7.png)

Beispielsweise wenn Synchronisierungsmodul Attribut C exportiert hat einen Wert von 5 mit einer verbundenen Datenquelle und speichert es C = 5 der Export Status. Jede zusätzliche Export auf dieses Objekt Versuch, C = 5 in der verbundenen Datenquelle erneut exportieren, da Synchronisierungsmodul wird davon ausgegangen, dass dieser Wert nicht dauerhaft auf das Objekt angewendet wurde (sofern ein anderer Wert zuletzt aus der verbundenen Datenquelle importiert wurde). Der Export-Speicher wird gelöscht, wenn C = 5 bei einem Importvorgang für das Objekt empfangen wird.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über die Konfiguration [Azure AD-Verbindung synchronisieren](active-directory-aadconnectsync-whatis.md) .

Erfahren Sie mehr über [die lokalen Identitäten Azure Active Directory integrieren](active-directory-aadconnect.md).
