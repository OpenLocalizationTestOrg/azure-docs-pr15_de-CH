<properties
    pageTitle="Ändern der Service Tier und Leistung einer SQL Azure-Datenbank | Microsoft Azure"
    description="Ändern der Dienstebene und Leistung einer Azure SQL-Datenbank zeigt die SQL-Datenbank nach oben oder unten skalieren. Ändern den Tarif einer Azure SQL-Datenbank."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-using-the-azure-portal"></a>Ändern der Service Tier und Leistung (Tarif) einer SQL Azure-Portal verwenden


> [AZURE.SELECTOR]
- [**Azure-portal**](sql-database-scale-up.md)
- [PowerShell](sql-database-scale-up-powershell.md)


Dienstebenen Leistung Beschreiben der Features und Ressourcen für die SQL-Datenbank und Bedarf der Änderung aktualisiert werden. Weitere Informationen finden Sie unter [Dienstebenen](sql-database-service-tiers.md).

Beachten Sie, dass die Dienstebene ändern oder einer Datenbank erstellt ein Replikat der ursprünglichen Datenbank auf Neues Leistungsniveau, und wechselt dann Verbindungen über das Replikat. Keine Daten geht dabei verloren während des kurzen Moments, wenn wir das Replikat zu wechseln, für die Datenbank sind jedoch deaktiviert, sodass Transaktionen in Flight zurückgesetzt werden. Dieses variiert, aber ist im Durchschnitt weniger als 4 Sekunden und mehr als 99 % der Fälle ist 30 Sekunden. Selten, insbesondere bei großen Anzahl von Transaktionen im Flug an den Moment deaktiviert, dieses Fenster mehr.  

Die Dauer der gesamten skalieren hängt von der Größe und Service-Ebene der Datenbank vor und nach der Änderung. Beispielsweise sollte eine 250-GB-Datenbank, die an, von oder innerhalb einer Dienstebene Standard ändern innerhalb von 6 Stunden abgeschlossen. Für eine Datenbank die gleiche Größe, die Performance innerhalb der Premium-Service-Tier geändert wird, sollte es innerhalb von 3 Stunden abgeschlossen.


Mithilfe der Informationen in [Azure SQL Datenbank Dienstebenen und](sql-database-service-tiers.md) entsprechenden Service-Tier und Performance für Ihre Azure SQL-Datenbank bestimmen.

- Zum Herabstufen einer Datenbank sollte die Datenbank kleiner als die maximal zulässige Größe der Dienstebene Ziel. 
- Beim Aktualisieren einer Datenbank mit [Geo-Replikation](sql-database-geo-replication-overview.md) aktiviert müssen Sie vor dem Aktualisieren der primären Datenbank zunächst sekundären Datenbanken auf die gewünschte Leistungsebene aktualisieren.
- Beim Herunterstufen einer Dienstebene müssen Sie alle Geo-Replikation Beziehungen beenden. 
- Restore Service-Angebote sind für verschiedene Dienstebenen. Wenn Sie herabstufen kann verlieren die Möglichkeit, zu einem Zeitpunkt wiederherstellen, oder eine niedrigere backup Aufbewahrungsdauer. Weitere Informationen finden Sie unter [Azure SQL-Datenbank sichern und Wiederherstellen](sql-database-business-continuity.md).
- Ändern der Datenbank Tarif ändert nicht die maximale Datenbankgröße. Ändern die Datenbank Max. verwenden Sie [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
- Neuen Eigenschaften für die Datenbank werden nicht angewendet, bis die Änderung abgeschlossen sind.



**Zum Abschließen dieses Artikels benötigen Sie Folgendes:**

- Eine SQL Azure-Datenbank. Wenn Sie keinen SQL-Datenbank erstellen einen der Schritte in diesem Artikel: [Erstellen Sie Ihre erste Azure SQL-Datenbank](sql-database-get-started.md).


## <a name="change-the-service-tier-and-performance-level-of-your-database"></a>Ändern Sie die Dienstebene Tier und Leistung der Datenbank


Öffnen Sie das Blade SQL-Datenbank für die Datenbank oder Sie:

1.  Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **Weitere Dienste** > **SQL-Datenbanken**.
2.  Klicken Sie auf die Datenbank, die Sie ändern möchten.
3.  Klicken Sie auf die **SQL-Datenbank** auf **Preisstufe (Skalieren DTUs)**:

    ![Preisstufe][1]

1.  Wählen Sie eine neue Ebene, und klicken Sie auf **auswählen**:

    **Wählen** Sie auf fordert Skalierung den Tarif ändern. Je nach Größe der Datenbank kann Skalierung abgeschlossen (siehe Informationen am Anfang dieses Artikels) einige Zeit dauern.

    > [AZURE.NOTE] Ändern der Datenbank Tarif ändert nicht die maximale Datenbankgröße. Ändern die Datenbank Max. verwenden Sie [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).

    ![Tarif wählen][2]

3.  Klicken Sie auf das Symbol (Fahrradklingel) in der oberen rechten Ecke:

    ![Benachrichtigung][3]

    Klicken Sie auf den Benachrichtigungstext öffnen im Detailbereich, den Status der Anforderung finden.




## <a name="next-steps"></a>Nächste Schritte

- Ändern Sie die maximale Datenbankgröße mit [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
- [Skalieren und](sql-database-elastic-scale-get-started.md)
- [Verbinden und eine SQL-Datenbank mit SSMS Abfragen](sql-database-connect-query-ssms.md)
- [Exportieren einer SQL Azure-Datenbank](sql-database-export.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Business Continuity – Überblick](sql-database-business-continuity.md)
- [Dokumentation zur SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)


<!--Image references-->
[1]: ./media/sql-database-scale-up/new-tier.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png
