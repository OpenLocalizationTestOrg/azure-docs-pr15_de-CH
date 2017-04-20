<properties 
    pageTitle="Wiederherstellen einer Anwendung in Azure" 
    description="Erfahren Sie, wie Ihre Anwendung von einer Sicherung wiederherstellen." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>Wiederherstellen einer Anwendung in Azure

Dieser Artikel beschreibt, wie eine Anwendung in [Azure App Service](../app-service/app-service-value-prop-what-is.md) wiederherstellen, das zuvor (siehe [Sichern Ihrer Anwendung in Azure](web-sites-backup.md)) gesichert. Sie können Ihre Anwendung mit der verknüpften Datenbanken (SQL-Datenbank oder MySQL) bei Bedarf in einem früheren Zustand wiederherstellen oder eine neue Anwendung basierend auf der ursprünglichen Anwendung Sicherung erstellen. Erstellen eine neue Anwendung, die parallel auf die neueste Version ausgeführt ist nützlich für A / B-Tests.

Wiederherstellen von Sicherungskopien steht apps im **Standard-** und **Premium** -Ebene. Informationen zur Skalierung Ihrer Anwendung finden Sie unter [Skalieren einer Anwendung in Azure](web-sites-scale.md). **Premium** -Ebene ermöglicht eine größere Anzahl von täglichen Backups als **Standard** Ebene ausgeführt werden.

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>Eine Anwendung von einer vorhandenen Sicherung wiederherstellen

1. Klicken Sie auf Blatt **Einstellungen** der Anwendung in Azure-Portal auf **Sicherung** **Backups** Blade angezeigt. Klicken Sie dann auf **Jetzt wiederherstellen** in der Befehlszeile. 
    
    ![Wählen Sie jetzt wiederherstellen][ChooseRestoreNow]

3. Blatt **Wiederherstellen** zuerst wählen Sie backup. 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    Die **App Backup** -Option zeigt alle vorhandenen Sicherungskopien der aktuellen Anwendung, und Sie können problemlos. 
    Die **Storage** -Option können Sie alle ZIP-Sicherungsdatei von vorhandenen Azure Storage-Konto und Container in Ihrem Abonnement auswählen. 
    Wenn Sie zu einer anderen Anwendung sichern, verwenden Sie die Option **Speichern** .

4. Geben Sie das Ziel für die Wiederherstellung app im **Ziel wiederherstellen**.

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] **Überschreiben**auswählen, werden alle vorhandene Daten in der aktuellen Anwendung verloren. Vergewissern Sie klicken Sie auf " **OK**", dass es genau, was Sie tun möchten.
    
    Sie können **Vorhandene Anwendung** einer anderen Anwendung in derselben Gruppe Resoure app Sicherung wiederherstellen auswählen. Bevor Sie diese Option verwenden, sollten Sie bereits einer anderen Anwendung in der Ressourcengruppe mit Spiegelung Datenbankkonfiguration, definiert in der app-Sicherung erstellt haben. 
    
5. Klicken Sie auf **OK**.

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Herunterladen Sie oder löschen Sie eine Sicherungskopie von einem Storage-Konto
    
1. Aus dem Blade Azure-Portal **Durchsuchen** **Speicherkonten**wählen.
    
    Eine Liste der vorhandenen Speicherkonten wird angezeigt. 
    
2. Wählen Sie das Speicherkonto mit der Sicherung, die Sie herunterladen oder löschen möchten.
    
    Blatt für das Speicherkonto wird angezeigt.

3. Wählen Sie im Blatt Accountn Speicher gewünschten container
    
    ![Container anzeigen][ViewContainers]

4. Wählen Sie die Sicherungsdatei zum Herunterladen oder löschen möchten.

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. Klicken Sie je nach Bedarf **herunterladen** oder **Löschen** .  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>Überwachen einer Wiederherstellungsoperation
    
1. Um Informationen über den Erfolg oder Misserfolg des Wiederherstellungsvorgangs app navigieren Sie das **Überwachungsprotokoll** Blade im Azure-Portal. 
    
    **Überwachungsprotokolle** Blade zeigt alle Operationen, Ebene, Status, Ressource und Zeitangaben.
    
2. Blättern Sie zu der gewünschten Vorgang und klicken sie.

Blatt Details Zeigt verfügbare Informationen zu den Wiederherstellungsvorgang.
    
## <a name="next-steps"></a>Nächste Schritte

Sie sichern und Wiederherstellen App Service apps mit REST-API (siehe [REST sichern und Wiederherstellen von App Service apps verwenden](websites-csm-backup.md)).

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
