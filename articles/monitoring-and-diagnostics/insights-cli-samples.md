<properties
    pageTitle="Azure Monitor CLI Schnellstart-Beispiele. | Microsoft Azure"
    description="Beispiel-CLI-Befehle für Azure Monitor-Funktionen Azure Monitor ist ein Microsoft Azure können Sie Benachrichtigungen senden, Web-URLs basierend auf Werten der konfigurierten Telemetriedaten und Clouddienste skalieren, virtuellen Maschinen und Web Apps aufrufen."
    authors="kamathashwin"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor--cross-platform-cli-quick-start-samples"></a>Azure Monitor plattformübergreifende CLI Schnellstart-Beispiele

Dieser Artikel beschreibt Beispiel Befehlszeilenschnittstelle (CLI) Befehle zu Azure Monitor-Funktionen zugreifen. Azure-Monitor können Sie Clouddienste skalieren, virtuellen Maschinen und Web Apps und Benachrichtigungen senden oder rufen Web-URLs basierend auf Daten konfiguriert.

>[AZURE.NOTE] Azure Monitor ist der neue Name für die sogenannten "Azure Einblicke" bis zum 25. September 2016. Namespaces und somit die folgenden Befehle enthalten jedoch weiterhin die "Erkenntnisse".


## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn der Azure-CLI bereits installiert haben, finden Sie unter [Installieren der Azure-CLI](../xplat-cli-install.md). Wenn Sie mit Azure CLI nicht vertraut sind, erhalten Sie weitere Informationen finden sie unter [Verwenden der Azure-CLI für Mac, Linux und Windows Azure Ressourcenmanager](../xplat-cli-azure-resource-manager.md).


Installieren Sie unter Windows Npm von [Node.js-Website](https://nodejs.org/). Nach Abschluss die Installation als Administrator ausführen Berechtigungen mit CMD.exe führen Sie Folgendes aus dem Ordner, in dem Npm installiert ist:

```console
npm install azure-cli --global
```

Wechseln Sie nun zu jeder/Ordner möchten und geben Sie an der Befehlszeile:

```console
azure help
```

## <a name="log-in-to-azure"></a>Melden Sie sich bei Azure

Der erste Schritt ist der Azure-Konto anmelden.

```console
azure login
```

Nach Ausführung dieses Befehls müssen Sie über die Anleitung auf dem Bildschirm anmelden. Danach sehen Sie Konto, TenantId und Standard-Abonnement-Id. Alle Befehle arbeiten im Kontext der Ihr Standardabonnement.

Verwenden Sie den folgenden Befehl, um die Details Ihres aktuellen Abonnements anzuzeigen.

```console
azure account show
```

Um ein anderes Abonnement Arbeitskontext ändern, Befehl.

```console
azure account set "subscription ID or subscription name"
```

Um Azure Resource Manager und Azure Monitor Befehle verwenden, müssen Sie in Azure-Ressourcen-Manager-Modus.

```console
azure config mode arm
```

Führen Sie folgende Schritte aus, um eine Liste aller unterstützten Azure Monitor Befehle anzuzeigen.

```console
azure insights
```

## <a name="view-audit-logs-for-a-subscription"></a>Anzeigen von Überwachungsprotokollen für ein Abonnement

Führen Sie folgende Schritte aus, um eine Liste der Überwachungsprotokolle.

```console
azure insights logs list [options]
```

Versuchen Sie Folgendes, um alle verfügbaren Optionen anzuzeigen.

```console
azure insights logs list -help
```

Hier ist ein Beispiel für die Liste Protokolle nach einem resourceGroup

```console
azure insights logs list --resourceGroup "myrg1"
```

Beispiel zur Liste Protokolle vom Anrufer

```console
azure insights logs list --caller "myname@company.com"
```

Beispiel zur Liste Aufrufer auf einen Ressourcentyp innerhalb einer Start- und protokolliert

```console
azure insights logs list --resourceProvider "Microsoft.Web" --caller "myname@company.com" --startTime 2016-03-08T00:00:00Z --endTime 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Arbeiten mit Warnungen
Die Informationen können im Abschnitt Sie Alerts arbeiten.

### <a name="get-alert-rules-in-a-resource-group"></a>Abrufen von Warnregeln in einer Ressourcengruppe

```console
azure insights alerts rule list abhingrgtest123
azure insights alerts rule list abhingrgtest123 --ruleName andy0323
```

### <a name="create-a-metric-alert-rule"></a>Metrische Warnregel erstellen

```console
azure insights alerts actions email create --customEmails foo@microsoft.com
azure insights alerts actions webhook create https://someuri.com
azure insights alerts rule metric set andy0323 eastus abhingrgtest123 PT5M GreaterThan 2 /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Web-EastUS/providers/Microsoft.Web/serverfarms/Default1 BytesReceived Total
```

### <a name="create-a-log-alert-rule"></a>Erstellen einer Warnregel Protokoll

```console
azure insights alerts rule log set ruleName eastus resourceGroupName someOperationName
```

### <a name="create-webtest-alert-rule"></a>Webtest Warnregel erstellen

```console
azure insights alerts rule webtest set leowebtestr1-webtestr1 eastus Default-Web-WestUS PT5M 1 GSMT_AvRaw /subscriptions/b67f7fec-69fc-4974-9099-a26bd6ffeda3/resourcegroups/Default-Web-WestUS/providers/microsoft.insights/webtests/leowebtestr1-webtestr1
```

### <a name="delete-an-alert-rule"></a>Löschen einer Regel

```console
azure insights alerts rule delete abhingrgtest123 andy0323
```

## <a name="log-profiles"></a>Protokoll-Profile
Verwenden Sie die Informationen in diesem Abschnitt Arbeiten mit Profilen anmelden.

### <a name="get-a-log-profile"></a>Ein Protokoll Profil

```console
azure insights logprofile list
azure insights logprofile get -n default
```


### <a name="add-a-log-profile-without-retention"></a>Hinzufügen eines Profils Protokoll ohne Aufbewahrung

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Protokoll Profil entfernen

```console
azure insights logprofile delete --name default
```

### <a name="add-a-log-profile-with-retention"></a>Hinzufügen eines Profils Anmelden mit

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Hinzufügen eines Profils Protokoll mit Aufbewahrung und EventHub

```console
azure insights logprofile add --name default --storageId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/insightsintegration7777 --serviceBusRuleId /subscriptions/1a66ce04-b633-4a0b-b2bc-a912ec8986a6/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/testshoeboxeastus/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia --retentionInDays 90
```


## <a name="diagnostics"></a>Diagnose
Verwenden Sie die Informationen in diesem Abschnitt Arbeiten mit Diagnose.

### <a name="get-a-diagnostic-setting"></a>Eine Diagnose Einstellung

```console
azure insights diagnostic get --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp
```

### <a name="disable-a-diagnostic-setting"></a>Eine Diagnose Einstellung deaktivieren

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled false
```

### <a name="enable-a-diagnostic-setting-without-retention"></a>Aktivieren einer Einstellung Diagnose ohne Aufbewahrung

```console
azure insights diagnostic set --resourceId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/andyrg/providers/Microsoft.Logic/workflows/andy0315logicapp --storageId /subscriptions/df602c9c-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/shibanitesting --enabled true
```


## <a name="autoscale"></a>Automatische Skalierung
Verwenden Sie die Informationen in diesem Abschnitt Arbeiten mit skalieren. Sie müssen diese Beispiele ändern.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Einstellungen Sie Skalierungsgröße für eine Ressourcengruppe

```console
azure insights autoscale setting list montest2
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Namen in einer Ressourcengruppe Autoscale Einstellungen abrufen

```console
azure insights autoscale setting list montest2 -n setting2
```


### <a name="set-auotoscale-settings"></a>Auotoscale einstellen

```console
azure insights autoscale setting set montest2 -n setting2 --settingSpec
```
