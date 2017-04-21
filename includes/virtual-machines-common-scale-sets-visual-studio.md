

Dieser Artikel beschreibt, wie ein Azure Virtual Machine Skalierung festlegen mithilfe eines Visual Studio Ressource Gruppe bereitstellen.


[Azure](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) sind Virtual Machine Skala eine Azure Compute-Ressource bereitstellen und Verwalten einer Auflistung ähnlicher virtueller Computer problemlos integrierte Optionen für automatische Skalierung und Lastenausgleich. Bereitgestellt und VM Maßstab legt [Azure Resource Manager (ARM)](https://github.com/Azure/azure-quickstart-templates)Vorlagen bereitstellen. ARM-Vorlagen mit Azure CLI PowerShell REST bereitgestellt werden können und direkt von Visual Studio. Visual Studio bietet eine Reihe von Vorlagen als Teil eines Azure Ressource Bereitstellung eingesetzt wird.

Bereitstellung von Azure-Ressourcengruppe sind zu gruppiert verwandte Azure Ressourcen in einer einzigen Bereitstellungsvorgang veröffentlichen. Hier erfahren sie mehr über: [Erstellen und Bereitstellen von Azure Ressourcengruppen über Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy/).

## <a name="pre-requisites"></a>Erforderliche Komponenten

Bereitstellen von VM Maßstab legt in Visual Studio zunächst benötigen Sie Folgendes:

- Visual Studio 2013 oder 2015
- Azure SDK 2.7 oder 2.8

Hinweis: Diese Anleitung setzt voraus, dass Sie Visual Studio 2015 mit [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Erstellen eines Projekts

1. Erstellen eines neuen Projekts in Visual Studio 2015 mit **Datei | Neue | Projekt**.

    ![Neue Datei][file_new]

2. Unter **Visual C# | Cloud**, **Azure-Ressourcen-Manager** zum Erstellen eines Projekts für die Bereitstellung einer ARM-Vorlage auswählen.

    ![Projekt erstellen][create_project]

3.  Wählen Sie aus der Liste der Vorlagen Linux oder Windows Virtual Machine Skalierung festlegen Vorlage.

    ![Wählen Sie aus][select_Template]

4. Nachdem das Projekt erstellt sehen PowerShell Bereitstellungsskripts, eine Azure-Ressourcen-Manager-Vorlage und einer Parameterdatei Sie für Virtual Machine Skalierung festlegen.

    ![Projektmappen-Explorer][solution_explorer]

## <a name="customize-your-project"></a>Das Projekt anpassen

Jetzt können Sie die Vorlage für die Anwendung benötigt wie VM Erweiterungseigenschaften hinzufügen oder Bearbeiten von Regeln für den Lastenausgleich anpassen bearbeiten. Standardmäßig sind die VM Skalierung Vorlagen konfiguriert Autoscale Regeln hinzufügen, erleichtert die Erweiterung AzureDiagnostics bereitgestellt. Außerdem stellt ein Lastenausgleich mit einer öffentlichen IP-Adresse mit eingehenden NAT-Regeln, mit denen Sie die VM-Instanzen mit SSH (Linux) oder RDP (Windows) – mit der front-End-Portbereich beginnt bei 50000, konfiguriert bei Linux bedeutet Sie SSH Port 50000 öffentliche IP-Adresse (oder Domänennamen) Sie weitergeleitet an Port 22 der ersten VM in der Skalierung festlegen. Herstellen einer Verbindung mit Port 50001 wird Port 22 der zweiten VM usw. weitergeleitet.

 So bearbeiten Sie die Vorlagen mit Visual Studio lässt mit JSON Gliederung um Parametern, Variablen und Ressourcen zu organisieren. Verständnis des Schemas können Visual Studio Fehler in der Vorlage zeigen, bevor Sie es bereitstellen.

![JSON-Explorer][json_explorer]

## <a name="deploy-the-project"></a>Bereitstellen des Projekts

6. Bereitstellen der ARM-Vorlage Azure festlegen VM-Ressource erstellen. Klicken Sie mit der rechten Maustaste auf den Projektknoten, wählen **Bereitstellen | Neue Bereitstellung**.

    ![Vorlage bereitstellen][5deploy_Template]

7. Wählen Sie Ihr Abonnement im Dialogfeld "Bereitstellen, Ressourcengruppe".

    ![Vorlage bereitstellen][6deploy_Template]

8. Von hier aus können Sie auch neue Azure Ressourcengruppe zum Bereitstellen der Vorlage zu erstellen.

    ![Neue Ressourcengruppe][new_resource]

9. Weiter wählen Sie **Parameter bearbeiten** eingeben von Parametern, die an der Vorlage bestimmte Werte wie den Benutzernamen und das Kennwort für das Betriebssystem sind die Bereitstellung erstellen.

    ![Parameter bearbeiten][edit_parameters]

10. Klicken Sie auf jetzt **Bereitstellen**. **Das Ausgabefenster** zeigt den Fortschritt der Bereitstellung. Beachten Sie, dass die Aktion **Bereitstellen AzureResourceGroup.ps1** Skript ausgeführt wird.

    ![Ausgabefenster][output_window]

## <a name="exploring-your-vm-scale-set"></a>Die VM-Skalierungsgruppe durchsuchen

Nach Abschluss die Bereitstellung der neuen VM Skalierung festlegen im Visual Studio- **Cloud Explorer** (Aktualisieren der Liste) angezeigt. Cloud-Explorer können Sie Azure in Visual Studio Ressourcen während der Anwendungsentwicklung. Sie können auch in der Azure-Portal und Azure Resource Explorer VM Skalierung festlegen anzeigen.

![Cloud Explorer][cloud_explorer]

 Das Portal bietet am besten Verwalten Ihrer Azure-Infrastruktur mit einem Webbrowser während Azure-Ressourcen-Explorer eine einfache Möglichkeit, Explorer bietet und Debuggen Azure Ressourcen ein Fenster in der Instanzansicht"" und auch PowerShell-Befehle für die Ressourcen, denen Sie suchen. Während VM Skalierung in Vorschau sind, zeigen im Ressourcen-Explorer die meisten Details für die VM skalieren.

## <a name="next-steps"></a>Nächste Schritte

Nachdem erfolgreich VM Maßstab legt durch Visual Studio bereitgestellt haben können Projekt Anwendung Bedarf weiter anpassen. Einrichten von skalieren z. B. Hinzufügen einer Ressource Einblicke, Infrastruktur zu Ihrer Vorlage wie eigenständige VMs hinzufügen oder Programme mit der Erweiterung benutzerdefiniertes Skript bereitstellen. Eine gute Quelle für Beispielvorlagen finden in [Azure Schnellstart Vorlagen](https://github.com/Azure/azure-quickstart-templates) GitHub Repository (Suche nach "Vmss").

[file_new]: ./media/virtual-machines-common-scale-sets-visual-studio/1-FileNew.png
[create_project]: ./media/virtual-machines-common-scale-sets-visual-studio/2-CreateProject.png
[select_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machines-common-scale-sets-visual-studio/6-DeployTemplate.png
[new_resource]: ./media/virtual-machines-common-scale-sets-visual-studio/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machines-common-scale-sets-visual-studio/8-EditParameter.png
[output_window]: ./media/virtual-machines-common-scale-sets-visual-studio/9-Output.png
[cloud_explorer]: ./media/virtual-machines-common-scale-sets-visual-studio/12-CloudExplorer.png