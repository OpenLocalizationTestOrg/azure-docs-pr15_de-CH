<properties 
    pageTitle="Ändern der Service Tier und Leistung einer Azure SQL-Datenbank mithilfe von PowerShell | Microsoft Azure" 
    description="Ändern der Dienstebene und Leistung einer Azure SQL-Datenbank zeigt die SQL-Datenbank mit PowerShell nach oben oder unten zu skalieren. Den Tarif einer Azure SQL-Datenbank mit PowerShell ändern." 
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


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>Ändern Sie Tier und Leistung Dienstebene (Tarif) einer SQL-Datenbank mit PowerShell


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-scale-up.md)
- [**PowerShell**](sql-database-scale-up-powershell.md)


Dienstebenen Leistung Beschreiben der Features und Ressourcen für die SQL-Datenbank und Bedarf der Änderung aktualisiert werden. Weitere Informationen finden Sie unter [Dienstebenen](sql-database-service-tiers.md).

Beachten Sie, dass die Dienstebene ändern oder einer Datenbank erstellt ein Replikat der ursprünglichen Datenbank auf Neues Leistungsniveau, und wechselt dann Verbindungen über das Replikat. Keine Daten geht dabei verloren während des kurzen Moments, wenn wir das Replikat zu wechseln, für die Datenbank sind jedoch deaktiviert, sodass Transaktionen in Flight zurückgesetzt werden. Dieses variiert, aber ist im Durchschnitt weniger als 4 Sekunden und mehr als 99 % der Fälle ist 30 Sekunden. Selten, insbesondere bei großen Anzahl von Transaktionen im Flug an den Moment deaktiviert, dieses Fenster mehr.  

Die Dauer der gesamten skalieren hängt von der Größe und Service-Ebene der Datenbank vor und nach der Änderung. Beispielsweise sollte eine 250-GB-Datenbank, die an, von oder innerhalb einer Dienstebene Standard ändern innerhalb von 6 Stunden abgeschlossen. Für eine Datenbank die gleiche Größe, die Performance innerhalb der Premium-Service-Tier geändert wird, sollte es innerhalb von 3 Stunden abgeschlossen.


- Zum Herabstufen einer Datenbank sollte die Datenbank kleiner als die maximal zulässige Größe der Dienstebene Ziel. 
- Beim Aktualisieren einer Datenbank mit [Geo-Replikation](sql-database-geo-replication-portal.md) aktiviert müssen Sie vor dem Aktualisieren der primären Datenbank zunächst sekundären Datenbanken auf die gewünschte Leistungsebene aktualisieren.
- Beim Herunterstufen von ein Premium-Service-Tier müssen Sie zuerst alle Geo-Replikation Beziehungen beenden. Sie können die Schritte im Thema [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md) des Replikationsvorgangs zwischen primären und aktiven sekundären Datenbanken zu folgen.
- Restore Service-Angebote sind für verschiedene Dienstebenen. Wenn Sie herabstufen kann verlieren die Möglichkeit, zu einem Zeitpunkt wiederherstellen, oder eine niedrigere backup Aufbewahrungsdauer. Weitere Informationen finden Sie unter [Azure SQL-Datenbank sichern und Wiederherstellen](sql-database-business-continuity.md).
- Neuen Eigenschaften für die Datenbank werden nicht angewendet, bis die Änderung abgeschlossen sind.



**Zum Abschließen dieses Artikels benötigen Sie Folgendes:**

- Ein Azure-Abonnement. Wenn Sie Azure-Abonnement lediglich **Kostenloses Konto** am oberen Rand dieser Seite klicken Sie und dann wieder zu diesem Artikel.
- Eine SQL Azure-Datenbank. Wenn Sie keinen SQL-Datenbank erstellen einen der Schritte in diesem Artikel: [Erstellen Sie Ihre erste Azure SQL-Datenbank](sql-database-get-started.md).
- Azure PowerShell.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>Welche Service-Tier und Leistung Ihrer SQL-Datenbank

Führen Sie [Set-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx)-Cmdlet und Set **-RequestedServiceObjectiveName** auf die Leistungsfähigkeit des gewünschten Tarif; z. B. *S0* *S1*, *S2*, *S3*, *P1*, *P2*,...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>PowerShell-Beispielskript der Service-Tier und Leistung Ihrer SQL-Datenbank ändern

Ersetzen Sie ```{variables}``` mit Ihren Werten (die geschweiften Klammern nicht enthalten).

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Nächste Schritte

- [Skalieren und](sql-database-elastic-scale-get-started.md)
- [Verbinden und eine SQL-Datenbank mit SSMS Abfragen](sql-database-connect-query-ssms.md)
- [Exportieren einer SQL Azure-Datenbank](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Business Continuity – Überblick](sql-database-business-continuity.md)
- [Dokumentation zur SQL-Datenbank](http://azure.microsoft.com/documentation/services/sql-database/)
- [Azure SQL-Datenbank-Cmdlets] (http://msdn.microsoft.com/library/azure/mt574084https :/ / msdn.microsoft.com/Library/Azure/mt619433(v=azure.300\).aspx.aspx)