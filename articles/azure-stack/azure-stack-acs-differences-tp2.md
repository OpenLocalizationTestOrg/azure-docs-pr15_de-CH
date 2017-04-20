
<properties
    pageTitle="Konsistente Azure Storage: Unterschiede und Hinweise | Microsoft Azure"
    description="Verstehen Sie die Unterschiede zwischen Azure-Speicher und andere Azure einheitliche Speicher Implementierung."
    services="azure-stack"
    documentationCenter=""
    authors="MChadalapaka"
    manager="siroy"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/26/2016"
    ms.author="mchad"/>

# <a name="azure-consistent-storage-differences-and-considerations"></a>Konsistente Azure Storage: Unterschiede und Aspekte

Azure einheitliche Speicher ist die Menge von Clouddiensten Microsoft Azure Stapel Speicher. Konsistente Azure Storage bietet Blob, Tabelle, Warteschlange und Account-Management-Funktionen mit Azure einheitlichen Semantik. Dieser Artikel beschreibt bekannte Azure konsistent Speicherunterschiede mit Azure-Speicher. Auch zusammengefasst andere Faktoren bedenken, die derzeit öffentlich Vorabversion von Microsoft Azure Stapel Bereitstellung.

<span id="Concepts" class="anchor"><span id="_Toc386544169" class="anchor"><span id="_Toc389466742" class="anchor"><span id="_Ref428966996" class="anchor"><span id="_Toc433223853" class="anchor"></span></span></span></span></span>
## <a name="known-differences"></a>Bekannte Unterschiede

Diese technische Vorabversion von Azure einheitliche Speicher muss nicht 100 % Featureparität mit Azure Storage API-Versionen, die unterstützt werden. Funktionsunterschiede Folgendes bekannt:

-   Bestimmte Konto Speicher noch nicht verfügbar, z. B. Standard\_RAGRS und Standard\_g.

-   Premium\_LRS Speicherkonten bereitgestellt werden. Es gibt jedoch zurzeit Leistungsgrenzwerte keine Garantien.

-   Azure Dateien Funktionalität ist noch nicht verfügbar.

-   Die erhalten Seite reicht-API unterstützt das Abrufen von Seiten nicht, die Snapshots Seite BLOBs unterscheiden.

-   Get Seite reicht API gibt Seiten mit 4 KB Granularität zurück.

-   Partition und Schlüssel in der Azure-Speicher Tabelle konsequent Zeile sind auf 400 Zeichen (d. h. 800 Byte).

-   BLOB-Namen in der Azure-Speicher Blob Service konsequent sind 880 Zeichen (d. h. 1760 Bytes) auf.

-   Der Azure-Speicher konsequent Mieter Speicherdaten reporting Speicher Verwendung Meter bereitstellt, mit denen in Azure mit derselben Semantik und Einheiten sind. Derzeit jedoch Speichertransaktionen Verwendung Anzeige umfasst nicht die IaaS-Transaktionen und Datenübertragung Verwendung Meter unterscheidet die Bandbreite von internen oder externen Netzwerkverkehr auf einen Bereich Azure Stapel.

-   Bestimmte Unterschiede im Bereich der Funktionalität für Speicher-Management. Sie können nicht z. B. Kontotyp ändern oder benutzerdefinierte Domänen. Darüber hinaus bietet nur API-Ebene Konsistenz Prämie\_LRS Speicher-Kontotyp.

## <a name="deployment-considerations"></a>Aspekte der Bereitstellung

-   **Test.** Konsistente Azure Storage produktionsumgebungen, mit denen die aktuelle Microsoft Azure Stapel Technical Preview-Version nicht bereitgestellt. Diese Version dient nur zu Evaluierungszwecken in einem Test Lab-Umgebung.

-   **Leistung**. Microsoft Azure Stapel Technical Preview-Version Azure einheitliche Speicher ist nicht Performance-optimierten.

-   **Skalierbar.** Microsoft Azure Stapel Technical Preview-Version Azure einheitliche Speicher wird für Skalierbarkeit optimiert.

## <a name="version-support-considerations"></a>Version Unterstützung Aspekte

Die folgenden Versionen werden von dieser Vorabversion von Azure einheitliche Speicher unterstützt:

> Azure-Speicher-Clientbibliothek: [Microsoft Azure Storage 6.x .NET SDK](http://www.nuget.org/packages/WindowsAzure.Storage/6.2.0)
>
> Azure Storage Data Services: [die REST API Version 2015-04-05](https://msdn.microsoft.com/library/azure/mt705637.aspx)
>
> Azure Storage Management Services: [2015-05-01-Vorschau](https://msdn.microsoft.com/library/azure/mt163683.aspx)
> [2015-06-15](https://msdn.microsoft.com/library/azure/mt163683.aspx)
## <a name="next-steps"></a>Nächste Schritte

-   [Einführung in Azure einheitliche Speicher](azure-stack-storage-overview.md)
