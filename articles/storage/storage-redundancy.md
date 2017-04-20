<properties
    pageTitle="Azure Speicherreplikation | Microsoft Azure"
    description="Daten in Ihrem Microsoft Azure Storage-Konto für Langlebigkeit und hohe Verfügbarkeit repliziert. Replikationsoptionen sind lokal redundanten Speicher (LRS), Zone redundanten Speicher (ZRS), Geo-redundanten Speicher (GRS) und Lesezugriff Geo redundanten Speicher (RA-GRS)."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="azure-storage-replication"></a>Azure Speicherreplikation

Die Daten in Ihrem Microsoft Azure Storage-Konto werden immer repliziert, Haltbarkeit und hohe Verfügbarkeit. Replikation kopiert die Daten im selben Rechenzentrum oder in einem zweiten Rechenzentrum je nach der Replikationsoption. Replikation schützt Ihre Daten und Ihre Anwendungszeit bei transienten Hardwarefehler behält. Wenn Ihre Daten in einem zweiten Rechenzentrum repliziert werden, schützt auch gegen ein schwerwiegender Fehler im primären Standort.

Replikation wird sichergestellt, dass das Speicherkonto [Service Level Agreement (SLA) für die Speicherung](https://azure.microsoft.com/support/legal/sla/storage/) trotz Fehlern entspricht. Finden Sie die SLA Informationen zum Azure-Speicher für Langlebigkeit und Verfügbarkeit gewährleistet. 

Wenn Sie ein Speicherkonto erstellen, können Sie eine der folgenden Optionen für die Replikation auswählen:  

- [Lokal redundanten Speicher (LRS)](#locally-redundant-storage)
- [Zone-redundanten Speicher (ZRS)](#zone-redundant-storage)
- [Geo-redundanten Speicher (GRS)](#geo-redundant-storage)
- [Lesezugriff Geo redundanten Speicher (RA-GRS)](#read-access-geo-redundant-storage)

Lesezugriff Geo redundanten Speicher (RA-GRS) ist die Standardoption, wenn Sie ein neues Speicherkonto erstellen.

Folgende Tabelle enthält einen Überblick über die Unterschiede zwischen LRS, ZRS, g und RA-GRS während Abschnitten einzelnen Replikationstypen ausführlicher behandeln.

| Strategie für die Replikation                                                               | LRS | ZRS | G | RA-GRS |
|:----------------------------------------------------------------------------------|:---|:---|:---|:------|
| Daten werden über mehrere Rechenzentren repliziert.                                     | Nein  | Ja | Ja | Ja    |
| Daten können zwischen dem sekundären und vom primären Standort gelesen werden. | Nein  | Nein  | Nein  | Ja    |
| Anzahl der Kopien der Daten auf separate Knoten verwaltet.                             | 3   | 3   | 6   | 6      |

Weitere [Azure Storage Preise](https://azure.microsoft.com/pricing/details/storage/) Preisinformationen für verschiedene redundanzoptionen.

>[AZURE.NOTE] Premium-Speicher unterstützt nur lokal redundanten Speicher (LRS). Weitere Informationen über Premium-Speicher [Storage Premium: Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](storage-premium-storage.md).

## <a name="locally-redundant-storage"></a>Lokal redundanter Speicher

Lokal redundanter Speicher (LRS) repliziert die Daten drei Mal in eine Skalierungseinheit Speicher, die in einem Datencenter im Bereich befindet, in der das Speicherkonto erstellt. Schreiben werden nur einmal in alle drei Replikationen geschrieben wurde erfolgreich zurückgegeben. Diese drei Kopien jedes befinden sich in separaten Fehler und Upgrade-Domänen innerhalb einer Skala Speicher. 

Eine Skalierungseinheit Speicher ist eine Sammlung von Racks Speicherknoten. Eine Fehlerdomäne (FD) ist eine Gruppe von Knoten, die eine physische Einheit darstellen und gelten als Knoten in einem physischen Rack. Eine Aktualisierungsdomäne (UD) ist eine Gruppe von Knoten, die bei der Service-Upgrades (Rollout) zusammen aktualisiert werden. Die drei Replikate werden verteilt UDs und FDs innerhalb einer Skala Aufbewahrung sicherzustellen, dass Daten ist verfügbar, auch wenn Hardwarefehler wirkt sich auf ein einzelnes Rack oder Knoten während einer Bereitstellung aktualisiert werden. 

LRS ist die kostengünstigste Option und bietet mindestens Haltbarkeit im Vergleich zu anderen Optionen. Katastrophenfall Datacenter Ebene (Feuer, Überschwemmung usw.) möglicherweise alle drei Replikationen verloren gehen oder nicht mehr wiederherstellbar. Um dieses Risiko zu verringern wird für die meisten Geo redundanten Speicher (GRS) empfohlen.

Lokal redundanter Speicher sollte in bestimmten Szenarien möglicherweise: 

- Bietet höchste maximale Bandbreite Replikationsoptionen Azure-Speicher.

- Die Anwendung speichert Daten problemlos wiederhergestellt werden können, können Sie für LRS entscheiden.

- Einige Programme sind auf Daten nur innerhalb eines Landes durch gesetzliche Auflagen Daten replizieren. Paarweise Region möglicherweise in einem anderen Land; Informationen auf Region finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/) .

## <a name="zone-redundant-storage"></a>Zone-redundanten Speicher

Zone-redundanten Speicher (ZRS) repliziert Daten asynchron für Rechenzentren in ein oder zwei Bereiche neben drei Replikationen wie LRS, wodurch höhere Haltbarkeit als LRS speichern. In ZRS gespeicherten Daten sind permanente ist das primäre Rechenzentrum nicht verfügbar oder nicht mehr wiederherstellbar.
Kunden, die ZRS verwenden sollten sich bewusst sein, dass: 

- ZRS ist nur für Block-Blobs im Allgemeinen Speicherkonten und ist nur in Storage Service Versionen 2014-02-14 oder höher unterstützt. 

- Da asynchroner Replikation eine Verzögerung beinhaltet, kann bei einem örtlichen Zwischenfall Änderungen, die noch nicht auf der sekundären repliziert wurde verloren vom primären Daten wiederhergestellt werden können.

- Das Replikat möglicherweise nicht verfügbar, bis Microsoft Failover auf den sekundären initiiert.

- ZRS-Konten können später LRS oder GRS konvertiert werden. Ebenso kann ein Konto LRS oder g ZRS Konto konvertiert werden.

- ZRS-Konten müssen nicht Metriken oder Protokollfunktion. 

## <a name="geo-redundant-storage"></a>Geo-redundanten Speicher

Geo-redundanten Speicher (GRS) repliziert Daten in eine sekundäre Region Hunderte von Meilen von der primären Region. Wenn das Speicherkonto GRS aktiviert ist, sind Ihre Daten dauerhaften auch bei vollständigen regionalen Ausfall oder eine in der primäre Region nicht wiederherstellbar ist.

Für ein Speicherkonto mit GRS aktiviert ein Update zunächst die primäre Region verpflichtet, diese dreimal repliziert wird. Update wird dann asynchron sekundäre Region repliziert, wo es auch dreimal repliziert wird. 

Mit GRS primären und sekundären Regionen Replikate auf separaten Fehlerdomänen verwalten und aktualisieren Domänen eine Skalierungseinheit Speicher als LRS beschrieben.

Hinweise:

- Da asynchroner Replikation eine Verzögerung beinhaltet, kann bei einer regionalen Katastrophe ändert die sekundäre Region noch nicht repliziert wurden verloren die Daten aus dem primären wiederhergestellt werden können.

- Das Replikat ist nicht verfügbar, wenn Microsoft Failover auf den sekundären Bereich initiiert.

- Wenn eine Anwendung aus dem sekundären lesen sollten Benutzer RA-GRS aktivieren. 

Wenn Sie ein Speicherkonto erstellen, wählen Sie die primäre Region für das Konto. Die sekundäre Region wird basierend auf der primären Region bestimmt und kann nicht geändert werden. Die folgende Tabelle zeigt die primären und sekundären Region Kombinationen.

| Primäre             | Sekundäre           |
|---------------------|---------------------|
| Norden der USA – zentral    | Südlichen zentralen USA    |
| Südlichen zentralen USA    | Norden der USA – zentral    |
| Osten der USA             | Westen der USA             |
| Westen der USA             | Osten der USA             |
| Osten der USA 2           | USA          |
| USA          | Osten der USA 2           |
| Nordeuropa        | Westeuropa         |
| Westeuropa         | Nordeuropa        |
| Südostasien     | Ostasien           |
| Ostasien           | Südostasien     |
| Ost-China          | Nordchina         |
| Nordchina         | Ost-China          |
| Japan OST          | Japan West          |
| Japan West          | Japan OST          |
| Brasilien Süd        | Südlichen zentralen USA    |
| Australien OST      | Australien Südost |
| Australien Südost | Australien OST      |
| Indien Süd         | Indien Central       |
| Indien Central       | Indien Süd         |
| US Gov Iowa         | US Gov Virginia     |
| US Gov Virginia     | US Gov Iowa         |
| Kanada zentral      | Kanada Osten          |
| Kanada Osten         | Kanada zentral      |
| Großbritannien West             | Großbritannien Süd            |
| Großbritannien Süd            | Großbritannien West             |
| Deutschland Central     | Deutschland Nordost   |
| Deutschland Nordost   | Deutschland Central     |
| Westen der USA 2           | Westen der USA – zentral     |
| Westen der USA – zentral     | Westen der USA 2           |

Aktuelle Informationen über Bereiche von Azure unterstützt anzeigen Sie [Azure Regionen](https://azure.microsoft.com/regions/)
 
## <a name="read-access-geo-redundant-storage"></a>Lesezugriff Geo redundante Speicherung

Lesezugriff Geo redundanten Speicher (RA-GRS) maximiert die Verfügbarkeit für das Speicherkonto schreibgeschützten Zugriff auf Daten in sekundären Standort neben Replikation über zwei Bereiche von g bereitgestellt. 

Beim Aktivieren von schreibgeschützten Zugriff auf die Daten in der sekundären steht Ihre Daten auf einem sekundären Endpunkt neben der primären für das Speicherkonto. Sekundäre Endpunkt das Suffix hängt jedoch ähnelt der primären `–secondary` dem Kontonamen. Beispielsweise der primären für den BLOB-Dienst wird `myaccount.blob.core.windows.net`, dann der zweite Endpunkt `myaccount-secondary.blob.core.windows.net`. Zugriffstasten für das Speicherkonto sind für die primären und sekundären Endpunkte.

Hinweise:

- Die Anwendung hat den Endpunkt verwalten sie interagiert mit RA-GRS Verwendung. 

- RA-GRS ist für hohe Verfügbarkeit vorgesehen. Skalierbarkeits-Leitfäden finden Sie [Performance-Checkliste](storage-performance-checklist.md).

## <a name="next-steps"></a>Nächste Schritte

- [Preise der Azure-Speicher](https://azure.microsoft.com/pricing/details/storage/)
- [Konteninformationen Azure-Speicher](storage-create-storage-account.md)
- [Erweiterbarkeit Azure-Speicher und Performance-Ziele](storage-scalability-targets.md)
- [Microsoft Azure Redundanz Speicheroptionen und Lesezugriff Geo redundante Speicherung](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
- [SOSP Papier - Azure-Speicher: Eine hochverfügbare Cloud-Speicherservice mit starker Konsistenz](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  
