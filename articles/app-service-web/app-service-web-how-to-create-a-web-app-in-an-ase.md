<properties
    pageTitle="Erstellen Sie eine Webanwendung in einer App Service-Umgebung"
    description="Erstellen Sie webapps und app Servicepläne in einer App Service-Umgebung"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Erstellen Sie eine Webanwendung in einer App Service-Umgebung

## <a name="overview"></a>Übersicht

In diesem Lernprogramm veranschaulicht erstellen Sie Web-apps und App Service-Pläne in einer [App Service-Umgebung](app-service-app-service-environment-intro.md) (ASE). 

> [AZURE.NOTE] Wenn Sie Web app erstellen möchten jedoch sie in einer App Service-Umgebung müssen finden Sie unter [.NET Web app erstellen](web-sites-dotnet-get-started.md) oder eines der zugehörigen Tutorials für andere Sprachen und Frameworks.

## <a name="prerequisites"></a>Erforderliche Komponenten

Es wird vorausgesetzt, Sie haben eine App Service-Umgebung erstellt. Wenn Sie dies noch nicht getan haben, finden Sie unter [Erstellen einer App Service-Umgebung](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Erstellen einer Webanwendung

1. Klicken Sie in [Azure-Portal](https://portal.azure.com/)auf **Neu > Web + Mobile > Web App**. 

    ![][1]

2. Wählen Sie Ihr Abonnement.  

    Haben Sie mehrere Abonnements, um eine Anwendung in Ihrer App Service-Umgebung erstellen, Sie müssen dieselbe Abonnement verwenden, das Sie beim Erstellen der Umgebung verwendet. 

3. Wählen Sie oder erstellen Sie eine Ressourcengruppe.

    *Ressourcengruppen* können Sie verwandte Azure Ressourcen als Einheit und sind hilfreich, wenn Regeln für Ihre apps *Rollenbasierte Zugriffskontrolle* (RBAC) einrichten. Weitere Informationen finden Sie unter [Verwalten von Azure Ressourcen][ResourceGroups]. 

4. Wählen Sie oder erstellen Sie einen App Service-Plan.

    *App Service-Pläne* verwaltet Sätze von webapps.  Normalerweise Wenn Sie Preise auswählen, wird der Preis App Service-Plan als einzelne apps angewendet. In einer ASE Zahlen für ASE zugewiesen Serverinstanzen nicht, was Sie mit ASP aufgelistet haben.  Um die Anzahl der Instanzen einer Web-app, die Instanzen von App Service skalieren, skalieren Plan und wirkt sich auf alle Web-apps in diesem Plan.  Einige Features wie Site-Steckplätze oder VNET-Integration gelten auch Menge im Plan.  Weitere Informationen finden Sie unter [Übersicht über Azure App Service-Pläne](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

    App Service-Pläne identifizieren in Ihrem ASE anhand der Speicherort, der unter dem Plannamen angegeben.  

    ![][5]

    Möchten Sie einen App Service-Plan verwenden, der bereits in Ihrer App Service-Umgebung, wählen Sie diesen aus. Wenn Sie einen neuen App Service-Plan erstellen möchten, finden Sie im folgenden Abschnitt dieses Lernprogramms [Erstellen einer App Service-Plan in einer App Service-Umgebung](#createplan).

5. Geben Sie den Namen Ihrer Anwendung, und klicken Sie auf **Erstellen**. 

    Die ASE externe VIP verwendet die URL der Anwendung in einer ASE ist: [*Sitename*]. [*Name der App Service-Umgebung*]. p.azurewebsites.net statt [*Sitename*]. *.azurewebsites.NET
    
    Nutzt die ASE interne VIP dann den URL einer App ist ASE: [*Sitename*]. [*Subdomäne während ASE Erstellung angegeben*]   
    Nach dem ASP finden Sie unter **Namen** aktualisieren Subdomäne während ASE erstellen auswählen

## <a name="createplan"></a>Erstellen eines App Service-Plans

Beim Erstellen eines App Service-Plans in einer App Service-Umgebung unterscheiden sich ausgewählten Arbeitskraft eine ASE gibt keine freigegebenen Arbeitskräfte.  Arbeitskräfte müssen Sie verwenden, sind die ASE durch den Administrator zugewiesen wurden  Dies bedeutet, dass zum Erstellen eines neuen Plans Sie weitere Arbeitskräfte Ihre ASE Arbeitspool als die Gesamtzahl der Instanzen aller Ihrer Pläne bereits in diesem Arbeitspool zugeordnet müssen.  Wenn Sie genügend Arbeitskräfte im Pool ASE Arbeitskraft Ihren Plan erstellt haben, müssen Sie arbeiten mit Ihrem ASE Admin zu hinzugefügt.

Ein weiterer Unterschied mit App von App Service-Umgebung gehostet ist mangelnde Auswahl Preise.  Eine App Service Umgebung Zahlen für Serverressourcen vom System verwendet und in dieser Umgebung keinen zusätzliche Kosten für die Pläne.  Beim Erstellen eines App Service-Plans wählen Sie normalerweise einen Zahlungsplan bestimmt die Abrechnung.  Eine App Service-Umgebung ist im Wesentlichen einem privaten Ort können Sie Inhalte erstellen.  Sie Zahlen für die Umgebung, und nicht den Inhalt hosten.

Die folgende Anleitung zeigt App Service-Plan erstellen, während Sie eine Webanwendung wie im vorherigen Abschnitt des Lernprogramms erstellen.

1. Plan Auswahloberfläche auf **Neu erstellen** , und geben Sie einen Namen für den Plan wie normalerweise außerhalb einer ASE.

2. Wählen Sie ASE, die Sie von Ihrem Speicherort Auswahl verwenden möchten.

    Da eine App Service-Umgebung im Wesentlichen eine private Bereitstellung handelt, wird unter Speicherort. 

    ![][2]

    Nach Auswahl der ASE Element in der Lage aktualisiert die Erstellung App Service Benutzeroberfläche.  Die Position zeigt jetzt den Namen ASE und der Region ist und Preisgestaltung Plan Datumsauswahl durch eine Arbeitskraft Pool Auswahl ersetzt.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Arbeitskraft-Pool auswählen

Normalerweise gibt in Azure App Service und außerhalb einer App Service-Umgebung 3 Compute-Größen, die mit der Auswahl einer dedizierten Preisplan verfügbar sind.  In ähnlicher Weise für eine ASE können Sie definieren bis 3 Pools von Arbeitskräften und geben computegröße, die für den Pool Arbeitskraft verwendet wird.  Das heißt für Mieter ASE ist statt Zahlungsplan mit Compute Ihren App Serviceplan auswählen, wählen Sie *Arbeitspool*nennt.  

Die Arbeitskraft Pool Auswahl zeigt Benutzeroberfläche die Größe berechnen, Arbeitspool unter dem Namen.  Die verfügbare Menge bezieht sich auf wie viele compute-Instanzen in diesem Pool verfügbar sind.  Der gesamte Pool möglicherweise mehr als diese Anzahl Instanzen dieser Wert verweist jedoch auf einfach wie viele nicht verwendet werden.  Müssen Sie Ihre App Service-Umgebung hinzufügen, mehr Ressourcen berechnen finden Sie unter [Konfigurieren der App Service-Umgebung](app-service-web-configure-an-app-service-environment.md).

![][4]

In diesem Beispiel sehen Sie zwei Arbeitskraft Pools verfügbar. Deshalb ASE Administrator nur Hosts in diesen zwei workerpools zugewiesen.  Die dritte werden bei VMs reserviert darin angezeigt.  

## <a name="after-web-app-creation"></a>Nach Web app erstellen

Gibt es Aspekte zu Web-apps und App Service-Pläne in einer ASE, die berücksichtigt werden müssen.  

Wie bereits erwähnt, ist des Besitzers der ASE für die Größe des Systems, und sie sind daher auch dafür verantwortlich, dass genügend zum Hosten der gewünschten App Service-Pläne Kapazität. Sind keine verfügbaren Arbeitskräfte, werden Sie nicht Ihren App Service-Plan erstellen können.  Das stimmt Skalierung Ihrer Anwendung.  Benötigen Sie weitere Instanzen müssten Sie Ihre App Service-Umgebung Administrator weitere Mitarbeiter hinzufügen.

Nach dem Erstellen der WebApp und App Service-Plan ist es sollten Sie es vergrößern.  In einer ASE müssen Sie immer mindestens 2 Instanzen von App Service-Plan für Fehlertoleranz für Ihre apps.  Skalieren eines App Service-Plans in einer ASE entspricht dem normalen App Service-Plan Benutzeroberfläche.  Weitere Informationen zum Skalieren, [wie eine Webanwendung in einer App Service-Umgebung](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
