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


# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Recovery-Pläne Azure Automatisierung Runbooks hinzufügen


In diesem Lernprogramm beschreibt die Integration von Azure Site Recovery mit Azure Automation Erweiterbarkeit für Recovery-Pläne bieten. Recovery-Pläne können Recovery Ihrer virtuellen Maschinen mithilfe von Azure Site Recovery für sekundäre Cloud-Replikation und Replikation Azure Szenarios geschützt koordinieren. Sie helfen auch bei der Wiederherstellung **präzise**, **wiederholbare**und **automatisierte**. Wenn Sie Ihre virtuellen Computer in Azure Failover, Integration in Azure Automation Recovery-Pläne erweitert und ermöglicht Runbooks so leistungsfähige Aufgaben ausführen.

Wenn Azure Automation noch nicht gehört haben, melden Sie sich [hier](https://azure.microsoft.com/services/automation/) und ihre Beispielskripts herunterladen [hier](https://azure.microsoft.com/documentation/scripts/). Lesen Sie mehr über [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) und Wiederherstellung in Azure mithilfe von Recovery-Plänen orchestrieren [hier](https://azure.microsoft.com/blog/?p=166264).

In diesem Lernprogramm betrachten wir, wie Sie Azure Automatisierung Runbooks in Recovery-Pläne integrieren. Wir automatisieren einfache Aufgaben, die früher ein manueller Eingriff erforderlich und sehen wie mehrstufige Recovery in einem Klick Wiederherstellungsaktion konvertiert. Wir betrachten außerdem wie Sie ein einfaches Skript beheben können, wenn es schief geht.

## <a name="customize-the-recovery-plan"></a>Anpassen des Recovery-Plans

1. Lassen Sie uns zunächst feinstromdosierung Blade Ressource des Recovery-Plans. Sie sehen der Wiederherstellungsplan hat zwei virtuelle Computer für die Wiederherstellung hinzugefügt. 

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
---------------------

2. Klicken Sie auf die Schaltfläche hinzufügen ein Runbook anpassen. Dies öffnet den Wiederherstellungsplan Blade anpassen.


    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


3. Klicken Sie auf die Startgruppe 1, und wählen Sie eine Aktion zum Buchen von hinzufügen hinzufügen.

4. Wählen Sie ein Skript in die neue-Blade.

5. Nennen Sie das Skript "Hello World".

6. Wählen Sie einen Kontonamen Automatisierung. Dies ist das Konto Azure Automation. Beachten Sie, dass dieses Konto muss im gleichen Abonnement als Site Recovery-Depot kann in Azure Geography.

7. Wählen Sie ein Runbook Automation-Konto. Dies ist das Skript während der Ausführung des Recovery-Plans nach der Wiederherstellung der ersten Gruppe ausgeführt wird.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)


8. Wählen Sie OK, um das Skript zu speichern. Das Skript Post Aktivitätsgruppe Gruppe 1 hinzugefügt wird: starten.


    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>Hinzufügen eines Skriptes Kernpunkte

1. Klicken Sie mit der rechten Maustaste auf das Skript, und 'löschen Schritt' oder 'update Skript'.

2. Ein Skript lokal auf Azure beim Failover in Azure ausgeführt und kann auf Azure als eine primäre Skript vor dem Herunterfahren, während des Failbacks von Azure lokal ausführen.

3. Wenn ein Skript ausgeführt wird, wird diese Recovery Plankontext einfügen.

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
CloudServiceName (in Ressourcen-Manager-Bereitstellungsmodell) | Azure-Ressourcengruppe Name unter dem virtuellen Computer erstellt wird.


## <a name="using-complex-variables-per-recovery-plan"></a>Verwenden komplexe Variablen pro Wiederherstellungsplan

Ein Runbook erfordert manchmal mehr Informationen als nur die RecoveryPlanContext. Es ist kein erster Klasse Mechanismus ein Runbook einen Parameter übergeben. Jedoch wenn Sie dasselbe Skript über mehrere Recovery-Pläne verwenden möchten Sie können die Wiederherstellung Plankontext Variable 'RecoveryPlanName' und die unter experimentelle Technik mit einer Azure Automatisierung komplexer Variablen in ein Runbook. Das folgende Beispiel zeigt, wie drei verschiedene komplexe Variablen erstellen und im Runbook anhand eines Recovery-Plans verwenden.

Beachten Sie, dass Sie 3 zusätzliche Parameter in ein Runbook verwenden möchten. Lassen Sie uns in JSON-Formular codiert {"Var1": "Testautomation", "Var2": "Nicht geplant", "Var3": "PrimaryToSecondary"}

Verwenden Sie [AA komplexe Variable](../automation/automation-variables.md#variable-types Complex variable) eine neue Automatisierung Anlage erstellen.
Benennen Sie die Variable als <RecoveryPlanName>- Parameter.
Hier können Sie eine [komplexe Variablen](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396)erstellen.

Benennen Sie die Variable als unterschiedliche Recovery-Pläne

1. recoveryPlanName1 >-Parameter
2. recoveryPlanName2 >-Parameter
3. recoveryPlanName3 >-Parameter

Finden Sie jetzt im Skript in Parameter als

1. Die $rpname RP Namen erhalten = $Recoveryplancontext variabel
2. Erhalten Anlage $paramValue = "$($rpname)-Parameter"
3. Verwenden Sie dies als eine komplexe Variable für den Wiederherstellungsplan, durch Aufrufen von Get-AzureAutomationVariable [-AutomationAccountName] <String> -Namen $paramValue.

Beispielsweise zum Abrufen des komplexen Variable-Parameters für SharepointApp Wiederherstellungsplan erstellen Sie eine Azure Automatisierung komplexer Variable namens 'SharepointApp-Parameter'

In den Wiederherstellungsplan durch Extrahieren der Variablen aus der Anlage mit der Anweisung Get-AzureAutomationVariable verwenden [-AutomationAccountName] <String> [-Name] $paramValue. [Diese Einzelheiten verweisen](https://msdn.microsoft.com/library/dn913772.aspx)

So dasselbe Skript kann für verschiedene Recovery-Plans Plan bestimmte komplexe Variable in den Anlagen speichern verwendet werden.

## <a name="sample-scripts"></a>Beispielskripts

Ein Repository für Skripts, die Sie direkt in Ihrem Automation-Konto importieren finden Sie unter [Kristian Neses OMS-Repository für Skripts](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/Solutions/asrautomation)

Das Skript ist eine Azure-Ressourcen-Manager-Vorlage, die alle Bereitstellen der folgenden Skripts

* NSG

NSG Runbook jeder VM innerhalb der Plan für die Wiederherstellung öffentlicher IP-Adressen zuweisen und eine Netzwerk-Sicherheitsgruppe, der Standard-Kommunikation ermöglicht die virtuelle Netzwerkadapter zuordnen

* PublicIP

Runbook öffentliche IP-Adresse werden alle virtuellen Computer in den Plan für die Wiederherstellung öffentlicher IP-Adressen zuweisen. Zugriff auf Computer und Applikationen hängt die Firewall Settings in jeden Gast


* CustomScript

Runbook CustomScript jede VM innerhalb der Plan für die Wiederherstellung öffentlicher IP-Adressen zuweisen und Erweiterung benutzerdefinierte Skripts, die während der Bereitstellung der Vorlage auf das Skript Ziehen wird installiert

* NSGwithCustomScript

NSGwithCustomScript, die Runbook öffentlicher IP-Adressen zuweisen, um jede VM in der Recovery Plan installieren ein benutzerdefiniertes Skript Erweiterung und verbinden die virtuellen Netzwerkadapter NSG zulassen Standard ein- und ausgehende Kommunikation für remote-Zugriff

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Azure Automation Service Ausführen als Konto](../automation/automation-sec-configure-azure-runas-account.md)

[Azure Automation Overview] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure Automation Overview")

[Beispielskripts Azure Automatisierung] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Beispielskripts Azure Automatisierung")
