<properties
    pageTitle="Azure Storage Service Verschlüsselung für ruhende Daten | Microsoft Azure"
    description="Verwenden Sie Azure Storage Service-Verschlüsselung, um Ihre Azure BLOB-Speicher auf der Dienstseite verschlüsseln, wenn die Daten gespeichert und beim Abrufen der Daten zu entschlüsseln."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="robinsh"/>

# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service Verschlüsselung für ruhende Daten

Azure Storage Service Verschlüsselung (SSE) für ruhende Daten können Sie schützen und Ihre Daten entsprechend Ihren Sicherheitsrichtlinien und Compliance Zusagen. Mit diesem Feature Azure-Speicher automatisch verschlüsselt vor Speicher beibehalten und entschlüsselt vor abrufen. Verschlüsselung, Entschlüsselung und Schlüsselmanagement sind transparent für Benutzer.

Die folgenden Abschnitte enthalten ausführliche Anleitung Storage Service-Verschlüsselungsfunktionen sowie der unterstützten Szenarien und Benutzer Erfahrungen.

## <a name="overview"></a>Übersicht

Azure-Speicher bietet umfassende Sicherheitsfunktionen zusammen ermöglicht Entwicklern, sichere Anwendung zu erstellen. Daten können zwischen einer Anwendung und Azure mithilfe von [Clientseitigen Encryption](storage-client-side-encryption.md), HTTPs oder SMB 3.0 gesichert werden. Storage Service Encryption bietet Verschlüsselung ruhender, Behandlung Verschlüsselung, Entschlüsselung und Schlüsselmanagement vollständig transparente Weise. 256-Bit- [AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), eine der stärksten Block-Chiffren alle Daten verschlüsselt.

SSE Verschlüsseln von Daten in Azure-Speicher geschrieben und Block-Blobs Seitenblobs einsetzbar und Blobs angefügt wird. Es funktioniert für Folgendes:

-   Allgemeine Speicherkonten und Speicherkonten Blob
-   Standard und Premium-Speicher 
-   Redundanz aller Ebenen (LRS, ZRS, g, g RA)
-   Azure Ressourcenmanager Speicher Konten (aber nicht classic) 
-   Alle Regionen

Zum Aktivieren oder deaktivieren die Verschlüsselung Storage Service für ein Speicherkonto, [Azure-Portal](https://azure.portal.com) melden Sie an und wählen Sie ein Speicherkonto. Blatt Einstellungen Bereich Blob suchen Sie, wie in dieser Abbildung und Verschlüsselung auf.

![Verschlüsselungsoption Portal Screenshot anzeigen](./media/storage-service-encryption/image1.png)

Nach dem Klicken auf die Einstellung der Verschlüsselung können Sie aktivieren oder deaktivieren Storage Service-Verschlüsselung.

![Verschlüsselungseigenschaften Portal Screenshot anzeigen](./media/storage-service-encryption/image2.png)

##<a name="encryption-scenarios"></a>Szenarios für die Verschlüsselung

Storage Service-Verschlüsselung kann auf einer Speicherebene Konto aktiviert werden. Es unterstützt die folgenden Kundenszenarien:

-   Verschlüsselung des Blocks Blobs Blobs und Seitenblobs anfügen.

-   Verschlüsselung von archivierten VHDs und Vorlagen Azure lokal gebracht.

-   Verschlüsselung des zugrunde liegenden Betriebssystems und Datenträger für IaaS VMs erstellt mit der virtuellen Festplatten.

SSE hat die folgenden Nachteile:

-   Verschlüsselung von klassischen Speicherkonten wird nicht unterstützt.

-   Verschlüsselung von klassischen Ressourcenmanager Speicherkonten Gruppenkonten Speicher wird nicht unterstützt.

-   Vorhandene Daten - SSE verschlüsselt nur neu erstellte nach der Verschlüsselung. Wenn beispielsweise registrieren ein Ressourcenmanager speichern jedoch keine Verschlüsselung aktivieren und dann in diesem Storage-Konto Blobs oder archivierte VHDs hochladen und schalten SSE werden die Blobs geschrieben oder kopiert nicht verschlüsselt.

-   Marketplace-Support - Verschlüsselung aktivieren VMs von Marketplace mit [Azure-Portal](https://portal.azure.com), PowerShell und Azure CLI erstellt. Das Basisabbild VHD bleibt unverschlüsselt. Allerdings werden alle Schreibvorgänge ausgeführt, nachdem die VM erstellt wurde verschlüsselt.

-   Tabelle, Warteschlangen und Dateien Daten nicht verschlüsselt.

##<a name="getting-started"></a>Erste Schritte

###<a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Schritt 1: [Erstellen Sie ein neues Speicherkonto](storage-create-storage-account.md).

###<a name="step-2-enable-encryption"></a>Schritt 2: Aktivieren der Verschlüsselung.

Sie können mithilfe der [Azure-Portal](https://portal.azure.com)Verschlüsselung aktivieren.

> [AZURE.NOTE] Möchten Sie programmgesteuert aktivieren oder Deaktivieren der Verschlüsselung Storage Service ein Speicherkonto, können Sie [Azure Storage Resource Provider REST-API](https://msdn.microsoft.com/library/azure/mt163683.aspx), die [Storage Provider Client Ressourcenbibliothek für .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](../powershell-install-configure.md)oder [Azure CLI](storage-azure-cli.md).

###<a name="step-3-copy-data-to-storage-account"></a>Schritt 3: Daten Sie zum Konto

Wenn Sie ein Speicherkonto SSE aktivieren, Schreiben Blobs in diesem Storage-Konto werden die Blobs verschlüsselt. Blobs befindet sich bereits in diesem Storage-Konto werden nicht verschlüsselt werden, bis sie umgeschrieben. Sie können Daten von einem Konto mit SSE verschlüsselt, oder aktivieren Sie SSE und die Blobs aus einem Container in einen anderen kopieren sicher, dass Daten verschlüsselt werden. Dazu können Sie eines der folgenden Tools verwenden.

#### <a name="using-azcopy"></a>AzCopy verwenden

AzCopy ist ein Windows-Befehlszeilenprogramm zum Kopieren von Daten zu und von Microsoft Azure BLOB-Datei und Tabelle Speicher mit einfachen Befehlen mit optimaler Leistung entwickelt. Dadurch können Sie Ihre Blobs von einem Storage-Konto in ein anderes kopieren, die SSE aktiviert wurde. 

Weitere besuchen Sie [Datenübertragung mit AzCopy Befehlszeilenprogramm](storage-use-azcopy.md).

#### <a name="using-the-storage-client-libraries"></a>Speicher-Clientbibliotheken verwenden

Sie können BLOB-Daten und von BLOB-Speicher oder zwischen Speicherkonten mit unseren vielfältigen Speicher Clientbibliotheken einschließlich .NET, C++, Java, Android, Node.js, PHP, Python und Ruby.

Weitere Informationen besuchen Sie unsere [Erste Schritte mit Azure BLOB-Speicher mit .NET](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Mit dem Speicher-Explorer

Speicher-Explorer können Sie Storage-Konten erstellen, laden und Daten herunterladen Inhalt Blobs und Verzeichnisse navigieren. Eines dieser können Sie das Speicherkonto mit aktivierter Verschlüsselung Blobs hinzufügen. Mit einigen Speicher-Explorers können Sie auch Daten aus vorhandenen BLOB-Speicher in einen anderen Container in Speicher-Konto oder ein neues Speicherkonto, das SSE aktiviert wurde.

Besuchen Sie weitere [Azure Storage-Explorers](storage-explorers.md).

###<a name="step-4-query-the-status-of-the-encrypted-data"></a>Schritt 4: Abfragen des Status der verschlüsselten Daten

Eine aktualisierte Version des Speicher-Clientbibliotheken wurde bereitgestellt, mit dem Sie Abfragen den Zustand eines Objekts zu ermitteln, ob sie verschlüsselt ist. Beispiele werden in naher Zukunft zu diesem Dokument hinzugefügt.

In der Zwischenzeit können Sie [Eigenschaften zu erhalten](https://msdn.microsoft.com/library/azure/mt163553.aspx) , überprüfen Sie, ob das Speicherkonto Verschlüsselung oder Eigenschaften der Speicher in Azure-Portal aufrufen.

##<a name="encryption-and-decryption-workflow"></a>Verschlüsselung und Entschlüsselung Workflow

Hier wird eine kurze Beschreibung des Workflows Verschlüsselung/Entschlüsselung:

-   Der Kunde kann Verschlüsselung für das Speicherkonto.

-   Wenn der Kunde neue Daten (Blob setzen PUT Block Seite PLATZIEREN, etc.) auf BLOB-Speicher schreibt. Jeder Schreibvorgang wird mit 256-Bit [AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), eine der stärksten Block-Chiffren verschlüsselt.

-   Wenn der Kunde benötigt Zugriff auf Daten (Blob abrufen usw.), werden automatisch entschlüsselt vor der Rückgabe an den Benutzer.

-   Wenn die Verschlüsselung deaktiviert, neuen Writes nicht mehr verschlüsselt werden und vorhandene verschlüsselte Daten bleibt verschlüsselt vom Benutzer geschrieben. Während der Verschlüsselung aktiviert ist, werden BLOB-Speicher schreibt verschlüsselt. Der Zustand der Daten ändert sich nicht mit dem Benutzer zwischen Aktivieren/Deaktivieren der Verschlüsselung für das Speicherkonto.

-   Alle Schlüssel gespeichert verschlüsselt und von Microsoft verwaltet.

##<a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Häufig gestellte Fragen zu Storage Service-Verschlüsselung für ruhende Daten

**F: Ich habe ein klassische Speicherkonto. Kann ich auf SSE aktivieren?**

SSE ist Nein, nur auf Speicherkonten Ressourcen-Manager unterstützt.

**F: wie kann ich mein Konto klassische Speicher Daten verschlüsseln?**

A: Sie können ein neues Ressourcenmanager Storage-Konto erstellen und die Daten der neu erstellten Ressourcenmanager Speicherkonto [AzCopy](storage-use-azcopy.md) aus Ihrem classic Speicherkonto mit. 

Eine weitere Option ist das klassische Speicherkonto ein Speicherkonto Ressource verwalten migrieren. Weitere Informationen finden Sie unter [Plattform unterstützt der IaaS Migrationsressourcen von Classic an Ressourcen-Manager](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/).

**F: Ich habe ein Speicherkonto Ressourcenmanager. Kann ich auf SSE aktivieren?**

A: Ja, aber nur neu geschrieben Blobs werden verschlüsselt. Sie nicht zurückgehen und verschlüsselt Daten, die bereits vorhanden ist. 

**F: möchten die aktuellen Daten in eine vorhandene Ressourcen-Manager Speicherkonto verschlüsseln?**

A: Sie können SSE ein Ressourcenmanager Speicherkonto jederzeit aktivieren. Blobs, die bereits vorhanden waren werden jedoch nicht verschlüsselt. Um die Blobs zu verschlüsseln, können einen anderen Namen oder einen anderen Container kopieren und unverschlüsselten Versionen entfernen.

**F: Ich verwende Premium-Speicher; kann SSE verwenden?**

SSE ist Ja, Standardspeicher und Premium-Speicher unterstützt.

**F: Wenn ich ein neues Speicherkonto erstellen und SSE aktivieren, dann erstellen Sie einen neuen virtuellen Computer mit diesem Storage-Konto heißt meine VM verschlüsselt?**

A: Ja. Alle Datenträger erstellt, mit dem das neue Speicherkonto werden verschlüsselt, solange sie erstellt werden, nachdem SSE aktiviert ist. Wenn die VM in Azure Markt erstellt wurde, bleibt das Basisabbild VHD unverschlüsselt. Allerdings werden alle Schreibvorgänge ausgeführt, nachdem die VM erstellt wurde verschlüsselt.

**F: erstellen kann ich neue Speicherkonten mit SSE Azure PowerShell und aktiviert Azure CLI?**

A: Ja.

**F: wie viel kostet Azure Storage SSE aktiviert ist?**

A: Es ist kostenlos.

**F: Wer verwaltet die Verschlüsselungsschlüssel?**

A: die Schlüssel werden von Microsoft verwaltet.

**F: verwenden kann ich eigenen Verschlüsselungsschlüssel?**

A: Wir arbeiten Funktionen für Kunden ihre eigenen Verschlüsselungsschlüssel.

**F: werden kann Zugriff auf die Verschlüsselungsschlüssel widerrufen?**

A: derzeit nicht; die Schlüssel werden von Microsoft vollständig verwaltet.

**F: ist SSE standardmäßig beim Erstellen eines neuen Speicherkontos?**

A: SSE ist standardmäßig nicht aktiviert. Azure-Portal können Sie es aktivieren. Sie können dieses Feature mithilfe der Ressourcen-REST API-Anbieter auch programmgesteuert aktivieren.

**F: wie unterscheidet Azure Drive Encryption?**

A: Diese Funktion wird zum Verschlüsseln von Daten in Azure BLOB-Speicher. Azure Datenträgerverschlüsselung zum Verschlüsseln von Betriebssystem und Daten Datenträger IaaS VMs. Weitere Informationen finden Sie auf unseren [Speicher-Sicherheitshandbuch](storage-security-guide.md).

**F: Was geschieht, wenn ich aktivieren SSE, gehen und aktivieren Azure Datenträgerverschlüsselung auf dem Datenträger?**

A: Dies funktioniert problemlos. Durch beide Methoden werden die Daten verschlüsselt werden.

**F: Mein Speicherkonto soll Geo-redundant repliziert werden. Verschlüsselt Meine redundante Kopie werden ebenfalls, wenn SSE aktivieren?**

Ja, alle Kopien des Speicherkontos verschlüsselt und alle redundanzoptionen – lokal redundanter Speicher (LRS), Zone redundante Speicher (ZRS) Geo-Redundant Storage (GRS) und Lesezugriff Geo redundante Speicher (RA-GRS) – unterstützt.

**F: kann nicht auf Mein Speicherkonto Verschlüsselung aktivieren.**

A: ist es ein Speicherkonto Resource Manager? Klassische Speicherkonten werden nicht unterstützt. 

**F: darf SSE nur in bestimmten Regionen?**

A: die SSE ist in allen Regionen verfügbar. 

**F: kontaktiere jemand ich, wenn ich Fragen oder Feedback Geben?**

A: Wenden Sie sich an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) für alle Probleme im Zusammenhang mit Speicher-Service-Verschlüsselung.

##<a name="next-steps"></a>Nächste Schritte

Azure-Speicher bietet umfassende Sicherheitsfunktionen zusammen ermöglicht Entwicklern, sichere Anwendung zu erstellen. Weitere Informationen finden Sie auf [Storage Security Guide](storage-security-guide.md).
