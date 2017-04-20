<properties
    pageTitle="Sync Inhalte aus einem Ordner Cloud Azure App Service"
    description="Erfahren Sie, wie Ihre Anwendung auf Azure App Service über Content Sync Cloud-Ordner bereitstellen."
    services="app-service"
    documentationCenter=""
    authors="dariagrigoriu"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="dariagrigoriu"/>
    
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Sync Inhalte aus einem Ordner Cloud Azure App Service

Dieses Lernprogramm veranschaulicht die [Azure App](http://go.microsoft.com/fwlink/?LinkId=529714) Service synchronisiert den Inhalt von gängigen Clouddienste wie Dropbox und OneDrive bereitstellen. 

## <a name="overview"></a>Übersicht über Sync Content-Bereitstellung

Inhalt Synchronisieren bei Bedarf Bereitstellung wird durch das [Bereitstellungsmodul Kudu](https://github.com/projectkudu/kudu/wiki) App Service integriert betrieben. In [Azure-Portal](https://portal.azure.com)können Sie legen Sie einen Ordner in die Cloud-Speicher, Ihre app-Code und Inhalt des Ordners und App-Dienst durch Klicken auf eine Schaltfläche synchronisieren. Inhalt synchronisieren verwendet Kudu Prozess zum Erstellen und bereitstellen. 
    
## <a name="contentsync"></a>Inhalt synchronisieren Bereitstellung aktivieren
Gehen Sie folgendermaßen vor, um Inhalte Synchronisieren von [Azure-Portal](https://portal.azure.com)zu aktivieren:

1. **Klicken Sie in Ihrer Anwendung Blade im Azure-Portal** > **Bereitstellungsquelle**. Klicken Sie auf **Auswählen**, und wählen Sie **OneDrive** oder **Dropbox** als Quelle für die Bereitstellung. 

    ![Inhalt synchronisieren](./media/app-service-deploy-content-sync/deployment_source.png)

    >[AZURE.NOTE] Aufgrund der zugrunde liegenden Unterschiede in den APIs ist **OneDrive für Unternehmen** zu diesem Zeitpunkt nicht unterstützt. 

2. Führen Sie den Autorisierung Workflow App Dienst Zugriff auf einen bestimmten vordefinierten festgelegten Pfad für OneDrive oder alle Inhalte App Service Speicherort Dropbox.  
    Nach Genehmigung der App Service erhalten Plattform die Option Inhaltsordner unter dem angegebenen Pfad erstellen oder eine vorhandene Content Ordner unter diesem festgelegten Pfad Sie. Die festgelegten Content Pfade unter Cloud Speicherkonten zur Anwendung synchron sind folgende:  
    * **OneDrive**:`Apps\Azure Web Apps` 
    * **Ablage**:`Dropbox\Apps\Azure`

3. Nach der anfänglichen Synchronisierung Content kann bei Bedarf vom Azure-Portal Content Sync initiiert werden. Bereitstellung Geschichte ist mit der **Bereitstellung** verfügbar.

    ![Verlauf der Bereitstellung](./media/app-service-deploy-content-sync/onedrive_sync.png)
 
Weitere Informationen zur Bereitstellung Dropbox steht unter [Bereitstellen von Dropbox](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 


