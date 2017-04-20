<properties
    pageTitle="Staging-Umgebungen für Web-apps in Azure App Service einrichten"
    description="Erfahren Sie mehr über das bereitgestellte Veröffentlichung für Web-apps in Azure App Service verwenden."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/09/2016"
    ms.author="cephalin"/>

# <a name="set-up-staging-environments-for-web-apps-in-azure-app-service"></a>Staging-Umgebungen für Web-apps in Azure App Service einrichten
<a name="Overview"></a>

Beim Bereitstellen Ihrer Anwendung für [App Service](http://go.microsoft.com/fwlink/?LinkId=529714)können Sie ein eigenes Steckplatz statt standardmäßige Produktion Steckplatz im planmodus **Standard** oder **Premium** App Service bereitstellen. Bereitstellung Steckplätze sind tatsächlich live webapps mit eigenen Hostnamen. Web app Inhalte und Konfigurationen Elemente können zwischen zwei Bereitstellung, einschließlich Produktion Steckplatz ausgetauscht werden. Bereitstellen der Anwendung in einen Steckplatz Bereitstellung bietet folgende Vorteile:

- Sie können Web app ändert sich in einen Stagingbereich Bereitstellung Steckplatz vor Austausch mit der Produktion überprüfen.

- Web app zuerst in einen Steckplatz bereitstellen und Austausch in der Produktion wird sichergestellt, dass alle Instanzen des Feldes vor in Betrieb ausgetauscht erwärmt werden. Dadurch werden Ausfallzeiten beim Bereitstellen Ihrer Anwendung. Die Umleitung des Datenverkehrs ist nahtlos und keine Anfragen durch Swap-Vorgänge gelöscht. Diese gesamten Workflow automatisiert konfigurieren [Automatisch austauschen](#configure-auto-swap-for-your-web-app) , wenn vor Swap-Prüfung nicht benötigt wird.

- Nach einem Austausch mittlerweile mit zuvor bereitgestellte WebApp vorherigen Produktion Web app. In der Produktion Steckplatz getauscht Änderungen nicht wie erwartet, können ausführen derselbe Swap sofort zu der "letzten bekannten guten Site" zurück.

Jede App Service Plan unterstützt verschiedene Bereitstellung Steckplätze. Die Anzahl der Steckplätze Web app Modus unterstützt, finden Sie unter [App Preisen](/pricing/details/app-service/).

- Wenn Ihrer Anwendung mehrere Steckplätze hat, kann nicht den Modus ändern.

- Skalierung ist nicht für nicht-produktiven Slots verfügbar.

- Verknüpfte Ressourcen-Management ist für nicht-produktiven Steckplätze nicht unterstützt. Nur im [Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=529715) können Sie diese Auswirkung auf die Produktion Steckplatz durch vorübergehend nicht produktiven Steckplatz zu einem anderen App Service-Plan-Modus vermeiden. Beachten Sie, dass nicht produktiven Steckplatz wieder freigeben muss denselben Steckplatz Produktion vor zwei Steckplätze austauschen können.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

<a name="Add"></a>
## <a name="add-a-deployment-slot-to-a-web-app"></a>Eine Webanwendung einen Steckplatz Bereitstellung hinzufügen ##

Web app muss im **Standard** oder **Premium** nacheinander mehrere Bereitstellung Steckplätze aktivieren ausgeführt werden.

1. Öffnen Sie in [Azure-Portal](https://portal.azure.com/)Web app Blade.
2. **Klicken Sie**und klicken Sie dann auf **Bereitstellung Steckplätze**. **Bereitstellung Steckplätze** Blatt klicken Sie **Steckplatz hinzufügen**.

    ![Fügen Sie einen neuen Bereitstellung Steckplatz hinzu][QGAddNewDeploymentSlot]

    > [AZURE.NOTE]
    > Ist die Web app nicht bereits im **Standard** oder **Premium** , erhalten Sie eine Meldung mit den unterstützten Modi zum Aktivieren der Veröffentlichung bereitgestellte. An diesem Punkt können Sie auswählen, **Aktualisieren** und navigieren Sie zu der Registerkarte **Skalierung** Ihrer Anwendung fortfahren.

2. Blatt **hinzufügen einen Steckplatz** benennen Sie dem Steckplatz, und wählen Sie aus, ob Web app-Konfiguration von einem anderen vorhandenen Bereitstellung Steckplatz klonen. Klicken Sie auf Weiter.

    ![Konfigurationsquelle][ConfigurationSource1]

    Beim ersten hinzufügen einen Steckplatz, müssen nur zwei Optionen: Klon-Konfiguration aus dem Standard-Steckplatz in der Produktion oder überhaupt nicht.

    Nach der Erstellung mehrere Steckplätze können Konfiguration aus einem Steckplatz als in der Produktion Klonen Sie:

    ![Konfigurationsquellen][MultipleConfigurationSources]

5. Klicken Sie auf Bereitstellung Steckplatz öffnen eine-Blade für den Slot mit Metriken und Konfiguration wie andere Web-app Blatt **Bereitstellung Steckplätze** . **your-Web-App-Name-Deployment-Slot-Name** erscheint oben zu erinnern, dass Sie den Steckplatz Bereitstellung anzeigen.

    ![Bereitstellung Steckplatz Titel][StagingTitle]

5. Klicken Sie auf die app-URL in den Steckplatz Blade. Beachten Sie Bereitstellung Steckplatz einen eigenen Hostnamen und eine live-app. Um Zugang zu den Bereitstellung-Steckplatz beschränken, finden Sie unter [App Service Web App-Block Web Access nicht Produktionsbetrieb Steckplätze](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Es ist kein Inhalt nach Bereitstellung Steckplatz erstellen. Sie können im Steckplatz in einen anderen Zweig oder ein anderes Repository bereitstellen. Sie können auch die Slot-Konfiguration ändern. Verwenden Sie die zugeordneten Bereitstellung für Inhaltsupdates veröffentlichen Profil oder Bereitstellung Anmeldeinformationen.  Beispielsweise können Sie [in diesem Steckplatz Git veröffentlichen](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>
## <a name="configuration-for-deployment-slots"></a>Konfiguration für Bereitstellung Steckplätze ##
Klonen Konfiguration versetzt Bereitstellung wird die geklonte Konfiguration bearbeitet werden. Darüber hinaus führen einige Konfigurationselemente Inhalt über Swap (nicht Steckplatz bestimmte) während andere Konfigurationselemente im selben Steckplatz nach Austausch (bestimmten Steckplatz) bleiben. Den folgenden Listen sind der Konfigurations geändert wird, wenn Steckplätze austauschen.

**Standardeinstellungen, die ausgetauscht werden**:

- Allgemeine Einstellungen - wie Framework Version 32/64-Bit Web sockets
- App-Einstellungen (können konfiguriert werden zu einem Steckplatz)
- Verbindungszeichenfolgen (können konfiguriert werden zu einem Steckplatz)
- Handlerzuordnungen
- Überwachung und Diagnose settings
- Webaufträge Inhalt

**Eigenschaften, die nicht ausgetauscht werden**:

- Publishing-Endpunkte
- Benutzerdefinierte Domänennamen
- SSL-Zertifikate und Bindung
- Einstellungen
- Webaufträge Planer

Konfigurieren Sie eine app Einstellung oder Verbindungszeichenfolge an einem Steckplatz (nicht vertauscht) Blade **Einstellungen** für einen bestimmten Steckplatz, und klicken Sie dann das Kontrollkästchen Sie **Steckplatz Einstellung** für Konfigurationselemente, die den Steckplatz halten. Beachten Sie diese Markieren eines Konfigurationselements als bestimmten Steckplatz Einrichten dieses Element als nicht Swap auf alle die Bereitstellung der Webanwendung zugeordnet wird.

![Steckplatz-Einstellung][SlotSettings]

<a name="Swap"></a>
## <a name="to-swap-deployment-slots"></a>Bereitstellung Steckplätze austauschen ##

>[AZURE.IMPORTANT] Vor dem Ersetzen einer Web app Deployment-Steckplatz in der Produktion stellen Sie sicher, dass alle-Slot-Einstellungen wie gewünscht haben im Swap-Ziel konfiguriert sind.

1. Swap Bereitstellung Steckplätze klicken Sie **Swap** Web App in der Befehlszeile oder in der Befehlsleiste eine Bereitstellung Steckplatz. Stellen Sie sicher, dass die Swap Quell- und Swap ordnungsgemäß festgelegt sind. Normalerweise wäre das Swap-Ziel Produktions-Steckplatz.  

    ![Schaltfläche "austauschen"][SwapButtonBar]

3. Klicken Sie auf **OK** , um den Vorgang abzuschließen. Nach Abschluss des Vorgangs wurden die Bereitstellung Steckplätze vertauscht.

## <a name="configure-auto-swap-for-your-web-app"></a>Konfigurieren Sie für Ihre Web automatisch austauschen ##

Auto Swap rationalisiert DevOps Szenarien in Ihrer Anwendung Ausfallzeiten mit NULL Kaltstart Kunden Web App kontinuierlich bereitstellen soll. Bereitstellung Steckplatz konfiguriert automatisch Swap in die Produktion bei jedem die Code-Aktualisierung zu diesem Slot drücken, tauschen App Service Web app automatisch in die Produktion an, nachdem es bereits im Steckplatz erwärmt hat.

>[AZURE.IMPORTANT] Können automatische Swap für einen Steckplatz Achten Sie ist die Konfiguration der Speichersteckplätze genau das Ziel Steckplatz (normalerweise der Produktions-Steckplatz) zur Konfiguration.

Für einen Steckplatz konfigurieren Auto Swap ist einfach. Führen Sie die folgenden Schritte aus:

1. Blatt **Bereitstellung Steckplätze** wählen Sie nicht produktiven Steckplatz, klicken Sie auf **Alle Einstellungen** für diesen Steckplatz Blade.  

    ![][Autoswap1]

2. Klicken Sie auf **Anwendung**. **Auf** **Automatisch zu ersetzen**, wählen Sie den gewünschten Steckplatz in **Auto Swap Steckplatz**, und wählen Sie **Speichern** in der Befehlszeile. Sicherstellen Sie, dass Konfiguration für den Slot genau die Ziel-Steckplatz zur Konfiguration ist.

    Registerkarte **Benachrichtigungen** blinkt grün **Erfolg** , nachdem der Vorgang abgeschlossen ist.

    ![][Autoswap2]

    >[AZURE.NOTE] Um automatisch austauschen Ihrer Anwendung testen, können Sie zunächst nicht Produktion Steckplatz in **Auto Swap Steckplatz** mit dem Feature vertraut auswählen.  

3. Führen Sie einen Push Code zu diesem bereitstellungsslot. Auto Swap erfolgt nach kurzer Zeit und die Aktualisierung wird an den Ziel-Steckplatz URL.

<a name="Multi-Phase"></a>
## <a name="use-multi-phase-swap-for-your-web-app"></a>Multi-Phase Swap für Ihr Web app verwenden ##

Multi-Phase Swap steht Validierung im Kontext der Konfigurationselemente an einem Steckplatz wie Verbindungszeichenfolgen zu vereinfachen. In diesen Fällen kann es hilfreich sein diese Konfigurationselemente aus dem Swap Ziel Swap-Quelle und vor dem Swap tatsächlich wirksam überprüfen. Wenn Swap Ziel Konfigurationselemente Swap-Quelle zugewiesen wurden die verfügbaren Aktionen abschließen ersetzen oder Wiederherstellen der ursprünglichen Konfiguration für die Swap-Quelle aus dem Austausch Abbrechen verfügt. Beispiele für die mehrstufigen Swap zur Azure PowerShell-Cmdlets sind in Azure PowerShell-Cmdlets für Bereitstellung Steckplätze Abschnitt enthalten.

<a name="Rollback"></a>
## <a name="to-rollback-a-production-app-after-swap"></a>Rollback Produktions-app nach dem Austausch ##

Bei Fehlern in der Produktion nach Steckplatz Austausch Rollback der Steckplätze in ihren Zustand vor Austausch dieselben zwei Steckplätze sofort austauschen.

<a name="Warm-up"></a>
## <a name="custom-warm-up-before-swap"></a>Benutzerdefinierte Aufwärmphase vor Austausch ##

Einige apps können benutzerdefinierte Aufwärmphase Aktionen erforderlich. Konfigurationselement ApplicationInitialization in der Datei web.config können Sie an benutzerdefinierte Initialisierungsaktionen ausgeführt werden, bevor eine Anforderung empfangen wird. Der Austauschvorgang wartet diese benutzerdefinierten Aufwärmphase abgeschlossen. Hier folgt ein Beispielfragment web.config.

    <applicationInitialization>
        <add initializationPage="/" hostName="[web app hostname]" />
        <add initializationPage="/Home/About" hostname="[web app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>
## <a name="to-delete-a-deployment-slot"></a>So löschen Sie einen Deployment-Steckplatz##

Bereitstellung Steckplatz Blatt klicken Sie in der Befehlszeile **Löschen** .  

![Löschen Sie einen Bereitstellung Steckplatz][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>
##Azure PowerShell-Cmdlets für Bereitstellung Steckplätze

Azure PowerShell ist ein Modul, die Cmdlets zum Verwalten von Azure über Windows PowerShell, einschließlich der Unterstützung für die Verwaltung von Web app Deployment Steckplätze in Azure App Service bereitstellt.

- Informationen zur Installation und Konfiguration von Azure PowerShell und authentifizieren Azure PowerShell mit Azure-Abonnement finden Sie unter [Installieren und Konfigurieren von Microsoft Azure PowerShell](../powershell-install-configure.md).  

----------

### <a name="create-web-app"></a>WebApp erstellen

```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [web app name] -Location [location] -AppServicePlan [app service plan name]
```

----------

### <a name="create-a-deployment-slot-for-a-web-app"></a>Erstellen Sie einen Bereitstellung Steckplatz für eine Webanwendung

```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [web app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

----------

### <a name="initiate-multi-phase-swap-and-apply-target-slot-configuration-to-source-slot"></a>Initiieren von mehrstufigen Swap und Quelle Slot Slot Zielkonfiguration zuweisen

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="revert-the-first-phase-of-multi-phase-swap-and-restore-source-slot-configuration"></a>Die erste Phase des mehrstufigen Swap zurückgesetzt und Steckplatz Wiederherstellungskonfiguration

```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

----------

### <a name="swap-deployment-slots"></a>Swap-Bereitstellung Steckplätze

```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [web app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

----------

### <a name="delete-deployment-slot"></a>Bereitstellung Steckplatz löschen

```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [web app name]/[slot name] -ApiVersion 2015-07-01
```

----------

<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>
##Azure Befehlszeilenschnittstelle (CLI Azure) Befehle für Bereitstellung

Azure-CLI bietet plattformübergreifenden Befehle zum Arbeiten mit Azure, einschließlich der Unterstützung für die Verwaltung von Web App Deployment Steckplätze.

- Installation und Konfiguration von Azure CLI, einschließlich Informationen über Ihr Abonnement Azure Azure CLI Verbindung finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md).

-  Rufen Sie zum Auflisten der verfügbaren Befehle für Azure App Azure CLI `azure site -h`.

----------
### <a name="azure-site-list"></a>Azure Siteliste
Rufen Sie Informationen über die Web-apps in das aktuelle Abonnement **Azure Liste**wie im folgenden Beispiel.

`azure site list webappslotstest`

----------
### <a name="azure-site-create"></a>Azure-Website erstellen
Erstellen Sie einen Bereitstellung Steckplatz rufen Sie **Azure-Website erstellen** , und geben Sie den Namen einer vorhandenen Web App und den Namen des Feldes, wie im folgenden Beispiel erstellen.

`azure site create webappslotstest --slot staging`

Um Datenquellen-Steuerelement für den neuen Slot zu aktivieren, verwenden Sie die Option **--Git** wie im folgenden Beispiel.

`azure site create --git webappslotstest --slot staging`

----------
### <a name="azure-site-swap"></a>Azure Site austauschen
Zu aktualisierten Bereitstellung Steckplatz Produktions-app, Befehl **Azure Website austauschen** einer Swap durchführen, wie im folgenden Beispiel. Produktions-app wird keine Ausfallzeiten auftreten und werden einen Kaltstart.

`azure site swap webappslotstest`

----------
### <a name="azure-site-delete"></a>Azure Website löschen
Um eine Bereitstellung Steckplatz löschen, der nicht mehr benötigt wird, verwenden Sie den Befehl **Azure Site löschen** wie im folgenden Beispiel.

`azure site delete webappslotstest --slot staging`

----------

>[AZURE.NOTE] Wenn Sie mit Azure App Service beginnen, bevor Sie sich für ein Azure-Konto, gehen Sie [Versuchen App Service](http://go.microsoft.com/fwlink/?LinkId=523751)sofort eine kurzlebige Starter Web app in App Service können Sie erstellen. Keine Kreditkarten erforderlich; keine Zusagen.

## <a name="next-steps"></a>Nächste Schritte ##
[Azure App Service Web App-Block Web Access nicht Produktionsbetrieb Steckplätze](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)

[Microsoft Azure-Testversion](/pricing/free-trial/)

## <a name="whats-changed"></a>Was hat sich geändert
* Eine Anleitung zur Änderung von Websites zu App Service finden Sie unter: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png
 
