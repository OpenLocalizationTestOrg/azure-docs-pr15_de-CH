## <a name="how-to-deploy-with-powershell"></a>PowerShell bereitstellen

1. Melden Sie Ihre Azure-Konto.

          Add-AzureAccount

   Nach Anmeldeinformationen, gibt der Befehl Informationen zu Ihrem Konto.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Wenn Sie mehrere Abonnements, bieten Sie die Abonnement-Id für die Bereitstellung verwenden möchten. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Wechseln Sie zur Azure-Ressourcen-Manager-Modul.

          Switch-AzureMode AzureResourceManager

4. Wenn Sie eine vorhandene Ressourcengruppe nicht haben, erstellen Sie eine neue Ressourcengruppe. Geben Sie den Namen der Ressourcengruppe und Speicherort, die Sie für Ihre Lösung benötigen.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

   Eine neue Ressourcengruppe wird zurückgegeben.

        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                    Actions  NotActions
                    =======  ==========
                    *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

5. Erstellen Sie eine neue Bereitstellung für die Ressourcengruppe **Neu AzureResourceGroupDeployment** Befehl und geben Sie die erforderlichen Parameter. Der Parameter enthält einen Namen für die Bereitstellung, den Namen der Ressourcengruppe, der Pfad oder URL der Vorlage erstellten und andere Parameter für Ihr Szenario erforderlich. 
   
   Sie haben die folgenden Optionen zum Bereitstellen der Parameterwerte: 
   
   - Verwenden Sie Inline-Parameter.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - Verwenden Sie ein Parameterobjekt.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Mithilfe einer Parameterdatei.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Wenn die Ressourcengruppe bereitgestellt wurde, sehen Sie eine Zusammenfassung der Bereitstellung.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Informationen zu Bereitstellungsfehlern.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Ausführliche Informationen zu Bereitstellungsfehlern.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
