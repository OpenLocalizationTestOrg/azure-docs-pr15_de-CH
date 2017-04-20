<properties 
    pageTitle="So konfigurieren Sie einen Cloud-Dienst (klassische Portal) | Microsoft Azure" 
    description="Informationen Sie zum Cloud-Services in Azure konfigurieren. Informationen Sie zum Cloud-Dienstkonfiguration aktualisieren und Konfigurieren des Remotezugriffs Rolleninstanzen." 
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

Sie können die am häufigsten verwendeten für einen Clouddienst im klassischen Azure-Portal konfigurieren. Oder möchten Sie die Konfigurationsdateien direkt aktualisieren, downloaden Dienstkonfigurationsdatei zu aktualisieren, und Laden Sie die aktualisierte Datei und Cloud-Dienst mit der Konfiguration zu aktualisieren. In beiden Fällen werden konfigurationsupdates alle Rolleninstanzen herausgedrückt.

Azure-Verwaltungsportal kann [Remotedesktopverbindung für eine Rolle in Azure Cloud Services](cloud-services-role-enable-remote-desktop.md) aktivieren

Azure kann nur 99,95 % Verfügbarkeit während der Konfiguration Aktualisierung sicherstellen, haben mindestens zwei Instanzen für jede Rolle. Dies ermöglicht ein virtueller Computer Clientanforderungen zu verarbeiten, während der andere aktualisiert wird. Weitere Informationen finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Cloud-Dienst ändern

1. Klicken Sie im [klassischen Azure-Portal](http://manage.windowsazure.com/)auf **Clouddienste**klicken Sie auf den Namen des Cloud-Dienst, und klicken Sie auf **Konfigurieren**.

    ![Seite "Konfiguration"](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage1.png)
    
    Auf der Seite **Konfigurieren** Sie Überwachung konfigurieren, Einstellungen aktualisieren und Gastbetriebssystem und Familie Instanzen auswählen. 

2. **Überwachung**legen Sie die Überwachungsstufe ausführlich oder minimale und konfigurieren Sie Diagnose Verbindungszeichenfolgen, die für ausführliche Überwachung.

3. Aktualisieren Sie unter Rollen (gruppiert nach Rolle) Folgendes:
    
    >**Einstellungen**  
    >Ändern Sie die Werte für verschiedene Konfigurationen, die in den Elementen *ConfigurationSettings* Dienstkonfigurationsdatei (.cscfg) angegeben werden.
    >
    >**Zertifikate**  
    >Ändern Sie den Fingerabdruck des Zertifikats, der SSL-Verschlüsselung für eine Rolle verwendet wird. Um ein Zertifikat zu ändern, müssen Sie zuerst das neue Zertifikat (auf der Seite **Zertifikate** ) hochladen. Dann aktualisieren Sie den Fingerabdruck des Zertifikats Zeichenfolge in den Einstellungen angezeigt.

4. Im **Betriebssystem**können Sie ändern das Betriebssystem oder die Version für Instanzen oder **Automatische** So aktivieren Sie automatische Updates Version des aktuellen Betriebssystems wählen. Die Standardeinstellungen des Betriebssystems Webrollen und Worker-Rollen zuweisen, aber wirken sich nicht auf virtuelle Computer.

    Während der Bereitstellung die aktuelle Betriebssystemversion auf alle Instanzen installiert und die Betriebssysteme werden standardmäßig automatisch aktualisiert. 
    
    Benötigen Sie für den Cloud-Dienst auf ein anderes Betriebssystemversion aufgrund Kompatibilität im Code ausführen, können Sie ein Betriebssystem und Version. Wenn Sie eine bestimmte Betriebssystemversion auswählen, werden automatische Betriebssystemupdates für Cloud-Dienst angehalten. Sie müssen sicherstellen, dass die Betriebssysteme Updates erhalten.
    
    Wenn Sie Ihre apps mit den neuesten Betriebssystems haben alle Kompatibilitätsprobleme beheben, können Sie automatische Betriebssystemupdates aktivieren, Version des Betriebssystems **automatisch**festlegen. 
    
    ![OS Settings](./media/cloud-services-how-to-configure/CloudServices_ConfigurePage_OSSettings.png)

5. Klicken Sie auf **Speichern**, um die Konfiguration zu speichern und den Rolleninstanzen drücken. (Klicken Sie auf die Änderungen **verwerfen** .) **Speichern** und **Löschen** werden nach einer Änderung der Befehlsleiste hinzugefügt.

## <a name="update-a-cloud-service-configuration-file"></a>Aktualisieren einer Cloud Service-Konfigurationsdatei

1. Herunterladen einer Cloud Service Konfigurationsdatei (.cscfg) mit der aktuellen Konfiguration. Klicken Sie auf der Seite **Konfigurieren** für den Clouddienst **herunterladen**. Dann klicken Sie auf **Speichern**oder **Speichern als** Datei speichern klicken.

2. Nach dem Aktualisieren der Dienstkonfigurationsdatei hochladen und die Konfiguration installieren:

    1. Klicken Sie auf **Hochladen**, auf der Seite **Konfigurieren** .
    
        ![Konfiguration laden](./media/cloud-services-how-to-configure/CloudServices_UploadConfigFile.png)
    
    2. Verwenden Sie in der **Konfigurationsdatei**können Sie die aktualisierte .cscfg **Durchsuchen** .
    
    3. Wenn Ihre Cloud-Dienst Rollen, die nur eine Instanz haben enthält, aktivieren Sie das Kontrollkästchen **Konfiguration auch anwenden, wenn eine oder mehrere Rollen eine einzelne Instanz enthalten** konfigurationsupdates für die Rollen Fortfahren aktivieren.
    
        Azure kann nicht, wenn Sie mindestens zwei Instanzen jeder Rolle definieren, garantieren mindestens 99,95 % Verfügbarkeit des Cloud-Diensts während Service Configuration-Updates. Weitere Informationen finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).
    
    4. Klicken Sie auf **OK** (Häkchen). 


## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie [einen Cloud-Dienst bereitgestellt](cloud-services-how-to-create-deploy.md).
* Konfigurieren Sie einen [benutzerdefinierten Domänennamen](cloud-services-custom-domain-name.md).
* [Verwalten der Cloud-Dienst](cloud-services-how-to-manage.md).
* [Aktivieren Sie Remotedesktop-Verbindung für eine Rolle in Azure Cloud Services](cloud-services-role-enable-remote-desktop.md)
* Konfigurieren von [Ssl-Zertifikaten](cloud-services-configure-ssl-certificate.md).
