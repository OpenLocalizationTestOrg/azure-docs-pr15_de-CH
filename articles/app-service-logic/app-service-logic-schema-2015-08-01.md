<properties 
    pageTitle="Neues Schema Version 2015-08-01-Vorschau" 
    description="Schreiben Sie die JSON-Definition für die neueste Version von Logik-apps" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>Neues Schema Version 2015-08-01-Vorschau

Das neue Schema und API-Version für Logik-apps hat eine Anzahl Verbesserung der Zuverlässigkeit und Einfachheit der Verwendung von Logik-apps. Es gibt 4 Hauptunterschiede:

1. Typ der **APIApp** wurde ein neuer Aktionstyp **APIConnection** aktualisiert.
2. **Wiederholen Sie** wurde **Foreach**umbenannt.
3. **HTTP-Listener** API-app ist nicht mehr erforderlich.
4. Aufrufen von untergeordneten Workflows verwendet ein neues Schema.

## <a name="1-moving-to-api-connections"></a>1. verschieben API-Verbindungen

Die größte Änderung ist, dass Sie nicht mehr Bereitstellung API-apps in Azure-Abonnement API verwenden. Es gibt 2 APIs verwenden können:
* Verwaltete API
* Ihre benutzerdefinierte Web API

Jede dieser wird anders behandelt, da deren Management und Hosting-Modelle unterscheiden. Ein Vorteil dieses Modells ist, dass Sie nicht mehr gezwungen sind bereitgestellt werden Ressourcen in der Ressourcengruppe. 

### <a name="managed-apis"></a>Verwaltete APIs

Zählen der API, die von Microsoft in Ihrem Auftrag wie Office 365 Salesforce Twitter, FTP usw. verwaltet werden. Einige dieser verwalteten API kann als verwendet werden-ist wie Bing übersetzen, während andere Konfiguration erfordern. Diese Konfiguration wird einer *Verbindung*aufgerufen.

Office 365 verwenden, müssen Sie z. B. eine Verbindung erstellen, die Office 365-Token enthält. Dieses Token wird sicher gespeichert und aktualisiert, sodass Ihre Anwendung Logik immer die Office 365-API aufrufen kann. Möchten Sie zum SQL oder FTP-Server herstellen, müssen Sie auch eine Verbindung erstellen die Verbindungszeichenfolge. 

Diese Aktionen werden in der Definition bezeichnet `APIConnection`. Hier ist ein Beispiel für eine Verbindung, die Office 365 e-Mails senden aufruft:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Der Teil der Eingaben zu API-Verbindungen ist die `host` Objekt. Dies umfasst zwei Teile: `api` und `connection`.

Die `api` hat die Common Language Runtime, die API verwaltet URL gehostet wird. Sehen Sie alle verfügbaren verwalteten APIs für Sie durch Aufrufen von `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Wenn Sie eine API verwenden, können oder haben keine **Parameter** definiert. Ansonsten ist keine **Verbindung** erforderlich. Andernfalls müssen Sie eine Verbindung herstellen. Erstellen, müssen der Name der, Verbindung als Sie verweisen, die in der `connection` -Objekt in der `host` Objekt. Rufen Sie zum Erstellen einer Verbindung in einer Ressourcengruppe

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Mit dem folgenden Text:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>Bereitstellen von verwalteten APIs in einer Azure-Ressourcen-Manager-Vorlage

Eine vollständige Anwendung können in einer ARM-Vorlage als interaktive Anmeldung erfordert. Verlangt anmelden, kann alles eingerichtet mit ARM-Vorlage jedoch noch immer auf das Portal zum Autorisieren der Verbindung besuchen. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Sie sehen in diesem Beispiel sind die Anschlüsse nur normale Ressourcen, die in der Ressourcengruppe befinden. Sie verweisen in Ihrem Abonnement verfügbar ManagedAPIs.

### <a name="your-custom-web-apis"></a>Benutzerdefinierte Web APIs

Verwenden Sie eigene APIs (insbesondere nicht von Microsoft verwaltet sind), sollten Sie die integrierte **HTTP** -Aktion verwenden, um zu nennen. Um eine ideale Lösung, sollte für Ihre API stolz Endpunkt verfügbar machen. Dadurch Logik app Designer, die Eingaben und Ausgaben für Ihre API dargestellt. Ohne eine stolz können Designer nur die Eingaben und Ausgaben als undurchsichtig JSON-Objekte anzeigen.

Hier ist ein Beispiel der neuen `metadata.apiDefinitionUrl` Eigenschaft:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Wenn Sie Ihre Web-API **App** -Dienst hosten wird dann es automatisch in der Liste der verfügbaren Aktionen im Designer angezeigt. Wenn dies nicht der Fall ist, müssen Sie die URL direkt einfügen. Stolz Endpunkt muss nicht authentifiziert sein, um in Designer apps Logik verwendet werden (obwohl Sie die API sichern mit beliebigen Methoden der stolz unterstützt werden).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>Verwenden die bereits bereitgestellte API-apps mit 2015-08-01-Vorschau

Wenn Sie zuvor eine API-app bereitgestellt, können Sie es über die **http-** Aktion aufrufen.

Beispielsweise verwenden Sie Dropbox um Dateien müssen etwas in der Schemadefinition Version **2014-12-01-Vorschau** Sie:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Sie können die entsprechende HTTP-Aktion wie unten (die Parameter von der Logik app Definition unverändert) erstellen:

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Diese Eigenschaften von nacheinander durchlaufen:

| Action-Eigenschaft |  Beschreibung |
| --------------- | -----------  |
| `type` | `Http`Statt`APIapp` |
| `metadata.apiDefinitionUrl` | Wenn Sie diese Aktion in der Logik apps Designer verwenden möchten, sollten Sie den Endpunkt enthalten. Dies ist aus erstellt:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Dies ist aus erstellt:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Immer`POST` |
| `inputs.body` | Mit der API-app-Parameter | 
| `inputs.authentication` | Api-app-Authentifizierung mit |

Dieser Ansatz sollte für alle API-app Aktionen funktionieren. Allerdings beachten Sie sollten verschieben auf einen der beiden anderen Optionen (entweder eine verwaltete API oder hosten Ihre benutzerdefinierte Web-API), die diese vorherigen API-apps werden nicht mehr unterstützt.

## <a name="2-repeat-renamed-to-foreach"></a>2 wiederholen Foreach umbenannt

**Wiederholen Sie** die vorherigen Schemaversion wir erhalten viel Kundenfeedback war verwirrend und nicht ordnungsgemäß erfasst, dass es wirklich eine for each-Schleife. Daher haben wir es an **Foreach**umbenannt. Zum Beispiel:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Wäre jetzt folgendermaßen geschrieben werden:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Zuvor die Funktion `@repeatItem()` wurde verwendet, um das aktuelle Element durchlaufen wird. Dies vereinfacht auf `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>Verweisen auf die Ausgaben der Foreach
Zur weiteren Vereinfachung werden die Ausgaben der **Foreach** -Aktionen nicht in ein Objekt namens **RepeatItems**eingeschlossen. Daher waren die Ausgaben der oben Wiederholen:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Jetzt werden:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Verweisen auf diese Ausgaben müssten Sie den Hauptteil der Aktion zu tun:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

Jetzt haben Sie stattdessen:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

Mit diesen Funktionen `@repeatItem()`, `@repeatBody()` und `@repeatOutputs()` werden entfernt.

## <a name="3-native-http-listener"></a>3. systemeigenen HTTP-listener 
HTTP-Listener-Funktionen sind jetzt müssen Sie nicht mehr zum Bereitstellen einer app HTTP-Listener-API integriert. Informationen Sie zu [Einzelheiten wie den Logik app Endpunkt aufgerufen hier](app-service-logic-http-endpoint.md). 

Mit dieser Funktion `@accessKeys()` entfernt und wurde durch die `@listCallbackURL()` Funktion für die Zwecke der erste Endpunkt (bei Bedarf). Darüber hinaus müssen Sie jetzt mindestens einen Trigger in Ihrer Anwendung Logik jetzt definieren. Möchten Sie `/run` Workflow müssen Sie die haben eine `manual`, `apiConnectionWebhook` oder `httpWebhook` Trigger. 

## <a name="4-calling-child-workflows"></a>4. Aufruf von untergeordneten workflows

Bisher aufrufen untergeordnete Workflows wird der Workflow Zugriffstoken abrufen und einfügen, die in der Definition der Anwendung Logik, die das Kind aufgerufen. Mit der neuen Schemaversion generiert Logik apps Engine automatisch SAS zur Laufzeit für den untergeordneten Workflow, sodass Sie vertrauliche Daten in die Definition eingefügt haben.  Hier ist ein Beispiel:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

Eine zweite Verbesserung ist, dass wir die untergeordneten Workflows Vollzugriff die eingehende Anforderung geben. Das bedeutet, Parameter in *Abfragen* und *Header* -Objekt übergeben werden können und der gesamte Text vollständig definieren können.

Schließlich sind auf den untergeordneten Workflow. Während bevor Sie nur untergeordneten Workflow direkt aufrufen Nun müssen Sie einen Trigger Endpunkt im Workflow für den übergeordneten aufrufen definieren. Im Allgemeinen bedeutet dies Sie Trigger Typ **manuell** hinzufügen und verwenden, die in der übergeordneten Definition. Beachten Sie, dass die `host` -Eigenschaft hat einen `triggerName`, da Sie immer die Trigger angeben müssen Sie aufrufen.

## <a name="other-changes"></a>Sonstige

### <a name="new-queries-property"></a>Neue Abfragen-Eigenschaft
Alle Aktivitätstypen unterstützt jetzt eine neue Eingabe **Abfragen**genannt. Dies ist ein strukturiertes Objekt anstatt Sie manuell die Zeichenfolge zusammenstellen.

### <a name="parse-function-renamed"></a>Parse() Funktion umbenannt
Wie wir bald weitere Inhaltstypen hinzufügen werden die `parse()` umbenannt wurde `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>In Kürze: Enterprise Integration APIs
Zu diesem Zeitpunkt nicht doch konnten die Enterprise-Integration-APIs (wie AS2)-Versionen. Diese werden als [Wegweiser](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/)bald. In der Zwischenzeit können Ihre vorhandenen bereitgestellten BizTalk-APIs über HTTP-Aktion Sie wie oben in"bereits bereitgestellte API apps."
