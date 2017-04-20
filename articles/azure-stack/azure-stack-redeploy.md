<properties
    pageTitle="Azure Stack erneut | Microsoft Azure"
    description="Azure Stack erneut."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>Azure Stack erneut

Um Azure Stack erneut bereitstellen, müssen Sie von vorn zu beginnen wie unten beschrieben.

## <a name="steps-to-redeploy-azure-stack"></a>Schritte zum Azure Stack erneut bereitstellen

1. Starten Sie den Host im ursprünglichen Betriebssystem (bare Metal). Dies ist nicht die Standardeinstellung im Startmenü, damit Sie KVM oder lokale Konsole verwenden, um beim Neustart wählen (während der Installation der "Boot von VHD" OS auf "AzureStack TP2" genannt, wird dadurch identifizieren OS ist).

    Sie müssen die bestehenden Starteintrags (die neue Unterstützung Skript "PrepareBootFromVHD.ps1", die Sie kümmert.) entfernen

2. Wenn Sie keinen KVM oder OS Boot vor dem Neustart auswählen möchten:
    
    1. Suchen Sie das Skript.\BootMenuNoKVM.ps1 aus. Diese Datei ist mit anderen Support Skripts zusammen mit diesem Build.
    
    2. Führen Sie das Skript mit erhöhten. Wählen Sie den Namen der ursprünglichen Host-Betriebssystem. Dieser startet Host der ursprünglichen Host-Betriebssystem ohne KVM-Zugriff.
    
    3. Die Skripts werden Sie aufgefordert, den Neustart zu bestätigen.

    - Wenn andere Benutzer angemeldet sind, schlägt dieser Befehl fehl.

    - Führen Sie den folgenden Befehl einfach: Computer neu starten-erzwingen 
 
3. Löschen Sie die Datei CloudBuilder.vhdx, die als Teil der vorherigen Bereitstellung verwendet wurde.

    Sie müssen vorhandenen Speicherpool aus vorherigen TP2 Bereitstellung löschen. Das Bereitstellungsskript erkennt und die vorhandene bereinigt und neu erstellt.

5. Erneut kopieren eine neue CloudBuilder.vhdx starten, usw.

## <a name="next-steps"></a>Nächste Schritte

[Verbinden mit Azure Stapel](azure-stack-connect-azure-stack.md)
