<properties
    pageTitle="Web Apps Ressourcenanbieter Azure Stapel hinzufügen | Microsoft Azure"
    description="Detaillierte Anleitung zum Bereitstellen von Web-Apps in Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>Einen Ressourcenanbieter Web Apps Azure Stapel hinzufügen

> [AZURE.NOTE] Folgendes gilt nur für Azure Stapel TP1 Bereitstellungen.

Web Apps Ressourcenanbieter Azure Stapel hinzufügen hat sieben Schritte:

1.  Erforderliche Komponenten herunterladen
2.  Erstellen Sie Zertifikate von Azure Stapel Web Apps verwendet werden.
3.  Verwenden Sie das Installationsprogramm herunterladen, Bühne und Azure Stack Web Apps installieren. 
4.  Web Apps-Installation zu überprüfen.
5.  Erstellen Sie DNS-Einträge für die Front-End und Lastenausgleich Verwaltungsserver.
6.  Registrieren des neu bereitgestellten Web Apps Ressourcenanbieters ARM.
7.  Testen den Web Apps Ressourcenanbieter.

## <a name="download-required-components"></a>Erforderliche Komponenten

1.  [Azure Stapel App Vorschau ServiceInstaller](http://aka.ms/azasinstaller)herunterladen 
2.  [Azure Stapel App Service Bereitstellungsskripts Helper](http://aka.ms/azashelper)downloaden. 
3.  Extrahieren Sie die Dateien aus dem Hilfsprogramm Skripts Zip-Datei, drei Skripts:
    - Erstellen AppServiceCerts.ps1
    - Erstellen AppServiceDnsRecords.ps1
    - AppServiceResourceProvider.ps1 registrieren 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>Erstellen von Zertifikaten von Azure Stapel Web Apps

Dieses erste arbeitet mit Azure Stapel Zertifizierungsstelle 3 Zertifikate erstellen, die von Web Apps benötigt werden. Führen Sie das Skript auf der ClientVM stellt sicher, dass Sie PowerShell als Azurestack\administrator ausgeführt:
1.  Führen Sie in einer PowerShell-Sitzung als **Azurestack\administrator** **Erstellen AppServiceCerts.ps1** -Skript.  Dadurch drei Zertifikate im gleichen Ordner wie das Skript von Web Apps benötigt werden.
2.  Geben Sie ein Kennwort zum Schützen der Pfx-Dateien und notieren Sie ihn wie in Azure Stapel Web Apps Installer eingeben müssen.

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>Verwenden Sie das Installationsprogramm zum Herunterladen und Installieren von Azure Stapel Web Apps

Das appservice.exe-Installationsprogramm wird:
1.  Der Benutzer aufgefordert, die von Microsoft und Drittanbietern EULA akzeptieren.
2.  Sammeln Sie Azure Stapel Bereitstellungsinformationen.
3.  Erstellen Sie einen BLOB-Container im Speicherkonto Azure Stapel angegeben.
4.  Herunterladen Sie den Ressourcenanbieter Azure Stapel Web App Installation benötigten Dateien
5.  Vorbereiten der Installation Ressourcenanbieter Web App in Azure-Stack-Umgebung bereitstellen.
6.  Hochladen Sie die Dateien in das Speicherkonto App Service.
7.  Vorhanden Sie Informationen zum Starten der Azure-Ressourcen-Manager-Vorlage.

Die folgenden Schritte führen Sie durch Installation Phasen:

>[AZURE.NOTE] Ein Konto mit erhöhten Rechten (lokaler Administrator oder Domänenadministrator) verwenden das Installationsprogramm ausführen. Wenn Sie als Azurestack\azurestackuser anmelden, werden Sie aufgefordert, erweiterten Anmeldeinformationen anzugeben. 

1.  Appservice.exe als **Azurestack\administrator**ausgeführt. 
2.  Klicken Sie auf **Deploy Azure-Ressourcen-Manager verwenden**.

![Azure Stapel App Service Technical Preview 1 Installer][1]

3.  Überprüfen und **Klicken Sie dann auf**Microsoft-Vorabversion Softwarelizenzvertrag akzeptieren.
4.  Überprüfen der dritten Partylicense akzeptieren und klicken Sie auf **Weiter**.
5.  Überprüfen Sie die Konfigurationsinformationen App Cloud-Dienst, und **Klicken Sie auf**.

![Azure App Service Technical Preview 1 App Service Cloud Konfiguration][2]

6. **Klicken Sie auf (neben dem Feld Azure Stapel Abonnements).**

![Azure Stapel App Service Technical Preview 1 App Service Cloud-Konfigurationsbildschirm zwei][3]

7.  Im Fenster Azure Stack Authentifizierung Ihre **Azure Active Directory Service Admin-Konto** und **Kennwort**und klicken Sie dann auf **Anmelden**.
**Hinweis:** Das Azure Active Directory-Konto, das Sie bei der Bereitstellung von Azure Stapel angegeben ist.
    - Klicken Sie auf den **Abwärtspfeil** auf der rechten Seite das Kontrollkästchen **Azure Stapel Abonnements** , und wählen Sie Ihr Abonnement.

![Azure Stapel App Technical Preview 1 Abonnementauswahl][5]

8.  Klicken Sie auf den **Abwärtspfeil** auf der rechten Seite das Kontrollkästchen **Positionen von Azure**.
    - Wählen Sie **lokale**.
9. Geben Sie den **Namen** für den Administrator.
10. Geben Sie ein **Kennwort** für den Administrator.
11. Überprüfen Sie die **SQL Server-Daten** und ändern ggf..
12. Das **Systemadministrator-Anmeldekonto** überprüfen und ggf. ändern.
13. Geben Sie das **Systemadministratorkennwort**ein.
14. Klicken Sie auf **Weiter**.  Zu diesem Zeitpunkt überprüft das Installationsprogramm jetzt die Verbindungsdetails für SQL Server bereitgestellt.

![Azure Stapel App Technical Preview 1 Abonnementauswahl][4]    

15. Klicken Sie auf neben der **Web Apps Standard SSL-Zertifikatdatei** **Durchsuchen** , und navigieren Sie zu **Webapps. AzureStack.Local** [zuvor erstellte](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)Zertifikat.
16. Geben Sie das **Kennwort für** die bei Erstellung der Zertifikate.
17. Klicken Sie auf neben der **Anbieter SSL Zertifikat Ressourcendatei** **Durchsuchen** , und navigieren Sie zu **-Management. AzureStack.Local** [zuvor erstellte](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)Zertifikat.
18. Geben Sie das **Kennwort für** die bei Erstellung der Zertifikate.
19. Klicken Sie auf neben der **Ressource Anbieter Stammzertifikatdatei** **Durchsuchen** , und navigieren Sie zu **-Management. AzureStack.Local** [zuvor erstellte](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps)Zertifikat.
20. Klicken Sie auf **nächste** Installers Zertifikatkennwort überprüfen.

![Azure App Service Technical Preview 1 Zertifikat Stapeldetails][6]

Die Bereitstellung dauert ungefähr 45 bis 60 Minuten.

![Azure Stapel App Service Technical Preview 1 Installationsstatus][7]

21. Klicken Sie auf **Beenden**, nachdem das Installationsprogramm erfolgreich abgeschlossen wurde.

## <a name="validate-web-apps-installation"></a>Web Apps-Installation überprüfen

1.  Öffnen Sie auf dem **Hostcomputer von Azure Stapel** **Hyper-V-Manager**.
2.  Suchen Sie **CN0 VM** und der VM **herstellen** .
![Azure Stack App Service Technical Preview 1 Hyper-V-Manager][8]
3.  Doppelklicken Sie auf diesem virtuellen Computer-Desktop **Web Cloud-Verwaltungskonsole**.
![Azure Stapel App Technical Preview 1 Dienstverwaltungskonsole][9]
4.  Navigieren Sie zum **verwalteten Server**.
5.  Alle Computer sind **bereit** fahren Sie fort mit dem nächsten Schritt. 
![Azure Stapel App Technical Preview 1 verwalteten Servern Dienststatus][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>Erstellen Sie DNS-Einträge für Management-Server und Front-End-Lastenausgleich
1.  Öffnen Sie eine PowerShell-Instanz als **Azurestack\administrator**.
2.  Navigieren Sie zum Speicherort der Skripts heruntergeladen und in [Schritt](#Download-Required-Components)extrahiert.
3.  Führen Sie das Skript **AppServiceDnsRecords.ps1 erstellen** , erstellt diese DNS-Einträge, um das Portal und app Anfragen an die Front-End-Server weitergeleitet werden können.  Bei ARM-Bereitstellung von Web Apps, zwei Software-Lastenausgleich (SLBs) der Ressource Netzwerkanbieter entstanden. Sie zeigen die Verwaltungsserver und die Front-End-Server. Portal und ARM-basierten Anfragen Azure Stapel App Ressource Anbieter gehen an den Verwaltungsserver.

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>ARM registrieren Sie neu bereitgestellten Azure Stapel Web Apps-Anbieter
1.  Öffnen Sie eine PowerShell-Instanz als **Azurestack\administrator**.
2.  Navigieren Sie zum Speicherort der Skripts heruntergeladen und in [Schritt](#Download-Required-Components)extrahiert.
3.  Führen Sie das Skript **AppServiceResourceProvider.ps1 registrieren** . 

>[AZURE.NOTE] Geben Sie den Benutzernamen und das Kennwort **genau (einschließlich Groß- und Kleinschreibung)** für die **Virtuellen Computer Administrator** und das **Kennwort** in der Installation eingegeben wurde oder der Ressource anbieterregistrierung schlägt fehl.

## <a name="test-drive-azure-stack-web-apps"></a>Test Drive Azure Stapel webapps

Sie bereitgestellt und Web Apps Ressourcenanbieter registriert haben, können Sie testen, um sicherzustellen, dass Mieter webapps bereitstellen können.

1.  Klicken Sie im Portal Azure Stapel auf neu, klicken Sie auf Web + Mobile und auf Web App.
2.  Geben Sie einen Namen im Web app Blatt Web App.
3.  Unter Ressourcengruppe auf neu, und geben Sie einen Namen im Feld Ressourcengruppe. 
4.  App Service-Plan/Verzeichnis klicken Sie und neu erstellen.
5.  Geben Sie einen Namen im Feld Plan App App Service Plan Blatt.
6.  Klicken Sie auf Preisstufe, klicken Sie auf Free Shared oder Shared-Shared, wählen Sie und klicken Sie dann auf erstellen.
7.  In weniger als einer Minute erscheint eine Kachel für das neue Web app im Schaltpult. Klicken Sie auf die Kachel.
8.  Web app Blatt klicken Sie auf Durchsuchen, um die Standardwebsite für diese Anwendung anzuzeigen.


**Bereitstellen einer Website WordPress, DNN oder Django (optional)**

1. Klicken Sie im Portal Azure Stapel auf "+" Gehen Sie Azure Marketplace Bereitstellen einer Websites Django und Abschluss warten. Django-Plattform verwendet eine Datenbank mit Datei System und erfordert zusätzliche Ressourcenanbieter wie SQL oder MySQL.  

2. Wenn Sie auch einen Ressourcenanbieter MySQL bereitgestellt, können Sie eine Website WordPress Markt bereitstellen. Wenn für Datenbankparameter aufgefordert werden, geben Sie den Benutzernamen als *User1@Server1* (mit Benutzernamen und Servernamen Ihrer Wahl).

3. Wenn Sie auch einen Ressourcenanbieter SQL Server bereitgestellt, können Sie eine Website DNN Markt bereitstellen. Wenn Sie für Datenbankparameter aufgefordert werden, wählen Sie eine Datenbank auf dem Computer mit SQL Server, die Verbindung mit der Ressource.

## <a name="next-steps"></a>Nächste Schritte

Sie können auch andere [Plattform als Dienst (PaaS)](azure-stack-tools-paas-services.md), wie [SQL Server Ressourcenanbieter](azure-stack-sql-rp-deploy-short.md) und [MySQL Ressourcenanbieter](azure-stack-mysql-rp-deploy-short.md)ausprobieren.

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
