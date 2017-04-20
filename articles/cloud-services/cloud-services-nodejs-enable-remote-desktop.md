<properties 
    pageTitle="Aktivieren des Remotedesktops für Cloud-Services (Node.js)" 
    description="Informationen Sie zum Remote Desktop-Zugriff für die virtuelle Computer hosten Sie Ihre Azure Node.js-Anwendung aktivieren." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="enabling-remote-desktop-in-azure"></a>Aktivieren von Remotedesktop in Azure

Remotedesktop können Sie auf dem Desktop eine in Azure ausgeführte Instanz der Rolle. Eine Remotedesktopverbindung können Sie Probleme mit der Anwendung oder den virtuellen Computer konfigurieren.

> [AZURE.NOTE] Dieser Artikel gilt für Node.js Applikationen als Azure Cloud Service.


## <a name="prerequisites"></a>Erforderliche Komponenten

- Installieren und Konfigurieren von [Azure Powershell](../powershell-install-configure.md).
- Bereitstellen Sie Node.js-Anwendung für ein Azure-Cloud-Dienst. Weitere Informationen finden Sie unter [Erstellen und Bereitstellen eine Anwendung Node.js Azure Cloud Service](cloud-services-nodejs-develop-deploy-app.md).


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Schritt 1: Verwendung Azure PowerShell zur Konfiguration des Dienstes für Remote Desktop-Zugriff

Um Remote Desktop verwenden, müssen Sie Azure Dienstdefinition und Konfiguration mit einem Benutzernamen, Kennwort und Zertifikat aktualisieren. 

Die folgenden Schritte von einem Computer mit den Quelldateien für Ihre Anwendung.

1. **Windows PowerShell** als Administrator ausführen. (Aus dem **Menü Start** oder **Startbildschirm**suchen Sie **Windows PowerShell**.)

2.  Navigieren Sie zum Verzeichnis mit den Dienstdefinition (.csdef) und (.cscfg) werden.

3. Geben Sie das folgende PowerShell-Cmdlet ein:

        Enable-AzureServiceProjectRemoteDesktop

4. Aufgefordert werden Geben Sie einen Benutzernamen und ein Kennwort.

    ![Azureserviceprojectremotedesktop aktivieren][enable-rdp]

3.  Geben Sie das folgende PowerShell-Cmdlet um die Änderungen zu veröffentlichen:

        Publish-AzureServiceProject

    ![Veröffentlichen azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Schritt 2: Verbinden Sie mit der Instanz der Rolle

Nach dem Veröffentlichen der Dienstdefinition Update können Sie zu der Rolleninstanz.

1.  Wählen Sie im [klassischen Azure-Portal] **Cloud-Services** , und wählen Sie den Dienst.

    ![Azure-Verwaltungsportal][cloud-services]

2.  Klicken Sie auf **Instanzen**und klicken Sie **Produktion** oder **Staging** Instanzen für Ihren Dienst darauf. Eine Instanz und klicken Sie auf **Connect** am unteren Rand der Seite.

    ![Die Seite Instanzen][3]

2.  Wenn **Verbinden**klicken, fordert der Webbrowser eine RDP-Datei speichern. Öffnen Sie diese Datei. (Z. B. Wenn Sie Internet Explorer verwenden, klicken Sie auf **Öffnen**.)

    ![Aufforderung zum Öffnen oder speichern Sie die RDP-Datei][4]

3.  Beim Öffnen der Datei wird der folgende Sicherheitshinweis angezeigt:

    ![Windows-Sicherheitshinweis][5]

4.  Klicken Sie auf **Verbinden**, und für die Eingabe von Anmeldeinformationen für den Zugriff auf die Instanz ein Sicherheitshinweis angezeigt. Geben Sie das Kennwort [Schritt 1 erstellt] [Schritt 1: Konfigurieren des Dienstes für Remotedesktop Zugriff mithilfe von Azure PowerShell], und klicken Sie dann auf **OK**.

    ![Benutzername und Kennwort Fragen][6]

Wenn die Verbindung hergestellt ist, zeigt Remotedesktopverbindung Desktop der Instanz in Azure. 

![Remotedesktop][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Schritt 3: Konfigurieren Sie den Dienst Remote Desktop-Zugriff deaktivieren 

Wenn Sie Remotedesktopverbindungen Rolleninstanzen in der Cloud nicht mehr benötigen, deaktivieren Sie remote desktop Zugriff mithilfe von [Azure PowerShell].

1.  Geben Sie das folgende PowerShell-Cmdlet ein:

        Disable-AzureServiceProjectRemoteDesktop

2.  Geben Sie das folgende PowerShell-Cmdlet um die Änderungen zu veröffentlichen:

        Publish-AzureServiceProject

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Remotezugriff auf Instanzen in Azure] 
- [Remotedesktop mit Azure-Rollen]
- [Node.js-Entwicklercenter](/develop/nodejs/)

  [Azure PowerShell]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure-Verwaltungsportal]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Remotezugriff auf Instanzen in Azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Remotedesktop mit Azure-Rollen]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 