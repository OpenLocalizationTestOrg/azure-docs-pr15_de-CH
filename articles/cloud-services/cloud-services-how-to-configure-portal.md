<properties 
    pageTitle="So konfigurieren Sie einen Clouddienst (Portal) | Microsoft Azure" 
    description="Informationen Sie zum Cloud-Services in Azure konfigurieren. Informationen Sie zum Cloud-Dienstkonfiguration aktualisieren und Konfigurieren des Remotezugriffs Rolleninstanzen. Diese Beispiele verwenden Azure-Portal." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>Cloud-Dienste konfigurieren

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-configure-portal.md)
- [Azure-Verwaltungsportal](cloud-services-how-to-configure.md)

Sie können die am häufigsten verwendeten für einen Clouddienst in Azure-Portal konfigurieren. Oder möchten Sie die Konfigurationsdateien direkt aktualisieren, downloaden Dienstkonfigurationsdatei zu aktualisieren, und Laden Sie die aktualisierte Datei und Cloud-Dienst mit der Konfiguration zu aktualisieren. In beiden Fällen werden konfigurationsupdates alle Rolleninstanzen herausgedrückt.

Sie können auch die Instanzen der Cloud-Dienstverwaltungsrollen oder Remotedesktop diese verwalten.

Azure kann nur 99,95 % Verfügbarkeit während der Konfiguration Aktualisierung sicherstellen, haben mindestens zwei Instanzen für jede Rolle. Dies ermöglicht ein virtueller Computer Clientanforderungen zu verarbeiten, während der andere aktualisiert wird. Weitere Informationen finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Cloud-Dienst ändern

Nach dem Öffnen der [Azure-Portal](https://portal.azure.com/), navigieren Sie zu Ihrem Cloud-Dienst. Hier verwalten Sie viele Aspekte. 

![Einstellungsseite](./media/cloud-services-how-to-configure-portal/cloud-service.png)

Die **Einstellungen** oder **Alle** Links öffnet Blatt **Einstellungen** wo Sie **Eigenschaften**ändern, ändern Sie die **Konfiguration**, **Zertifikate**, Setup **Warnregeln**verwalten und **Benutzer** mit Zugriff auf diese Cloud-Dienst verwalten.

![Azure-Cloud Einstellungen Blatt](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>Das Betriebssystem für den Clouddienst kann nicht geändert werden, mithilfe der **Azure-Portal**, ändern Sie nur diese Einstellung über das [klassische Azure-Portal](http://manage.windowsazure.com/). Diese detaillierte [hier](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).

## <a name="monitoring"></a>Überwachung

Sie können Ihre Cloud-Dienst Alerts hinzufügen. **Klicken Sie** > **Warnungsregeln** > **Benachrichtigung hinzufügen**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Hier können Sie eine Warnung einrichten. Mit der Dropdownliste **Mertic** können Sie eine Warnung für die folgenden Typen von Daten einrichten.

- Gelesen
- Schreiben
- Im Netzwerk
- Netzwerk
- CPU-Prozentsatz 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Konfigurieren der Überwachung von metrischen Kachel

Anstatt **Eigenschaften** > **Warnregeln**können auf einer metrischen Kacheln im Abschnitt **Überwachung** der **Cloud** Blatt klicken.

![Cloud-Dienst überwachen](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Hier können Sie passen Sie das Diagramm mit der Kachel verwendet oder eine Warnungsregel hinzufügen.


## <a name="reboot-reimage-or-remote-desktop"></a>Neustart neu abbilden oder Remotedesktop

Zu diesem Zeitpunkt kann nicht remote Desktop mithilfe der **Azure-Portal**konfigurieren. Allerdings können Sie es über das [Azure-Verwaltungsportal](cloud-services-role-enable-remote-desktop.md) [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)oder [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)einrichten. 

Klicken Sie zunächst auf die Cloud-Dienstinstanz.

![Cloud-Dienstinstanz](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Blatt geöffnet gefegt kann eine Remotedesktopverbindung initiieren, Remote-Neustart die Instanz oder Remote Image (mit einem erneuerten Abbild die Instanz starten).

![Cloud-Dienst Instanz Schaltflächen](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>Konfigurieren der .cscfg

Möglicherweise müssen Sie Clouddienst über [Service](cloud-services-model-and-package.md#cscfg) -Konfigurationsdatei (mithilfe). Zuerst müssen Sie die Datei .cscfg herunter, bearbeiten und hochladen.

1. Klicken Sie auf **das Symbol** oder den Link **Alle** **Einstellungen** Blatt geöffnet.

    ![Einstellungsseite](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. Klicken Sie auf **Konfiguration** .

    ![Konfiguration-Blade](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. Klicken Sie auf die Schaltfläche **Download** .

    ![Herunterladen](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. Nach dem Aktualisieren der Dienstkonfigurationsdatei hochladen und die Konfiguration installieren:

    ![Hochladen](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. Wählen Sie die Datei .cscfg, und klicken Sie auf **OK**.

            
## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie [einen Cloud-Dienst bereitgestellt](cloud-services-how-to-create-deploy-portal.md).
* Konfigurieren Sie einen [benutzerdefinierten Domänennamen](cloud-services-custom-domain-name-portal.md).
* [Verwalten der Cloud-Dienst](cloud-services-how-to-manage-portal.md).
* Konfigurieren von [Ssl-Zertifikaten](cloud-services-configure-ssl-certificate-portal.md).