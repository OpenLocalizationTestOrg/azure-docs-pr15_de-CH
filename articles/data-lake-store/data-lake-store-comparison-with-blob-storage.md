<properties
   pageTitle="Azure See Datenspeicher Vergleich mit Azure Storage Blob | Microsoft Azure"
   description="Vergleich mit Azure Storage Blob Azure See Datenspeicher"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/15/2016"
   ms.author="nitinme"/>

# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Vergleich von Azure See Datenspeicher und Azure BLOB-Speicher

Die Tabelle in diesem Artikel sind die Unterschiede zwischen Azure See Datenspeicher und Azure BLOB-Speicher auf einige wichtige Aspekte der großen Datenverarbeitung. Azure BLOB-Speicher ist eine allgemeine, skalierbarer Speicher, die für eine Vielzahl von speicherszenarien entwickelt. Azure See Datenspeicher ist ein Großkonzerne, die big Data Analytics Arbeitslasten optimiert ist.

|    | Azure See Datenspeicher  | Azure BLOB-Speicher  |
|----|-----------------------|--------------------|
| Zweck  | Optimierte Speicherung von big Data Analytics Arbeitslasten                                                                          | Allgemeine Objekt für eine Vielzahl von speicherszenarien speichern                                                                                |
| Anwendungsbeispiele                                   | Charge, interaktive, streaming Analytics und lerndaten wie Protokolldateien, IoT Daten, klicken Sie auf Streams große datasets | Jede Art von Texten oder binären Daten wie Ende Sicherungsdaten, Speichermedien für streaming und allgemeine Daten zurück |
| Grundbegriffe                                | Datenspeicher See Konto enthält Ordner enthält wiederum Daten als Dateien | Storage-Konto hat Container, das Daten in Form von blobs |
| Struktur | Hierarchisches Dateisystem                                                                                                    | Objektspeicher mit flachen namespace  |
| API   | REST-API über HTTPS | REST-API über HTTP/HTTPS                                                                                                                            |
| Serverseitige API                             | [WebHDFS-kompatible REST-API](https://msdn.microsoft.com/library/azure/mt693424.aspx)                                                                                                 | [Azure BLOB-Speicher REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx)                                                                                                                         |
| Hadoop Datei System Client                   | Ja                                                                                                                         | Ja                                                                                                                                                 |
| Datenoperationen - Authentifizierung            | Basierend auf [Azure Active Directory Identitäten](../active-directory/active-directory-authentication-scenarios.md) | Basierend auf gemeinsamen geheimen Schlüsseln - [Konto Zugriffstasten](../storage/storage-create-storage-account.md#manage-your-storage-account) und [freigegebene Access](../storage/storage-dotnet-shared-access-signature-part-1.md)-Signatur.                                                                       |
| Datenoperationen - Authentifizierungsprotokoll     | OAuth 2.0. Aufrufe müssen eine gültige JWT (JSON Web Token) ausgestellte Azure Active Directory enthalten.                     | Hash-Nachrichtenauthentifizierungscode (HMAC). Aufrufe müssen einen Hash SHA-256 Base64-codierte über einen Teil der HTTP-Anforderung enthalten. |
| Datenoperationen - Autorisierung               | POSIX-Zugriffssteuerungslisten (ACLs).  ACLs basierend auf Azure Active Directory Identitäten können auf Dateien und Ordner festlegen. | Ebene Autorisierung – [Konto Zugriffstasten](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>Konto, Container oder BLOB-Autorisierung - [Zugriff Signatur Schlüsseln](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Datenoperationen - Überwachung                    | Verfügbar. Finden Sie [hier](data-lake-store-diagnostic-logs.md) Weitere Informationen.                                                                                                                   | Verfügbar                                                                                                                                           |
| Ruhender Verschlüsselungsdaten                     | Transparente, serverseitige (in Kürze verfügbar)<ul><li>Mit managed service</li><li>Vom Kunden gemanagte Schlüssel in Azure-Schlüsseltresor</li></ul>| <ul><li>Transparente, serverseitige</li> <ul><li>Mit managed service</li><li>Vom Kunden gemanagte Schlüssel in Azure-Schlüsseltresor (in Kürze verfügbar)</li></ul><li>Clientseitige Verschlüsselung</li></ul> |
| Management-Operationen (z. B. Konto erstellen) | [Rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-what-is.md) (RBAC) vorgesehenen Azure für die Kontenverwaltung                                                                       | [Rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-what-is.md) (RBAC) vorgesehenen Azure für die Kontenverwaltung                                                                                               |
| Entwickler-SDKs                              | Java, .NET Node.js                                                                                                         | .NET Java, Python, Node.js, C++, Ruby                                                                                                              |
| Analytics-Arbeitslast              | Optimierte Performance für parallele Analytics Arbeitslasten. Hoher Durchsatz und IOPS.                                           | Nicht optimiert für Analytics Arbeitslasten                                                                                                               |
| Grenzwert                                 | Unbegrenzt Konto Größe, Dateigröße oder Anzahl der Dateien                                                                   | Bestimmte Grenzwerte dokumentierte [hier](../azure-subscription-service-limits.md#storage-limits)                                                                                                                     |
| Geo-Redundanz                              | Lokal redundant (mehrere Kopien von Daten in Azure Region)                                                             | Lokal redundant (LRS) Global redundante (GRS), Lesezugriff Global redundante (RA-GRS). Finden Sie [hier](../storage/storage-redundancy.md) Weitere Informationen |
| Dienststatus                               | Public Preview                                                                                                              | Allgemein verfügbar                                                                                                                                 |
| Regionaler Verfügbarkeit  | Finden Sie [hier](https://azure.microsoft.com/regions/#services)| Finden Sie [hier](https://azure.microsoft.com/regions/#services) |
| Preis                                       |     Siehe [Preise](https://azure.microsoft.com/pricing/details/data-lake-store/)| Siehe [Preise](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Azure See Datenspeicher](data-lake-store-overview.md)
- [Erste Schritte mit See Datenspeicher](data-lake-store-get-started-portal.md)



