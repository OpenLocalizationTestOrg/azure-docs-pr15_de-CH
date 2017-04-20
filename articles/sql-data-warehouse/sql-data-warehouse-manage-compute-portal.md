<properties
   pageTitle="Verwaltung Compute Azure SQL Data Warehouse (Azure Portal) | Microsoft Azure"
   description="Azure ausgeführte Aufgaben verwalten berechnen macht. Skalierung compute-Ressourcen durch DWUs anpassen. Anhalten oder Fortsetzen von computeressourcen, um Kosten zu sparen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Verwaltung Compute Azure SQL Data Warehouse (Azure Portal)

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-manage-compute-overview.md)
- [Portal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
- [REST](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Leistung durch dezentrales Skalieren berechnen Ressourcen und des Speichers ändern Ihre Arbeitslast gerecht. Kosten Sie durch Skalierung zurück Ressourcen Nebenzeiten oder Anhalten Compute insgesamt. 

Diese Auflistung von Tasks verwendet im Azure-Portal:

- Skalierung compute
- Pause compute
- Resume compute

Weitere Informationen finden Sie unter [Verwalten berechnen Overview][].

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Rechenleistung skalieren

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Serverressourcen zu ändern:

1. [Azure-Portal][]zu öffnen Sie, öffnen Sie Ihre Datenbank und **Skalierung**auf.

    ![Klicken Sie auf skalieren][1]

1. Blatt Skalierung den Schieberegler nach links oder rechts die DWU-Einstellung ändern.

    ![Schieberegler][2]

1. Klicken Sie auf **Speichern**. Eine Meldung wird angezeigt. Klicken Sie auf **Ja** zu bestätigen oder **nicht** Abbrechen.

    ![Klicken Sie auf Speichern][3]

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pause compute

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

So halten Sie eine Datenbank an

1. Öffnen Sie der [Azure-Portal][] und der Datenbank. Beachten Sie, dass der Status **Online**. 

    ![Onlinestatus][6]

1. Compute und Speicherressourcen unterbrechen, klicken Sie auf **Anhalten**und dann eine Meldung angezeigt wird. Klicken Sie auf **Ja** zu bestätigen oder **nicht** Abbrechen.

    ![Pause bestätigen][7]

1. Während SQL Data Warehouse die Datenbank gestartet wird, ist der Status **Anhalten**.
2. Der Status **angehalten**ist, das Anhalten ausgeführt wird und Sie sind nicht für DWUs belastet wird.

    ![Pause-status][4]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Resume compute

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]Fortsetzen eine Datenbank:

1. Öffnen Sie der [Azure-Portal][] und der Datenbank. Beachten Sie, dass der Status **angehalten**ist. 

    ![Pause-Datenbank][4]

1. Zum Fortsetzen die Datenbank klicken Sie auf **Start**und anschließend eine Meldung angezeigt. Klicken Sie auf **Ja** zu bestätigen oder **nicht** Abbrechen.

    ![Bestätigen fortsetzen][5]

1. Während SQL Data Warehouse die Datenbank gestartet wird, ist der Status "Fortsetzen".
2. Wenn der Status **online**ist, ist die Datenbank bereit.

    ![Onlinestatus][6]

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Überblick über][].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png
[4]: ./media/sql-data-warehouse-manage-compute-portal/resume-database.png
[5]: ./media/sql-data-warehouse-manage-compute-portal/resume-confirm.png
[6]: ./media/sql-data-warehouse-manage-compute-portal/pause-database.png
[7]: ./media/sql-data-warehouse-manage-compute-portal/pause-confirm.png

<!--Article references-->
[Überblick]: ./sql-data-warehouse-overview-manage.md
[Verwalten von Compute-Übersicht]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure-portal]: http://portal.azure.com/
