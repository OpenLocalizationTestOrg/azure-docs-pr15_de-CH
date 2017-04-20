<properties
    pageTitle="Bereitstellung und Microservices vorhersehbar in Azure bereitstellen"
    description="Informationen Sie zum Bereitstellen einer Anwendung in Azure App Service als Einheit und vorhersagbare mit JSON Gruppe Ressourcenvorlagen PowerShell scripting Microservices bestehen."
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
    ms.date="01/06/2016"
    ms.author="cephalin"/>


# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Bereitstellung und Microservices vorhersehbar in Azure bereitstellen #

Dieses Lernprogramm veranschaulicht die Bereitstellung und Bereitstellen einer Anwendung bestehend aus [Microservices](https://en.wikipedia.org/wiki/Microservices) in [Azure App Service](/services/app-service/) als Einheit und vorhersagbare mit JSON Gruppe Ressourcenvorlagen PowerShell-Skripting. 

Bei Bereitstellung und Bereitstellung von entkoppelt sehr bestehen stark-Applikationen sind Microservices, Wiederholbarkeit und Vorhersagbarkeit Erfolg. [Azure App Service](/services/app-service/) können Sie Microservices erstellen, die Web-apps, mobile apps, API-apps und Logik apps. [Azure-Ressourcen-Manager](../azure-resource-manager/resource-group-overview.md) können Sie alle Microservices als eine Einheit mit Resource Dependencies wie Datenbank verwalten und Quell-Einstellungen. Jetzt können Sie eine solche Anwendung mit JSON-Vorlagen und einfache PowerShell Skripts bereitstellen. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-do"></a>Vorgehensweise ##

Im Lernprogramm werden Sie eine Anwendung bereitstellen, die enthält:

-   Zwei web apps (d. h. zwei Microservices)
-   Back-End-SQL-Datenbank
-   App-Einstellungen, Verbindungszeichenfolgen und Versionskontrolle
-   Anwendung Einblicke Alarmen, Skalierung settings

## <a name="tools-you-will-use"></a>Verwenden Sie Tools ##

In diesem Lernprogramm werden die folgenden Tools verwenden. Da es nicht umfassende Diskussion Tools, werde das End-to-End-Szenario und Ihnen nur eine kurze Einführung, und wo finden Sie weitere Informationen. 

### <a name="azure-resource-manager-templates-json"></a>Azure Ressourcenmanager Vorlagen (JSON) ###
 
Erstellen eine Webanwendung in Azure App Service verwendet beispielsweise Azure Ressourcenmanager JSON-Vorlage mit den Ressourcen die gesamten Ressourcengruppe erstellen. Eine komplexe Vorlage [Azure Marketplace](/marketplace) wie [Skalierbare WordPress](/marketplace/partners/wordpress/scalablewordpress/) app der MySQL-Datenbank, Speicherkonten App Serviceplan Web app, kann Warnregeln, app-Einstellungen Autoscale Einstellungen und mehr, und alle Vorlagen sind PowerShell verfügbar. Informationen zu diesen Vorlagen finden Sie unter [Mithilfe von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).

Informationen auf Azure-Ressourcen-Manager finden Sie unter [Erstellen von Azure Ressourcenmanager Vorlagen](../resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Azure SDK 2.6 für Visual Studio ###

Das neueste SDK enthält verbesserte Unterstützung der Ressourcen-Manager im JSON-Editor. Sie können können schnell eine Gruppenvorlage Ressource erstellen oder Öffnen einer JSON-Vorlage (z. B. eine Vorlage heruntergeladen Gallery) geändert, die Datei füllen und sogar die Ressourcengruppe direkt aus einer Ressourcengruppe Azure-Lösung bereitstellen.

Weitere Informationen finden Sie unter [Azure SDK 2.6 für Visual Studio](/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure PowerShell 0.8.0 oder höher ###

Ab Version 0.8.0 beinhaltet die Installation Azure PowerShell Azure Resource Manager-Modul neben der Azure-Modul. Das neue Modul ermöglicht das Skript für der Bereitstellung von Ressourcengruppen.

Weitere Informationen finden Sie unter [Verwendung von Azure PowerShell mit Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Azure-Ressourcen-Explorer ###

Diese [Vorschau](https://resources.azure.com) können Sie die JSON-Definitionen aller Ressourcengruppen in Ihrem Abonnement und die einzelnen Ressourcen zu. Im Tool können Sie die JSON Definitionen einer Ressource, gesamte Hierarchie Ressourcen löschen und Ressourcen.  Die Informationen in diesem Tool verfügbar ist sehr hilfreich für Vorlage erstellen, weil es Eigenschaften zeigt für einen bestimmten Typ von Ressource die richtigen Werte usw. müssen. Sie können auch der Ressourcengruppe in [Azure-Portal](https://portal.azure.com/)erstellen dann prüfen die JSON-Definitionen im Explorer-Tool können Sie die Ressourcengruppe nicht als Vorlage dienen.

### <a name="deploy-to-azure-button"></a>Azure Schaltfläche bereitstellen ###

Verwenden Sie GitHub für Datenquellen-Steuerelement, setzen Sie in der Infodatei [Azure Schaltfläche bereitstellen](/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) . MD schlüsselfertige Bereitstellung Benutzeroberfläche Azure ermöglicht. Dabei für alle einfachen WebApp können, können Sie dies ermöglichen eine gesamte Ressourcengruppe mit einer azuredeploy.json-Datei in das Repository-Stammverzeichnis bereitstellen erweitern. Diese JSON-Datei enthält die Gruppenvorlage Ressource wird die Ressourcengruppe Erstellen von Azure Schaltfläche bereitstellen verwendet werden. Ein Beispiel finden Sie [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) , das in diesem Lernprogramm verwenden.

## <a name="get-the-sample-resource-group-template"></a>Ressource Gruppenvorlage abrufen ##

Jetzt gleich zu.

1.  Navigieren Sie zu der [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) App Beispiel.

2.   Readme.md klicken Sie auf **in Azure bereitstellen**.
 
3.  Sie sind der Website [bereitstellen, Azure](https://deploy.azure.com) und Bereitstellung Parameter aufgefordert. Beachten Sie, dass die meisten Felder mit Namen des Repository und einige zufälligen Zeichenfolgen gefüllt werden. Sie können alle Felder ändern, wenn Sie jedoch das einzige, was eingegeben werden den administrativen SQL Server-Benutzernamen und das Kennwort und dann auf **Weiter**.
 
    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)

4.  Klicken Sie **Bereitstellen** , um den Bereitstellungsprozess zu starten. Sobald der Prozess bis zum Abschluss ausgeführt wird, klicken Sie auf http://todoapp*XXXX*. *.azurewebsites.NET Link bereitgestellte Anwendung durchsuchen. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)

    Die Benutzeroberfläche würde etwas langsam sein, wenn Sie zuerst zu durchsuchen, da die apps nur gestartet werden, aber eine voll funktionsfähige Anwendung überzeugen.

5.  Klicken Sie in der Seite bereitstellen **Verwalten** , die neue Anwendung in Azure-Portal.

6.  Klicken Sie in der Dropdownliste **Essentials** auf den Link Ressource. Beachten Sie, dass Web app bereits GitHub Repository unter **Externes Projekt**verbunden ist. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
 
7.  Ressource Gruppe Blatt Beachten Sie, dass bereits zwei webapps und eine SQL-Datenbank in der Ressourcengruppe.

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)
 
Alles, was Sie gerade gesehen in wenigen Minuten eine vollständig bereitgestellte zwei Microservice-Anwendung mit alle Komponenten, Abhängigkeiten, Einstellungen, Datenbank und kontinuierliche Veröffentlichung von einer automatisierten Orchestrierung in Azure-Ressourcen-Manager eingerichtet ist. Dies erfolgte durch zwei Dinge:

-   Azure Schaltfläche bereitstellen
-   azuredeploy.JSON im Stammverzeichnis repo

Sie können diese Anwendung bereitstellen Dutzende, Hunderte oder Tausende Male und genaue die gleiche Konfiguration jedes Mal. Die Wiederholbarkeit und die Vorhersagbarkeit dieses Ansatzes können Sie stark Applikationen mit Leichtigkeit und Zuversicht bereitstellen.

## <a name="examine-or-edit-azuredeployjson"></a>Überprüfen oder bearbeiten AZUREDEPLOY. JSON ##

Jetzt sehen wir uns wie GitHub Repository eingerichtet wurde. Sie werden mit dem JSON-Editor in Azure .NET SDK also wenn bereits [Azure .NET SDK 2.6](/downloads/)installiert haben getan.

1.  Klonen Sie [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) Repository mit Ihrem bevorzugten Git Tool. Im Screenshot unten mache ich das in Team Explorer in Visual Studio 2013.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)

2.  Das Repository-Stammverzeichnis öffnen Sie azuredeploy.json in Visual Studio. Wenn JSON Gliederungsfenster nicht angezeigt wird, müssen Sie Azure .NET SDK installieren.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Werde ich nicht jedes Detail im JSON-Format beschreiben, aber [Mehr](#resources) Ressourcenabschnitt verbindet die Ressource Gruppe Vorlage Sprache. Hier werde nur ich Ihnen zeigen, die interessanten Funktionen, die Sie beginnen dabei eine eigene Vorlage für die Bereitstellung.

### <a name="parameters"></a>Parameter ###

Sehen Sie im Parameterabschnitt zu sehen, dass die meisten dieser Parameter wie die Schaltfläche **Bereitstellen in Azure** zur Eingabe aufgefordert. Die Website hinter die Schaltfläche **Bereitstellen in Azure** füllt Input Benutzeroberfläche mit den Parametern, die in azuredeploy.json definiert. Diese Parameter werden in die Ressourcendefinitionen wie Ressourcennamen Eigenschaftswerte usw. verwendet.

### <a name="resources"></a>Ressourcen ###

Im Ressourcenknoten sehen 4 der obersten Ebene Ressourcen definiert werden, einschließlich einer SQL Server-Instanz, eine App Service-Plan und zwei webapps. 

#### <a name="app-service-plan"></a>App Service-plan ####

Beginnen Sie mit einer einfachen auf Stammebene in JSON. Klicken Sie in der Gliederung JSON App Service namens **[HostingPlanName]** , markieren Sie den entsprechenden JSON-Code. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Beachten Sie, dass die `type` -Element gibt die Zeichenfolge für eine App Service-Plan (hieß eine Serverfarm vor langer Zeit), und andere Elemente und Eigenschaften gefüllt werden mit den Parametern in der JSON-Datei definiert und diese Ressource nicht geschachtelten Ressourcen haben.

>[AZURE.NOTE] Beachten Sie auch, dass den Wert der `apiVersion` teilt Azure, welche Version von REST-API mit JSON Ressourcendefinition mit und die Formatierung der Ressource beeinflussen innerhalb, der `{}`. 

#### <a name="sql-server"></a>SQL Server ####

Klicken Sie auf die SQL Server-Ressource **SQLServer** JSON Gliederung.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)
 
Beachten Sie Folgendes zu der hervorgehobenen JSON-Code:

-   Die Verwendung von Parametern wird sichergestellt, dass die erstellten Ressourcen benannt und in einer Weise, die sie mit anderen konfiguriert.
-   Die SQL Server-Ressource zwei geschachtelte Ressourcen hat jede hat einen anderen Wert für `type`.
-   Geschachtelten Ressourcen `“resources”: […]`, in dem die Datenbank und die Firewall-Regeln definiert sind, haben eine `dependsOn` Element, das die Ressourcen-ID der Stammebene SQLServer Ressource angibt. Dies weist Azure-Ressourcen-Manager "vor der Erstellung dieser Ressource anderen Ressource bereits vorhanden sein; und wenn die Ressource in der Vorlage definiert ist, dann erstellen, zuerst".

    >[AZURE.NOTE] Ausführliche Informationen zur Verwendung der `resourceId()` finden Sie im [Azure Ressourcenmanager Vorlage Funktionen](../resource-group-template-functions.md).

-   Die Wirkung der `dependsOn` ist, dass wissen Azure Ressourcenmanager Ressourcen gleichzeitig erstellt werden können und welche Ressourcen sequenziell erstellt werden müssen. 

#### <a name="web-app"></a>WebApp ####

Jetzt gehen wir zu den tatsächlichen Web-apps, die komplizierter sind. Klicken Sie auf [variables('apiSiteName')] Web app JSON-Gliederung, um die JSON-Code hervorheben. Sie werden feststellen, dass Dinge interessanter. Zu diesem Zweck werde ich über die Funktionen einzeln sprechen:

##### <a name="root-resource"></a>Stammverzeichnis #####

Web app abhängig zwei unterschiedliche Ressourcen. Dies bedeutet, dass Azure Resource Manager Web app erstellen erst App Service-Plan und die SQL Server-Instanz erstellt werden.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>App-Einstellungen #####

App-Einstellungen werden auch als geschachtelte Ressource definiert.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

In der `properties` -Element `config/appsettings`, Sie haben zwei app-Einstellungen im Format `“<name>” : “<value>”`.

-   `PROJECT`ist eine [KUDU Einstellung](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) , der Azure-Bereitstellung welches Projekt in einer Visual Studio-Projektmappe mit mehreren Projekten verwendet wird. Erfahren Sie später Datenquellen-Steuerelement konfiguriert ist, jedoch ist der ToDoApp Code in einem Visual Studio-Projektmappe mit mehreren Projekten benötigen wir diese Einstellung.
-   `clientUrl`ist einfach eine Anwendung festlegen, den Code.

##### <a name="connection-strings"></a>Verbindungszeichenfolgen #####

Die Verbindungszeichenfolgen werden auch als geschachtelte Ressource definiert.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

In der `properties` -Element für `config/connectionstrings`, jede Verbindungszeichenfolge wird auch als Name-Wert-Paar mit dem Format der definierten `“<name>” : {“value”: “…”, “type”: “…”}`. Für die `type` , mögliche Werte sind `MySql`, `SQLServer`, `SQLAzure`, und `Custom`.

>[AZURE.TIP] Führen Sie den folgenden Befehl für eine endgültige Liste der Zeichenfolge Verbindungstypen in Azure PowerShell: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
    
##### <a name="source-control"></a>Datenquellen-Steuerelement #####

Source Control Settings werden auch als geschachtelte Ressource definiert. Azure Ressourcen-Manager verwendet diese Ressource fortlaufende Veröffentlichung konfigurieren (siehe Einschränkung auf `IsManualIntegration` später) und die Bereitstellung der Anwendungscode automatisch während der Verarbeitung der JSON-Datei beginnen.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`und `branch` ziemlich intuitiv sein und sollte auf Git Repository und den Namen der Verzweigung aus veröffentlichen. Wiederum werden diese durch Parameter definiert. 

Beachten Sie in der `dependsOn` Element neben der app Webressource, `sourcecontrols/web` auch `config/appsettings` und `config/connectionstrings`. Ist nach `sourcecontrols/web` wird konfiguriert, Azure Bereitstellungsprozess versucht automatisch bereitstellen, erstellen und Starten des Anwendungscodes. Daher soll Ihnen diese Abhängigkeit einfügen sicherstellen, dass die Anwendung Zugriff auf die erforderliche app-Einstellungen und Verbindungszeichenfolgen vor der Anwendungscode ausgeführt wird. 

>[AZURE.NOTE] Beachten Sie auch `IsManualIntegration` wird `true`. Diese Eigenschaft ist in diesem Lernprogramm erforderlich, da Sie nicht tatsächlich GitHub Repository besitzen, und daher können nicht tatsächlich erteilen Azure fortlaufende Veröffentlichung von [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) (d. h. konfigurieren Automatische Repository Updates zu drücken Azure). Sie können den Standardwert `false` für das angegebene Repository nur, wenn Sie der Besitzer GitHub Anmeldeinformationen in der [Azure-Portal](https://portal.azure.com/) konfiguriert haben. Also wenn Sie zuvor eine Versionskontrolle BitBucket zu GitHub für jede Anwendung in [Azure-Portal](https://portal.azure.com/) eingerichtet, wird unter Angabe Ihrer Benutzeranmeldeinformationen Azure die Anmeldeinformationen speichern und sie verwenden, wenn Sie jede Anwendung von GitHub oder BitBucket zukünftig bereitstellen. Aber wenn Sie dies bereits getan haben, fehl Bereitstellung von JSON-Vorlage, wenn Azure-Ressourcen-Manager versucht, Web app quelleinstellungen konfigurieren, da es in GitHub oder BitBucket mit dem Repository-Eigentümer Anmeldeinformationen anmelden kann.

## <a name="compare-the-json-template-with-deployed-resource-group"></a>Vergleichen der JSON-Vorlage bereitgestellten Ressourcengruppe ##

Hier alle der Web app Blades in [Azure-Portal](https://portal.azure.com/)durchlaufen, aber es ist ein weiteres Tool, das als nützlich, wenn nicht mehr. Fahren Sie mit JSON-Darstellung aller Ressourcengruppen in Ihre Abonnements zu erhalten, wie sie tatsächlich in Azure Back-End- [Ressourcen-Explorer Azure](https://resources.azure.com) -Vorschautool. Sie können sehen, wie die Ressourcengruppe JSON Hierarchie in Azure in der Hierarchie entspricht, mit der es erstellt.

Beispielsweise wenn zum [Azure-Ressourcen-Explorer](https://resources.azure.com) -Tool und erweitern Sie die Knoten im Explorer sehe ich die Ressourcengruppe und die auf Stammebene Ressourcen, die unter der jeweiligen Ressourcenart erfasst werden.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Wenn Drilldown Web app sollte man Web app-Konfigurationsdetails ähnlich sehen den Screenshot unten:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Wiederum geschachtelten Ressourcen müssen eine Hierarchie sehr ähnlich der JSON-Vorlagendatei und müsste die app-Einstellungen, Verbindungszeichenfolgen usw. im Bereich JSON wiedergegeben. Fehlender hier möglicherweise ein Problem mit der JSON-Datei und die JSON-Vorlagendatei bei der Problembehandlung hilfreich.

## <a name="deploy-the-resource-group-template-yourself"></a>Die Gruppenvorlage Ressource selbst bereitstellen ##

Die Schaltfläche **Bereitstellen in Azure** ist groß, aber können die Gruppenvorlage Ressource im azuredeploy.json nur, wenn Sie bereits azuredeploy.json GitHub gedrängt bereitstellen. Azure .NET SDK stellt auch die Tools für jede JSON-Vorlagendatei direkt von Ihrem lokalen Computer bereitstellen. Zu diesem Zweck wie folgt:

1.  **Klicken Sie in Visual Studio** > **neu** > **Projekt**.

2.  Klicken Sie auf **Visual C#** > **Cloud** > **Azure-Ressourcengruppe**, klicken Sie auf **OK**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)

3.  Wählen Sie in **Azure-Vorlage auswählen** **Leere Vorlage aus** und auf **OK**.

4.  Ziehen Sie die azuredeploy.json in **den Vorlagenordner des neuen Projekts** .

    ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)

5.  Öffnen Sie im Projektmappen-Explorer kopiert azuredeploy.json.

6.  Um die Demo fügen einige Application Insight Standardressourcen unsere JSON-Datei durch Klicken auf **Ressource hinzufügen**. Wenn Sie nur für JSON-Datei bereitstellen, fahren Sie mit den Schritten bereitstellen.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)

7.  Wählen Sie **Application Insights für Web Apps**sicherstellen einer vorhandenen App Service planen und WebApp ausgewählt ist, und klicken Sie dann auf **Hinzufügen**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)

    Sie werden jetzt mehrere neuen Ressourcen je nach Ressource und was sehen, Abhängigkeiten App Service-Plan oder Web app. Diese Ressourcen werden durch die vorhandene Definition nicht aktiviert und Sie ändern wollen.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
 
8.  Klicken Sie in der Gliederung JSON **AppInsights skalieren** um die JSON-Code hervorheben. Dies ist die Skalierung Einstellung für Ihren App Service-Plan.

9.  Suchen Sie in den hervorgehobenen JSON-Code die `location` und `enabled` Eigenschaften und wie unten dargestellt.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)

10. Klicken Sie in der Gliederung JSON auf **CPUHigh AppInsights** um die JSON-Code hervorheben. Dies ist eine Warnung.

11. Suchen Sie die `location` und `isEnabled` Eigenschaften und wie unten dargestellt. Führen Sie dasselbe für die anderen drei Alarme (Lila Glühbirnen).

    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)

12. Sie sind jetzt bereitstellen. Maustaste auf das Projekt und wählen **Bereitstellen** > **Neue Bereitstellung**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)

13. Melden Sie Ihre Azure-Konto, wenn Sie dies nicht bereits getan haben.

14. Wählen Sie eine vorhandene Ressourcengruppe in Ihrem Abonnement oder erstellen Sie eines neuen wählen Sie **azuredeploy.json**und klicken Sie dann auf **Parameter bearbeiten**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)

    Sie jetzt können in die Vorlagendatei in Tabellenform gut definierten Parameter bearbeiten. Parameter, die Standardwerte definieren bereits Standardwerte und Parameter, die eine Liste der zulässigen Werte definieren als Dropdownmenüs angezeigt.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)

15. Füllen Sie alle leeren Parameter und [GitHub Repo Adresse für ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) in **RepoUrl**verwenden. Klicken Sie dann auf **Speichern**.
 
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)

    >[AZURE.NOTE] Automatische Skalierung ist im **Standard** -Ebene oder höher und Plan auf Alerts sind Features angeboten **grundlegende** Ebene oder höher müssen Sie **Sku** -Parameter auf **Standard** oder **Premium** um finden alle Ihre App Erkenntnisse Ressourcen Leuchten festgelegt.
    
16. Klicken Sie auf **Bereitstellen**. Wenn Sie **Kennwörter speichern**ausgewählt haben, wird das Kennwort in dem Parameter Datei **im nur-Text**gespeichert. Andernfalls werden Sie aufgefordert, geben Sie das Kennwort während der Bereitstellung.

Das wars! Jetzt müssen Sie nur zu [Azure-Portal](https://portal.azure.com/) und [Azure Resource Explorer](https://resources.azure.com) -Tools neue Alarme und skalieren Einstellungen hinzugefügt in Ihrem JSON Anwendung bereitgestellt.

Die Schritte in diesem Abschnitt erreicht vor allem Folgendes:

1.  Die Vorlagendatei vorbereitet
2.  Erstellt eine Parameterdatei mit der Vorlagendatei
3.  Bereitstellung die Vorlagendatei mit der Datei

Der letzte Schritt von PowerShell-Cmdlets problemlos. Öffnen Sie Scripts\Deploy AzureResourceGroup.ps1 haben Visual Studio beim Bereitstellen der Anwendung finden. Gibt es eine Menge Code, aber ich werde nur den relevanten Code markieren, den Sie die Vorlagendatei mit der Parameterdatei bereitstellen müssen.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Letzte Cmdlet `New-AzureResourceGroup`, ist die eigentlich die Aktion ausführt. Dies sollte Ihnen zeigen, dass mit Hilfe der Werkzeuge zur Bereitstellung einer Cloudanwendung vorhersehbar relativ einfach ist. Jedes Mal, wenn Sie das Cmdlet auf derselben Vorlage mit der gleichen Parameterdatei ausführen, benötigen Sie das gleiche Ergebnis zu erzielen.

## <a name="summary"></a>Zusammenfassung ##

In DevOps sind die Wiederholbarkeit und Vorhersagbarkeit Schlüssel zur erfolgreichen Bereitstellung einer hochgradig skalierbar Anwendung bestehend aus Microservices. In diesem Lernprogramm haben Sie eine Anwendung zwei Microservice Azure als einzigen Ressourcengruppe mit der Azure-Ressourcen-Manager-Vorlage bereitgestellt. Hoffentlich hat es Sie wissen gegeben um start die Anwendung in Azure in eine Vorlage konvertieren bereitstellen und vorhersehbar bereitstellen müssen. 

## <a name="next-steps"></a>Nächste Schritte ##

Finden Sie [agile Methoden anwenden und kontinuierlich Veröffentlichen Ihrer Anwendung Microservices einfache](app-service-agile-software-development.md) und erweiterte Techniken wie [flighting Bereitstellung](app-service-web-test-in-production-controlled-test-flight.md) .

<a name="resources"></a>
## <a name="more-resources"></a>Weitere Ressourcen ##

-   [Azure Ressourcenmanager Vorlagensprache](../resource-group-authoring-templates.md)
-   [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md)
-   [Azure Ressourcenmanager Vorlage Funktionen](../resource-group-template-functions.md)
-   [Bereitstellen einer Anwendung mit Azure-Ressourcen-Manager-Vorlage](../resource-group-template-deploy.md)
-   [Mithilfe von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md)
-   [Problembehandlung bei Bereitstellung in Azure Ressourcengruppe](../resource-manager-troubleshoot-deployments-portal.md)




 
