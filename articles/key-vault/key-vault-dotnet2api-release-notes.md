<properties
   pageTitle="Wichtige Versionsinformationen Depot .NET 2.x-API | Microsoft Azure"
   description=".NET Entwickler verwenden diese API Code Azure Key Vault"
   services="key-vault"
   documentationCenter=""
   authors="BrucePerlerMS"
   manager="mbaldwin"
   editor="bruceper" />
<tags
   ms.service="key-vault"
   ms.devlang="CSharp"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/07/2016"
   ms.author="bruceper" />

# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure Key Vault .NET 2.0 - Versionshinweise und Migrationshandbuch

Die folgenden Hinweise und Anleitung für Entwickler mit der Azure Key Vault werden / C#-Bibliothek. Die Änderung von Version 1.0 auf Version 2.0 eine Reihe von Updates wurden Migration Arbeit in Ihrem Code zu funktionalen Verbesserungen profitieren und Zusätze wie Schlüssel Depot Zertifikate verfügen wird.

Key Vault Zertifikate unterstützt für das Management der X509 Zertifikate und das folgenden Verhalten:  

-   Zertifikatsinhabers ein Zertifikat über eine Taste Vault-Erstellungsprozess oder Importieren eines vorhandenen Zertifikats erstellen können. Dazu gehören selbstsigniert und Zertifizierungsstelle Zertifikate generiert.

- Ein Schlüssel Depot Zertifikatsinhabers sichere Speicherung und Verwaltung von X509 können Zertifikate ohne Interaktion mit privaten Schlüsseln.  

-   Ermöglicht einen Zertifikat Besitzer eine Richtlinie erstellen, die Key Vault zum Verwalten des Lebenszyklus eines Zertifikats weist.  

-   Inhabern Kontaktinformationen für Benachrichtigung Lebenszyklusereignisse Ablauf und Erneuerung des Zertifikats bereitstellen können.  

-   Unterstützt automatische Erneuerung mit ausgewählten Emittenten - Schlüssel Depot Partner X509 Zertifikat Anbieter / Zertifizierungsstellen.
    - Hinweis: nicht Partnerschaft Anbieter-Behörden dürfen jedoch unterstützt nicht die automatische Erneuerung Funktion.


## <a name="net-support"></a>.NET unterstützt
- **.NET 4.0** nicht von Azure Key Vault .NET Version 2.0 unterstützt / C#-Bibliothek
- **.NET Core** von Azure Key Vault .NET Version 2.0 unterstützt / C#-Bibliothek

## <a name="namespaces"></a>Namespaces
- Der Namespace für **Modelle** wird von **Microsoft.Azure.KeyVault** in **Microsoft.Azure.KeyVault.Models**geändert.
- Der **Microsoft.Azure.KeyVault.Internal** -Namespace wird gelöscht.
- Azure SDK Abhängigkeiten Namespace **Hyak.Common** und **Hyak.Common.Internals** **Microsoft.Rest** und **Microsoft.Rest.Serialization** geändert werden


## <a name="type-changes"></a>Ändert
- *Schlüssel* in *SecretBundle* geändert
- *Wörterbuch* *IDictionary* geändert
- *Liste<T>, string []* geändert *IList<T> *
- *NextList* in *NextPageLink* geändert


## <a name="return-types"></a>Rückgabetypen
- **KeyList** und **SecretList** liefert *IPage<T> * statt *ListKeysResponseMessage*
- Generierte **BackupKeyAsync** liefert *BackupKeyResult* *Wert* (Backup-Blob) enthält. Bevor die Methode umschlossen wurde und der Wert zurückgegeben.

## <a name="exceptions"></a>Ausnahmen
- *KeyVaultClientException* wird in *KeyVaultErrorException* geändert.
- -Fehler ist *Ausnahme geändert. Fehler beim* *Ausnahme. Body.Error.Message*.
- Zusätzliche Informationen entfernt über die Fehlermeldung **[JsonExtensionData]**.

## <a name="constructors"></a>Konstruktoren
- Statt eine *HttpClient* als Konstruktorargument, akzeptiert der Konstruktor nur *HttpClientHandler* oder *DelegatingHandler []*.



## <a name="downloaded-packages"></a>Heruntergeladene Pakete  
Wenn ein Client eine Abhängigkeit auf Schlüssel verarbeitet wurden folgende heruntergeladen.
#### <a name="previous-package-list"></a>Vorherige Paketliste
- id="Hyak.Common-Paket" Version = "1.0.2" TargetFramework = "net45"
- id="Microsoft.Azure.Common-Paket" Version = "2.0.4" TargetFramework = "net45"
- id="Microsoft.Azure.Common.Dependencies-Paket" Version = "1.0.0" TargetFramework = "net45"
- id="Microsoft.Azure.KeyVault-Paket" Version = "1.0.0" TargetFramework = "net45"
- id="Microsoft.Bcl-Paket" Version = "1.1.9" TargetFramework = "net45"
- id="Microsoft.Bcl.Async-Paket" Version = "1.0.168" TargetFramework = "net45"
- id="Microsoft.Bcl.Build-Paket" Version = "1.0.14" TargetFramework = "net45"
- id="Microsoft.Net.Http-Paket" Version = "2.2.22" TargetFramework = "net45"

#### <a name="current-package-list"></a>Aktuelle Paketliste
- id="Microsoft.Azure.KeyVault-Paket" Version = "2.0.0-preview" TargetFramework = "net45"
- id="Microsoft.Rest.ClientRuntime-Paket" Version = "2.2.0" TargetFramework = "net45"
- id="Microsoft.Rest.ClientRuntime.Azure-Paket" Version = "3.2.0" TargetFramework = "net45"


## <a name="class-changes"></a>Klasse ändern

- **UnixEpoch** -Klasse wurde entfernt
- **Base64UrlConverter** -Klasse wird in **Base64UrlJsonConverter** umbenannt.

## <a name="other-changes"></a>Sonstige

- Unterstützung für die Konfiguration des KV wiederholen Politik auf vorübergehende Fehler wurde in dieser Version der API hinzugefügt.



## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet
- Für Vorgänge, die ein *Depot*zurückgegeben wurde der Rückgabetyp einer Klasse, die eine Vault-Eigenschaft enthalten. Der Rückgabetyp ist jetzt *Depot*.
- *PermissionsToKeys* und *PermissionsToSecrets* sind jetzt *Permissions.Keys* und *Permissions.Secrets*
- Rückgabetypen Änderungen gelten für den Steuerelement-Ebene.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet
- Das Paket ist für die kryptografischen Operationen **Microsoft.Azure.KeyVault.Extensions** und **Microsoft.Azure.KeyVault.Cryptography** aufgeteilt.
