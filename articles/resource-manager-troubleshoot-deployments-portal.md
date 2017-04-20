<properties
   pageTitle="Bereitstellungsvorgänge Portal anzeigen | Microsoft Azure"
   description="Beschreibt das Azure-Portal Ressourcenmanager Bereitstellung Fehler zu erkennen."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-portal"></a>Bereitstellungsvorgänge Azure-Portal anzeigen

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-API](resource-manager-troubleshoot-deployments-rest.md)

Sie können die Vorgänge für eine Bereitstellung über das Azure-Portal anzeigen. Möglicherweise interessiert Sie Fehler während der Bereitstellung erhalten haben, damit dieser Artikel konzentriert sich auf Vorgänge, die nicht die Vorgänge anzeigen. Das Portal bietet eine Schnittstelle, mit der Sie problemlos die Fehler und mögliche Updates ermitteln.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="use-deployment-operations-to-troubleshoot"></a>Verwenden Sie Bereitstellungsvorgänge Problembehandlung

Um Bereitstellungsvorgänge anzuzeigen, gehen Sie folgendermaßen vor:

1. Bei der Bereitstellung Ressourcengruppe Beachten Sie den Status der letzten Bereitstellung. Wählen Sie diesen Status um mehr zu erfahren.

    ![Bereitstellungsstatus](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. Die Geschichte der Bereitstellung wird angezeigt. Wählen Sie die Bereitstellung fehlgeschlagen.

    ![Bereitstellungsstatus](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. SELECT **fehlgeschlagen. Klicken Sie hier, um Details** , Beschreibung die Bereitstellung fehlschlagen. In der folgenden Abbildung ist der DNS-Eintrag nicht eindeutig.  

    ![Anzeigen von fehlgeschlagenen Weitergabe](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    Diese Fehlermeldung sollte für Sie mit der Problembehandlung beginnen. Benötigen Sie weitere Informationen über die Aufgaben abgeschlossen wurden jedoch können Vorgänge wie nachfolgend gezeigt.

4. Sie können alle Bereitstellung **Bereitstellung** Blatt anzeigen. Wählen Sie einen Vorgang, um weitere Details anzuzeigen.

    ![Vorgänge anzeigen](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    In diesem Fall sehen Sie, dass Speicherkonto, virtuelle Netzwerke und Verfügbarkeit Satz erstellt wurden. Konnte die öffentliche IP-Adresse und andere Ressourcen wurden nicht angewendet.

5. Auswählen von **Ereignissen**können Sie Ereignisse für die Bereitstellung anzeigen.

    ![Ereignisse anzeigen](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. Sie alle Ereignisse für die Bereitstellung und wählen Sie eine weitere Informationen.

    ![Ereignisse](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## <a name="use-audit-logs-to-troubleshoot"></a>Verwenden von Überwachungsprotokollen beheben

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Fehler bei einer Bereitstellung finden gehen Sie folgendermaßen vor:

1. Zeigen Sie die Audit-Protokolle für eine Ressourcengruppe durch Auswählen von **Audit-Protokolle an**.

    ![Wählen Sie Überwachungsprotokolle](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. Blatt **Überwachungsprotokolle** sehen Sie eine Zusammenfassung der letzten Operationen aller Ressourcengruppen in Ihrem Abonnement. Es enthält die Zeit grafisch und Status der Vorgänge sowie eine Liste der Vorgänge.

    ![Aktivitäten anzeigen](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. Sie können die Ansicht der Überwachungsprotokolle besonderen darauf filtern. Wählen Sie **Filter** am oberen Rand der **Überwachungsprotokolle** Blade.

    ![Filter-Protokolle](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. Wählen Sie **Filter** Blade Vorschriften beschränkt die Ansicht der Überwachungsprotokolle für diese Vorgänge zu sehen. Beispielsweise können Sie Vorgänge nur Fehler für die Ressourcengruppe anzeigen filtern.

    ![Set Filter-Optionen](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. Sie können die Vorgänge Filtern durch Festlegen eines Zeitraums. Das folgende Bild filtert die Ansicht auf einen bestimmten 20 Minuten Timespan.

    ![Zeit festlegen](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. Sie können die Vorgänge in der Liste auswählen. Wählen Sie die Operation, die den Fehler enthält, die, den Sie untersuchen möchten.

    ![Wählen Sie Vorgang](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. Sie sehen alle Ereignisse für diesen Vorgang. Beachten Sie die **Korrelations-IDS** in der Zusammenfassung. Diese ID wird verwendet, um Ereignisse zu verfolgen. Es kann hilfreich sein beim Arbeiten mit technischen Support, um ein Problem zu beheben. Sie können jedes Ereignis Details zum Ereignis auswählen.

    ![Ereignis auswählen](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Details zum Ereignis sehen. Insbesondere Achten Sie auf die **Eigenschaften** für Informationen über den Fehler.

    ![Audit Log-Details anzeigen](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

Im Überwachungsprotokoll angewendete Filter wird beim nächsten beibehalten, so Sie zum Ändern dieser Werte zum Erweitern der Ansicht der Vorgänge müssen anzeigen.

## <a name="next-steps"></a>Nächste Schritte

- Hilfe zu bestimmten Bereitstellungsfehler beheben finden Sie unter [beheben Sie häufige Fehler bei der Bereitstellung von Ressourcen in Azure mit Azure-Ressourcen-Manager](resource-manager-common-deployment-errors.md).
- Weitere Informationen zur Verwendung der Überwachungsprotokolle andere Aktionen überwachen, finden Sie unter [Vorgänge mit Ressourcen-Manager überwachen](resource-group-audit.md).
- Überprüfen Sie die Bereitstellung vor Ausführung finden Sie unter [Bereitstellen einer Ressourcengruppe Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).
