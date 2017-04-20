<properties
    pageTitle="Web-Apps in Azure Stack - bekannte Probleme und Problembehandlung | Microsoft Azure"
    description="Detaillierte Anleitung zum Bereitstellen von Web-Apps in Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Web Apps Ressourcenanbieter - bekannte Probleme und Problembehandlung

> [AZURE.NOTE] Folgendes gilt nur für Azure Stapel TP1 Bereitstellungen.

Wenn Sie Probleme beim Bereitstellen von oder Arbeiten mit Web Apps Azure Stapel finden Sie in der folgenden Anleitung.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure Stapel Web Apps nicht verfügbar im portal

![Azure Stapel webapps Erstellen neuer Web App][1]

Nachdem die [Registrierung des Anbieters Azure Stapel Web Apps](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) Abschluss kann nicht Web + Mobile Blade finden Sie Folgendes versuchen:
* **Abmelden des Portals** und **Ihren Browser schließen** und eine **neue Browser-Fenster Anmeldung zum Portal** und versuchen Sie es erneut.

Wenn Sie noch nicht das Web und mobile Blade, versuchen Sie Folgendes angezeigt:

1.  **Azure-Stack-Host-Computer** in **Hyper-V-Manager** suchen Sie **xRPVM** und der VM **herstellen** .
2.  Öffnen Sie ein **Eingabeaufforderungsfenster als Administrator** und führen **IISRESET aus**
3.  Zurück zu **ClientVM** und öffnen Sie ein **Browserfenster**, **Anmeldung zum Portal** und versuchen Sie es erneut.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Bereitstellung schlägt fehl, wenn eine Webanwendung in eine neue Ressourcengruppe erstellen

In der ersten Vorschau der Web-Apps müssen **Alle Web-Apps** in derselben Ressourcengruppe als Web Apps Ressource (**Lokale AppService**) erstellt werden.  Dies ist ein bekanntes Problem und wird in einer zukünftigen Vorschau aufgelöst werden.

## <a name="other-issues"></a>Andere Probleme

Sollten Sie andere Probleme mit Web Apps Azure Stapel schreibe [Azure Stapel Forum] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack), unser Team Beiträge überwachen.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
