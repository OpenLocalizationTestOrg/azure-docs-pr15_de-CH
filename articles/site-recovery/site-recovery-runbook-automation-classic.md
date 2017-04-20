<properties
   pageTitle="Recovery-Pläne Azure Automatisierung Runbooks hinzufügen | Microsoft Azure"
   description="Dieser Artikel beschreibt, wie Azure Site Recovery jetzt erweitern Recovery-Pläne mit Azure Automatisierung komplexer Aufgaben während der Wiederherstellung in Azure ermöglicht"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans---classic"></a>Recovery-Pläne - Classic Azure Automatisierung Runbooks hinzufügen


In diesem Lernprogramm beschreibt die Integration von Azure Site Recovery mit Azure Automation Erweiterbarkeit für Recovery-Pläne bieten. Recovery-Pläne können Recovery Ihrer virtuellen Maschinen mithilfe von Azure Site Recovery für sekundäre Cloud-Replikation und Replikation Azure Szenarios geschützt koordinieren. Sie helfen auch bei der Wiederherstellung **präzise**, **wiederholbare**und **automatisierte**. Wenn Sie Ihre virtuellen Computer in Azure Failover, Integration in Azure Automation Recovery-Pläne erweitert und ermöglicht Runbooks so leistungsfähige Aufgaben ausführen.

Wenn Azure Automation noch nicht gehört haben, melden Sie sich [hier](https://azure.microsoft.com/services/automation/) und ihre Beispielskripts herunterladen [hier](https://azure.microsoft.com/documentation/scripts/). Lesen Sie mehr über [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) und Wiederherstellung in Azure mithilfe von Recovery-Plänen orchestrieren [hier](https://azure.microsoft.com/blog/?p=166264).

In diesem kurzen Tutorial betrachten wir, wie Sie Azure Automatisierung Runbooks in Recovery-Pläne integrieren. Wir automatisieren einfache Aufgaben, die früher ein manueller Eingriff erforderlich und Informationen zum Recovery eine Multi-Schritt in eine Wiederherstellungsaktion Klick konvertieren. Wir betrachten außerdem wie Sie ein einfaches Skript beheben können, wenn es schief geht.

## <a name="protect-the-application-to-azure"></a>Schützen der Anwendung in Azure

Beginnen Sie wir mit einer einfachen Anwendung bestehend aus zwei virtuellen Maschinen. Hier haben wir eine HRweb Anwendung von Fabrikam. Fabrikam-HRweb-Frontend und Fabrikam-Hrweb-Backend sind die zwei virtuelle Computer in Azure geschützt mit Azure Site Recovery. Gehen Sie folgendermaßen vor um virtuelle Computer mithilfe von Azure Site Recovery zu schützen.

1.  Aktivieren Sie Schutz für die virtuellen Computer.

2.  Sicherstellen Sie, dass die virtuellen Computer replizieren die erste Replikation abgeschlossen haben.

3.  Warten Sie, bis die erste Replikation abgeschlossen und der Replikationsstatus geschützt sagt.

![](media/site-recovery-runbook-automation/01.png)
---------------------

In diesem Lernprogramm erstellen wir einen Wiederherstellungsplan für Fabrikam HRweb Anwendung Failover der Anwendung in Azure. Und wir es mit einem Runbook integrieren, die auf den fehlerhaften erstellen wird über Azure virtuellen Computer Webseiten an Port 80.

Zunächst erstellen Sie einen Wiederherstellungsplan für die Anwendung.

## <a name="create-the-recovery-plan"></a>Den Wiederherstellungsplan erstellen

Um die Anwendung in Azure wiederherzustellen, müssen Sie einen Wiederherstellungsplan erstellen.
Verwenden einen Wiederherstellungsplan können Sie die Reihenfolge der Wiederherstellung der virtuellen Computer angeben. Gruppe 1 VM wiederherstellen und beginnen und dann den virtuellen Computer in Gruppe 2 folgen.

Erstellen Sie einen Wiederherstellungsplan, der aussieht wie unten.

![](media/site-recovery-runbook-automation/12.png)

Weitere Informationen zu Recovery-Pläne Dokumentation lesen [hier](https://msdn.microsoft.com/library/azure/dn788799.aspx "hier").

Als Nächstes erstellen Sie die erforderlichen Artefakte in Azure Automation.

## <a name="create-the-automation-account-and-its-assets"></a>Automation-Konto und ihre Ressourcen zu erstellen

Sie benötigen ein Konto Azure Automatisierung Runbooks erstellen. Wenn Sie nicht bereits über ein Konto verfügen, navigieren Sie zur Azure Automation Registerkarte gekennzeichnet durch ![](media/site-recovery-runbook-automation/02.png)und ein neues Konto erstellen.

1.  Benennen Sie dem Konto zu identifizieren.

2.  Geben Sie einen geografischen Bereich, in dem das Konto erstellt werden soll.

Es wird empfohlen, setzen Sie das Konto im Bereich der ASR-Depot.

![](media/site-recovery-runbook-automation/03.png)

Erstellen Sie anschließend die folgenden Elemente im Konto.

### <a name="add-a-subscription-name-as-asset"></a>Ein Abonnement als Anlage hinzufügen

1.  Fügen Sie eine neue Einstellung ![](media/site-recovery-runbook-automation/04.png) in Azure Automation Vermögenswerte und auswählen![](media/site-recovery-runbook-automation/05.png)

2.  Wählen Sie die Variable als **Zeichenfolge**

3.  Geben Sie Variablennamen als **AzureSubscriptionName**

    ![](media/site-recovery-runbook-automation/06.png)

4.  Namen der tatsächlichen Azure-Abonnement als Wert der Variablen.

    ![](media/site-recovery-runbook-automation/07_1.png)

Sie können den Namen Ihres Abonnements auf der Einstellungsseite Ihres Kontos Azure-Portal identifizieren.

### <a name="add-an-azure-login-credential-as-asset"></a>Ein Azure Anmeldung als Anlage hinzufügen

Azure Automatisierung verwendet Azure PowerShell Verbindung zum Abonnement und Artefakte vorhanden. Dafür müssen Sie Ihr Microsoft-Konto oder ein Arbeits- oder Schulcomputer Konto authentifizieren.
Die Anmeldeinformationen speichern Sie in einer Anlage von Runbooks sicher verwendet werden.

1.  Fügen Sie eine neue Einstellung ![](media/site-recovery-runbook-automation/04.png) in Azure Automatisierung Anlagen und wählen![](media/site-recovery-runbook-automation/09.png)

2.  Wählen Sie Anmeldeinformationen als **Windows PowerShell Anmeldeinformationen**

3.  Geben Sie als **AzureCredential**

    ![](media/site-recovery-runbook-automation/10.png)

4.  Geben Sie den Benutzernamen und das Kennwort zum Anmelden mit.

Beide Einstellungen in Ihrer Ressourcen verfügbar sind.

![](media/site-recovery-runbook-automation/11.png)

Weitere Informationen zum Herstellen einer Verbindung zu Ihrem Abonnement über PowerShell [hier](../powershell-install-configure.md).

Als Nächstes erstellen Sie ein Runbook in Azure Automation, die einen Endpunkt für den Front-End-virtuellen Computer nach einem Failover hinzufügen können.

## <a name="azure-automation-context"></a>Azure Automation-Kontext

ASR übergibt eine Kontextvariable an Runbook deterministisch Skripts schreiben. Man könnte argumentieren, die Namen der Cloud-Dienst und den virtuellen Computer sind vorhersehbar, dass geschieht, ist nicht immer der Fall durch bestimmte Szenarien wie dem Namen des virtuellen Computers kann durch nicht unterstützte Zeichen in Azure geändert haben. Daher ist diese Informationen ASR Wiederherstellungsplan als *Kontext*übergeben.

Es folgt ein Beispiel der Context-Variablen.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Die Tabelle enthält Namen und Beschreibung für jede Variable im Kontext.

**Name der Variablen** | **Beschreibung**
---|---
RecoveryPlanName | Name der Plan ausgeführt wird. Können Sie die Aktion basierend auf dasselbe Skript verwenden
FailoverType | Gibt an, ob das Failover testen, geplant oder ungeplant.
FailoverDirection | Geben Sie an, ob Recovery auf Primär oder sekundär
GroupID | Identifizieren Sie die Gruppe innerhalb der Wiederherstellungsplan Ausführung des Plans
VmMap | Array aller virtuellen Computer in der Gruppe
VMMap Schlüssel | Eindeutiger Schlüssel (GUID) für jeden virtuellen Computer. Es entspricht dem VMM-ID des virtuellen Computers gegebenenfalls.
Rollenname | Name der Azure-VM, die wiederhergestellt werden
CloudServiceName | Azure Cloud Service Name unter dem virtuellen Computer erstellt wird.


VmMap Key im Kontext identifizieren Sie auch zur Seite Eigenschaften VM in ASR und sehen Sie sich die VM-GUID-Eigenschaft.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Erstellen Sie ein Runbook Automatisierung

Jetzt erstellen Sie Runbook öffnen Sie Port 80 auf dem Front-End-virtuellen Computer.

1.  Erstellen Sie eine neue Runbook in Azure Automation-Konto mit dem Namen **OpenPort80**

    ![](media/site-recovery-runbook-automation/14.png)

2.  Autor anzeigen von Runbooks navigieren Sie und geben Sie den Entwurfsmodus.

3.  Zuerst geben Sie die Variable als Recovery Plankontext verwenden

    ```
        param (
            [Object]$RecoveryPlanContext
        )

    ```

4.  Weiter mit dem Anmeldeinformationen und Abonnement Abonnement verbinden

    ```
        $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

        # Connect to Azure
        $AzureAccount = Add-AzureAccount -Credential $Cred
        $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
        Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    ```

    Beachten Sie, dass Azure Ressourcen – **AzureCredential** und **AzureSubscriptionName** hier.

5.  Geben Sie jetzt die Endpunktdaten und die GUID des virtuellen Computers den Endpunkt verfügbar gemacht werden soll. In diesem Fall die Front-End-VM.

    ```
        # Specify the parameters to be used by the script
        $AEProtocol = "TCP"
        $AELocalPort = 80
        $AEPublicPort = 80
        $AEName = "Port 80 for HTTP"
        $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
    ```

    Dies gibt Azure Endpunktprotokoll lokalen Anschluss des virtuellen Computers und seiner zugeordneten öffentlichen Port. Diese Variablen werden Parameter von Azure Befehle, die VMs Endpunkte hinzufügen. Die VMGUID enthält die GUID des virtuellen Computers arbeiten müssen.

6.  Das Skript wird jetzt Kontext für die angegebene VM GUID extrahiert und erstellen auf dem virtuellen Computer auf sie verweist.

    ```
        #Read the VM GUID from the context
        $VM = $RecoveryPlanContext.VmMap.$VMGUID

        if ($VM -ne $null)
        {
            # Invoke pipeline commands within an InlineScript

            $EndpointStatus = InlineScript {
                # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
                # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

                $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                    Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                    Update-AzureVM
                Write-Output $Status
            }
        }
    ```

7. Dies ist erreicht veröffentlichen ![](media/site-recovery-runbook-automation/20.png) Ihr Skript für die Ausführung zu ermöglichen.

Das vollständige Skript für Referenzzwecke nachstehend

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Der Wiederherstellungsplan Skript hinzufügen

Wenn das Skript bereit ist, können Sie es den Wiederherstellungsplan hinzufügen, die Sie zuvor erstellt haben.

1.  Wählen Sie im erstellten Wiederherstellungsplan ein Skript nach der Gruppe 2 hinzufügen. ![](media/site-recovery-runbook-automation/15.png)

2.  Geben Sie ein Skript. Dies ist nur ein angezeigter Name für dieses Skript zeigt im Recovery-Plan.

3.  Failover Azure Skript – wählen Sie Azure Automatisierung Kontonamen aus

4.  Wählen Sie in Azure Runbooks Runbooks, die Sie erstellt.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Primäre serverseitige Skripts

Beim Ausführen eines Failovers in Azure können Sie primäre serverseitige Skripts ausführen. Diese Skripts werden auf dem VMM-Server bei einem Failover ausgeführt.
Primäre serverseitige Skripts sind nur für vor dem Herunterfahren und Herunterfahren Phasen buchen. Dies ist den primären Standort in der Regel nicht verfügbar, wenn eine Katastrophe erwarten.
Bei einem ungeplanten Failover nur bei im primären Standort Operationen wird versucht, die primären Skripte. Wenn sie nicht erreichbar oder Timeout Failover weiterhin die virtuellen Computer wiederherstellen.
Primäre serverseitige Skripts stehen nicht für VMware/physische/Hyper-V-Sites ohne VMM beim Failover in Azure Azure - geschützt.
Aber wenn Sie Failback von Azure lokal primäre serverseitige Skripts (Runbooks) für alle Ziele außer VMware dienen.

## <a name="test-the-recovery-plan"></a>Testen Sie den Wiederherstellungsplan

Wenn Sie planen, ein testfailover initiiert und erleben sie Runbooks hinzugefügt haben. Immer wird empfohlen, führen Sie ein testfailover zum Testen der Anwendung und den Wiederherstellungsplan sicherstellen, dass keine Fehler vorhanden sind.

1.  Wählen Sie den Wiederherstellungsplan und ein testfailover initiiert.

2.  Während der Ausführung Plan können Sie sehen, ob Runbook ausgeführt wurde oder nicht über den Status.

    ![](media/site-recovery-runbook-automation/17.png)

3.  Sie sehen auch detaillierte Runbook Ausführungsstatus auf Aufträge Azure Automatisierung für Runbooks.

    ![](media/site-recovery-runbook-automation/18.png)

4.  Nach Abschluss des Failovers neben Ausführungsergebnis Runbook sehen Sie, ob die Ausführung erfolgreich war oder nicht Seite Azure Virtual Machine und Endpunkte auf.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Beispielskripts

Während wir durchlaufen automatisieren eine häufig verwendete hinzufügen einen Endpunkt zu Azure VM in diesem Lernprogramm, zahlreiche andere leistungsstarke Automatisierungsaufgaben Azure Automatisierung möglich. Microsoft und der Community Azure Automation bieten Beispiel Runbooks welche können Schritte Erstellen eigener Projektmappen und Dienstprogramm Runbooks, die Sie als Bausteine für größere Automatisierungsaufgaben verwenden können. Beginnen mit der Galerie und leistungsstarke Mausklick Recovery-Pläne für Ihre Anwendung mit Azure Site Recovery.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Azure Automation Overview] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation Overview")

[Beispielskripts Azure Automatisierung] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Beispielskripts Azure Automatisierung")
