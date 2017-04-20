<properties 
    pageTitle="Phase der Bereitstellung einer Cloud-Dienst (Node.js) | Microsoft Azure" 
    description="Informationen Sie zum Ihre Azure-Anwendung in einer Stagingumgebung bereitstellen und dann auf einer produktionsumgebung VIP (Virtual IP) austauschen." 
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



# <a name="staging-an-application-in-azure"></a>Bereitstellen einer Anwendung in Azure

Eine Anwendung kann bereitgestellt werden, in die Stagingumgebung in Azure getestet werden, bevor Sie in dieser Umgebung verschieben in denen Anwendung auf das Internet zugegriffen werden kann. Die Stagingumgebung entspricht Produktionsumfeld, außer dass Sie nur die bereitgestellte Anwendung eine verschleierte URL zugreifen können, die von Azure generiert. Nachdem Sie überprüft haben, dass die Anwendung ordnungsgemäß funktioniert, kann es die produktionsumgebung bereitgestellt werden, anhand einer Swapkette VIP (Virtual IP).

> [AZURE.NOTE] Die Schritte in diesem Artikel gelten nur für Knoten Applikationen als Azure Cloud Service.

## <a name="step-1-stage-an-application"></a>Schritt 1: Bereitstellen einer Anwendung

Diese Aufgabe umfasst das Bereitstellen einer Anwendung mit **Microsoft Azure PowerShell**.

1.  Wenn Sie einen Dienst veröffentlichen, übergeben Sie einfach die **-Steckplatz** Parameter **Veröffentlichen-AzureServiceProject** -Cmdlet.

    ```powershell
    Publish-AzureServiceProject -Slot staging
    ```

2.  Das [klassische Azure-Portal] anmelden und **Cloud-Dienste**wählen. Cloud-Dienst erstellt und der Spalte Status **Staging** **ausgeführt**wurden, klicken Sie auf den Namen.

    ![Anzeigen eines ausgeführten Diensts Portal][cloud-service]

3.  Wählen Sie das **Dashboard**und dann **Bereitstellen**.

    ![Cloud-Dienst-dashboard][cloud-service-dashboard]

4. Beachten Sie den Wert im **Website-URL** -Eintrag nach rechts. Der DNS-Name ist eine verborgene interne ID, Azure generiert.

    ![Website-url][cloud-service-staging-url]

Nun können Sie überprüfen, dass die Anwendung korrekt in die Stagingumgebung arbeitet mit dem staging Website-URL.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Schritt 2: Aktualisieren einer Anwendung in der Produktion durch Auslagern VIPs

Nach der Überprüfung der aktualisierten Version der Anwendung in die Stagingumgebung machen Sie schnell in der Produktion verfügbar es virtuelle IPs (VIPs) Umgebung Staging- und austauschen.

> [AZURE.NOTE] Dieser Schritt setzt voraus, dass Sie bereits eine Anwendung für die Produktion bereitgestellt und die aktualisierte Version der Anwendung bereitgestellt.

1.  [Klassische Azure-Portal]melden Sie an auf **Clouddienste** und wählen Sie dann den Namen.

2.  **Dashboard**wählen Sie **Staging**und **tauschen** Sie am unteren Rand der Seite klicken. VIP-Swap-Dialogfeld wird geöffnet.

    ![VIP-Swap-Dialogfeld][vip-swap-dialog]

3.  Überprüfen Sie die Informationen und klicken Sie dann auf **OK**. Zwei Installationen beginnen wechselt staging-Bereitstellung für die Produktion und den Produktionsbetrieb wechselt zum Staging aktualisieren.

Erfolgreichen Bereitstellung bereitgestellt und ein Produktionsbetrieb durch Austausch VIPs durch Bereitstellung Staging aktualisiert.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Service-Aktualisierung durch Austausch VIPs in Azure Produktion bereitstellen]

[Azure-Verwaltungsportal]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Service-Aktualisierung durch Austausch VIPs in Azure Produktion bereitstellen]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
