
<properties
   pageTitle="Erstellen Sie einen sicheren Service Fabric-Cluster mit Azure Resource Manager | Microsoft Azure"
   description="Dieser Artikel beschreibt das Einrichten einer sicheren Service Fabric-Cluster in Azure mithilfe von Azure-Ressourcen-Manager, Azure Key Vault und Azure Active Directory (AAD) für die Clientauthentifizierung."
   services="service-fabric"
   documentationCenter=".net"
   authors="chackdan"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="create-a-service-fabric-cluster-in-azure-using-azure-resource-manager"></a>Erstellen Sie einen Cluster Service Fabric in Azure Azure-Ressourcen-Manager verwenden

> [AZURE.SELECTOR]
- [Azure Ressourcenmanager](service-fabric-cluster-creation-via-arm.md)
- [Azure-portal](service-fabric-cluster-creation-via-portal.md)

Dies ist eine schrittweise Anleitung, die durch die Schritte zum Einrichten einer sicheren Azure Service Fabric Clusters in Azure Azure-Ressourcen-Manager mit geführt. Diese Anleitung führt Sie durch die folgenden Schritte aus:

 - Richten Sie Key Vault Schlüssel für Cluster und Anwendung verwalten.
 - Erstellen Sie einen gesicherten Cluster in Azure mit Azure-Ressourcen-Manager.
 - Benutzerauthentifizierung mit Azure Active Directory (AAD) für die Clusterverwaltung.

Ein sicherer Cluster ist ein Cluster, der verhindert den unberechtigten Zugriff auf Management-Operationen bereitstellen, aktualisieren und Löschen von Applikationen, Services und der darin enthaltenen Daten. Unsicherer Cluster ist ein Cluster, dass jeder jederzeit verbinden und Verwaltungsvorgänge ausführen. Obwohl einen unsicheren Cluster erstellen kann, ist es **dringend empfohlen, einen sicheren Cluster erstellen**. Eine unsichere Cluster, **nicht später gesichert** ist - muss ein neuer Cluster erstellt werden.

Die Konzepte sind für sichere Cluster erstellen, ob der Linux-Cluster oder Windows-Cluster sind. Weitere Informationen und Helfer Skripts zum Erstellen von sicheren Linux-Clustern finden Sie unter [Erstellen sicherer Cluster unter Linux](#secure-linux-clusters)

## <a name="log-in-to-azure"></a>Melden Sie sich bei Azure
[Azure PowerShell]verwendet[azure-powershell]. Beim Starten einer neuen Sitzung PowerShell Azure-Konto anmelden und vor Azure Befehle ausführen wählen.

Melden Sie sich bei Ihrem Azure-Konto:

```powershell
Login-AzureRmAccount
```

Wählen Sie Ihr Abonnement:

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a>Schlüssel Depot einrichten

Dieser Abschnitt führt ein Schlüssel Depot für einen Cluster Service Fabric in Azure und Service Fabric-Anwendung erstellen. Eine vollständige Anleitung auf Schlüssel finden Sie unter [Key Vault erste Schritte mit][key-vault-get-started].

Service Fabric verwendet x. 509-Zertifikate Sichern eines Clusters und Anwendung von Sicherheitsfeatures. Azure Key Vault zum Verwalten von Zertifikaten für Cluster in Azure Fabric Service. Bei der Bereitstellung eines Clusters in Azure Azure Ressourcenprovider verantwortlich für das Erstellen von Service Fabric-Cluster Zertifikate von Key Vault abruft und im Cluster virtuellen Computer installiert.

Das folgende Diagramm veranschaulicht die Beziehung zwischen Schlüssel Depot Service Fabric-Cluster und Azure Ressourcenanbieter, die Zertifikate im Schlüssel Depot gespeichert, wenn ein Cluster erstellt wird:

![Zertifikat installieren][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Der erste Schritt ist eine Ressourcengruppe für Schlüssel Depot erstellen. Inbetriebnahme einer eigenen Ressourcengruppe Schlüssel Depot wird empfohlen. Dadurch entfernen Sie die Computing- und Ressourcengruppen, einschließlich der Ressourcengruppe, die den Service Fabric-Cluster ohne Schlüssel und geheime Schlüssel. Die Ressourcengruppe, die den Schlüsseltresor muss im Bereich des Clusters, die verwendet wird.

```powershell

    New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    
    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a>Schlüsseltresor erstellen 

Erstellen Sie ein Depot Schlüssel in die neue Ressourcengruppe Der Schlüssel Depot **Bereitstellung aktiviert werden muss** den Ressourcenanbieter Service Fabric zu Zertifikaten und installieren auf Clusterknoten:

```powershell

    New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment
    
    
    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions to Keys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions to Secrets   :    all
    
    
    Tags                             :
```

Haben Sie einen vorhandenen Schlüssel Depot, können Sie es zur Bereitstellung mit Azure CLI aktivieren:

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```

<a id="add-certificate-to-key-vault"></a>
## <a name="add-certificates-to-key-vault"></a>Schlüsseltresor Zertifikate hinzufügen

Zertifikate werden in Service Fabric zur Authentifizierung und Verschlüsselung auf verschiedene Aspekte von einem Cluster und seine Anwendung sichern. Finden Sie weitere Informationen zur Verwendung von Zertifikaten in Service Fabric [Service Fabric-Cluster Sicherheitsszenarios][service-fabric-cluster-security].

### <a name="cluster-and-server-certificate-required"></a>Cluster und Server Certificate (erforderlich) 

Dieses Zertifikat muss einen Cluster sichern und nicht autorisierten Zugriff zu verhindern. Es bietet Clustersicherheit auf wenige Arten:
 
 - **Cluster-Authentifizierung:** Authentifiziert die Kommunikation zwischen Knoten, für den Cluster-Verbund. Nur Knoten, die mit diesem Zertifikat ausweisen können, können dem Cluster beitreten.
 - **Authentifizierung:** Authentifiziert Cluster-Management-Endpunkte an einen Client Management, Kommunikation mit den richtigen Cluster Management Client kennt. Dieses Zertifikat enthält auch SSL für HTTPS-Management-API und Service Fabric-Explorer über HTTPS.

Für diese Zwecke, muss das Zertifikat folgenden Vorschriften erfüllen:

 - Das Zertifikat muss einen privaten Schlüssel enthalten.
 - Das Zertifikat muss für den Schlüsselaustausch exportiert eine privater Informationsaustausch (PFX) erstellt werden.
 - Das Zertifikat muss die Domäne Zugriff auf den Service Fabric-Cluster verwendet. Diese Matchng ist erforderlich, um SSL für HTTPS-Management-Endpunkte des Clusters Service Fabric-Explorer bereitzustellen. Ein SSL-Zertifikat von einer Zertifizierungsstelle (CA) kann nicht abgerufen werden, für die `.cloudapp.azure.com` Domäne. Sie müssen einen benutzerdefinierten Domänennamen für den Cluster erwerben. Wenn Sie ein von einer Zertifizierungsstelle Zertifikat muss Antragstellernamen des Zertifikats den benutzerdefinierten Domänennamen für den Cluster verwendet.

### <a name="application-certificates-optional"></a>Anwendung Zertifikate (optional)

Eine beliebige Anzahl zusätzlicher Zertifikate kann in einem Cluster aus Sicherheitsgründen Anwendung installiert. Beachten Sie vor dem Erstellen des Clusters Sicherheitsszenarios Anwendung, die ein Zertifikat z. B. auf den Knoten installiert werden müssen:

 - Verschlüsselung und Entschlüsselung der Anwendung Konfigurationswerte
 - Verschlüsselung von Daten auf Knoten während der Replikation 

### <a name="formatting-certificates-for-azure-resource-provider-use"></a>Formatierung Zertifikate Azure Ressource Anbieter

Private Dateien (PFX) können hinzugefügt und direkt über Schlüsseltresor verwendet werden. Azure Ressourcenanbieter jedoch Schlüssel in einem speziellen JSON-Format gespeichert werden, die die PFX-Datei als Base-64 codierte Zeichenfolge und das Kennwort für den privaten Schlüssel. Um diese Bedürfnisse Schlüssel in eine JSON-Zeichenfolge eingefügt und dann als *Schlüssel* Schlüssel Depot gespeichert.

Zur Vereinfachung dieses Vorgangs ein PowerShell-Modul zur [GitHub][service-fabric-rp-helpers]. Führen Sie diese Schritte, um das Modul zu verwenden:

  1. Downloaden Sie den gesamten Inhalt des Repo in ein lokales Verzeichnis. 
  2. Importieren Sie das Modul im PowerShell-Fenster:

  ```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
  ```
     
Die `Invoke-AddCertToKeyVault` Befehl in diesem Modul PowerShell automatisch privaten Zertifikatschlüssel in eine JSON-Zeichenfolge formatiert und Schlüssel Tresor hochgeladen. Verwenden sie Schlüssel Depot Cluster-Zertifikat und alle weiteren Zertifikate hinzu. Wiederholen Sie diesen Schritt für alle weiteren Zertifikate im Cluster installieren möchten.

```powershell
 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"
    
    Switching context to SubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: The output object type of this cmdlet is going to be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret to myvault in vault myvault
    
    
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

Die obigen Zeichenfolgen sind alle Schlüssel Vault-Komponenten für das Konfigurieren einer Vorlage Service Fabric Cluster Resource Manager, die Zertifikate für Knoten Authentifizierung, Management Endgeräte-Sicherheit und Authentifizierung installiert und zusätzliche Sicherheitsfunktionen, die x. 509-Zertifikate verwenden. Zu diesem Zeitpunkt sollten Sie in Azure jetzt das folgende Setup haben:

 - Key Vault-Ressourcengruppe
   - Key Vault
     - Cluster-Serverauthentifizierungszertifikat
     - Anwendung Zertifikate

## <a name="set-up-azure-active-directory-for-client-authentication"></a>Einrichten von Azure Active Directory für die Clientauthentifizierung

AAD ermöglicht Organisationen (Mieter genannt) Benutzerzugriff auf Applikationen mit einer Web-basierten Anmeldeoberfläche geteilt und mit einer einheitlichen Clientumgebung verwalten. In diesem Dokument wird angenommen, dass einen Mieter bereits erstellt haben. Wenn nicht, wie [ein Mieter Azure Active Directory zu]starten[active-directory-howto-tenant].

Service Fabric-Cluster bietet mehrere Einstiegspunkte für die Management-Funktionen, einschließlich Web-basierten [Service Fabric-Explorer] [ service-fabric-visualizing-your-cluster] und [Visual Studio][service-fabric-manage-application-in-visual-studio]. Daher erstellen Sie zwei AAD-Anwendung zur Steuerung des Zugriffs auf den Cluster, eine Webanwendung und eine systemeigene Anwendung.

Vereinfachung einiger Schritte AAD Service Fabric-Cluster konfigurieren, haben wir eine Reihe von Windows PowerShell-Skripts erstellt.

>[AZURE.NOTE] Sie müssen diese Schritte *vor* der Clustererstellung so in Fällen, in denen Skripts, Clusternamen und Endpunkte erwarten, sollten nicht die bereits erstellte geplante Werte sein.

1. [Die Skripts downloaden] [ sf-aad-ps-script-download] auf Ihren Computer.

2. Mit der rechten Maustaste der Zip-Datei, wählen Sie **Eigenschaften**, aktivieren Sie das Kontrollkästchen **Zulassen** und anwenden.

3. Extrahieren Sie die Zip-Datei.

4. Führen Sie `SetupApplications.ps1`, TenantId, Clusternamen und WebApplicationReplyUrl als Parameter bereitstellen. Zum Beispiel:

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    Finden Sie Ihre **TenantId** PowerShell-Befehl "Get-AzureSubscription" ausführen ". **TenantId** für jede Subskription wird angezeigt.

    **ClusterName** wird vom Skript erstellten AAD-Applikationen Präfix. Es muss nicht den tatsächliche Clustername entspricht genau nur erleichtern Sie AAD Artefakte Cluster Service Fabric zuordnen, die sie mit verwendet werden soll.

    **WebApplicationReplyUrl** ist der Standardendpunkt AAD nach Abschluss der Anmeldung an die Benutzer zurückgegeben. Dieser sollte Service Fabric-Explorer-Endpunkt für den Cluster festlegen werden:

    https://&lt;Cluster_domain&gt;: 19080-Explorer

    Sie werden aufgefordert, ein Konto anmelden, das Administratorberechtigungen für AAD-Mandanten. Anschließend wird das Skript zum Erstellen von Web und systemeigene Anwendung Service Fabric-Cluster dargestellt. Beim Betrachten des Mieters Anwendung in [Azure-Verwaltungsportal][azure-classic-portal], zwei neue Einträge sollte angezeigt werden:

    - *ClusterName*\_Cluster
    - *ClusterName*\_Client

    Das Skript druckt erforderliche Vorlage Azure-Ressourcen-Manager bei der Erstellung des Clusters im nächsten Abschnitt Json so PowerShell geöffnet halten.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a>Erstellen einer Vorlage Service Fabric Cluster Resource Manager

In diesem Abschnitt die Ausgabe des obigen PowerShell-Befehlen in einer Vorlage Service Fabric-Cluster Resource Manager verwendet.

Beispiel-Ressourcen-Manager Vorlagen stehen in [Azure Schnellstart-Vorlagenkatalog auf GitHub][azure-quickstart-templates]. Diese Vorlagen können als Ausgangspunkt für die Cluster-Vorlage verwendet. 

### <a name="create-the-resource-manager-template"></a>Ressourcen-Manager erstellen

[Sichere Cluster mit 5 Knoten] verwendet[ service-fabric-secure-cluster-5-node-1-nodetype-wad] Beispielvorlage und Vorlagenparameter. Download `azuredeploy.json` und `azuredeploy.parameters.json` auf Ihren Computer und öffnen Sie beide Dateien in Ihrem bevorzugten Editor.

### <a name="add-certificates"></a>Zertifikate hinzufügen

Zertifikate werden Cluster Resource Manager Vorlage hinzugefügt, durch einen Verweis auf den Schlüsseltresor mit dem Schlüssel. Es wird empfohlen, diese Schlüssel Depot Werte in Ressourcen-Manager-Vorlagendatei Parameter zum Ressourcen-Manager wieder verwendbare Vorlage-Datei und der Werte für eine Bereitstellung befinden.

#### <a name="add-all-certificates-to-the-vmss-osprofile"></a>VMSS OsProfile alle Zertifikate hinzufügen

Alle Zertifikate, die im Cluster installiert werden, muss im Abschnitt OsProfile der Ressource VMSS (Microsoft.Compute/virtualMachineScaleSets) konfiguriert werden. Dies weist den Ressourcenanbieter auf den virtuellen Computern installieren. Dazu gehören die Cluster sowie alle Anwendung Sicherheitszertifikate für Ihre Anwendung verwenden möchten:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-service-fabric-cluster-certificate"></a>Service Fabric-Cluster Zertifikat konfigurieren

Das Authentifizierungszertifikat Cluster muss auch Service Fabric-Clusterressource (Microsoft.ServiceFabric/clusters) und die Service Fabric-Erweiterung für VMSS in der VMSS konfiguriert werden. Dadurch können Service Fabric Ressourcenanbieter für Cluster-Authentifizierung und Authentifizierung für Management-Endpunkte konfigurieren.

##### <a name="vmss-resource"></a>VMSS Ressourcen:

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a>Service Fabric-Ressourcen:

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-aad-config"></a>AAD Config einfügen

Die zuvor erstellte AAD Konfiguration direkt in der Ressourcen-Manager-Vorlage eingefügt werden jedoch empfohlen wird, die Werte zunächst in eine Datei zu der Ressourcen-Manager-Vorlage wiederverwendet und frei von Werten bestimmte einer Bereitstellung Parameter extrahieren.

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### < ein "Konfigurieren-Arm" ></a>Vorlagenparameter Ressource-Manager konfigurieren

Verwenden Sie die Ausgabewerte der Schlüssel Depot und AAD PowerShell Befehle darin, die Datei schließlich:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
An diesem Punkt sollten Sie jetzt den folgenden haben:

 - Key Vault-Ressourcengruppe
    - Key Vault
    - Cluster-Serverauthentifizierungszertifikat
    - Daten verschlüsseln Zertifikat
 - Azure Active Directory Mieter 
    - AAD Anwendung für Web-basiertes Management und Service Fabric-Explorer
    - AAD Anwendung für native Client-management
    - Benutzer mit Rollen 
 - Service Fabric-Cluster Resource Manager Vorlage
    - Zertifikate über Schlüssel Depot
    - Azure Active Directory konfiguriert 

Das folgende Diagramm zeigt Schlüssel Depot und AAD Konfiguration, in der Ressourcen-Manager-Vorlage entsprechen.

![Ressourcenmanager Abhängigkeit Karte][cluster-security-arm-dependency-map]

## <a name="create-the-cluster"></a>Erstellen des Clusters

Können nun [ARM]Bereitstellung Cluster erstellen[resource-group-template-deploy].

#### <a name="test-it"></a>Testen

Verwenden Sie den folgenden PowerShell-Befehl zum Testen der Ressourcen-Manager-Vorlage einer Datei:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a>Bereitstellen

Verwenden Sie der Ressourcenmanager Vorlage bestanden den folgenden PowerShell-Befehl bereitstellen die Ressourcen-Manager-Vorlage mit einer Datei:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>
## <a name="assign-users-to-roles"></a>Zuweisen von Benutzern zu Rollen

Erstellte Anwendung Cluster darstellen müssen durch Service unterstützten Rollen Benutzer zuweisen: schreibgeschützt und Admin. Sie können dazu das [klassische Azure-Portal][azure-classic-portal].

1. Navigieren Sie zu Ihrem Mandanten und wählen Sie Applikationen.
2. Wählen Sie die Webanwendung hat einen Namen wie `myTestCluster_Cluster`.
3. Klicken Sie auf die Registerkarte Benutzer.
4. Wählen Sie einen Benutzer und klicken Sie auf **zuweisen** am unteren Bildschirmrand.

    ![Zuweisen von Benutzern zu Rollen-Taste][assign-users-to-roles-button]

5. Wählen Sie die Rolle für den Benutzer.

    ![Zuweisen von Benutzern zu Rollen][assign-users-to-roles-dialog]

>[AZURE.NOTE] Weitere Informationen zu Rollen in Service Fabric finden Sie unter [Rollenbasierte Zugriffskontrolle für Fabric Service Clients](service-fabric-cluster-security-roles.md).

 <a name="secure-linux-cluster"></a> 
##  <a name="create-secure-clusters-on-linux"></a>Erstellen Sie sichere Cluster unter Linux

Um den Prozess zu vereinfachen, ein Helfer Skript bereitgestellt wurden [hier](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux). Dieses Hilfsprogramm verwenden, wird davon ausgegangen, dass schon Azure-CLI installiert und wird in Ihrem Pfad. Stellen Sie sicher, dass das Skript mit Ausführungsberechtigungen für `chmod +x cert_helper.py` nach dem herunterladen. Der erste Schritt besteht in der Azure-Konto CLI mit sich die `azure login` Befehl. Verwenden Sie nach der Azure-Konto anmelden, den Helfer mit der Zertifizierungsstelle signiertes Zertifikat wie der folgende Befehl zeigt:

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]

The -ifile parameter can take a .pfx or a .pem file as input, with the certificate type (pfx or pem, or ss if it is a self-signed cert).
The parameter -h prints out the help text.
```

Dieser Befehl gibt die folgenden drei Zeichenfolgen als Ausgabe: 

1. Eine SourceVaultID, die die ID für das neue KeyVault ResourceGroup für Sie erstellt. 

2. Ein CertificateUrl für den Zugriff auf das Zertifikat.

3. Ein CertificateThumbprint, das für die Authentifizierung verwendet wird.


Im folgenden Beispiel wird veranschaulicht, wie der Befehl:

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
Vorherigen Befehl bietet drei Zeichenfolgen wie folgt:

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

 Das Zertifikat muss die Domäne Zugriff auf den Service Fabric-Cluster verwendet. Dies ist erforderlich, um SSL für HTTPS-Management-Endpunkte des Clusters Service Fabric-Explorer bereitzustellen. Ein SSL-Zertifikat von einer Zertifizierungsstelle (CA) kann nicht abgerufen werden, für die `.cloudapp.azure.com` Domäne. Sie müssen einen benutzerdefinierten Domänennamen für den Cluster erwerben. Wenn ein von einer Zertifizierungsstelle Zertifikat muss das Zertifikat den benutzerdefinierten Domänennamen für den Cluster verwendet.

Die Einträge zum Erstellen eines sicheren Service Fabric-Clusters (ohne AAD) beschriebenen [Ressourcenmanager konfigurieren Vorlage](#configure-arm)Parameter erforderlich sind. Sie können sichere Cluster über an [Client Zugriffssteuerungsmechanismus zu einem Cluster](service-fabric-connect-to-secure-cluster.md)herstellen. Linux-Clustern, Vorschau unterstützt AAD-Authentifizierung nicht. Sie können Rollen Administrator und Client gemäß Abschnitt [Rollen Benutzer zuweisen](#assign-roles). Wenn Sie Rollen für Linux Vorschau Cluster Administrator und Client angeben, müssen Sie bieten Zertifikatfingerabdruck für die Authentifizierung (im Gegensatz zu Antragstellername, da keine Überprüfung oder Sperrung in dieser Vorabversion ausgeführt wird).


Wenn Sie ein selbstsigniertes Zertifikat für Tests verwenden möchten, können dasselbe Skript ein selbstsigniertes Zertifikat erstellen und hochladen, Schlüsseltresor, mit dem Flag `ss` anstatt der Pfad und Name des Zertifikats. Finden Sie beispielsweise den folgenden Befehl zum Erstellen und Hochladen eines selbstsignierten Zertifikats:

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net" 
```

Dieser Befehl gibt die gleichen drei Zeichenfolgen, SourceVault, CertificateUrl und CertificateThumbprint, die zum Erstellen eines sicheren Linux Clusters sowie den Speicherort das selbstsignierte Zertifikat platziert verwendet wird. Sie benötigen das selbstsignierte Zertifikat herstellen.  Sie können sichere Cluster über an [Client Zugriffssteuerungsmechanismus zu einem Cluster](service-fabric-connect-to-secure-cluster.md)herstellen. Das Zertifikat muss die Domäne Zugriff auf den Service Fabric-Cluster verwendet. Dies ist erforderlich, um SSL für HTTPS-Management-Endpunkte des Clusters Service Fabric-Explorer bereitzustellen. Ein SSL-Zertifikat von einer Zertifizierungsstelle (CA) kann nicht abgerufen werden, für die `.cloudapp.azure.com` Domäne. Sie müssen einen benutzerdefinierten Domänennamen für den Cluster erwerben. Wenn ein von einer Zertifizierungsstelle Zertifikat muss das Zertifikat den benutzerdefinierten Domänennamen für den Cluster verwendet.

Im Portal können vom Helfer Skript bereitgestellten Parameter ausgefüllt werden im Abschnitt [Cluster im Azure-Portal erstellen](service-fabric-cluster-creation-via-portal.md#create-cluster-portal).

## <a name="next-steps"></a>Nächste Schritte

Zu diesem Zeitpunkt haben Sie einen sicheren Cluster mit Azure Active Directory bietet Management-Authentifizierung. Weiter [zum Cluster herstellen](service-fabric-connect-to-secure-cluster.md) und [Anwendung Kennwörter](service-fabric-application-secret-management.md)verwalten.

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a>Problembehandlung bei Azure Active Directory für die Clientauthentifizierung einrichten

Wenn ein Problem beim Einrichten von Azure Active Directory für die Clientauthentifizierung auftreten, überprüfen Sie die folgenden Suggestings für Lösungen.

### <a name="service-fabric-explorer-prompts-for-selecting-certificate"></a>Service Fabric-Explorer fordert auswählen

#### <a name="problem"></a>Problem

Nach Anmeldung erfolgreich auf AAD Anmeldeseite in Service Fabric-Explorer Browser Homepage wieder ohne ein Dialogfeld zur Auswahl eines Zertifikats aufgefordert.

![SFX Zertifikat auswählen-Dialogfeld][sfx-select-certificate-dialog]

#### <a name="reason"></a>Grund

Der Benutzer wurde nicht bei AAD Clusteranwendung zugewiesen. Daher fehl AAD Service Fabric-Cluster. Service Fabric-Explorer wird Zertifikatauthentifizierung.

#### <a name="solution"></a>Lösung

Die Anweisungen AAD einrichten und Zuweisen von Benutzerrollen. "Benutzer Aufgaben erforderlich, ACCESS APP" wird empfohlen als eingeschaltet `SetupApplications.ps1` wird.

### <a name="connect-with-powershell-fails-with-error-the-specified-credentials-are-invalid"></a>Verbinden mit PowerShell schlägt fehl mit Fehler: die angegebenen Anmeldeinformationen sind ungültig

#### <a name="problem"></a>Problem

Wenn PowerShell Verbindung zum Cluster mit "AzureActiveDirectory" Sicherheitsmodus verwenden, nach Anmeldung erfolgreich auf AAD Anmeldeseite Verbindung schlägt fehl mit Fehler: die angegebenen Anmeldeinformationen sind ungültig angezeigt.

#### <a name="solution"></a>Lösung

Dasselbe wie oben.

### <a name="service-fabric-explorer-signing-in-return-failure-aadsts50011"></a>Service Fabric-Explorer Unterzeichnung im Gegenzug Fehler: AADSTS50011

#### <a name="problem"></a>Problem

Nach der Anmeldung auf AAD Anmeldeseite in Service Fabric-Explorer gibt Seite Anmeldung Fehler - AADSTS50011: die Antwortadresse &lt;Url&gt; für die Anwendung konfiguriert wurde Antwortadressen stimmt nicht überein: &lt;Guid&gt;. 

![SFX Antwortadresse nicht überein][sfx-reply-address-not-match]

#### <a name="reason"></a>Grund

Die cluster(web) Anwendung Service Fabric-Explorer versucht AAD Authentifizierung darstellt wird als Teil der Anforderung die URL umleiten. Jedoch wird nicht in der Liste "Antwort-URL" AAD aufgeführt.

#### <a name="solution"></a>Lösung

Fügen Sie Url des Service Fabric-Explorer hinzu "Antwort-URL" in der Registerkarte Konfigurieren cluster(web) Anwendung oder Ersetzen Sie eines der Elemente in der Liste. Speichern.

![Web Application Antwort-url][web-application-reply-url]

### <a name="can-i-reuse-the-same-aad-tenant-for-multiple-clusters"></a>Kann ich dieselbe AAD Pächter für mehrere Cluster verwenden?

#### <a name="answer"></a>Antwort

Ja. Aber die URL des Service Fabric-Explorer zur Anwendung cluster(web) fügen Sie andernfalls Service Fabric-Explorer funktioniert nicht.

### <a name="why-do-i-still-need-server-certificate-while-aad-enabled"></a>Noch wozu Serverzertifikat aktivierten AAD?

#### <a name="answer"></a>Antwort

FabricClient und FabricGateway führen die gegenseitige Authentifizierung. Bei AAD Authentifizierung bietet AAD Integration Clientidentität Server und Server mit der Identität des Servers überprüfen. Weitere Informationen zur Funktionsweise von Zertifikat auf Service überprüfen Sie, [x. 509-Zertifikate und Service][x509-certificates-and-service-fabric]

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype-wad]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype-wad/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png