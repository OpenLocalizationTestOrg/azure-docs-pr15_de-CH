<properties 
    pageTitle="Starten und Beenden von virtuellen Maschinen mit Azure Automation - PowerShell Workflow | Microsoft Azure"
    description="Grafisch Version von Azure Szenarium einschließlich Runbooks starten und stoppen classic virtuelle Computer."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor="tysonn" />
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="07/06/2016"
    ms.author="bwren" />

# <a name="azure-automation-scenario---starting-and-stopping-virtual-machines"></a>Azure Szenarium - starten und Beenden von virtuellen Maschinen

Dieses Szenarium Azure enthält Runbooks starten und stoppen classic virtuelle Computer.  Dieses Szenario können Sie für Folgendes:  

- Verwenden Sie die Runbooks unverändert in Ihrer eigenen Umgebung. 
- Ändern Sie Runbooks benutzerdefinierte Funktionen ausführen.  
- Rufen Sie die Runbooks anderen Runbook als Teil einer Lösung. 
- Die Runbooks als Lernprogramme Runbook authoring Konzepte zu verwenden. 

> [AZURE.SELECTOR]
- [Grafik](automation-solution-startstopvm-graphical.md)
- [PowerShell-Workflow](automation-solution-startstopvm-psworkflow.md)

Dies ist die PowerShell Workflow Runbook Version dieses Szenarios. Es steht auch [grafisch Runbooks](automation-solution-startstopvm-graphical.md)verwenden.

## <a name="getting-the-scenario"></a>Abrufen des Szenarios

Dieses Szenario besteht aus zwei PowerShell Workflow Runbooks, die Sie unter den folgenden Links downloaden.  [Grafisch Version](automation-solution-startstopvm-graphical.md) dieses Szenarios Links zu Runbooks grafisch angezeigt.

| Runbook | Link | Typ | Beschreibung |
|:---|:---|:---|:---|
| AzureVMs starten | [Azure VMs Classic starten](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | PowerShell-Workflow | Startet alle klassischen virtuellen Computer in Azure Subscriptionor alle virtuellen Computer mit einem bestimmten Namen. |
| AzureVMs beenden | [Beenden von Azure klassische VMs](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | PowerShell-Workflow | Beendet alle virtuellen Computer in Automation-Konto oder alle virtuellen Computer mit einem bestimmten Namen.  |


## <a name="installing-and-configuring-the-scenario"></a>Installieren und Konfigurieren des Szenarios

### <a name="1-install-the-runbooks"></a>1. installieren Sie 1. die runbooks

Nach dem Herunterladen der Runbooks, können Sie sie mithilfe des Verfahrens in [ein Runbook importieren](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook)importieren.

### <a name="2-review-the-description-and-requirements"></a>2. Überprüfen Sie die Beschreibung und
Die Runbooks enthalten kommentierte Hilfetext, der Beschreibung und erforderlichen Ressourcen enthält.  Sie können auch Informationen aus diesem Artikel abrufen. 

### <a name="3-configure-assets"></a>3. konfigurieren Sie Anlagen
Die Runbooks erfordern die folgenden Ressourcen, die Sie erstellen und mit den entsprechenden Werten füllen.

| Asset-Typ | Name der Anlage | Beschreibung |
|:---|:---|:---|:---|
| Anmeldeinformationen | AzureCredential | Enthält die Anmeldeinformationen für ein Konto mit Berechtigungen zum Starten und Beenden der virtuellen Computer in der Azure-Abonnement.  Alternativ können Sie eine andere Anmeldeinformationen Anlage in der Aktivität **Hinzufügen AzureAccount** **Credential** -Parameter angeben. |
| Variable | AzureSubscriptionId | Enthält die Abonnement-ID der Azure-Abonnement. |

## <a name="using-the-scenario"></a>Verwenden des Szenarios

### <a name="parameters"></a>Parameter

Die Runbooks haben die folgenden Parameter.  Sie können müssen Werte für alle erforderlichen Parameter angeben und optional Werte für die anderen Parameter je nach Bedarf.

| Parameter | Typ | Obligatorisch | Beschreibung |
|:---|:---|:---|:---|
| ServiceName | Zeichenfolge | Nein | Wenn ein Wert angegeben ist, werden alle virtuellen Computer mit diesem Dienstnamen gestartet oder angehalten.  Wenn kein Wert angegeben ist, werden alle klassischen virtuellen Computer in der Azure-Abonnement gestartet oder angehalten. |
| AzureSubscriptionIdAssetName | Zeichenfolge | Nein | Enthält den Namen der [Variable Anlage](#installing-and-configuring-the-scenario) mit der Abonnement-ID Ihres Azure-Abonnements.  Wenn Sie keinen Wert angeben, wird *AzureSubscriptionId* verwendet.  |
| AzureCredentialAssetName | Zeichenfolge | Nein | Enthält den Namen der [Anlage Anmeldeinformationen](#installing-and-configuring-the-scenario) , die Anmeldeinformationen für das Runbook verwendet enthält.  Wenn Sie keinen Wert angeben, wird *AzureCredential* verwendet.  |

### <a name="starting-the-runbooks"></a>Die Runbooks starten

Eine der Methoden können starten [ein Runbook in Azure Automation](automation-starting-a-runbook.md) Sie entweder die Runbooks in diesem Szenario beginnen.

Die folgenden Beispielbefehle verwendet Windows PowerShell ausführen **StartAzureVMs** starten Sie alle virtuellen Computer mit den Namen *MyVMService*.

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### <a name="output"></a>Ausgabe

Die Runbooks wird [eine Meldung](automation-runbook-output-and-messages.md) für jeden virtuellen Computer, der angibt, ob die Start oder Stop-Anweisung erfolgreich gesendet wurde.  Sie können nach einer bestimmten Zeichenfolge in die Ausgabe das Ergebnis für jedes Runbook bestimmen suchen.  Ausgabe-Zeichenfolgen werden in der folgenden Tabelle aufgeführt.

| Runbook | Bedingung | Nachricht |
|:---|:---|:---|
| AzureVMs starten | Virtuelle Computer wird bereits ausgeführt.  | MyVM wird bereits ausgeführt. |
| AzureVMs starten | Start-Anforderung für den virtuellen Computer erfolgreich gesendet | "MyVM" wurde gestartet |
| AzureVMs starten | Fehler bei Anforderung für den virtuellen Computer starten  | "MyVM" konnte nicht gestartet werden |
| AzureVMs beenden | Virtual Machine wurde bereits beendet.  | "MyVM" wurde bereits beendet. |
| AzureVMs beenden | Beenden der Anforderung für den virtuellen Computer erfolgreich gesendet | "MyVM" wurde beendet. |
| AzureVMs beenden | Stop-Anforderung für den virtuellen Computer fehlgeschlagen  | "MyVM" konnte nicht beendet werden |

Beispielsweise startet der folgende Codeausschnitt aus ein Runbook alle virtuellen Computer mit den Namen *MyServiceName*.  Ggf. nicht Anfragen starten können Vorgänge durchgeführt. 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## <a name="detailed-breakdown"></a>Detaillierte Aufschlüsselung

Es folgt eine detaillierte Aufschlüsselung der Runbooks in diesem Szenario.  Diese Informationen können Sie die Runbooks anpassen oder nur von ihnen Informationen zum Erstellen eigener Szenarios für die Automatisierung.

### <a name="parameters"></a>Parameter

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

Abrufen der Werte für die [Parameter](#using-the-scenario)startet der Workflow.  Wenn Sie die Elementnamen nicht werden Standardnamen verwendet.

### <a name="output"></a>Ausgabe

    # Returns strings with status messages
    [OutputType([String])]

Dieser Zeile deklariert, dass die Ausgabe des Runbooks eine Zeichenfolge sein.  Dies ist nicht erforderlich, jedoch empfiehlt für Runbooks als [untergeordnete Runbook](automation-child-runbooks.md) wird, damit eine übergeordnete Runbook Ausgabetyp erwarten kann.

### <a name="authentication"></a>Authentifizierung

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

Die nächsten Zeilen legen [Anmeldeinformationen](automation-configuring.md#configuring-authentication-to-azure-resources) und Azure-Abonnement für den Rest des Runbooks verwendet werden.
Zuerst wird mit **Get-AutomationPSCredential** die Anlage erhalten, die die Anmeldeinformationen Zugriff zum Starten und Beenden der virtuellen Computer in der Azure-Abonnement enthält. **Hinzufügen AzureAccount** mithilfe dieser Anlage die Anmeldeinformationen festzulegen.  Die Ausgabe ist eine Dummyvariable zugewiesen, damit Runbook Ausgabe enthalten ist.  

Die Variable Anlage mit der Abonnement-ID wird mit **Get-AutomationVariable** und das Abonnement festgelegt **AzureSubscription wählen**abgerufen.

### <a name="get-vms"></a>Abrufen von VMs

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**AzureVM Sie** zum virtuellen Computer abrufen Runbooks mit arbeiten.  Wenn ein Wert in die Eingabevariable **Dienstname** angegeben, werden nur die virtuellen Computer mit diesem Namen Service abgerufen.  Wenn der **Dienstname** leer ist, werden alle virtuellen Computer abgerufen.

### <a name="startstop-virtual-machines-and-send-output"></a>Virtuelle Maschinen starten/stoppen und Ausgabe

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

Die nächsten Zeilen schrittweise jede virtuelle Maschine.  Zuerst **Energiestatus** des virtuellen Computers geprüft, ob es bereits ausgeführt wird oder angehalten, je nach Runbook.  Wenn sie bereits das Ziel ist, wird eine Meldung Ausgabe und Ende Runbook gesendet.  Wenn nicht, dann **AzureVM Start** oder **Stop AzureVM** werden versuchen, starten oder beenden Sie den virtuellen Computer mit dem Ergebnis einer Variablen gespeichert.  Eine Nachricht wird dann Ausgabe angibt, ob die Anforderung zum Starten oder beenden Sie erfolgreich gesendet wurde.


## <a name="next-steps"></a>Nächste Schritte

- Finden Sie weitere Informationen zum Arbeiten mit untergeordneten Runbooks [untergeordnete Runbooks in Azure Automation](automation-child-runbooks.md) 
- Weitere Informationen zu Nachrichten Runbook Ausführung und Protokollierung zur Problembehandlung finden Sie unter [Runbook Ausgabe und Nachrichten in Azure Automation](automation-runbook-output-and-messages.md)
