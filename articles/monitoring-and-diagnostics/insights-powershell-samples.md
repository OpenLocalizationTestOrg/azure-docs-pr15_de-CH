<properties
    pageTitle="Azure Monitor PowerShell Schnellstart-Beispiele. | Microsoft Azure"
    description="Mithilfe von PowerShell Azure Monitor Funktionen wie skalieren, Alerts, Webhooks Aktivitäten suchen."
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
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Azure Monitor PowerShell Schnellstart-Beispiele

Dieser Artikel zeigt Beispiele von PowerShell-Befehlen zu Azure Monitor-Funktionen zugreifen. Azure-Monitor können Sie Clouddienste skalieren, virtuellen Maschinen und Web Apps und Benachrichtigungen senden oder rufen Web-URLs basierend auf Daten konfiguriert.

>[AZURE.NOTE] Azure Monitor ist der neue Name für die sogenannten "Azure Einblicke" bis zum 25. September 2016. Namespaces und somit die folgenden Befehle enthalten jedoch weiterhin die "Erkenntnisse".

## <a name="set-up-powershell"></a>Einrichten von PowerShell
Wenn nicht bereits geschehen soll PowerShell auf Ihrem Computer ausführen. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von PowerShell](../powershell-install-configure.md) .

## <a name="examples-in-this-article"></a>Beispiele in diesem Artikel

Die in Artikel Beispielen Verwendung Azure Monitor Cmdlets. Sie können auch die gesamte Liste der Azure-Monitor PowerShell-Cmdlets zur [Azure-Monitor (Erkenntnisse) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx)überprüfen.


## <a name="sign-in-and-use-subscriptions"></a>Anmelden und Abonnements

Zunächst Ihre Azure-Abonnement anmelden.

```PowerShell
Login-AzureRmAccount
```

Dazu müssen Sie sich anmelden. Nachdem Sie Ihr Konto ausführen, werden TenantId und Standard-Abonnement-Id angezeigt. Azure Cmdlets funktionieren dabei Ihr Standardabonnement. Die Liste der Abonnements haben Sie Zugriff auf den folgenden Befehl verwenden.

```PowerShell
Get-AzureRmSubscription
```

Verwenden Sie den folgenden Befehl, um Arbeitskontext in ein anderes Abonnement ändern.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Abrufen von Überwachungsprotokollen für ein Abonnement
Verwenden der `Get-AzureRmLog` Cmdlet.  Im folgenden sind einige Beispiele.

Beziehen Sie Protokolleinträge von diesem Zeitpunkt an:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Protokolleinträge zwischen Zeitpunkt zu erhalten:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Einträge aus einer Ressourcengruppe abrufen:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Einträge aus einer bestimmten Ressource zwischen Zeitpunkt abrufen:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Alle Protokolleinträge bestimmte Anrufer zu erhalten:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

Der folgende Befehl ruft die letzten 1000 Ereignisse aus dem Überwachungsprotokoll ab:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`viele Parameter unterstützt. Siehe die `Get-AzureRmLog` Weitere Informationen.

>[AZURE.NOTE] `Get-AzureRmLog`enthält nur 15 Tage. Mit dem **MaxEvents -** Parameter können Sie die letzten N Ereignisse 15 Tagen Abfragen. Älter als 15 Tage Zugriffsereignisse einsetzen Sie REST-API oder SDK (C#-Beispiel mit dem SDK). Wenn Sie keine **Startzeit**einfügen, ist der Standardwert **EndTime** minus 1 Stunde. Wenn Sie keine **Endzeit**einfügen, ist der Standardwert Uhrzeit. Alle Zeiten in UTC.

## <a name="retrieve-alerts-history"></a>Alerts-Verlauf abrufen
Um alle Warnungsereignisse anzuzeigen, können Sie anhand der folgenden Beispiele Azure-Ressourcen-Manager-Protokolle Abfragen.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Um den Verlauf einer bestimmten Regel anzuzeigen, können Sie die `Get-AzureRmAlertHistory` Cmdlets, die Ressourcen-ID der Warnregel übergeben.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

Die `Get-AzureRmAlertHistory` Cmdlet unterstützt verschiedene Parameter. Weitere Informationen finden Sie unter [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>Abrufen von Informationen über Warnregeln
Alle folgenden Befehle werden für eine Ressourcengruppe mit dem Namen "Montest".

Alle Eigenschaften der Warnregel anzeigen:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Alle Alarme auf einer Ressourcengruppe abrufen:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Rufen Sie alle Warnregeln für eine Ressource festlegen. Legen z. B. alle Warnregeln auf einem virtuellen Computer.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`andere Parameter unterstützt. Weitere Informationen finden Sie unter [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) .

## <a name="create-alert-rules"></a>Erstellen von Warnregeln
Sie können die `Add-AlertRule` -Cmdlet zum Erstellen, aktualisieren oder Deaktivieren einer Warnregel.

E-Mail- und Webhook Eigenschaften erstellen `New-AzureRmAlertRuleEmail` und `New-AzureRmAlertRuleWebhook`bzw.. Weisen Sie im Cmdlet Warnregel als Aktionen **Actions** -Eigenschaft der Warnregel zu.

Der nächste Abschnitt enthält ein Beispiel, das zeigt, wie eine Warnregel mit verschiedenen Parametern erstellen.

### <a name="alert-rule-on-a-metric"></a>Regel auf eine Metrik
Die folgende Tabelle beschreibt die Parameter und Werte zum Erstellen einer Warnung mithilfe einer Metrik verwendet.


|Parameter|Wert|
|---|---|
|Name|  simpletestdiskwrite|
|Speicherort der diese Warnungsregel|   Osten der USA|
|ResourceGroup| montest|
|TargetResourceId|  /Subscriptions/s1/resourceGroups/montest/Providers/Microsoft.Compute/virtualMachines/testconfig|
|MetricName der Warnung erstellt wird|   \PhysicalDisk (_Total) \Disk Schreibvorgänge pro Sekunde. Siehe die `Get-MetricDefinitions` Cmdlet unter Informationen zum genauen metrischen Namen abrufen|
|Operator|  Größer als|
|Schwellenwert (Anzahl/s, in diese Metrik)|    1|
|WindowSize (ss-Format)|  00:05:00|
|Aggregator (Statistik Metrik, die durchschnittliche Anzahl in diesem Fall verwendet)|  Durchschnitt|
|angepasste e-Mails (String Array)|'foo@example.com','bar@example.com'|
|e-Mail an Besitzer, Mitwirkende und Leser|    -SendToServiceOwners|

Erstellen einer e-Mail-Aktion

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Erstellen einer Aktion Webhook

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

CPU % Metrik auf einer klassischen VM in erstellen Sie der Warnregel

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Abrufen der Regel

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Warnung hinzufügen-Cmdlet wird die Regel außerdem aktualisiert, wenn eine Warnregel den angegebenen Eigenschaften bereits. Deaktivieren Sie eine Warnungsregel enthalten den Parameter **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>Ereignis im Sicherheitsüberwachungsprotokoll warnen

>[AZURE.NOTE] Dieses Feature ist noch in der Vorschau.

In diesem Szenario werden Sie e-Mail senden, wenn eine Website im Abonnement Ressource Gruppe *abhingrgtest123*erfolgreich gestartet wird.

Einrichten einer e-Mail-Regel

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Einrichten einer Regel webhook

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Erstellen Sie die Regel für das Ereignis

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

Abrufen der Regel

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

Die `Add-AlertRule` Cmdlet ermöglicht verschiedene Parameter. Weitere Informationen finden Sie unter [Add-AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Abrufen einer Liste der verfügbaren Metriken für Alarme
Sie können die `Get-AzureRmMetricDefinition` Cmdlet alle Kriterien für eine bestimmte Ressource anzeigen.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Im folgenden Beispiel wird eine Tabelle mit den Namen und die es.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Eine vollständige Liste der verfügbaren Optionen für `Get-AzureRmMetricDefinition` finden Sie unter [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).


## <a name="create-and-manage-autoscale-settings"></a>Erstellen Sie und verwalten Sie automatisch skalieren
Eine Ressource, wie eine Web app VM, Cloud-Dienst oder VM Skalierung festlegen können nur eine automatische Skalierung Einstellung konfiguriert.
Jede Einstellung skalieren kann jedoch mehrere Profile haben. Beispielsweise basiert eine Skalierung Leistungsbasierte Profil und ein Zeitplan für Profil. Jedes Profil können mehrere Regeln konfiguriert. Weitere Informationen zur automatischen Skalierung finden Sie unter [Skalieren einer Anwendung](../cloud-services/cloud-services-how-to-scale.md).

Hier sind die Schritte, die wir verwenden:

1. Erstellen Sie Regeln
2. Erstellen Sie Profile zuordnen Sie erstellten Regeln zuvor Profile.
3. Optional: Erstellen Sie durch Konfigurieren der Eigenschaften der Webhook und e-Mail-Benachrichtigungen für skalieren.
4. Erstellen einer Einstellung Skalieren mit einem Namen für die Zielressource durch Zuordnen von Profilen und Benachrichtigungen, die in den vorherigen Schritten erstellt.

Die folgenden Beispiele zeigen, erstellen eine Einstellung Skalieren für eine VM für ein Windows-Betriebssystem mithilfe der CPU-Auslastung Metrik festgelegt.

Erstellen Sie zunächst eine Regel skalieren, mit einer Instanz zählen.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Anschließend erstellen Sie eine Regel zum Skalieren-in mit einer Instanz Count.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Erstellen Sie ein Profil für die Regeln.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Erstellen Sie eine Webhook-Eigenschaft.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Erstellen Sie die Benachrichtigungseigenschaft Autoscale Einstellung, einschließlich e-Mail und Webhook, die Sie zuvor erstellt haben.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Erstellen Sie skalieren Einstellung oben erstellten Profil hinzufügen.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Weitere Informationen zu skalieren Einstellungen finden Sie unter [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Skalierungsgröße Verlauf
Das folgende Beispiel zeigt, wie Sie kürzlich skalieren und Warnungsereignisse anzeigen können. Audit Log Suchen mithilfe von Autoscale Verlauf anzeigen.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Sie können die `Get-AzureRmAutoScaleHistory` Cmdlet AutoScale Historie abrufen.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Weitere Informationen finden Sie unter [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Anzeigen von Details für eine Einstellung skalieren
Sie können die `Get-Autoscalesetting` -Cmdlet zum Abrufen von Informationen über die Einstellung skalieren.

Das folgende Beispiel zeigt Details zu allen Autoscale Einstellungen in der Ressource 'myrg1'.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Das folgende Beispiel zeigt Details aller skalieren in der Ressource "myrg1" und insbesondere Autoscale Einstellung mit dem Namen "MyScaleVMSSSetting".

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Entfernen einer Einstellung skalieren
Sie können die `Remove-Autoscalesetting` -Cmdlet zum Löschen einer Einstellung skalieren.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>Protokoll-Profile für Überwachungsprotokolle verwalten

*Protokoll-Profil* erstellen und Exportieren von Daten aus der Überwachungsprotokolle auf ein Speicherkonto und Data Retention können dafür konfigurieren. Optional können Sie die Daten auf Ihrem Haupt-Ereignis streamen. Beachten Sie, dass dieses Feature derzeit Vorschau, und Sie können nur eine Protokolldatei pro Abonnement Profil erstellen. Können Sie die folgenden Cmdlets mit Ihrem aktuellen Abonnement erstellen und Verwalten von Profilen anmelden. Sie können auch ein bestimmtes Abonnement. Obwohl standardmäßig PowerShell auf das aktuelle Abonnement kann jederzeit geändert werden, mit `Set-AzureRmContext`. Sie können Überwachungsprotokolle, Routendaten Speicherkonto oder Ereignis Hub innerhalb dieses Abonnement. Daten werden als BLOB-Dateien im JSON-Format geschrieben.

### <a name="get-a-log-profile"></a>Ein Protokoll Profil
Verwenden Sie zum Abrufen von bestehenden Protokolldatei Profile der `Get-AzureRmLogProfile` Cmdlet.

### <a name="add-a-log-profile-without-data-retention"></a>Hinzufügen eines Profils Protokoll ohne Data retention

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Protokoll Profil entfernen

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Hinzufügen eines Profils Protokoll mit Daten

Sie können **RetentionInDays -** Eigenschaft mit der Anzahl von Tagen als positive Ganzzahl angeben, wo die Daten gespeichert.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Profil mit Aufbewahrung und EventHub Protokoll hinzufügen
Neben routing Daten Storage-Konto können Sie auch mit einem Ereignis-Hub streamen. Beachten Sie, dass diese Vorabversion und der Konfiguration obligatorisch Event Hub-Konfiguration ist optional.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Konfigurieren Sie Diagnoseprotokolle
Viele Azure Dienste bieten zusätzliche Protokolle und Telemetrie, einschließlich Azure Security Netzwerkgruppen Software Lastenausgleich Key Vault, Azure Search Services und Logik-Apps und zum Speichern von Daten in Azure Storage-Konto konfiguriert werden. Dieser Vorgang kann nur auf eine Ressource ausgeführt und das Speicherkonto sollte im selben Bereich wie die Ressource, die Diagnose-Einstellung konfiguriert ist, vorhanden sein.

### <a name="get-diagnostic-setting"></a>Diagnose Einstellung

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Diagnose Einstellung deaktivieren

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Diagnose Einstellung ohne Archivierung aktivieren

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Mit Diagnose Einstellung aktivieren

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Aktivieren Sie Einstellung für eine bestimmte Kategorie mit Diagnose

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```
