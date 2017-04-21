## <a name="how-to-deploy-with-azure-cli"></a>Zum Bereitstellen von Azure-CLI

1. Melden Sie Ihre Azure-Konto.

        azure login

  Nach Anmeldeinformationen, gibt der Befehl aus Ihren Benutzernamen.

        ...
        info:    login command OK

2. Wenn Sie mehrere Abonnements, bieten Sie die Abonnement-Id für die Bereitstellung verwenden möchten.

        azure account set <YourSubscriptionNameOrId>

3. Wechseln Sie zur Azure Resource Manager-Modul

        azure config mode arm

   Sie können den neuen Modus Bestätigung.

        info:     New mode is arm

4. Wenn Sie eine vorhandene Ressourcengruppe nicht haben, erstellen Sie eine neue Ressourcengruppe. Geben Sie den Namen der Ressourcengruppe und Speicherort, die Sie für Ihre Lösung benötigen.

        azure group create -n ExampleResourceGroup -l "West US"

   Eine neue Ressourcengruppe wird zurückgegeben.

        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Eine neue Bereitstellung für die Ressourcengruppe erstellen, führen Sie den folgenden Befehl und die erforderlichen Parameter. Der Parameter enthält einen Namen für die Bereitstellung, den Namen der Ressourcengruppe, der Pfad oder URL der Vorlage erstellten und andere Parameter für Ihr Szenario erforderlich.

   Sie haben die folgenden Optionen zum Bereitstellen der Parameterwerte:

   - Verwenden Sie Inline-Parameter und lokale Vorlage.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Verwenden Sie Inline-Parameter und einen Link zu einer Vorlage.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Verwenden einer Parameterdatei.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Wenn die Ressourcengruppe bereitgestellt wurde, sehen Sie eine Zusammenfassung der Bereitstellung.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Informationen zu der aktuellen Bereitstellung.

         azure group log show -l ExampleResourceGroup

7. Ausführliche Informationen zu Bereitstellungsfehlern.

         azure group log show -l -v ExampleResourceGroup
