<properties
    pageTitle="Vertikale Skalierung Azure virtuelle Maschine mit Azure Automatisierung | Microsoft Azure"
    description="Vertikal skalieren einer virtuellen Linux-Maschine auf Überwachungswarnmeldungen mit Azure Automation"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/29/2016"
    ms.author="singhkay"/>

# <a name="vertically-scale-azure-virtual-machine-with-azure-automation"></a>Vertikale Skalierung Azure VM mit Azure Automation

Vertikale Skalierung ist der Prozess der Erhöhung oder Verringerung der Ressourcen eines Computers auf die Arbeitslast. In Azure kann dies durch Ändern der Größe des virtuellen Computers erreicht. Dies kann in den folgenden Szenarien helfen.

- Wenn der virtuelle Computer nicht häufig verwendet wird, können Sie es auf eine kleinere Größe Ihre monatlichen Kosten reduzieren ändern.
- Wenn der virtuelle Computer auch sehen, kann es größer ihre Kapazitäten verändert werden

Die Gliederung für die Schritte hierzu ist unten

1. Setup Azure Automatisierung auf die virtuellen Computer
2. Ihr Abonnement importieren Sie Azure Automatisierung vertikale Skalierung runbooks
3. Ihr Runbook ein Webhook hinzufügen
4. Hinzufügen einer Benachrichtigung auf dem virtuellen Computer

> [AZURE.NOTE] Aufgrund der Größe des ersten virtuellen Computers Größe, skaliert werden kann, aufgrund der Verfügbarkeit der Größen im Cluster möglicherweise, denen in aktuelle virtuelle Computer bereitgestellt wird. In veröffentlichte Automatisierung Runbooks in diesem Artikel verwendeten wir hierbei und skalieren nur innerhalb der unter VM Größenpaare. Dies bedeutet, dass Standard_D1v2 virtuellen Computer nicht, Standard_G5 skaliert oder Basic_A0 verkleinert.

>| VM-Größen skalieren Paar |   |
|---|---|
|  Basic_A0 |  Basic_A4 |
|  Standard_A0 | Standard_A4 |
|  Standard_A5 | Standard_A7  |
|  Standard_A8 | Standard_A9  |
|  Standard_A10 |  Standard_A11 |
|  Standard_D1 |  Standard_D4 |
|  Standard_D11 | Standard_D14  |
|  Standard_DS1 |  Standard_DS4 |
|  Standard_DS11 | Standard_DS14  |
|  Standard_D1v2 |  Standard_D5v2 |
|  Standard_D11v2 |  Standard_D14v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="setup-azure-automation-to-access-your-virtual-machines"></a>Setup Azure Automatisierung auf die virtuellen Computer

Zunächst müssen Sie ist ein Azure Automation-Konto erstellen, die Instanzen VM festlegen skalieren verwendet Runbooks gehostet wird. Kürzlich eingeführten Automation Service das Feature "Ausführen als Konto" die Einstellung von Dienstprinzipalnamen für die automatische Ausführung der Runbooks im Auftrag des Benutzers sehr einfach macht. Weitere Informationen finden Sie in folgenden Artikel:

* [Runbooks mit Azure ausführen als Konto authentifizieren](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-the-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Ihr Abonnement importieren Sie Azure Automatisierung vertikale Skalierung runbooks

Runbooks, die erforderlich sind für die vertikale Skalierung virtuellen Computers sind bereits in Azure Automation Runbook Galerie veröffentlicht. Sie müssen in Ihrem Abonnement importieren. Sie können Runbooks importieren, indem Sie den folgenden Artikel lernen.

* [Runbook und Modul Kataloge für Azure Automation](../automation/automation-runbook-gallery.md)

Runbooks, die importiert werden unten angezeigt.

![Runbooks importieren](./media/virtual-machines-vertical-scaling-automation/scale-runbooks.png)

## <a name="add-a-webhook-to-your-runbook"></a>Ihr Runbook ein Webhook hinzufügen

Sobald Sie importiert haben hinzufügen Runbooks, müssen eine Webhook Runbook durch eine Warnung von einem virtuellen Computer ausgelöst werden können. Erstellen einer Webhook für Ihr Runbook Details finden Sie hier

* [Azure Automatisierung webhooks](../automation/automation-webhooks.md)

Sicherstellen Sie, dass die Webhook vor dem Webhook Dialogfeld schließen, wie Sie dies im nächsten Abschnitt werden kopiert.

## <a name="add-an-alert-to-your-virtual-machine"></a>Hinzufügen einer Benachrichtigung auf dem virtuellen Computer

1. Virtuellen Computer bearbeiten
2. Wählen Sie "Warnungsregeln"
3. Wählen Sie "Benachrichtigung hinzufügen"
4. Wählen Sie eine Metrik zum Auslösen der Warnung auf
5. Wählen Sie eine Bedingung, bei erfüllt wird die Warnung ausgelöst wird
6. Wählen Sie einen Schwellenwert für die Bedingung in Schritt 5. erfüllt werden
7. Wählen Sie einen Zeitraum über das Überprüfen der Überwachungsdienst für Bedingung und Schwellenwert in Schritte 5 und 6
8. Fügen Sie in der Webhook, die Sie im vorherigen Abschnitt kopiert.

![Virtuelle Maschine 1 Warnung hinzufügen](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-1.png)

![Hinzufügen einer Benachrichtigung für virtuellen Maschine 2](./media/virtual-machines-vertical-scaling-automation/add-alert-webhook-2.png)