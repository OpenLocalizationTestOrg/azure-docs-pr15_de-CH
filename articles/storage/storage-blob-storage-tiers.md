<properties
    pageTitle="Coole Azure-Speicher für Blobs | Microsoft Azure"
    description="Speicher-Tiers für Azure BLOB-Speicher bieten kostengünstigen Speicher für Daten basierend auf Zugriffsmuster. Coole Speicherebene ist optimiert für Daten, die selten zugegriffen wird."
    services="storage"
    documentationCenter=""
    authors="michaelhauss"
    manager="vamshik"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="mihauss"/>


# <a name="azure-blob-storage-hot-and-cool-storage-tiers"></a>Azure BLOB-Speicher: Heißen Sie und kühlen Sie Speicherebenen

## <a name="overview"></a>Übersicht

Azure-Speicher bietet jetzt zwei Speicherebenen für BLOB-Speicher (Objektspeicher) die Daten am kostengünstig je nach Verwendung gespeichert. Azure **hot Speicherebene** ist optimiert, zum Speichern von Daten, auf die häufig zugegriffen werden. Azure **cool Speicherebene** wird zum Speichern von Daten, die nur selten zugegriffen und langlebigen optimiert. Daten in coole Speicherebene Verfügbarkeit eine etwas niedrigere tolerieren können aber weiterhin benötigt Langlebigkeit und ähnliche Zeit Zugriff und Durchsatz Merkmale wie "heiße" Daten. Coole Daten sind geringfügig Verfügbarkeit SLA und höhere Kosten akzeptable Kompromisse für viel geringere Speicherkosten.

Heute wächst die Daten in der Cloud exponentiellen Tempo. Zum Verwalten von Kosten für Ihre wachsenden Speicherbedarf ist es hilfreich, die Daten basierend auf Attributen wie Häufigkeit und geplante Aufbewahrungsdauer. Daten in der Cloud werden anders wie es generiert, verarbeitet und während ihrer Lebensdauer zugegriffen wird. Einige Daten aktiv zugegriffen und während seiner Lebensdauer geändert wird. Einige Daten erfolgt häufig in seiner Lebensdauer mit fallen drastisch Alterung der Daten. Einige Daten im Leerlauf in der Cloud ist selten und wenn, einmal zugegriffen.

Diese Daten zugreifen Szenarien profitiert eine differenzierte Speicherkategorie, die für einen bestimmten Zugriffsmuster optimiert. Mit der Einführung von aktiven und coole Speicherkategorien trennen Azure Blob-Speicher nun diese Notwendigkeit differenzierte Speicherkategorien mit Adressen Preismodelle.

## <a name="blob-storage-accounts"></a>BLOB-Speicherkonten

**BLOB-Speicherkonten** sind spezielle Konten für Ihre unstrukturierten Daten als Blobs (Objekte) in Azure-Speicher gespeichert. BLOB-Speicher Konten jetzt können zwischen aktiven und coole Speicherebenen speichern Sie Ihre weniger genutzten coolen Daten auf eine niedrigere Kosten und Datenspeicher häufiger Zugriff hot auf eine niedrigere Kosten. BLOB-Speicher ähnelt der allgemeinen Speicherkonten und teilen alle hervorragende Haltbarkeit, Verfügbarkeit, Skalierung und Leistungsfunktionen, einschließlich 100 % API Konsistenz für Block-Blobs und Blobs anfügen.

> [AZURE.NOTE] BLOB-Speicherkonten nur unterstützen und Blobs und keine Seitenblobs anfügen.

BLOB-Speicherkonten verfügbar Attribut **Zugriffsschicht** soll der Speicherebene **Hotspot** oder **coole** je nach Konto gespeicherten Daten ermöglichen. Ist eine Änderung der Nutzungsmuster der Daten, können Sie auch diese Speicherkategorien jederzeit wechseln.

> [AZURE.NOTE] Ändern der Speicherebene möglicherweise zusätzliche Gebühren. [Preise und Abrechnung](storage-blob-storage-tiers.md#pricing-and-billing) im Abschnitt Weitere Details anzeigen

Szenarien mit Verwendungsbeispielen für hot Speicherebene gehören:

- Aktiv oder Daten (aus gelesen und geschrieben) häufig zugegriffen werden soll.
- Daten, die für die Verarbeitung und tatsächlichen Migration zu cool Speicherebene bereitgestellt.

Szenarien mit Verwendungsbeispielen für coole Speicherebene gehören:

- Backup, Archivierung und Disaster Recovery Datasets.
- Ältere Medieninhalte nicht häufig mehr angezeigt, sondern sofort bei zugegriffen werden soll.
- Großen Datenmengen, die gespeicherte effektiv sein während zukünftige Verarbeitung mehr Daten gesammelt werden müssen. (*z.B.*Langzeitspeicher raw Telemetriedaten aus einem wissenschaftlichen Daten)
- (Unformatiert) Originaldaten, die auch nach der Verarbeitung in nutzbare Endform beibehalten müssen. (*z.B.*Raw Mediendateien nach transcodieren in andere Formate)
- Compliance und Archivierung, die längere Zeit gespeichert werden und selten zugegriffen. (*z. B.*Überwachungskameras, Alter X-Computertomographie/Mrt für Organisationen im Gesundheitswesen, Audioaufnahmen und Protokolle von Finanzdienstleistungen fordert)

Weitere Informationen zu Speicherkonten finden Sie unter [über Azure-Speicherkonten](storage-create-storage-account.md) .

Für Applikationen erfordert nur blockieren oder BLOB-Speicher Anhängen empfehlen wir Blob Speicherkonten differenzierte Preismodell von tiered Storage nutzen. Wir verstehen jedoch, dass dies möglicherweise nicht unter Umständen mit allgemeinen Speicherkonten Weg zu gehen, wie sein:

- Sie müssen Tabellen, Warteschlangen oder Dateien und Ihre Blobs in demselben Speicherkonto gespeichert. Beachten Sie, dass keine technischen Vorteile speichern in das gleiche Konto als der freigegebenen Schlüssel.
- Sie müssen noch die klassischen Bereitstellungsmodell. BLOB-Speicherkonten sind nur über das Bereitstellungsmodell Azure-Ressourcen-Manager.
- Sie müssen Seitenblobs verwenden. Seitenblobs unterstützt Speicherkonten BLOB nicht. Wir empfehlen generell blockieren Blobs, wenn Sie eine bestimmte Seitenblobs benötigen.
- Sie verwenden eine Version vor 2014-02-14 [Storage Services REST-API](https://msdn.microsoft.com/library/azure/dd894041.aspx) oder eine Clientbibliothek mit eine niedrigere Version als 4.x und die Anwendung können nicht aktualisiert werden.

> [AZURE.NOTE] BLOB-Speicher-Konten werden in den meisten Regionen mit Azure folgen unterstützt. Sie finden die aktualisierte Liste der verfügbaren Bereiche auf der Seite [Azure Services nach Region](https://azure.microsoft.com/regions/#services) .

## <a name="comparison-between-the-storage-tiers"></a>Vergleich zwischen Speicher-tiers

Die folgende Tabelle zeigt den Vergleich zwischen zwei Speicherebenen:

<table border="1" cellspacing="0" cellpadding="0" style="border: 1px solid #000000;">
<col width="250">
<col width="250">
<col width="250">
<tbody>
<tr>
    <td><strong><center></center></strong></td>
    <td><strong><center>Hot Speicherebene</center></strong></td>
    <td><strong><center>Coole Speicherebene</center></strong></td
</tr>
<tr>
    <td><strong><center>Verfügbarkeit</center></strong></td>
    <td><center>99,9 %</center></td>
    <td><center>99 %</center></td>
</tr>
<tr>
    <td><strong><center>Verfügbarkeit<br>(RA-GRS gelesen)</center></strong></td>
    <td><center>99,99 %</center></td>
    <td><center>99,9 %</center></td>
</tr>
<tr>
    <td><strong><center>Nutzungsgebühren</center></strong></td>
    <td><center>Höhere Speicherkosten<br>Zugriff und Transaktion Senken</center></td>
    <td><center>Geringere Speicherkosten<br>Höhere Kosten für Access und Transaktion</center></td>
</tr>
<tr>
    <td><strong><center>Minimale Größe<center></strong></td>
    <td colspan="2"><center>N/A</center></td>
</tr>
<tr>
    <td><strong><center>Minimale Lagerdauer<center></strong></td>
    <td colspan="2"><center>N/A</center></td>
</tr>
<tr>
    <td><strong><center>Wartezeit<br>(Zeit bis zum ersten Byte)<center></strong></td>
    <td colspan="2"><center>Millisekunden</center></td>
</tr>
<tr>
    <td><strong><center>Skalierbarkeits- und Performance-Ziele<center></strong></td>
    <td colspan="2"><center>Wie allgemeine Speicherkonten</center></td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] BLOB-Speicherkonten unterstützen die gleiche Performance und Skalierung Ziele als allgemeine Speicherkonten. Weitere Informationen finden Sie unter [Azure Storage Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md) .

## <a name="pricing-and-billing"></a>Preise und Abrechnung

BLOB-Speicherkonten verwenden ein neues Preismodell für BLOB-Speicher basierend auf der Speicherebene. Ein Speicherkonto Blob mit Abrechnung Folgendes gelten:

- **Speicherkosten**: Zusätzlich Daten, speichern Daten variieren je nach der Speicherebene. Die Kosten pro Gigabyte ist für das coole Speicherebene als hot Speicherebene.
- **Kosten für den Datenzugriff**: Daten in coole Speicherebene wird berechnet eine Gebühr pro Gigabyte Daten Zugriff für Lese- und Schreibvorgänge.
- **Transaktionskosten**: Gibt es eine Gebühr pro Transaktion für beide Ebenen. Die Kosten pro Transaktion cool Speicherebene ist jedoch höher als bei der hot Speicherebene.
- **Geo-Replikation Datenübertragung Kosten**: Dies gilt nur für Konten mit Geo-Replikation konfiguriert, einschließlich GRS und RA-GRS. Geo-Replikation Datenübertragung ist eine Gebühr pro Gigabyte.
- **Ausgehende Datenübertragung Kosten**: ausgehende Datenübertragungen (Daten, die aus einer Azure-Region übertragen) entstehen Abrechnung Bandbreite pro Gigabyte pro, allgemeine Speicherkonten entsprechen.
- **Ändern der Speicherebene**: Ändern der Speicherebene Cool, hot gleich auf alle Daten im Speicherkonto für jeden Übergang Ladung entstehen. Auf der anderen Seite werden Ändern der Speicherebene hot abkühlen kostenlos.

> [AZURE.NOTE] Um Benutzern neue Speicher-Tiers und Funktionalität Post starten kostenlos ändern Speicher überprüfen Tier von Cool hot aus bis zum 30. Juni 2016 entfällt. Ab 1. Juli 2016 wird der Zuschlag auf alle Übergänge von Cool hot angewendet werden. Weitere Informationen über das Preismodell für BLOB-Speicher Seite Konten, [Azure Storage Preise](https://azure.microsoft.com/pricing/details/storage/) . Weitere Informationen über ausgehenden Daten finden transfergebühren [Daten überträgt Preise](https://azure.microsoft.com/pricing/details/data-transfers/) Seite.

## <a name="quick-start"></a>Schnellstart

In diesem Abschnitt zeigen wir Azure-Portal mit folgenden Szenarien:

- Ein BLOB-Speicher-Konto erstellen
- Wie ein BLOB-Speicher-Konto zu verwalten.

### <a name="using-the-azure-portal"></a>Mithilfe des Azure-Portals

#### <a name="create-a-blob-storage-account-using-the-azure-portal"></a>Erstellen Sie ein BLOB-Speicherkonto Azure-portal

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.

2. Wählen Sie im Hub **neu** > **Daten + Speicher** > **Konto**.

3. Geben Sie einen Namen für das Speicherkonto.

    Dieser Name muss eindeutig sein. Es wird als Teil der URL zum Zugriff auf Objekte im Speicherkonto verwendet.  

4. **Ressourcen-Manager** als Bereitstellungsmodell auswählen

    Tiered Storage kann nur mit Storage Resource Manager verwendet werden. Dies ist die empfohlene Bereitstellungsmodell für neue Ressourcen. Weitere Informationen finden Sie der [Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md).  

5. Wählen Sie in der Dropdownliste Art Konto **BLOB-Speicher**.

    Dies ist das Speicherkonto wählen Sie. Tiered Storage ist nicht im allgemeinen Speicher verfügbar. Sie steht nur in den BLOB-Speicher an.    

    Beachten Sie, dass bei Auswahl dieses Tier Leistung auf Standard festgelegt ist. Tiered Storage ist nicht mit der Leistung Premium verfügbar.

6. Die Option Replikation für das Speicherkonto: **LRS** **g**oder **RA-GRS**. Der Standardwert ist **RA-GRS**.

    LRS = lokal redundanten Speicher; G = Geo redundanten Speicher (2 Regionen); RA-GRS ist Lesezugriff Geo redundanten Speicher (2 Bereiche mit Lesezugriff auf die zweite).

    Weitere Informationen zu Azure Storage Replikationsoptionen anzeigen Sie [Azure Storage-Replikation](storage-redundancy.md)

7. Wählen Sie den richtigen Storage Tiers für Ihre Bedürfnisse: **Zugriffsschicht** **Cool** oder **Hotspot**festlegen. Der Standardwert ist **interessant**.

8. Wählen Sie das Abonnement auf das neue Storage-Konto erstellt werden soll.

9. Eine neue Ressourcengruppe oder wählen Sie eine vorhandene Ressourcengruppe. Weitere Informationen zu Ressourcengruppen finden Sie unter [Übersicht über Azure Ressource-Manager](../azure-resource-manager/resource-group-overview.md).

10. Wählen Sie den Bereich für das Speicherkonto.

11. Klicken Sie auf **Erstellen** , um das Speicherkonto erstellen.

#### <a name="change-the-storage-tier-of-a-blob-storage-account-using-the-azure-portal"></a>Ändern der Speicherebene ein BLOB-Speicherkonto Azure-portal

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.

2. Navigieren an das Speicherkonto alle Ressourcen auswählen, und wählen Sie dann das Speicherkonto.

3. Klicken Sie auf **Konfiguration** anzeigen oder Ändern der Kontokonfiguration Blatt Einstellungen.

4. Wählen Sie den richtigen Storage Tiers für Ihre Bedürfnisse: **Zugriffsschicht** **Cool** oder **Hotspot**festlegen.

5. Klicken Sie am oberen Rand der Blade.

> [AZURE.NOTE] Ändern der Speicherebene möglicherweise zusätzliche Gebühren. [Preise und Abrechnung](storage-blob-storage-tiers.md#pricing-and-billing) im Abschnitt Weitere Details anzeigen

## <a name="evaluating-and-migrating-to-blob-storage-accounts"></a>Evaluierung und BLOB-Speicher Konten migrieren

Dieser Abschnitt soll Benutzern reibungslos mit BLOB-Speicherkonten. Es gibt zwei Benutzerszenarien:

- Sie haben ein allgemeine Speicherkonto und sich ein Speicherkonto Blob mit den richtigen Storage Tiers auswerten möchten.
- Sie haben ein Speicherkonto Blob oder bereits vorhanden und auswerten, ob heiß oder kalt Speicherebene verwenden soll.

In beiden Fällen ist die erste Aufgabe zu schätzen Sie die Kosten für Speicherung und Zugriff auf Ihre Daten in ein Blob Speicherkonto, die Ihre aktuellen Kosten vergleichen.

### <a name="evaluating-blob-storage-account-tiers"></a>Auswertung von BLOB-Konto Speicherebenen

Schätzung die Kosten für Speicherung und Zugriff auf Daten in einem BLOB-Speicher-Konto müssen Sie vorhandene Verwendungsmuster oder der erwarteten Verwendungsmuster annähern. Im Allgemeinen sollten Sie wissen:

- Der Speicherbedarf - wie viele Daten gespeichert werden und wie ändert monatlich?
- Die Zugriffsmuster Storage - Datenmenge wird gelesen und geschrieben, das Konto (einschließlich Daten)? Wie viele Transaktionen für den Datenzugriff verwendet werden und welche Arten von Transaktionen sind?

#### <a name="monitoring-existing-storage-accounts"></a>Vorhandene Speicherkonten überwachen

Vorhandene Speicherkonten überwachen und diese Daten können Sie nutzen Azure Speicheranalyse führt Protokollierung und Metrikdaten für ein Speicherkonto.
Speicheranalyse speichert Metriken, die aggregierte Statistiken und Kapazität Transaktionsdaten zu Anfragen an die BLOB-Service für allgemeine Speicherkonten und als Blob Speicherkonten enthalten.
Diese Daten werden in bekannten Tabellen in demselben Speicherkonto gespeichert.

Weitere Informationen finden Sie [Über Storage Analytics-Metriken](https://msdn.microsoft.com/library/azure/hh343258.aspx) und [Storage Analytics Metriken Tabellenschema](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [AZURE.NOTE] BLOB-Speicherkonten verfügbar Dienstendpunkt Tabelle nur zum Speichern von und Zugreifen auf Metrikdaten für dieses Konto.

Speicherbedarf für den BLOB-Speicher-Dienst zu überwachen, müssen Sie die Kapazität Metriken aktivieren.
Diese werden täglich ein Speicherkonto BLOB-Dienst und als ein Tabelleneintrag in der *$MetricsCapacityBlob* Tabelle innerhalb desselben Speicher geschrieben aufzuzeichnen Kapazitätsdaten aufgezeichnet.

Um das Zugriffsmuster der Daten für den BLOB-Speicher-Dienst zu überwachen, müssen Sie stündlich Transaktion Metriken auf API-Ebene aktivieren.
Diese pro API werden Transaktionen stündlich zusammengefasst und als ein Tabelleneintrag in der *$MetricsHourPrimaryTransactionsBlob* Tabelle innerhalb desselben Speicher geschrieben. Die Tabelle *$MetricsHourSecondaryTransactionsBlob* enthält Transaktionen an den sekundären Endpunkt bei RA-GRS Speicherkonten.

> [AZURE.NOTE] Bei allgemeinen Speicherkonto in der Seitenblobs und virtuellen Datenträger mit Block gespeichert haben und BLOB-Daten, ist dabei Schätzung nicht anwendbar. Dies ist keine Möglichkeit, charakteristische Kapazität haben und Transaktion Metriken anhand des BLOBs für nur blockieren Anhängen Blobs, die eine BLOB-Speicher Konto migriert werden können.

Um eine gute Annäherung Sie Daten und Zugriffsmuster erhalten, empfehlen wir Sie einen Aufbewahrungszeitraum für die reguläre Nutzung ist Metriken und extrapolieren.
Eine Möglichkeit ist die Metrikdaten für 7 Tage und die Daten jede Woche für die Analyse am Ende des Monats.
Eine weitere Option ist die Metrikdaten für die letzten 30 Tage beibehalten und sammeln und Analysieren von Daten am Ende der 30 Tage.

Details zum Aktivieren finden sammeln und Anzeigen von Metrikdaten, [Aktivieren von Azure Storage Metriken und Metrikdaten](storage-enable-and-view-metrics.md).

> [AZURE.NOTE] Speichern, zugreifen und herunterladen Analysedaten wird auch wie reguläre Daten berechnet.

#### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Verwendung Verwendung Metriken, Kosten

##### <a name="storage-costs"></a>Speicherkosten

Der letzte Eintrag in Kapazität Metriken Tabelle *$MetricsCapacityBlob* Zeile Schlüssel *'Daten'* zeigt die Speicherkapazität von Benutzerdaten verwendet.
Der letzte Eintrag in Kapazität Metriken Tabelle *$MetricsCapacityBlob* Zeile Schlüssel *'Analyse'* zeigt Protokolle Analytics belegter Speicherkapazität.

Diese Gesamtkapazität von beiden Daten genutzt und Analytics-Protokolle (falls aktiviert) können die Kosten zum Speichern von Daten im Speicherkonto verwendet werden.
Dieselbe Methode kann auch verwendet werden, für die Schätzung der Speicherkosten für Block und Blobs im Allgemeinen Speicherkonten anfügen.

##### <a name="transaction-costs"></a>Transaktionskosten

Die Summe von *'TotalBillableRequests'*in allen Einträgen einer API in der Transaktionstabelle Metriken gibt die Gesamtzahl der Transaktionen für diese bestimmte API. *z. B.*insgesamt *GetBlob"* Transaktionen in einem bestimmten Zeitraum berechnet durch die Summe der berechenbaren Gesamtanzahl für alle mit der Zeile *" User. GetBlob'*.

Schätzung der Kosten für BLOB-Speicherkonten müssen Sie Transaktionen in drei Gruppen unterteilen, da sie unterschiedlich festgesetzt werden.

- Schreibtransaktionen Sie wie *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'*und *'CopyBlob'*.
- Transaktionen wie *'DeleteBlob'* und *'DeleteContainer'*löschen.
- Alle anderen Transaktionen.

Um allgemeine Speicherkonten Kosten zu schätzen, müssen Sie alle Transaktionen unabhängig von Betrieb/API zusammenfassen.

##### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Zugriff und Geo Daten übertragen, Kosten

Speicheranalyse nicht die Menge der Daten lesen und Schreiben auf ein Speicherkonto ermöglichen, können sie anhand der metriktabelle Grob geschätzt.
Die Summe von *'TotalIngress'* in allen Einträgen einer API in der Transaktionstabelle Metriken gibt für diese bestimmte API insgesamt eingehende Datenmenge in Bytes an.
Entsprechend gibt die Summe von *'TotalEgress'* die Gesamtzeit Ausgang Daten in Bytes.

Um schätzen die Kosten für BLOB-Speicherkonten Zugriff müssen Sie Transaktionen in zwei Gruppen aufteilen.

- Die Menge von Daten das Speicherkonto abgeschätzt werden anhand der Summe der *'TotalEgress'* für hauptsächlich die *GetBlob'* und *'CopyBlob'* .
- Das Speicherkonto geschriebene Datenmenge kann suchen auf *'TotalIngress'* in erster Linie die *'PutBlob'*, *'PutBlock'* *'CopyBlob'* und *'AppendBlock'* Vorgänge geschätzt werden.

Die Datenübertragung für Blob-Speicherkonten Geo-Replikation können auch berechnet mithilfe der Vorkalkulation Datenmenge bei ein Speicherkonto g oder RA-GRS geschrieben.

> [AZURE.NOTE] Ein ausführliches Beispiel zur Berechnung der Kosten für die Verwendung der hot oder coole Speicherebene Bitte sehen Sie sich mit dem Titel *"Was sind Hot und Kühlung Zugriff Ebenen und wie fest welche Version Use?"* häufig gestellte Fragen in der [Azure-Speicher Preisseite](https://azure.microsoft.com/pricing/details/storage/).

### <a name="migrating-existing-data"></a>Migrieren von vorhandenen Daten

Ein Speicherkonto Blob zum Speichern von nur spezialisiert und Blobs anfügen. Vorhandene allgemeine Speicherkonten, Tabellen, Warteschlangen, Dateien und Festplatten sowie Blobs speichern, die können, werden nicht auf BLOB-Speicher konvertiert. Um Speicher-Tiers zu verwenden, müssen Sie neue Blob Storage-Konten erstellen und die vorhandenen Daten in den neu erstellten Konten migrieren.
Folgendermaßen können Sie vorhandene Daten in Blob Speicherkonten lokale Speichergeräte, Drittanbieter-Cloud-Speicheranbieter oder vorhandene allgemeine Speicherkonten in Azure migrieren:

#### <a name="azcopy"></a>AzCopy

AzCopy ist ein Windows-Befehlszeilenprogramm entwickelt für leistungsfähige Kopieren von Daten von Azure-Speicher. AzCopy können Daten in das Speicherkonto Blob aus vorhandenen allgemeinen Speicherkonten kopieren oder Hochladen von Daten aus der lokalen Speicher-Devices in das Speicherkonto Blob.

Weitere Informationen finden Sie unter [Datenübertragung mit AzCopy Befehlszeilenprogramm](storage-use-azcopy.md).

#### <a name="data-movement-library"></a>Daten verschieben Bibliothek

Azure Storage-Bewegung-Bibliothek für .NET basiert auf das Framework Core Daten Bewegung, die auch AzCopy. Leistungsstarke, zuverlässige und einfache Übertragung Datenvorgänge wie AzCopy dient die Bibliothek. Dadurch können Sie alle Vorteile der Features von AzCopy in der Anwendung direkt ohne und Überwachung externe Instanzen von AzCopy zu.

Weitere Informationen finden Sie in [Azure Storage Data Bewegung Bibliothek für .net](https://github.com/Azure/azure-storage-net-data-movement)

#### <a name="rest-api-or-client-library"></a>REST-API oder Client-Bibliothek

Sie können eine benutzerdefinierte Anwendung zum Migrieren der Daten berücksichtigt eine BLOB-Speicher mit einer Azure Clientbibliotheken und die Azure Storage REST-API erstellen. Azure-Speicher bietet umfangreiche Clientbibliotheken für mehrere Sprachen und Plattformen wie .NET, Java, C++, Node.JS, PHP, Ruby, Python. Client-Bibliotheken bieten erweiterte Funktionen wie Wiederholungslogik, Protokollierung und parallele Uploads. Sie können auch direkt gegen die REST-API entwickeln, die von einer beliebigen Sprache aufgerufen werden kann, die HTTP/HTTPS-Anforderungen.

Weitere Informationen finden Sie unter [Erste Schritte mit Azure BLOB-Speicher](storage-dotnet-how-to-use-blobs.md).

> [AZURE.NOTE] Speichern von BLOBs mit clientseitigen Verschlüsselung verschlüsselt Verschlüsselung bezogenen Metadaten mit dem Blob gespeichert. Es ist unerlässlich, alle Kopiermechanismus sicherstellen, dass die BLOB-Metadaten und insbesondere die Verschlüsselung bezogenen Metadaten beibehalten. Kopieren von Blobs ohne diese Metadaten wird der BLOB-Inhalt nicht wieder abrufbar. Weitere Informationen zur Verschlüsselung-bezogenen Metadaten finden Sie unter [Azure Storage clientseitige Verschlüsselung](storage-client-side-encryption.md).

## <a name="faqs"></a>Häufig gestellte Fragen

1. **Bestehende Speicherkonten verfügbar?**

    Ja, vorhandene Speicherkonten stehen und Preise oder Funktionen unverändert.  Sie haben nicht die Möglichkeit, einer Speicherebene und tiering-Funktionen nicht in der Zukunft haben.

2. **Warum und wann sollte ich beginnen mit BLOB-Speicherkonten?**

    BLOB-Speicher sind speziell zum Speichern von Blobs und neuen Blob-orientierten Features ermöglichen. Blob Speicherkonten sind die empfohlene Methode zum Speichern von Blobs zukünftiger Funktionen wie hierarchische Speicher und tiering vorgestellt werden basierend auf diesem Konto. Allerdings ist es bis beim migrieren möchten auf Ihre Bedürfnisse.

3. **Kann ich meine vorhandenen Speicherkonto ein Speicherkonto Blob konvertieren?**

    Nein. BLOB-Konto ist eine andere Art von Konto, und Sie müssen neue erstellen und die Daten migrieren, wie oben beschrieben.

4. **Können Objekte in beiden Speicherkategorien in demselben Konto werden gespeichert?**

    *' Zugriffsschicht '* -Attribut, das womit der Speicherebene, auf Kontoebene festgelegt und gilt für alle Objekte in diesem Konto. Zugriff auf Tier-Attribut kann nicht auf Objektebene festgelegt werden.

5. **Kann der Speicherebene Mein Konto BLOB-Speicher ändern?**

    Ja. Sie werden der Speicherebene durch Festlegen des Attributs *"Access Tier* für das Speicherkonto ändern. Ändern der Speicherebene gelten für alle Objekte in das Konto. Ändern der Speicherebene heißen abkühlen entstehen Gebühren, beim Ändern von Cool hot entstehen eine pro GB für das Lesen der Daten in das Konto.

6. **Wie oft kann der Speicherebene Mein Konto BLOB-Speicher ändern?**

    Während wir nicht erzwingen eine Beschränkung der Häufigkeit der Speicherebene geändert werden kann, beachten Sie, Ändern der Speicherebene von Cool zu erhebliche Kosten entstehen. Wir empfehlen nicht der Speicherebene häufig ändern.

7. **Sich Blobs in der cool Speicherebene anders als die in der aktiven Speicherebene verhält?**

    BLOBs in der hot Speicherebene haben die gleiche Wartezeit als Blobs in allgemeine Speicherkonten. BLOBs in der cool Speicherebene haben eine ähnliche Wartezeit (in Millisekunden) als Blobs in allgemeine Speicherkonten.

    BLOBs in der cool Speicherebene müssen ein geringfügig Verfügbarkeit Servicelevel (SLA) als Blobs in hot Speicherebene gespeichert. Weitere Informationen finden Sie unter [SLA für Speicher](https://azure.microsoft.com/support/legal/sla/storage).

8. **Kann Seitenblobs und virtuellen Festplatten im BLOB-Speicher-Konten werden gespeichert?**

    BLOB-Speicherkonten nur unterstützen und Blobs und keine Seitenblobs anfügen. Azure virtuellen Festplatten Seite BLOBs unterstützt und daher Speicherkonten Blob können nicht zum Speichern von virtuellen Festplatten verwendet werden. Jedoch ist es möglich, Backups der virtuellen Festplatten als Block-Blobs in einem BLOB-Speicher-Konto speichern.

9. **Muss ich meine vorhandenen Programme BLOB-Speicher Konten ändern?**

    BLOB-Speicherkonten sind 100 % API mit allgemeinen Speicherkonten für Block und fügen Blobs. Wie die Anwendung Blobs blockieren oder Anhängen Blobs 2014-02-14-Version von [Storage Services REST-API](https://msdn.microsoft.com/library/azure/dd894041.aspx) oder höher verwenden und die Anwendung sollte nur ausgeführt werden. Bei Verwendung eine ältere Version des Protokolls müssen Sie die Anwendung mit der neuen Version um beide Speicherkonten problemlos aktualisieren. Im Allgemeinen verwenden wir empfehlen, die neueste Version unabhängig davon, welches Speicherkonto geben.

10. **Werden sich Benutzer?**

    BLOB-Speicherkonten ähneln allgemeine Speicherkonten zum Speichern von Block und Anhängen Blobs und unterstützt alle Hauptmerkmale der Azure-Speicher, einschließlich Langlebigkeit und Verfügbarkeit, Skalierbarkeit, Leistung und Sicherheit. Funktionen und Einschränkungen für Blob-Speicherkonten und seine Speicherebenen über alles aufgerufen wurden bleibt unverändert.

## <a name="next-steps"></a>Nächste Schritte

### <a name="evaluate-blob-storage-accounts"></a>Blob-Speicherkonten auswerten

[Verfügbarkeit von BLOB-Speicherkonten nach region](https://azure.microsoft.com/regions/#services)

[Auswerten Sie Verwendung des aktuellen Speicherkonten Azure Storage Metriken aktivieren](storage-enable-and-view-metrics.md)

[BLOB-Speicher Preise nach region](https://azure.microsoft.com/pricing/details/storage/)

[Kontrollkästchen Datenübertragungen Preise](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-blob-storage-accounts"></a>Verwenden Sie Speicherkonten Blob

[Erste Schritte mit Azure BLOB-Speicher](storage-dotnet-how-to-use-blobs.md)

[Das Verschieben von Daten aus dem Azure-Speicher](storage-moving-data.md)

[Daten mit AzCopy-Befehlszeilenprogramm](storage-use-azcopy.md)

[Suchen Sie und Durchsuchen Sie Speicherkonten](http://storageexplorer.com/)
