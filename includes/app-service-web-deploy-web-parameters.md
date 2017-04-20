Mit Azure-Ressourcen-Manager definieren Sie Parameter für Werte zu geben, wenn die Vorlage bereitgestellt wird. Die Vorlage enthält einen Abschnitt Parameter, der alle Parameterwerte enthält.
Definieren Sie Parameter für die Werte, die variieren basierend auf das Projekt, das Sie bereitstellen oder die Umgebung, der Sie bereitstellen. Definieren Sie Parameter für die Werte, die immer gleich bleiben. Jeder Parameterwert wird in der Vorlage zur Ressourcen definieren, die bereitgestellt werden. 

Wenn Sie Parameter definieren, verwenden Sie das Feld **AllowedValues** Werte angegeben Benutzer während der Bereitstellung bieten kann. Verwenden Sie das Feld **DefaultValue** der Parameter einen Wert zuweisen, wird kein Wert, während der Bereitstellung angegeben ist.

Jeder Parameter in der Vorlage beschrieben wird.

### <a name="sitename"></a>siteName

Der Name der Webanwendung, die Sie erstellen möchten.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

Der Name der App Service-Plan für die Webanwendung hosten.
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU

Der Tarif für Hosting-Plan.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Die Vorlage definiert die Werte, die für diesen Parameter zulässig sind und weist einen Standardwert (S1), wenn kein Wert angegeben ist.

### <a name="workersize"></a>workerSize

Die Instanzgröße hosting Plan (klein, Mittel oder groß).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
Die Vorlage definiert die Werte für diesen Parameter (0, 1 oder 2) dürfen und weist einen Standardwert (0), wenn kein Wert angegeben ist. Die Werte entsprechen kleine, mittlere und große.
