<properties 
    pageTitle="Azure-Portal Azure Ressourcen nutzen | Microsoft Azure" 
    description="Verwenden Sie Azure-Portal und Azure Ressource verwalten die Ressourcen." 
    services="azure-resource-manager,azure-portal" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="timlt" 
    editor="tysonn"/>

<tags 
    ms.service="azure-resource-manager" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a>Bereitstellen von Ressourcen mit Ressourcen-Manager und Azure-portal

> [AZURE.SELECTOR]
- [PowerShell](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-API](resource-group-template-deploy-rest.md)

In diesem Thema veranschaulicht, wie mit [Azure-Portal](https://portal.azure.com) mit [Azure Ressourcenmanager](azure-resource-manager/resource-group-overview.md) Azure Ressourcen bereitstellen. Informationen zum Verwalten von Ressourcen finden Sie unter [Verwalten von Azure Ressourcen-Portal](./azure-portal/resource-group-portal.md).

Derzeit unterstützt nicht alle Service Portal oder Ressourcen-Manager. Für diese Dienste müssen Sie das [Verwaltungsportal](https://manage.windowsazure.com). Der Status jedes Diensts finden Sie unter [Azure Portal Verfügbarkeitsdiagramm](https://azure.microsoft.com/features/azure-portal/availability/).

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

1. Um eine leere Ressourcengruppe zu erstellen, wählen Sie **neu** > **Management** > **Ressourcengruppe**.

    ![leere Ressourcengruppe erstellen](./media/resource-group-template-deploy-portal/create-empty-group.png)

2. Geben sie einen Namen und Speicherort, und wählen Sie ein Abonnement. Sie müssen einen Speicherort für die Ressourcengruppe bereitstellen, da die Ressourcengruppe Metadaten Ressourcen gespeichert. Aus Gründen der Kompatibilität sollten Sie angeben, wo die Metadaten gespeichert ist. Es wird empfohlen, dass Sie einen Speicherort angeben, in dem die Ressourcen befinden. Mit demselben Speicherort kann Ihre Vorlage vereinfachen.

    ![Werte festlegen](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a>Ressourcen von Marketplace

Nachdem Sie eine Ressourcengruppe erstellen, können Sie Ressourcen, aus dem Markt bereitstellen. Der Markt bietet vordefinierte Lösungen für Szenarien.

1. Wählen Sie zum start einer Bereitstellung **neu** und den Typ der Ressource, die Sie bereitstellen möchten. Suchen Sie dann die bestimmte Version der Ressource, die Sie bereitstellen möchten.

    ![Bereitstellen von Ressourcen](./media/resource-group-template-deploy-portal/deploy-resource.png)

2. Wenn bereitstellen möchte bestimmte Lösung nicht angezeigt wird, können Sie den Markt suchen.

    ![Suche-Markt](./media/resource-group-template-deploy-portal/search-resource.png)

3. Je nach ausgewählten Ressource müssen Sie eine Auflistung der relevanten Eigenschaften vor der Bereitstellung festlegen. Diese Optionen werden nicht hier angezeigt sie Ressourcentyp abhängig. Für alle Typen müssen Sie eine Ziel-Ressourcengruppe auswählen. Die folgende Abbildung zeigt das Web app erstellen und Bereitstellen der Ressourcengruppe, die Sie erstellt haben.

    ![Ressourcengruppe erstellen](./media/resource-group-template-deploy-portal/select-existing-group.png)

    Alternativ können Sie beim Bereitstellen von Ressourcen eine Ressourcengruppe erstellen. Wählen Sie **neu erstellen** und benennen Sie der Ressourcengruppe aus.

    ![neue Ressourcengruppe erstellen](./media/resource-group-template-deploy-portal/select-new-group.png)

4. Die Bereitstellung beginnt. Die Bereitstellung kann einige Minuten dauern. Wenn die Bereitstellung abgeschlossen ist, wird eine Benachrichtigung angezeigt.

    ![Benachrichtigung anzeigen](./media/resource-group-template-deploy-portal/view-notification.png)

5. Nach dem Bereitstellen von Ressourcen, können weitere Ressourcen der Ressourcengruppe hinzufügen Befehl auf dem Blatt Ressource **Hinzufügen** .

    ![Ressource hinzufügen](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a>Bereitstellen von Ressourcen aus benutzerdefinierten Vorlage

Möchten Sie Ausführen einer Bereitstellung jedoch keine Vorlagen auf dem Markt, kann eine benutzerdefinierte Vorlage erstellen, die die Infrastruktur für die Lösung definiert. Zum Erstellen von Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen](resource-group-authoring-templates.md).

1. Zum Bereitstellen einer benutzerdefinierten Vorlage über das Portal **neu**und für **Bereitstellung** suchen Sie, bis Sie die Optionen auswählen können.

    ![Bereitstellung von Suchen](./media/resource-group-template-deploy-portal/search-template.png)

2. **Bereitstellung** von den verfügbaren Ressourcen auswählen

    ![Bereitstellung auswählen](./media/resource-group-template-deploy-portal/select-template.png)

3. Starten der vorlagenbereitstellung öffnen Sie die leere Vorlage zur Anpassung.

    ![Vorlage erstellen](./media/resource-group-template-deploy-portal/show-custom-template.png)

    Fügen Sie im Editor die JSON-Syntax, die Ressourcen definiert, die Sie bereitstellen möchten. Wählen Sie **Speichern** . Anleitung zum Schreiben von JSON-Syntax finden Sie unter [Exemplarische Vorgehensweise Ressourcenmanager Vorlage](resource-manager-template-walkthrough.md).

    ![Vorlage bearbeiten](./media/resource-group-template-deploy-portal/edit-template.png)

4. Oder [Azure Schnellstart Vorlagen](https://azure.microsoft.com/documentation/templates/)eine bereits vorhandene Vorlage auswählen. Diese Vorlagen werden von der Community bereitgestellt. Sie viele Standardszenarien behandeln und jemand habe eine Vorlage ähnelt, was Sie bereitstellen möchten. Sie können Vorlagen um etwas zu finden, der Ihr Szenario entspricht.

    ![Schnellstart-Vorlage wählen](./media/resource-group-template-deploy-portal/select-quickstart-template.png)

    Sie können die ausgewählte Vorlage im Editor anzeigen.

5. Wählen Sie nach, alle anderen Werte, **Erstellen** die Vorlage bereitgestellt. 

    ![Vorlage bereitstellen](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a>Bereitstellen von Ressourcen aus einer Vorlage in Ihrem Konto gespeichert

Das Portal können Sie Ihre Azure-Konto eine Vorlage speichern und später erneut. Weitere Informationen zum Arbeiten mit diesen Vorlagen [beginnen mit privaten Vorlagen Azure-Portal](./marketplace-consumer/mytemplates-getstarted.md)gespeichert.

1. Gespeicherten Vorlagen wählen Sie **Durchsuchen,** > **Vorlagen**.

    ![Vorlagen durchsuchen](./media/resource-group-template-deploy-portal/browse-templates.png)

2. Wählen Sie aus der Liste der Vorlagen in Ihrem Konto gespeichert, die Sie bearbeiten möchten.

    ![gespeicherte Vorlagen](./media/resource-group-template-deploy-portal/saved-templates.png)

3. Wählen Sie **Deploy** gespeicherte Vorlage erneut bereitstellen.

    ![gespeicherte Vorlage bereitstellen](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a>Nächste Schritte

- Überwachungsprotokolle finden Sie unter [Überwachen von Vorgängen mit Ressourcen-Manager](resource-group-audit.md).
- Um Fehler der Bereitstellung finden Sie unter [Problembehandlung Ressource Gruppe Deployments mit Azure-Portal](resource-manager-troubleshoot-deployments-portal.md).
- Rufen Sie eine Vorlage aus einer Bereitstellung oder Ressourcengruppe finden Sie unter [Exportieren von Azure Ressourcenmanager Vorlage von vorhandenen Ressourcen](resource-manager-export-template.md).





