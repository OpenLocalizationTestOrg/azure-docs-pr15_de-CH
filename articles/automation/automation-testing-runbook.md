<properties 
    pageTitle="Testen ein Runbook in Azure Automatisierung | Microsoft Azure"
    description="Bevor Sie ein Runbook in Azure Automation veröffentlichen, können Sie testen, um sicherzustellen, dass dies funktioniert wie erwartet.  Dieser Artikel beschreibt, wie ein Runbook testen und die Ausgabe anzeigen."
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
    ms.date="09/12/2016"
    ms.author="magoedte;bwren" />

# <a name="testing-a-runbook-in-azure-automation"></a>Testen ein Runbook in Azure Automation
Beim Testen eines Runbooks [Entwurf](automation-creating-importing-runbook.md#publishing-a-runbook) ausgeführt und diese führt Aktionen abgeschlossen. Keine Historie erstellt, aber die Datenströme [Ausgabe](automation-runbook-output-and-messages.md#output-stream) und [Warnung und Fehler](automation-runbook-output-and-messages.md#message-streams) im Test angezeigt Bereich ausgegeben. Nachrichten in den [ausführlichen Stream](automation-runbook-output-and-messages.md#message-streams) werden im Ausgabebereich nur angezeigt, wenn die [Variable $VerbosePreference](automation-runbook-output-and-messages.md#preference-variables) auf Weiter setzen.

Entwurf ausgeführt wird, das Runbook noch führt der Workflow normalerweise und führt Aktionen für Ressourcen in der Umgebung. Aus diesem Grund sollten Sie nur Runbooks nicht Fertigungsressourcen testen.

So testen Sie jeden [Typ Runbooks](automation-runbook-types.md) entspricht und es gibt keinen Unterschied zwischen den Text-Editor und der Grafik-Editor in Azure-Portal testen.  


## <a name="to-test-a-runbook-in-the-azure-portal"></a>So testen Sie ein Runbook in Azure-portal

Sie können mit einem beliebigen [Typ Runbook](automation-runbook-types.md) in Azure-Portal arbeiten.

1. Öffnen Sie die Entwurfsversion der Runbook in den [Text-Editor](automation-editing-a-runbook.md#Portal) oder [Grafikeditor](automation-graphical-authoring-intro.md).
2. Klicken Sie auf **Test** , um das Blade Test öffnen.
3. Wenn Runbooks Parameter verfügt, werden diese im linken Bereich aufgelistet, in denen Werte für die Prüfung möglich.
4. Ggf. der Testlauf [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) **Run Settings** **Hybrid** Arbeitskraft ändern Sie und wählen Sie die Zielgruppe.  Andernfalls behalten Sie die Standardeinstellung **Azure** zum Ausführen des Tests in der Cloud.
5. Klicken Sie auf die Schaltfläche **Start** , um den Test zu starten.
6. Wenn Runbooks [PowerShell Workflow](automation-runbook-types.md#powershell-workflow-runbooks) oder [Graphical](automation-runbook-types.md#graphical-runbooks)ist, können Sie beenden oder unterbrechen sie mit den Schaltflächen unterhalb der Ausgabebereich getestet wird. Wenn Sie Runbook anhalten, wird die aktuelle Aktivität vor Unterbrechung abgeschlossen. Nach Runbook angehalten wird, können Sie beenden oder starten Sie ihn neu.
7. Überprüfen Sie die Ausgabe im Ausgabebereich Runbook.


## <a name="next-steps"></a>Nächste Schritte

- Erstellen oder importieren ein Runbook finden Sie unter [Erstellen oder importieren ein Runbook in Azure Automation](automation-creating-importing-runbook.md)
- Erfahren Sie mehr zum Erstellen von Grafiken finden Sie unter [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md)
- Zunächst mit PowerShell Workflow Runbooks finden Sie [meinen ersten PowerShell Workflow runbook](automation-first-runbook-textual.md)
- Weitere Informationen zum Konfigurieren von Runboks Nachrichten und Fehler, einschließlich empfohlene Vorgehensweisen finden Sie unter [Runbook Ausgabe und Nachrichten in Azure Automation](automation-runbook-output-and-messages.md)