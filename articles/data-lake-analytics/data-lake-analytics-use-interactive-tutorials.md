<properties 
   pageTitle="Erfahren Sie See Datenanalyse und U-SQL Azure Portal interaktive Tutorials mit | Azure" 
   description="Schnellstart mit See Datenanalyse und U-SQL. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Verwenden Sie interaktive Tutorials Azure Data Lake Analytics

Azure-Portal bietet ein interaktives Lernprogramm zur Datenanalyse See Einstieg. In diesem Artikel wird veranschaulicht, wie das Tutorial für die Analyse der Website anmeldet.


>[AZURE.NOTE]Ggf. das gleiche Tutorial mit Visual Studio finden Sie unter [Analysieren Website Protokolle mit See Datenanalyse](data-lake-analytics-analyze-weblogs.md).
>Weitere interaktive Tutorials Portal hinzugefügt werden.


Andere Lernprogramme finden Sie unter:

- [Erste Schritte mit See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md)
- [Erste Schritte mit See Datenanalyse mithilfe von Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Erste Schritte mit See Datenanalyse mit .NET SDK](data-lake-analytics-get-started-net-sdk.md)
- [Entwickeln Sie U-SQL-Skripts mit Daten See Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) 

**Erforderliche Komponenten**

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Eine Datenanalyse See Konto**.  [Erste Schritte mit Azure See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md)anzeigen

##<a name="create-data-lake-analytics-account"></a>Datenanalyse See-Konto erstellen 

Datenanalyse See Konto müssen vor dem Ausführen alle Aufträge verwendet werden.

Jedes Konto See Datenanalyse hat eine Abhängigkeit [See Datenspeicher Azure](../data-lake-store/data-lake-store-overview.md) -Konto.  Dieses Konto wird als Standardkonto See Datenspeicher bezeichnet.  Konto See Datenspeicher können vorher oder Ihr Konto See Datenanalyse zu erstellen. In diesem Lernprogramm erstellen Sie Konto See Datenspeicher mit Analytics-Konto

**Erstellen eines Kontos See Datenanalyse**

1. [Azure-Portal](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute)anmelden.
2. Klicken Sie auf **Microsoft Azure** in der oberen linken Ecke, um das Startmenü zu öffnen.
3. Klicken Sie auf die Kachel **Marketplace** .  
3. Geben Sie im Suchfeld auf das **Alles** -Blade, und drücken Sie die **EINGABETASTE** **Azure Data Lake Analytics** . **Azure Data Lake Analytics** sehen in der Liste.
4. Klicken Sie in der Liste auf **Azure Data Lake Analytics** .
5. Klicken Sie auf **Erstellen** auf der Unterseite des Blades.
6. Geben Sie oder wählen Sie Folgendes aus:

    ![Azure Data Lake Analytics Portal blade](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Name**: Name Analytics-Konto.
    - **Datenspeicher See**: jede Datenanalyse See Konto hat ein abhängiger See Datenspeicher Konto. Datenanalyse See und der abhängigen See Datenspeicher müssen im gleichen Azure-Rechenzentrum befinden. Führen Sie die Anweisung zum Erstellen eines neuen Kontos See Datenspeicher oder wählen Sie ein vorhandenes Profil aus.
    - **Abonnement**: Azure Abonnements Analytics-Konto auswählen.
    - **Ressourcengruppe**. Wählen Sie eine vorhandene Ressourcengruppe Azure oder erstellen Sie eine neue. Anwendung normalerweise viele Komponenten, z. B. eine Webanwendung, Datenbank Datenbank, Speicher und 3. Dienste bestehen. Azure Resource Manager (ARM) können Sie die Ressourcen in der Anwendung als Gruppe als eine Azure-Ressourcengruppe arbeiten. Sie bereitstellen, aktualisieren, überwachen oder alle Ressourcen für die Anwendung in einem einzigen koordinierten Vorgang löschen. Verwenden Sie eine Vorlage für die Bereitstellung und die Vorlage kann für verschiedene Unternehmen wie Tests, Staging und Produktion. Abrechnung für Ihr Unternehmen zu klären die mehrstufigen Kosten für die gesamte Gruppe anzeigen. Weitere Informationen finden Sie unter [Azure-Ressourcen-Manager (Übersicht)](azure-resource-manager/resource-group-overview.md). 
    - **Speicherort**. Ein Azure-Rechenzentrum für die Datenanalyse See Konto auswählen 
7. Wählen Sie **an Startmenü anheften**. Dies ist für dieses Lernprogramm erforderlich.
8. Klicken Sie auf **Erstellen**. Es dauert das Portal Startmenü. Eine neue Tile wird die Homepage mit der Bezeichnung "Bereitstellen von Azure Data Lake Analytics" zeigt hinzugefügt. Es dauert einen Moment, See Datenanalyse-Konto erstellen. Beim Erstellen des Kontos wird das Portal des Kontos auf ein neues Blatt geöffnet.

    ![Azure Data Lake Analytics Portal blade](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##<a name="run-website-log-analysis-interactive-tutorial"></a>Interaktives Lernprogramm Websiteanalyse ausführen

**Website-Protokollanalyse interaktive Lernprogramm öffnen**

1. Das Portal klicken Sie auf **Microsoft Azure** aus dem linken Menü um das Startmenü zu öffnen.
2. Klicken Sie auf die Kachel See Datenanalyse-Konto verknüpft ist.
3. Klicken Sie auf **Explorer interaktive Tutorials** **Essentials** Bar.

    ![Data Lake Analytics interaktive tutorials](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. Angezeigt eine orange Warnung sagen "Proben nicht eingerichtet, klicken Sie auf...", **Beispieldaten** um Beispieldaten in das Standardkonto See Datenspeicher zu kopieren. Interaktive Lernprogramm benötigt die Daten.
5. Klicken Sie auf **Website Protokollanalyse**Blatt **Interaktiven Tutorials** . Das Portal Öffnet das Lernprogramm in einem neuen Portal Blade.
5. **1 Einführung** klicken und dann

##<a name="see-also"></a>Siehe auch

- [Übersicht über Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
- [Erste Schritte mit See Datenanalyse mithilfe von Azure-Portal](data-lake-analytics-get-started-portal.md)
- [Erste Schritte mit See Datenanalyse mithilfe von Azure PowerShell](data-lake-analytics-get-started-powershell.md)
- [Entwickeln Sie U-SQL-Skripts mit Daten See Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Analysieren Sie Website-Protokolle mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md)
