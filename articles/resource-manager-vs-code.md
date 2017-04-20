<properties
   pageTitle="VS-Code mit Ressourcen-Manager Vorlagen verwenden | Microsoft Azure"
   description="Veranschaulicht, wie Visual Studio Code Azure-Ressourcen-Manager Vorlagen erstellen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>

# <a name="working-with-azure-resource-manager-templates-in-visual-studio-code"></a>Arbeiten mit Azure Ressourcenmanager Vorlagen in Visual Studio Code

Azure Ressourcenmanager Vorlagen sind JSON-Dateien, die eine Ressource und zugehörige Abhängigkeiten beschreiben. Diese Dateien kann Groß und daher tooling Unterstützung wichtig. Visual Studio-Code wird eine neue einfache Open Source, plattformübergreifenden Code-Editor. Erstellen und Bearbeiten von Vorlagen für Ressourcen-Manager über eine [neue Erweiterung](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)unterstützt. VS Code überall ausgeführt und erfordert Zugang zum Internet, wenn Sie auch Ressourcenmanager Vorlagen bereitstellen möchten.

Wenn Sie nicht bereits über VS Code verfügen, können Sie es auf [https://code.visualstudio.com/](https://code.visualstudio.com/)installieren.

## <a name="install-the-resource-manager-extension"></a>Die Ressourcen-Manager-Erweiterung installieren

Arbeiten mit JSON Vorlagen in VS Code müssen Sie eine Erweiterung installieren. Die folgenden Schritte herunter und installieren sprachunterstützung für Ressourcenmanager JSON-Vorlagen:

1. VS Code starten 
2. Öffnen Sie schnell öffnen (STRG + P) 
3. Führen Sie den folgenden Befehl ein: 

        ext install azurerm-vscode-tools

4. Starten Sie VS Code Aufforderung die Erweiterung zu aktivieren. 

 Arbeit!

## <a name="set-up-resource-manager-snippets"></a>Ressourcenmanager Ausschnitte einrichten

Die vorherigen Schritte an Unterstützung installiert, aber jetzt müssen wir Code Verwendung von JSON Vorlage Snippets VS konfigurieren.

1. Kopieren Sie den Inhalt der Datei aus dem Repository [Azure-befehlsschnittstelleninstanz-Arm-Werkzeuge](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) in die Zwischenablage.
2. VS Code starten 
3. In VS Code öffnen Sie JSON Ausschnitte Datei durch Navigieren zur **Datei** -> **Voreinstellungen** -> **Benutzer Ausschnitte** -> **JSON**oder durch **F1** auswählen und **Voreinstellungen** eingeben, bis Sie auswählen können **Voreinstellungen: Ausschnitte**.

    ![Voreinstellung Ausschnitte](./media/resource-manager-vs-code/preferences-snippets.png)

    Wählen Sie die Optionen **JSON**.

    ![Wählen Sie json](./media/resource-manager-vs-code/select-json.png)

4. Fügen Sie den Inhalt der Datei in Schritt 1 in der Benutzerdatei Ausschnitte vor dem letzten "}" 
5. Sicher JSON richtig und gibt keine Wellenlinien überall. 
6. Speichern und Schließen der Benutzerdatei Ausschnitte.

Das ist alles, die erforderlich ist, verwenden die Ressourcenmanager Ausschnitte. Als Nächstes stellen wir diesem testen.

## <a name="work-with-template-in-vs-code"></a>Arbeiten Sie mit Vorlagen in VS Code

Am einfachsten beginnen mit einer Vorlage ist einfach eine Quick Start Vorlagen auf [Github](https://github.com/Azure/azure-quickstart-templates) oder eine eigene. Sie können [eine Vorlage exportieren](resource-manager-export-template.md) eines der Ressourcengruppen über das Portal. 

1. Exportiert eine Vorlage aus einer Ressourcengruppe öffnen Sie die extrahierten Dateien in VS Code.

    ![Dateien anzeigen](./media/resource-manager-vs-code/show-files.png)

2. Öffnen Sie die Datei template.json, damit bearbeiten und zusätzlichen Ressourcen hinzufügen. Nach dem **"Ressourcen": [** EINGABETASTE drücken, um eine neue Zeile zu beginnen. Bei der Eingabe von **Arm**wird eine Liste mit Optionen angezeigt. Diese Optionen sind installierte Vorlage Ausschnitte. Es sollte wie folgt aussehen: 

    ![Codeausschnitte anzeigen](./media/resource-manager-vs-code/type-snippets.png)

3. Wählen Sie den gewünschten Ausschnitt. Für diesen Artikel lasse ich **Arm Ip -** erstellt eine neue öffentliche IP-Adresse. Setzen Sie ein Komma nach der schließenden Klammer "}" die neu erstellte Ressource zu Ihrer Vorlage Syntax ist gültig.

     ![Komma](./media/resource-manager-vs-code/add-comma.png)

4. VS-Code hat integrierte IntelliSense. Bearbeiten von Vorlagen wird VS Code verfügbaren Werte vorgeschlagen. Z. B. zum Hinzufügen eines Abschnitts Variablen zu einer Vorlage hinzufügen **""** (zwei Anführungszeichen), und wählen Sie **STRG + Leerzeichen** zwischen den Anführungszeichen. Einschließlich der **Variablen**wird angezeigt.

    ![Fügen Sie Variablen hinzu](./media/resource-manager-vs-code/add-variables.png)

5. IntelliSense kann auch verfügbaren Werte oder Funktionen vorschlagen. Erstellen Sie zum Festlegen einer Eigenschaft einen Parameterwert Ausdruck **"[]"** mit **STRG + LEERTASTE**. Sie können den Namen einer Funktion starten. **Wenn Sie die Funktion gefunden haben, die gewünschte Registerkarte.**

    ![Parameter hinzufügen](./media/resource-manager-vs-code/select-parameters.png)

6. Wählen Sie **STRG + LEERTASTE** erneut innerhalb der Funktion eine Liste der verfügbaren Parameter in der Vorlage.

    ![Parameter hinzufügen](./media/resource-manager-vs-code/select-avail-parameters.png)

7. Haben Sie Probleme Validierung Schema in der Vorlage sehen Sie bekannte Schnörkel im Editor. Sie können die Liste der Fehler und Warnungen durch **STRG + UMSCHALT + M** eingeben oder Auswählen der Symbole in der unteren linken Statusleiste anzeigen.

    ![Fehler](./media/resource-manager-vs-code/errors.png)

    Überprüfung der Vorlage hilft Ihnen die Syntax erkennen; Allerdings können Sie Fehler anzeigen, die Sie ignorieren können. In einigen Fällen vergleicht der Editor Ihrer Vorlage anhand eines Schemas, die nicht auf dem neuesten Stand und daher meldet einen Fehler, obwohl Sie wissen, dass er korrekt ist. Angenommen Sie, eine Funktion hat wurde vor kurzem zum Ressourcen-Manager jedoch das Schema wurde nicht aktualisiert. Der Editor meldet einen Fehler trotz der Tatsache, dass die Funktion während der Bereitstellung funktioniert.

    ![Fehlermeldung](./media/resource-manager-vs-code/unrecognized-function.png)

## <a name="deploy-your-new-resources"></a>Neuen Ressourcen bereitzustellen

Wenn die Vorlage fertig ist, können Sie die neuen Ressourcen wie folgt bereitstellen: 

### <a name="windows"></a>Windows

1. Öffnen Sie eine PowerShell-Befehlszeile 
2. Login-Typ: 

        Login-AzureRmAccount 

3. Wenn Sie mehrere Abonnements verfügen, erhalten Sie eine Liste der Abonnements mit:

        Get-AzureRmSubscription

    Und wählen Sie den Dauerauftrag verwendet.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. Die Parameter in der Datei parameters.json aktualisieren
5. Führen Sie Deploy.ps1 um die Vorlage auf Azure bereitgestellt werden

### <a name="osxlinux"></a>OSX-Linux

1. Öffnen Sie ein terminal-Fenster 
2. Login-Typ:

        azure login 

3. Wenn mehrere Abonnements verfügen, wählen Sie das richtige Abonnement mit:

        azure account set <subscriptionNameOrId> 

4. Aktualisieren Sie die Parameter in der Datei parameters.json.
5. Um die Vorlage bereitzustellen, führen Sie Folgendes aus:

        azure group deployment create -f <PathToTemplate> 

## <a name="next-steps"></a>Nächste Schritte

- Über Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen](resource-group-authoring-templates.md).
- Vorlage-Funktionen finden Sie unter [Azure Ressourcenmanager Vorlagenfunktionen](resource-group-template-functions.md).
- Weitere Beispiele zur Arbeit mit Visual Studio finden Sie unter [Build Cloud apps mit Visual Studio](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) verbinden [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 [Demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Weitere Schnellstarts HealthClinic.biz Demo finden Sie unter [Azure Developer Tools Schnellstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
