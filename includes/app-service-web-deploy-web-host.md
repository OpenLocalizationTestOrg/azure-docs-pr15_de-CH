### <a name="app-service-plan"></a>App Service-plan

Service-Plan für das Hosten der Webanwendung erstellt. Sie können den Namen des Plans durch den **HostingPlanName** -Parameter angeben. Speicherort des Plans ist der Speicherort für die Ressourcengruppe verwendet. Die Preisgestaltung Tier und Arbeitskraft Größe **Sku** und **WorkerSize** -Parameter angegeben sind

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

