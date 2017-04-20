<properties
    pageTitle="Clientseitige Verschlüsselung mit Python für Microsoft Azure Storage | Microsoft Azure"
    description="Azure Storage-Clientbibliothek für Python unterstützt clientseitige Verschlüsselung für maximale Sicherheit für Ihre Azure-Speicher."
    services="storage"
    documentationCenter="python"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Clientseitige Verschlüsselung mit Python für Microsoft Azure-Speicher

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Übersicht

[Azure Storage-Clientbibliothek für Python](https://pypi.python.org/pypi/azure-storage) unterstützt Verschlüsselung von Daten in Clientanwendungen Azure-Speicher hochgeladen und Entschlüsseln von Daten beim Herunterladen auf den Client.

>[AZURE.NOTE] Azure Storage Python-Bibliothek ist in der Vorschau.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Verschlüsselung und Entschlüsselung über die Umschlag-Technik
Die Prozesse der Ver- und Entschlüsselung folgen Umschlag Verfahren

### <a name="encryption-via-the-envelope-technique"></a>Verschlüsselung über die Umschlag-Technik
Verschlüsselung über die Umschlag-Technik funktioniert wie folgt:

1.  Die Azure-Speicher-Clientbibliothek generiert eine Inhaltsverschlüsselungsschlüssel (CEK) ist ein one-time-Use symmetrischen Schlüssel.

2.  Benutzerdaten werden mit diesem CEK verschlüsselt.

3.  Die CEK wird dann eingeschlossen (verschlüsselt) Verschlüsselung Taste (KEK). Die KEK kann wird durch eine Schlüssel-ID identifiziert und ein asymmetrisches Schlüsselpaar oder symmetrischen Schlüssel lokal verwaltet wird.
Speicher-Clientbibliothek selbst verfügt nicht über KEK. Die Bibliothek ruft Schlüssel Umbruch-Algorithmus, der durch die KEK bereitgestellt wird. Benutzer können benutzerdefinierte Anbieter für wichtige Umbruch/Entpacken verwenden bei Bedarf.

4.  Die verschlüsselten Daten werden dann in Azure Storage Service hochgeladen. Umschlossene Schlüssel zusammen mit einigen zusätzlichen Verschlüsselung Metadaten als Metadaten (Blob) gespeichert oder interpoliert die verschlüsselten Daten (Nachrichten in Warteschlange und Tabellenentitäten).

### <a name="decryption-via-the-envelope-technique"></a>Entschlüsselung über die Umschlag-Technik
Entschlüsselung über die Umschlag-Technik funktioniert wie folgt:

1.  Die Clientbibliothek wird davon ausgegangen, dass der Benutzer den Schlüssel Chiffrierschlüssel (KEK) lokal verwaltet. Der Benutzer muss keine bestimmten Schlüssel kennen, der für die Verschlüsselung verwendet wurde. Stattdessen kann Schlüssel Resolver andere Schlüsselbezeichner Schlüssel ergibt, eingerichtet und verwendet werden.

2.  Die Clientbibliothek downloadet die verschlüsselten Daten zusammen mit Verschlüsselung Material, die im Dienst gespeichert wird.

3.  Der umschlossene Inhaltsverschlüsselungsschlüssel (CEK) wird mit der Verschlüsselung ausgepackt (entschlüsselt) Taste (KEK). Hier wird wieder die Clientbibliothek nicht auf KEK zugreifen. Einfach auspacken Algorithmus des benutzerdefinierten Anbieters aufgerufen.

4.  Der Inhaltsverschlüsselungsschlüssel (CEK) wird dann zum Entschlüsseln der verschlüsselten Daten.

## <a name="encryption-mechanism"></a>Verschlüsselungsmechanismus  
Speicher-Clientbibliothek verwendet [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) , um Daten zu verschlüsseln. Insbesondere [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) Modus mit AES. Jeder Dienst funktioniert etwas anders, damit wir Sie hier besprechen.

### <a name="blobs"></a>BLOBs
Die Clientbibliothek unterstützt derzeit die Verschlüsselung des gesamten Blobs. Insbesondere Verschlüsselung unterstützt, wenn Benutzer **Erstellen*** Methoden. Downloads, beides abgeschlossen und Bereich Downloads unterstützt und Parallelisierung Upload und Download verfügbar.

Bei der Verschlüsselung die Clientbibliothek generiert einen zufälligen Initialisierungsvektor (IV) von 16 Bytes mit einem zufälligen Content Verschlüsselungsschlüssel (CEK) 32 Bytes und Umschlag Verschlüsselung von BLOB-Daten mithilfe dieser Informationen ausführen. Umschlossene CEK und zusätzliche Verschlüsselung Metadaten werden als BLOB-Metadaten zusammen mit den verschlüsselten Blob für den Dienst dann gespeichert.

>[AZURE.WARNING] Wenn bearbeiten oder Hochladen eigener Metadaten für das Blob müssen Sie sicherstellen, dass diese Metadaten beibehalten wird. Hochladen von neuen Metadaten ohne diese Metadaten umschlossene CEK, IV und andere Metadaten verloren und der BLOB-Inhalt wird nie wieder abrufbar sein.

Ein verschlüsseltes Blob Download umfasst den Inhalt des gesamten Blob mit **get*** Hilfsmethoden. Umschlossene CEK ausgepackt und mit IV (in diesem Fall als BLOB-Metadaten gespeichert) verwendet, um die entschlüsselten Daten an den Benutzer zurückzugeben.

Herunterladen eines beliebigen Bereichs (**get*** mit Bereich Parameter übergeben) im verschlüsselten Blob wird angepasst, um eine kleine Menge Daten abgerufen, mit der den angeforderten Bereich erfolgreich entschlüsseln, übermittelten.

Block-Blobs und Seitenblobs beschränkt verschlüsselt/entschlüsselt dieses Schema. Es gibt keine Unterstützung für Verschlüsselung Blobs anfügen.

### <a name="queues"></a>Warteschlangen
Da Warteschlangennachrichten beliebigem Format ist, definiert die Clientbibliothek ein benutzerdefiniertes Format, das der Initialisierungsvektor (IV) und den verschlüsselten Inhalt Verschlüsselungsschlüssel (CEK) in den Nachrichtentext enthält.

Bei der Verschlüsselung die Clientbibliothek generiert einen zufälligen IV von 16 Bytes mit zufälligen CEK 32 Bytes und Umschlag Verschlüsselung des Nachrichtentextes Warteschlange mithilfe dieser Informationen. Umschlossene CEK und einige zusätzliche Verschlüsselung Metadaten sind die verschlüsselte warteschlangennachricht hinzugefügt. Diese geänderte Nachricht (siehe unten) wird vom Dienst gespeichert.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Bei der Entschlüsselung umschlossene Schlüssel aus der warteschlangennachricht extrahiert und ausgepackt. IV ist auch aus der warteschlangennachricht extrahiert und zusammen mit dem allein stehenden Schlüssel zum Entschlüsseln von Nachrichtendaten Warteschlange verwendet. Beachten Sie, dass Verschlüsselung Metadaten klein (unter 500 Bytes), während es auf 64 KB-Limit für eine warteschlangennachricht zählt, verwaltbare auswirken soll.

### <a name="tables"></a>Tabellen
Die Clientbibliothek unterstützt die Verschlüsselung von Entitätseigenschaften für einfügen und Ersetzen-Operationen.

>[AZURE.NOTE] Zusammenführung wird derzeit nicht unterstützt. Eine Teilmenge der Eigenschaften verschlüsselt wurden möglicherweise bereits mit einem anderen Schlüssel, werden einfach Zusammenführen neuen Eigenschaften und Aktualisieren der Metadaten Datenverlust führen. Zusammenführen entweder erfordert zusätzliche Service-Aufrufe der Dienst bereits vorhandene Entität gelesen oder mit einem neuen Schlüssel pro Eigenschaft, die nicht für Leistung.

Tabelle-Verschlüsselung funktioniert wie folgt:

1.  Benutzer geben die Eigenschaften verschlüsselt werden.

2.  Die Clientbibliothek generiert einen zufälligen Initialisierungsvektor (IV) von 16 Bytes mit einem zufälligen Inhaltsverschlüsselungsschlüssel (CEK) 32 Bytes für jede Entität und führt Umschlag Verschlüsselung für die einzelnen Eigenschaften durch Ableiten neuen IV pro Eigenschaft verschlüsselt werden. Die verschlüsselte Eigenschaft ist als Binärdaten gespeichert.

3.  Umschlossene CEK und zusätzliche Verschlüsselung Metadaten werden dann als zwei zusätzliche reservierte Eigenschaften gespeichert. Die erste Eigenschaft reserviert (\_ClientEncryptionMetadata1) ist eine Zeichenfolgeneigenschaft, die Informationen über IV, Version und umschlossenen Schlüssel enthält. Die zweite Eigenschaft reserviert (\_ClientEncryptionMetadata2) ist eine binäre Eigenschaft, die den Informationen enthält, die verschlüsselt sind. Die Informationen in dieser zweiten Eigenschaft (\_ClientEncryptionMetadata2) selbst ist verschlüsselt.

4.  Diese zusätzliche reservierte Eigenschaften für die Verschlüsselung benötigten Benutzer jetzt nur 250 benutzerdefinierte Eigenschaften statt 252 möglicherweise. Die Gesamtgröße der Entität muss weniger als 1 MB sein.

    Beachten Sie, dass nur Eigenschaften verschlüsselt werden können. Andere Eigenschaften sind verschlüsselt werden, müssen sie in Zeichenfolgen konvertiert. Die verschlüsselten Zeichenfolgen werden als binäre Eigenschaften im Dienst gespeichert und sie zurück in Zeichenfolgen konvertiert (unformatierte Zeichenfolgen, nicht EntityProperties mit EdmType.STRING) nach der Entschlüsselung.

    Für Tabellen, die Verschlüsselungsrichtlinie geben Benutzer Eigenschaften verschlüsselt werden. Dies kann durch die Speicherung dieser Eigenschaften TableEntity Objekte mit dem Typ EdmType.STRING und Verschlüsseln auf True festgelegt oder Festlegen der Encryption_resolver_function für das Tableservice-Objekt. Ein Resolver Verschlüsselung ist eine Funktion, die einen Partitionsschlüssel Zeilenschlüssel und Eigenschaftennamen und gibt einen booleschen Wert, der angibt, ob diese Eigenschaft verschlüsselt werden soll. Bei der Verschlüsselung nutzt die Clientbibliothek diese Informationen um zu entscheiden, ob eine Eigenschaft beim Schreiben in das Netzwerk verschlüsselt werden soll. Der Delegat kann auch der Logik vor wie Eigenschaften sind verschlüsselt. (Z. B. Wenn X, dann verschlüsseln Eigenschaft A; andernfalls verschlüsseln Eigenschaften A und b) Beachten Sie, dass es nicht erforderlich, diese Informationen lesen oder Abfragen von Entitäten.

### <a name="batch-operations"></a>Batchvorgänge
Verschlüsselungsrichtlinie gilt für alle Zeilen im Batch. Die Clientbibliothek generiert intern eine neue zufällige IV und zufällige CEK pro Zeile im Stapel. Benutzer können auch unterschiedliche Eigenschaften für jeden Vorgang im Batch definieren dieses Verhalten in der Resolver Verschlüsselung verschlüsseln.
Erstellt ein Stapel als Kontextmanager über die Tableservice batch()-Methode der Tableservice Verschlüsselung automatisch zum Stapel gelten. Ein Stapel explizit durch Aufrufen des Konstruktors erstellt wird, muss die Verschlüsselungsrichtlinie als Parameter übergeben wird und Links für die Lebensdauer des Stapels unverändert.
Beachten Sie, dass Entitäten verschlüsselt werden, wenn sie mit der Batch-Verschlüsselung (Entitäten sind nicht bei Commit Batch mit der Tableservice-Verschlüsselung verschlüsselt) Batch eingefügt werden.

### <a name="queries"></a>Abfragen
Um Abfrage durchzuführen, geben Sie einen Schlüssel Konfliktlöser, der alle Schlüssel im Resultset auflösen kann. Wenn eine Entität im Abfrageergebnis enthalten einen Anbieter aufgelöst werden kann, wird die Clientbibliothek einen Fehler ausgelöst. Für jede Abfrage, die Projektionen der Server-Seite führt, die Clientbibliothek Metadateneigenschaften spezielle Verschlüsselung hinzufügen (\_ClientEncryptionMetadata1 und \_ClientEncryptionMetadata2) standardmäßig ausgewählten Spalten.

>[AZURE.IMPORTANT] Achten Sie auf diese wichtigen Punkte, wenn clientseitige Verschlüsselung:
>
>- Verwenden Sie beim Lesen oder Schreiben auf ein verschlüsseltes Blob ganze Blob hochladen und Range-ganze Blob Download Befehle. Vermeiden Sie schreiben ein verschlüsseltes Blob Protokollvorgänge wie Put Block, Put Block List, Seiten schreiben oder löschen Seiten verwenden; andernfalls möglicherweise beschädigte verschlüsselte Blob und nicht gelesen werden.
>
>- Für Tabellen existiert eine ähnliche Einschränkung. Achten Sie darauf, nicht verschlüsselte Eigenschaften ohne Verschlüsselung Metadaten aktualisieren aktualisieren.
>
>- Wenn Sie Metadaten für verschlüsselten Blob festlegen, können Sie die Verschlüsselung bezogenen Metadaten für Entschlüsselung erforderlich, da das Festlegen von Metadaten nicht additiv überschreiben. Dies gilt auch für Snapshots. Vermeiden Sie beim Erstellen einer Momentaufnahme ein verschlüsseltes Blob Metadaten angeben. Wenn Metadaten festgelegt werden muss, müssen Sie die **Get_blob_metadata** -Methode zuerst zu aktuellen Verschlüsselung Metadaten und Vermeidung gleichzeitige Metadaten festgelegt wird.
>
>- Aktivieren Sie das **Require_encryption** -Flag auf das Dienstobjekt für Benutzer, die mit Daten arbeiten muss. Weitere Informationen finden Sie weiter unten.

Die Speicher-Clientbibliothek erwartet bereitgestellten KEK und wichtige Resolver folgende Schnittstelle implementieren. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) -Unterstützung für Python KEK steht und integriert diese Bibliothek Abschluss.


## <a name="client-api--interface"></a>Client-API / Schnittstelle
Nachdem ein Service-Speicherobjekts (d. h. Blockblobservice) erstellt, der Benutzer kann Werte zuweisen die Felder, die eine Verschlüsselungsrichtlinie bilden: Key_encryption_key, Key_resolver_function und Require_encryption. Benutzer können nur KEK bereitstellen ein Konfliktlöser oder beides. Key_encryption_key ist der grundlegende Schlüssel, die mit einer Schlüssel-ID gekennzeichnet wird und die Logik für Verpackung/Auspacken dient. Key_resolver_function dient zum Auflösen eines Schlüssels während der Entschlüsselung. Eine gültige KEK eine Schlüssel-ID angegeben wird. Dies ermöglicht Benutzern zwischen mehreren Schlüsseln auswählen, die an mehreren Standorten verwaltet werden.

Die KEK müssen die folgenden Methoden, um Daten zu verschlüsseln:
- wrap_key(cek): Schließt die angegebenen CEK (Bytes) mit einem Algorithmus der Auswahl des Benutzers. Der umschlossenen Schlüssel zurückgegeben.
- get_key_wrap_algorithm(): die Algorithmus verwendet Schlüssel gibt.
- get_kid(): Gibt die Zeichenfolge Schlüssel-Id für dieses KEK.
Die KEK müssen die folgenden Methoden, um Daten zu entschlüsseln:
- Unwrap_key (Cek, Algorithmus): ausgepackt aus den angegebenen CEK mit der Zeichenfolge angegebenen Algorithmus gibt.
- get_kid(): Gibt eine Zeichenfolge Schlüssel-Id für dieses KEK.

Die wichtigste Auflösung muss mindestens eine Methode implementieren, die eine Schlüssel-Id angegebenen entsprechende KEK implementiert die Schnittstelle zurückgibt. Diese Methode ist die Key_resolver_function-Eigenschaft auf das Objekt zugewiesen werden.

- Für die Verschlüsselung des Schlüssels immer und das Fehlen eines Schlüssels führt zu einem Fehler.
- Für die Entschlüsselung:
    - Key Resolver wird aufgerufen, wenn der Schlüssel angegeben. Wenn der Resolver hat keine Zuordnung für den Schlüsselbezeichner angegeben ist, wird ein Fehler ausgelöst.
    - Auflösung nicht angegeben, jedoch ein Schlüssel angegeben wird, wird der Schlüssel verwendet, wenn die erforderliche Schlüssel-ID übereinstimmt. Wenn der Bezeichner nicht übereinstimmt, wird ein Fehler ausgelöst.

      Die Verschlüsselung Beispiele in azure.storage.samples <fix URL>ein detailliertere End-to-End-Szenario für Blobs, Queues und Tabellen veranschaulichen.
        Implementierungsbeispiele KEK und wichtige Konfliktlöser werden bzw. in den Beispieldateien KeyWrapper und KeyResolver bereitgestellt.

### <a name="requireencryption-mode"></a>RequireEncryption Modus
Benutzer können optional einen Betriebsmodus, in dem alle Uploads und Downloads verschlüsselt werden müssen. In diesem Modus fehl, versucht, ohne eine Verschlüsselungsrichtlinie upload oder download für den Dienst nicht verschlüsselten Daten auf dem Client. Das **Require_encryption** -Flag auf das Objekt steuert dieses Verhalten.

### <a name="blob-service-encryption"></a>BLOB-Dienst Verschlüsselung
Die Verschlüsselung Bereiche des Blockblobservice-Objekts festgelegt. Alles von der Clientbibliothek intern erfolgt.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_block_blob_service.key_encryption_key = kek
    my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

    # Upload the encrypted contents to the blob.
    my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

    # Download and decrypt the encrypted contents from the blob.
    blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)

### <a name="queue-service-encryption"></a>Die Verschlüsselung der service
Die Verschlüsselung Bereiche des Queueservice-Objekts festgelegt. Alles von der Clientbibliothek intern erfolgt.

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Set the KEK and key resolver on the service object.
    my_queue_service.key_encryption_key = kek
    my_queue_service.key_resolver_funcion = key_resolver.resolve_key

    # Add message
    my_queue_service.put_message(queue_name, content)

    # Retrieve message
    retrieved_message_list = my_queue_service.get_messages(queue_name)

### <a name="table-service-encryption"></a>Service Verschlüsselung
Eine Verschlüsselungsrichtlinie erstellen und auf Wunsch Optionen festlegen, müssen Sie eine **Encryption_resolver_function** auf der **Tableservice**angeben oder Attributs Verschlüsseln der EntityProperty festlegen.

### <a name="using-the-resolver"></a>Der Konfliktlöser

    # Create the KEK used for encryption.
    # KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
    kek = KeyWrapper('local:key1') # Key identifier

    # Create the key resolver used for decryption.
    # KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
    key_resolver = KeyResolver()
    key_resolver.put_key(kek)

    # Define the encryption resolver_function.
    def my_encryption_resolver(pk, rk, property_name):
            if property_name == 'foo':
                    return True
            return False

    # Set the KEK and key resolver on the service object.
    my_table_service.key_encryption_key = kek
    my_table_service.key_resolver_funcion = key_resolver.resolve_key
    my_table_service.encryption_resolver_function = my_encryption_resolver

    # Insert Entity
    my_table_service.insert_entity(table_name, entity)

    # Retrieve Entity
    # Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
    my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])

### <a name="using-attributes"></a>Mithilfe von Attributen
Wie bereits erwähnt, kann eine Eigenschaft für die Verschlüsselung gekennzeichnet werden in einem EntityProperty Objekt speichern und Festlegen des verschlüsseln.

    encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)

## <a name="encryption-and-performance"></a>Verschlüsselung und Leistung
Beachten Sie, dass Verschlüsselung Speicher Datenergebnisse im zusätzlichen Leistungsaufwand. Schlüssel und IV generiert werden muss, muss der Inhalt verschlüsselt und zusätzliche Metadaten formatiert und übertragen werden. Dieser Aufwand variiert je nach der verschlüsselten Daten. Wir empfehlen Kunden immer ihre Anträge auf Leistung während der Entwicklung testen.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Storage-Clientbibliothek für Java PyPi Paket](https://pypi.python.org/pypi/azure-storage) herunterladen
- [Azure-Speicher-Clientbibliothek für Python Quellcode von GitHub](https://github.com/Azure/azure-storage-python) herunterladen
