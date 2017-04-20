<properties
   pageTitle="SQL Datenbank-Backups – automatische, georedundanten | Microsoft Azure" 
   description="SQL-Datenbank erstellt eine lokale Sicherung alle fünf Minuten automatisch und Azure Lesezugriff Geo redundanten Speicher (RA-GRS) Geo-Redundanz verwendet. "
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/20/2016"
   ms.author="carlrab;barbkess"/>

<!------------------
This topic is annotated with TEMPLATE guidelines for FEATURE TOPICS.


Metadata guidelines

pageTitle
    60 characters or less. Includes name of the feature - primary benefit. Not the same as H1. Its 60 characters or fewer including all characters between the quotes and the Microsoft Azure site identifier.

description
    115-145 characters. Duplicate of the first sentence in the introduction. This is the abstract of the article that displays under the title when searching in Bing or Google. 

    Example: "SQL Database automatically creates a local database backup every few minutes and uses Azure read-access geo-redundant storage for geo-redundancy."
------------------>

<!----------------

TEMPLATE GUIDELINES for feature topics

The Feature Topic is a one-pager (ok, sometimes longer) that explains a capability of the product or service. It explains what the capability is and characteristics of the capability.  

It is a "learning" topic, not an action topic.

DO explain this:
    • Definition of the feature terminology.  i.e., What is a database backup?
    • Characteristics and capabilities of the feature. (How the feature works)
    • Common uses with links to overview topics that recommend when to use the feature.
    • Reference specifications (Limitations and Restrictions, Permissions, General Remarks, etc.)
    • Next Steps with links to related overviews, features, and tasks.

DON'T explain this:
    • How to steps for using the feature (Tasks)
    • How to solve business problems that incorporate the feature (Overviews)
------------------->

<!------------------
GUIDELINES for the H1 
    
    The H1 should answer the question "What is in this topic?" Write the H1 heading in conversational language and use search key words as much as possible. Since this is a learning topic, make sure the title indicates that and doesn't mislead people to think this will tell them how to do tasks.  
    
    To help people understand this is a learning topic and not an action topic, start the title with "Learn about ... "

    Heading must use an industry standard term. If your feature is a proprietary name like "Elastic database pools", use a synonym. For example:    "Learn about elastic database pools for multi-tenant databases". In this case multi-tenant database is the industry-standard term that will be an anchor for finding the topic.

-------------------->

# <a name="learn-about-sql-database-backups"></a>Erfahren Sie mehr über SQL Datenbank-backups

<!------------------
    GUIDELINES for introduction
    
    The introduction is 1-2 sentences.  It is optimized for search and sets proper expectations about what to expect in the article. It should contain the top key words that you are using throughout the article.The introduction should be brief and to the point of what the feature is, what it is used for, and what's in the article. 

    If the introduction is short enough, your article can pop to the top in Google Instant Answers.

    In this example:
    
 

Sentence #1 Explains what the article will cover, which is what the feature is or does. This is also the metadata description. 
    SQL Database automatically creates a local database backup every five minutes and uses Azure read-access geo-redundant storage (RA-GRS) to provide geo-redundancy. 

Sentence #2 Explains why I should care about this.  
    Database backups are an essential part of any business continuity and disaster recovery strategy because they protect your data from accidental corruption or deletion.

-------------------->

SQL-Datenbank erstellt eine lokale Sicherung alle paar Minuten automatisch und Azure Lesezugriff Geo redundante Speicherung Geo-Redundanz verwendet. Datenbank-Backups sind Bestandteil jeder Business Continuity und Disaster Recovery-Strategie, da sie Ihre Daten vor versehentlichen Beschädigung oder Löschung schützen. 

<!-- This image needs work, so not putting it in right now.

This diagram shows SQL Database running in the US East region. It creates a database backup every five minutes, which it stores locally to Azure Read Access Geo-redundant Storage (RA-GRS). Azure uses geo-replication to copy the database backups to a paired data center in the US West region.

![geo-restore](./media/sql-database-geo-restore/geo-restore-1.png)

-->

<!---------------
GUIDELINES for the first ## H2.

    The first ## describes what the feature encompasses and how it is used. It points to related task articles.
    
    For consistency, being the heading with "What is ... "
----------------->

## <a name="what-is-a-sql-database-backup"></a>Was ist eine SQL-Datenbank-Sicherung?  

<!-- 
    Explains what a SQL Database backup is and answers an important question that people want to know.
-->

Eine SQL-Datenbank-Sicherung enthält sowohl lokale Datenbank und Geo redundante Sicherungen. Diese Backups werden automatisch und kostenlos erstellt. Sie müssen nichts Unternehmen, um geschehen.

<!----------------- 
    Explains first component of the backup feature
------------------>

Lokale Backups verwendet SQL Datenbank SQL Server-Technologie [vollständige](https://msdn.microsoft.com/library/ms186289.aspx), [differenzielle](https://msdn.microsoft.com/library/ms175526.aspx )und [Transaktionsprotokoll](https://msdn.microsoft.com/library/ms191429.aspx) -Backups erstellen. Sicherungskopien der Transaktionsprotokolle passieren alle fünf Minuten, wodurch Sie eine Point-in-Time-Wiederherstellung auf demselben Server, der die Datenbank hostet. Wenn Sie eine Datenbank wiederherstellen, ermittelt der Dienst vollständige, differenzielle und Transaktions Protokolldateien Backups wiederhergestellt werden müssen.

<!--------------- 
    Explicit list of what to do with a local backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Verwenden Sie eine lokale Sicherung auf:

- Wiederherstellen einer Datenbank auf eine Point-in-Time innerhalb der Aufbewahrungsdauer. Mit eine Sicherung können Sie Wiederherstellen einer Datenbank auf eine Point-in-Time, Mal gelöscht wurde eine gelöschte Datenbank wiederherstellen oder Wiederherstellen einer Datenbank an einem anderen geografischen Region. Zum Durchführen einer Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank von einer Sicherung](sql-database-recovery-using-backups.md).

- Kopieren einer Datenbank auf einem SQL-Server in derselben oder einer anderen Region. Die Kopie ist mit der aktuellen SQL-Datenbank transaktionell konsistent. So eine anzeigen Sie [Datenbankkopie](sql-database-copy.md)

- Archivieren Sie eine Sicherung über die Sicherung Aufbewahrungsdauer. Ein Archiv, exportieren [eine SQL-Datenbank eine BACPAC](sql-database-export.md) ausführen. Sie können dann BACPAC im Langzeitspeicher archiviert und über die Aufbewahrungsdauer. Oder verwenden Sie die BACPAC eine Kopie Ihrer Datenbank auf SQL Server entweder lokal oder in einer Azure Virtual Machine (VM) übertragen.

<!----------------- 
    Explains first component of the backup feature
------------------>

Geo redundanter Backups verwendet Datenbank SQL [Azure Storage Replication](../storage/storage-redundancy.md). SQL-Datenbank speichert lokale Datenbank-Sicherungsdateien in einem Konto [Lesezugriff Geo redundanten Speicher (RA-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) . Azure repliziert Sicherungsdateien [gepaarten Rechenzentrum](../best-practices-availability-paired-regions.md). 

<!--------------- 
    Explicit list of what to do with a geo-redundant backup. "Use a ..." helps people to scan the topic and find the uses quickly.
---------------->

Geo-redundantes Backup zu verwenden:

- Wiederherstellen einer Datenbank auf einer anderen geographischen Region bei die Sicherung aus der primären Datenbank zugreifen kann. 

>[AZURE.NOTE] In Azure-Speicher bezieht sich der Begriff *Replikation* zum Kopieren von Dateien von einem Speicherort zu einem anderen. SQL *Replikation* bezieht sich auf sekundäre Datenbanken mit einer primären Datenbank synchronisiert. 

<!----------------
    The next ## H2's discuss key characteristics of how the feature works. The title is in conversational language and asks the question that will be answered.
------------------->
## <a name="how-much-backup-storage-is-included-at-no-cost"></a>Wie viele backup-Speicher ist kostenlos enthalten?

SQL-Datenbank bietet bis zu 200 % der maximalen bereitgestellte Datenbankspeicher als backup-Speicher ohne zusätzliche Kosten. Beispielsweise haben Sie einen Standard Datenbankinstanz bereitgestellte DB-Größe von 250 GB haben Sie 500 GB backup-Speicher kostenlos. Wenn Ihre Datenbank bereitgestellten backup-Speicher überschreitet, können Sie die Aufbewahrungsdauer von Azure Kundendienst reduzieren. Eine weitere Option ist für zusätzliche backup-Speicher bezahlen, die Lesezugriff geografisch redundanten Speicher (RA-GRS) Standardsatz berechnet. 

## <a name="how-often-do-backups-happen"></a>Wie oft geschehen Backups?

Lokale Datenbank-Backups haben vollständige Backups wöchentlich sichern differenzieller Datenbanken auftreten, stündlich und Transaktionsprotokolldateien Sicherungskopien fünf Minuten auftreten. Die erste vollständige Sicherung soll unmittelbar nach dem Erstellen eine Datenbank. In der Regel innerhalb von 30 Minuten abgeschlossen ist, aber kann länger dauern, wenn die Datenbank von großem Umfang. Beispielsweise kann die anfängliche Sicherung wiederhergestellte Datenbank oder eine Datenbank länger dauern. Nach der ersten vollständigen Sicherung werden alle weiteren Backups automatisch geplant und automatisch im Hintergrund verwaltet. Das genaue Timing vollständige und [differenzielle](https://msdn.microsoft.com/library/ms175526.aspx) Backups wird bestimmt, wie die gesamte Arbeitslast verteilt. 

Geo-redundante Backups werden vollständige und differenzielle Backups nach Replikationszeitplan Azure-Speicher kopiert.

## <a name="how-long-do-you-keep-my-backups"></a>Wie lange halten Sie meine Backups?

Jede SQL-Sicherung hat einen Aufbewahrungszeitraum [Service-Tier](sql-database-service-tiers.md) der Datenbank basiert. Die Aufbewahrungsdauer für eine Datenbank in der:

<!------------------

    Using a list so the information is easy to find when scanning.
------------------->

- Standard-Ebene ist.
- Dienstebene Standard beträgt 35 Tage.
- Premium-Service-Tier ist 35 Tagen.


Downgrade die Datenbank Dienstebenen Standard oder Premium Basisdatenträger werden Sicherungskopien die sieben Tage lang gespeichert. Alle vorhandene Sicherungskopien älter als sieben Tage sind nicht mehr verfügbar. 

Wenn Sie die Datenbank von der Standard-Ebene Standard oder Premium aktualisieren, behält SQL Datenbank Sicherungskopien bis 35 Tage alt sind. Neue Backups hält 35 Tage an.
 
Wenn Sie eine Datenbank löschen, behält SQL-Datenbank die Backups in wie bei einer online-Datenbank. Angenommen Sie, eine Datenbank löschen, die über einen Zeitraum von sieben Tagen. Eine Sicherung, die vier Tage werden für drei Tage.

>[AZURE.IMPORTANT]
    SQL Azure-Server hostet SQL-Datenbanken löschen alle Datenbanken auf dem Server gehören ebenfalls gelöscht und können nicht wiederhergestellt werden. Sie können keinen gelöschten Server wiederherstellen.

<!-------------------
OPTIONAL section
## Best practices 
--------------------->

<!-------------------
OPTIONAL section
## General remarks
--------------------->

<!-------------------
OPTIONAL section
## Limitations and restrictions
--------------------->

<!-------------------
OPTIONAL section
## Metadata
--------------------->

<!-------------------
OPTIONAL section
## Performance
--------------------->

<!-------------------
OPTIONAL section
## Permissions
--------------------->

<!-------------------
OPTIONAL section
## Security
--------------------->

<!-------------------
GUIDELINES for Next Steps

    The last section is Next Steps. Give a next step that would be relevant to the customer after they have learned about the feature and the tasks associated with it.  Perhaps point them to one or two key scenarios that use this feature.

    You don't need to repeat links you have already given them.
--------------------->

## <a name="next-steps"></a>Nächste Schritte

Datenbank-Backups sind Bestandteil jeder Business Continuity und Disaster Recovery-Strategie, da sie Ihre Daten vor versehentlichen Beschädigung oder Löschung schützen. Sehen wie das Sichern von Datenbanken in einer umfassenderen Strategie finden Sie unter [Übersicht über Business Continuity](sql-database-business-continuity.md).


