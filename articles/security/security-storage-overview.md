<properties
   pageTitle="Übersicht über die Sicherheit von Azure-Speicher | Microsoft Azure"
   description=" Azure-Speicher ist der Cloud-Storage-Lösung für moderne, die auf Dauerhaftigkeit, Verfügbarkeit und Skalierbarkeit für die Bedürfnisse ihrer Kunden. Dieser Artikel enthält einen Überblick über die wichtigsten Azure Sicherheitsfunktionen mit Azure Storage. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/16/2016"
   ms.author="terrylan"/>

# <a name="azure-storage-security-overview"></a>Übersicht über die Sicherheit von Azure-Speicher

Azure-Speicher ist der Cloud-Storage-Lösung für moderne, die auf Dauerhaftigkeit, Verfügbarkeit und Skalierbarkeit für die Bedürfnisse ihrer Kunden. Azure-Speicher bietet umfassende Sicherheitsfunktionen:

- Das Speicherkonto kann über rollenbasierte Zugriffskontrolle und Azure Active Directory gesichert werden.
- Daten können zwischen einer Anwendung und Azure mithilfe von clientseitigen Encryption, HTTPS oder SMB 3.0 gesichert werden.
- Daten können festgelegt werden automatisch verschlüsselt werden, beim Schreiben in Azure Storage Storage Service Verschlüsselung.
- Betriebssystem und Daten von virtuellen Computern verwendete Datenträger lassen Azure Datenträger Verschlüsselung verschlüsselt werden.
- Delegierter Zugriff auf die Datenobjekte in Azure Storage kann mit Shared Access Signatures erteilt werden.
- Die von einem Benutzer beim Zugriff auf Speicher verwendete Authentifizierungsmethode kann mit Speicheranalyse überwacht werden.

Eine ausführlichere Betrachtung Sicherheit in Azure-Speicher finden Sie im [Sicherheitshandbuch Azure Storage](../storage/storage-security-guide.md). Dieses Handbuch bietet einen tiefen Einblick in die Sicherheitsfunktionen der Azure-Speicher wie Konto Speicherschlüssel Verschlüsselung übertragen und Rest und Speicheranalyse.

Dieser Artikel bietet eine Übersicht über Azure Sicherheitsfunktionen mit Azure Storage verwendet werden kann. Links erhalten zu Artikeln, die alle Details der Features geben Sie informieren.

Hier sind die Kernfunktionen in diesem Artikel behandelt:

- Rollenbasierte Zugriffskontrolle
- Delegierten Zugriff auf Speicherobjekte
- Verschlüsselung in transit
- Verschlüsselung auf Rest/Service-Verschlüsselung
- Verschlüsselung Azure Festplatten
- Azure-Tresor

## <a name="role-based-access-control-rbac"></a>Rollenbasierte Zugriffskontrolle (RBAC)

Sie können das Speicherkonto mit Role-Based Access Control (RBAC) sichern. Einschränken des Zugriffs auf Grundlage der Sicherheitsprinzipien [benötigen](https://en.wikipedia.org/wiki/Need_to_know) und [geringsten](https://en.wikipedia.org/wiki/Principle_of_least_privilege) ist für Organisationen, die Sicherheitsrichtlinien für den Datenzugriff erzwingen möchten. Diese Zugriffsrechte erteilt Gruppen und Programme in einem bestimmten Bereich die entsprechende RBAC-Rolle zuweisen. [Integrierte RBAC-Rollen](../active-directory/role-based-access-built-in-roles.md)wie Storage-Konto Contributor können Sie Benutzern Berechtigungen zuweisen.

Weitere Informationen:

- [Azure Active Directory rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-configure.md)

## <a name="delegated-access-to-storage-objects"></a>Delegierten Zugriff auf Speicherobjekte

SAS (SAS) bietet delegierten Zugriff auf Ressourcen in Ihrem Speicherkonto. Die SAS bedeutet, dass ein Client eingeschränkte Berechtigungen für Objekte in Ihrem Speicherkonto für einen bestimmten Zeitraum mit einer bestimmten Gruppe von Berechtigungen erteilen können. Sie können diese beschränkten Berechtigungen erteilen, ohne Ihr Konto Zugriffstasten freigeben. SAS ist ein URI, der die Abfrageparameter für den authentifizierten Zugriff auf eine Speicherressource erforderlichen Informationen umfasst. Zugriff auf Speicherressourcen mit SAS muss der Client nur SAS an den entsprechenden Konstruktor oder die Methode übergeben.

Weitere Informationen:

- [Grundlegendes zu SAS-Modell](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Erstellen und Verwenden von SAS mit BLOB-Speicher](../storage/storage-dotnet-shared-access-signature-part-2.md)

## <a name="encryption-in-transit"></a>Verschlüsselung in transit
Verschlüsselung in Transit ist ein Mechanismus zum Schutz der Daten bei der Übertragung über Netzwerke. Mit Azure Storage können Sie die Daten sichern:

- [Verschlüsselung auf Transportebene](../storage/storage-security-guide.md#encryption-in-transit)wie HTTPS bei der Übertragung von Daten in oder aus dem Azure-Speicher.
- [Verschlüsselung](../storage/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), wie SMB 3.0-Verschlüsselung für Azure Dateifreigaben.
- [Clientseitige Verschlüsselung](../storage/storage-security-guide.md#using-client-side-encryption-to-secure-data-that-you-send-to-storage)zum Verschlüsseln der Daten vor der Übertragung in den Speicher und zum Entschlüsseln der Daten nach der Übertragung aus.

Weitere Informationen zu clientseitigen Verschlüsselung:

- [Clientseitige Verschlüsselung für Microsoft Azure-Speicher](https://blogs.msdn.microsoft.com/windowsazurestorage/2015/04/28/client-side-encryption-for-microsoft-azure-storage-preview/)
- [Cloud-Sicherheit steuert Serie: Verschlüsseln von Daten während der Übertragung](http://blogs.microsoft.com/cybertrust/2015/08/10/cloud-security-controls-series-encrypting-data-in-transit/)

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender

Für viele Unternehmen ist [Verschlüsselung ruhender](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) ein obligatorischer Schritt Datenschutz, Compliance und datenhoheit. Es gibt drei Azure-Funktionen, die Daten bereitstellen, die "at Rest":

- [Storage Service-Verschlüsselung](../storage/storage-security-guide.md#encryption-at-rest) können Sie anfordern, dass der Speicherdienst beim Schreiben in Azure-Speicher automatisch Daten verschlüsseln.
- [Clientseitige Verschlüsselung](../storage/storage-security-guide.md#client-side-encryption) bietet auch die Funktion der Verschlüsselung ruhender.
- [Verschlüsselung der Azure-Festplatten](../storage/storage-security-guide.md#using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) können Sie OS Festplatten und Datenträger von einer virtuellen Maschine IaaS verwendet verschlüsseln.

Weitere Informationen zu Speicher-Service-Verschlüsselung:

- [Verschlüsselung von Azure Storage Service](https://azure.microsoft.com/services/storage/) steht für [Azure BLOB-Speicher](https://azure.microsoft.com/services/storage/blobs/). Details zu anderen Typen Azure-Speicher finden Sie unter [Datei](https://azure.microsoft.com/services/storage/files/), [Datenträger (Premium-Speicher)](https://azure.microsoft.com/services/storage/premium-storage/), [Tabelle](https://azure.microsoft.com/services/storage/tables/)und [Warteschlange](https://azure.microsoft.com/services/storage/queues/).
- [Azure Storage Service Verschlüsselung für ruhende Daten](../storage/storage-service-encryption.md)

## <a name="azure-disk-encryption"></a>Verschlüsselung Azure Festplatten

Azure Verschlüsselung virtuelle Maschinen (VMs) können organisatorische Sicherheit und Vorschriften die VM Datenträger (einschließlich Start- und Festplatten) Richtlinien steuern in [Azure Schlüsseltresor](https://azure.microsoft.com/services/key-vault/)mit Schlüssel verschlüsseln.

Verschlüsselung VMs funktioniert für Betriebssysteme Linux und Windows. Es werden auch Schlüsseltresor sichern, verwalten und Überwachen der Verschlüsselungsschlüssel Datenträger verwenden. Alle Daten in der VM-Datenträger ist statisch mit Industriestandard-Verschlüsselung in Azure-Speicherkonten verschlüsselt. Datenträger-Verschlüsselungssystem für Windows basiert auf [Microsoft BitLocker Drive Encryption](https://technet.microsoft.com/library/cc732774.aspx)und Linux-Lösung basiert auf [dm-Crypt](https://en.wikipedia.org/wiki/Dm-crypt).

Weitere Informationen:

- [Azure Verschlüsselung Windows und Linux IaaS virtuelle Computer](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)

## <a name="azure-key-vault"></a>Azure-Tresor

Azure verschlüsselt Datenträger [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) steuern und verwalten Datenträger Schlüssel und geheime Informationen in Ihrem Abonnement Key Vault dabei sicherzustellen, dass alle virtuellen Laufwerke ruhende in Azure-Speicher verschlüsselt werden. Verwenden Sie Schlüssel Depot Schlüssel und Richtlinienverwendung.

Weitere Informationen:

- [Was ist Azure Key Vault?](../key-vault/key-vault-whatis.md)
- [Erste Schritte mit Azure Schlüssel](../key-vault/key-vault-get-started.md)
