<properties
    pageTitle="Replikation der globalen DocumentDB | Microsoft Azure"
    description="Informationen Sie zum globalen Replikation Ihres DocumentDB-Kontos Azure Portal."
    services="documentdb"
    keywords="globale Datenbank, Replikation"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>DocumentDB globale Datenbank-Replikation mithilfe von Azure-Portal durchführen

Informationen Sie zum Replizieren von Daten in mehreren Regionen für die globale Verfügbarkeit von Daten in Azure DocumentDB Azure-Portal verwenden.

Informationen zur globalen Datenbank Replikation in DocumentDB, finden Sie unter [Verteilen von Daten mit DocumentDB](documentdb-distribute-data-globally.md). Informationen zum globalen Datenbankreplikation programmgesteuert durchführen finden Sie unter [Entwickeln mit mit mehreren DocumentDB](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Globales DocumentDB Datenbanken ist allgemein verfügbar und für alle neu erstellten DocumentDB-Konten automatisch aktiviert. Wir arbeiten daran, globale Verteilerlisten für alle vorhandenen Konten aktivieren, aber in der Zwischenzeit sollten globale Verteilerlisten für Ihr Konto aktiviert [wenden](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) , und wir werden aktivieren sie Sie jetzt.

## <a id="addregion"></a>Globale Datenbank Regionen

DocumentDB ist in den meisten [Regionen Azure] [azureregions]. Nachdem Sie Konsistenz Standardstufe für Ihr Konto haben, können Sie eine oder mehrere Regionen (je nach Standard Konsistenz Ebene und globale Verteilerlisten muss) zuordnen.

1. Klicken Sie in der [Azure-Portal](https://portal.azure.com/), in dem Indexleiste **DocumentDB Konten**.
2. Wählen Sie Blatt **DocumentDB Konto** das Konto ändern.
3. Blatt Konto **Global repliziert Daten** klicken Sie im auf.
4. Blatt **Datenreplikation Global** wählen Sie Bereiche hinzufügen oder Entfernen aus und dann auf **Speichern**. Kosten Hinzufügen von Regionen, siehe [Preisseite](https://azure.microsoft.com/pricing/details/documentdb/) oder [verteilen Daten mit DocumentDB](documentdb-distribute-data-globally.md) Artikel Weitere Informationen.

    ![Klicken Sie auf die Bereiche der Karte hinzufügen oder entfernen][1]

### <a name="selecting-global-database-regions"></a>Globale Datenbank Bereiche auswählen

Bei der Konfiguration von zwei oder mehr Bereiche wird empfohlen, Regionen basierend auf der Region gemäß ausgewählt werden der [Business Continuity und Disaster Recovery (BCDR): Azure kombiniert Bereiche]  [ bcdr] Artikel.

Achten Sie insbesondere mehrere Bereiche konfigurieren, gepaarten Region Spalten die gleiche Anzahl von Regionen (+/-1 für gerade/ungerade) aus. Beispielsweise wenn Sie vier US-Regionen bereitstellen möchten, wählen Sie zwei US-Bereiche von links und von rechts. Daher wäre die folgenden geeigneten: Westen der USA, USA Osten US North Central und südlichen zentralen USA.

Diese Anleitung ist wichtig, wenn nur zwei Bereiche für Disaster Recovery-Szenarien konfiguriert werden. Für mehr als zwei Bereiche nach dieser Anleitung empfiehlt, aber nicht als ausgewählte Regionen diese Verbindung entsprechen.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Nächste Schritte

Erfahren Sie, wie die Konsistenz des Kontos Global replizierten lesen [konsistenzebenen DocumentDB](documentdb-consistency-levels.md)verwalten.

Informationen zur globalen Datenbank Replikation in DocumentDB, finden Sie unter [Verteilen von Daten mit DocumentDB](documentdb-distribute-data-globally.md). Informationen zur Replikation von Daten in mehreren Regionen programmgesteuert finden Sie unter [Entwickeln mit mit mehreren DocumentDB](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
