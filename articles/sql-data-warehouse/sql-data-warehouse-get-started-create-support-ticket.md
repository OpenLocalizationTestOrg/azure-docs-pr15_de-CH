<properties
   pageTitle="Erstellen Sie ein Support-Ticket für SQL Data Warehouse | Microsoft Azure"
   description="Support-Ticket in Azure SQL Data Warehouse erstellen"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess"/>

# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Erstellen Sie ein Support-Ticket für SQL Data Warehouse
 
Wenn Sie Probleme mit Ihrem SQL Data Warehouse erstellen Sie ein Support-ticket, damit unser engineering-Team helfen kann.

## <a name="create-a-support-ticket"></a>Supportticket erstellen

1. [Azure-Portal][]zu öffnen.

2. Home-Bildschirm klicken Sie **Hilfe und Support** .

    ![Hilfe und support](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)

3. Auf + Unterstützung Blade **Supportanfrage erstellen**klicken.

    ![Neue Anfrage](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
    
    <a name="request-quota-change"></a> 

4. Wählen Sie die **Anforderung**.

    ![Typ](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
    
    >[AZURE.NOTE]  Standardmäßig hat jeder SQL Server (z.B. myserver.database.windows.net) **DTU Kontingent** von 45.000. Dieses Kontingent ist einfach eine Sicherheitsgrenze. Ihr Kontingent können Support-Ticket erstellen und als Anfragetyp *Kontingent* auswählen. Berechnet die DTU muss die Summe der 7.5 Multiplizieren [DWU][] benötigt. Beispielsweise möchten zwei DW6000s auf eine SQL Server hosten und bitte DTU Kontingent von 90.000.  Sie können Ihre aktuellen DTU Verbrauch aus dem SQL Server-Blade im Portal anzeigen. Unterbrochen und die Ausführung Datenbanken DTU Kontingent gezählt. 

5. Wählen Sie das **Abonnement** , die Datenbank mit dem Problem zu hostet.

    ![Abonnement](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)

6. Wählen Sie **SQL Datawarehouse** als Ressource.

    ![Ressource](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)

7. Wählen Sie Ihre [Azure unterstützen][].

    - **Abrechnung Kontingent und Abonnement-Management** Support steht auf allen unterstützen.
    - **Break-Fix** [Developer][], [Standard][], [Professional Direct][] oder [Premier][] Support unterstützt. Probleme sind Probleme von Kunden mit Azure besteht berechtigterweise Microsoft das Problem verursacht.
    - **Entwickler mentoring** und **Beratung** stehen auf [Direkte Professional][] und [Premier][] Support zur Verfügung. 
    
    Haben Sie einen Premier support-Plan können Sie auch SQL Data Warehouse melden Probleme im [Microsoft Premier Online-Portal][].  Erfahren Sie mehr über die verschiedenen Support-Pläne Bereich, Reaktionszeiten, Preise usw. finden Sie in der [Azure support-Pläne][Azure unterstützen] .  Häufig gestellte Fragen zur Azure unterstützen Sie, finden Sie unter [Azure FAQs unterstützen][].  

    ![Support-plan](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)

8. Wählen Sie den **Problemtyp** und **Kategorie**.

    ![Problemkategorie-Typ](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)

9. Beschreiben Sie das Problem, und wählen Sie die geschäftlichen.

    ![Beschreibung des Problems](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)

10. Die **Kontaktinformationen** für dieses supportticket werden bereits ausgefüllt. Aktualisieren Sie diese gegebenenfalls.

    ![Kontaktinformationen](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)

11. Klicken Sie auf **Erstellen** , um die Anfrage abzusenden.


## <a name="monitor-a-support-ticket"></a>Support-Ticket zu überwachen

Nachdem Sie die Anfrage gesendet haben, kontaktiert das Azure-Support-Team Sie. Der Status der Anforderung und Details klicken Sie auf **Support-Anfragen verwalten** im Schaltpult.

![Überprüfen des status](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Andere Ressourcen

Darüber hinaus können Sie mit SQL Data Warehouse Community [Stapelüberlauf][] oder [Azure SQL Data Warehouse MSDN-Forum][].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Azure-portal]: https://portal.azure.com/
[Azure Support-plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Entwickler]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure Support-FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier Online-portal]: https://premier.microsoft.com/
[Stack-Überlauf]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN-forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

