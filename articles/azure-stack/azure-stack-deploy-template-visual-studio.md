<properties
    pageTitle="Bereitstellen von Vorlagen mit Visual Studio in Azure Stapel | Microsoft Azure"
    description="Weitere Informationen zum Bereitstellen von Vorlagen mit Visual Studio in Azure Stapel."
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Bereitstellen von Vorlagen in Azure Stapel mit Visual Studio

Verwenden Sie Visual Studio Azure Resource Manager Vorlagen POC Stapel Azure bereitstellen.

Ressourcen-Manager Vorlagen Bereitstellung und alle Ressourcen für die Anwendung in einer einzigen koordinierten Operation.

1.  Öffnen Sie Visual Studio 2015 Update 1.

2.  Klicken Sie auf **Datei**, klicken Sie auf **neu**, und klicken Sie im Dialogfeld **Neues Projekt** auf **Azure-Ressourcengruppe**.

3.  Geben Sie einen **Namen** für das neue Projekt, und klicken Sie dann auf **OK**.

4.  Klicken Sie im Dialogfeld **Azure-Vorlage auswählen** klicken Sie auf **virtuellen Windows-Maschine**und klicken Sie dann auf **OK**.

  In dem neuen Projekt sehen Sie eine Liste der verfügbaren Vorlagen **Vorlagen** Knoten im **Projektmappen-Explorer** erweitern.

5.  Klicken Sie im **Projektmappen-Explorer** Namen Sie den des Projekts, **Bereitstellen**, klicken auf **Neue Bereitstellung**.

6.  Wählen Sie im Dialogfeld **Bereitstellen Ressourcengruppe** in der Dropdownliste **Abonnement** Abonnements Microsoft Azure Stapel.

7.  In der Liste **Gruppe** eine vorhandene Ressourcengruppe auswählen oder eine neue erstellen.

8.  Wählen Sie in der Liste **Gruppe Ort** , und klicken Sie auf **Bereitstellen**.

9.  Im Dialogfeld **Parameter bearbeiten** Geben Sie Werte für den Parameter (die Vorlage variieren), und klicken Sie auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit der Befehlszeile](azure-stack-deploy-template-command-line.md)
