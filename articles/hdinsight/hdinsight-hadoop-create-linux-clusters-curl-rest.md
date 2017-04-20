<properties
    pageTitle="Hadoop, HBase oder Sturm Cluster unter Linux in HDInsight Aufrollen mit Azure REST API erstellen | Microsoft Azure"
    description="Erfahren Sie, wie Linux-basierten HDInsight Cluster mit cURL, Azure-Ressourcen-Manager Vorlagen und Azure REST API erstellen. Sie können geben den Cluster (Hadoop, HBase oder Sturm) oder mithilfe von Skripts die angepassten Komponenten installieren."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/11/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-curl-and-the-azure-rest-api"></a>Erstellen Sie Linux-basierten Clustern in HDInsight Aufrollen mit Azure REST API

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure REST-API können Sie Verwaltungsvorgänge in der Azure-Plattform, einschließlich des neuen Ressourcen wie HDInsight Linux-basierten Cluster gehostete Dienste ausführen. In diesem Dokument erfahren Sie, wie Azure Ressourcenmanager Vorlagen zum Konfigurieren einer HDInsight-Cluster und der zugehörige Speicher und dann mit Aufrollen die Vorlage der Azure-REST-API zum Erstellen eines neuen HDInsight Clusters bereitstellen.

> [AZURE.IMPORTANT] Die Schritte in diesem Dokument verwenden Sie die Standardanzahl von workerknoten (4) für einen HDInsight-Cluster. Wenn Sie mehr als 32 Arbeitskraft zur Clustererstellung oder Knoten durch Skalierung des Clusters nach Erstellung planen müssen Sie Headknoten Größe 14 GB Ram mit mindestens 8 Kernen auswählen.
>
> Weitere Informationen zu Knoten Größen und Kosten finden Sie unter [HDInsight Preisgestaltung](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Erforderliche Komponenten

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __Azure CLI__. Azure-CLI wird verwendet, um einen Dienstprinzipal zu erstellen, dann Authentifizierungstoken Anfragen Azure REST-API erstellen.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

- __Aufrollen__. Dieses Dienstprogramm ist Ihr Verwaltungssystem erhältlich oder kann von [http://curl.haxx.se/](http://curl.haxx.se/)heruntergeladen werden.

    > [AZURE.NOTE] Wenn Sie zum Ausführen der Befehle in diesem Dokument PowerShell verwenden, Sie müssen zuerst die `curl` Alias standardmäßig erstellt. Dieser Alias verwendet Invoke-WebRequest, PowerShell-Cmdlet statt Aufrollen bei Verwendung der `curl` Befehl von PowerShell Prompt und Fehlermeldungen für viele Befehle in diesem Dokument verwendet wird.
    > 
    > Entfernen Sie diesen Alias verwenden Sie PowerShell Prompt Folgendes:
    >
    > `Remove-item alias:curl`
    >
    > Wenn der Alias entfernt wurden, sollten Sie möglicherweise Version curl verwenden, die Sie auf Ihrem System installiert haben.

### <a name="access-control-requirements"></a>Steuerelement erforderlich

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="create-a-template"></a>Erstellen einer Vorlage

Azure Ressourcenmanagement Vorlagen sind JSON Standarddokumenten, die zur Beschreibung einer __Ressourcengruppe__ und alle Ressourcen (z. B. HDInsight.) Dieser Ansatz Vorlage können Sie die Ressourcen definieren, die Sie für HDInsight in einer Vorlage und verwalten die Gruppe als Ganzes durch __Installationen__ , die für die Gruppe zu verwenden.

Vorlagen werden in der Regel in zwei Teilen bereitgestellt. die Vorlage selbst und einer Datei, die Sie für Ihre Konfiguration mit bestimmten Werten füllen. Für Beispiel, den Clusternamen Admin-Name und Kennwort. Wenn Sie die REST-API direkt verwenden, müssen Sie in einer Datei kombinieren. Das Format dieses JSON ist:

    {
        "properties": {
            "template": {
                contents of template file
            },
            "mode": "deploymentmode",
            "Parameters": {
                contents of the parameters element from parameters file
            }
        }
    }

Beispielsweise ist folgende Zusammenschluss der Vorlage und Parameter aus [https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-ssh-password), die einen Linux-basierten Cluster mit einem Kennwort sichern SSH-Benutzerkonto erstellt.

    {
        "properties": {
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {
                    "location": {
                        "type": "string",
                        "allowedValues": ["Central US",
                        "East Asia",
                        "East US",
                        "Japan East",
                        "Japan West",
                        "North Europe",
                        "South Central US",
                        "Southeast Asia",
                        "West Europe",
                        "West US"],
                        "metadata": {
                            "description": "The location where all azure resources will be deployed."
                        }
                    },
                    "clusterType": {
                        "type": "string",
                        "allowedValues": ["hadoop",
                        "hbase",
                        "storm",
                        "spark"],
                        "metadata": {
                            "description": "The type of the HDInsight cluster to create."
                        }
                    },
                    "clusterName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the HDInsight cluster to create."
                        }
                    },
                    "clusterLoginUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
                        }
                    },
                    "clusterLoginPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "sshUserName": {
                        "type": "string",
                        "metadata": {
                            "description": "These credentials can be used to remotely access the cluster."
                        }
                    },
                    "sshPassword": {
                        "type": "securestring",
                        "metadata": {
                            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
                        }
                    },
                    "clusterStorageAccountName": {
                        "type": "string",
                        "metadata": {
                            "description": "The name of the storage account to be created and be used as the cluster's storage."
                        }
                    },
                    "clusterWorkerNodeCount": {
                        "type": "int",
                        "defaultValue": 4,
                        "metadata": {
                            "description": "The number of nodes in the HDInsight cluster."
                        }
                    }
                },
                "variables": {
                    "defaultApiVersion": "2015-05-01-preview",
                    "clusterApiVersion": "2015-03-01-preview"
                },
                "resources": [{
                    "name": "[parameters('clusterStorageAccountName')]",
                    "type": "Microsoft.Storage/storageAccounts",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('defaultApiVersion')]",
                    "dependsOn": [],
                    "tags": {
                        
                    },
                    "properties": {
                        "accountType": "Standard_LRS"
                    }
                },
                {
                    "name": "[parameters('clusterName')]",
                    "type": "Microsoft.HDInsight/clusters",
                    "location": "[parameters('location')]",
                    "apiVersion": "[variables('clusterApiVersion')]",
                    "dependsOn": ["[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"],
                    "tags": {
                        
                    },
                    "properties": {
                        "clusterVersion": "3.2",
                        "osType": "Linux",
                        "clusterDefinition": {
                            "kind": "[parameters('clusterType')]",
                            "configurations": {
                                "gateway": {
                                    "restAuthCredential.isEnabled": true,
                                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                                }
                            }
                        },
                        "storageProfile": {
                            "storageaccounts": [{
                                "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                                "isDefault": true,
                                "container": "[parameters('clusterName')]",
                                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                            }]
                        },
                        "computeProfile": {
                            "roles": [{
                                "name": "headnode",
                                "targetInstanceCount": "2",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            },
                            {
                                "name": "workernode",
                                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                                "hardwareProfile": {
                                    "vmSize": "Standard_D3"
                                },
                                "osProfile": {
                                    "linuxOperatingSystemProfile": {
                                        "username": "[parameters('sshUserName')]",
                                        "password": "[parameters('sshPassword')]"
                                    }
                                }
                            }]
                        }
                    }
                }],
                "outputs": {
                    "cluster": {
                        "type": "object",
                        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
                    }
                }
            },
            "mode": "incremental",
            "Parameters": {
                "location": {
                    "value": "North Europe"
                },
                "clusterName": {
                    "value": "newclustername"
                },
                "clusterType": {
                    "value": "hadoop"
                },
                "clusterStorageAccountName": {
                    "value": "newstoragename"
                },
                "clusterLoginUserName": {
                    "value": "admin"
                },
                "clusterLoginPassword": {
                    "value": "changeme"
                },
                "sshUserName": {
                    "value": "sshuser"
                },
                "sshPassword": {
                    "value": "changeme"
                }
            }
        }
    }

In diesem Beispiel wird in den Schritten in diesem Dokument verwendet werden. Mit den Werten für den Cluster verwenden möchten, müssen Sie die Platzhalter- _Werte_ im Abschnitt __Parameter__ am Ende des Dokuments ersetzen.

##<a name="login-to-your-azure-subscription"></a>Melden Sie Ihre Azure-Abonnement

Folgen Sie den Schritten in [Verbindung mit einer Azure-Abonnement aus der Azure-Befehlszeilenschnittstelle (CLI Azure)](../xplat-cli-connect.md) dokumentiert und verbinden Ihr Abonnement mit dem `azure login` Befehl.

##<a name="create-a-service-principal"></a>Erstellen Sie einen Dienstprinzipalnamen

> [AZURE.NOTE] Diese Schritte sind eine gekürzte Version der Informationen im Bereich _Authentifizierung Service principal mit einem Kennwort - Azure CLI_ des Dokuments [authentifizieren Dienstprinzipalnamen mit Azure-Ressourcen-Manager](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password---azure-cli) . Erstellen ein neues Dienstes principal, der REST API Azure Ressourcen wie HDInsight-Cluster erstellt authentifizieren verwendet werden kann.

1. Befehlszeile, Terminaldienste oder Shell verwenden Sie folgenden Befehl Listen Sie Ihre Azure-Abonnements.

        azure account list
        
    Wählen Sie in der Liste des Abonnements und beachten Sie die __ID-__ Spalte. Dies ist die __Abonnement-ID__ und die meisten Schritte in diesem Dokument verwendet wird.

2. Erstellen einer neuen Anwendung in Azure Active Directory.

        azure ad app create --name "exampleapp" --home-page "https://www.contoso.org" --identifier-uris "https://www.contoso.org/example" --password <Your_Password>
        
    Ersetzen Sie die Werte für die `--name`, `--home-page`, und `--identifier-uris` mit Ihren eigenen Werten. Geben Sie ein Kennwort für den neuen Active Directory-Eintrag.
    
    > [AZURE.NOTE] Da diese Anwendung für die Authentifizierung durch einen Dienstprinzipalnamen erstellen die `--home-page` und `--identifier-uris` Werte müssen nicht auf einer Webseite der im Internet gehostet; Sie müssen nur eindeutige URIs.
    
    Die zurückgegebenen Daten speichern Sie __AppId__ -Wert.
    
        data:    AppId:          4fd39843-c338-417d-b549-a545f584a745
        data:    ObjectId:       4f8ee977-216a-45c1-9fa3-d023089b2962
        data:    DisplayName:    exampleapp
        ...
        info:    ad app create command OK
    
3. Erstellen Sie einen Service principal mit __AppId__ -Wert zurückgegeben.

        azure ad sp create 4fd39843-c338-417d-b549-a545f584a745
        
     Die zurückgegebenen Daten speichern Sie den __Objekt-Id__ -Wert.
     
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
4. Der Service principal mit dem __Objekt-ID__ -Wert zurückgegeben weisen Sie __Besitzerrolle zu__ . Sie müssen auch die __Abonnement-ID__ verwenden, die Sie zuvor erworben.
    
        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Owner -c /subscriptions/{SubscriptionID}/
        
    Sobald dieser Befehl abgeschlossen ist, hat Service principal Besitzerzugriff auf die angegebene Abonnement-ID.

##<a name="get-an-authentication-token"></a>Ein Authentifizierungstoken abrufen

1. Verwenden Sie folgende __Mandanten-ID__ für Ihr Abonnement finden.

        azure account show -s <subscription ID>
        
    Die zurückgegebenen Daten suchen Sie die __Mandanten-ID__.
    
        info:    Executing command account show
        data:    Name                        : MyAzureAccount
        data:    ID                          : 45a1014d-0f27-25d2-b838-b8f373d6d52e
        data:    State                       : Enabled
        data:    Tenant ID                   : 22f988bf-56f1-41af-91ab-3d7cd011db47
        data:    Is Default                  : true
        data:    Environment                 : AzureCloud
        data:    Has Certificate             : No
        data:    Has Access Token            : Yes
        data:    User name                   : myname@contoso.org
        data:    
        info:    account show command OK

2. Erstellen Sie ein neues Token mit der Azure-REST-API.

        curl -X "POST" "https://login.microsoftonline.com/TenantID/oauth2/token" \
        -H "Cookie: flight-uxoptin=true; stsservicecookie=ests; x-ms-gateway-slice=productionb; stsservicecookie=ests" \
        -H "Content-Type: application/x-www-form-urlencoded" \
        --data-urlencode "client_id=AppID" \
        --data-urlencode "grant_type=client_credentials" \
        --data-urlencode "client_secret=password" \
        --data-urlencode "resource=https://management.azure.com/"
    
    Ersetzen Sie __TenantID__ __AppID__und __Kennwort__ durch Werte abgerufen oder früher.

    Wenn diese Anforderung erfolgreich ist, erhalten Sie eine Antwort 200 Serie und der Antworttext ein JSON-Dokument enthält.

    Diese Anforderung zurückgegebene JSON-Dokument enthält ein Element mit dem Namen __Zugriffstoken__. der Wert dieses Elements ist das Zugriffstoken, das Sie Anfragen in den nächsten Abschnitten dieses Dokuments verwendeten Authentifizierung verwenden müssen.
    
        {
            "token_type":"Bearer",
            "expires_in":"3599",
            "expires_on":"1463409994",
            "not_before":"1463406094",
            "resource":"https://management.azure.com/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWoNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI2Ny8iLCJpYXQiOjE0NjM0MDYwOTQsIm5iZiI6MTQ2MzQwNjA5NCwiZXhwIjoxNDYzNDA5OTk5LCJhcHBpZCI6IjBlYzcyMzM0LTZkMDMtNDhmYi04OWU1LTU2NTJiODBiZDliYiIsImFwcGlkYWNyIjoiMSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0Ny8iLCJvaWQiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJzdWIiOiJlNjgxZTZiMi1mZThkLTRkZGUtYjZiMS0xNjAyZDQyNWQzOWYiLCJ0aWQiOiI3MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDciLCJ2ZXIiOiIxLjAifQ.nJVERbeDHLGHn7ZsbVGBJyHOu2PYhG5dji6F63gu8XN2Cvol3J1HO1uB4H3nCSt9DTu_jMHqAur_NNyobgNM21GojbEZAvd0I9NY0UDumBEvDZfMKneqp7a_cgAU7IYRcTPneSxbD6wo-8gIgfN9KDql98b0uEzixIVIWra2Q1bUUYETYqyaJNdS4RUmlJKNNpENllAyHQLv7hXnap1IuzP-f5CNIbbj9UgXxLiOtW5JhUAwWLZ3-WMhNRpUO2SIB7W7tQ0AbjXw3aUYr7el066J51z5tC1AK9UC-mD_fO_HUP6ZmPzu5gLA6DxkIIYP3grPnRVoUDltHQvwgONDOw"
        }

##<a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Verwenden Sie folgende neue Ressourcengruppe erstellen. Vor der Erstellung von Ressourcen wie HDInsight-Cluster müssen Sie die Gruppe zunächst erstellen. 

* Die Abonnement-ID erhalten beim Erstellen der Dienstprinzipalnamen ersetzen Sie __SubscriptionID__ .
* Ersetzen Sie im vorherigen Schritt erhalten Zugriffstoken __Zugriffstoken__ .
* Ersetzen Sie __DataCenterLocation__ durch das Rechenzentrum in Ressourcengruppe und Ressourcen erstellen möchten. Beispielsweise 'South Central US'. 
* Ersetzen Sie __ResourceGroupName__ durch den Namen für diese Gruppe verwenden möchten:

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName?api-version=2015-01-01" \
    -H "Authorization: Bearer AccessToken" \
    -H "Content-Type: application/json" \
    -d $'{
"location": "DataCenterLocation"
}'
```

Wenn diese Anforderung erfolgreich ist, erhalten Sie eine Antwort 200 Serie und Antworttext ein JSON-Dokument mit Informationen über die Gruppe enthält. Die `"provisioningState"` Element enthält den Wert `"Succeeded"`.

##<a name="create-a-deployment"></a>Eine Bereitstellung erstellen

Verwenden Sie die folgenden Clusterkonfiguration (Vorlage und Parameterwerten) Bereitstellen der Ressourcengruppe.

* Ersetzen Sie __SubscriptionID__ und __Zugriffstoken__ mit Werte. 
* Ersetzen Sie im vorherigen Abschnitt erstellten Ressourcengruppenname __ResourceGroupName__ .
* Ersetzen Sie __DeploymentName__ durch den Namen für die Bereitstellung verwenden möchten.

```
curl -X "PUT" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json" \
-d "{set your body string to the template and parameters}"
```

> [AZURE.NOTE] Falls das JSON-Dokument mit der Vorlage Parameter in einer Datei gespeichert haben, können Sie die folgenden `-d "{ template and parameters}"`:
>
> `--data-binary "@/path/to/file.json"`

Wenn diese Anforderung erfolgreich ist, erhalten Sie eine Antwort 200 Serie und Antworttext enthält ein JSON-Dokument mit Informationen über den Bereitstellungsvorgang.

> [AZURE.IMPORTANT] Beachten Sie, dass die Bereitstellung wurde übermittelt, aber zu diesem Zeitpunkt nicht abgeschlossen. Es dauert einige Minuten, normalerweise ca. 15 für die Bereitstellung abgeschlossen.

##<a name="check-the-status-of-a-deployment"></a>Überprüfen Sie den Status der Bereitstellung

Verwenden Sie folgende Schritte aus, um den Status der Bereitstellung überprüfen:

* Ersetzen Sie __SubscriptionID__ und __Zugriffstoken__ mit Werte. 
* Ersetzen Sie im vorherigen Abschnitt erstellten Ressourcengruppenname __ResourceGroupName__ .

```
curl -X "GET" "https://management.azure.com/subscriptions/SubscriptionID/resourcegroups/ResourceGroupName/providers/microsoft.resources/deployments/DeploymentName?api-version=2015-01-01" \
-H "Authorization: Bearer AccessToken" \
-H "Content-Type: application/json"
```

Dadurch wird ein JSON-Dokument mit Informationen über den Bereitstellungsvorgang Informationen zurückgegeben. Die `"provisioningState"` Element enthält den Status der Bereitstellung; Dieser Wert enthält `"Succeeded"`, und die Bereitstellung erfolgreich war. Zu diesem Zeitpunkt sollte Ihr Cluster zur Verfügung.

##<a name="next-steps"></a>Nächste Schritte

Damit Sie erfolgreich einen HDInsight-Cluster erstellt haben, anhand der folgenden Informationen zum Cluster arbeiten. 

###<a name="hadoop-clusters"></a>Hadoop-Cluster

* [Struktur mit HDInsight verwenden](hdinsight-use-hive.md)
* [Verwenden Sie Schwein mit HDInsight](hdinsight-use-pig.md)
* [Verwenden Sie MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase-Cluster

* [Erste Schritte mit HBase auf HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Java-Anwendungsentwicklung für HBase auf HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Storm-Cluster

* [Entwickeln von Java-Topologien für auf HDInsight](hdinsight-storm-develop-java-topology.md)
* [Python-Komponenten in auf HDInsight verwenden](hdinsight-storm-develop-python-topology.md)
* [Bereitstellen und Überwachen von Topologien mit auf HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
