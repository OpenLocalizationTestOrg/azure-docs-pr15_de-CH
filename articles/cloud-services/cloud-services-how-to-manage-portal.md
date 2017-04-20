<properties 
    pageTitle="Cloud Service Managementaufgaben | Microsoft Azure" 
    description="Informationen Sie zum Clouddienste im Azure-Portal verwalten. Diese Beispiele verwenden Azure-Portal." 
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
    ms.date="08/02/2016"
    ms.author="adegeo"/>


# <a name="how-to-manage-cloud-services"></a>Cloud-Dienste verwalten

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-manage-portal.md)
- [Azure-Verwaltungsportal](cloud-services-how-to-manage.md)

Cloud-Dienst wird im Bereich **Cloud-Dienste (classic)** Azure-Portal. Dieser Artikel beschreibt einige häufige Aktionen würde beim Verwalten von Cloud-Diensten. Aktualisieren, löschen, Skalierung und fördern eine gestaffelte Bereitstellung für die Produktion einschließlich.

Weitere Informationen wie Ihre Cloud-Dienst steht [hier](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Gewusst wie: Aktualisieren einer Cloud-Rolle oder Bereitstellung

**Update** auf die Cloud-Dienst ggf. den Anwendungscode für den Clouddienst aktualisieren verwenden. Sie können eine einzelne Rolle oder allen Rollen aktualisieren. Um zu aktualisieren, können Sie ein neues Servicepaket oder Dienst hochladen.

1. Wählen Sie in [Azure-Portal][]Cloud-Dienst zu aktualisieren. Dadurch wird die Cloud Instanz Blatt geöffnet.

2. Blatt klicken Sie auf die Schaltfläche **Aktualisieren** .

    ![Schaltfläche "Aktualisieren"](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Aktualisieren Sie die Bereitstellung mit eine neue Service-Paket (.cspkg) und Service-Konfigurationsdatei (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Optional** aktualisiert Bereitstellung Beschriftung und das Speicherkonto. 

5. Haben alle Rollen nur eine Instanz der Rolle, wählen Sie die **Bereitstellung einer oder mehreren Rollen eine einzelne Instanz enthalten** ermöglichen die Aktualisierung fortgesetzt. 

    Azure garantieren nur 99,95 % Verfügbarkeit während ein Cloud-Update, hat jede Rolle mindestens zwei Instanzen (virtuelle Maschinen). Mit zwei Rolleninstanzen verarbeitet ein virtueller Computer Clientanforderungen, während der andere aktualisiert wird.

6. Überprüfen Sie **installieren starten** , um das Update angewendet, nachdem der Upload des Pakets abgeschlossen ist.

7. Klicken Sie auf **OK** , um den Dienst aktualisieren beginnen.



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Gewusst wie: Bereitstellung eine gestaffelte Bereitstellung für die Produktion zu tauschen

Wenn Sie eine neue Version eines Cloud-Diensts Stufe bereitstellen und Testen der neuen Version im Cloud-Dienst Stagingumgebung entscheiden. Verwenden Sie URLs wechseln, zwei Installationen werden und fördern neue Freigabe zur Produktion, **austauschen** . 

Bereitstellung von **Cloud-** Seite oder das Dashboard tauschen.

1. Wählen Sie in [Azure-Portal][]Cloud-Dienst zu aktualisieren. Dadurch wird die Cloud Instanz Blatt geöffnet.

2. Blatt klicken Sie **austauschen** .

    ![Cloud-Dienste austauschen](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Die folgende Aufforderung zur Bestätigung wird angezeigt.

    ![Cloud-Dienste austauschen](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Für die Bereitstellung zu überprüfen, klicken Sie auf **OK** Swap Bereitstellungen.

    Bereitstellung Swap geschieht schnell, da ändert lediglich die virtuellen IP-Adressen (VIPs) ist für die Bereitstellung.

    Compute Kostengründen können staging-Bereitstellung löschen Sie nach dem überprüfen die Bereitstellung in der Produktion wie erwartet funktioniert.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Gewusst wie: verknüpfen eine Ressource auf einen Clouddienst

Azure-Portal werden Ressourcen nicht miteinander verknüpfen, wie der aktuelle Azure-Verwaltungsportal. Stellen Sie stattdessen zusätzliche Ressourcen der gleichen Ressourcengruppe von Cloud-Dienst verwendet wird.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Gewusst wie: Bereitstellung und Cloud-Dienst löschen

Cloud-Dienst löschen zu können, müssen Sie jede vorhandene Bereitstellung löschen.

Compute Kostengründen können staging-Bereitstellung löschen Sie nach dem überprüfen die Bereitstellung in der Produktion wie erwartet funktioniert. Sie werden für computekosten für bereitgestellte Rolleninstanzen berechnet, die beendet wurden.

Gehen Sie zum Löschen einer Bereitstellung oder dem Cloud-Dienst. 

1. Wählen Sie in [Azure-Portal][]Cloud-Dienst, den Sie löschen möchten. Dadurch wird die Cloud Instanz Blatt geöffnet.

2. Blatt klicken Sie auf die Schaltfläche **Löschen** .

    ![Cloud-Dienste austauschen](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Löschen den gesamten Clouddienst **Cloud-Dienst** und seine Bereitstellung oder wählen Sie die **Bereitstellung in der Produktion** oder **Staging-Bereitstellung**.

    ![Cloud-Dienste austauschen](./media/cloud-services-how-to-manage-portal/delete-blade.png) 

4. Klicken Sie auf die Schaltfläche **Löschen** am unteren.

5. Klicken Sie Cloud-Dienst **Cloud-Dienst löschen**. Um Bestätigung gebeten werden, klicken Sie auf **Ja**.

> [AZURE.NOTE]
> Bei ein Clouddienst gelöscht und ausführliche Überwachung konfiguriert ist, müssen Sie die Daten aus Ihrem Speicherkonto manuell löschen. Informationen über die Metriken Tabellen finden Sie [in diesem](cloud-services-how-to-monitor.md) Artikel.

[Azure-portal]: https://portal.azure.com

## <a name="next-steps"></a>Nächste Schritte

* [Konfiguration des Cloud-Dienst](cloud-services-how-to-configure-portal.md).
* Erfahren Sie, wie [einen Cloud-Dienst bereitgestellt](cloud-services-how-to-create-deploy-portal.md).
* Konfigurieren Sie einen [benutzerdefinierten Domänennamen](cloud-services-custom-domain-name-portal.md).
* Konfigurieren von [Ssl-Zertifikaten](cloud-services-configure-ssl-certificate-portal.md).