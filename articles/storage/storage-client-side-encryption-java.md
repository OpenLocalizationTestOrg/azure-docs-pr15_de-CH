<properties
    pageTitle="Clientseitige Verschlüsselung mit Java für Microsoft Azure Storage | Microsoft Azure"
    description="Azure Storage-Clientbibliothek für Java unterstützt clientseitige Verschlüsselung und Integration mit Azure Schlüssel für maximale Sicherheit für Ihre Azure-Speicher."
    services="storage"
    documentationCenter="java"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>


# <a name="client-side-encryption-with-java-for-microsoft-azure-storage"></a>Clientseitige Verschlüsselung mit Java für Microsoft Azure-Speicher   

[AZURE.INCLUDE [storage-selector-client-side-encryption-include](../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Übersicht  

[Azure Storage-Clientbibliothek für Java](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) unterstützt Verschlüsselung von Daten in Clientanwendungen Azure-Speicher hochgeladen und Entschlüsseln von Daten beim Herunterladen auf den Client. Die Bibliothek unterstützt auch die Integration mit [Azure Schlüssel](https://azure.microsoft.com/services/key-vault/) für das Speichermanagement für Konto.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Verschlüsselung und Entschlüsselung über die Umschlag-Technik    
Die Prozesse der Ver- und Entschlüsselung folgen Umschlag Verfahren  

### <a name="encryption-via-the-envelope-technique"></a>Verschlüsselung über die Umschlag-Technik  
Verschlüsselung über die Umschlag-Technik funktioniert wie folgt:  

1.  Die Azure-Speicher-Clientbibliothek generiert eine Inhaltsverschlüsselungsschlüssel (CEK) ist ein one-time-Use symmetrischen Schlüssel.  

2.  Benutzerdaten werden mit diesem CEK verschlüsselt.   

3.  Die CEK wird dann eingeschlossen (verschlüsselt) Verschlüsselung Taste (KEK). Die KEK kann wird durch eine Schlüssel-ID identifiziert und ein asymmetrisches Schlüsselpaar oder eines symmetrischen Schlüssels und können werden lokal verwaltete oder in Azure Schlüssel Depots gespeichert.  
Speicher-Clientbibliothek selbst verfügt nicht über KEK. Die Bibliothek ruft Schlüssel Umbruch-Algorithmus, der von Schlüssel bereitgestellt. Benutzer können benutzerdefinierte Anbieter für wichtige Umbruch/Entpacken verwenden bei Bedarf.  

4.  Die verschlüsselten Daten werden dann in Azure Storage Service hochgeladen. Umschlossene Schlüssel zusammen mit einigen zusätzlichen Verschlüsselung Metadaten als Metadaten (Blob) gespeichert oder interpoliert die verschlüsselten Daten (Nachrichten in Warteschlange und Tabellenentitäten).

### <a name="decryption-via-the-envelope-technique"></a>Entschlüsselung über die Umschlag-Technik  
Entschlüsselung über die Umschlag-Technik funktioniert wie folgt:  

1.  Die Clientbibliothek wird davon ausgegangen, dass der Benutzer den Schlüssel Chiffrierschlüssel (KEK) lokal oder in Azure Schlüssel Depots verwaltet. Der Benutzer muss keine bestimmten Schlüssel kennen, der für die Verschlüsselung verwendet wurde. Stattdessen kann wichtiger Resolver, der anderen Schlüsselbezeichner Tasten löst, eingerichtet und verwendet.  

2.  Die Clientbibliothek downloadet die verschlüsselten Daten zusammen mit Verschlüsselung Material, die im Dienst gespeichert wird.  

3.  Der umschlossene Inhaltsverschlüsselungsschlüssel (CEK) wird mit der Verschlüsselung ausgepackt (entschlüsselt) Taste (KEK). Hier wird wieder die Clientbibliothek nicht auf KEK zugreifen. Es ruft einfach das benutzerdefinierte oder Schlüssel Depot Anbieter Entpacken Algorithmus.  

4.  Der Inhaltsverschlüsselungsschlüssel (CEK) wird dann zum Entschlüsseln der verschlüsselten Daten.

## <a name="encryption-mechanism"></a>Verschlüsselungsmechanismus  
Speicher-Clientbibliothek verwendet [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) , um Daten zu verschlüsseln. Insbesondere [Cipher Block Chaining (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) Modus mit AES. Jeder Dienst funktioniert etwas anders, damit wir Sie hier besprechen.

### <a name="blobs"></a>BLOBs  
Die Clientbibliothek unterstützt derzeit die Verschlüsselung des gesamten Blobs. Insbesondere Verschlüsselung unterstützt, wenn Benutzer das **Hochladen** * Methoden oder * *OpenOutputStream** Methode. Für Downloads, beides abgeschlossen und Bereich Downloads unterstützt werden.  

Bei der Verschlüsselung die Clientbibliothek generiert einen zufälligen Initialisierungsvektor (IV) von 16 Bytes mit einem zufälligen Content Verschlüsselungsschlüssel (CEK) 32 Bytes und Umschlag Verschlüsselung von BLOB-Daten mithilfe dieser Informationen ausführen. Umschlossene CEK und zusätzliche Verschlüsselung Metadaten werden als BLOB-Metadaten zusammen mit den verschlüsselten Blob für den Dienst dann gespeichert.

>[AZURE.WARNING] Wenn bearbeiten oder Hochladen eigener Metadaten für das Blob müssen Sie sicherstellen, dass diese Metadaten beibehalten wird. Hochladen von neuen Metadaten ohne diese Metadaten umschlossene CEK, IV und andere Metadaten verloren und der BLOB-Inhalt wird nie wieder abrufbar sein.

Ein verschlüsseltes Blob Download umfasst den Inhalt des gesamten Blob mit */openInputStream *herunterladen*** Hilfsmethoden. Umschlossene CEK ausgepackt und mit IV (in diesem Fall als BLOB-Metadaten gespeichert) verwendet, um die entschlüsselten Daten an den Benutzer zurückzugeben.

Herunterladen eines beliebigen Bereichs (**DownloadRange***-Methoden) im verschlüsselten Blob wird angepasst, um eine kleine Menge Daten abgerufen, mit der den angeforderten Bereich erfolgreich entschlüsseln, von Benutzern bereitgestellt.  

Alle BLOB-Typen (blockieren Blobs Seite Blobs und Anhängen Blobs) können verschlüsselt/entschlüsselt werden mit diesem Schema.

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

3.  Umschlossene CEK und zusätzliche Verschlüsselung Metadaten werden dann als zwei zusätzliche reservierte Eigenschaften gespeichert. Die erste reservierte Eigenschaft (_ClientEncryptionMetadata1) ist eine Zeichenfolgeneigenschaft, die Informationen über IV, Version und umschlossenen Schlüssel enthält. Die zweite reservierte Eigenschaft (_ClientEncryptionMetadata2) ist eine binäre Eigenschaft, die den Informationen enthält, die verschlüsselt sind. Die Informationen in dieser zweiten Eigenschaft (_ClientEncryptionMetadata2) ist selbst verschlüsselt.  

4.  Diese zusätzliche reservierte Eigenschaften für die Verschlüsselung benötigten Benutzer jetzt nur 250 benutzerdefinierte Eigenschaften statt 252 möglicherweise. Die Gesamtgröße der Entität muss weniger als 1 MB sein.  

    Beachten Sie, dass nur Eigenschaften verschlüsselt werden können. Andere Eigenschaften sind verschlüsselt werden, müssen sie in Zeichenfolgen konvertiert. Die verschlüsselten Zeichenfolgen werden als binäre Eigenschaften im Dienst gespeichert und sie nach der Entschlüsselung in Zeichenfolgen konvertiert.

    Für Tabellen, die Verschlüsselungsrichtlinie geben Benutzer Eigenschaften verschlüsselt werden. Dies erfolgt durch Festlegen [verschlüsseln] Attributs (für POCO-Entitäten, die von TableEntity abgeleitet) oder einen Konfliktlöser Verschlüsselung Anforderung Optionen. Ein Resolver Verschlüsselung ist ein Delegat, die einen Partitionsschlüssel Zeilenschlüssel und Eigenschaftennamen und gibt einen booleschen Wert, der angibt, ob diese Eigenschaft verschlüsselt werden soll. Bei der Verschlüsselung nutzt die Clientbibliothek diese Informationen um zu entscheiden, ob eine Eigenschaft beim Schreiben in das Netzwerk verschlüsselt werden soll. Der Delegat kann auch der Logik vor wie Eigenschaften sind verschlüsselt. (Z. B. Wenn X, dann verschlüsseln Eigenschaft A; andernfalls verschlüsseln Eigenschaften A und b) Beachten Sie, dass es nicht erforderlich, diese Informationen lesen oder Abfragen von Entitäten.

### <a name="batch-operations"></a>Batchvorgänge  
In Batch-Vorgängen wird dieselbe KEK über alle Zeilen im Batch-Betrieb verwendet, da die Clientbibliothek nur ein Options-Objekt (und somit eine Gruppenrichtlinien-KEK) kann pro Arbeitsgang. Jedoch wird die Clientbibliothek intern einen neuen zufälligen IV und zufällige CEK pro Zeile im Stapel generiert. Benutzer können auch unterschiedliche Eigenschaften für jeden Vorgang im Batch definieren dieses Verhalten in der Resolver Verschlüsselung verschlüsseln.

### <a name="queries"></a>Abfragen  
Um Abfrage durchzuführen, geben Sie einen Schlüssel Konfliktlöser, der alle Schlüssel im Resultset auflösen kann. Wenn eine Entität im Abfrageergebnis enthalten einen Anbieter aufgelöst werden kann, wird die Clientbibliothek einen Fehler ausgelöst. Für jede Abfrage, die Server Seite Projektionen führt, wird die Clientbibliothek Metadateneigenschaften spezielle Verschlüsselung (_ClientEncryptionMetadata1 und _ClientEncryptionMetadata2) standardmäßig ausgewählten Spalten hinzufügen.

## <a name="azure-key-vault"></a>Azure-Tresor  
Azure Key Vault können kryptografische Schlüssel schützen und Geheimnisse von Cloudanwendungen und Diensten verwendet. Mithilfe von Azure Key Vault können Benutzer Schlüssel und geheime Schlüssel (wie Authentifizierungsschlüssel, speicherkontoschlüssel, Verschlüsselungsschlüssel, verschlüsselt. PFX-Dateien und Kennwörter) Tasten, die Hardwaresicherheitsmodule (HSMs) geschützt sind. Weitere Informationen finden Sie unter [Neuigkeiten Azure Key Vault?](../key-vault/key-vault-whatis.md).

Die Speicher-Clientbibliothek verwendet Schlüssel Vault-Kernbibliothek um ein allgemeines Framework zum Verwalten von Schlüsseln in Azure bereitzustellen. Benutzer erhalten auch den zusätzlichen Vorteil von Key Vault-Erweiterungsbibliothek. Die Erweiterungsbibliothek bietet nützliche Funktionalität einfach und nahtlos symmetrisch/RSA lokalen und Cloud Schlüsselanbieter und Aggregation und Zwischenspeichern.

### <a name="interface-and-dependencies"></a>Schnittstelle und Abhängigkeit  
Es gibt drei Schlüssel Vault-Pakete:  

- Azure-schlüsseltresor-Core enthält die IKey und IKeyResolver. Es ist ein kleines Paket ohne Abhängigkeiten. Die Speicher-Clientbibliothek für Java definiert als Abhängigkeit.
- Azure-schlüsseltresor enthält Schlüssel Depot REST-Client.  
- Azure-schlüsseltresor-Erweiterungen enthält Erweiterungscode, die Implementierung von Kryptografiealgorithmen und ein RSAKey und ein SymmetricKey enthält. Es hängt Namespaces Core und Schlüsseltresor und definieren eine aggregierte Resolver (wenn Benutzer mehrere Schlüsselanbieter verwenden möchten) und wichtige Cacheauflösungsdienst ermöglicht. Obwohl Speicher-Clientbibliothek nicht direkt von diesem Paket abhängt möchten Benutzer Azure Key Vault ihre Schlüssel oder Schlüssel Depot Extensions lokalen und cloud-Kryptografiedienstanbieter verwenden, benötigen sie dieses Paket.  

  Key Vault erhält für hochwertige Hauptschlüssel und Drosselung Grenzwerte pro Depot Schlüssel dienen dabei beachten. Bei clientseitigen Encryption mit Schlüssel ist das bevorzugte Modell lokal mit symmetrischen Hauptschlüssel Geheimnisse Schlüssel Depot gespeichert und zwischengespeichert. Benutzer müssen Folgendes tun:  

1.  Erstellen Sie ein Geheimnis offline und auf Schlüssel Depot hochladen.  

2.  Verwenden Sie das Geheimnis Basisbezeichner als Parameter die aktuelle Version der geheime Schlüssel für die Verschlüsselung und Informationen lokal zwischenspeichern. Verwenden Sie CachingKeyResolver zum Zwischenspeichern. Benutzer werden voraussichtlich Implementieren ihrer eigenen Logik Zwischenspeichern.  

3.  Verwenden Sie der Cacheauflösungsdienst beim Erstellen von die Verschlüsselungsrichtlinie als Eingabe.
Weitere Informationen zur Verwendung von Schlüsseltresor finden in den Codebeispielen Verschlüsselung. <fix URL>  

## <a name="best-practices"></a>Bewährte Methoden  
Unterstützung für Verschlüsselung ist nur in der Speicher-Clientbibliothek für Java.

>[AZURE.IMPORTANT] Achten Sie auf diese wichtigen Punkte, wenn clientseitige Verschlüsselung:
>  
>- Verwenden Sie beim Lesen oder Schreiben auf ein verschlüsseltes Blob ganze Blob hochladen und Range-ganze Blob Download Befehle. Vermeiden Sie schreiben auf ein verschlüsseltes Blob mit Protokoll wie Put Block, Put Block List, Seiten schreiben, Seiten löschen oder Anhängen blockieren; andernfalls möglicherweise beschädigte verschlüsselte Blob und nicht gelesen werden.
>
>- Für Tabellen existiert eine ähnliche Einschränkung. Achten Sie darauf, nicht verschlüsselte Eigenschaften ohne Verschlüsselung Metadaten aktualisieren aktualisieren.  
>
>- Wenn Sie Metadaten für verschlüsselten Blob festlegen, können Sie die Verschlüsselung bezogenen Metadaten für Entschlüsselung erforderlich, da das Festlegen von Metadaten nicht additiv überschreiben. Dies gilt auch für Snapshots. Vermeiden Sie beim Erstellen einer Momentaufnahme ein verschlüsseltes Blob Metadaten angeben. Wenn Metadaten festgelegt werden muss, müssen Sie die **DownloadAttributes** -Methode zuerst zu aktuellen Verschlüsselung Metadaten und Vermeidung gleichzeitige Metadaten festgelegt wird.  
>
>- Aktivieren Sie das Flag **RequireEncryption** in die Anforderung Standardoptionen für Benutzer, die mit Daten arbeiten müssen. Weitere Informationen finden Sie weiter unten.  

## <a name="client-api--interface"></a>Client-API / Schnittstelle  
Beim Erstellen eines Objekts EncryptionPolicy, erhalten Benutzer nur einen Schlüssel (implementieren IKey) nur ein Resolver (implementieren IKeyResolver) oder beides. IKey ist der grundlegende Schlüssel, die mit einer Schlüssel-ID gekennzeichnet wird und die Logik für Verpackung/Auspacken dient. IKeyResolver dient zum Auflösen eines Schlüssels während der Entschlüsselung. Es definiert eine ResolveKey Methode, die ein IKey erhalten eine Schlüssel-ID zurückgibt. Dies ermöglicht Benutzern zwischen mehreren Schlüsseln auswählen, die an mehreren Standorten verwaltet werden.

- Für die Verschlüsselung des Schlüssels immer und das Fehlen eines Schlüssels führt zu einem Fehler.  
- Für die Entschlüsselung:  
    - Key Resolver wird aufgerufen, wenn der Schlüssel angegeben. Wenn der Resolver hat keine Zuordnung für den Schlüsselbezeichner angegeben ist, wird ein Fehler ausgelöst.  
    - Auflösung nicht angegeben, jedoch ein Schlüssel angegeben wird, wird der Schlüssel verwendet, wenn die erforderliche Schlüssel-ID übereinstimmt. Wenn der Bezeichner nicht übereinstimmt, wird ein Fehler ausgelöst.  

      Die [Verschlüsselung Beispiele](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) <fix URL>Key Vault-Integration zeigen ein detailliertere End-to-End-Szenario für Blobs, Queues und Tabellen zusammen.

### <a name="requireencryption-mode"></a>RequireEncryption Modus  
Benutzer können optional einen Betriebsmodus, in dem alle Uploads und Downloads verschlüsselt werden müssen. In diesem Modus fehl, versucht, ohne eine Verschlüsselungsrichtlinie upload oder download für den Dienst nicht verschlüsselten Daten auf dem Client. Das **RequireEncryption** -Flag des Anforderungsobjekts Optionen steuert dieses Verhalten. Wenn die Anwendung alle Objekte in Azure Storage verschlüsselt wird, können Sie die **RequireEncryption** -Eigenschaft auf Anforderung Standardoptionen für das Serviceobjekt Client festlegen.   

Z. B. **CloudBlobClient.getDefaultRequestOptions().setRequireEncryption(true)** , Verschlüsselung für alle Blob-Operationen über das Clientobjekt.

### <a name="blob-service-encryption"></a>BLOB-Dienst Verschlüsselung  
Erstellen Sie ein **BlobEncryptionPolicy** -Objekt, und legen sie die Anforderung Optionen (pro-API oder auf Clientebene mit **DefaultRequestOptions**). Alles von der Clientbibliothek intern erfolgt.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

    // Set the encryption policy on the request options.
    BlobRequestOptions options = new BlobRequestOptions();
    options.setEncryptionPolicy(policy);

    // Upload the encrypted contents to the blob.
    blob.upload(stream, size, null, options, null);

    // Download and decrypt the encrypted contents from the blob.
    ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
    blob.download(outputStream, null, options, null);

### <a name="queue-service-encryption"></a>Die Verschlüsselung der service  
Erstellen Sie ein **QueueEncryptionPolicy** -Objekt, und legen sie die Anforderung Optionen (pro-API oder auf Clientebene mit **DefaultRequestOptions**). Alles von der Clientbibliothek intern erfolgt.

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

    // Add message
    QueueRequestOptions options = new QueueRequestOptions();
    options.setEncryptionPolicy(policy);

    queue.addMessage(message, 0, 0, options, null);

    // Retrieve message
    CloudQueueMessage retrMessage = queue.retrieveMessage(30, options, null);

### <a name="table-service-encryption"></a>Service Verschlüsselung  
Eine Verschlüsselungsrichtlinie erstellen und auf Wunsch Optionen festlegen, müssen Sie eine **EncryptionResolver** in **TableRequestOptions**angeben oder Festlegen [verschlüsseln]-Attribut die Getter und Setter.

### <a name="using-the-resolver"></a>Der Konfliktlöser  

    // Create the IKey used for encryption.
    RsaKey key = new RsaKey("private:key1" /* key identifier */);

    // Create the encryption policy to be used for upload and download.
    TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

    TableRequestOptions options = new TableRequestOptions()
    options.setEncryptionPolicy(policy);
    options.setEncryptionResolver(new EncryptionResolver() {
        public boolean encryptionResolver(String pk, String rk, String key) {
            if (key == "foo")
            {
                return true;
            }
            return false;
        }
    });

    // Insert Entity
    currentTable.execute(TableOperation.insert(ent), options, null);

    // Retrieve Entity
    // No need to specify an encryption resolver for retrieve
    TableRequestOptions retrieveOptions = new TableRequestOptions()
    retrieveOptions.setEncryptionPolicy(policy);

    TableOperation operation = TableOperation.retrieve(ent.PartitionKey, ent.RowKey, DynamicTableEntity.class);
    TableResult result = currentTable.execute(operation, retrieveOptions, null);

### <a name="using-attributes"></a>Mithilfe von Attributen  
Wie erwähnt, wenn die Entität TableEntity implementiert, können dann die Eigenschaften Getter und Setter mit [verschlüsseln]-Attribut statt **EncryptionResolver**ergänzt werden.

    private string encryptedProperty1;

    @Encrypt
    public String getEncryptedProperty1 () {
        return this.encryptedProperty1;
    }

    @Encrypt
    public void setEncryptedProperty1(final String encryptedProperty1) {
        this.encryptedProperty1 = encryptedProperty1;
    }

## <a name="encryption-and-performance"></a>Verschlüsselung und Leistung  
Beachten Sie, dass Verschlüsselung Speicher Datenergebnisse im zusätzlichen Leistungsaufwand. Schlüssel und IV generiert werden muss, muss der Inhalt verschlüsselt und zusätzliche Metadaten formatiert und übertragen werden. Dieser Aufwand variiert je nach der verschlüsselten Daten. Wir empfehlen Kunden immer ihre Anträge auf Leistung während der Entwicklung testen.

## <a name="next-steps"></a>Nächste Schritte  

- [Azure Storage-Clientbibliothek für Java Maven-Paket](http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage) herunterladen  
- [Azure-Speicher-Clientbibliothek für Java-Quellcode von GitHub](https://github.com/Azure/azure-storage-java) herunterladen   
- Azure Key Vault Maven Library für Java Maven Pakete herunterladen:
    - [Paket](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault-core)
    - [Client](http://mvnrepository.com/artifact/com.microsoft.azure/azure-keyvault) -Paket
- Besuchen Sie [Azure Key Vault Dokumentation](../key-vault/key-vault-whatis.md)  
